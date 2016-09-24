# koa源码分析

## 目录结构

koa的源码非常简单，只有四个文件。模块从`lib/application.js`启动。

- application.js
- context.js
- request.js
- response.js

## 重点模块分析

### application.js

#### 构造函数

1. 声明类Application，在类的构造函数中判断this指向，可以使用调用函数和`new`关键字两种方式来实例化Application。
2. 在构造函数中初始化`middleware context request response`这几个重要对象。
3. 继承Emitter

#### listen

1. 将`this.callback`传入`http.createServer`node原生方法
2. 调用原生listen方法

#### use

1. 判断是否存在`this.experimental`，禁止es7 async方法
2. 将函数push进`this.middleware`数组中

#### callback

最重要的核心方法

1. 处理es7 async方法，提示deprecated，使用koa v2
2. 判断是否存在`this.experimental`，存在使用compose_es7处理middelware数组，否则使用`co.wrap(compose(this.middleware))`。
3. 得到了一个可以返回promise对象的函数fn
4. 返回一个handleRequest的函数，该函数内部以context为函数上下文来调用fn

##### compose方法

这是一个独立的模块，用于组织koa middleware，核心方法只有一个compose函数，**它的目的在于，返回一个Generator函数，这个函数包含了所有middleware的遍历器，只需要不断执行`遍历器对象.next()`，就可以顺序得到middleware的遍历器对象。**

1. 接受一个middleware数组
2. 返回一个接受next参数的generator函数
3. 如果next不存在，赋值一个空generator函数，用于首次调用。
4. 从后往前，遍历middleware数组
5. 执行数组元素，传入next，得到新next，新next其实是Generator函数执行后得到的遍历器对象。
6. 从数组尾部遍历middleware数组，每次执行数组元素得到的遍历器对象都会保存在前一个数组元素执行后的遍历器对象中。
7. 最后一个遍历元素，也就是middleware数组的第一个元素，是所有遍历器对象引用的源头。
8. 返回`yield *next`，由于compose函数是返回一个Generator函数，所以需要使用*号

##### co.wrap方法

执行一个Generator函数，返回一个可以返回promise对象的普通函数。

#### createContext





