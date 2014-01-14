---
layout: post
title: "把python当日常的”计算器”用"
description: ""
category: ""
tags: [python,]
---
作为程序员，常常看见其他的程序员在windows下打开程序/附件/计算器，觉得很不可思议，我们有很多好用的东西可替代。
随便装一个动态语言在命令行下就运行的很好：notejs和Rhino用js的语法， perl，ruby，jvm系列的groovy也比较流行。 比较熟悉的，就用python举举例子。
正常的加减乘除括号优先级，好像几乎所有语言一样，总之敲键盘比点那个计算器方便多了
{% highlight python %}
>>>12345+324*23
19797
{% endhighlight %}

计算幂
{% highlight python %}
>>>3**4 或 pow(3,4)
81 
{% endhighlight %}
开方也相当于求倒数的幂
{% highlight python %}
>>>2**0.5

1.4142135623730951
{% endhighlight %}
另外通常数字是十进制，前面加0表示八进制，0x表示十六进制，0b表示二进制
{% highlight python %}
>>> 0b101

5
{% endhighlight %}
其他进制如果需要可以自己转，看看“29”在26进制数里表示什么
{% highlight python %}
>>> int("29",26)
61
{% endhighlight %}
这些应该可以足以代替计算器了，但是平常我们会用的更多
ascii码和字符的转换，chr和ord可以理解成相反运算
{% highlight python %}
>>> chr(97)
'a'

>>>ord('a')
97
{% endhighlight %}
关于时间和编码问题，用python在命令行下敲一敲，很好用。
数据库经常会保存int形的时间，看看一个时间戳（since epoch）表示的时间是什么时候，
{% highlight python %}
>>> import time
>>> time.ctime(1347984629)
'Wed Sep 19 00:10:29 2012'
{% endhighlight %}
我看到有人还专门为这个用VC开发一个工具，图形界面的，也很好用，但是不跨平台，到别人机器上还得copy过来……
当然时间也可以转成其他格式，关于时间各种类型转换以及下面的编码问题，都值得另开一篇了，不再详述。
我最常用的使用是在碰到乱码问题时，以“中文”这两个汉字来举例
看看他的unicode是什么
{% highlight python %}
>>> u"中文"
u'\u4e2d\u6587' 
{% endhighlight %}
看他的gbk编码
{% highlight python %}
>>> u"中文".encode("gbk")
'\xd6\xd0\xce\xc4' 
{% endhighlight %}
看看台湾人民怎么表示
{% highlight python %}
>>> u"中文".encode("big5")

'\xa4\xa4\xa4\xe5' 
{% endhighlight %}
再比如如果抓包的时候发现抓到某个字符串内容d6d0cec4 （这里不考虑大端小端的，可以把这个因素按实际情况调整） 看看他是什么，
因为我们碰到编码问题，无非就是GBK，utf-8, iso8859-1,再大不了有utf16,试试就行了,
{% highlight python %}
>>> print '\xd6\xd0\xce\xc4'.decode("gbk")
中文 
{% endhighlight %}
用其他编码试会直接报错的 这个地方凭经验可以猜测，最常用的utf-8表示汉字是3个一组，GBK时两个一组 使用的多了熟悉之后。
很多标准库就可以拿来提高效率，比如，想一下子生成多个目录结构,a下放个b，b下放个c，又不想创建一个，进去再创建一个……。*nix下mkdir有个-p参数，用下面这招虽然麻烦点，但windows也能用。
{% highlight python %}
>>> import os
>>> os.makedirs("a/b/c/d")
{% endhighlight %}
只想提出来抛砖引玉一下，熟悉perl的，熟悉ruby的，熟悉shell的，可以各挖掘自己熟悉的，各取所需：）

{% include JB/setup %}
