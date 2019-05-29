## 安装docker
1、Docker 要求 CentOS 系统的内核版本高于 3.10 ，查看本页面的前提条件来验证你的CentOS 版本是否支持 Docker 。

1. 通过 uname -r 命令查看你当前的内核版本
```shell
 $ uname -r
```

2. 使用 root 权限登录 Centos。确保 yum 包更新到最新。
```shell
 $ sudo yum update
```
3. 卸载旧版本
```shell
 $ sudo yum remove docker  docker-common docker-selinux docker-engine
```
4. 安装需要的软件包
```shell
$ sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```
5. 设置yum源
```shell
$ sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```
6. 查看可安装docker版本
```shell
$ yum list docker-ce --showduplicates | sort -r
```
7. 安装docker
```shell
$ sudo yum install docker-ce  #由于repo中默认只开启stable仓库，故这里安装的是最新稳定版17.12.0
$ sudo yum install <FQPN>  # 例如：sudo yum install docker-ce-17.12.0.ce
```

8. 启动并加入开机启动
```shell
$ sudo systemctl start docker
$ sudo systemctl enable docker
```

9. 
