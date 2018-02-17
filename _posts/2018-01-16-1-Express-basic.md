---
layout:     post
title:      Express介绍
subtitle:   介绍一下Express
date:       2018-01-16
author:     brackenbo
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - js
    - express
---


> Fast, unopinionated, minimalist web framework for Node.js

## Hello world

先从一个最简单的例子入手：
    
    const express = require('express')
    const app = express()
    
    app.get('/', (req, res) => res.send('Hello World!'))
    
    app.listen(3000, () => console.log('Example app listening on port 3000!'))
    
这是一个最简单直观的使用Express的例子，如果想在本地运行，只需要如下的步骤：

* npm init
* 创建app.js，并将刚才的代码粘贴进去
* 运行 `node app.js`


## 基本的路由设置

Express提供了一个固定的路由访问的格式：

    app.METHOD(PATH, HANDLER)
    
* app是Express的一个实例
* METHOD即HTTP中的方法，如GET, POST, DELETE...
* PATH是可访问的server上的路径
* HANDLER即回调函数

重复一个简单的栗子：
    
    app.get('/', function (req, res) {
      res.send('Hello World!')
    })
    
## 静态文件的访问

Express 提供了静态文件的访问，方式如下：

    express.static(root, [options])
    
* root指明了要使用哪个文件夹下的静态文件

基本的提供静态文件的方式如下：

    app.use('/static', express.static('public'))
    
这样就可以通过static路径来访问public下的文件：

    http://localhost:3000/static/images/kitten.jpg
    http://localhost:3000/static/css/style.css
    http://localhost:3000/static/js/app.js
    http://localhost:3000/static/images/bg.png
    http://localhost:3000/static/hello.html
    

    
    
