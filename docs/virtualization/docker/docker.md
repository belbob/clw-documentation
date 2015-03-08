# docker
Created Sunday 19 October 2014

## Using docker

### install docker

``# yum install docker``

### start docker

``# service docker start``

``# chkconfig docker on``

To start the docker service use systemctl:

``$ sudo systemctl start docker``

``$ sudo systemctl enable docker``

### search images

``$ docker search``

### download images

``$ docker pull centos``

or only latest

``$ docker pull centos:latest``

or only centos6

``$ docker pull centos6``

### remove all images and containers

Delete all containers

``$ docker rm $(docker ps -a -q)``

Delete all images

``$ docker rmi $(docker images -q)``

## Docker create images

### Dockerfile webserver example

	FROM centos:latest
	MAINTAINER belbob <robert.keersse@gmail.com>
	RUN yum update -y
	RUN yum install -y httpd php
	RUN echo "<?php phpinfo(); ?>" > /var/www/html/info.php
	EXPOSE 80
	ENTRYPOINT ["/usr/sbin/httpd"]
	CMD ["-D", "FOREGROUND"]

### Build docker images

``$ docker build -t="robert/centos:http" .``

(don't forget . dot)

### Run container

``$ docker run -d -p 8080:80 --name=web_server robert/centos:http``

``$ docker ps -a``

	CONTAINER ID        IMAGE                COMMAND                CREATED             STATUS              PORTS                  NAMES
	a376e05670fd        robert/centos:http   "/usr/sbin/httpd -D    5 minutes ago       Up 5 minutes        0.0.0.0:8080->80/tcp   web_server



