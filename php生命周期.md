php声明周期
## 运行模式 ##
PHP两种运行模式是WEB模式、CLI模式。
1. 当我们在终端敲入php这个命令的时候，使用的是CLI模式。
2. 当使用Nginx或者别web服务器作为宿主处理一个到来的请求时,使用的是WEB模式。

## 生命周期 ##
1. 模块初始化（MINIT），即调用 php.ini 中指明的扩展的初始化函数进行初始化工作，如 mysql 扩展。

2. 请求初始化（RINIT），即初始化为执行本次脚本所需要的变量名称和变量值内容的符号表，如 $_SESSION变量。

3. 执行该PHP脚本。

4. 请求处理完成(Request Shutdown)，按顺序调用各个模块的 RSHUTDOWN 方法，对每个变量调用 unset函数，如 unset $_SESSION 变量。

5. 关闭模块(Module Shutdown) ， PHP调用每个扩展的 MSHUTDOWN 方法，这是各个模块最后一次释放内存的机会。这意味着没有下一个请求了。


框架运行在第三阶段