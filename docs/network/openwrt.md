# config OpenWRT
Created Thursday 17 July 2014

### Install OpenWRT (on TP-Link WDR4300)

#### Download OpenWRT
goto <http://wiki.openwrt.org/toh/start> and select <http://wiki.openwrt.org/toh/tp-link/tl-wdr4300> and download: <http://downloads.openwrt.org/attitude_adjustment/12.09/ar71xx/generic/openwrt-ar71xx-generic-tl-wdr4300-v1-squashfs-factory.bin>

#### Install OpenWRT
Poweron router, connect lan to PC, enable wired connection and look for Default route -> 192.168.0.1.

Point your browser to 192.168.0.1 and login with admin, pw -> admin (see <http://www.routerpasswords.com/> )

After login:
goto System Tools -> Firmware Upgrade -> browse openwrt-ar-xxx-xx.bin and upgrade. After upgrade reboot router (poweroff -> poweron)

#### First login

``# telnet 192.168.1.1``

type passwd into the prompt. You will be prompted to set a new password for the user root. After you set a password the telnet daemon will be disabled, type exit into the prompt. Login again with 

``# ssh root@192.168.1.1``


### Rename the router and set timezone for europe/brussels

``# uci set system.@system[0].hostname='4451-X Integrated Services Router``'

``# uci set system.@system[0].timezone='CET-1CEST,M3.5.0,M10.5.0/3``'

``# uci commit system``

``# /etc/init.d/system restart``

or 

``# vi /etc/config/system``

	option hostname '4451-X Integrated Services Router'
	option timezone 'CET-1CEST,M3.5.0,M10.5.0/3'


### Set network

``# uci set network.lan.ipaddr=192.168.244.254``

``# uci set network.lan.dns=192.168.225.12``

``# uci set network.wan.proto=static``

``# uci set network.wan.ipaddr=192.168.223.244``

``# uci set network.wan.gateway=192.168.223.254``

``# uci set network.wan.dns=192.168.225.12``

``# uci commit network``

``# /etc/init.d/network restart``

#### use system DNS

``# echo "nameserver 192.168.225.12">/etc/resolv.conf``

``# echo "dhcp-option=6,192.168.225.12">>/etc/dnsmasq.conf``

``# reboot``

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


### Wireless

#### edit wireless config

``# uci set wireless.@wifi-iface[0].ssid=clw_pclabo``

``# uci set wireless.@wifi-iface[0].encryption=psk2``

``# uci set wireless.@wifi-iface[0].key=password``

``# uci set wireless.radio0.disabled=0``

``# uci commit wireless``

``# wifi``

show running configuration with:

``# uci show wireless``

#### Start/Stop Wireless

To (re)start the wireless after a configuration change, use: 

``# wifi``

to disable the wireless, run: 

``# wifi down``

##### multiple wireless devices
start the interface wlan2 issue: 

``# wifi up wlan2``

to stop that interface: 

``# wifi down wlan2``

If the platform has also e.g. wlan0 and wlan1 these will not be touched by stopping or starting wlan2 selectively.

#### Regenerate Configuration

To rebuild the configuration file, e.g. after installing a new wireless driver, remove the existing wireless configuration (if any) and use the wifi detect command with stdout redirected to the /etc/config/wireless file:

	# rm -f /etc/config/wireless 
	# wifi detect > /etc/config/wireless


wifi detect gives UCI configuration entries for all installed interfaces that do not have UCI entries in /etc/config/wireless. So you can remove /etc/config/wireless and run the above again to reset your wifi configuration.

### upgrade

``# cd /tmp``

``# wget http://downloads.openwrt.org/snapshots/trunk/ar71xx/openwrt-ar71xx-generic-tl-wdr4300-v1-squashfs-sysupgrade.bin``

``# sysupgrade -v /tmp/openwrt-ar71xx-generic-tl-wdr4300-v1-squashfs-sysupgrade.bin``

``# opkg update``

### tweaks
Add the following lines before the exit 0 statement in [/etc/rc.local:](file:///etc/rc.local%3A)

	cat << EOT > /etc/rc.local
	# Put your custom commands here that should be executed once
	# the system init finished. By default this file does nothing.
	ifconfig eth0 txqueuelen 10500
	ifconfig wlan0 txqueuelen 7935 mtu 1328
	ifconfig wlan1 txqueuelen 7935 mtu 1328
	exit 0
	EOT


and add to [/etc/sysctl.conf:](file:///etc/sysctl.conf%3A)

	cat << EOT >> /etc/sysctl.conf
	
	# tweaks
	vm.min_free_kbytes=4096
	net.core.rmem_max = 16777216
	net.core.wmem_max = 16777216
	net.ipv4.tcp_rmem = 4096 87380 16777216
	net.ipv4.tcp_wmem = 4096 65536 16777216
	net.core.netdev_max_backlog = 30000
	EOT


### Zabbix client

#### install zabbix_agentd

``# opkg update``

``# opkg install zabbix-agent zabbix-agentd zabbix-extra-mac80211 zabbix-extra-network zabbix-extra-wifi libiwinfo-lua libuci-lua``

#### config zabbix_agentd.conf

``# sed -i 's/Server=127.0.0.1/Server=192.168.225.3/' /etc/zabbix_agentd.conf``

#### start zabbix_agentd

``# /etc/init.d/zabbix_agentd enable``

``# /etc/init.d/zabbix_agentd restart``

#### add firewall rule for zabbix_agentd

	cat << EOT >> /etc/config/firewall
	config rule                                     
	option target 'ACCEPT'                  
	option src 'wan'                        
	option proto 'tcp udp'                  
	option dest_port '10050'                
	option name 'zabbixagent'               
	option src_ip '192.168.225.3' 
	EOT


``# /etc/init.d/firewall restart``

