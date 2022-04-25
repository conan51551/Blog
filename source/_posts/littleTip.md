---
title: 前端小技巧
date: 2022-04-25 17:20:36
tags: js
---

记录一些看到的小技巧(虽然可能没用~)，

<!-- more -->

## indexOf 按位操作简化

避免写很多的>-1

```javascript
//longhand
if (arr.indexOf(item) > -1) {
  // item found
}
if (arr.indexOf(item) === -1) {
  // item not found
}
//shorthand
if (~arr.indexOf(item)) {
  // item found
}
if (!~arr.indexOf(item)) {
  // item not found
}
```

## 简短函数调用

有时候会根据一个函数的返回值判断执行某个函数（比如每次提交表单的时候，会根据`validateData`函数判断是否通过）

```javascript
// Longhand
function test1() {
  console.log("test1");
}
function test2() {
  console.log("test2");
}
var test3 = 1;
if (test3 == 1) {
  test1();
} else {
  test2();
}
// Shorthand
(test3 === 1 ? test1 : test2)();
```

## 双按位操作取整数

```javascript
// Longhand
Math.floor(1.9); // 1
// Shorthand
~~1.9; // 1
```
