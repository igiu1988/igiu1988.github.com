---
layout: post
title: "power your debug in xcode"
date: 2013-08-05 10:29
keywords: xcode, debug, breakpoint condition, LLDB, GDB, change variable debug
comments: true
categories: 
---

在 Xcode 里 debug 时你会不会有这种想法：在一个 for 循环里，我只需要在 i==102时才开始 debug；或者一个比较实际的例子，我想在 tableView:cellForRowAtIndexPath:当 indexPath 是[15, 0]才开始 debug。
我肯定有那么一部分人会和我一样在那个地方写一个类似这样的代码，在 nslog 处打个断点。

```objective-c
	if (i == 102)
	{
		NSLog(@"a");
	}
```

看完这篇文章你就不需要这么做了，你会知道更多高级的技巧。**<font color='red' size='5'>Power your debug in xcode!!!</font>**

该文包括

* 让断点在合适的时候停下
* 断点条件语法注意
* 在断点条件发生时执行特别的任务
* 在调试时改变变量的值

<!--more-->

##让断点在合适的时候停下
在断点箭头上右键，Edit Breakpoint，在 Conditions 里添加断点条件。解释图1.

下面这段代码取自一个 for 循环，```for (int i = 0; i < _channels.count; i++)``` , 我想在 i == _channels.count - 2 时开始进行断点调试。如图添加断点条件，然后运行一下，你就会发现奇迹了。

->![断点条件](http://h.hiphotos.bdimg.com/album/s%3D550%3Bq%3D90%3Bc%3Dxiangce%2C100%2C100/sign=f3d78ed918d8bc3ec20806cfb2b0d723/810a19d8bc3eb135acac8c58a71ea8d3fc1f44ed.jpg?referer=e248dcc80ff41bd58344dcc4a4a3&x=.jpg)<-

->图1<-

##断点条件语法注意
```
	i >= (int)[_channels count] - 2
```
我猜想，这个功能是 xcode 基于调试器（GDB或者LLDB）的高级封装，所以一些调试语句的语法仍要使用调试器能够支持的语法。
**比如图1中的测试条件，如果写成_channels.count，那么调试器会报错。所以我们就注意些，本来可以使用 dot 语法的都改为使用相应的 get 语法。**

```shell
	Error parsing breakpoint condition.  Will try again when we hit the breakpoint.Error parsing breakpoint condition.  Will try again when we hit the breakpoint.(gdb) 
```

**可以加上数据类型说明的就加上，比如(int)。**

##在断点条件发生时执行特别的任务
可能你也发现了，在图1的断点条件编辑框里有一个 Actions 功能，对！就是这儿，你可以通过添加一个 Debugger Command 加上你想要的调试语句，比如 p i

##在调试时改变变量的值
可以在 Actions 里添加Debugger Command，比如执行一个 set var m=10。

---

###参考
* [XCODE LLDB TUTORIAL](http://www.cimgf.com/2012/12/13/xcode-lldb-tutorial/)
* [Debugging with GDB - Assignment to variables](http://www.delorie.com/gnu/docs/gdb/gdb_118.html)