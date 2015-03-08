# webserver2
Created Saturday 20 December 2014

	FROM centos:latest
	MAINTAINER belbob <robert.keersse@gmail.com>
	RUN yum update -y
	RUN yum install -y httpd php
	RUN echo "<?php phpinfo(); ?>" > /var/www/html/info.php
	EXPOSE 80
	ENTRYPOINT ["/usr/sbin/httpd"]
	CMD ["-D", "FOREGROUND"]
