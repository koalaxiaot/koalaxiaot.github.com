---
layout: post
title: fping的安装和使用
tag: Linux
---

简单说，fping是ping命令的加强版，可以同时ping多台主机，更多信息可参考文末的参考文档。

###下载安装fping

{% highlight shell-session %}
[root@koala ~]# wget http://www.fping.org/dist/fping-3.10.tar.gz
[root@koala ~]# tar xvzf fping-3.10.tar.gz
[root@koala ~]# cd fping-3.10
[root@koala fping-3.10]# ./configure && make && make install
{% endhighlight %}

安装完成后系统中会有fping的命令，查看fping的帮助:

{% highlight shell-session %}
[root@koala fping-3.10]# fping -h

Usage: fping [options] [targets...]
  -a         show targets that are alive
  -A         show targets by address
  -b n       amount of ping data to send, in bytes (default 56)
  -B f       set exponential backoff factor to f
  -c n       count of pings to send to each target (default 1)
  -C n       same as -c, report results in verbose format
  -D         print timestamp before each output line
  -e         show elapsed time on return packets
  -f file    read list of targets from a file ( - means stdin) (only if no -g specified)
  -g         generate target list (only if no -f specified)
               (specify the start and end IP in the target list, or supply a IP netmask)
               (ex. fping -g 192.168.1.0 192.168.1.255 or fping -g 192.168.1.0/24)
  -H n       Set the IP TTL value (Time To Live hops)
  -i n       interval between sending ping packets (in millisec) (default 25)
  -I if      bind to a particular interface
  -l         loop sending pings forever
  -m         ping multiple interfaces on target host
  -n         show targets by name (-d is equivalent)
  -O n       set the type of service (tos) flag on the ICMP packets
  -p n       interval between ping packets to one target (in millisec)
               (in looping and counting modes, default 1000)
  -q         quiet (don't show per-target/per-ping results)
  -Q n       same as -q, but show summary every n seconds
  -r n       number of retries (default 3)
  -s         print final stats
  -S addr    set source address
  -t n       individual target initial timeout (in millisec) (default 500)
  -T n       ignored (for compatibility with fping 2.4)
  -u         show targets that are unreachable
  -v         show version
  targets    list of targets to check (if no -f specified)
{% endhighlight %}

### 常用参数：

**-a** 显示存活(ping可达)的主机

**-u** 显示ping不可达的主机

**-s** 显示最终统计信息

**-q** 静默输出，即不显示ping的过程，只显示结果，一般配合上述3个参数使用

**-g** 指定ip地址段，参考基本用法2

**-f file** 指定从文件中读取ip地址，不可与-g同时使用，参考基本用法3

### 基本用法 1

`# fping IP1 IP2 IP3 [IPn...]` 即命令后直接接多个想要进行ping的目标IP

如 **ping 192.168.1.3 192.168.1.10 192.168.1.20** 这3台主机

{% highlight shell-session %}
[root@koala ~]# fping 192.168.1.3 192.168.1.10 192.168.1.20
192.168.1.10 is alive
ICMP Host Unreachable from 192.168.1.248 for ICMP Echo sent to 192.168.1.3
ICMP Host Unreachable from 192.168.1.248 for ICMP Echo sent to 192.168.1.3
ICMP Host Unreachable from 192.168.1.248 for ICMP Echo sent to 192.168.1.3
ICMP Host Unreachable from 192.168.1.248 for ICMP Echo sent to 192.168.1.20
ICMP Host Unreachable from 192.168.1.248 for ICMP Echo sent to 192.168.1.20
ICMP Host Unreachable from 192.168.1.248 for ICMP Echo sent to 192.168.1.20
192.168.1.3 is unreachable
192.168.1.20 is unreachable
{% endhighlight %}

可以看到1.10是可达的，另外2台主机ping不可达。

### 基本用法 2

`#fping -g IP1 IP2` ping从IP1到IP2的所有IP

如 **ping 192.168.1.3-192.168.1.10** 的所有主机

{% highlight shell-session %}
[root@koala ~]# fping -aqs -g 192.168.1.3 192.168.1.10
192.168.1.6
192.168.1.7
192.168.1.8
192.168.1.10

    8 targets
    4 alive
    4 unreachable
    0 unknown addresses
    
    4 timeouts (waiting for response)
    20 ICMP Echos sent
    4 ICMP Echo Replies received
    12 other ICMP received

  1.33 ms (min round trip time)
  1.71 ms (avg round trip time)
  2.25 ms (max round trip time)
      4.239 sec (elapsed real time)
{% endhighlight %}

可以看到有4台主机可以ping通，分别是 1.6/1.7/1.8/1.10

另外也支持ping整个网段，如下

{% highlight shell-session %}
[root@koala ~]# fping -aqs -g 192.168.1.0/29
192.168.1.1
192.168.1.2
192.168.1.6

    6 targets
    3 alive
    3 unreachable
    0 unknown addresses
    
    3 timeouts (waiting for response)
    15 ICMP Echos sent
    3 ICMP Echo Replies received
    9 other ICMP received
    
  0.75 ms (min round trip time)
  7.34 ms (avg round trip time)
  19.8 ms (max round trip time)
      4.191 sec (elapsed real time)
{% endhighlight %}

### 基本用法 3

`#fping -f file` 从文件中读取ip进行ping，示例如下

{% highlight shell-session %}
[root@koala ~]# cat ips.txt
192.168.1.3
192.168.1.4
192.168.1.10
[root@koala ~]# fping -uqs -f ips.txt 
192.168.1.3
192.168.1.4

    3 targets
    1 alive
    2 unreachable
    0 unknown addresses
    
    2 timeouts (waiting for response)
    9 ICMP Echos sent
    1 ICMP Echo Replies received
    6 other ICMP received

  1.20 ms (min round trip time)
  1.20 ms (avg round trip time)
  1.20 ms (max round trip time)
      4.116 sec (elapsed real time)
{% endhighlight %}

参考文档：

> [fping官方站点](http://www.fping.org/){:target="_blank"}
> [Github 地址](https://github.com/schweikert/fping){:target="_blank"}
