---
title: 用纯CSS实现滚动阴影
date: 2023-03-10 10:51:47
tags: css
---

首先，介绍一下 `background-attachment`，如果指定了 `background-image` ，那么 `background-attachment` 决定背景是在视口中固定的还是随着包含它的区块滚动的。

`background-attachment: local`，这个就是和我们日常使用中的用法是一致的，可滚动容器的背景图案随着容器进行滚动
`background-attachment: scroll`，这个是今天的主角，它表明背景相对于元素本身固定， 而不是随着它的内容滚动

滚动阴影的难点在于，初始没有滚动的时候是没有阴影展现的，只有当开始滚动，阴影才会出现。

所以这里，我们借助 `background-attachment: srcoll` 和 `background-attachment: local` 两个属性，在滚动初始的时候，利用两层背景叠加在一起隐藏阴影背景，真正滚动的时候，将叠加的部分移走，只漏出阴影部分即可。

html 代码如下

```
<ul class="g-final">
    <li>AAAAAAAA AAAA</li>
    <li>BBBBBBBB CCCC</li>
    <li>DDDDDDDD DDDD</li>
    <li>AAAAAAAA AAAA</li>
    <li>BBBBBBBB CCCC</li>
    <li>DDDDDDDD DDDD</li>
    <li>AAAAAAAA AAAA</li>
    <li>BBBBBBBB CCCC</li>
    <li>DDDDDDDD DDDD</li>
    <li>AAAAAAAA AAAA</li>
    <li>BBBBBBBB CCCC</li>
    <li>DDDDDDDD DDDD</li>
</ul>
```

css 代码如下

```
.g-final {
    background:
        linear-gradient(#fff, transparent 100%),
        linear-gradient(rgba(0, 0, 0, .5), transparent 100%);
    background-size: 100% 50px, 100% 10px;
    background-repeat: no-repeat;
    background-attachment: local, scroll;
    overflow: scroll;
    height: 200px;
}
```
