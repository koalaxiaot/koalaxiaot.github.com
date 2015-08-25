---
layout: post
title: yum Errno 14 问题解决
category: Linux
---

今天在安装openstack的yum源时，出现了如下错误

{% highlight shell-session %}
[root@koalaxiaot yum.repos.d]# yum repolist
Loaded plugins: fastestmirror
Determining fastest mirrors  
http://balabala/repodata/repomd.xml: [Errno 14] problem making ssl connection
Trying other mirror.
http://balabala/repodata/repomd.xml: [Errno 14] problem making ssl connection
Trying other mirror.
{% endhighlight %}

排查应该是ssl证书问题有关，网上查了资料发现需要安装 **ca-certificates** 才行。

{% highlight shell-session %}
[root@koalaxiaot yum.repos.d]# yum install ca-certificates -y
{% endhighlight %}

之后就可以正常使用了~
