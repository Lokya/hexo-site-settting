---
title: 前端分支管理
categories: Git
tags:
    - Git
---

# 前端分支管理

## 背景

最近要对前端分支管管理进一步规范化，于是就是现在公司项目中的分支管理进行总结和扩展，设计分支和管理规范。

由于项目采用 `monorepo` 模式，各个分组代码维护在 `package` 下面各个模块中，要做到各组开发方便，不同组间不干扰，同时也要可以实时测试开发环境，而且发版发布稳定版本，于是就指定如下的分支管理规范。

项目中对权限有所管控，由于部分人员的`git`水准实在不敢恭维，于是只有小组leader可以进行合并、升级等操作。

## 前言

软件的版本控制以及分支管理贯穿于整个软件产品的生命周期，日常的项目管理对于开发团队能否有节奏且顺利的交付软件也很重要。

版本控制有很多种，例如我们常见的`Git`、`SVN`、`VSS`等，本项目中主要使用`Git` 进行版本控制。

`Git`分支管理模型有三个，即`GitFlow`、`GitLabFlow`、`GitHubFlow`。下面将介绍这三种分支模型的原理，使用场景和优缺点等

**GitFlow**

---

`GitFlow` 是最早诞生并得到广泛应用的一种工作流程。

该模型中存在两种长期分支：`master` 和 `develop`。 `master`中存放对外发布的版本，只有稳定的发布版本才会合并到`master`中。 `develop`用于日常开发，存放最新的开发版本。

也存在三种临时分支：`feature`, `hotfix`, `release`。

- `feature`分支是为了开发某个特定功能，从`develop`分支中切出，开发完成后合并到`develop`分支中。
- `hotfix`分支是修复发布后发现的Bug的分支，从`master`分支中切出，修补完成后再合并到`master`和`develop`分支。
- `release`分支指发布稳定版本前使用的预发布分支，从`develop`分支中切出，预发布完成后，合并到`develop`和`master`分支中。

优点：

- `feature` 分支使开发代码隔离，可以独立的完成开发、构建、测试
- `feature` 分支开发周期长于`release`时，可避免未完成的`feature`进入生产环境

缺点：

- 无法支持持续发布。
- 过于复杂的分支管理，加重了开发者的负担，使开发者不能专注开发。

**GitHubFlow**

---

`GitHubFlow`分支模型只存在一个`master`主分支，日常开发都合并至`master`，永远保持其为最新的代码且随时可发布的。

- 在需要添加或修改代码时， 基于`master`创建分支，提交修改。
- 创建`Pull Request`，所有人讨论和审查你的代码。
- 然后部署到生产环境中进行验证。
- 待验证通过后合并到`master`分支中。

这个分支模型的优势在于简洁易理解，将`master`作为核心的分支，代码更新持续集成至`master`上。根据目前收集到的反应来看，得到了更多的好评，认为`GitHubFlow`分支模型更加轻便快捷。

**GitLabFlow**

---

`GitLabFlow` 是`GitFlow`和`GitHubFlow`的结合,它吸取了两者的优点，既有适应不同开发环境的弹性，又有单一主分支的简单和便利。

该模型采取上游优先的原则，即只存在一个`master`主分支，它是所以分支的上游。只有上游分支采纳的变动才能应用到其他分支。

- 对于持续发布的项目，建议在`master`之外再建立对应的环境分支，如预生产环境`pre-production`，生产环境`production`。
- 对于版本发布的项目，建议基于`master`创建稳定版本对应的分支，如`stable-1`，`stable-2`。

## 分支命名

| 前缀          | 含义                                     |
| :------------ | :--------------------------------------- |
| master        | 主分支，可用的、稳定的、可直接发布的版本 |
| develop       | 开发主分支，最新的代码分支               |
| publish   | 稳定发布分支 |
| 模块名称-dev  | 模块开发主分支                           |
| 模块名称-工号 | 模块下个人开发分支                       |
| release-**    | 预发布分支                               |
| hotfix-**     | 已发布bug修复分支                        |
| 1.x.x | 例如1.1.x分支用于保留1.1.x版本中所有模块代码 |

## 分支管理

- 主分支 ` develop`,  `master`, `publish`全部被保护起来，禁止直接修改主分支以及把本地分支 push 到主分支。

- 主分支 ` develop`,只能在 Gitlab 上通过 MergeRequests发起合并请求，并且只能由各组前端负责人在 Gitlab 上做合并。

- 主分支`master`,只能每天由负责人进行合并`develop` 到 `master`。
-  `1.1.x` 分支可以合并到除了`master`、`publish` 外的其他分支，一般通过这个分支进行升级相关包和文件。
- `xxxx-dev` 分支可以发起合并请求到 `develop`.  禁止其他分支(例如: `feature-xxx`) 合并到 `develop` 分支。
- 个人的开发分支只能合并到各组的 `xxxx-dev` 分支。
- 大版本整体升级后，会保留前一版本的分支，如果有bug可以基于其建立` 1.x.x-hotfix`分支对上面的bug进行修复。例如从`1.1.10`升级到`1.2.0`后，在`1.1.x`分支会保留`1.1`版本中所有代码，如果有bug基于`1.1.x`建立`1.1.x-hotfix`分支对bug进行修复。

分支管理图如下

![分支管理图](/images/knowledge/gitManage.png '分支管理图')

## 提交规范

### 提交步骤

1. `git add` 后执行 `yarn commit`。
2. 要选择提交的类型。（可选类型如下）。
3. 要填写提交的模块，例如`srm-front-spfm`。
4. 要填写提交说明。例如：修复xxx导致的bug。

### 提交类型

| 类型     | 说明                                                         |
| -------- | ------------------------------------------------------------ |
| build    | 主要目的是修改项目构建系统(例如 glup，webpack，rollup 的配置等)的提交 |
| ci       | 主要目的是修改项目继续集成流程(例如 Travis，Jenkins，GitLab CI，Circle等)的提交 |
| docs     | 文档更新                                                     |
| feat     | 新增功能                                                     |
| fix      | bug 修复                                                     |
| perf     | 性能优化                                                     |
| refactor | 重构代码(既没有新增功能，也没有修复 bug)                     |
| style    | 不影响程序逻辑的代码修改(修改空白字符，补全缺失的分号等)     |
| test     | 新增测试用例或是更新现有测试                                 |
| revert   | 回滚某个更早之前的提交                                       |
| chore    | 不属于以上类型的其他类型                                     |









