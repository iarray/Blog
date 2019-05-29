## docker安装wordpress
1. 下载mysql容器
```shell
docker pull mysql
```

2. 下载wordpress容器
```shell
docker pull wordpress
```

3. 运行mysql
```shell
cd /root
mkdir mysql-data
docker run --name mysql-wordpress -d -v /root/mysql-data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD="Sa123" mysql
```
解释一下参数，--name就是给容器取名字。-d就是把它放在后台运行，-v加上后面的目录表示把容器中的/var/lib/mysql目录和宿主机中的/root/mysql-data目录做映射，把数据库数据保存在本地，-e后面加的参数就是设置mysql的密码，最后就是使用的镜像的名字

4. 运行wordpress并链接mysql
