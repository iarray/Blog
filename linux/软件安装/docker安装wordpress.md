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
docker run --name mysql-wordpress -d -v /root/mysql-data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD="woyaoxuehuilinux" hypriot/rpi-mysql
```

4. 运行wordpress并链接mysql
