---
title: 在php中使用curl多线程下载图片
id: 676
categories:
  - 开发笔记
date: 2016-12-30 10:25:39
tags:
  - php
  - curl
---

> 遇到一个需求：要下载这个网站[http://www.laredoute.com/](http://www.laredoute.com/)上面的商品图片到本地。
>   分析了一下，这个网站是一个国外的站点，受cdn节点的影响，在国内打开的速度比较慢。另一方面，要下载的商品图片较大，单张图片的大小有超过200kb的。
>   现在的需求是，要在短时间内批量下载该网站上面的商品图片到本地，鉴于这两点考虑，如果使用php来做的话，单纯的用file_get_contents可能行不通，于是想到了用php的curl库来处理。

虽然用curl来下载图片比起用file_get_contents来处理效率更高，稳定性更好，但是如果使用curl单线程来处理的话，效率并不会得到非常明显的提升。所幸的是，curl库提供了curl_multi功能，让我们可以使用curl多线程来处理业务逻辑，可以显著的提升效率。下面拿下载上述站点上面的8张图片为例，分别使用curl单线程和curl多线程来进行图片下载，并对两者的效率做一个比较。

## single.php：curl单线程下载图片

```php
<?php 

/**
 * 单线程下载远程图片
 * @author 艾逗笔<765532665@qq.com>
 */

set_time_limit(300);                        // 设置程序执行超时时间为5分钟

// 要下载的图片数组
$pics = array(
    'http://media.laredoute.com/products/1200by1200/37/01/01/37010132_90_CO_1_2850804.jpg',
    'http://media.laredoute.com/products/1200by1200/37/01/01/37010132_10_CO_2_2850799.jpg',
    'http://media.laredoute.com/products/1200by1200/57/02/02/57020206_1_CO_1_057020206-9be173d2-6bc3-44ba-b9cb-111010ed0051.jpg',
    'http://media.laredoute.com/products/1200by1200/37/01/01/37010132_10_CO_1_2850800.jpg',
    'http://media.laredoute.com/products/641by641/35/00/42/350042687_1_CO_1_7639577.jpg',
    'http://media.laredoute.com/products/641by641/35/00/34/350034174_1_CO_1_7669890.jpg',
    'http://media.laredoute.com/products/641by641/35/00/42/350042718_1_CO_1_7009919.jpg',
    'http://media.laredoute.com/products/641by641/35/00/39/350039748_1_CO_3_6958274.jpg'
);

$beginTime = time();            // 开始下载图片的时间
$lastTime = $beginTime;         // 上一次下载图片的时间
$count = 0;                     // 计数器
echo 'begin download at ' . date('Y-m-d H:i:s', $beginTime) . '<br/>';      // 输出开始下载图片的时间

// 循环下载图片
foreach ($pics as $k => $v) {
    $ch = curl_init();                                      // 初始化curl句柄
    curl_setopt($ch, CURLOPT_URL, $v);                      // 设置要下载的图片
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);            // 设置获取图片内容而不直接在浏览器输出
    curl_setopt($ch, CURLOPT_HEADER, 0);                    // 设置只获取图片内容，不返回header头信息
    $content = curl_exec($ch);                              // 获取图片内容
    curl_close($ch);                                        // 关闭curl句柄
    $picName = substr($v, strrpos($v, '/')+1);              // 获取图片名称
    $savePath = './single/';                                // 设置保存图片的路径
    if (!is_dir($savePath)) {                               // 如果图片路径不存在，则创建路径
        @mkdir($savePath, 0777);
    }
    $saveName = $savePath . $picName;                       // 设置图片保存为的文件名称
    $fp = @fopen($saveName, 'w');                           // 打开文件
    fwrite($fp, $content);                                  // 把获取到的图片内容写入到新图片
    fclose($fp);                                            // 关闭文件句柄
    $nowTime = time();                                      // 当前时间
    $takeTime = $nowTime - $lastTime;                       // 下载此张图片消耗的时间
    ++$count;                                               // 计数器+1
    echo 'downloaded ' . $count . 'th picture take time ' . $takeTime . 's<br/>';               // 输出下载当前图片消耗的时间
    $lastTime = $nowTime;                                   // 把当前时间设置为上一张图片下载时间

}

$endTime = time();                                          // 结束下载图片的时间
$totalTime = $endTime - $beginTime;                         // 总耗时
echo 'end download at ' . date('Y-m-d H:i:s', $endTime) . '<br/>';          // 输出下载图片完成的时间
echo 'downloaded ' . $count . ' pictures take time ' . $totalTime . ' s<br/>';         // 输出总耗时
```

在浏览器执行single.php，程序运行结束后，我们看到要下载的8张图片已经全部下载到了本地：
![00](http://ov13triio.bkt.clouddn.com/2016/12/00.png)

因为在上述代码中写了一部分调试代码，所以在浏览器我们可以看到这样的输出结果：
![42](http://ov13triio.bkt.clouddn.com/2016/12/42.png)

## multi.php：curl多线程下载图片

```php
<?php 

/**
 * 多线程下载远程图片
 * @author 艾逗笔<765532665@qq.com>
 */

set_time_limit(300);                        // 设置程序执行超时时间为5分钟

// 要下载的图片数组
$pics = array(
    'http://media.laredoute.com/products/1200by1200/37/01/01/37010132_90_CO_1_2850804.jpg',
    'http://media.laredoute.com/products/1200by1200/37/01/01/37010132_10_CO_2_2850799.jpg',
    'http://media.laredoute.com/products/1200by1200/57/02/02/57020206_1_CO_1_057020206-9be173d2-6bc3-44ba-b9cb-111010ed0051.jpg',
    'http://media.laredoute.com/products/1200by1200/37/01/01/37010132_10_CO_1_2850800.jpg',
    'http://media.laredoute.com/products/641by641/35/00/42/350042687_1_CO_1_7639577.jpg',
    'http://media.laredoute.com/products/641by641/35/00/34/350034174_1_CO_1_7669890.jpg',
    'http://media.laredoute.com/products/641by641/35/00/42/350042718_1_CO_1_7009919.jpg',
    'http://media.laredoute.com/products/641by641/35/00/39/350039748_1_CO_3_6958274.jpg'
);

$beginTime = time();            // 开始下载图片的时间
$lastTime = $beginTime;         // 上一次下载图片的时间
$count = 0;                     // 计数器
echo 'begin download at ' . date('Y-m-d H:i:s', $beginTime) . '<br/>';      // 输出开始下载图片的时间

// 循环添加curl句柄
$mh = curl_multi_init();                                        // 开启curl多线程
foreach ($pics as $k => $v) {
    $ch[$k] = curl_init();                                      // 初始化curl句柄
    curl_setopt($ch[$k], CURLOPT_URL, $v);                      // 设置要下载的图片
    curl_setopt($ch[$k], CURLOPT_RETURNTRANSFER, 1);            // 设置获取图片内容而不直接在浏览器输出
    curl_setopt($ch[$k], CURLOPT_HEADER, 0);                    // 设置只获取图片内容，不返回header头信息
    curl_multi_add_handle($mh, $ch[$k]);                        // 添加curl多线程句柄    
}

// 开启curl多线程下载图片
do {
    $status = curl_multi_exec($mh, $active);
    $result = curl_multi_info_read($mh);
    if ($result !== false) {
        $content = curl_multi_getcontent($result['handle']);                          // 获取图片内容
        $picName = substr($pics[$count], strrpos($pics[$count], '/')+1);              // 获取图片名称
        $savePath = './multi/';                                 // 设置保存图片的路径
        if (!is_dir($savePath)) {                               // 如果图片路径不存在，则创建路径
            @mkdir($savePath, 0777);
        }
        $saveName = $savePath . $picName;                       // 设置图片保存为的文件名称
        $fp = @fopen($saveName, 'w');                           // 打开文件
        fwrite($fp, $content);                                  // 把获取到的图片内容写入到新图片
        fclose($fp);                                            // 关闭文件句柄
        $nowTime = time();                                      // 当前时间
        $takeTime = $nowTime - $lastTime;                       // 下载此张图片消耗的时间
        ++$count;                                               // 计数器+1
        echo 'downloaded ' . $count . 'th picture take time ' . $takeTime . 's<br/>';               // 输出下载当前图片消耗的时间
        $lastTime = $nowTime;                                   // 把当前时间设置为上一张图片下载时间
    }
} while ($status == CURLM_CALL_MULTI_PERFORM || $active);
curl_multi_close($mh);                                      // 关闭curl多线程句柄

$endTime = time();                                          // 结束下载图片的时间
$totalTime = $endTime - $beginTime;                         // 总耗时
echo 'end download at ' . date('Y-m-d H:i:s', $endTime) . '<br/>';          // 输出下载图片完成的时间
echo 'downloaded ' . $count . ' pictures take time ' . $totalTime . ' s<br/>';         // 输出总耗时
```

对single.php进行简单的修改，我们在multi.php中使用curl_multi来初始化多个curl句柄，使用curl多线程来下载图片，在浏览器执行multi.php，程序执行完毕后我们可以看到远程的8张图片也下载到了本地：
![01](http://ov13triio.bkt.clouddn.com/2016/12/01.png)

依然来看一下浏览器输出的调试结果：
![41](http://ov13triio.bkt.clouddn.com/2016/12/41.png)

> 通过上面的结果对比，我们可以大概的看出使用curl单线程与curl多线程下载图片的效率差别：curl单线程下载是一个同步执行的过程，在循环下载图片的过程中，每张图片的下载耗时基本一致。而curl多线程下载图片是一个异步执行的过程，curl句柄会持续的请求需要下载的图片，直到图片被成功下载为止，平均每张图片的下载耗时明显小于curl单线程下载的耗时。

为了验证上述结论，我们可以多执行几次，看一看二者的对比：
![11](http://ov13triio.bkt.clouddn.com/2016/12/11.png)

![12](http://ov13triio.bkt.clouddn.com/2016/12/12.png)

![21](http://ov13triio.bkt.clouddn.com/2016/12/21.png)

![22](http://ov13triio.bkt.clouddn.com/2016/12/22.png)

![31](http://ov13triio.bkt.clouddn.com/2016/12/31.png)

![32](http://ov13triio.bkt.clouddn.com/2016/12/32.png)

> 通过上面的几次执行结果的对比，我们可以很肯定的说，使用curl多线程下载图片，效率有很大的提升。

## 关于提升业务处理效率的更多想法

针对上面的需求，我们最终要完成的目标是在最短时间内把所需的图片全部下载下来，上面给出的demo都是通过在浏览器执行php脚本来进行图片下载的，但是如果一次性要下载的图片数量过多，网络存在延迟的情况下，很有可能会出现php脚本超时的问题。所以提升业务处理效率的一方面是：我们可以把multi.php放到linux服务器，通过crontab执行定时任务来下载图片。另一方面，为了进一步提升业务处理效率，缩短图片下载总耗时，我们可以使用php的swoole扩展。上文提到的多线程下载图片只是针对curl库的一种异步多线程模式，并非php的多线程，我们知道php是没有多线程的。而swoole扩展刚刚提供了解决了这个问题，使得php也能具备多线程事务处理能力。使用swoole+crontab+curl_multi，我们就可以在短时间内批量下载所需的图片。

更多关于swoole与curl_multi的知识请自行查阅相关资料。本文所用demo源码下载请点此：[download-picture](http://ov13triio.bkt.clouddn.com/2016/12/download-picture.zip)