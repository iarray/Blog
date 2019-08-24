### 拉取镜像
```shell
docker pull consul
```

### 运行
```shell
$ docker run -d --name=dev-consul -e CONSUL_BIND_INTERFACE=eth0 consul
```
