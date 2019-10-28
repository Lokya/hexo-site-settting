---
title: node is incompatible with this module
categories: node
tags:
    - node
    - note
---

# "node" is incompatible with this module

## 背景

今天在执行 `yarn` 进行安装的时候，服务器报错 `error file-loader@2.0.0: The engine "node" is incompatible with this module. Expected version ">= 6.0.0`

一开始以为是 `node` 版本问题，但是执行 `node -v` 后发现 `node` 版本是 `v8.12.0`, 版本完全大于 `6.0.0`

查询后需要忽略 `yarn` 的引擎检查

执行

```shell
 > yarn config set ignore-engines true

 #或者

 > yarn --ignore-engines
```

即可执行成功。

## 备注

在这里再添加点 `yarn` 官网的一些操作

`yarn install`用于安装一个项目的所有依赖。 这个命令最常见的使用场景是在你刚`Check out`一份项目代码之后，或者在你需要使用其他开发者新增加的项目依赖的时候。

如果习惯使用 `npm` ， 你可能希望使用` --save` 或 `--save-dev`， 这些已经被 `yarn add` 和 `yarn add --dev` 所取代。

执行不带任何命令的 `yarn`，等同于执行`yarn install`，并透传所有参数。

如果需要可重现的依赖环境（比如在持续集成系统中），应该传入 `--frozen-lockfile` 标志。

### yarn install
在本地 `node_modules` 目录安装 `package.json` 里列出的所有依赖。

### yarn install --check-files
验证 `node_modules` 中已安装的文件没有被移除。

### yarn install --flat
安装所有依赖，但每个依赖只允许有一个版本存在。 第一次运行这个命令时，会提示你在每个依赖包的多个版本范围中选择一个版本。 这会被添加到你的 `package.json` 文件的 `resolutions` 字段。

```json
"resolutions": {
  "package-a": "2.0.0",
  "package-b": "5.0.0",
  "package-c": "1.5.2"
}
```
### yarn install --force
这回重新拉取所有包，即使之前已经安装的。

### yarn install --har
从安装期间的所有网络请求输出一个 `HTTP archive`。 `HAR` 文件通常用于排查网络性能，并能用 `Google’s HAR Analyzer` 或 `HAR Viewer` 这样的工具分析。

### yarn install --ignore-scripts
不执行项目 `package.json` 及其依赖定义的任何脚本。

### yarn install --modules-folder <path>
为 `node_modules` 目录指定另一位置，代替默认的 `./node_modules`。

### yarn install --no-lockfile
不读取或生成 `yarn.lock` 锁文件。

### yarn install --production[=true|false]
如果 `NODE_ENV` 环境变量设为 `production`，`Yarn` 将不安装任何列于 `devDependencies` 的包。 使用此标志指示 `Yarn` 忽略 `NODE_ENV` 并用它取代“生产”与否的状态。

注意： `--production` 等同 `--production=true`。`--prod` 是 `--production` 的别名。

### yarn install --pure-lockfile
不生成 `yarn.lock` 锁文件。

### yarn install --frozen-lockfile
不生成 `yarn.lock` 锁文件，并且，如果需要更新则会报错。

### yarn install --silent
执行 `yarn install` 而不显示安装日志

### yarn install --ignore-engines
忽略引擎检查。

### yarn install --ignore-optional
不安装可选依赖。

### yarn install --offline
运行 `yarn install` 于离线模式。

### yarn install --non-interactive
禁用询问交互，比如当没有合适版本的依赖时

### yarn install --update-checksums
当跟对应包的校验和不一致时， 更新 `yarn.lock` 锁文件的校验和



