---
title: BUg集合
date: 2021-12-30 11:16:37
tags: web
---

这里用来记载一些平时开发会遇到的坑，持续更新

<!-- more -->

# zorro antd 的虚拟滚动列表乱序问题

使用了 nzVirtualScroll 虚拟滚动，发现切换数据展示时数据会乱序
只要在每次赋值之前将列表清空即可

# pre 标签自动换行问题

pre 标签默认文字过长不会换行

```css
white-space: pre-wrap; /* css3.0 */
white-space: -moz-pre-wrap; /* Firefox */
white-space: -pre-wrap; /* Opera 4-6 */
white-space: -o-pre-wrap; /* Opera 7 */
word-wrap: break-word; /* Internet Explorer 5.5+ */
```

加上以上代码即可
