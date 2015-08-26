---
layout: post
title: PHP生成随机密码
tag: PHP
---

经常会用到生成随机密码或者随机数的问题，以下代码做为备忘。

以下代码可实现不同类型密码强度的生成，generate_password($length, $type)函数，可设置长度和类型这2个参数来生成length长度的type类型的随机字符。

{% highlight php linenos %}
<?php
function generate_password($length,$type) {
  $lowers = 'abcdefghijklmnopqrstuvwxyz';   //小写字母集
  $uppers = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ';   //大写字母集
  $numbers = '0123456789';                  //数字集
  $symbols = '!@#$%^&*()-_[]{}<>~`+=,.;:/?|';  //特殊符号集
  $password = '';
  for ( $i = 0; $i < $length; $i++ ) 
  {
    switch ($type) {
      case 'lowers':
        $password .= $lowers[ mt_rand(0, strlen($lowers) - 1) ];
        break;
      case 'uppers':
        $password .= $uppers[ mt_rand(0, strlen($uppers) - 1) ];
        break;
      case 'numbers':
        $password .= $numbers[ mt_rand(0, strlen($numbers) - 1) ];
        break;
      case 'symbols':
        $password .= $symbols[ mt_rand(0, strlen($symbols) - 1) ];
        break;
    }
  }
  return $password;
}
$lowers = generate_password(4,'lowers');               //生成4个小写字母
$uppers = generate_password(2,'uppers');               //2个大写字母
$numbers = generate_password(4,'numbers');             //4个数字
$symbols = generate_password(2,'symbols');             //2个特殊字符
echo str_shuffle($lowers.$uppers.$numbers.$symbols);   //打乱顺序
echo "\n";
?>
{% endhighlight %}

根据密码需求，生成各个类型密码，然后对生成的密码连接起来，使用str_shuffle函数打乱顺序。

测试如下，代码中我们生成了 4个小写、2个大写、4个数字、2个特殊字符组成的密码。

{% highlight shell-session %}
[root@koalaxiaot php]# php generate_pwd.php 
qQ$2Ex9u3a%8
{% endhighlight %}
