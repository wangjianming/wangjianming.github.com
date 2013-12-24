---
layout: post
title: "windows上抓dump"
description: ""
category: ""
tags: [dump]
---
#总结

用procdump或userdump抓取当前正常运行进程dump文件

用userdump或设置注册表（windows7 & windows2008可用）获取进程奔溃的堆栈

如果自己机器上的问题，就只用windbg好了，因为分析还要靠windbg，windbg是全能的  

#其他方法
##userdump  
配置监控进程core时自动生成dump  
安装userdump后在控制面板设置，可监控进程，待进程core时自动取dump  


对运行中的进程生成dump文件(userdump 不支持 Vista 系统?)  
userdump.exe 12345   xxx.dump  
userdump.exe imapsysd.exe xxx.dump  
userdump.exe -m  123  456  -d d:\userdump  
userdump.exe -m  a.exe b.exe  -d d:\userdump  

##任务管理器功能
windows7 可以在任务管理器中，选中进程，右键生成转储文件（生成User Mini Dump File with Full Memory，可能类似/ma）

##windbg
用windbg附到进程上，执行.dump命令，常用.dump /f xxxx.dump  （或参数用/ma）
此法也可以定位coredump时的问题，如果有代码还可以直接定位到代码行

##windbg中的工具
ntsd.exe，cdb.exe或adplus.vbs（内部调用了cdb） 脚本

把 ntsd 设置为即时调试器，当程序崩溃后可以获得 dump 文件。
ntsd.exe -pv -p %ld -e %ld -g -noio -c ".dump /ma /u d:\dbgdmp\jit.dmp;q"

    -pv         非侵入式附加
    -p PID      指定 PID
    -e Event    设置为即时调试器时才需要，表示传递一个事件给调试器
    -g          附加后不设置初始断点
    -noio       可以调试任何账户下的进程，不会创建命令行窗口
    -c "cmd"    执行命令

ntsd.exe -pv -p PID -g -noio -c ".dump /ma /u d:\dbgdmp\jit.dmp;q"


adplus用法

adplus -crash -p 2972 -o d:\dbgdmp  
在崩溃前执行，会启动 cdb 调试器监视进程，崩溃时会生成 dump 文件。

adplus -hang -p 2140 -o d:\dbgdmp  
相当于 userdump 的功能，直接生成 dump 文件。

##procdump

Procdump是一个轻量级的Sysinternal团队开发的命令行工具, 它的主要目的是监控应用程序的CPU异常动向, 并在此异常时生成crash dump文件
procdump.exe  -ma test.exe|或pid  hello.dump
procdump可以抓取cpu或内存达到设定阈值时自动抓dump

另一个示例
procdump -ma -c 50 -s 3 -n 2 5844 (Process Name or PID)  -0 c:\dumpfile

    -ma 生成full dump, 即包括进程的所有内存. 默认的dump格式包括线程和句柄信息.
    -c 在CPU使用率到达这个阀值的时候, 生成dump文件.
    -s CPU阀值必须持续多少秒才抓取dump文件.
    -n 在该工具退出之前要抓取多少个dump文件.
    -o dump文件保存目录. 

##drwtsn32
示例略
##DebugDiag

可以选择监视的进程，崩溃后会生成 dump 文件。
The Debug Diagnostic Tool (DebugDiag) is designed to assist in troubleshooting issues such as hangs, slow performance, memory leaks or fragmentation, and crashes in any user-mode process. 

##windows7 & windows 2008 设置注册表项支持

如下表示imapsvcd.exe进程core时自动生成dump到d:\temp目录

**导入注册表格式**

	Windows Registry Editor Version 5.00    
  
	[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps\imapsvcd.exe]  
	"DumpCount"=dword:00000005  
	"DumpType"=dword:00000002  
	"DumpFolder"=hex(2):64,00,3a,00,5c,00,74,00,65,00,6d,00,70,00,00,00  


其中  

	>>binascii.b2a_hex('d:\\temp')  
	'643a5c74656d70'


**参考**  
<a href="http://msdn.microsoft.com/en-us/library/windows/hardware/ff560033(v=vs.85).aspx">msdn User-Mode Dump Files</a>  
<a href="http://msdn.microsoft.com/en-us/library/windows/hardware/ff538058(v=vs.85).aspx">Analyzing a User-Mode Dump File with WinDbg</a>

{% include JB/setup %}
