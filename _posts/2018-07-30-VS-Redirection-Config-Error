---
layout:     post
title: VS2015/VS2017 提示cannot read configuration file, filename: redirection.config     
subtitle:
date:       2018-07-30
author:     brackenbo
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - VS2015
    - redirection.config
---


## 问题的现象

用VS2015或者VS2017打开代码，总是提示https://stackoverflow.com/questions/12170133/creating-a-virtual-directory-failed-with-the-error

VS2015会在termial提示，VS2017会弹出对话框，非常令人烦躁。

另外就是代码的工程目录结构打不开，右键reload就会报错找不到redirection.config.

## 解决办法

google了很多，最终发现可能是用户权限导致的

* 关闭VS2015
* 到代码目录下，删除*.csproj.user文件
* 重新打开VS2015