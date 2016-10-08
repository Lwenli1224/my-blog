# RequireJS源码学习（1）——核心函数

RequireJS是最早的[AMD](https://github.com/amdjs/amdjs-api/blob/master/AMD.md)模块加载器之一，我们可以利用其方便地组织高质量的模块化代码，并且配套有专门的打包压缩工具。很多后续的优秀模块加载器的设计思想都脱胎于该项目，所以学习其源码十分必要。

## 核心需求

根据AMD规范，其加载器要求有以下四个主要功能

- 定义AMD模块
- require()方法
- loader插件
- 通用设置

### AMD模块

可通过`define`方法定义模块

```javascript
define(id?, dependencies?, factory)
```

#### id

模块唯一标识，有以下几条重要原则

- 可选参数，可自定义，默认为模块被loader请求的路径
- 路径分为“顶级路径”和“相对路径”
- 解析顶级路径时，需要将模块ID作为路径来看待，顶级路径的根路径即为模块ID的根路径（待验证）
- 路径不需要“.js”扩展名

**RequireJS中实现**

```javascript
// 涉及变量
var commentRegExp = /(\/\*([\s\S]*?)\*\/|([^:]|^)\/\/(.*)$)/mg,
    cjsRequireRegExp = /[^.]\s*require\s*\(\s*["']([^'"\s]+)["']\s*\)/g;
    
// 其它代码...
 
// 定义函数
define = function (name, deps, callback) {
        var node, context;

        // 允许匿名模块，当首个参数不为模块ID名称时，校正参数位置
        if (typeof name !== 'string') {
            // 假设第一个参数为依赖模块数组
            callback = deps;
            deps = name;
            name = null;
        }

        // 不存在依赖模块时处理
        if (!isArray(deps)) {
            callback = deps;
            deps = null;
        }

        // 如果不存在依赖模块，并且存在回调函数，判断是否为CMD规范模块，并转换为字符串后分析其依赖模块
        if (!deps && isFunction(callback)) {
            deps = [];
            // 
            if (callback.length) {
                callback
                    .toString()
                    .replace(commentRegExp, commentReplace)
                    .replace(cjsRequireRegExp, function (match, dep) {
                        deps.push(dep);
                    });

                //May be a CommonJS thing even without require calls, but still
                //could use exports, and module. Avoid doing exports and module
                //work though if it just needs require.
                //REQUIRES the function to expect the CommonJS variables in the
                //order listed below.
                deps = (callback.length === 1 ? ['require'] : ['require', 'exports', 'module']).concat(deps);
            }
        }

        //If in IE 6-8 and hit an anonymous define() call, do the interactive
        //work.
        if (useInteractive) {
            node = currentlyAddingScript || getInteractiveScript();
            if (node) {
                if (!name) {
                    name = node.getAttribute('data-requiremodule');
                }
                context = contexts[node.getAttribute('data-requirecontext')];
            }
        }

        //Always save off evaluating the def call until the script onload handler.
        //This allows multiple modules to be in a file without prematurely
        //tracing dependencies, and allows for anonymous module support,
        //where the module name is not known until the script onload event
        //occurs. If no context, use the global queue, and get it processed
        //in the onscript load callback.
        if (context) {
            context.defQueue.push([name, deps, callback]);
            context.defQueueMap[name] = true;
        } else {
            globalDefQueue.push([name, deps, callback]);
        }
    };
```










