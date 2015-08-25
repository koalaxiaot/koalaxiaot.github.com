---
layout: post
title: 手动更新zimbra证书
category: Linux
---

zimbra的证书过期，有时会导致一些意想不到的问题，比如命令无法执行，ldap无法启动等。

{% highlight shell-session %}
Unable to determine enabled services from ldap.
Unable to determine enabled services. Cache is out of date or doesn't exist.
{% endhighlight %}

手动更新步骤如下，以root用户执行：

{% highlight shell-session %}
[root@mailtest zimbra]#/opt/zimbra/bin/zmcertmgr createca -new 
[root@mailtest zimbra]#/opt/zimbra/bin/zmcertmgr deployca 
[root@mailtest zimbra]#/opt/zimbra/bin/zmcertmgr createcrt -new -days 3650
[root@mailtest zimbra]#/opt/zimbra/bin/zmcertmgr deploycrt self 
[root@mailtest zimbra]#/opt/zimbra/bin/zmcertmgr viewdeployedcrt
{% endhighlight %}

最后一条命令查看当前的证书，查看notAfter值，可以看到证书过期时间~
