#### 拉取
```shell
docker pull mongo
```

`````shell
运行
docker run -d -p 27017:27017 --name mongodb \
    -e MONGO_INITDB_ROOT_USERNAME=root \
    -e MONGO_INITDB_ROOT_PASSWORD=123456 \
    mongo
```