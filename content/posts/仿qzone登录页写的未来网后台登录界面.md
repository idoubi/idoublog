---
title: 仿Qzone登录页写的未来网后台登录界面
tags:
  - 仿站
  - 前台
id: 152
categories:
  - 开发笔记
date: 2014-01-18 23:03:01
---

一个很简单的前台程序，背景图片直接用的Qzone登录页背景图，自己写了个html页面和几十行css，写了一个常用的js切换tab功能，demo页面放在sae上，用的是thinkphp框架。本来想实现模拟登录Qzone功能，但是Qzone的表单设置得比较巧妙，还没搞清楚应该怎样post数据，关于网页抓取和模拟登录的开发一定要抓紧时间学习，这是一个很关键的技术，武大助手就是用的模拟登录教务部管理系统的功能，我要是能尽快摸索出模拟登录的方法，这必定是极好的事情。
![](http://ov13triio.bkt.clouddn.com/2014/01/20120603195648-1612487160.jpg)

Qzone登录页地址：[http://i.qq.com/](http://i.qq.com/)

HTML code：
```
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
    <head>
        <meta http-equiv="Content-Type" content="text/html;charset=utf8" />
        <link href="style.css" rel="stylesheet" type="text/css" />
        <title>用户登录</title>
        <script type="text/JavaScript">
        <!--
        function settab_zzjs(name,num,n){
         for(i=1;i<=n;i++){
          var menu=document.getElementById(name+i);
          var con=document.getElementById(name+"_"+"zzjs"+i);
          menu.className=i==num?"on_zzjs":"";
            con.style.display=i==num?"block":"none";

         }
        }
        //-->
        </script>
    </head>
    <body>
        <div id="logo"></div>
        <form name="login_form" method="post" action="http://zhaopin.future.org.cn/manage.php" id="login_form">
            <ul id="login_way">
                <li id="zzjs1" onClick="settab_zzjs('zzjs',1,4)">部门登录</li>
                <li id="zzjs2" onClick="settab_zzjs('zzjs',2,4)">个人登录</li>
            </ul>
            <table id="zzjs_zzjs1" class="login_table">
                <tr>
                    <td><input name="username" id="username" type="text" value="部门账号" onClick="this.value=''" /></td>
                </tr>
                <tr>
                    <td><input name="password" id="password" type="password" value="" /></td>
                </tr>
                <tr>
                    <td><input name="login" id="login" type="submit" value="部门登录" /></td>
                </tr>
                <input type="hidden" name="select1" value="部门登录" />
            </table>
            <table id="zzjs_zzjs2" class="login_table" style="display:none;">
                <tr>
                    <td><input name="username" id="username" type="text" value="学号" onClick="this.value=''" /></td>
                </tr>
                <tr>
                    <td><input name="password" id="password" type="password" value="" /></td>
                </tr>
                <tr>
                    <td><input name="login" id="login" type="submit" value="个人登录" /></td>
                </tr>
                <input type="hidden" name="select1" value="个人登录" />
            </table>
            <img id="pic" src="http://ipic-ipic.stor.sinaapp.com/original/f2c1188842701e23fa70499c98619d35.gif" title="二维码登录" />
            <ul class="extra">
                <li><a href="">忘记密码？</a></li>
                <li><a href="">加入青传</a></li>
                <li><a href="">留言反馈</a><li>
            </ul>
        </form>
   </body>
</html>
```

CSS code：
```
body{background:url('http://ipic-ipic.stor.sinaapp.com/original/112a9d67cf1d4b399b0770fbbd9dad20.jpg') no-repeat center;}
#logo{background:url("http://ipic-ipic.stor.sinaapp.com/original/d60ace3a6b74a98fa4dee46394169dc2.jpg") no-repeat;width:180px;height:180px;position:absolute;top:100px;left:200px;}
#login_form{width:418px;height:316px;background:#fff;border-radius:5px;position:absolute;top:90px;right:240px;}
.login_table{width:258px;margin:40px auto;}
#login_way{width:258px;height:40px;border-bottom:1px solid #e2e2e2;;padding:0 80px;}
#login_way li{list-style:none;float:left;width:100px;height:40px;color:#999;font-size:16px;line-height:40px;text-align:left;margin-right:20px;text-align:center}
#login_way li:hover{cursor:pointer;color:#333;}
#username,#password{width:258px;height:38px;padding:9px 0 9px 10px;border:1px solid #ccc;border-radius:2px;color:#999;margin:8px 0;} 
#login{width:115px;height:35px;background:#81D620;border:none;border-radius:5px;color:#fff;font-size:18px;font-weight:bold;margin-top:5px;}
#pic{width:51px;height:77px;position:absolute;top:55px;right:10px;}
.extra{position:absolute;bottom:0px;right:10px;}
.extra li{list-style:none;float:left;font-size:12px;}
.extra li a{color:#333;text-decoration:none;padding:2px 5px;}
.extra li a:hover{text-decoration:underline;}
```
