### 为什么需要有服务注册与服务发现？

### 拉取镜像
```shell
docker pull consul
```

### 运行
```shell
$ docker run -d --name=c1 -p 8500:8500 -e CONSUL_BIND_INTERFACE=eth0 consul agent --server=true --bootstrap-expect=3 --client=0.0.0.0 -ui
```
