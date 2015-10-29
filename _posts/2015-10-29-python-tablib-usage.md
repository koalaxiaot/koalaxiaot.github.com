---
layout: post
title: 使用tablib导出Excel数据
tag: Python
---

Tablib是一个处理表格数据的Python库，支持很多种导出格式，如JSON、HTML、CSV、TSV、YAML、Excel。

而今天我们主要操作的就是导出数据到Excel文件。

安装过程很简单，可以直接`yum python-tablib -y`

直接看示例：

{% highlight python %}
#!/bin/env python
# -*- coding: utf-8 -*-
#

import tablib    #引入tablib库

data = tablib.Dataset()

data.headers = ('name', 'age')    #定义标题部分

users = []                        #数据
users.append((u'小明','20'))
users.append(('xiaohong','18'))
users.append(('koala','23'))

for (name, age) in users:
	  data.append((name, age))      #使用append方法，添加到data中

	  print data.json               #打印json格式，同理可打印csv(`print data.csv`)等格式

	  fp = open('output.xls', 'w')  #写入到外部文件中
	  fp.write(data.xls)            #因xls(x)为二进制文件，所以直接print会是乱码
	  fp.close()
{% endhighlight %}

在命令行执行结果如下


{% highlight shell-session %}
[root@lyra1 py]# ./tabExcel.py 
[{"name": "\u5c0f\u660e", "age": "20"}, {"name": "xiaohong", "age": "18"}, {"name": "koala", "age": "23"}]
{% endhighlight %}

打开`output.xls`可以看到

![output.xls](/uploads/images/2015-10-29-excel.png)
