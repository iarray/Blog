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
* --restart=always 为了应对停电等情况影响正常web项目的访问，会把Docker容器设置为开机自动启动。 
	* no -  容器退出时，不重启容器；
	* on-failure - 只有在非0状态退出时才从新启动容器；
	* always - 无论退出状态是如何，都重启容器；

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

### 进入容器
```shell
sudo docker exec --help   
```
例如:
```shell
docker exec -it 775c7c9ee1e1 /bin/bash  
```


### 查看已下载的镜像
```shell
docker images
```

### 删除镜像镜像
```shell
docker rmi imageIdOrName
```


### 查看应用输出日志
```shell
docker log containerIdOrName
```
参数: 容器的id或名称


### 查看容器状态(cpu, 内存)
```shell
docker stats containerIdOrName 
```

### 容器间连接
```sh

```


## 多容器管理 docker-compose
### 1.安装
[官方安装](https://docs.docker.com/compose/install/
)或者到[github release](https://github.com/docker/compose/releases
)页面安装

### 2.使用
我们平时操作 docker 还是很原始的一系列动作，比如
```shell
1. docker pull xxxx
2. docker run -name xx -d xxxx
3. .....
```
把这一些列的动作写到docker-compose.file 里面就很好了。 你只需要写好后 只运行一句
```
docker-compose up -d
```

