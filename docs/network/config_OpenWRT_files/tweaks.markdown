# tweaks
Created Thursday 24 July 2014

Tweaks (<http://blog.philippklaus.de/2012/12/openwrt-on-a-tp-link-tl-wdr4300-router/>)

Add the following lines before the exit 0 statement in /etc/rc.local:

ifconfig eth0 txqueuelen 10500
ifconfig wlan0 txqueuelen 7935 mtu 1328
ifconfig wlan1 txqueuelen 7935 mtu 1328

and change /etc/sysctl.conf:

vm.min_free_kbytes=4096
net.core.rmem_max = 16777216
net.core.wmem_max = 16777216
net.ipv4.tcp_rmem = 4096 87380 16777216
net.ipv4.tcp_wmem = 4096 65536 16777216
net.core.netdev_max_backlog = 30000


