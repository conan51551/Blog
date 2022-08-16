<!--
 * @Author: Cross
 * @Date: 2022-08-16 15:36:59
 * @LastEditTime: 2022-08-16 15:48:08
 * @FilePath: /Blog/source/_posts/Chromecookie鉴权.md
 * @Description: 
-->
---
title: Chromecookie鉴权
date: 2022-08-16 15:36:59
tags:
---

请听题！把一个项目放入另一个系统里分几步？
答：分3步。

1、 把项目打包
2、 把项目对公地址填入要引入的系统
3、 构建系统

在之前我也是这么认为的，然而在实际操作的时候遇到问题了，因为这个项目是通过`url`中的`query`来获取鉴权信息，然后存入`cookie`中的，这个项目和系统又是不同的域名，然而在`Chrome`某个版本开始就不支持跨域存储`cookie`，所以就把cookie放到接口的`reponse`中去`set-cookie`。

一切看上去就是这么美好，然后真的运行起来后，还是没成功。接下来就开始了踩坑的旅途。

初次尝试，将`set-cookie`设置为`read-only:true`，失败
再次尝试，设置`same-site`，失败
最后发现必须是在`https`下才能生效

以此记录这趟坑爹之旅


