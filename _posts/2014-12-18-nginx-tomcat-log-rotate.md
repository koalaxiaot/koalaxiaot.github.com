---
layout: post
title: nginx和tomcat日志切割
category: Linux
---

之前源码安装了nginx和tomcat，服务运行了一段时间，发现访问日志达到了2G多，为了方便以后查看和管理方便，于是用logrotate进行日志切割。

在**/etc/logrotate.d/**目录下面分别创建了**nginx**和**tomcat** 2个文件，内容如下

**/etc/logrotate.d/nginx**

{% highlight %}
/usr/local/nginx/logs/access.log
/usr/local/nginx/logs/error.log
{
  daily
  missingok
  dateext
  rotate 15
  sharedscripts
  postrotate
    kill -USR1 $(cat /usr/local/nginx/logs/nginx.pid)
  endscript
}
{% endhighlight %}

每天会进行切割nginx的**access.log**以及**error.log**，总共会保留最近15天的日志。切割完会执行 `kill -USR1` 命令来使得nginx重新打开日志。

以下为执行过程样例：

{% highlight shell-session %}
[root@koalaxiaot logs]# logrotate -vf /etc/logrotate.d/nginx 
reading config file /etc/logrotate.d/nginx
reading config info for /usr/local/nginx/logs/access.log
/usr/local/nginx/logs/error.log


Handling 1 logs

rotating pattern: /usr/local/nginx/logs/access.log
/usr/local/nginx/logs/error.log
forced from command line (15 rotations)
empty log files are rotated, old logs are removed
considering log /usr/local/nginx/logs/access.log
  log needs rotating
considering log /usr/local/nginx/logs/error.log
  log needs rotating
rotating log /usr/local/nginx/logs/access.log, log->rotateCount is 15
dateext suffix '-20141218'
glob pattern '-[0-9][0-9][0-9][0-9][0-9][0-9][0-9][0-9]'
glob finding old rotated logs failed
rotating log /usr/local/nginx/logs/error.log, log->rotateCount is 15
dateext suffix '-20141218'
glob pattern '-[0-9][0-9][0-9][0-9][0-9][0-9][0-9][0-9]'
glob finding old rotated logs failed
renaming /usr/local/nginx/logs/access.log to /usr/local/nginx/logs/access.log-20141218
renaming /usr/local/nginx/logs/error.log to /usr/local/nginx/logs/error.log-20141218
running postrotate script
{% endhighlight %}

**/etc/logrotate.d/tomcat**

{% highlight %}
/usr/local/tomcat/logs/catalina.out
{
  daily
  dateext
  missingok
  rotate 15
  copytruncate
}
{% endhighlight %}

每天会切割tomcat的**catalina.out**日志，保留最近15天日志，每次切割时会先拷贝一份备份，然后清空当前日志（copytruncate）。

以下为执行过程样例：

{% highlight shell-session %}
[root@koalaxiaot logs]# logrotate -vf /etc/logrotate.d/tomcat 
reading config file /etc/logrotate.d/tomcat
reading config info for /usr/local/tomcat/logs/catalina.out


Handling 1 logs

rotating pattern: /usr/local/tomcat/logs/catalina.out
forced from command line (15 rotations)
empty log files are rotated, old logs are removed
considering log /usr/local/tomcat/logs/catalina.out
  log needs rotating
rotating log /usr/local/tomcat/logs/catalina.out, log->rotateCount is 15
dateext suffix '-20141218'
glob pattern '-[0-9][0-9][0-9][0-9][0-9][0-9][0-9][0-9]'
glob finding old rotated logs failed
copying /usr/local/tomcat/logs/catalina.out to /usr/local/tomcat/logs/catalina.out-20141218
truncating /usr/local/tomcat/logs/catalina.out
{% endhighlight %}
