# Ghost开发初探

### config文件加载

在Ghost的根目录下，包含一个名为*config.example.js*的配置示例文件，在实际运行时，我们需要将该文件命名为*config.js*，并由*core/config/index.js*模块进行加载管理，首次加载流程如下：

1. 整个程序通过根目录下的*index.js*启动
2. 通过*core/server/utils/startup-check.js*进行启动环境变量及配置检查，如果不符合要求，会直接以一个特定的错误码退出应用程序
2. 得到一个ghostServer的实例
2. 获取实例的过程依赖*core/index.js*
3. *core/index.js*依赖*core/server/index.js*
4. 在*core/server/index.js*的执行过程依赖*core/config/index.js*，在*require* *core/config/index.js*时，该模块会返回一个实例化后的ConfigManager对象，在构造函数
5. 函数报错后，会


