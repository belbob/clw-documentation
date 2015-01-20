# zabbix-server
Created Tuesday 23 December 2014


### Server Install

**Configure the ZabbixZone package repository and GPG key**

``# rpm --import http://repo.zabbix.com/RPM-GPG-KEY-ZABBIX``

``# rpm -ihv http://repo.zabbix.com/zabbix/2.4/rhel/7/x86_64/zabbix-release-2.4-1.el7.noarch.rpm``

``# yum -y install zabbix-server-mysql zabbix-web-mysql zabbix-agent``

### Creating initial database

#### Create zabbix database and user on MySQL

``# mysql -u root -p``

	MariaDB [(none)]> create database zabbix character set utf8 collate utf8_bin;
	MariaDB [(none)]> grant all privileges on zabbix.* to zabbix@localhost identified by 'password';
	MariaDB [(none)]> exit


#### Import initial schema and data

``# cd /usr/share/doc/zabbix-server-mysql-2.4.3/create/``

``# mysql -u root -p zabbix < schema.sql``

``# mysql -u root -p zabbix < images.sql``

``# mysql -u root -p zabbix < data.sql``

#### Edit database configuration

``# vi /etc/zabbix/zabbix_server.conf``

	DBHost=localhost
	DBName=zabbix
	DBUser=zabbix
	DBPassword=password


### Editing PHP configuration for Zabbix frontend

``# vi /etc/httpd/conf.d/zabbix.conf``

	php_value max_execution_time 300
	php_value memory_limit 128M
	php_value post_max_size 16M
	php_value upload_max_filesize 2M
	php_value max_input_time 300
	php_value date.timezone Europe/Brussels


#### restart apache

``# systemctl restart httpd.service``

### firewalld

``# firewall-cmd --zone=public --add-port=10051/tcp --permanent``

``# firewall-cmd --zone=public --add-port=10051/udp --permanent``


### Start the zabbix-server process

``# systemctl enable zabbix-server.service``

``# systemctl start zabbix-server.service``

goto: <http://admin.donboscowilrijk.be/zabbix> 
	login: Admin
	password: zabbix


