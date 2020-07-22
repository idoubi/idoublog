---
title: ajax在新闻资讯类产品中的应用
id: 324
categories:
  - 开发笔记
date: 2014-10-17 11:15:17
tags:
  - ajax
---

最近一直在做一款叫做“高校头条”的新闻资讯类产品，初衷是想通过数据抓取的技术把大学生最常浏览的新闻资讯类信息汇集到一起，通过一定的算法分析，根据用户的兴趣给他们推荐相应的内容。微信端产品已经上线一段时间了，目前在捣鼓安卓端需要怎么做，希望等安卓端做出来之后就开始推广给所有人用。

所谓的微信端“高校头条”，其实就是在微信中链接了一个wap站点，使用bootstrap做了一个移动端适配的前台，使用weiphp作为后台，通过在后台设置抓取地址和一定的抓取规则，使用curl技术抓取各个微信订阅号里面的内容，再把抓取到的内容存储到数据库，在wap站点直接输出数据库里面的新闻资讯信息，在微信端通过关键词查询获取相应的内容。整个产品的实现原理就是这样子，今天想要总结的一个技术是：通过ajax调用数据，在wap站点输出新闻列表。

AJAX即“Asynchronous Javascript And XML[1] ”（异步JavaScript和XML[1] ），是指一种创建交互式网页应用的网页开发技术。百度百科上是这么解释的。ajax也是业内评价非常高的一种技术，一般功能较为完善的网站都或多或少使用了ajax。ajax最主要的特点是异步，使用异步最主要的作用是提高用户体验，也许我们会经常遇到，在点击某一个网站链接的时候，浏览器跳转半天都不能把我们想看的内容呈现出来，这时候网页浏览者的耐心肯定会受到很大的挑战，也就是所谓的用户体验极差。而使用ajax获取数据，一般网页是不会出现这种情况的，ajax可以设置局部刷新，我们只要把需要通过点击链接而切换内容的区域设置成ajax获取数据就行了，这样用户点击某个链接的时候，网站是不会整体跳转的，也不会出现一片空白的情况，用户看到所需内容所等待的时间也非常少，很明显的提高了用户体验。

不知不觉废话又说了一大堆。现在要开始进入正题了。

![](http://ov13triio.bkt.clouddn.com/2014/10/QQ图片20141017095834.png)
![](http://ov13triio.bkt.clouddn.com/2014/10/QQ图片20141017095814.png)

先来看一下这两张图片。这是”高校头条“wap网站上的新闻展示页面，导航栏我们分了”本校“、”考研“、”就业“、”创业“几个栏目。很明显我们希望用户点击每个栏目之后，下面会展示对应栏目下的新闻列表。在没学会ajax之前我是怎么做的呢？
```
<nav class="table-responsive">
	<table class="table">
		<tr>	<td><if condition="$column_id eq ''"><a href="{:U('index',array('school_id'=>$school_id))}" class="active"><else/><a href="{:U('index',array('school_id'=>$school_id))}"></if>本校</a></td>
			<volist name="columnList" id="vo">
			<td><if condition="$column_id eq $vo['id']"><a href="{:U('index',array('column_id'=>$vo['id']))}" class="active"><else/><a href="{:U('index',array('column_id'=>$vo['id']))}"></if>{$vo.column_name}</a></td>
			</volist>
		</tr>
	</table>
</nav>
```

就像上面代码看到的，我是对每个栏目设置一个`<a>`标签，然后点击每个栏目之后跳转到index方法，传递参数school_id或者column_id进行处理，然后在index方法中根据传递过来的参数去数据库中调用对应栏目下面的新闻，再把调用到的数据输出到前台页面显示。使用这种方法，用户每一次点击栏目时，都会跳转到index方法对应的模板html，所以会在浏览器上看到一个圆圈来回转动（微信浏览器最新版是一条绿色的直线往右挺进）。如果网速慢一点的话，这样的浏览新闻的方式肯定会让用户奔溃，跳转半天看不到所需要的页面是很正常的事情。

我是一个完美主义者，我肯定不希望使用我产品的用户感觉到如此差的体验。于是我各种百度ajax使用方法，终于在一个夜黑风高的日子把ajax调用数据的方法学会了，共享出来给各位开发者，希望大家少走弯路。

首先要做的就是对栏目展示的方式进行一次重构。
```
<nav class="table-responsive">
	<table class="table" id="columnList">
		<tr>
			<td><a rel="{:U('returnNewsJson')}" type="{$school_id}" class="active">本校</a></td>
			<volist name="columnList" id="vo">
			<td><a rel="{:U('returnNewsJson')}" title="{$vo.id}">{$vo.column_name}</a></td>
			</volist>
		</tr>
	</table>
</nav>
```

现在的这种栏目展示方式较之前的方式而言，首先是把`<a>`标签的href属性去掉了。也就是设置了用户点击栏目链接的时候，网页是不能跳转的。然后就是给`<a>`标签添加了rel、type、title三个属性，其实也不一定非要添加这三个属性，添加其他的属性也是可以的，这样做的关键是为了获取所需属性的值。

我们到通过修改栏目展示代码后我们去掉了栏目的跳转href属性，但是我们添加了一个rel属性，属性的值是returnNewsJson，也就是说用户每次点击栏目链接的时候，我们会通过ajax去returnNewsJson这个方法中调用数据，再把调用到的数据展示出来。

看一下returnNewsJson这个方法是如何写的。
```
public function returnNewsJson(){

	$action=I('action');
	$column_id=intval(I('column_id'));
	$school_id=intval(I('school_id'));

	if($action=='getNewsJson'){

		if($column_id &amp;amp;&amp;amp; !$school_id){
			$map['column_id']=$column_id;
			$newsList=M('toutiao_news')-&amp;gt;where($map)-&amp;gt;order('date desc')-&amp;gt;select();
		}

		if(!$column_id &amp;amp;&amp;amp; $school_id){
			$map['school_id']=$school_id;
			$newsList=M('toutiao_news')-&amp;gt;where($map)-&amp;gt;order('date desc')-&amp;gt;select();
		}

		$newsJson=json_encode($newsList);
		echo $newsJson;
	}
}
```
这个方法很简单，就是根据传递过来的column_id和school_id去数据库中调用相应的数据，再把数据封装成json格式返回给调用者。

把这些准备工作做好了，我们现在开始要写ajax调用数据的代码了。
```
<script type="text/javascript">// <![CDATA[
$(document).ready(function(){
	$("a[rel]").click(function(){
		$("a[rel]").removeClass("active");
		//alert(this.rel);
		var url=this.rel;
		var column_id=this.title;
		var school_id=this.type;
		$("a[title="+column_id+"]").addClass("active");
		$("a[type="+school_id+"]").addClass("active");
		$.ajax({
			type:"post",
			url:url,
			dataType:"json",
			data:"action=getNewsJson&amp;column_id="+column_id+"&amp;school_id="+school_id,
			success:function(msg){
				$("#1").html("");
				$.each(msg,function(i,item){
					$("#1").append(
						"<a href="+item.url+">"+
							"<div class='well'>"+
								"<img src="+item.imglink+">"+
									"<h4>"+item.title+"</h4>"+
									"<h6><span>"+item.sourcename+"</span>&amp;nbsp;&amp;nbsp;<span>"+item.date+"</span><span class='pull-right'>浏览：99</span></h6>"+
							"</div>"+
						"</a>"
					);
				});
			}
		});
	});
});

// ]]></script>
```

上面写的是一个javascript代码，通过ajax调用returnNewsJson方法获取数据。对ajax的几个参数进行了设置，ajax的type方式设置为post，url为需要post数据的目标网址，dataType设置返回数据的格式为json，data中设置需要post的数据字符串，success设置如果post数据成功之后调用的函数，函数的参数为返回的json数据。使用$.each循环输出返回的数据并把它们展示在新闻列表区域。

通过上面的几个步骤，使用ajax方便的替换掉了原来那种点击跳转的新闻展示方式，极大的提高了用户体验，关注微信公众号”高校头条“，进入wap站点击栏目试一下你就知道，新闻列表切换速度快的一笔，毫无违和感。

总结：

1、任何技术或者知识不是每个人天生就会的，肯定需要后天付出很多的努力，经过很多次的试错才能最终学会的。（ajax技术我摸索了一个礼拜，试错了两天，最终才只是入了个门）。

2、做产品，一定要重视用户体验，只有把用户体验做到极致，你的产品才能获得最后的成功。

3、产品需要迭代，技术需要总结。对以前写过的代码进行一次重构，可以发现存在的很多问题，重构过程中往往能有新的创意，而进行总结的目的是方便自己在以后遇到类似的问题上方便查询，也能给其他人相应的帮助。