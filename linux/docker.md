### 拉取镜像
```shell
docker pull imageName:version
```
imageName是镜像名称
version是镜像的标签,一般指版本号


### 启动容器
```shell
docker run 
```
可选参数
* --name 容器名称 
* -d 后台运行
* -p 端口映射(本机端口:容器内端口)
* -v 目录/root/mysql-data/mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 mysql:5.7