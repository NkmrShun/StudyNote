# 从零开始学docker #
## 一、安装docker ##
### Ubuntu [链接](https://docs.docker.com/engine/install/ubuntu/)###
#### 某些linux系统可能会预安装了docker，需要先删除旧版本的docker  ####
```
sudo apt-get remove docker docker-engine docker.io containerd runc
```
#### 设置 Repository ####
1. 更新Apt包    
```
sudo apt-get update
```
2. 安装支持库
```
sudo apt-get install \  
apt-transport-https \
ca-certificates \
curl \
gnupg-agent \
software-properties-common
```
3. 添加Docker的官方GPG密钥
```
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```
4. 使用以下命令来设置稳定的Repository
```
sudo add-apt-repository \
"deb [arch=amd64] https://download.docker.com/linux/ubuntu \
$(lsb_release -cs) \
stable"
```

#### 安装docker引擎 ####
1. 更新apt程序包索引，并安装最新版本的Docker Engine和容器
```
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
```
2. 要安装特定版本的Docker Engine,请在仓库中选择版本
```
  apt-cache madison docker-ce

  docker-ce | 5:18.09.1~3-0~ubuntu-xenial | https://download.docker.com/linux/ubuntu  xenial/stable amd64 Packages
  docker-ce | 5:18.09.0~3-0~ubuntu-xenial | https://download.docker.com/linux/ubuntu  xenial/stable amd64 Packages
  docker-ce | 18.06.1~ce~3-0~ubuntu       | https://download.docker.com/linux/ubuntu  xenial/stable amd64 Packages
  docker-ce | 18.06.0~ce~3-0~ubuntu       | https://download.docker.com/linux/ubuntu  xenial/stable amd64 Packages
  ...
```
```
  sudo apt-get install docker-ce=<VERSION_STRING> docker-ce-cli=<VERSION_STRING> containerd.io
```

#### 卸载Docke ####
1. 卸载Docker Engine，CLI和Containerd软件包
```
  sudo apt-get purge docker-ce docker-ce-cli containerd.io
```
2. 主机上的映像，容器，卷或自定义配置文件不会自动删除。要删除所有图像，容器和卷
```
  sudo rm -rf /var/lib/docker
```
3. 验证安装完毕
```
  docker version
```

## 二、使用Docker ##
### Docker命令 ###
#### Docker镜像 ####
1. 列出所有镜像
```
docker image ls 
```
```
docker images
```
2. 拉取镜像
```
docker pull [选项] [Docker Registry 地址[:端口号]/]仓库名[:标签]
```
例子：
```
docker pull library/centos:latest
```
拉取一个镜像，需要指定Docker Registry的地址和端口号，默认是Docker Hub，还需要指定仓库名和标签，仓库名和标签唯一确定一个镜像，而标签是可能省略，如果省略，则默认使用latest作为标签名，另外，仓库名则由作者名和软件名组成。
那么，我们上面使用CentOS，那是因为省略作者名，则作者名library，表示Docker官方的镜像，所以上面的命令等同于：
```
docker pull library/centos:latest
```
3. 运行镜像
```
docker run [镜像名]
```
4. 删除镜像
```
dockere image rm image_name/image_id
```
如果有使用该镜像创建的容器未删除，需要先停止该镜像的容器再删除

#### Docker容器 ####
1. 启动容器
通过镜像启动容器
```
docker run 镜像名
```
启动一个停止的容器
```
docker start 容器id
```
2. 停止容器
```
docker stop 容器id
```
2. 查看容器
```
docker 容器id
```
或者简洁写法
```
docker ps
```
4. 删除容器
```
docker rm 容器id
```
删除所有容器
```
docker rm $(docker ps -q)
```
删除所有退出的容器
```
docker container prune
```
5. 进入容器(?)
```
docker exec -it container_id command
```


