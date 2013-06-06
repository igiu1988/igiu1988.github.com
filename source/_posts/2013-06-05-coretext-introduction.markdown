---
layout: post
title: "CoreText（1）-介绍"
date: 2013-06-05 16:05
description: "Core Text Programming Guide - Introduce"
keywords: CoreText, 介绍, introduce, 入门 
comments: true
categories: 
---

##介绍

Core Text是一门为了实现文字排版和控制字体的高级底层技术。它以高性能及易用性为基本标准进行设计。Core Text API适用于OS X v10.5以后版本，在所有的的这些OS X系统应用环境中都可以使用。在iOS3.2及以后版本也同样适用。

<!--more-->

Core Text 布局引擎（layout engine）是专门设计用来使文字布局操作更简单和避免副作用而设计。<font color=green>既然Core Text能够实现文字排版和控制字体，那么它内部就会有布局引擎（布局引擎就是用来实现排版）和字体编程接口（字体编程接口就是用来控制字体的）</font>。Core Text字体编程接口（font programming interface）与Core Text布局引擎是互补的，它们是被设计用于能够原生地处理Unicode编码字体，统一不同的OS X字体设备成一个独立且全面的编程接口。

该文档适用于那些想在底层进行文字布局操作和字体处理的开发者。如果你想使用高封装接口（using higher-level construct）来开发应用，比如NSTextView（<font color=green>这是一个OS X的控件，并不适用于iOS，iOS里有UITextView，不过UITextView照NSTextView差得太多了，只能说UITextView是简单封装，不过在iOS6之后的UITextView就好多了，支持NSAttributedString了</font>），那么你应该使用Cocoa text system，具体介绍可以查看Text System Overview。如果并不想使用高封装接口，你想自己在Core Graphics context上渲染文字，那么你就要使用Core Text。

##文档组织

该文档以如下章节组织

“Core Text 概述”：该章在设计目的和功能点方面介绍了Core Text system。同样也介绍了系统中中些不透明类型（opaque types），这些类型封装了文字布局和字体处理功能。
“常用操作（Common Operation）”: 提供了一些代码段和注释，用来示例主要的Core Text不透明类型的主要用法。

##参见

除了这篇文档，还有一些其它的文档覆盖了关于Core Text的更多方面或者描述了Core Text所使用的软件服务。

* Core Text Reference Collection 为Core Text布局和字体API提供了完整的参考信息.
* CoreTextTest 示例项目，它展示了在完整的应用（a complete Carbon application）的内部中如何使用Core Text。
* CoreTextArcCocoa 示例项目，它示范了在Core Text Cocoa 应用中如何使用fonts, lines, runs。
* Core Foundation Design Concepts 和 Core Foundation Framework Reference 描述了Core Foundation，Core Foundation是一个提供了通用数据类型抽象和Core Text所用到的基本软件服务的框架

有关Cocoa Text system

* Text System Overview介绍了Cocoa text system.
* Text Layout Programming Guide for Cocoa讲解了Cocoa text布局引擎
