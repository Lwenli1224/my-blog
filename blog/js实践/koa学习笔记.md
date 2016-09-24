# koa学习笔记

koa.js是下一代高性能基于node的web框架，与express类似，但是细节上又有很大区别，引入了Generator的概念。想真正发挥koa，就必须对Generator这个概念有深刻了解。

## 基本概念

学习koa需要学习ES6的相关知识，有两个概念很重要，**Generator函数**和**协程**

### Generator函数

介绍Generator函数前，需要了解另外一个概念，Iterator。

### Iterator

首先要清楚的是，Iterator是一个指针对象，指向数据结构的起始位置。

Iterator包含一个next方法，每次调用next方法时，Iterator的内部指针会移至下一个成员。他的返回值包含*value*和*done*。*value*为成员对象，*done*标识是否遍历结束。

### Generator

Generator是函数，一个返回Iterator的函数。

所以，我们可以使用*next*方法来遍历Generator


### 协程

## 基本使用

## 关于web框架

一个好用的web框架笔者认为应该包含以下模块

- 中间件
- 模块化
- 路由
- 日志
- request/response封装
- 配置管理（框架/业务）
- autoload
- session
- 与模板引擎无关的view层
- 与数据库无关的model层

## 中间件

与express不同，koa的中间件必须是Generator函数，支持*yield*的语法。

koa仍然保留了express中间件中的*next*函数，但调用方式和原理都大不相同。在express中，传递给app.use的函数中，会注入一个next函数，在


## 相关中间件

### co

koa有许多以*co-\**命名的中间件模块，他们基本上都是co模块与相关业务模块的结合体，那么为了更好地使用这些模块，那么了解co模块的使用及其原理就尤为重要。

由于需要Generator函数自动执行，那么需要推迟callback的调用，从而有了*Trunk*的概念。*co*解决了前者的问题，并且也可使*Promise*对象自动执行。

co模块目的是自动执行Generator函数。

### koa-static


### co-body



### koa-route





