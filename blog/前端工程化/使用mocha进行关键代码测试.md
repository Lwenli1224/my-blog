# 使用Mocha进行关键代码测试

人不可能一直保持不犯错误，优秀的程序员也一样，所以单元测试还是很必要的。关于单元测试，我的观点是在通用框架组件和涉及交易方面一定要全部覆盖，普通业务方面有时间就写，完成需求优先，后期再补。

## 概念介绍

### Mocha

[Mocha](http://mochajs.org/)是一个具有众多特性的Javascript测试框架，可以运行在Node.js和浏览器环境中。使用Mocha可以快速和方便的创建异步测试程序。

### 单元测试

用于判断某个特定条件下，某个特定单元（函数/模块/类）的行为。

### 断言

即为布尔表达式，用于在代码中捕捉一些假设，并且一般会在捕捉到相应条件后，中断程序的继续执行。此外，必须在单元测试中使用断言。

## Getting Started

### 安装Mocha

#### 全局安装

```shell
npm install -g mocha
```

#### 开发依赖安装

```shell
npm install --save-dev mocha
```

### 安装其他依赖

如果进行单元测试，断言是必不可少的，推荐使用[should.js](https://www.npmjs.com/package/should)作为单元测试的断言模块

#### should.js

```shell
npm install --save-dev should
```


### 代码示例

简单的Mocha测试代码解读见注释

```javascript
// 引入断言模块
var assert = require('assert');

// describe，声明一个block，一些钩子函数的作用范围均在describe定义的函数中
describe('Array', function() {
  describe('#indexOf()', function() {
  //  一个测试用例，用于判断“4”是否存在于“[1,2,3]”这个数组中
    it('should return -1 when the value is not present', function() {
      assert.equal(-1, [1,2,3].indexOf(4));
    });
  });
});
```

### 运行测试

假定将上节的代码保存在'/project/test/testArray.js'中。

如果为全局安装，`cd`到项目根目录下：

```shell
mocha
```
默认情况下，mocha会在运行命令的目录下寻找`./test/*.js`或`./test/*.coffee`进行执行，如果存在多级目录，请使用`--recursive`参数：

```shell
mocha --recursive ./test/ 
```

如果为依赖安装，则需要借助[npm script](https://docs.npmjs.com/misc/scripts)来执行mocha命令，在项目中`package.json`中加入以下片段

```json
"scripts": {   "test": "mocha" }
```
由于`test`是npm-script预先定义好的模块，所以执行以下命令即可执行mocha测试脚本：

```shell
npm test
```

### ES2015

Mocha提供了对Babel的良好支持，如果测试脚本的后缀仅为`.js`，通过`--require`参数即可运行符合ES2015的代码。当然，需要执行`npm install --save-dev babel-register`安装babel支持。

```shell
mocha --require babel-register --recursive ./test
```

### 使用其它编译器

现在比较注重前沿技术的前端团队，开始使用ES2015或ts等语法进行开发了。通过Mocha所提供的指定compiler功能，我们可以使用第三方的编译器来运行测试脚本。






