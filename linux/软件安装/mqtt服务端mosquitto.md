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
1. openssl req -new -x509 -days 36500 -extensions v3_ca -keyout ca.key -out ca.crt 
	* 根据提示输入密码
	* 再次确认密码

2. 

### 3.运行
```shell

docker run --name mqttsrv -it -p 1883:1883 -p 9001:9001 -v $(pwd)/mosquitto/config/mosquitto.conf:/mosquitto/config/mosquitto.conf -v $(pwd)/mosquitto/data:/mosquitto/data -v $(pwd)/mosquitto/log:/mosquitto/log eclipse-mosquitto
```
