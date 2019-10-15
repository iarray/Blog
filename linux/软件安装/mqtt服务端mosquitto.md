### 1.准备工作
```shell
docker pull eclipse-mosquitto

cd $(pwd)
mkdir mosquitto
cd mosquitto
mkdir data
mkdir config
mkdir log

chmod -R 0777 $(pwd)/mosquitto/
```


### 2.配置ssl


### 3.运行
```shell

docker run --name mqttsrv -it -p 1883:1883 -p 9001:9001 -v $(pwd)/mosquitto/config:/mosquitto/config -v $(pwd)/mosquitto/data:/mosquitto/data -v $(pwd)/mosquitto/log:/mosquitto/log eclipse-mosquitto
```
