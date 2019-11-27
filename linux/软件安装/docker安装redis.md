### 1、搜索源

```shell
docker search redis
```


### 2、下载源
```shell
docker pull redis
```

### 3、创建并启动myredis容器(设置端口才可以连接)
```shell
docker run -d -p 6379:6379 -it   --name="myredis"  redis

docker run -d --name myredis -p 6379:6379 --restart=always redis --requirepass "mypassword"
```

### 4、查看容器
```shell
docker ps myredis
```

### 5、进入myredis容器
```shell
docker  exec -it myredis  redis-cli
```