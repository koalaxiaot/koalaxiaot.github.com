---
layout: post
title: logrotate日志不轮转问题解决
category: Linux
---

之前做了[nginx和tomcat日志切割]({site.url}/linux/2014/12/18/nginx-tomcat-log-rotate.html){:target="_blank"} ，之后发现每天凌晨并没有进行切割。经过排查发现原来是selinux的问题，需要将要切割的日志文件所在目录下所有文件都打上 `var_log_t` 标签。

{% highlight shell-session %}
[root@koalaxiaot ~]# chcon -Rv --type=var_log_t /usr/local/nginx/logs/
[root@koalaxiaot ~]# chcon -Rv --type=var_log_t /usr/local/tomcat/logs/
{% endhighlight %}

之后就可以正常按计划任务切割了。
