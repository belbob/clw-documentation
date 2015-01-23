# ocsinventory
Created Tuesday 23 December 2014

### Install epel

``# yum -y install epel-release``

### Install OCS-inventory

``# yum -y install ocsinventory*``

To be able to make SNMP using OCS Unified Unix agent:

``# yum -y install perl-Net-SNMP``

### config php

``# sed -i 's/;date.timezone =/&\ndate.timezone = Europe\/Brussels/' /etc/php.ini``

``# sed -i 's/post_max_size = 8M/post_max_size = 64M/' /etc/php.ini``

``# sed -i 's/upload_max_filesize = 2M/upload_max_filesize = 64M/' /etc/php.ini``


### start apache

``# systemctl enable httpd.service``

``# systemctl start httpd.service``

### Create database

``# systemctl enable mariadb.service``

``# systemctl start mariadb.service``

``# mysql_secure_installation``

### Thats All
goto <http://admin.donboscowilrijk.be/ocsreports> and create with root/passwd and localhost

The default user is "**admin**" and password is "**admin**".

Replace the admin password Immediately!

### after install set security
remove install.php

``# rm /usr/share/ocsinventory-reports/ocsreports/install.php`` 

Change default SQL login/password is activate on your database: ocsweb

``# mysql -u root -p``

	MariaDB [(none)]> UPDATE mysql.user SET Password=PASSWORD('password') WHERE User='ocs';
	MariaDB [(none)]> FLUSH PRIVILEGES;
	MariaDB [(none)]> quit;


edit database password for ocs in webinterface

``# sed -i 's/define("PSWD_BASE","ocs");/define("PSWD_BASE","password");/' /etc/ocsinventory/ocsinventory-reports/dbconfig.inc.php``

``# sed -i 's/PerlSetVar OCS_DB_PWD ocs/PerlSetVar OCS_DB_PWD password/' /etc/httpd/conf.d/ocsinventory-server.conf``

### restart apache
``# systemctl restart httpd.service``

### config ocsinventory-agent
replace ocsinventory-agent.cfg (only for localhost)

	# cat << EOT > /etc/ocsinventory/ocsinventory-agent.cfg 
	server = http://localhost/ocsinventory
	logger = Stderr
	logfile = /var/log/ocsinventory-agent/ocsinventory-agent.log
	EOT


#### and run

``ocsinventory-agent``


### Client install ocsinventory-agent

``# yum -y install ocsinventory-agent``

#### Config ocsinventory-agent

	# cat << EOT > /etc/ocsinventory/ocsinventory-agent.cfg
	server = http://192.168.225.3/ocsinventory
	logger = Stderr
	logfile = /var/log/ocsinventory-agent/ocsinventory-agent.log
	EOT


and 

``# sed -i 's/none/cron/' /etc/sysconfig/ocsinventory-agent``

#### run

``ocsinventory-agent``

