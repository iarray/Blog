```shell
docker pull eclipse-mosquitto

cd $(pwd)
mkdir mosquitto
cd mosquitto
mkdir data
mkdir config
mkdir log

docker run -name -it -p 1883:1883 -p 9001:9001 -v $(pwd)/mosquitto/config:/mosquitto/config -v $(pwd)/mosquitto/data:/mosquitto/data -v $(pwd)/mosquitto/log:/mosquitto/log eclipse-mosquitto
```
