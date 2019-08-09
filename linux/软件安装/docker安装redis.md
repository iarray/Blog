### 1、搜索源

docker search redis

### 2、下载源

docker pull redis

3、创建并启动myredis容器(设置端口才可以连接)

docker run -d -p 6379:6379 -it   --name="myredis"  redis

4、查看容器

docker ps myredis

5、进入myredis容器

docker  exec -it myredis  redis-cli