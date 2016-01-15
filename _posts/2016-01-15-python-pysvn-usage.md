---
layout: post
title: 使用pysvn获取提交日志记录
tag: Python
---

最近在项目中需要程序去自动抓取svn服务的日志记录，网上搜寻半天，感觉pysvn这个库不错，挺适合来使用，就拿来一试。

先看示例：

{% highlight python linenos %}
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

> [pysvn手册](http://pysvn.tigris.org/docs/pysvn.html){:target="blank"}
