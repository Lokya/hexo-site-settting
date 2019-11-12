---
title: Xcode 报错解决记录
categories: react-native
tags:
    - react-native
---

# Xcode: library not found for -l xxxx 报错问题定位

最近在进行 react native 的学习和使用，在使用 Xcode 进行打包的时候，经常会看见 library not found for -lxxx 的这种报错。结合网络上给的资料和自己的一些分析，做一个记录吧。

## 问题分析

这个问题其实就是第三方库没有加载到导致的保存。

## 问题处理

### pod 安装包

IOS 的环境有一部分人是使用 cocoapods 来进行包管理的，所以可以怀疑是第三方库没有安装。

在 ios 文件夹下执行以下语句

``` bash
pod deintegrate 

pod instll
```
一般会解决大部分问题

### 添加搜索路径

Xcode > Target > Build Settings > 搜索 Library Search Path

在里面添加第三方库的路径

### 文件目录变动

有可能项目中的一些静态文件位置移动，毕竟项目是由多人开发的

Xcode > Target > Build Phases > Link binary With Libraries

然后查看引入的文件，是否有红色提示，如果是红色的话说明这个文件位置进行的变动，现在找不到。

可以在选中红色文件右键 Reveal in Project Navigator 然后定位一下，或者手动去寻找文件位置。重新引入即可。

### 项目部分文件被篡改

这种问题就比较恶心了，有可能其他人在操作项目的时候执行了一些命令，撤销的时候有文件残留导致的或者引起了一些路径变化不可逆。

如果可以使用 git 的话，查看文件变动，还原后再调试启动，定位解决。

推荐以上3中方法不可以解决问题的话，重新创建项目，然后一步一步迁移原工程代码中进行进行测试，当然这个是最傻最笨的办法。

## 其他

Xcode 的坑有时候还是蛮多的，后续还会再开一个文档写Xcode的一些常见问题的解决。。。。
