---
title: 使用Apache的ab工具进行网站性能压力测试
id: 600
categories:
  - 开发笔记
date: 2016-02-15 09:06:02
tags:
  - 压力测试
  - Apache
---

年前给中百仓促超市做了微信摇一摇抽奖的功能开发，顾客凭借在中百购物的发票联可以在微信中参与抽奖。由于中百仓储超市微信公众号的关注量较大，客户担心太多用户同时参与抽奖的话会导致服务器崩溃而不能给用户提供正常的服务，因此要求我们对中百仓促超市微信公众号接入的网站性能进行压力测试。下面总结一下使用Apache的ab工具进行压力测试的过程。

装了wampserver的情况下可以直接使用ab工具，win+R后输入cmd弹出dos命令窗口，进入wamp的安装目录并且启动ab工具。

![](http://ov13triio.bkt.clouddn.com/2016/02/1.png)


## 1000请求，200并发下的压力测试结果：

![](http://ov13triio.bkt.clouddn.com/2016/02/2.png)


## 1000请求，300并发下的压力测试结果：

![](http://ov13triio.bkt.clouddn.com/2016/02/3.png)


## 1000请求，100并发下的压力测试结果：

![](http://ov13triio.bkt.clouddn.com/2016/02/4.png)


由上面的测试结果分析可知，这台服务器的最佳并发量应该控制在100以下。


参考资料：

如何使用Apache的ab工具进行网站性能测试：

[http://jingyan.baidu.com/article/e3c78d647a57833c4c85f502.html](http://jingyan.baidu.com/article/e3c78d647a57833c4c85f502.html)

ab 压力测试 命令详解与结果分析：

[http://lihuipeng007.blog.163.com/blog/static/121084388201082542924962/](http://lihuipeng007.blog.163.com/blog/static/121084388201082542924962/)


压力测试所用工具下载：

wampserver：[https://www.baidu.com/s?ie=UTF-8&amp;wd=wampserver](https://www.baidu.com/s?ie=UTF-8&amp;wd=wampserver)


测试程序：

```
<?php 

class Test {

	function yao($invoice, $mobile) {
		$key = 'a63d590******190af93fe5387045795';
		$corpid = '1**1';
		$action = 'check';
		$tel = $mobile;
		$reciptnum = $invoice;
		$tmpArr = array($key, $corpid, $action, $tel, $reciptnum);
		sort($tmpArr);
		$tmpStr = implode($tmpArr);
		$sign = sha1($tmpStr);

		$url = 'http://1************3:8001/lottery/ws/api/win?corpid=1001&amp;action=check&amp;tel='.$tel.'&amp;reciptnum='.$reciptnum.'&amp;sign=' . $sign;
		$content = $this->http_get($url);
		$result = json_decode($content, true);

		return $result;
	}

	/**
	 * GET 请求
	 * @param string $url
	 */
	function http_get($url){
	    $oCurl = curl_init();
	    if(stripos($url,&quot;https://&quot;)!==FALSE){
	        curl_setopt($oCurl, CURLOPT_SSL_VERIFYPEER, FALSE);
	        curl_setopt($oCurl, CURLOPT_SSL_VERIFYHOST, FALSE);
	        curl_setopt($oCurl, CURLOPT_SSLVERSION, 1); //CURL_SSLVERSION_TLSv1
	    }
	    curl_setopt($oCurl, CURLOPT_URL, $url);
	    curl_setopt($oCurl, CURLOPT_RETURNTRANSFER, 1 );
	    $sContent = curl_exec($oCurl);
	    $aStatus = curl_getinfo($oCurl);
	    curl_close($oCurl);
	    if(intval($aStatus[&quot;http_code&quot;])==200){
	        return $sContent;
	    }else{
	        return false;
	    }
	}

}

$test = new Test();
$result = $test->yao('1004-0408-1610451', '1341111111111');
var_dump($result);

```