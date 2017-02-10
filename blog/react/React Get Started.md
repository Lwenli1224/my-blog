# React Get Started

关于React的种种优势就不多提了，在笔者看来，React的优势在于提供了一套较为完整的前端绘图方案，以及基于状态的组件化编程思想。那么，就开始React之旅吧。需要注意的是，本文面向对象是已经有诸如angularjs等MVVM框架使用经验的前端开发者，并且对webpack的使用有基本了解。

## 开发准备

### React框架依赖

- react：React核心库
- react-dom：React支持包，用于渲染DOM节点，旨在达到React同构的目的，可进行服务器端渲染。


### 编译依赖

- webpack：用于预编译js及jsx文件
- babel-core：babel核心库
- babel-loader：webpack loader，用于编译ES2015语法的javascript
- babel-preset-react：babel编译预设，用于react
- es2015
- babel-preset-stage-0
- babel-preset-stage-1
- babel-plugin-transform-es2015-arrow-functions

### 其它插件

- webpack-dev-server：用于快速构建webpack文件，模拟真实环境
- style-loader
- css-loader

### 安装依赖


```bash
# 安装React依赖
npm install --save react react-dom

# 安装编译依赖
npm install --save-dev webpack babel-core babel-loader babel-preset-react

# 安装webpack loader
npm install --save-dev style-loader css-loader less-loader
```

## Hello World

### without JSX

如果不使用jsx语言以及ES6的特性，那么安装React框架依赖和webpack后就能开始使用了。

首先，需要建立*webpack.config.js*文件，方便webpack打包设置，代码如下：


```javascript
module.exports = {
	  // 入口文件
    entry: "./modules/entry.js",
    output: {
        path: "./build",
        filename: "bundle.js",
        // 发布地址
        publicPath: "/build"
    }
}
```

编写入口文件*entry.js*，代码如下：

```javascript
var React = require('react'),
    ReactDom = require('react-dom');

// 创建组件
var HelloWorld = React.createClass({
    displayName: 'HelloWorld',
    render: function () {
        return React.createElement('div', {className: 'hello-world'}, 'Hello World!');
    }
});

ReactDom.render(
    React.createElement(HelloWorld, null),
    document.getElementById('content')
);
```

html文件如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title></title>
</head>
<body>
<div id="content"></div>
<script src="/build/bundle.js"></script>
</body>
</html>
```

使用webpack-dev-server运行代码，在项目根目录下执行命令`webpack-dev-server --watch`，打开http:localhost:8080/相对根目录下的html文件路径即可。

### with JSX

我们可以使用JSX以及ES6的语法大大加速我们的代码编写过程，需要参考之前的编译依赖小章节

首先，在*package.json*文件中增加依赖：babel-preset-react babel-loader，执行`npm install`安装依赖。

在项目根目录下增加*.babelrc*文件，增加配置如下，让babel采用react的预设：

```json
{
  "presets": ["react"]
}
```

更新*webpack.config.js*文件，增加babel-loader，代码如下：

```javascript
module.exports = {
    entry: "./modules/entry.js",
    output: {
        path: "./build",
        filename: "bundle.js",
        publicPath: "/build"
    },
    module: {
        loaders: [
            {test: /\.(js|jsx)$/, loader: "babel"}
        ]
    }
};
```

这样我们就可以在项目中直接使用JSX语法进行书写了，采用JSX的*entry.js*文件如下：

```javascript
var React = require('react'),
    ReactDom = require('react-dom');

var HelloWorld = React.createClass({
    render: function () {
        return (
            <div className="hello-world">Hello World!</div>
        )
    }
});

ReactDom.render(
    <HelloWorld />,
    document.getElementById('content')
);
```
因为之前执行webpack-dev-server命令时包含了watch参数，webpack会监视你的相关文件变化，并重新编译，所以当编译完成后，刷新之前打开的网页即可看到效果。

## React API

### 核心静态属性

- React.createClass()：通过给定对象，创建React组件类。需要注意的是，通过该方法创建的组建类与常见的原型类不同，所以无需对它们进行实例化操作。
- React.createElement()：通过给定的类型创建React元素，类型参数可以为html标签或React组件类。
- ReactDom.render()：将React元素插入DOM树中的指定节点进行渲染，如果该元素已经被渲染过，会根据需要进行更新或者忽略该次操作请求。

### 实例中的重要属性

在通过给定对象创建React组件类时，我们在保证this对象为当前组件类的情况下，可以通过this访问一些框架为我们生成的属性和方法，列举如下：

- this.state：当前组件的状态集合，要注意==不要==直接操作该属性来改变组件状态。
- this.props：当前组件的属性集合
- this.setState(state, callback)：设置当前组件的状态，第二个参数可传入回调函数，该函数会在setState方法执行完毕后执行。也可直接传入一个函数，函数的返回值为需要设置的state。

### 组件对象中的回调方法

之前说到创建React组件类时，需要传入组件对象，这个对象包含预设回调函数和自定义函数，并且必须包含`render`方法。其中重要的预设函数及属性列举如下：

- getInitialState：在组件初次渲染时调用，获得初始状态
- render：组件对象的必需方法，该方法会检查组件的`props`和`state`的属性，返回一个仅有一个根节点的React元素，用于渲染组件。需要注意的是，要保持该方法的“纯净“，它不能改变组件的状态，并且返回结果与调用次数无关。
- statics：定义React组件类的静态方法，可直接通过组件类调用
- displayName：组件名称，用于调试信息，如果使用JSX语法，则会自动设置为组件名称


## 组件

组件就是有限状态机。在React的编程思想里，UI的变化归根结底是由于组件自身的状态变化，并且React会通过一些算法让我们的DOM更新效率更高。

总的来说，React的组件渲染有以下三个重要的环节：

- 初次渲染
- 更新
- 销毁

在下一小节我们会通过这三个环节来学习React组件的生命周期。


### 组件生命周期

#### 初次渲染

- componentWillMount：在组件即将被渲染之前
- componentDidMount：组件完成渲染，可在该方法内发送AJAX请求或调用其它框架协同工作

#### 更新

- componentWillReceiveProps：当组件接受到一个新属性时调用该方法
- shouldComponentUpdate：当组件接受到新状态和属性时调用，如果返回false组件将停止更新，减少无谓的更新操作
- componentWillUpdate：组件即将更新时调用，可以在该方法中进行对更新组件的优化
- componentDidUpdate：组件更新后调用，可以在该方法中操作更新组件后的DOM

#### 销毁

- componentWillUnmount：在组件即将被销毁时触发，在该方法中需要对之前在`componentDidMount`方法中创建的计时器和DOM进行清理。

## React编程模式

### 分解页面结构

如果我们想把一个页面分解为组件，那一定有多种规划方法，以哪种原则进行划分最合理呢？官方推荐我们遵循设计模式中的单一职责，每个组件只做一件事。同时，我们也要考虑组件的复杂性，如果组件过于复杂，那应该将其分解为子组件。

### 编写静态版本

这是编写React前端代码最简单的一个环节，只需要使用JSX语法构建展示模板，并且不涉及任何交互。需要注意的是，我们不要在这个阶段使用state属性去构建静态版本，因为在React的设计思想中，state仅用于存储交互相关的信息。在没有经过详细思考过关于state粒度的问题时，不要轻易使用它。

我们的模拟数据可以存放在一个变量里，当然也可以使用mock.js这样的框架生成，在JSX模板里我们均使用props进行来绑定数据，之后再逐步为应用添加state。

### 确定State

当底层数据发生变化时，界面程序需要产生相应的变化，React使用`state`来简化这一过程。

正确的做法是，使state变量最小化。

数据的类型是多种多样的，在确定哪种数据作为state时，针对每种类型的数据检验以下三个问题：

1. 这种数据是父组件通过`props`传入的吗？如果是，它可能不是state
1. 这种数据随着时间变化而变化吗？如果不是，它可能不是state
1. 计算该数据时，是否依赖其它state或props？如果是，它可能不是state

总之，与行为（包括用户操作、实时改变数据）相关并且不依赖其它数据的数据类型最有可能是state。

### 定位State

当确定state后，下一步则需要确定由哪个组件操作或拥有该state，这才是编写高效的React应用最重要的一环。

同样，官方也给出了一些关于state的建议：

- 分辨出所有基于该state渲染的组件
- 找出父组件，用于设置state（共同组件：）
- 共同组件，


## React性能优化

### 概念介绍

- Immutable object：函数式编程中概念，暂且叫不可变对象。React elements就是一种不可变对象，一但它被创建，就不可更改它的子节点和属性，React.js官网原文如下，类比非常优美：

> React elements are immutable. Once you create an element, you can't change its children or attributes. An element is like a single frame in a movie: it represents the UI at a certain point in time.
> With our knowledge so far, the only way to update the UI is to create a new element, and pass it to ReactDOM.render().

- Not Mutating Data：

- PureComponent：React提供的用于“浅比较”的抽象类，对组件的*state*和*props*进行*shallow comparison*，判断是否需要调用*render()*函数

- Reconciliation：

### 核心优化

#### DOM Diff

如果想从一棵树转换成另一棵新树，现在计算机领域中已经有很多成熟的解决方案可以去生成最小的操作步骤。但是他们算法复杂度太高了，需要O(n3)。如将其用于React，重新渲染1000个元素节点，需要1000的3次方次比较，这实在是太昂贵了。

所以React自己实现了一套启发式算法，复杂度只有O(n)，这套算法基于两种假设：

- 两个不同类型的节点，会生成两棵不同树
- 开发者可以通过节点属性*key*来明确告诉React需要重新渲染

核心算法如下：

#### DOM元素不同类型

当根节点为不同DOM类型时，React会销毁旧DOM树并重新建立新树，并且需要重新渲染的组件会调用*componentWillUnmount()*生命周期函数。

#### 同类型DOM元素

如果两个元素为相同DOM类型，React会比较其属性，保持其真正的DOM结构不变，只更新变化的属性。

#### 同类型组件元素

当一个组件元素更新时，组件实例保持不变，React会更新组件的内部属性，并且调用*componentWillReceiveProps()*和*componentWillUpdate()*方法。

最后，调用*render()*方法，执行DOM Diff算法。

#### 递归子节点

React会遍历一个线性表每个虚拟节点并且生成一个新突变，与是否线性表是否有变化无关。

但对于线性表增加数据对象这一操作上有两种不同的处理方式：

1. 数据对象追加在线性表尾部，React保持原始DOM树不变，在列表DOM尾部直接插入新子DOM节点。
2. 数据对象追加在线性表头部，React需要重建列表DOM树，会造成性能问题

#### Keys

为了解决递归子节点的问题，React引入了属性*key*。通过将线性表中的每个元素增加唯一*key*值来“告诉”React如何处理列表DOM。这个*key*值通常来源于后台数据的id或者也可以是该数据对象在该线性表中的索引。

#### 权衡

每个动作（来自于用户或网络）按照理论来说都会使React渲染整个App，但通常渲染结果都是相同的。所以我们需要在日常开发不断地调整React的机制来使我们的项目性能更优。

在当前React的实现中，我们可以告诉React一个子树已经从它的兄弟里移动了，但是React无法得知这个子树被移动到了什么位置，基于这个原因，React会重新渲染所有子树。

因为React基于启发式算法，如果一些预定义的假设不成立，这时性能就会下降，例如：

1. 即使最终渲染的效果相同，但是由于组件类型不同，一样会造成重渲染
2. *key*值必须为稳定的，可预测的和唯一的。











