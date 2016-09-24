# ES2015 in Node

ES2015规范大大丰富了javascript的语法，但是在node大部分版本中还并未对其有全面的支持。所以，如果需要在生产环境中的Node.js中使用ES2015语法，需要对源代码进行编译。

笔者主要通过**预编译**及**原生支持**两个方面来介绍如何在开发中应用ES2015规范及后续打包上线等知识。

## 预编译

Babel应该是现在最流行的编译器了，使用Babel并完成配置后，即可在项目中使用ES2015规范的javascript了。在使用之前，我们需要了解一些前置知识。

### Babel体系组成

#### Babel CLI

#### babel-node

#### preset



### 配置Babel

#### .babelrc文件

需要在项目根目录下新建*.babelrc*文件，内容格式如下：

```json
{
  "presets": [],
  "plugins": []
}
```

presets和plugins的关系是，presets是一组plugins.

## 原生支持

