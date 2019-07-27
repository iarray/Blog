### 拉取镜像
```shell
docker pull imageName:version
```
imageName是镜像名称
version是镜像的标签,一般指版本号


### 启动容器
```shell
docker run imageName:version
```
imageName是镜像名称
version是镜像的标签,一般指版本号

可选参数
* --name 容器名称 
* -d 后台运行
* -p 端口映射(本机端口:容器内端口)
* -v 目录映射(本机目录:容器内目录)
* -e 参数设置(KEY=VALUE)
* -P 将容器内部使用的网络端口映射到我们使用的主机上。

### 停止容器
```shell
docker stop containerIdOrName
```
容器的id或

### 查看正在运行的容器
```shell
docker ps
```

### 查看已下载的镜像
```shell
docker images
```

### 查看应用输出日志
```shell
docker log containerIdOrName
```

