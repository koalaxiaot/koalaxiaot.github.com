---
layout: post
title: sysbench安装排错
tag: Linux
---

到github上下载最新版的sysbench并安装

{% highlight shell-session %}
[root@koala ~]# wget https://github.com/akopytov/sysbench/archive/0.5.zip
[root@koala ~]# unzip 0.5.zip
[root@koala ~]# cd sysbench-0.5/
[root@koala sysbench-0.5]# ./autogen.sh 
automake 1.10.x (aclocal) wasn't found, exiting
{% endhighlight %}

提示报错: ***automake 1.10.x (aclocal) wasn’t found， exiting***
需要安装 **libtool** 这个包依赖，libtool主要包括 **autoconf** 和 **automake** 这2个工具, 当然可以只暂时安装 **automake** 这个包，不过后续会继续报错，所以就直接安装了 **libtool** 更直接些。使用yum安装并继续。

{% highlight shell-session %}
[root@koala sysbench-0.5]# yum install libtool -y

[root@koala sysbench-0.5]# ./autogen.sh 
./autogen.sh: running `aclocal -I m4' 
./autogen.sh: running `libtoolize --copy --force' 
libtoolize: putting auxiliary files in AC_CONFIG_AUX_DIR, `config'.
libtoolize: copying file `config/ltmain.sh'
libtoolize: putting macros in AC_CONFIG_MACRO_DIR, `m4'.
libtoolize: copying file `m4/libtool.m4'
libtoolize: copying file `m4/ltoptions.m4'
libtoolize: copying file `m4/ltsugar.m4'
libtoolize: copying file `m4/ltversion.m4'
libtoolize: copying file `m4/lt~obsolete.m4'
./autogen.sh: running `autoheader' 
./autogen.sh: running `automake -c --foreign --add-missing' 
configure.ac:23: installing `config/compile'
configure.ac:11: installing `config/config.guess'
configure.ac:11: installing `config/config.sub'
configure.ac:16: installing `config/install-sh'
configure.ac:16: installing `config/missing'
sysbench/Makefile.am: installing `config/depcomp'
./autogen.sh: running `autoconf' 
Libtoolized with: libtoolize (GNU libtool) 2.2.6b
Automade with: automake (GNU automake) 1.11.1
Configured with: autoconf (GNU Autoconf) 2.63
{% endhighlight %}

成功生成了configure文件，接下来就是正常的编译安装了

{% highlight shell-session %}
[root@koala sysbench-0.5]# ./configure
[root@koala sysbench-0.5]# make && make install
{% endhighlight %}

注意：如果在make过程中提示 **error: mysql.h: No such file or directory** 报错的话，须先安装 **mysql-devel** 这个依赖（ *yum install mysql-devel -y* ），然后重新编译安装即可。

至此，sysbench安装完毕。


参考文档：

> [维基百科Libtool](http://zh.wikipedia.org/wiki/Libtool){:target="_blank"}
> [sysbench Github地址](https://github.com/akopytov/sysbench){:target="_blank"}
