---
title: Git 不能推送远程分支
discription: git操作提示 dst refspec XXX matches more than one
categories: Git
tags:
    - Git
---

# dst refspec XXX matches more than one

## 场景

项目中有个分支是 `1.1.0` 分支，用来进行升级和版本代码保留，这个分支最近被某个`坑货`给玩坏了，发现我本地的代码是完整保留的，于是要将代码推送到远程。

## 问题

我在执行下面的代码之后

``` bash
git push origin --delete 1.1.0
```

报错提示：

dst refspec XXX matches more than one

意思大概是具体的引用匹配到的不止一个。

查询相关资料发现，不推荐`分支`和 `tag` 名字完全一样。。。

当远程仓库同时存在相同名称的 `branch` 和 `tag` 时，不指明绝对路径的前提下，操作这个名称的 `branch` 和 `tag` 都会出现这个问题。

## 解决

我在项目中去搜索发现同样命名为 `1.1.0`的`branch`和命名为 `1.1.0`的 `tag` ，于是在删除远程分支的时候，搜索到的匹配就不只一个。



## 解决方法

### 1、删除

1. 检出 `tag`,然后进行重命名或者基于它新建 `tag` 。

2. 然后再通过命令去删除远程分支。

3. 推送`tag`到远程

### 2、使用绝对路径

1. 直接指明绝对路径

```bash
$ git push origin :refs/heads/1.1.0
```

2. 如果是推代码带`tag`

```bash
$ git push origin :refs/tags/1.1.0
```

## 结论

我比较推荐使用第二种方法，当然这次的做法可以直接去仓库进行手动分支删除，一切可视化操作，但是使用命令去实现更能了解一些`git`的知识。



