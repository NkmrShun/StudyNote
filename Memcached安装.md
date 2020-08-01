# Memcached安装 #
## Linux 下安装 ##
- 安装C编译器
```
yum -y install gcc
```
- 先安装libevent类库
```
wget https://github.com/libevent/libevent/releases/download/release-2.1.8-stable/libevent-2.1.8-stable.tar.gz
tar -zxvf libevent-2.1.8-stable.tar.gz
cd libevent-2.1.8-stable
./configure --prefix=/usr/local/libevent
make && make install
```
- 安装Memcached，[下载地址](http://memcached.org/downloads)
```
wget http://memcached.org/files/memcached-1.6.6.tar.gz
tar -zxvf memcached-1.6.6.tar.gz
cd memcached-1.6.6
./configure --prefix=/usr/local/memcached
make
make install
```
- 启动Memcached
```
memcached -m 64 -p 11211 -u nobody -d
```




