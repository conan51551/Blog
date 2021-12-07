---
title: SourceMap
date: 2021-12-06 14:06:42
tags: web
---

**`source map`是为了解决即使打包过后的代码，也可以找到具体的报错位置，使我们的`debug`代码变得更轻松**

<!-- more -->

# Webpack中的配置

``` javascript
const path = require('path');

module.exports = {   
    entry: './src/index.js',  
    output: {      
        filename: 'bundle.js',      
        path: path.resolve(__dirname, 'dist')  
    },  
    devtool: "source-map"
};
```
其中devtool有20多种不同取值，分别生成不同类型`Source Map`,主要有以下几种

* `source-map`：外部。可以查看错误代码准确信息和源代码的错误位置。
* `inline-source-map`：内联。只生成一个内联 `Source Map`，可以查看错误代码准确信息和源代码的错误位置
* `hidden-source-map`：外部。可以查看错误代码准确信息，但不能追踪源代码错误，只能提示到构建后代码的错误位置。
* `eval-source-map`：内联。每一个文件都生成对应的 `Source Map`，都在 `eval` 中，可以查看错误代码准确信息 和 源代码的错误位置。
* `nosources-source-map`：外部。可以查看错误代码错误原因，但不能查看错误代码准确信息，并且没有任何源代码信息。
* `cheap-source-map`：外部。可以查看错误代码准确信息和源代码的错误位置，只能把错误精确到整行，忽略列。
* `cheap-module-source-map`：外部。可以错误代码准确信息和源代码的错误位置，`module` 会加入 `loader` 的 `Source Map`。

内联和外部的区别：
1. 外部生成了文件（.map），内联没有。
2. 内联构建速度更快。

## 总结
测试环境用`source-map` => `cheap-module-souce-map`
正式环境用`nosources-source-map`=>`hidden-source-map`


# .map文件解构
下面是一个`bundle.js`文件生成的`.map`文件

原始文件

``` javascript
var a = 1;
console.log(a);
```

打包后的文件 

``` javascript
/******/
(() => { // webpackBootstrap   
    var __webpack_exports__ = {};  
    /*!**********************!*\    
      !*** ./src/index.js ***!     
      \**********************/   
    var a = 1;   
    console.log(a);   
    /******/
})();
//# sourceMappingURL=bundle.js.map
```


``` javascript
{  
    "version": 3,  // sourceMap版本
    "sources": [  // 转换前的文件   
        "webpack://learn-source-map/./src/index.js"   
    ],  
    "names": [],  // 转换钱所有变量名  
    "mappings": "AAAA;AACA,c",   // 记录位置信息的字符串
    "file": "bundle.js",  // 转换后的文件名
    "sourcesContent": [ // 转换前文件原始内容    
        "var a = 1;\r\nconsole.log(a);"  
    ],   
    "sourceRoot": "" // 转换钱文件的所在目录
}
```

其中`mappings`要单独拿出来理解一下

* `；`表示不同行，每个分号都表示转换后源码的一行 （以此为例就是分了两行）
* `，`表示对应转换后源码的一个位置(以此为例就是AAACA,c表示`console`和`log(a)`)
* 每个字母表示不同的位置转换(每个字母表示不同意思，最多由5个字母组成）
    1. 第一位，表示这个位置在（转换后的代码的）的第几列。
    2. 第二位，表示这个位置属于 sources 属性中的哪一个文件。
    3. 第三位，表示这个位置属于转换前代码的第几行。
    4. 第四位，表示这个位置属于转换前代码的第几列。
    5. 第五位，表示这个位置属于 names 属性中的哪一个变量。

    





