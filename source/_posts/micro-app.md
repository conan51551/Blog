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
    name={'salary-microApp'}
    r-show={xcUrlNew}
    url={xcUrlNew}
    style={{ height: '100%' }} ></micro-app>
```

将需要开启的薪酬类型加到数组中
```
// 1： 正常工资
export const supportMicroAppIds = [1]
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

#### 3. 页面中的 layer ui 修改

`layer ui`引用变更

```
    <script type="text/javascript" src="<%=PUBLIC_URL%>/static/public/layer/microApp/layer/layer.js"></script>
    <!-- 下面两个css前后顺序有关系 -->
    <link rel="stylesheet" type="text/css" href="<%=PUBLIC_URL%>/static/public/layer/microApp/layer/skin/layer.css" />
    <link
      rel="stylesheet"
      type="text/css"
      href="<%=PUBLIC_URL%>/static/public/layer/microApp/layer/skin/yzf/layer.css"
    />

```

#### 4.在原生页面中通过`plugin`使用了`react`组件，但是组件不生效的问题

需要在`page-plugins.js`文件中，调用新的`createOnePage`方法匹配对应的插件。
同时在新拷贝出来的插件中，需要将`window`或者一些全局变量按照显示的调用重新编写一份

### 5.接口方面报错或者报接口找不到

需要引入修改过的`old-api.js`

```
<script src="<%=PUBLIC_URL%>/static/public/yzf/js/old-api.js"></script>
```

这是后有可能会不生效，是因为`utils.microApp.min.js`在`old-api`之前引入的，所以需要将`old-api`放在`utils.microApp.min.js`之前引入

```
<script type="text/javascript" src="<%=PUBLIC_URL%>/static/public/yzf/js/utils.microApp.min.js"></script>
```

### 6.使用了微前端会导致 url 中引入之前 iframe 的参数

为了是`url`干净，现将`url`的参数放到`sesstionStorage`中，在页面中获取
首先改写`fintax-web`中`salary`目录下的的`getIframeUrl`方法，将参数放到`sessionStorage`中

```
// 判断是否是支持微应用
  if (supportMicroAppIds.includes(kXclxid) && model_lastest[kXclxid].iframeUrl) {
    const _key = model_lastest[kXclxid].iframeUrl.split('?')[0]
    sessionStorage.setItem(
      _key,
      JSON.stringify({
        xctjid,
        xclxid: kXclxid,
        xcssq,
        xcssqq,
        xcssqz,
        pzzt,
        qyid: qyId,
        kjnd,
        kjqj,
        ztdm,
      })
    )
  }
```
然后在`sub-accounts`中，在对应的页面js改写`getUrlParam`方法
```
;(function ($, $w) {
  //name
  $w.Utils = $w.Utils || {}

  //url
  $w.Utils.url = $w.Utils.url || {}
  $.extend(true, $w.Utils.url, {
    getUrlParam: function () {
      const param = sessionStorage.getItem('/salary_lastest.html')
      return param ? JSON.parse(param) : {}
    },
  })
})(jQuery, window)
```

这样就要注意，文件对应的js就要在`utils.microApp.min.js`之后引入

### 总结

微前端改造主要的问题是原来是用`iframe`引入的页面，相当于是一个浏览器去单独请求的页面，所以很多执行的机制是完整，通过项目的形式去走的流程，改成微前端后，页面是在一个沙箱中，数据和样式都是隔离的，很多子项目的流程不走了，所以造成了很多问题，所以需要手动去解决这些问题。

`micro-app`没有使用主流的`single-spa`，而是用了`webComponent`的思想,不需要子应用修改渲染逻辑并且暴露出来(*`single-spa`的核心就是定义了一套协议，主要就包括主应用的配置信息和子应用的生命周期函数。*)



<img src="/images/性能对比.png" width="800" style="margin:0"/>
大致对比一下，在3秒内打开的正常工资，微前端会快200ms左右


#### ps： 微前端的路由也有相应的改动，目前并没有遇到相应的需求，如果有需要，可以参考`microapp`的官方文档
