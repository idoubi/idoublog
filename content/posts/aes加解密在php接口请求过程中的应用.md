---
title: AES加解密在php接口请求过程中的应用
tags:
  - AES加解密
  - php
id: 679
categories:
  - 开发笔记
date: 2016-10-25 12:10:37
---

在php请求接口的时候，我们经常需要考虑的一个问题就是数据的安全性，因为数据传输过程中很有可能会被用fillder这样的抓包工具进行截获。一种比较好的解决方案就是在客户端请求发起之前先对要请求的数据进行加密，服务端api接收到请求数据后再对数据进行解密处理，返回结果给客户端的时候也对要返回的数据进行加密，客户端接收到返回数据的时候再解密。因此整个api请求过程中数据的安全性有了一定程度的提高。

今天结合一个简单的demo给大家分享一下AES加解密技术在php接口请求中的应用。

## AES加解密基础类：

```
<?php

/**
 * 加密基础类
 */

class Crypt_AES
{
    protected $_cipher = "rijndael-128";
    protected $_mode = "cbc";
    protected $_key;
    protected $_iv = null;
    protected $_descriptor = null;

    /**
     * 是否按照PKCS #7 的标准进行填充
     * 为否默认将填充“&#92;&#48;”补位
     * @var boolean
     */
    protected $_PKCS7 = false;

    /**
     * 构造函数，对于密钥key应区分2进制字符串和16进制的。
     * 如需兼容PKCS#7标准，应选项设置开启PKCS7，默认关闭
     * @param string $key  
     * @param mixed $iv      向量值
     * @param array $options
     */
    public function __construct($key = null, $iv = null, $options = null)
    {
        if (null !== $key) {
            $this->setKey($key);
        }

        if (null !== $iv) {
            $this->setIv($iv);
        }

        if (null !== $options) {
            if (isset($options['chipher'])) {
                $this->setCipher($options['chipher']);
            }

            if (isset($options['PKCS7'])) {
                $this->isPKCS7Padding($options['PKCS7']);
            }

            if (isset($options['mode'])) {
                $this->setMode($options['mode']);
            }
        }
    }

    /**
     * PKCS#7状态查看，传入Boolean值进行设置
     * @param  boolean  $flag
     * @return boolean
     */
    public function isPKCS7Padding($flag = null)
    {
        if (null === $flag) {
            return $this->_PKCS7;
        }
        $this->_PKCS7 = (bool) $flag;
    }

    /**
     * 开启加密算法
     * @param  string $algorithm_directory locate the encryption 
     * @param  string $mode_directory
     * @return Crypt_AES
     */
    public function _openMode($algorithm_directory = " , $mode_directory = ") 
    {
        $this->_descriptor = mcrypt_module_open($this->_cipher, 
                                                $algorithm_directory, 
                                                $this->_mode,
                                                $mode_directory);
        return $this;
    }

    public function getDescriptor()
    {
        if (null === $this->_descriptor) {
            $this->_openMode();
        }
        return $this->_descriptor;
    }

    protected function _genericInit()
    {
        return mcrypt_generic_init($this->getDescriptor(), $this->getKey(), $this->getIv());
    }

    protected function _genericDeinit()
    {
        return mcrypt_generic_deinit($this->getDescriptor());
    }

    public function getMode()
    {
        return $this->_mode;
    }

    public function setMode($mode)
    {
        $this->_mode = $mode;
        return $this;
    }

    public function getCipher()
    {
        return $this->_cipher;
    }

    public function setCipher($cipher)
    {
        $this->_cipher = $cipher;
        return $this;
    }    
    /**
     * 获得key
     * @return string
     */
    public function getKey()
    {
        return $this->_key;
    }

    /**
     * 设置可以
     * @param string $key
     */
    public function setKey($key)
    {
        $this->_key = $key;
        return $this;
    }    

    /**
     * 获得加密向量块,如果其为null时将追加当前Descriptor的IV大小长度
     *
     * @return string
     */
    public function getIv()
    {
        if (null === $this->_iv && in_array($this->_mode, array( "cbc", "cfb", "ofb", ))) {
            $size = mcrypt_enc_get_iv_size($this->getDescriptor());
            $this->_iv = str_pad(", 16, "&#92;&#48;");
        }
        return $this->_iv;
    }

    /**
     * 获得向量块
     *
     * @param  string $iv
     * @return Crypt_AES $this
     */
    public function setIv($iv)
    {
        $this->_iv = $iv;
        return $this;
    }   

    /**
     * 加密
     * @param  string $str 被加密文本
     * @return string
     */
    public function encrypt($str){
        $td = $this->getDescriptor();
        $this->_genericInit();
        $bin = mcrypt_generic($td, $this->padding($str));
        $this->_genericDeinit();

        return $bin;
    }

    public function padding($dat)
    {
        if ($this->isPKCS7Padding()) {
            $block = mcrypt_enc_get_block_size($this->getDescriptor());

            $len = strlen($dat);
            $padding = $block - ($len % $block);
            $dat .= str_repeat(chr($padding),$padding);            
        }

        return $dat;
    }

    public function unpadding($str)
    {
        if ($this->isPKCS7Padding()) {
            $pad = ord($str[($len = strlen($str)) - 1]);
            $str = substr($str, 0, strlen($str) - $pad);
        }
        return $str;
    }

    /**
     * 解密
     * @param  string $str 
     * @return string
     */
    public function decrypt($str){
        $td = $this->getDescriptor();

        $this->_genericInit();
        $text = mdecrypt_generic($td, $str);
        $this->_genericDeinit();

        return $this->unpadding($text);
    }

    /**
     * 16进制转成2进制数据
     * @param  string $hexdata 16进制字符串
     * @return string
     */
    public static function hex2bin($hexdata) 
    {
        return pack("H*" , $hexdata);
    }

    /**
     * 字符串转十六进制
     * @param  string $hexdata 16进制字符串
     * @return string
     */
    public static function strToHex($string)
    {
        $hex='';
        for($i=0;$i<strlen($string);$i++)
            $hex.=dechex(ord($string[$i]));
        $hex=strtoupper($hex);
        return $hex;
    }

    /**
     * 十六进制转字符串
     * @param  string $hexdata 16进制字符串
     * @return string
     */
    function hexToStr($hex)
    {
        $string='';
        for($i=0;$i<strlen($hex)-1;$i+=2)
            $string.=chr(hexdec($hex[$i].$hex[$i+1]));
        return  $string;
    }
}
```

## 客户端请求部分：
```
<?php 

include 'AES.php';

$md5Key = 'ThisIsAMd5Key';                              // 对应服务端：$md5key = 'ThisIsAMd5Key';
$aesKey = Crypt_AES::strToHex('1qa2ws4rf3edzxcv');      // 对应服务端：$aesKey = '3171613277733472663365647A786376';
$aesKey = Crypt_AES::hex2bin($aesKey);
$aesIV  = Crypt_AES::strToHex('dfg452ws');              // 对应服务端：$aesIV = '6466673435327773';
$aes = new Crypt_AES($aesKey,$aesIV,array('PKCS7'=>true, 'mode'=>'cbc'));

// var_dump($aes);

$data['name'] = 'idoubi';
$data['sex']= 'male';
$data['age'] = 23;
$data['signature'] = '白天我是一个程序员，晚上我就是一个有梦想的演员。';

$content = base64_encode($aes->encrypt(json_encode($data)));
$content = urlencode($content);
$sign = md5($content.$md5Key);

$url = 'http://localhost/aesdemo/api.php';
$params = "version=1.0&sign=$sign&content=$content";

// 请求接口
post($url, $params);

/**
 * 接口请求函数
 */
function post($url, $params) {
    $curlPost= $params;
    $ch = curl_init();      //初始化curl
    curl_setopt($ch, CURLOPT_URL, $url);    //提交到指定网页
    curl_setopt($ch, CURLOPT_HEADER, 0);    //设置header
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);   //要求结果为字符串且输出到屏幕上
    curl_setopt($ch, CURLOPT_POST, 1);    //post提交方式
    curl_setopt($ch, CURLOPT_POSTFIELDS, $curlPost);
    $result = curl_exec($ch);//运行curl
    curl_close($ch);
    var_dump(json_decode($result, true));
}
```

## 接口处理逻辑：
```
<?php 

include 'AES.php';

$data = $_POST;  // 接口请求得到的数据
$content = $data['content'];
$sign = $data['sign'];

$aesKey = '3171613277733472663365647A786376';
$aesIV = '6466673435327773';
$md5key = 'ThisIsAMd5Key';

// 校验数据
if(strcasecmp(md5(urlencode($content).$md5key),$sign) == 0) {
    // 数据校验成功
    $key = Crypt_AES::hex2bin($aesKey);
    $aes = new Crypt_AES($key, $aesIV, array('PKCS7'=>true, 'mode'=>'cbc'));

    $decrypt = $aes->decrypt(base64_decode($content));
    if (!$decrypt) {      // 解密失败
        echo json_encode('can not decrypt the data');
    } else {
        echo json_encode($decrypt);     // 解密成功
    }
} else{
    echo json_encode('data is not integrity');       // 数据校验失败
}

```

上述接口请求过程中定义了三个加解密需要用到的参数：$aesKey、$aesIV、$md5key，在实际应用过程中，只要与客户端用户约定好这三个参数，客户端程序员利用这几个参数对要请求的数据进行加密后再请求接口，服务端程序员在接收到数据后利用同样的加解密参数对数据进行解密，整个api请求过程中的数据就很安全了。

[aesdemo](http://ov13triio.bkt.clouddn.com/2016/10/aesdemo.zip)