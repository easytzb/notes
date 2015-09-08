用于查看和配置网络设备

###命令格式
	ifconfig [网络设备] [参数]

###命令
	ifconfig eth0 up
	ifconfig eth0 down
	ifconfig eth0 192.168.120.56	
	ifconfig eth0 192.168.120.56 netmask 255.255.255.0
	ifconfig eth0 192.168.120.56 netmask 255.255.255.0 broadcast 192.168.120.255
	ifconfig eth0 arp		#开启arp协议
	ifconfig eth0 -arp		#关闭arp协议
	ifconfig eth0 mtu 1500	#最大传输单元 能通过的最大数据包大小为 1500 bytes
	
***用ifconfig命令配置的网卡信息，在网卡重启后机器重启后，配置就不存在。要想将上述的配置信息永远的存的电脑里，那就要修改网卡的配置文件了***
