---
layout: post
title: "解决Xcode升级后插件失效的问题"
date: 2015-03-15 17:24:59 +0800
comments: true
categories: 
---

##问题
 今天升级了xcode到6.2，给代码加注释的时候习惯性的输入了///，发现并没有自动生成注释格式。又看了一下菜单栏，所有之前安装的插件都消失了。怎么把他们找回来呢？查了下，找到以下解决方案。
 
## 解决方案
1.	前往到插件安装目录：~/Library/Application Support/Developer/Shared/Xcode/Plug-ins；
2.	右键单击需要修改的插件==》显示包内容==》双击Contents==》打开Info.plist文件；
3.	找到 DVTPlugInCompatibilityUUIDs，这是个数组，包含多条记录，在其中添加一条`A16FF353-8441-459E-A50C-B071F53F51B7`。
4.	重启xcode，修改过的插件就回来了。

## 原理
`A16FF353-8441-459E-A50C-B071F53F51B7`是什么鬼？`DVTPlugInCompatibilityUUIDs`又是什么东东？查了一下，原来为了解决插件兼容性的问题，`DVTPlugInCompatibilityUUIDs`会保存支持的Xcode版本对应的uuid，而`A16FF353-8441-459E-A50C-B071F53F51B7`就是Xcode6.2对应的uuid。

那么怎么查到当前版本的uuid呢？去这个文件下找就可以了`/Applications/Xcode.app/Contents/PlugIns/DVTiOSPlistStructDefs.dvtplugin/Contents/Info.plist`。当然，有时候Xcode的安装路径会有所改变，下面的目录结构也会有所改变，总之打开Xcode的app包，搜索一下这个关键字就可以了。

