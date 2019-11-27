## docker安装wordpress
1. 下载mysql容器, 指定版本5.7
```shell
docker pull mysql:5.7
```

2. 下载wordpress容器
```shell
docker pull wordpress
```

3. 运行mysql
```shell
cd /root
mkdir mysql-data
docker run --name mysql-wordpress -d --restart=always -p 8844:3306 -v /root/mysql-data/mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 mysql:5.7
```
解释一下参数，
* --name就是给容器取名字。
* -d就是把它放在后台运行
* -v加上后面的目录表示把容器中的/var/lib/mysql目录和宿主机中的/root/mysql-data目录做映射，把数据库数据保存在本地，
* -e后面加的参数就是设置mysql的密码，最后就是使用的镜像的名字
* -p: 端口映射，8844表示宿主，3306表示容器中的端口。 这里表示将宿主机的8844映射给镜像的3306.
* --privileged=true: CentOS系统下的安全Selinux禁止了一些安全权限，导致MySQL容器在运行时会因为权限不足而报错，所以需要增加该选项

4. 运行wordpress并链接mysql
```shell
cd /root
mkdir wordpress-html
docker run --name mywp -d --restart=always -p 8080:80 --link mysql-wordpress:mysql -v /root/wordpress-html:/var/www/html wordpress

```

还是先解释下参数--name就是给容器取名字，-d就是把容器放在后台运行-p表示容器的80端口和宿主机的80端口做映射--link表示和刚才生成的mysql容器做连接，-v表示把网站的目录和宿主机中的/root/wordpress-html做映射最后就是要使用的镜像的名字了

5. 进入Mysql容器
```shell
docker exec -it mysql-wordpress bash
mysql -u root -p
//输入密码
CREATE DATABASE IF NOT EXISTS wordpress default charset utf8 COLLATE utf8_general_ci;

```
