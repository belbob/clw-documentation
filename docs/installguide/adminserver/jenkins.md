# JENKINS
Created Monday 19 January 2015

### Install Jenkins

``# wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat/jenkins.repo``

``# rpm --import http://pkg.jenkins-ci.org/redhat/jenkins-ci.org.key``

``# yum install jenkins java-1.7.0-openjdk``

### Start Jenkins

``# service jenkins start``

``# systemctl restart jenkins.service``

``# chkconfig jenkins on``

### set firewall

``# firewall-cmd --zone=public --add-port=8080/tcp --permanent``

``# firewall-cmd --reload``

goto: <http://admin.donboscowilrijk.be:8080>
