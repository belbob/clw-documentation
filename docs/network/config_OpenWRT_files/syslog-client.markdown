# syslog-client
Created Saturday 21 June 2014


# enable remote logging
uci set [system.@system[0].log_ip=192.168.225.3](mailto:system.@system[0].log_ip=192.168.225.3)
uci set [system.@system[0].conloglevel](mailto:system.@system[0].conloglevel)=7
uci commit system
/etc/init.d/log restart


Enable firewall logging (-j LOG)

In recent Openwrt builds this is as simple as editing /etc/config/firewall and adding a line to each zone that you want to get logged

config 'zone'
option 'name' 'wan'
...
option 'log' '1'

