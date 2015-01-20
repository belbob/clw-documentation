# elasticsearch
Created Tuesday 23 December 2014

### Download Public Signing Key

``# rpm --import https://packages.elasticsearch.org/GPG-KEY-elasticsearch``

### Add elasticsearch.repo

	# cat << EOT  > /etc/yum.repos.d/elasticsearch.repo
	[elasticsearch-1.4]
	name=Elasticsearch repository for 1.4.x packages
	baseurl=http://packages.elasticsearch.org/elasticsearch/1.4/centos
	gpgcheck=1
	gpgkey=http://packages.elasticsearch.org/GPG-KEY-elasticsearch
	enabled=1
	EOT


### Install elasticsearch

``# yum -y install elasticsearch``
	
``# systemctl daemon-reload``
	
``# systemctl enable elasticsearch.service``


### Configure elasticsearch

``# vi /etc/elasticsearch/elasticsearch.yml``

edit:

	node.name: admin2.belbob.thuis

add:

	http.cors.allow-origin: "/.*/"
	http.cors.enabled: true


### Restart elasticsearch

``# systemctl restart elasticsearch.service``

``# systemctl status elasticsearch.service``

### test elasticsearch connection

``# curl -X GET http://admin.donboscowilrijk.be:9200/``

result must like;

	{
	  "status" : 200,
	  "name" : "admin.donboscowilrijk.be",
	  "cluster_name" : "elasticsearch",
	  "version" : {
	"number" : "1.4.2",
	"build_hash" : "927caff6f05403e936c20bf4529f144f0c89fd8c",
	"build_timestamp" : "2014-12-16T14:11:12Z",
	"build_snapshot" : false,
	"lucene_version" : "4.10.2"
	  },
	  "tagline" : "You Know, for Search"
	}

# kibana

### Install kibana

``# yum -y install wget``

``# wget https://download.elasticsearch.org/kibana/kibana/kibana-3.1.2.tar.gz``

``# tar -zxvf kibana-3.1.2.tar.gz``

``# mkdir /var/www/html/kibana``

``# cp -R kibana-3.1.2/* /var/www/html/kibana/``

``# chown -R apache:root /var/www/html/kibana`` 

### Config kibana

``# sed -i 's/http:\/\/"+window.location.hostname+":9200/http:\/\/admin.donboscowilrijk.be:9200/' /var/www/html/kibana/config.js``

### Restart apache

``# systemctl restart httpd.service``

# rsyslog

Created Tuesday 23 December 2014

### Install rsyslog-elasticsearch
``# yum -y install rsyslog-elasticsearch``
``# systemctl restart rsyslog.service``

### Config rsyslog
``# mv /etc/rsyslog.conf /etc/rsyslog.conf.org``

#### Create new rsyslog.conf
``# vi /etc/rsyslog.conf``

	# rsyslog configuration file
	# For more information see /usr/share/doc/rsyslog-*/rsyslog_conf.html
	# If you experience problems, see http://www.rsyslog.com/doc/troubleshoot.html
	
	#### MODULES ####
	# The imjournal module bellow is now used as a message source instead of imuxsock.
	$ModLoad imuxsock # provides support for local system logging (e.g. via logger command)
	$ModLoad imjournal # provides access to the systemd journal
	#$ModLoad imklog # reads kernel messages (the same are read from journald)
	#$ModLoad immark  # provides --MARK-- message capability
	$ModLoad omelasticsearch
	# Provides UDP syslog reception
	$ModLoad imudp
	$UDPServerRun 514
	# Provides TCP syslog reception
	$ModLoad imtcp
	$InputTCPServerRun 514
	
	#### TEMPLATES ####
	# server Rsyslog template
	$template TmplAuth, "/var/log/%HOSTNAME%/%PROGRAMNAME%.log" 
	authpriv.*   ?TmplAuth
	*.info,mail.none,authpriv.none,cron.none   ?TmplMsg
	# this is for index names to be like: logstash-YYYY.MM.DD
	template(name="logstash-index"
	  type="list") {
	constant(value="logstash-")
	property(name="timereported" dateFormat="rfc3339" position.from="1" position.to="4")
	constant(value=".")
	property(name="timereported" dateFormat="rfc3339" position.from="6" position.to="7")
	constant(value=".")
	property(name="timereported" dateFormat="rfc3339" position.from="9" position.to="10")
	}
	
	# this is for formatting our syslog in JSON with @timestamp
	template(name="plain-syslog"
	  type="list") {
	constant(value="{")
	  constant(value="\"@timestamp\":\"")     property(name="timereported" dateFormat="rfc3339")
	  constant(value="\",\"host\":\"")        property(name="hostname")
	  constant(value="\",\"severity\":\"")    property(name="syslogseverity-text")
	  constant(value="\",\"facility\":\"")    property(name="syslogfacility-text")
	  constant(value="\",\"tag\":\"")   property(name="syslogtag" format="json")
	  constant(value="\",\"message\":\"")    property(name="msg" format="json")
	constant(value="\"}")
	}
	# this is where we actually send the logs to Elasticsearch (localhost:9200 by default)
	action(type="omelasticsearch"
	template="plain-syslog"
	searchIndex="logstash-index"
	dynSearchIndex="on")


### restart rsyslog
``# systemctl restart rsyslog.service``
