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
# 简介
在工作当中，避免不了使用Promise来解决异步回调问题。回调地狱一直深深的在我脑海里。Promise的出现直接解决了这种痛苦。当熟练的使用Promise后，就会想去看看它到底是怎样写出来的，手写一个Promis也是面试中一个常见问题。接下来我们就一起来研究一下Promise是怎样写出来的。
<!-- more -->

# Promise的声明
首先Promis一定是一个类
* 由于**new Promise((resolve,reject)=>{})**, 所以传入一个参数，这个参数是一个函数，这里我们根据A+规范称作它为executor。
* executor里面有两个参数：**resolve**（成功），**reject**（失败）。
* 由于**reslove**和**reject**都是可执行的，所以都是函数，我们用let声明。

{% codeblock lang:Javascript %}
class Promise {
  constructor(executor) {
    let reslove = () => {}; // 成功
    let reject = () => {}; // 失败
    executor(reslove,reject) // 立即执行
  }
}
{% endcodeblock %}

# 解决基本状态
在A+规范中对Promise有规定：
* Promise存在三个状态：**pending**、**fulfilled**、**rejected**
* pending（等待态）为初始态，并可以转化为fulfilled（成功态）和rejected（失败态）

在这样的情况下Promise会由pending转化为fulfilled
{% codeblock lang:javascript %}
let p1 = new Promise(( reslove, reject ) => {
  ...逻辑处理
  reslove(value)
})
{% endcodeblock %}
上面代码使用了Promise，并且reslove出一个值。zai在这里Promise会由pending转化为fulfilled状态，并且不可以再次改变状态

同理：
{% codeblock lang:javascript %}
let p2 = new Promise(( reslove, reject ) => {
  ...逻辑处理
  reject(value)
})
{% endcodeblock %}
p2 reject出一个值。在这里Promise会由pending转化为rejected状态，也不可以再次改变状态

如果new Promise传入的函数executor出现错误时会直接执行reject();
所以我们得出以下代码
{% codeblock lang:javascript %}
class Promise {
  cconstructor(executor) {
    this.state = 'pending' // 初始状态
    this.value = undefined // 成功的值
    this.reason = undefined // 失败的原因
    let reslove = (value) => {
      // state改变,resolve调用就会失败
      if (this.state === 'pending') {
        // resolve调用后，state转化为成功态
        this.state = 'fulfilled';
        // 储存成功的值
        this.value = value;
      }
    };
    let reject = (reason) => {
      // state改变,reject调用就会失败
      if (this.state === 'pending') {
        // reject调用后，state转化为失败态
        this.state = 'rejected';
        // 储存失败的原因
        this.reason = reason;
      }
    };
    try {
      executor(resolve, reject);
    }catch {
      reject(err);
    }
  }
}
{% endcodeblock %}

# then方法