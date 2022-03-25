---
title: 一些React使用的例子
date: 2022-03-25 17:50:59
tags:
---

React一些功能的使用方式
<!-- more -->
# Ref
ref有三种使用方式

``` javascript
this.objRef = React.createRef() // create ref使用
componentDidMount() {
    // console.log(`span1: ${this.refs.ref1.textContent}`)
    // console.log(`span2: ${this.ref2.textContent}`)
    // console.log(`span3: ${this.ref3.current.textContent}`)
    setTimeout(() => {
      this.refs.stringRef.textContent = 'string ref got'
      this.methodRef.textContent = 'method ref got'
      this.objRef.current.textContent = 'obj ref got'
    }, 1000)
  }

  render() {
    return (
      <>
        <p ref="stringRef">span1</p> // sting ref使用
        <p ref={ele => (this.methodRef = ele)}>span3</p>// function ref使用
        <p ref={this.objRef}>span3</p>
      </>
    )
  }
```

## forward ref
当ref的是一个函数组件
``` javascript
const TargetComponent = React.forwardRef((props, ref) => (
  <input type="text" ref={ref} />
))

export default class Comp extends React.Component {
  constructor() {
    super()
    this.ref = React.createRef()
  }

  componentDidMount() {
    this.ref.current.value = 'ref get input'
  }

  render() {
    return <TargetComponent ref={this.ref} />
  }
}
```
