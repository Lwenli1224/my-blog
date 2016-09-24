# 在Jenkins中使用nvm

nvm是一款非常好用的管理多个node版本的工具，在命令行中安装没有什么问题，但是在Jenkins中直接使用，就会出现`command not found`的报错提示。

我们来分析一下原因：

## 简单解决

- 按照官网说明安装nvm
- 得到nvm.sh完整路径，如/Users/ye/.nvm/nvm.sh
- 在Jenkins中使用`source /Users/ye/.nvm/nvm.sh`
- 即可在其后使用`nvm use 4.4.3`



