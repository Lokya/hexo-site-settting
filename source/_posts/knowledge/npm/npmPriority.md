---
title: npm config 优先级
categories: npm
tags:
    - NPM
---

# NPM config 优先级

`npm` 在读取配置的时候会按照优先级进行读取，然后以高优先级的配置进行处理.

## 优先级

`npm` 的优先级如下，优先级从高到低

1、命令行

```bash
npm config set registry http://registry.npmjs.org 
```

2、环境变量

以 `npm_config_` 为前缀的环境变量会被识别为 `npm` 的配置属性。

npm_config_proxy=http://xxxx.com

3、工程的 `.npmrc` 文件

存在于开发工程根目录下的 `.npmrc` 配置文件

4、用户 `.npmrc` 文件

存在于用户根目录下的 `.npmrc` 文件。

5、全局 `.npmrc` 文件

存在于 `node` 全局的 `.npmrc` 文件。

6、`npm` 内置的 `.npmrc` 文件

存在于 `npm` 包的内置 `.npmrc` 文件`/path/to/npm/.npmrc`。

7、`npm` 的默认配置

`npm` `本身有默认配置。对于以上情况下都没有设置的配置，npm` 会使用默认配置
