---
title: git submodule foreach 忽略子模块
discription: git submodule foreach 忽略子模块
categories: Git
tags:
    - Git
---


# git submodule foreach 忽略子模块

## 背景

我们公司的项目是采用 `lerna` 进行管理的，主模块下面的每个模块都是由不同的开发团队进行开发的，存储在不同的 `git` 仓库，在每次大版本进行发布的时候，需要批量对所有模块进行操作，例如历史分支代码 `tag `保留，删除迭代分支，创建新的分支等。
但是项目在不同的迭代周期中会有新的模块临时添加进来，新的模块并没有创建和其他分支一样的迭代周期分支，在发版期间批量处理时，在每个模块下执行一些 `git` 命令会报错，导致执行中断。

## git submodule foreach

### git submodule

`git` 子模块的用法，我就不多废话了，好多人都用过了，如果没有用过和了解过的朋友，可以参考官网文档： [git 子模块](https://git-scm.com/book/zh/v2/Git-%E5%B7%A5%E5%85%B7-%E5%AD%90%E6%A8%A1%E5%9D%97)

### git submodule foreach

简单来说 `git submodule foreach` 它能在每一个子模块中运行任意命令

#### 基本使用

比如，我们希望所有的子模块都切换到 `develop` 分支

```bash
git submodule foreach git checkout develop
```
再比如 ，我们希望拉取所有子模块的代码

```bash
git submodule foreach git pull
```

#### 不报错退出的执行

但是 `foreach` 是遍历所有的子模块，我们要在所有的子模块下进行执行，势必会有场景中提到的报错情况，于是我去查看了官方文档: [git-submodule](https://git-scm.com/docs/git-submodule)


![](https://user-gold-cdn.xitu.io/2020/5/31/172689a578f67a41?w=1366&h=830&f=png&s=315486)

官方给了一种解决方案，可以解决 `git` 在子模块中执行报错后直接退出，而是继续执行。

```bash
git submodule foreach '其他命令 || :'
```

**任何子模块中命令的非零返回都会导致处理终止**

其实我们还可以这样：

```bash
git submodule foreach '其他命令 || true'
```

#### 选择跳过

现在我们已经实现了执行命令后不报错退出，但是还没解决可以自主选择某几个子模块不执行/执行我们的命令，想想这样一个场景，我们需要在某个命名为 `release-bak` 的分支上面打个 `tag` 对分支进行保存，但是现在某个子模块的分支上没有 `release-bak` 这个分支，在批量执行 `git tag xxx` 的时候，由于不报错，所有的模块都会执行，但是没有 `release-bak` 分支的这个模块打的 `tag` 不是我们预期的，这种情况如何处理呢？

其实官网也给了我们解决方式，让我们看下 `git foreach` 的说明


![](https://user-gold-cdn.xitu.io/2020/5/31/17268a7ff6320932?w=668&h=62&f=png&s=13621)

我们再回忆下解释：

**git submodule foreach 它能在每一个子模块中运行任意命令**

那么是否我们可以在 `git submodule foreach` 后面执行任何 `bash` 命令呢？

于是我在 `git submodule foreach` 后面写了一个 `cash 语句`来进行执行:

``` bash
 git submodule foreach 'case $name in a-Module|b-Module|c-Module) ;; *) git status ;; esac'
```

这个命令是去批量执行，如果遇到 `a-Module`，`b-Module`，`c-Module` 中的任何一个，什么都不操作，其他的子模块中，执行 `git status`。
测试后，的确可以跳过 `a-Module|b-Module|c-Module` 这三个模块进行处理。

那么了解这个特性后，我们就彻底解放天性，可以随心的去通过条件控制我们 执行或者不执行 的各种情况。

## 其他

其实 `git` 拥有很多特性，好多我们都没学习过、使用过，还是需要多积累和学习，在这里祝各位 **0 error, 0 bug**


![](https://user-gold-cdn.xitu.io/2020/5/31/17268b159f13f49e?w=540&h=400&f=png&s=118912)




