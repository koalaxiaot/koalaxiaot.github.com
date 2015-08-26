---
layout: post
title: 使用parse_ini_file()读取配置文件
tag: PHP
---

项目开发中使用php时，遇到需要定义一些全局环境变量的问题，经常是放到一个配置文件中，如 `config.ini`，然后使用 `parse_ini_file()`将其引用起来。示例如下，

{% highlight ini linenos %}
[global]
website = http://www.koalaxiaot.me
author = koalaxiaot
version = 0.0.1
[website]
dir = /var/www/html
name = myWeb
{% endhighlight %}

在index.php中引用这个配置文件

{% highlight php linenos %}
<?php
  $configFile = "conf/config.ini";
  $config = parse_ini_file($configFile, true);    //true返回一个多维数组
  print_r($config);
?>
{% endhighlight %}

输入结果如下

{% highlight shell-session %}
[root@koala ~]# php index.php
Array
(
  [global] => Array
    (
      [website] => http://www.koalaxiaot.me
      [author] => koalaxiaot
      [version] => 0.0.1
    )

  [website] => Array
    (
      [dir] => /var/www/html
      [name] => myWeb
    )

)
{% endhighlight %}
