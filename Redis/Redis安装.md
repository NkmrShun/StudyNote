# Redis安装
## windows下安装Redis
- [下载地址](https://github.com/microsoftarchive/redis/releases) 根据平台系统需求下载相应版本，复制到需要安装的目录下解压：c:/redis
- 打开一个cmd窗口，cd到目录 c:/redis运行：
```
redis-server.exe redis.windows.conf
```
```
redis-cli.exe -h 127.0.0.1 -p 6379
```

[原文地址](https://www.runoob.com/redis/redis-install.html)

[windows下php安装redis扩展（详解版）](https://www.cnblogs.com/emmmmmm/p/10005726.html)
## linux下安装Redis
- 需要先安装支持库
```
yum -y install gcc-c++
```
- [下载redis](Linux部署Redis及PHP-redis扩展)，或在root下执行（版本可选）
```
wget http://download.redis.io/releases/redis-4.0.9.tar.gz
```
- 解压安装
```
tar -zxvf redis-4.0.9.tar.gz
mv redis-4.0.9 /usr/local/redis
cd /usr/local/redis
make 
make install
```
- 启动Redis服务
```
//以默认配置运行
./src/redis-server
//以配置文件运行
./src/redis-server redis.conf 
```
- 以后台模式启动Redis
修改配置文件
```
vi redis.conf
//修改daemonize为yes
daemonize=yes
```
启动服务
```
./src/redis-server redis.conf 
```
- 进入客户端
```
./src/redis-cli
```
- 配置redis服务管理脚本
```
cp /usr/local/redis/utils/redis_init_script /etc/init.d/redis
```
修改redis
```
vim /etc/init.d/redis
修改
CONF="/usr/local/redis/redis.conf"
```
启动redis服务
```
/etc/init.d/redis start
```
## 参考资料 ##

 1. [linux安装redis](https://www.cnblogs.com/gaojingya/p/10600418.html)
 3. [Linux部署Redis及PHP-redis扩展](https://www.cnblogs.com/eeds-wangwei/p/11016160.html)