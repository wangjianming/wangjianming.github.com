---
layout: post
title: "time_in_python"
description: ""
category: ""
tags: []
---
#      python时间处理

![python-time](http://www.zybuluo.com/static/img/toolbar-reader.png)

其中
time.strftime( 格式字符串, 时间对象 )# str format time-->返回时间字符串
time.strptime(时间字符串, 格式字符串)# str parse time-->返回时间对象
其中的格式字符串是规定好的，比如%y代表两位数的年份，%Y代表四位数的年份，具体更详细含义可见
http://docs.python.org/library/time.html#time.struct_time
例子
```python
time.strftime( "%a, %d %b %Y %H:%M:%S +0000", time.localtime() )
>>"Sat, 15 Aug 2009 10:22:44 +0000"
time.strptime("30 Nov 00", "%d %b %y")
>>time.struct_time(tm_year=2000, tm_mon=11, tm_mday=30, tm_hour=0, tm_min=0, tm_sec=0, tm_wday=3, tm_yday=335, tm_isdst=-1)
```
另外还有datetime和calendar
{% include JB/setup %}
