---
layout: post
title: "Xcode文件管理"
categories: coding ios
type: origin
tags: ios
---

Xcode文件组织有两种形式，一种是folder（蓝色文件夹），一种是group（黄色文件夹）
folder对应实际的文件目录
group与实际的目录没有联系，只是一种逻辑组织

移动group中得文件不会影响到实际的文件路径，这样为代码的管理带来很多的不便。在工程中整整齐齐的代码，在目录中却是杂乱的。
可以用[synx](https://github.com/venmo/synx)这个工具来重新组织代码。

移动文件可以分为以下几个步骤：([stackoverflow](http://stackoverflow.com/questions/4414181/moving-files-into-a-real-folder-in-xcode)有讨论这个问题)

1. 建立group对应到本地目录
1. 在Finder中将文件从源目录移动到目标目录，这时Xcode中得文件将会显示为红色（链接丢失）
1. 将红色的链接移动到新的目录
1. 从File Inspector中选择新的文件路径

