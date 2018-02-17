---
layout:     post
title:      github -- delete branch
subtitle:   How to delete git branch
date:       2018-01-03
author:     brackenbo
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - github
    - branch
---


简要的描述

## 删除本地分支

    git branch -d feature/login
or
    git branch -D feature/login

## 删除远程分支

删除远程分支不用git branch，而是要使用git push

    git push origin --delete feature/login

记住本地分支和远端分支之间没有关系，需要各自删除。



