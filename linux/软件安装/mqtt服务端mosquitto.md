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
1. step1 -- 产生CA的key和证书文件
  openssl req -new -x509 -days 36500 -extensions v3_ca -keyout ca.key -out ca.crt 
	* 根据提示输入密码
	* 再次确认密码
	* 后面直接回车略过
2. 为mosquittoserver产生一个私钥文件server.key
  openssl genrsa -out server.key 2048
  openssl genrsa -des3 -out server.key 2048
	* 根据提示输入两次密码
3. 为mosquitto server产生一个签发证书的请求文件“server.csr”
  openssl req -out server.csr -key server.key -new
4. 

### 3.运行
```shell

docker run --name mqttsrv -it -p 1883:1883 -p 9001:9001 -v $(pwd)/mosquitto/config/mosquitto.conf:/mosquitto/config/mosquitto.conf -v $(pwd)/mosquitto/data:/mosquitto/data -v $(pwd)/mosquitto/log:/mosquitto/log eclipse-mosquitto
```
