---
layout: post
title: DELL服务器网卡命名问题
tag: Linux及运维
---

最近在DELL服务器上安装centos，发现网卡名称为em1、em2，与我们平时使用的eth0不同。为了使用方便，修改为eth0，eth1，方法如下

1\. 修改内核启动参数，在kernel后加入 biosdevname=0, 如下修改 /etc/grub.conf

```
kernel /vmlinuz-2.6.32-431.17.1.el6.x86_64 .... rhgb quiet biosdevname=0    //省略了部分参数
```

2\. 修改网卡配置文件，包括文件名和文件内DEVICE名称

```
[root@koalaxiaot ~]# mv /etc/sysconfig/network-scripts/ifcfg-em1 /etc/sysconfig/network-scripts/ifcfg-eth0
[root@koalaxiaot ~]# sed -i 's/DEVICE=.*$/DEVICE=eth0/g' /etc/sysconfig/network-scripts/ifcfg-eth0
```

3\. 清空udev规则


```
[root@koalaxiaot ~]# echo > /etc/udev/rules.d/70-persistent-net.rules
```

eth1的修改方法同eth0

> biosdevname参考：[Support for biosdevname in Red Hat Enterprise Linux 6.1](https://access.redhat.com/articles/53579){:target="_blank"}
