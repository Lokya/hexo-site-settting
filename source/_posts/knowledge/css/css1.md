---
title: filter - 网页灰色效果
categories: CSS
tags:
    - CSS3
---

# 网页灰色效果

百度百科在有人去世会把搜索信息页面灰化，今天访问b站的时候发现在公祭日这一天b站也全站灰化，各网站都以一种互联网的风格来哀悼死者。

哀悼的同时给大家顺便讲下全网站灰色化的实现。

## filter(滤镜)

### 用法：

filter: none | blur() | brightness() | contrast() | drop-shadow() | grayscale() | hue-rotate() | invert() | opacity() | saturate() | sepia() | url();

### 说明：
- none: 默认值。
- blur(): 给图像设置高斯模糊。
- brightness(): 给图片应用一种线性乘法，使其看起来更亮或更暗。
- contrast(): 调整图像的对比度。
- drop-shadow(): 给图像设置一个阴影效果
- grayscale(): 将图像转换为灰度图像。
- hue-rotate(): 给图像应用色相旋转。
- invert(): 反转输入图像。
- opacity(): 转化图像的透明程度。
- saturate(): 转换图像饱和度。
- sepia(): 将图像转换为深褐色。
- url(): URL函数接受一个XML文件，该文件设置了一个SVG滤镜，且可以包含一个锚点来指定一个具体的滤镜元素。

### 兼容性


![](https://user-gold-cdn.xitu.io/2019/12/13/16efde4e93ba4c52?w=2024&h=648&f=png&s=128012)

## 网站灰色实现


### 使用 `grayscale` 来实现效果。
可以设置 grayscale(100%); 来图像转换为灰度图像。

``` css
html {
    filter: grayscale(100%);
    -webkit-filter: grayscale(100%);
    -moz-filter: grayscale(100%);
    -ms-filter: grayscale(100%);
    -o-filter: grayscale(100%);
}
```

### 展示效果对比

#### 正常页面

![](https://user-gold-cdn.xitu.io/2019/12/13/16efde6d85eec500?w=3358&h=1864&f=png&s=178294)

#### 灰色化后页面


![](https://user-gold-cdn.xitu.io/2019/12/13/16efde724f1efe64?w=3358&h=1862&f=png&s=167963)

### 图片
也可以单独给图片设置灰色效果

``` css
img {
    filter: grayscale(100%);
    -webkit-filter: grayscale(100%);
    -moz-filter: grayscale(100%);
    -ms-filter: grayscale(100%);
    -o-filter: grayscale(100%);
}
```

# 最后

其他效果大家也可以尝试下，会有意想不到的效果。。。。