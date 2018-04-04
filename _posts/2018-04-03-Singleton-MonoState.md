---
layout:     post
title:      Singleton 和 MonoState模式
subtitle:
date:       2018-04-03
author:     brackenbo
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - 敏捷
    - Singleton
    - MonoState
    - 单例模式
---

# Singleton 和 MonoState原则

### 应用于只需要一个实例的场景，Singleton关注于结构上的单个实例，MonoState关注于行为上的单个实例。

> 这里的所有观点摘抄自《敏捷软件开发原则、模式与实践》，原著Robert C. Martin，邓辉等译。


## Singleton模式

实现上其实很简单，也有多种方式，最简单的一种如下：

    public class Singleton
    {
        private static Singleton instance = null;

        private Singleton() {}

        public static Singleton Instance()
        {
            if (instance == null)
            {
                instance = new Singletone();
            }

            return instance;
        }
    }

这样使用的时候，只需要获取这样一个实例就可以了。

优点：

* 跨平台： 需要使用合适的中间件
* 适用于任何类
* 可以派生
* 延迟求值

付出的代价：

* 不能回收/销毁这个实例
* 不能继承
* 效率问题
* 不透明性

## MonoState模式

实现如下：

    public class MonoState
    {
        private static int index = 0;

        public MonoState() {}

        public setIndex(int x)
        {
            index = x;
        }

        public int getIndex()
        {
            return index;
        }

    }

可以看出MonoState并不限定实例的个数，但是这些实例都共用同一个静态变量index.这也是MonoState和Singleton的最大区别。

优点：

* 透明
* 可派生
* 多态

代价：

* 不可转换
* 效率
* 内存占用多
* 不能跨平台

> Singleton所谓的结构上的单一很容易理解，就是只有一个实例。MonoState重点关注行为，指的是可以有多个实例，这些实例共用同一个静态变量。对于这个
静态变量可以有多重不同的行为。
