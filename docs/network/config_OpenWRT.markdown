# config OpenWRT
Created Thursday 17 July 2014

### Rename the router and set timezone for europe/brussels

``# uci set system.@system[0].hostname='4451-X Integrated Services Router``'

``# uci set system.@system[0].timezone='CET-1CEST,M3.5.0,M10.5.0/3``'

``# uci commit system``

``# /etc/init.d/system restart``

or 

``# vi /etc/config/system``

	option hostname '4451-X Integrated Services Router'
	option timezone 'CET-1CEST,M3.5.0,M10.5.0/3'


### Set firewall
Howto add firewall rule

This is a good example of both adding a firewall rule to forward the TCP SSH port, and of the negative (-1) syntax used with uci.

``# uci add firewall rule`` 

``# uci set firewall.@rule[-1].name=Allow-SSH-from-wan`` 

``# uci set firewall.@rule[-1].src=wan`` 

``# uci set firewall.@rule[-1].target=ACCEPT`` 

``# uci set firewall.@rule[-1].proto=tcp`` 

``# uci set firewall.@rule[-1].dest_port=22`` 

``# uci commit firewall`` 

``# /etc/init.d/firewall restart``

### Enable remote logging

``# uci set system.@system[0].log_ip=192.168.225.3``

``# uci set system.@system[0].conloglevel=7``

``# uci commit system``

``# /etc/init.d/log restart``

#### Enable firewall logging (-j LOG)

In recent Openwrt builds this is as simple as editing /etc/config/firewall and adding a line to each zone that you want to get logged

	config 'zone'
	option 'name' 'wan'
	...
		option 'log' '1'


