# Linux下安装php-redis扩展 #
- [下载扩展包](https://github.com/phpredis/phpredis/releases "下载扩展包")，或者执行：
```
wget https://github.com/phpredis/phpredis/archive/4.0.2.tar.gz
```
- 安装
```
tar -zxvf 4.0.2.tar.gz
cd phpredis-4.0.2
/usr/local/php/bin/phpize
./configure --with-php-config=/usr/local/php/bin/php-config
make && make install
```
注意自己的php安装位置
- 修改php.ini配置
```
vi /usr/local/php/etc/php.ini
添加
extension=redis.so
```
- **重启php-fpm服务**
```
/etc/init.d/php-fpm restart
```
- 查看redis扩展是否安装成功
```
php -m | grep redis
```
## 常见问题 ##
- Q. php -m 显示redis安装成功，但是phpinfo()显示没有安装成功
```
cd /usr/local/redis
find -name redis.so
找出地址
修改php.ini
extension=搜索到的地址
```
## 参考文章 ###
1. [Linux部署Redis及PHP-redis扩展](https://www.cnblogs.com/eeds-wangwei/p/11016160.html)