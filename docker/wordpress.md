# 使用docker简单建立wordpress #
1. 获取mysql镜像
```
docker pull mysql:5.6
```
2. 获取wordpress镜像
```
docker pull wordpress
```
3. 创建mysql容器
```
docker run --name mysql5.6 -e MYSQL_ROOT_PASSWORD=123456 -p 3306:3306 -d mysql:5.6
```
4. 创建wordpress
```
docker run --link mysql:wordpressdb -e WORDPRESS_DB_HOST=wordpressdb:3306 -e WORDPRESS_DB_PASSWORD=123456 -e WORDPRESS_DB_NAME=wordpress -p 80:80 wordpress
```