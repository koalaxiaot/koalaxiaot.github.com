---
layout: post
title: PHP获取命令行参数
tag: PHP
---

有时需要在linux的shell环境下测试PHP脚本，需要调用参数，使用到的是PHP的 `$_SERVER` 预定义变量。

{% highlight php linenos %}
<?php
  $argc = $_SERVER["argc"];
  $argv = $_SERVER["argv"];
  echo $argc."\n";
  print_r($argv);
?>
{% endhighlight %}

其中argc为参数的个数，argv为参数数组。

{% highlight shell-session %}
[root@koalaxiaot php]# php args.php arg1 arg2 arg3
Array
(
  [0] => args.php
  [1] => arg1
  [2] => arg2
  [3] => arg3
)
{% endhighlight %}

第一个参数为我们执行的文件名，其后是各个参数，根据获取参数我们可以再在php中执行判断代码来进一步实现我们需要的功能。

另外，`$_SERVER` 数组中包含很多系统环境变量信息，如当前工作目录 `PWD` 、主机名 `HOSTNAME` 等信息。
