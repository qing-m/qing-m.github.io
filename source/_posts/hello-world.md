---
title: Hello World
date: 2021-10-27 14:44:32
toc: true
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

## Quick Start

### Create a new post

{% codeblock lang:JavaScript %}
[rectangle setX: 10 y: 10 width: 20 height: 20];
const a = 1
const b = 2
console.log(a + b)
{% endcodeblock %}

{% codeblock lang:HTML Array.map %}
<label class="inputContainer">
  <i class="icon icon-admin-youxiang beforeIcon"></i>
  <input
    class="inputItem"
    v-model="email" 
    placeholder="请输入邮箱"
    clearable
    maxlength='20'
    @focus="inputFocus"
    @blur="inputBlur"
  ></input>
</label>
{% endcodeblock %}

``` [language] [title] [url] [link text] [additional options]
code snippet
```

``` JavaScript 你好
const Koa = require('koa')
const WebSocket = require('koa-websocket')
const InitManager = require('./core/init')
const catchError = require('./middleware/exception.js')
const parser = require('koa-bodyparser')
const cors = require('koa2-cors')

const html = `
  <label class="inputContainer">
    <i class="icon icon-admin-youxiang beforeIcon"></i>
    <input
      class="inputItem"
      v-model="email" 
      placeholder="请输入邮箱"
      clearable
      maxlength='20'
      @focus="inputFocus"
      @blur="inputBlur"
    ></input>
  </label>
`
```

More info: [Writing](https://hexo.io/docs/writing.html)

### Run server

``` bash
$ hexo server
```

More info: [Server](https://hexo.io/docs/server.html)

### Generate static files

``` bash
$ hexo generate
```

More info: [Generating](https://hexo.io/docs/generating.html)

### Deploy to remote sites

``` bash
$ hexo deploy
```

More info: [Deployment](https://hexo.io/docs/one-command-deployment.html)
