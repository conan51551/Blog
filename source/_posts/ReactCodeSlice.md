---
title: ReactCodeSlice
date: 2022-03-24 20:56:20
tags:
---

一些读 react 源码的时候发现的一些实用的代码片段

<!-- more -->


读取函数中除了前两个参数的剩余参数

```javascript
function createElement(type, config, children) {
  ...
  const childrenLength = arguments.length - 2;
  if (childrenLength === 1) {
    props.children = children;
  } else if (childrenLength > 1) {
    const childArray = Array(childrenLength);
    for (let i = 0; i < childrenLength; i++) {
      childArray[i] = arguments[i + 2];
    }
    props.children = childArray;
    ...
  }
}
```
