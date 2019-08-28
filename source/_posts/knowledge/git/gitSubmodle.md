---
title: Git 子模块
categories: Git
tags:
    - Git
---

# Git 子模块

## 介绍

`git子模块`，即 `git submodule`, 是一种在一个 `git` 仓库中嵌套另外一个 `git` 仓库代码的实现方式。

有时候我们需要将一些代码放到一个仓库中，由其他人进行开发和维护，而在另外一个项目中，只需要引入这个仓库中的代码，并不对代码进行修改这种情形。我们就可以使用 `git submodule` 来实现。

形象一点说明：我要搭建一个博客，博客的结构如下

blog
    ├── content
    ├── index.html
    ├── public
    ├── theme
    └── conf文件

里面的 `theme` 文件夹内容是我的博客主题，可以自己手动去写，也可以找好看的主题进行魔改。但是在魔改后我又想提交到自己的仓库进行存储。这种时候可以把 `theme` 文件夹下面的代码放到一个 `git` 仓库中。然后整个博客工程放到一个 `git` 仓库中。 使用 `git submodule` 结合起来。

## 使用

### 添加

子模块的使用很简单，拿到子模块的 `git` 仓库地址。在项目的根文件夹下执行添加 `git submodule` 命令。

```shell
> git submodule add -b develop https://code.xxxxxx.com.cn/lokya-theme.git theme/lokya-theme
```

解释下上面的命令，上面的命令是个组合命令，实现的效果是给当前项目添加子模块，添加的是子模块的 `develop` 分支，将代码放到 `theme` 文件夹下，名称为 `lokya-theme`。

- git submodule add https://code.xxxxxx.com.cn/lokya-theme.git
    添加仓库
- git submodule add -b develop https://code.xxxxxx.com.cn/lokya-theme.git
    添加仓库的 `develop` 代码
- git submodule add https://code.xxxxxx.com.cn/lokya-theme.git theme/lokya-theme
    将代码放到theme文件夹中，命名为 `lokya-theme`

这样就把子模块添加进去了。

### 初始化

子模块的初始化可以使用一下命令，也可直接组合使用

```shell
> git clone <repository> --recursive 递归的方式克隆整个项目
> git submodule init 初始化子模块
> git submodule update 更新子模块
```

组合使用

```shell
> git submodule update --init --recursive
```

### 多个子模块使用

可以使用多个子模块，使用 `git submodule add` 添加多个 git 子模块

### 统一操作子模块

可以使用 `git submodule foreach` 来进行批量操作子模块，比如所有子模块切换分支和拉取代码

```shell
> git submodule foreach git checkout develop
> git submodule foreach git pull
```

## 踩坑记录

### 问题描述

在多个子模块进行 `docker` 打镜像的时候，初始化仓库拉取不到代码。

由于在 `runner` 中设置不了 `git` 仓库的账号密码，拉取不到。

### 问题解决

由于我这边子模块和主工程的仓库在同一个账号下，于是修改 `.gitmodules` 文件

```txt
[submodule "theme/lokya-theme"]
	path = theme/lokya-theme
	url = https://code.xxxxxx.com.cn/lokya-theme.git
	branch = develop
```

修改为

```txt
[submodule "theme/lokya-theme"]
	path = theme/lokya-theme
	url = ../../lokya-theme.git
	branch = develop
```

通过相对路径去获取 `git` 仓库，这样可以默认使用同一套账号密码，子模块也可以进行代码拉取。

## 反思总结

`git submodule` 是一种很好的解决多个库拆分、组合的方式，可以完全避免代码冲突，多人权限维护等。

但是 `git submodule` 的使用时候要注意每次切换分支都会将子模块的提交指向游离态，需要手动去切换分支拉取最新的子模块代码。