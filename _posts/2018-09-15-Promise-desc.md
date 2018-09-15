---
layout:     post
title:      Promise介绍
subtitle:
date:       2018-09-14
author:     brackenbo
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - Promise
    - 异步
    - javascript
---


# 使用Promise


## 基本用法

> A Promise is an object representing the eventual completion or failure of an asynchronous operation. Promise是表示异步操作最终完成或失败的对象。

Promise 本质上是一个绑定了回调函数的对象。不同于我们平时看到的将回调作为参数传递给一个函数的形式。

首先来看下，我们熟悉的将回调作为函数传递给函数的例子：
    
        function successCallback(result) {
          console.log("Audio file ready at URL: " + result);
        }
        
        function failureCallback(error) {
          console.log("Error generating audio file: " + error);
        }
        
        createAudioFileAsync(audioSettings, successCallback, failureCallback);

上面这个函数作用是异步的创建一个音频文件，第一个参数是配置对象，第二和第三个分别是创建音频文件成功和失败后的回调函数。

那如果将上面的例子用Promise来处理，会是怎样的形式呢？

1. Promise用then来表示处理结果

        createAudioFileAsync(audioSettings).then(successfulCallback, failureCallback)
    
2. 也可以写成另外一种简化形式

        createAudioFileAsync(audioSettings).then(successfulCallback).catch(failureCallback)
    
我个人喜欢第二种形式，可读性更强一些


## Chain 链式调用

这个是Promise一个特别有意思的用法，对于处理多个串行的异步调用非常好用。

考虑这样的场景，我们要处理多个异步调用，每一个异步的调用都要利用前一个的结果。那么就可以使用promise的chain。

例如，我们要获取某些资源，并且对资源处理之后，发送PUT请求更新资源的结果。而第一步是验证用户的合法性，是否具有这样操作的权限。那么用Promise就可以写成如下形式：

    LoginPromise(userProfile)
        .then(resp => {
            if (resp.status == OK) return resp.authToken;
        })
        .then(token => {
           return axios.get(url, {authToken: token}); 
        })
        .then(resp => {
          let resource = prepareResource();
          return axios.post(url, {resource: resource}, header);
        })
        .catch(err => {
          console.log(err);
        })

上述的代码就会比之前嵌套的回调要优雅而且表意很多。

> 当然，catch之后依然还可以跟then，表示出错之后的继续处理。

## 改造旧的异步调用

我们可能会碰到许多已经存在的异步调用，如何使用Promise来改造他们呢，看一下下面这个例子：

    setTimeout(() => saySomething("10 seconds passed"), 10000);
    
`setTimeout` 设置一个定时器，在10000ms之后发出消息。
    
    const wait = ms => new Promise(resolve => setTimeout(resolve, ms));
    
    wait(10000).then(() => saySomething("10 seconds")).catch(failureCallback);
    
可以看到`wait`只是作为一个Promise对象存在，至于回调要做什么，在then中可以来设置。

## 组合

Promise.resolve() 和 Promise.reject() 可以手动创建一个已经resolve或者reject的Promise对象。在某些场景下是一个非常有用的特性。

Promise.all() 和 Promise.race() 用来处理并行的Promise。

例如下面的代码，就使用了Promise和reduce：

    [func1, func2].reduce((p, f) => p.then(f), Promise.resolve());
    
首先创建Promise.resolve()，然后依次处理func1和func2. 上述代码等价于：

    Promise.resolve().then(func1).then(func2);
    
以此类推，就可以将上述代码扩展到多个函数的处理：
    
    const applyAsync = (acc,val) => acc.then(val);
    const composeAsync = (...funcs) => x => funcs.reduce(applyAsync, Promise.resolve(x));
    
composeAsync是一个接收可变参数作为一组回调函数的函数。使用方式如下：

    const transformData = composeAsync(func1, asyncFunc1, asyncFunc2, func2);
    
composeAsync 的返回值是一个函数，此函数输入参数为x，执行Promise.resolve(x).then(func1).then(func2).then(func3)...

    transformData(data)
    
这个函数实现的功能是对data，由一组函数（func1, asyncFunc1, asyncFunc2, func2）来处理。


## Nesting

这里需要注意的是Promise的Nesting，嵌套。如果你想实现的是Promise的串行调用，那么就一定使用Promise的Chain来操作。

如果出现Promise的嵌套，就会出现一些隐藏的问题，看如下的例子：

    promise1()
        .then(res => {
          promise2(res)
            .then(newRes => {
            })
            .catch(err) {
              console.log(err)
            }
        })
        .then(result => {
          promise3()
            .then( () => {})
            .catch(() => {})
        })
        .catch(err => {})

上述代码中，也许你想在promise2出现失败的情况下，后续的异步操作promise3不用再执行，但实际上做不到，内层的catch并不会传递到外层，所以then.promise3就会继续执行下去。

## 容易出的错
    
    // Bad example! Spot 3 mistakes!
    
    doSomething().then(function(result) {
      doSomethingElse(result) // Forgot to return promise from inner chain + unnecessary nesting
      .then(newResult => doThirdThing(newResult));
    }).then(() => doFourthThing());
    // Forgot to terminate chain with a catch!

上面的代码有三处错误：

* 内层并没有return新的promise，这样就会到这外层不会等到内层结束，而直接运行doFourthThing()。
* nesting 在前面已经分析过了
* 没有catch，在众多浏览器中没有catch会导致promise出错

一个比较合理的promise例子如下：
    
    doSomething()
    .then(function(result) {
      return doSomethingElse(result);
    })
    .then(newResult => doThirdThing(newResult))
    .then(() => doFourthThing());
    .catch(error => console.log(error));
    
