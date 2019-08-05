---
title: ES6-变量的解构
categories: javascript
tags:
    - javascript
    - ES6
---

## (1) 数组
```javascript
let [a, b, c] = [1, 2, 3];
// 报错
let [foo] = 1;
let [foo] = false;
let [foo] = NaN;
let [foo] = undefined;
let [foo] = null;
let [foo] = {};
```

## (2) 对象
```javascript
let { foo, bar } = { foo: "aaa", bar: "bbb" };
```
区分模式还是变量
```javascript
let obj = {
  p: [
    'Hello',
    { y: 'World' }
  ]
};

let { p: [x, { y }] } = obj;
x // "Hello"
y // "World"
```
注意，这时p是模式，不是变量，因此不会被赋值
注意: 默认值生效的条件是，对象的属性值严格等于undefined。

## (3)字符串
看做数组
```javascript
const [a, b, c, d, e] = 'hello';
a // "h"
b // "e"
c // "l"
d // "l"
e // "o"
```

## (4)数值的布尔值
```javascript
let {toString: s} = 123;
s === Number.prototype.toString // true
```
解构赋值的规则是，只要等号右边的值不是对象或数组，就先将其转为对象。由于undefined和null无法转为对象，所以对它们进行解构赋值，都会报错

## (5)例子
```javascript 
let x = 1;
let y = 2;

[x, y] = [y, x];
```