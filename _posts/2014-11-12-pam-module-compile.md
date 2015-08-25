---
layout: post
title: pam模块编译链接
category: 'Linux C'
---

在用写pam模块编译时，需要执行特定的编译命令，经常忘记参数，特备忘之。

{% highlight shell-session %}
[root@koalaxiaot c]# gcc -fPIC -c pam.c
[root@koalaxiaot c]# ld -x --shared -o /lib64/security/mypam.so pam.o
{% endhighlight %}

pam.c为自己写的pam模块

如果是32位系统，则路径为 /lib/security/mypam.so

之后可以在 /etc/pam.d/下根据需要使用 mypam.so 模块文件
