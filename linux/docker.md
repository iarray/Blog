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
--name mysql-wordpress -d -p 8844:3306 -v /root/mysql-data/mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 mysql:5.7