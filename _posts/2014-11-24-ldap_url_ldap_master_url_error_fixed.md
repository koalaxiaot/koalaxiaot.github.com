---
layout: post
title: ldap_url and ldap_master_url cannot be the same on an ldap replica 错误解决
category: Linux
---

在启动zimbra服务时候碰到了如下错误

{% highlight shell-session %}
[zimbra@koalaxiaot ~]$ zmcontrol start
Host localhost
Connect: Unable to determine enabled services from ldap.
Enabled services read from cache. Service list may be inaccurate.
	Starting ldap...Done.
	Failed.
	ldap_url and ldap_master_url cannot be the same on an ldap replica
{% endhighlight %}

经过排查发现是 /opt/zimbar/conf/localconfig.xml 文件权限问题，因为之前已root用户修改了这个文件，导致文件所有者和所属组变为了root。

于是修改所有者说数组

{% highlight shell-session %}
[root@koalaxiaot ~]$ ll /opt/zimbra/conf/localconfig.xml 
-rw-r----- 1 root root 4302 Nov 24 10:28 /opt/zimbra/conf/localconfig.xml
 
[root@koalaxiaot ~]$ chown zimbra.zimbra /opt/zimbra/conf/localconfig.xml
 
[root@koalaxiaot ~]$ ll /opt/zimbra/conf/localconfig.xml 
-rw-r----- 1 zimbra zimbra 4302 Nov 24 10:28 /opt/zimbra/conf/localconfig.xml
{% endhighlight %}

然后就可以正常启动服务了。
