# ICINGA2 old install
Created Wednesday 24 December 2014

<http://www.holediscover.com/install-icinga2-on-centos7/>

### Install GCC and development tools:
# yum -y groupinstall Development tools

### PHP dependencies
# yum -y install php-cli php-pear php-xmlrpc php-xsl php-pdo php-soap php-gd php-ldap

### Install icinga.repo
# rpm --import <http://packages.icinga.org/icinga.key>
# wget <http://packages.icinga.org/epel/ICINGA-release.repo> -O [/etc/yum.repos.d/ICINGA-release.repo](file:///etc/yum.repos.d/ICINGA-release.repo)

### Install icinga
# yum install  boost-regex boost-system boost-test boost-thread icinga2-common libboost_regex-mt  libboost_system-mt libboost_thread-mt  icinga2-bin icinga2

### Create icinga database
# mysql -u root -p
Type in below mysql commands:
CREATE DATABASE icinga;
GRANT SELECT, INSERT, UPDATE, DELETE, DROP, CREATE VIEW, INDEX, EXECUTE ON icinga.* TO 'icinga'@'localhost' IDENTIFIED BY 'icinga';
GRANT EXECUTE ON icinga.* TO 'icinga'@'localhost';
FLUSH PRIVILEGES;
QUIT

### Install icinga web
# yum -y install icinga-web icinga-web-mysql

### Create icinga web database
# mysql -u root -p
Type in below mysql commands:
CREATE DATABASE icinga_web;
GRANT SELECT, INSERT, UPDATE, DELETE, DROP, CREATE VIEW, INDEX, EXECUTE ON icinga_web.* TO 'icinga_web'@'localhost' IDENTIFIED BY 'icinga_web';
FLUSH PRIVILEGES;
QUIT

### Install DB-ido
# yum -y install icinga2-ido-mysql icinga-idoutils-libdbi-mysql
Import database schema for incinga and incinga-web
# mysql -u root -p icinga < /usr/share/icinga2-ido-mysql/schema/mysql.sql
# mysql -u root -p icinga_web < /usr/share/icinga-web/etc/schema/mysql.sql

### Install Nagios plugins is used for Icinga.
# yum -y install nagios-plugins-all

### Enable and start Icinga and IDO
# systemctl enable icinga2
# systemctl enable ido2db
# systemctl start icinga2
# systemctl start ido2db
# systemctl restart httpd

### Go to Icinga:
<http://192.168.200.4/icinga-web>
Default log in user is root, password is password.


