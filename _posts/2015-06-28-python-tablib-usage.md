---
layout: post
title: 使用Tablib导出Excel数据
tag: Python
---

Tablib是一个处理表格数据的Python库，支持很多种导出格式，如JSON、HTML、CSV、TSV、YAML、Dict、Excel。

而今天我们主要操作的就是导出数据到Excel文件。

安装过程很简单，可以直接`yum python-tablib -y`

直接看示例：

{% highlight python linenos %}
#!/bin/env python
# -*- coding: utf-8 -*-
#

import tablib                         #引入Tablib库

data = tablib.Dataset()

data.headers = ('name', 'age')        #定义标题部分

users = []                            #数据
users.append((u'小明','20'))
users.append(('xiaohong','18'))
users.append(('koala','23'))

for (name, age) in users:
    data.append((name, age))          #使用append方法，添加到data中

    print '------ CSV  OUTPUT ------'
    print data.csv
    print '------ JSON OUTPUT ------'
    print data.json

    fp = open('output.xls', 'w')      #写入到外部文件中
    fp.write(data.xls)                #因xls(x)为二进制文件，所以直接print会是乱码
    fp.close()
{% endhighlight %}

在命令行执行结果如下

{% highlight shell-session %}
[root@koala py]# python tabExcel.py 
------ CSV  OUTPUT ------
name,age
小明,20
xiaohong,18
koala,23

------ JSON OUTPUT ------
[
	{"name": "\u5c0f\u660e", "age": "20"}, 
	{"name": "xiaohong", "age": "18"}, 
	{"name": "koala", "age": "23"}
]
{% endhighlight %}

打开`output.xls`可以看到

![output.xls](/uploads/images/2015-10-29-excel.png)

到这里，是Tablib的基本用法，这应该足以满足导出数据的基本需求了。

另外，Tablib还支持一些高级的用法，如添加列(`append_col(['China','USA','China'], header='Country')`)，删除行/列等操作，更多操作可参考官方文档，官方的文档还是很详细的。

> [Tablib官方文档](http://docs.python-tablib.org/en/latest/){:target="_blank"}
