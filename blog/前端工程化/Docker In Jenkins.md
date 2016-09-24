# Docker In Jenkins

Docker和Jenkins结合可以大大提升我们的CI效率，每个新发布的前端版本需要打tag，Jenkins通过tag拉取git源代码，构建出响应的前端上线代码包或前端机镜像。目标是美好的，但实现过程仍需要相关的基础知识和耐心的配置，尤其是在Docker on Mac的情况下进行配置。

## 前提

- Docker for Mac
- Jenkins on Mac
- Jenkins基本原理及使用
- Shell脚本基础
- Docker基本原理及使用


## 解决方案

新建jenkins-game项目，随便echo一些语句，测试jenkins环境的正确性，有两种方案可供Jenkins on Mac的用户选择。

### 使用Docker环境变量进行配置

在安装Docker for Mac后，编写jenkins脚本

```shell
docker images
```

运行发现jenkins报错，docker: command not found。

因为在*nix系统中，不包含执行路径的指令时，系统是透过PATH这个系统变量来搜寻指令

我们重新编写jenkins脚本，来观察docker的执行路径是否包含在PATH中

```shell
where docker
echo $PATH
```

### 使用Docker plugin




