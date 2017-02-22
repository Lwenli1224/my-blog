# Redux Get Started

React解决了UI编程中组件化问题，长久以来UI框架中组件与事件及后台数据存在耦合，而React真正使其解耦，组件的渲染只与*state*有关，这样使组件变化变得可预测，但同样也使得我们需要管理更多的*state*。遗憾的是，至于如何管理*state*，React框架并没有提供明确的说明，而Redux的出现就解决了这个问题。

## 简介

## Get Started
 	

## 重要概念

### 三大原则

可以用三个原则来涵盖Redux的编程思想，后续在我们应用Redux过程中会详细讲解：

- 单一数据源
- 状态只读
- 变化来自纯函数：纯函数满足以下三个条件
	- 其结果只能通过参数的值来计算
	- 不依赖于能被外部操作改变的数据
	- 不能改变外部状态

### state，action和reducer

一个很重要的公式：f(state)=UI

在Redux的哲学思想里，UI界面状态是可预测有限个的，所以可以用state为自变量的函数来表示UI，任意一个state都有唯一确定的UI快照与其相对应。

然而UI的状态是多种多样难以维护的，并且界面显示所需的数据结构，和需要与服务器交互的数据结构常常大不相同。对于复杂的业务场景，通过双向数据绑定来维护一个数据模型，在后期会使得情况更加复杂。

Redux将[CQRS](https://martinfowler.com/bliki/CQRS.html)这个领域驱动设计中的概念应用在构造前端数据流上，CQRS将操作数据和查询数据所基于的数据模型重构为查询模型与命令模型。在Redux中具体表现为：通过action与服务器发生交互，将得到的数据通过reducer函数计算出界面所需的state存储在内存中的一课状态树上，界面层直接访问该状态树来进行UI渲染。

那么Redux中的action其实对应了CQRS概念中的命令模型，而通过reducer计算出的状态树则对应着查询模型。这样规划，使得React应用的状态变化可预测及可测试，数据的流向更加清晰。

### 单向数据流

### 事件溯源

## 进阶使用

### 中间件

### 减少代码量

### selector函数

## 测试

## 相关信息

### 配套设施

- [react-redux](https://github.com/reactjs/react-redux)：react与redux绑定使用，官方对react支持组件
- [redux-thunk](https://github.com/gaearon/redux-thunk)：redux中间件，延迟计算表达式，一般用于解决action creator中异步分发action的问题




