---
layout: post
title: gcc编译math.h方法
category: 'Linux C'
---

在用gcc编译C时，碰到了undefined reference to `sin’的错误，源代码如下

{% highlight c linenos %}
#include <math.h>
#include <stdio.h>

int main(int argc, char *argv[]){
  double pi = 3.14159;
  printf("sin(pi/2)=%f\nln1=%f\n", sin(pi/2), log(1.0));
  return 0;
}
{% endhighlight %}

gcc编译提示出错

{% highlight shell-session %}
[root@koalaxiaot c]# gcc math.c -o math
/tmp/ccN4DXtA.o: In function `main':
math.c:(.text+0x28): undefined reference to `sin'
collect2: ld returned 1 exit status
{% endhighlight %}

解决办法：使用 -lm 参数。

gcc默认是调用 glibc.so 库文件，-lc 是默认选项，math位于 glibm.so 中，因此需要添加 -lm 参数指定。

{% highlight shell-session %}
[root@koalaxiaot c]# gcc math.c -o math -lm     //添加了-lm参数
[root@koalaxiaot c]# ./math 
sin(pi/2)=1.000000
ln1=0.000000
{% endhighlight %}
