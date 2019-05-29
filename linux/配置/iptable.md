## 流量统计

### 统计特定端口流量
统计80端口入网流量:

	iptables -I INPUT -d 127.0.0.1 -p tcp --dport 80

统计80端口出网流量:

	iptables -I OUTPUT -s 127.0.0.1 -p tcp --sport 80


-d是目标IP，-s是源IP