---
title: Scintilla文档阅读笔记
date: 2021-02-22 22:51:41
tags:
---

# Scintilla文档阅读

## 一.	设计文档

​	scintilla主要有三部分构成：可移植库，核心代码，平台的events和API

最上层的结构，最开始的目的是用来分离平台的依赖代码。让Scintilla更便捷的移植到一个新的平台。并且让大多数代码阅读者不需要处理平台的细节。为了最小化移植问题和避免代码肿胀，Scintilla使用了很保守的C++子集，里面没有一些扩展和运行时的信息，也没有需要依赖平台的C++标准库。

现有的支持平台包括：windows，GTK/Linux,Cocoa和wxWidgets在很多方面是相似的。每一个都有窗体，菜单和bitmap。这些特性大致工作的方式都是相似的，比如移动一个窗体和画一条红线。有时候平台要求一系列的消息而不是一个简单的消息。有时候，不同点是更深的。例如在windows里面读取剪切板和GTK读取剪切板就有很大的不同。

​	a)	可移植库，

​			可移植库被定义在Platform.h文件，且对于每个平台只执行一次。PlatWin.cxx定义了windows的各种函数的变体，PlatGTK.cxx是GTK的

​			有少数几种类是平台特殊定义的，并且代表了这些平台的objects。大多数的客户端代码可以不关注现有平台来操作平台的类。有时候客户端代码需要通过GetID函数来获取下面被定义的类。下面这些平台特殊的类是被定义成相同的名字，让它们能够被客户端代码方便的迁移。

​	Point PRectangle

​	ColourDesired 

​	Font ————HFONT for Windows  ，PangoFontDescription* for GTK

​	Surface 

​	Window

​	ListBox

​	Menu

​	Platform 这个类是用来获取平台特性的类，例如：双击速度 和chrome color。

​	b)	核心代码

​			这部分Scintilla的代码是平台独立的。由以下几部分组成： CellBuffer, ContractionState, Document, Editor, Indicator, LineMarker, Style, ViewStyle, KeyMap, ScintillaBase, CallTip, and AutoComplete。

​			CellBuffer包含了文本，风格信息，撤销堆栈，行标注，文件结构。

​			一个cell 包含了一个字符比特和关联的风格比特，

​			Document类包含了CellBuffer和处理一些更高级的抽象的东西，例如：word ，DBCS 等，包括文档发生改变的时候通知其他类。

​			Editor是Scintilla的核心类。它需要做到展示文档，对用户的action进行反应，还有对其他容器发出指令。展示文档的类有：ContractionState, Indicator, LineMarker, Style, and ViewStyle。快捷键类是KeyMap。**可能有多个Editor对象依附在一个文档对象上，文档的修改可能通过DocWatcher传到很多个Editor对象上**（There may be multiple Editor objects attached to one Document object. Changes to a document are broadcast to the editors through the DocWatcher mechanism.）

​			ScintillaBase 是Editor的子类，它添加了一些窗口特性。如果不需要这些添加的窗口特性，这个类是可以选择不用的，这是一个轻量级的Scintilla组件。

​	c)	Plaform Events and API

​			每个平台都使用了不同的机制来接收事件信息。在Windows，事件通过消息和COM传递。在GTK，使用的是回调函数。

​			对每一个平台。都会有一个类从ScintillaBase(或者Editor)衍生。ScintillaWiin是Windows的，ScintillaGTK是GTK的。这些类用来连接平台事件，也用来实现一些父类的虚函数（每个平台都是不同的）。例如：这层结构需要处理不同平台的剪切板事件，这在每个平台是不一样的。

## 二.	Scintilla使用要点

使用自动缩进

关键点是 使用 SCN_CHARADDED 添加缩进到每一个新行。

