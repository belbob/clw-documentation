# GRAPHITE
Created Monday 19 January 2015


* <http://graphite.readthedocs.org/en/latest/install.html>
* <http://centoshowtos.org/monitoring/graphite/>
* <http://www.ozmonet.com/projects/graphite.html>


### Install epel
``# yum -y install epel-release``

### Install graphite
``# yum -y install graphite-web graphite-web-selinux mysql mysql-server MySQL-python``
 

### Create graphite database
Type in below mysql commands:

	CREATE DATABASE graphite;
	GRANT ALL PRIVILEGES ON graphite.* TO 'graphite'@'localhost' IDENTIFIED BY 'password';
	FLUSH PRIVILEGES;
	QUIT


### Configure Graphite
``vi /etc/graphite-web/local_settings.py``
	
	Change the database configuration to look like this:
	DATABASES = {
	'default': {
	'NAME': 'graphite',
	'ENGINE': 'django.db.backends.mysql',
	'USER': 'graphite',
	'PASSWORD': 'password',
	}
	}



Run the manage script for graphite to create the MySQL tables

``/usr/lib/python2.6/site-packages/graphite/manage.py syncdb``

It will prompt you to create a superuser, you can go ahead and do so.

Start apache and then you can browse to it

``/etc/init.d/httpd start``

Browse to <http://IP> address or hostname

