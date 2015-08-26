---
layout: post
title: zimbra mta 启动失败问题解决
tag: Linux
---

今天在测试安装新的zimbra时，出现了mta启动失败的情况。

现象是在 `$zmcontral start` 过程中启动都正常成功，但是 查看服务状态时发现mta是stopped状态

{% highlight shell-session %}
[zimbra@test1 ~]$ zmcontrol status
Host test1.zimbratest.com
antispam                Running
antivirus               Running
imapproxy               Running
ldap                    Running
logger                  Running
mailbox                 Running
memcached               Running
mta                     Stopped
  postfix is not running
{% endhighlight %}
查看 **/var/spool/mail/zimbra** 中发现提示

**postqueue: fatal: Queue report unavailable - mail system is down**

后来发现原来本机的sendmail服务已经启动，占用了25端口导致。于是杀掉sendmail服务

{% highlight shell-session %}
[root@test1 ~]# pkill sendmail
[root@test1 ~]# chkconfig sendmail off
{% endhighlight %}

之后重启zimbra，服务状态都正常了。
