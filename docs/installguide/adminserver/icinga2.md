# ICINGA2
Created Sunday 19 April 2015

<http://docs.icinga.org/icinga2/latest/doc/module/icinga2/chapter/getting-started#getting-started>

Setting up Icinga 2
-------------------

### Install repo

``# rpm --import http://packages.icinga.org/icinga.key``

``# curl -o /etc/yum.repos.d/ICINGA-release.repo http://packages.icinga.org/epel/ICINGA-release.repo``

``# yum makecache``

### Install Icinga2

``# yum install icinga2``

``# systemctl enable icinga2``

``# systemctl start icinga2``

#### Enabled Features during Installation
The default installation will enable three features required for a basic Icinga 2 installation:


* checker for executing checks
* notification for sending notifications
* mainlog for writing the icinga2.log file


You can verify that by calling icinga2 feature list CLI command to see which features are enabled and disabled.

	# icinga2 feature list
	Disabled features: api command compatlog debuglog graphite icingastatus ido-mysql ido-pgsql livestatus notification perfdata statusdata syslog
	Enabled features: checker mainlog notification


#### Setting up Check Plugins
Without plugins Icinga 2 does not know how to check external services. The Monitoring Plugins Project provides an extensive set of plugins which can be used with Icinga 2 to check whether services are working properly.

``# yum install epel-release``

``# yum install nagios-plugins-all``

### Running Icinga 2
Icinga2 packages automatically install the necessary systemd unit files

``# systemctl enable icinga2``

``# systemctl restart icinga2``

Setting up Icinga Web 2
-----------------------

### Installing MySQL database server

``# yum install mariadb-server mariadb ``

``# systemctl enable mariadb``

``# systemctl start mariadb``

``# mysql_secure_installation``

#### Installing the IDO modules for MySQL

``# yum install icinga2-ido-mysql php-mysql php-ldap``

#### Setting up the MySQL database

Set up a MySQL database for Icinga 2:

``# mysql -u root -p``

	mysql>  CREATE DATABASE icinga;
			GRANT SELECT, INSERT, UPDATE, DELETE, DROP, CREATE VIEW, INDEX, EXECUTE ON icinga.* TO 'icinga'@'localhost' IDENTIFIED BY 'icinga';


``# mysql -u root -p icinga < /usr/share/icinga2-ido-mysql/schema/mysql.sql``

### Enabling the IDO MySQL module

``# icinga2 feature enable ido-mysql``

``# systemctl restart icinga2``

### Setting up the webserver

``# sed -i 's/;date.timezone =/&\ndate.timezone = Europe\/Brussels/' /etc/php.ini``

``# yum install httpd``

``# systemctl enable httpd``

``# systemctl start httpd``

#### Firewall Rules

``# firewall-cmd --add-service=http``

``# firewall-cmd --permanent --add-service=http``

### Setting Up External Command Pipe
Web interfaces and other Icinga addons are able to send commands to Icinga 2 through the external command pipe.

``# icinga2 feature enable command``

``# systemctl restart icinga2``

By default the command pipe file is owned by the group icingacmd with read/write permissions. Add your webserver's user to the group icingacmd to enable sending commands to Icinga 2 through your web interface:

``# usermod -a -G icingacmd ``*www-data*

``# usermod -a -G icingacmd apache``

The webserver's user is different between distributions so you might have to change www-data to wwwrun, www, or apache. Change "www-data" to the user you're using to run queries.

### Installing Icinga Web 2

``# yum install icingaweb2``

``# systemctl restart httpd``

#### Preparing Web Setup
You can set up Icinga Web 2 quickly and easily with the Icinga Web 2 setup wizard which is available the first time you visit Icinga Web 2 in your browser. When using the web setup you are required to authenticate using a token. In order to generate a token use:

``# yum install icingacli``

``# icingacli setup token create``

In case you do not remember the token you can show it using

``# icingacli setup token show``

Finally visit Icinga Web 2 in your browser to access the setup wizard and complete the installation: /icingaweb2/setup

attention: resource icinga_ido database = icinga !!!!



