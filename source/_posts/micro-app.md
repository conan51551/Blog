---
title: 微前端嵌入项目的流程
date: 2024-11-15 15:36:32
tags:
---

### 代账项目引入 microapp 流程

<!-- more -->

在原 iframe 的地方使用 microapp

```
import microApp from '@micro-zoe/micro-app'
```

```
<micro-app
    // @ts-ignore
    name={'my-app'}
    r-show={xcUrlNew}
    url={xcUrlNew}
    style={{ height: '100%' }} ></micro-app>
```

然后页面就会出现`<micro-app>`标签,挂在的就是原`iframe`的页面

### 注意事项

`microapp` 的 url 不能是跨域的，如果不在一个域名需要解决跨域资源请求的问题

### 解决方案

页面打开后，大概率是白屏报错的状态,下面就是需要解决的问题

#### 1. `window.YZF.GlobalData` 是空的

原因是在`common.min.js`脚本中，有一段代码
<img src="/images/clip-img2.png" width="200" style="margin:0"/>
将`window.YZF.GlobalData` 设置为空了，然后在另一个地方
<img src="/images/clip-img1.png" width="800" style="margin:0"/>
将`window.YZF.GlobalData` 设置为 `window.parent.YZF.GlobalData || {}`

现在因为已经没有`window.parent`了，所以`window.YZF.GlobalData` 就为空了

现在`common.min.js`脚本的平级加了`common.microapp.min.js`，将`html`文件中的`common.min.js`替换为`common.microapp.min.js`就可以

#### 2. 页面样式错乱

造成这个问题的原因可能有如下四个：

- `$('body')`或者`$('document')`找不到，所以需要修改`$('body')`或`$('document')`为`$('micro-app-body')`

- 所有`absolute`的元素可能定位不对，需要修改为`fixed`
- 页面中引用的`fintax-web`中的组件使用了`tailwind-css`，在`microapp`中无法生效，需要依据情况手动修改
- 页面元素的主题色不生效，需要自己在`css`文件中设置（因为微前端为了样式隔离，会在`css`外侧包一层`micro-app-body`）

#### 3. 页面中各种脚本找不到

下面将一些需要特殊处理的脚本和公共的资源放入 cos 中以便使用

```
// 日历组件
<script
  type="text/javascript"
  src="https://fintax-web-1257122416.cos.ap-shanghai.myqcloud.com/static-resource/subaccount-20241108/js/%08salary_new/mCalendar.js"
></script>

// 打印设置
<script
  src="https://fintax-web-1257122416.cos.ap-shanghai.myqcloud.com/static-resource/subaccount-20241108/js/%08salary_new/common_dayin_modal.67.min.js"
  type="text/javascript"
></script>

// 导入
<script
  type="text/javascript"
  src="https://fintax-web-1257122416.cos.ap-shanghai.myqcloud.com/static-resource/subaccount-20241108/js/%08salary_new/ajaxfileupload.js"
></script>

// layer-ui
<script type="text/javascript" src="https://fintax-web-1257122416.cos.ap-shanghai.myqcloud.com/static-resource/subaccount-20241108/js/%08salary_new/layer/layer.js"></script>
<link rel="stylesheet" type="text/css" href="https://fintax-web-1257122416.cos.ap-shanghai.myqcloud.com/static-resource/subaccount-20241108/js/%08salary_new/layer/skin/yzf/layer.css" />

<!-- 为了解决micro-app拿不到window.Api的问题，所以自己写一份old-api，将Api对象挂到window上 -->
<script src="https://fintax-web-1257122416.cos.ap-shanghai.myqcloud.com/static-resource/subaccount-20241108/js/%08salary_new/old-api.min.js"></script>
```

#### 4.在原生页面中通过`plugin`使用了`react`组件，但是组件不生效的问题

需要在`page-plugins.js`文件中，调用新的`createOnePage`方法匹配对应的插件。
同时在新拷贝出来的插件中，需要将`window`或者一些全局变量按照显示的调用重新编写一份

#### 5.页面中引入的脚本找到了，但是没生效

判断是引用时机的问题，将脚本位置前移可以解决

#### 6.微前端的`window`对象被框架改造成了代理对象，需要使用的话用`Object.assign`的方式结构

#### 7.页面中原本`inline`的元素变成`block`导致页面样式出问题

`jquery`的 show 方法会缓存之前的的 display，所以一开始就设置为`display:none`就会导致这个问题，不在 css 中设置，而放在 js 中用 hide 代替

### 总结

微前端改造主要的问题是原来是用`iframe`引入的页面，相当于是一个浏览器去单独请求的页面，所以很多执行的机制是完整，通过项目的形式去走的流程，改成微前端后，页面是在一个沙箱中，数据和样式都是隔离的，很多子项目的流程不走了，所以造成了很多问题，所以需要手动去解决这些问题。

#### ps： 微前端的路由也有相应的改动，目前并没有遇到相应的需求，如果有需要，可以参考`microapp`的官方文档
