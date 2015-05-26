---
layout: post
title: "Xcode文件管理"
categories: coding ios
type: origin
tags: ios
---

Folder References vs Groups
-------------------------
Xcode有两种文件组织形式，一种是folder references（蓝色文件夹），一种是groups（黄色文件夹）

folder references 就是目录链接，它会将Xcode的目录和本地的目录链接起来，两个目录的结构都是一样的，修改也是同步的，即修改Xcode中的目录会影响到本地目录，反之亦同。

groups是Xcode的一个虚拟的目录，并不代表本地的目录，group的文件组织和本地目录的组织也可以是不同的。移动、删除group中的文件并不会影响到本地的目录，然而移动或删除本地目录中的文件可能会导致group中的文件断链（红色）。

优点和缺点
---------

Group 文件管理
--------------
移动文件可以分为以下几个步骤：([stackoverflow](http://stackoverflow.com/questions/4414181/moving-files-into-a-real-folder-in-xcode)有讨论这个问题)

1. 建立group对应到本地目录
1. 在Finder中将文件从源目录移动到目标目录，这时Xcode中得文件将会显示为红色（链接丢失）
1. 将红色的链接移动到新的目录
1. 从File Inspector中选择新的文件路径

工具
----
可以用[synx](https://github.com/venmo/synx)这个工具来重新组织代码。

