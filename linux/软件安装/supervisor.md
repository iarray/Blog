### 1. 安装

Ubuntu/Debian 安装   

```shell
apt-get install supervisor
```

Centos 安装
```shell
yum install supervisor
```

Pip安装
```shell
pip install supervisor
```

### 2.配置
默认配置文件路径在/etc/supervisor/supervisord.conf
添加需要进程守护的进程目录在/etc/supervisor/conf.d

如果不存在/etc/supervisor/supervisord.conf, 可以执行下面命令,手动创建
默认配置内容如下:
```
; supervisor config file

[unix_http_server]
file=/var/run/supervisor.sock   ; (the path to the socket file)
chmod=0700                       ; sockef file mode (default 0700)

[supervisord]
logfile=/var/log/supervisor/supervisord.log ; (main log file;default $CWD/supervisord.log)
pidfile=/var/run/supervisord.pid ; (supervisord pidfile;default supervisord.pid)
childlogdir=/var/log/supervisor            ; ('AUTO' child log dir, default $TEMP)

; the below section must remain in the config file for RPC
; (supervisorctl/web interface) to work, additional interfaces may be
; added by defining them in separate rpcinterface: sections
[rpcinterface:supervisor]
supervisor.rpcinterface_factory = supervisor.rpcinterface:make_main_rpcinterface

[supervisorctl]
serverurl=unix:///var/run/supervisor.sock ; use a unix:// URL  for a unix socket

; The [include] section can just contain the "files" setting.  This
; setting can list multiple files (separated by whitespace or
; newlines).  It can also contain wildcards.  The filenames are
; interpreted as relative to this file.  Included files *cannot*
; include files themselves.

[include]
files = /etc/supervisor/conf.d/*.conf
```

```shell
mkdir /etc/supervisor
echo_supervisord_conf > /etc/supervisor/supervisord.conf
```
### 3.添加应用到进程守护
这里以frps为例
``` shell
nano /etc/supervisor/conf.d/frps.conf
```
输入如下内容并保存:
```
[program:fprs]
command=/root/frps/frps -c /root/frps/frps.ini
user=root
autostart=true
autorestart=true
startsecs=60
stopasgroup=true
ikillasgroup=true
startretries=1
redirect_stderr=true
```
###  4.启动
```shell
supervisord -c /etc/supervisor/supervisord.conf
```


### 5.配置Supervisor开机启动
新建supervisord.service
```shell
sudo nano /usr/lib/systemd/system/supervisord.service
```

输入一下内容:
```
# dservice for systemd (CentOS 7.0+)
# by ET-CS (https://github.com/ET-CS)
[Unit]
Description=Supervisor daemon

[Service]
Type=forking
ExecStart=/usr/bin/supervisord -c /etc/supervisor/supervisord.conf
ExecStop=/usr/bin/supervisorctl shutdown
ExecReload=/usr/bin/supervisorctl reload
KillMode=process
Restart=on-failure
RestartSec=42s

[Install]
WantedBy=multi-user.target
```
执行命令
```shell
systemctl enable supervisord
```

### 6.常用的相关管理命令
```
supervisorctl restart <application name> ;重启指定应用
supervisorctl stop <application name> ;停止指定应用
supervisorctl start <application name> ;启动指定应用
supervisorctl restart all ;重启所有应用
supervisorctl stop all ;停止所有应用
supervisorctl start all ;启动所有应用
```