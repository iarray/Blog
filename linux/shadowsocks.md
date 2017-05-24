## Shadowsocks 搭建
### 安装
* Debian / Ubuntu

		sudo apt-get install python-pip
		pip install shadowsocks
* Centos

		yum install python-setuptools && easy_install pip
		pip install shadowsocks 

### 使用

* 命令行
  
	  	ssserver -p 443 -k password -m aes-256-cfb
* 如果要后台运行
  
		sudo ssserver -p 443 -k password -m rc4-md5 --user nobody -d start
    
* 如果要停止

		sudo ssserver -d stop
* 如果要检查日志
    
		sudo less /var/log/shadowsocks.log
用 -h 查看所有参数。你也可以使用 配置文件 进行配置。


### 配置文件
* 单用户配置

		{
		"server":"你的服务器ip地址",
		"server_port":8388,
		"local_address": "127.0.0.1",
		"local_port":1080,
		"password":"mypassword",
		"timeout":300,
		"method":"aes-256-cfb",
		"fast_open": false
		}

* 多用户配置

		{
		"server": "你的服务器ip地址",
		"port_password": {
		   "8381": "foobar1",
		   "8382": "foobar2",
		   "8383": "foobar3",
		   "8384": "foobar4"
		},
		"timeout": 300,
		"method": "aes-256-cfb"
		}

* 配置说明

		server 	你的服务器IP地址
		local_address 	本地代理地址
		local_port 	本地代理端口
		port_password 	自己设置的服务器端口和密码
		timeout 	超时断开，单位为秒
		method 	加密方式，默认为aes-256-cfb
		fast_open 	是否使用TCP

* 使用配置文件运行

		ssserver -c /etc/shadowsocks.json
		
		后台运行
		开始：ssserver -c /etc/shadowsocks.json -d start
		结束：ssserver -c /etc/shadowsocks.json -d stop 

### 设置开机启动
编辑rc.local文件
	vi /etc/rc.local
然后将里面的最后带有ssserver的注释(在语句前加一个#即可注释)，然后换行输入`ssserver -c /etc/shadowsocks.json -d start`保存退出即可。


----------


### OpenVZ 网络加速
* finalspeed
	
		//一键安装
		wget -N --no-check-certificate https://raw.githubusercontent.com/91yun/finalspeed/master/install_fs.sh && bash install_fs.sh

		//一键卸载
		wget -N --no-check-certificate https://raw.githubusercontent.com/91yun/finalspeed/master/install_fs.sh && bash install_fs.sh uninstall

		//其它操作 
		/etc/init.d/finalspeed start #启动
		/etc/init.d/finalspeed stop #停止命令
		/etc/init.d/finalspeed status # 状态命令（查看日志）
 
### KVM 网路加速
* 锐速

		//CentOS6内核更换为：2.6.32-504.3.3.el6.x86_64：
		rpm -ivh http://soft.91yun.org/ISO/Linux/CentOS/kernel/kernel-firmware-2.6.32-504.3.3.el6.noarch.rpm

		rpm -ivh http://soft.91yun.org/ISO/Linux/CentOS/kernel/kernel-2.6.32-504.3.3.el6.x86_64.rpm --force

		//CentOS7内核更换为： 3.10.0-229.1.2.el7.x86_64：
		rpm -ivh http://soft.91yun.org/ISO/Linux/CentOS/kernel/kernel-3.10.0-229.1.2.el7.x86_64.rpm --force

		//Ubuntu14.04更换内核为3.13.0-24-generic：
		sudo apt-get install linux-image-extra-3.13.0-24-generic
		//查看现有内核		
		dpkg -l|grep linux-image
		//卸载其他内核
		sudo apt-get purge 填写上面列出的内核
		//更新 grub 系统引导文件		
		sudo update-grub 
		reboot

		//一键安装
		wget -N --no-check-certificate https://raw.githubusercontent.com/91yun/serverspeeder/master/serverspeeder-all.sh && bash serverspeeder-all.sh

		//一键卸载
		chattr -i /serverspeeder/etc/apx* && /serverspeeder/bin/serverSpeeder.sh uninstall -f

		//其它操作 
		/serverspeeder/bin/serverSpeeder.sh start #启动锐速
		/serverspeeder/bin/serverSpeeder.sh stop #停止锐速
		/serverspeeder/bin/serverSpeeder.sh restart #重启锐速
		/serverspeeder/bin/serverSpeeder.sh status #锐速运行状态




----------

### 安装管理工具ss-mgr
ss-mgr 推荐使用ss-libev版本,上面提到的是python版的安装

安装ss-libev

		sudo add-apt-repository ppa:max-c-lv/shadowsocks-libev
		sudo apt-get update
		sudo apt install shadowsocks-libev

如果提示`add-apt-repository not found` 请先安装下面的:

		apt-get install python-software-properties 
		apt-get install software-properties-common 

安装node.js (6.0以上版本)
	
		curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
		sudo apt-get install -y nodejs
如果 `node` 命令使用不了, 但nodejs命令可以用,执行下面的
		
		sudo ln -s /usr/bin/nodejs /usr/bin/node

安装npm

		apt-get install npm

安装SSMGR
	
		npm i -g shadowsocks-manager
		ln -s /usr/local/nodejs/node-v6.9.1-linux-x64/bin/ssmgr /usr/local/bin/ssmgr
ubuntu 16.04位置不一样,可以使用下面的
	
		ln -s /usr/local/nodejs/node-v6.9.1-linux-x64/bin/ssmgr /usr/local/lib/ssmgr



----------
 

配置

我们在根目录下创建一个目录 .ssmgr

    mkdir -p ~/.ssmgr/

配置ssmgr

	nano ~/.ssmgr/ss.yml

复制下面内容

	type: s
	empty: false
	
	shadowsocks:
	  address: 127.0.0.1:6001
	  #portRange: 50000-51000,55000,55002
	
	manager:
	  address: 0.0.0.0:6002
	  password: '123456'
	
	db: 'db.sqlite'
	
	# db:
	#   host: '1.1.1.1'
	#   user: 'root'
	#   password: 'abcdefg'
	#   database: 'ssmgr'

按键盘 Ctrl 和 X 键，然后按一下 Y，再按一下回车即可保存此文件


----------


配置WebGui

	nano ~/.ssmgr/ss.yml

复制下面内容	

	type: m
	empty: false
	
	manager:
	    address: 127.0.0.1:6002
	    password: '123456'
	
	plugins:
	    flowSaver:
	        use: true
	    user:
	        use: true
	    account:
	        use: true
	        pay:
	            hour:
	                price: 0.05
	                flow: 500000000
	            day:
	                price: 0.5
	                flow: 7000000000
	            week:
	                price: 3
	                flow: 50000000000
	            month:
	                price: 10
	                flow: 200000000000
	            season:
	                price: 30
	                flow: 200000000000
	            year:
	                price: 120
	                flow: 200000000000
	    email:
	        use: true
	        username: 'mmmxcc@yeah.net'
	        password: 'yue12345'
	        host: 'smtp.yeah.net'
	        
	    webgui:
	        use: true
	        host: '0.0.0.0'
	        port: '80'
	        site: 'http://127.0.0.1'
	        gcmSenderId: '456102641793'
	        gcmAPIKey: 'AAAAGzzdqrE:XXXXXXXXXXXXXX'
	    alipay:
	        use: true
	        appid: 2017051207221985
	        notifyUrl: ''
	        merchantPrivateKey: 'MIIEogIBAAKCAQEAuGcQZUeERDN5a18S339HoifX/pEFIJ86/SyIPhi84hcZ/ER4JnkWRvOOIS1hVP3PJbS8WrAD55fb90sKNHEkuFoIYhJanANU5bjd3Tc5vFNill8zQjuwQhMuuxNGCmns67lDzC3MUSnOvJNZe6CmC1sdkDoEAe4Ltf23S0Ct9huaXwY4NU9OeDYZYE9rMD3E547PjSV2/TuW7OxDt9b03GGwZ6+aOPlATDGf7qOQ03/NedADbrf8M7ddAzyW1+vddF8jGR5S7GTk1xs8rDfiAuW2SESqirSrKr5qHVBHt8qU8ius0/U3tX49K0GJ2tv+YQzdeCSNHHXU8G/CpfNx6wIDAQABAoIBAGid6DpdaZe70E3vIWj07/H6Qttk8RDB5ZiO9DiuljJ6wr5RJg1FszLLqL3/3fF3EVemAxo8M+rnR98zjnWUHy4LvifNm+RZK4gs6oo/c5z5fbwsBIu2C1hhQ6jU1W7BC63sbJ9g+EKvwl9QBXtS+AEHT21szKs/nxs/9wZVipxNs+wbipNdl1FlPXy2VMLZ8r2zB6I97Hzd4otPSd5fvpfDzaiWN7mrw8nt+CSh64opq53b+eOAHrSvrPdrxmh0pkONRieJ0zwwC8pify5cJqwDzYWH9xihXUiTItgVNuOF3FZMLCVmEh2GW1jzfX4kjmXHR7ZohDwQHOI83718LWECgYEA42RDRer9nVZG8S08lxx2hmMPag9a4E3U9lws0LsiBWZ2fwK7pmCoPV9woVolxKkDD1hTMVBkb1m430ObTo7gVhwyWZu2cdhdrJGfkiwGVBWQj5rMfTjGaC9t/hjH6Xn5y5oTZXGyWHbbgKurXHwKkKa4HjRsZ1feqzbr3hv2CDECgYEAz5o6L+nwO8tgRL04XtBDf301L2DgXJF9JV+YZRaSBUUA1lPYDusjJw+Gp8RPFj5glWanED2c4ik0xRl/dpgGcwZTkRZZAjd/vvwpAwEsJHN0KlaYVWpiR0rSbRVaWO9MTT5XLeeO05HxnU5fQCmoJd1E2kVEVVrNpW+qc0D1cNsCgYABm0C6armV9fkUVRx7eNEBjAuc1TTwJrydbPNIGjpYTWwC47MNrtsha94w1aE0rXj/D8GutIGxI7IhqamcOXguQ467gVUdosiuomy9LKBy0aAu04okTFVigQtG40fuR1VTceg3LEF1TImqdAwG4D123iao1/f4Fu/Ux9OMwyfO0QKBgB48XyclUkjTKRU53eDpQjtqN/nLiUHSKsDruF1YuTGtSB7vHTOnCXqh50G99KrPsO9xgPl7j8Lcr4PZX9gc8iuEi11BPPt2q9NcFHkMjapNamjULeGa2e96NUWP65WyXJGPd9EGvaWQxvjbNkI5DvVc0a9ckURBMlxa9B9PIbArAoGAQccrgw1NV29+3A7bilAYLfTuZnk9CJrXYhmEjQrwLM4eQawftvIzbE2lDYOgI8zNaYzvJam49NiH2YvN0gtMIWnpm6UDDjNX+gkWrUJz2/UwJxTddys6OWuKVgDbUj98fEVdgCHEn0tlc/cXvb8M1X5Wj678c+k9YdS2dIqYwtU='
	        alipayPublicKey: 'MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAmeB6yomjdReHlKW3Z3cJqteqdzvNtp+mBecZYcM0/SYpJDgZwEojpE9944u+T2jxo7yJH1ndV0LmV004bocFakE9ZIlajYqsO4FpSWWRoBmtD95D36olGQWLpiOA/RYa7kpGWPfl8cl3Qis/Efl3Dkm++ns8GdYZvQkNbLl5RfLAUfBDp6DCxuWaHlQWMQi6rRwWmyuNDrTtSQd77LZrY8FgRwdsy3VoKRrjhac6JEdgWopxO3Ngg3qvuQuvRoXwTSYSJPt4ipmNTm75jQgzPKbVjgLRLgJYld5FVaeTgMW4LbgNYRt0LZCkp874Z331SXTaL6CPZQQgW4RHb+Ho0wIDAQAB'
	        gatewayUrl: 'https://openapi.alipay.com/gateway.do'
	
	db: 'webgui.sqlite'


启动ssmgr

	screen -dmS ss-manager ss-manager -m $method -u --manager-address 127.0.0.1:6001
	或
	screen -dmS ss-manager ss-server --manager-address 127.0.0.1:6001 -c /etc/shadowsocks-libev/config.json -u

	screen -dmS ssmgr ssmgr -c ~/.ssmgr/default.yml
	screen -dmS webgui ssmgr -c ~/.ssmgr/webgui.yml

关闭screen打开的程序, `screen -S session_name -X quit`

	screen -S ss-manager -X quit
	screen -S ssmgr -X quit
	screen -S webgui -X quit


防火墙开放端口

	sudo ufw allow 80
	sudo ufw allow 6002
	sudo ufw allow 6001