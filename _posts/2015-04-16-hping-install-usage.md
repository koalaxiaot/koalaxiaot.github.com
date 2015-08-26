---
layout: post
title: hping的安装和使用
tag: Linux
---

之前介绍了一篇关于fping的文章《[fping的安装和使用](/linux/2015/03/03/fping-install-usage.html){:target="_blank"}》，今天介绍另外一个也是ping的加强版工具hping。

普通我们进行ping(fping)检测主机都是ICMP协议的，而fping则除了支持ICMP之外，还支持TCP、UDP等协议，可见其强大。

从github下载安装hping

{% highlight shell-session %}
[root@koala downloads]# git clone https://github.com/antirez/hping.git
Initialized empty Git repository in /usr/local/downloads/hping/.git/
remote: Counting objects: 225, done.
remote: Total 225 (delta 0), reused 0 (delta 0), pack-reused 225
Receiving objects: 100% (225/225), 778.99 KiB | 52 KiB/s, done.
Resolving deltas: 100% (35/35), done.
[root@koala downloads]# cd hping
[root@koala hping]# ./configure
build byteorder.c...
create byteorder.h...
./configure: line 81: -: command not found
--------------------------------------
system type: LINUX

LIBPCAP      : PCAP=-lpcap
PCAP_INCLUDE : 
MANPATH      : /usr/local/man
USE_TCL      : -DUSE_TCL
TCL_VER      : 
TCL_INC      : 
LIBTCL       : -ltcl -lm -lpthread
TCLSH        : 

(to modify try configure --help)
--------------------------------------
creating Makefile...
creating dependences...
now you can try `make'
[root@koala hping]# yum install libpcap-devel tcl-devel -y
[root@koala hping]# ln -s /usr/include/pcap-bpf.h /usr/include/net/bpf.h
[root@koala hping]# make && make install
{% endhighlight %}

其中安装了依赖包 **libpcap-devel**、**tcl-devel** 以及对应创建了一个软链接。

使用示例

{% highlight shell-session %}
[root@koala ~]# hping -p 80 -c 3 -S www.baidu.com
HPING www.baidu.com (eth0 61.135.169.121): S set, 40 headers + 0 data bytes
len=46 ip=61.135.169.121 ttl=56 id=27868 sport=80 flags=SA seq=0 win=512 rtt=1.6 ms
len=46 ip=61.135.169.121 ttl=56 id=38818 sport=80 flags=SA seq=1 win=512 rtt=1.6 ms
len=46 ip=61.135.169.121 ttl=56 id=59274 sport=80 flags=SA seq=2 win=512 rtt=2.9 ms
 
--- www.baidu.com hping statistic ---
3 packets tramitted, 3 packets received, 0% packet loss
round-trip min/avg/max = 1.6/2.0/2.9 ms
{% endhighlight %}

上例我们对baidu的80端口进行了3次hping检测。

这种比较适合有些网站或IP禁止了ICMP协议，我们可以进行这种端口式的ping操作来检测可达性。


参考文档：

> [hping 官方站点](http://www.hping.org/){:target="_blank"}
> [Github地址](https://github.com/antirez/hping){:target="_blank"}
