---
layout: post
title: 解决zimbra手动修改ldap后缓存问题
tag: Linux
---

在zimbra使用时，为了方便用户重置密码，后台专门写了程序修改ldap值，但是出现了修改后无法及时生效问题。

出现的现象是每次修改完用户信息后，大约要等几分钟才能生效，最多10几分钟左右，困扰了很久。

起初是怀疑系统时间问题，以为时间有误差导致无法及时生效，也怀疑过时区问题，但就是无法找到问题所在。

参考了网上很多文档，最后发现原来是ldap cache问题。

> 参考文档：[Flushing LDAP Cache](http://www.zimbra.com/docs/os/6.0.10/administration_guide/5_Zimbra_LDAP.05.7.html){:target="_blank"}

zimbra的ldap关于account的缓存默认是15分钟，20000个。

根据文档，是需要手动执行命令 `zmprov flushCache account` 刷新，这样很不方便，我们无法知道用户什么时候调用程序接口修改了ldap。

于是考虑修改系统配置，将缓存大小设置为0，关闭缓存。修改ldap_cache_account_maxsize参数

```
[zimbra@mailtest ~]$ zmlocalconfig -e ldap_cache_account_maxsize=0
[zimbra@mailtest ~]$ zmmailboxdctl reload      //须zimbra用户执行，否则报错
Stopping mailboxd...done.
Starting mailboxd...done.
```

修改完参数后，须重启服务，经验证，只需重启zmmailboxdctl即可，因为重启全部服务时间太久，影响线上服务。

再次手动修改或用程序修改ldap属性值时，可及时生效。

