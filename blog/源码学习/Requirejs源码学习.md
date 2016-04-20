# Requirejs源码学习

## 架构

## 实现

### 判断

- isBrowser：window是否存在，navigator是否存在，document是否存在
- is(Function|Array)：判断类型函数
- 加载完成Regx：通过readyState判断资源是否加载完成，对PS3特殊处理

### 工具方法

- each：迭代数组
- eachProp：迭代对象
- mixin：合并对象，类似extend，包含深拷贝功能
- bind：绑定函数作用域
- scripts：返回所有script dom节点
- getGlobal：获得全局对象上的“连点”结果
- trimDots：去掉路径中的“.“和“..”
- normalize：获取模块绝对路径

## 重点函数分析

### newContext(contextName)

生成context对象，保存加载器方法的引用

暴露重点方法：

- context.configure()
- context.makeRequire()

### context.configuire(cfg)

解析处理配置对象

cfg：配置对象

函数中包含以下处理：

- 如果baseUrl没有以反斜杠结尾，增加反斜杠
- 转换urlArgs
- 与默认配置合并
- 处理shim



### normalize(name, baseName, applyMap)

name: 模块相对路径，包含文件名

baseName: 基于此路径查找模块

applyMap: 模块多版本匹配

如果name和baseName均存在，先使用split函数，将name和baseName均分解为数组后再连接。使用trimDots函数去掉数组中的“.“和”..”，尽可能得到绝对路径。

然后分析name和applyMap之间的关系。

### trimDots(ary)

ary: 包含“.”或“..”的路径数组

遍历路径数组，如果当前的元素为“.”，直接删除该元素，因为它不起任何作用。

如果当前元素为“..”，则需要详细判断：
	
- 当前索引为0，不存在绝对根路径，进入下一轮循环，不做处理
- 当前索引为1，下一元素“..”，说明需要进到上两级，仍不存在绝对根路径，不做处理
- 上一元素为“..”，无法确定上一级目录，仍不做处理
- 当条件不为以上三种时，进行处理，删除“..”和上级目录，调整循环索引

### define(name, deps, callback)

定义模块

name：模块名称，可不传该参数，建议为匿名模块
deps：模块依赖数组，不为必选
callback：模块执行函数

分析参数：
	
- 如果不存在依赖模块，需要分析是否为CMD规范模块
- 如果callback的length大于0，分析函数（分析注释的regx可能有bug），需要删除注释，分析require函数依赖的模块，装入依赖数组中
- 为了支持IE6-8，需要使用特殊的方法处理
- 如果context已存在，将[name,deps,callback]加入到context的依赖数组中；反之，加入到全局依赖数组中


### req(deps, callback, errback, optional)

核心函数，加载器

deps：依赖数组
callback：加载成功回调
errback：加载失败回调
optional：

分析参数：

- 如果第一个参数为对象，那么认为其为config对象，根据config对象，设置context
- 如果context为设置，调用newContext方法生成context
- 如果config对象存在，调用context.configure()方法处理config


## 工作流程




