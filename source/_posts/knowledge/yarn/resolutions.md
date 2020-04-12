---
title: package.json resolutions 深入浅出
categories: Yarn
tags:
    - Yarn
---

# package.json resolutions 深入浅出

## 基本使用

`yarn install --flat` 命令将会自动在 `package.json` 文件里加入 `resolutions` 字段或者直接在 `package.json` 中增加字段配置

``` json
{
  "resolutions": {
    "package-1": "0.0.1"
  }
}
```
`resolutions` 字段允许再依赖关系中定义自定义软件包版本。

## 使用场景

- 项目中可能依赖于不经常更新的软件包，但这个依赖包依赖于另一个进行了重要升级的软件包。在这种情况下，如果直接依赖项指定的版本范围不包含新的子依赖项版本，只能等中间包的作者进行优化修改。

- 项目的子依赖项获得了重要的安全更新，并不希望等待直接依赖项发布最低版本更新。

- 依赖一个无人维护，但工作包和其中一个依赖项得到升级。

- 依赖性定义了一个广泛的版本范围，并且您的子依赖关系刚刚得到了一个有问题的更新，因此您希望将其固定到较早的版本。

## 官方示例

**package.json**

```
"dependencies": {
    "package-a": "1.0.0",
    "package-b": "1.0.0"
  },
"resolutions": {
"**/package-d1": "2.0.0"
}
```

**依赖图**

```
package-a@1.0.0
 |_ package-d1@1.0.0
     |_ package-d2@1.0.0

package-a@2.0.0
 |_ package-d1@2.0.0
     |_ package-d2@1.0.0

package-b@1.0.0
 |_ package-d1@2.0.0
     |_ package-d2@1.0.0

package-c@1.0.0
 |_ package-a@2.0.0
     |_ package-d1@2.0.0
         |_ package-d2@1.0.0
```

## resolutions 和其他依赖关系

`devDependencies` `optionalDependencies` 和`依赖关系字段`总是优先于 `resolutions` :如果用户定义显式的依赖,这意味着他希望这个版本,即使是指定不明确的规范。

## 项目和项目依赖的包同时具有 resolutions 字段

在我们的项目中，我们同时维护项目和项目中依赖的包，而且在项目和我们所依赖的包中，同时对同一个第三方包进行 `resolutions` 版本规范，会是什么样子呢？

首先，我在子包中，将 `choerodon-ui` 的版本规范为` 0.8.58`

![resolutions](/images/knowledge/resolutions1.png 'resolutions1')

然后它的 `lock` 版本为：

![resolutions](/images/knowledge/resolutions2.png 'resolutions2')

发布 `npm` 包后，将这个 `srm-front-boot` 依赖进主项目

![resolutions](/images/knowledge/resolutions3.png 'resolutions3')

没有加 `resolutions` `之前，lock` 文件的版本是这样的：

![resolutions](/images/knowledge/resolutions4.png 'resolutions4')

然后在 `devDependencies` 中增加 `choerodon-ui` 的依赖，发现会依赖成两个版本

![resolutions](/images/knowledge/resolutions5.png 'resolutions5')
![resolutions](/images/knowledge/resolutions6.png 'resolutions6')

总结： 

由上面的情况我们可以得出，子模块中的 `resolutions` 自会限制子模块中某个依赖的版本，如果在父工程项目中也同时需要对某个包进行版本规范，需要在父工程中也添加 `resolutions` 对版本进行规范，父项目和子依赖包中的 `resolutions` 字段不会进行冲突。