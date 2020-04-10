### 1. 拉取镜像

```shell
docker pull nsqio/nsq
```

### 2. 启动nsq lookup 进程守护

nsqlookupd是守护进程负责管理拓扑信息。客户端通过查询 nsqlookupd 来发现指定话题（topic）的生产者，并且 nsqd 节点广播话题（topic）和通道（channel）信息

简单的说nsqlookupd就是中心管理服务，它使用tcp(默认端口4160)管理nsqd服务，使用http(默认端口4161)管理nsqadmin服务。同时为客户端提供查询功能, 提供一个http查询服务，给客户端定时更新nsqd的地址目录 

```
docker run -d --name lookupd -p 4160:4160 -p 4161:4161 --restart=always nsqio/nsq /nsqlookupd
```

### 3. 启动nsq

 nsqd 是一个守护进程，负责接收，排队，投递消息给客户端 

假设lookup运行在192.168.0.105上

```
docker run -d --name nsqd -p 4150:4150 -p 4151:4151 --restart=always  nsqio/nsq /nsqd     --broadcast-address=192.168.0.105     --lookupd-tcp-address=192.168.0.105:4160
```

### 4. 启动web管理后台

```
docker run -d --name nsqadmin -p 4171:4171 nsqio/nsq /nsqadmin --lookupd-http-address=192.168.0.105:4161
```



### 5. 单机部署测试

```shell
$docker network create nsq

#查看网络id
$docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
e9f4691302bf        bridge              bridge              local
c39237e6c0c5        host                host                local
14f58c210df8        none                null                local
e14a312f0397        nsq                 bridge              local

#找到对应网络id的ip地址
$ifconfig
br-e14a312f0397: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.19.0.1  netmask 255.255.0.0  broadcast 172.19.255.255
        inet6 fe80::42:bcff:fe36:d2c3  prefixlen 64  scopeid 0x20<link>
        ether 02:42:bc:36:d2:c3  txqueuelen 0  (Ethernet)
        RX packets 293  bytes 373401 (373.4 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 239  bytes 28352 (28.3 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0


docker run -d --net=nsq --name lookupd -p 4160:4160 -p 4161:4161 --restart=always nsqio/nsq /nsqlookupd

docker run -d --net=nsq --name nsqd -p 4150:4150 -p 4151:4151 --restart=always  nsqio/nsq /nsqd     --broadcast-address=172.19.0.1     --lookupd-tcp-address=172.19.0.1:4160

docker run -d --net=nsq --name nsqadmin -p 4171:4171 nsqio/nsq /nsqadmin --lookupd-http-address=172.19.0.1:4161
```



### 6. 注意防火墙端口开启

```
#nsqd 端口
ufw allow 4150
ufw allow 4151

#nsqlookupd 端口
ufw allow 4160
ufw allow 4161

#nsqadmin 端口
ufw allow 4171
ufw allow 4170
```



