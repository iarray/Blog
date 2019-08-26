## 镜像使用
### 查找镜像
```shell
docker search imageName
```

### 拉取镜像
```shell
docker pull imageName:version
```
imageName是镜像名称
version是镜像的标签,一般指版本号

### 通过dockerfile构建镜像
```shell
docker build dockerfile
```

### 设置镜像标签
```shell
docker tag containerIdOrName user/repositoryName:newTag
```
docker tag 镜像ID或名称 用户名称、镜像源名(repository name)和新的标签名(new tag)。

---
## 容器使用
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
参数: 容器的id或名称

### 删除容器
```shell
docker rm containerIdOrName
```
参数: 容器的id或名称


### 查看正在运行的容器
```shell
docker ps
```
可选参数:
* -l 查看最近一次创建的容器

### 查看已下载的镜像
```shell
docker images
```

### 查看应用输出日志
```shell
docker log containerIdOrName
```
参数: 容器的id或名称


