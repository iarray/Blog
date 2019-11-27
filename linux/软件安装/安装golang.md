### 1.创建目录

```
sudo mkdir /usr/local/go
```

### 2.下载go安装包

到https://studygolang.com/dl下载最新golang,在这里以1.13.4为例

```
cd /usr/local/go
wget https://studygolang.com/dl/golang/go1.13.4.linux-amd64.tar.gz
tar -C /usr/local -xzf go1.13.4.linux-amd64.tar.gz
```

### 3. 添加环境变量

```shell
nano /etc/profile 
#在末尾加上
export PATH=$PATH:/usr/local/go/bin
export GOPATH=/home/ubuntu/go
```

执行下面, 使环境变量生效

```
source /etc/profile
```

执行env可以查看是否配置成功