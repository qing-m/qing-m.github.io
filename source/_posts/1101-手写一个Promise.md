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
const p1 = new Promise((resolve,reject) => {
  console.log('1')
  resolve('成功')
  reject('失败')
  throw new Error('发生错误')
})

console.log(2)

p1.then(data => {
  console.log('success', data)
}, err => {
  console.log('failed', err)
})

// 保留resolve 输出结果 1 2 success 成功
// 保留reject 输出结果 1 2 failed 失败
// 保留throw new Error 输出结果 1 2 failed 发生错误
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
    this.status = PENDING
    this.value = undefined
    this.reason = undefined

    let resolve = (value) => {
      if(this.status === PENDING) {
        this.status = RESOLVED
        this.value = value
      }
    };

    let reject = (reason) => {
      if(this.status === PENDING) {
        this.status = REJECTED
        this.reason = reason
      }
    };

    try {
      executor(resolve, reject)
    }catch(error) {
      reject(error)
    };
  }

  then(onFufilled, onRejected) {
    if(this.status === RESOLVED) {
      onFufilled(this.value)
    }

    if(this.status === REJECTED) {
      onRejected(this.reason)
    }
  }
}
{% endcodeblock %}

# 考虑executor执行异步函数的情况
有如下代码：
{% codeblock lang:js %}
const p1 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('成功')
  }, 1000)
})

p1.then(data => {
  console.log('success', data)
}, err => {
  console.log('failed', err)
})

// 无结果输出
{% endcodeblock %}
## 分析
- 当JavaScript主线程执行到**executor**，setTimeout作为宏观任务被暂时储存，按照**主线程 => 微观任务 => 宏观任务**的事件循环顺序执行
- 此时执行到then，由于没有调用resolve，所以status的状态为pending，所以上面的代码什么也不会输出。

## 解决
- 创建两个数组**onResolvedCallbacks**、**onRejectedCallbacks**。**excutor**执行异步代码时，在then里增加判断，如果状态为**pending**，就将**onFufilled**和**onRejected**分别存入**onResolvedCallbacks**和**onRejectedCallbacks**，当异步代码被执行完毕，调用resolve或reject的时候，从数组里面取出，依次执行。

由上所述，添加以下代码
{% codeblock lang:js %}
constructor(executor) {
  ...
  // 成功存放的数组
  this.onResolvedCallbacks = []
  // 失败存放的数组
  this.onRejectedCallbacks = []
  
  let resolve = (value) => {
    if(this.status === PENDING) {
      this.status = RESOLVED
      this.value = value
      // 一旦resolve执行，调用成功数组的函数
      this.onResolvedCallbacks.forEach(fn => fn())
    }
  }

  let resolve = (reason) => {
    if(this.status === PENDING) {
      this.status = REJECTED
      this.reason = reason
      // 一旦reject执行，调用失败数组的函数
      this.onRejectedCallbacks.forEach(fn => fn())
    }
  }

  ...
}

then(onFufilled, onRejected) {
  ...
  if(this.status === PENDING) {
    this.onResolvedCallbacks.push(() => {
      onFufilled(this.value)
    })

    this.onRejectedCallbacks.push(() => {
      onRejected(this.reason)
    })
  }
}
{% endcodeblock %}

# 实现Promise的链式调用

有如下代码：
{% codeblock lang:js %}
// 原生的Promise
const p1 = new Promise((resolve,reject) => {
  setTimeout(() => {
    <!-- resolve('成功') -->
    reject('失败')
    // resolve或reject
  }, 1000);
}).then(data => {
  console.log('success1', data)
  return 123
}, err => {
  console.log('failed1', err)
  return 456
}).then(data => {
  console.log('success2', data)
}, err => {
  console.log('failed2', err)
})
// 输出结果： failed1 失败 success2 456
{% endcodeblock %}

## 分析

- 当**promise**被**reject**后，第一个**then**打印没有问题，但是第二个**then**被**onFufilled**打印了，证明此时是resolved状态，如果链式调用返回**this**的话，**promise**状态应该不会改变，仍然是**rejected**才对，由此可见，链式调用每次都会返回一个**新的promise**

由上稍微改下代码：
{% codeblock lang:js %}
...
then(onFufilled, onRejected) {
  let promise2 = new Promise((resolve, reject) => {
    if(this.status === RESOLVED) {
      let x = onFufilled(this.value)
      resolve(x)
    }

    if(this.status === REJECTED) {
      let x = onRejected(this.reason)
      resolve(x)
    }

    if(this.status === PENDING) { 
      this.onResolvedCallbacks.push(() => { 
        let x = onFufilled(this.value) 
        resolve(x) 
      }) 
      this.onRejectedCallbacks.push(() => { 
        let x = onRejected(this.reason) 
        resolve(x) 
      }) 
    }
  })
  return promise2
}
{% endcodeblock %}

>到这里链式调用就可以跑了，效果和原生Promise一样

# 实现透传
有如下代码：
{% codeblock lang:js %}
const p1 = new Promise((resolve, rject) => {
  setTimeout(() => {
    reslove('成功')
    reject('失败')
  }, 1000)
})

p1.then().then().then().then(data => {
  console.log('success', data)
}, err => {
  console.log('failed', err)
})
// 输出：success 成功
// 输出：failed 失败
{% endcodeblock %}
## 分析
- 当resolve的时候，实际上可以看做
  {% codeblock lang:js %}
    p1.then(v => {
      return v
    }).then(v => {
      return v
    }).then(v => {
      return v
    }).then(data => {
      console.log('success', data) 
    }, err => {
      console.log('failed', err)
    })
  {% endcodeblock %}
  **每次返回当前的data，直到最后被打印**
- 当reject的时候，实际上可以看做
  {% codeblock lang:js %}
    p1.then(()=>{}, err => {
      throw err
    }).then(()=>{}, err => {
      throw err
    }).then(()=>{}, err => {
      throw err
    }).then(data => { 
      console.log('success', data) 
    }, err => { 
      console.log('failed', err) 
    })
  {% endcodeblock %}
  **每次抛出当前错误，直到最后被打印**

因此then方法稍微加点代码：
{% codeblock lang:js %}
then(onFufilled, onRejected) {
  onFufilled = typeof onFufilled === 'function' ? onFufilled : v=>v
  onRejected = typeof onRejected === 'function' ? onRejected : err => { throw err }
  ...
}
{% endcodeblock %}