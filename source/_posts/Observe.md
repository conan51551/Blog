---
title: 观察者与发布订阅
date: 2021-12-09 11:38:33
tags: web
---

用每个互联网工作者都能看懂的栗子说明发布订阅与观察者区别
<!-- more -->

# 观察者模式

<!-- more -->
## 实现

``` javascript
class Observe {
  static __message = {};

  static on(type, fn) {
    if (typeof this.__message[type] === "undefined") {
      this.__message[type] = [fn];
    } else {
      this.__message[type].push(fn);
    }
  }
  static subscribe(type, args) {
    if (!this.__message[type]) return;
    for (let i = 0; i < this.__message[type].length; i++) {
      this.__message[type][i].call(this, args);
    }
  }
  static off(type, fn) {
    if (this.__message[type] instanceof Array) {
      let i = this.__message[type].length - 1;
      for (; i >= 0; i--) {
        this.__message[type][i] === fn && this.__message[type].splice(i, 1);
      }
    }
  }
}
```

## 调用

``` javascript
  Observe.on("say", function(data) {
    console.log(data);
  });
  Observe.subscribe("say", "123");
  Observe.subscribe("say", "321");

  Observe.on("hello", hello);
  function hello(res) {
    console.log(res);
  }
  Observe.subscribe("hello", "hello");
  Observe.subscribe("hello", "world");
  Observe.off("hello", hello);
  Observe.subscribe("hello", "world");
```

# 发布订阅模式

## 实现

``` javascript
// 调度中心
let eventEmitter = {};

// 缓存列表，存放 event 及 fn
eventEmitter.list = {};

// 订阅
eventEmitter.on = function (event, fn) {
    let _this = this;
    // 如果对象中没有对应的 event 值，也就是说明没有订阅过，就给 event 创建个缓存列表
    // 如有对象中有相应的 event 值，把 fn 添加到对应 event 的缓存列表里
    (_this.list[event] || (_this.list[event] = [])).push(fn);
    return _this;
};

// 发布
eventEmitter.emit = function () {
    let _this = this;
    // 第一个参数是对应的 event 值，直接用数组的 shift 方法取出
    let event = [].shift.call(arguments),
        fns = [..._this.list[event]];
    // 如果缓存列表里没有 fn 就返回 false
    if (!fns || fns.length === 0) {
        return false;
    }
    // 遍历 event 值对应的缓存列表，依次执行 fn
    fns.forEach(fn => {
        fn.apply(_this, arguments);
    });
    return _this;
};

```

## 调用

``` javascript
function user1 (content) {
    console.log('用户1订阅了:', content);
};

function user2 (content) {
    console.log('用户2订阅了:', content);
};

// 订阅
eventEmitter.on('article', user1);
eventEmitter.on('article', user2);

// 发布
eventEmitter.emit('article', 'Javascript 发布-订阅模式');
```

# 区别
网上关于两者区别说的已经很多了，结合我自己的思考，举个栗子总结一下。平时开发中与测试打交道是不可避免的，下面就两种场景分析一下

## 观察者
开发者直接对接测试，测试有发现问题直接通知给开发者。

* 优点：测试发现一个bug就通知一遍开发，响应及时
* 缺点：开发与测试耦合在一起，如果开发请假，测试就不好通知，因此开发与测试需同时引入

## 发布订阅
有一套BUG单系统，测试发现问题提BUG单，开发者去系统中订阅派发给自己的BUG单。

* 优点：不管是开发请假还是测试请假，系统都在，双方都可以在BUG单系统上执行自己的操作。
* 缺点：同时要维护一套BUG单系统，增加了成本





