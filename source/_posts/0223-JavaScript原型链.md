---
title: JavaScript原型链
categories:
  - JavaScript
tags:
  - JavaScript原型
date: 2022-02-23 15:03:32
toc: true
---

面不到也要懂

<!-- more -->

# 简介

**JavaScript 万物皆为对象**
为什么称为原型链呢？链字面意思：链是汉语汉字，拼音 liàn，是指用金属的环连接成的长条形的东西。从字面意思来讲就是一个**有头有尾**的，能链接在一起的长条。**但是** js 的原型链**是不连结在一起的**，它就是一个长条。**长条的尾部连结的为 null**

# Object

> 引用类型：Object、Array、Function、Date、RegExp。

1. 引用类型，都具有对象特性，即可自由扩展属性。
2. 引用类型，都有一个隐式原型 `__proto__` 属性，属性值是一个普通的对象。
3. 引用类型，隐式原型 `__proto__` 的属性值指向它的构造函数的显式原型 prototype 属性值。
4. 当你试图得到一个对象的某个属性时，如果这个对象本身没有这个属性，那么它会去它的隐式原型 `__proto__`。也就是它的构造函数的显示原型 prototype 中寻找

我们也可以将`__proto__`当作链接的纽带，就是它将 js 原型组成了一条链。

### 总结

上面我们说到，原型链的尾部为**null**
所以 **Object.prototype** 指向为 null。**加上纽带链接起来**
即：**Object.prototype.`__proto__` = null**

# 类 Foo

```javascript
class Foo {
  constructor(name) {
    this.name = name;
  }
}
// console.log(Foo.prototype.__proto__ === Object.prototype); // true
```

当一个类为头部时，它的原型链为：
**`Foo.prototype.__proto__ => Object.Prototype`**

# 实例

```javascript
class Foo {
  constructor(name) {
    this.name = name;
  }
}

const f1 = new Foo("胖虎");

// console.log(f1.__proto__ === Foo.prototype); // true
```

当一个实例为头部时，它的原型链为：
**`f1.__proto__ => Foo.prototype`**
