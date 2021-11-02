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