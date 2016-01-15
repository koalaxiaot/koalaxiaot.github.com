---
layout: post
title: 使用pysvn获取提交日志记录
tag: Python
---

最近在项目中需要程序去自动抓取svn服务的日志记录，网上搜寻半天，感觉pysvn这个库不错，挺适合来使用，就拿来一试。

### pysvn安装

可以直接`yum install pysvn -y`或者`pip install pysvn`

### 使用

先看示例：

{% highlight python linenos %}
#!/usr/bin/python
# -*- coding:utf-8 -*-

import pysvn

class svnSync:

    def __init__(self, url):

        self.svn_user="svn_user"
        self.svn_password='svn_pass'

        # 日志条数
        self.log_num = 10

    # 获取日志
    def getLog(self):

        try:
            client = pysvn.Client()
            #参考 http://pysvn.tigris.org/docs/pysvn_prog_ref.html#pysvn_client_callback_get_login
            client.callback_get_login = self.svn_login

            # 参考 http://pysvn.tigris.org/docs/pysvn_prog_ref.html#pysvn_client_log
            log = client.log(self.svn_url, revision_start=pysvn.Revision(pysvn.opt_revision_kind.head),limit=self.log_num)

            for info in log:
                logAuthor = info.author
                logTime = time.strftime('%Y-%m-%d %H:%M:%S', time.localtime(info.date))

        except Exception, e:

            print 'svn error: ', e

    # svn 登录验证函数
    def svn_login(self, realm, username, may_save):
        return True, self.svn_user, self.svn_password, False

if __name__ == '__main__':
    url = 'http://svn.example.com/svn_repo'
    svnSync(url)
{% endhighlight%}

核心是`client.log()`函数

返回的日志信息中，如果需要查看详细的提交日志信息，可以添加`discover_changed_paths=True`，这样就可以查看到这次提交日志的详细文件信息，包括path，action，copyfrom_path，copyfrom_revision。

如果需要查看某个时间段的日志记录，可以修改对应的`revision_start`和`revision_end`参数。比如

`revision_end = pysvn.Revision( pysvn.opt_revision_kind.date, time.time() )`

本示例中是通过最后的head来取limit条记录

`pysvn.Revision( pysvn.opt_revision_kind.head )`

如果需要根据提交的commit ID来获取，可以

`pysvn.Revision( pysvn.opt_revision_kind.number, 4721 )`

更详细的使用方法，可以查看文档。

> [pysvn手册](http://pysvn.tigris.org/docs/pysvn.html){:target="blank"}
