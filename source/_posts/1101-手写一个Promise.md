---
title: 手写一个Promise
categories:
  - 深入理解源码
tags:
  - Promise
  - JavaScript
date: 2021-11-01 16:18:38
toc: true
---
**由浅入深实现符合Promise A+规范的Promise代码**

<!-- more -->

# 实现简单版(非异步)
{% codeblock lang:js %}
const p1 = new Promise((reslove,reject) => {
  console.log('1')
  reslove('成功')
  reject('失败')
  throw new Error('发生错误')
})
p1.then(data => {
  console.log(data)
}, err => {
  console.log(err)
})

// 保留resolve 输出结果 1 2 success 成功
// 保留reject 输出结果 1 2 failed 失败
// 保留throw new Error 输出结果 1 2 failed Error: 抛出错误
{% endcodeblock %}

## 总结
1. 传入Promise构造函数的**executor**是立即执行函数
2. then方法是promise对象上的方法，并接收两个函数:**onFufilled**, **onRejected**
3. onFufilled接收参数**value**, onRejected接收参数**reason**, 并且值是由resolve和reject传递过来的，由此可见，value和reason也是构造函数上的属性
4. 当调用了resolve方法后，promise状态由pending改为resolved且不能再被更改，同理reject
5. 抛出错误后，同样会走reject方法，reason由onRejected方法打印

由上所述，我们可以写出以下代码
{% codeblock lang:javascript %}
const RESOLVED = 'RESOLVED'
const REJECTED = 'REJECTED'
const PENDING = 'PENDING'

class Promise {
  constructor(executor) {
    this.state = PENDING
    this.value = undefined
    this.reason undefined

    let reslove = (value) => {
      if(this.state === PENDING) {
        this.state = RESOLVED
        this.value = value
      }
    };

    let reject = (reason) = > {
      if(this.state === PENDING) {
        this.state = RESOLVED
        this.reason = reason
      }
    };

    try {
      executor(reslove, reject)
    }catch(error) {
      reject(error)
    };
  }

  then(onFufilled, onRejected) {
    if(this.state === RESLOVED) {
      onFufilled(this.value)
    }

    if(this.state === REJECTED) {
      onRejected(this.reason)
    }
  }
}
{% endcodeblock %}

# 考虑executor执行异步函数的情况