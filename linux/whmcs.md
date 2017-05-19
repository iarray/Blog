## WHMCS

### 一键安装LNMP

	wget -c http://soft.vpser.net/lnmp/lnmp1.3-full.tar.gz && tar zxf lnmp1.3-full.tar.gz && cd lnmp1.3-full && ./install.sh lnmp

或者下面的手动安装

### PHP7 安装

添加源

	sudo add-apt-repository ppa:ondrej/apache2
	sudo add-apt-repository ppa:ondrej/php
	sudo apt-get update

如果添加安装源报了字符编码的错误,请先安装下面的

	sudo apt-get install -y language-pack-en-base
	sudo LC_ALL=en_US.UTF-8 add-apt-repository ppa:ondrej/php


安装PHP7
	
	sudo apt-get install php7.0-cli php7.0-common  php7.0-curl php7.0-gd php7.0-mysql php7.0-curl php7.0-xml php7.0-memcache php7.0-memcached php7.0-intl php7.0-mongo php7.0-mongodb php7.0-mbstring

	sudo apt-get install libapache2-mod-php7.0
	sudo a2enmod php7.0

如果遇到Apache环境变量问题，可以执行
	
	source /etc/apache2/envvars
	

### 安装Mysql
 
	sudo apt-get install mysql-server mysql-client libmysqlclient-dev

启动
	
	sudo service mysql start


----------

