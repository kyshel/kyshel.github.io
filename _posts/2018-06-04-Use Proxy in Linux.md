---
tags: [proxy]
---



# 0x01 Make it work
1. `wget https://bootstrap.pypa.io/get-pip.py && python get-pip.py`
1. `pip install shadowsocks`
1. `sslocal -s your-ss-ip -p 443 -k "your-ss-code" -l 1080 -m aes-256-cfb --user nobody -d start`
1. `curl --socks5 127.0.0.1:1080 ip.cn` 
1. `yum -y install privoxy`
1. `service privoxy start`
1. `cp /etc/privoxy/config /etc/privoxy/config_bak`
1. `vim /etc/privoxy/config`ensure below lines are not commented, line1 usually not, line2 need modify
	```
	listen-address 127.0.0.1:8118 
	forward-socks5t / 127.0.0.1:1080 . 
	```
1. run below in bash
	```
	export http_proxy=http://127.0.0.1:8118
	export https_proxy=http://127.0.0.1:8118
	```
1. `chkconfig privoxy on && service privoxy start`
1. `curl ipinfo.io` 

# 0x02 Be elegant
## 2.1 CentOS version
1. `pip install shadowsocks`
1. `yum -y install privoxy`
1. `cp /etc/privoxy/config /etc/privoxy/config_bak`
1. `vim /etc/privoxy/config`ensure below lines are not commented, line1 usually not, line2 need modify
	```
	listen-address 127.0.0.1:8118 
	forward-socks5t / 127.0.0.1:1080 . 
	```

1. save below code to a file like `ssclient`    
	``` sh
	#!/bin/bash
	if [ "$1" == "back" ]; then
	        sslocal -s your-ss-ip -p 443 -k "your-ss-code" -l 1080 -m aes-256-cfb --user nobody -d start
	elif [ "$1" == "stop" ]; then
	        sslocal -s your-ss-ip -d stop
	elif [ "$1" == "log" ]; then
	        tail -f /var/log/shadowsocks.log
	else
	        sslocal -s your-ss-ip -p 443 -k "your-ss-code" -l 1080 -m aes-256-cfb
	fi
	```

1. save below code to a file like `setproxy`, then `. setproxy on` means open,`. setproxy off` means shutdown
	``` sh
	#!/bin/bash
	if [ "$1" == "on" ]; then
	        service privoxy start
	        export http_proxy=http://127.0.0.1:8118
	        export https_proxy=http://127.0.0.1:8118
	        # /root/bin/ssclient back  # this line start socks client
	elif [ "$1" == "off" ]; then
	        export http_proxy=""
	        export https_proxy=""
	        service privoxy stop
	elif [ "$1" == "status" ]; then
	        service privoxy status
	else
	        echo 'Please input argument: on,off,status'
	fi
	echo 'Now your ip is: '
	curl ipinfo.io
	echo ''
	```


## 2.2 Ubuntu Version
1. install
	```
	sudo apt install python-pip git privoxy -y
	pip install -U git+https://github.com/shadowsocks/shadowsocks.git@master
	cp /etc/privoxy/config /etc/privoxy/config_bak
	```
	
1. `vim /etc/privoxy/config` uncomment below line   
	```
	forward-socks5t / 127.0.0.1:9050 . 
	```

1. save below code to a file like `setproxy` , then `. setproxy on` means open,`. setproxy off` means shutdown   
	``` sh
	#!/bin/bash

	ip=1.1.1.1
	port=11111
	key=123456

	if [ "$1" == "on" ]; then
		sslocal -s $ip -p $port -k $key -l 9050 -m aes-256-cfb --user nobody -d start
		service privoxy start
		export http_proxy=http://127.0.0.1:8118
		export https_proxy=http://127.0.0.1:8118
	elif [ "$1" == "off" ]; then
		sslocal -s $ip -d stop
		service privoxy stop
		export http_proxy=""
		export https_proxy=""
	elif [ "$1" == "status" ]; then
		service privoxy status
	elif [ "$1" == "log" ]; then
		tail -f /var/log/shadowsocks.log
	elif [ "$1" == "test" ]; then
		sslocal -s $ip -p $port -k $key -l 9050 -m aes-256-cfb

	else
		echo 'Please input argument: on,off,status'
	fi
	echo 'Now your ip is: '
	curl ipinfo.io
	echo ''
	```








# 0x03 Another host comes
An image that the stream flow:  
- Listen: ss-server[socks 1080] --> privoxy(forward-socks5t[socks 1080]>listen-address[http,https 8118])
- Request: application, like curl --> localhost[http,https 8118]  

Now comes another machine: C (A is ss-server, B is ss-client configured as 0x02), let C join the play:

1. on B, change `listen-address 127.0.0.1:8118` to `listen-address 0.0.0.0:8118` in `/etc/privoxy/config`
2. on C, run `export http_proxy=http://127.0.0.1:8118; export https_proxy=http://127.0.0.1:8118`, or make a `proxy_switch.sh`:
	``` sh
	#!/bin/bash
	if [ "$1" == "on" ]; then
	        export http_proxy=http://B_ip:8118
	        export https_proxy=http://B_ip:8118
		echo 'http and https proxy has set to B_ip:8118'
	        curl ipinfo.io
	elif [ "$1" == "off" ]; then
	        export http_proxy=""
	        export https_proxy=""
		echo 'http and https proxy OFF'
	else
		echo 'http_proxy:'
		echo $http_proxy
		echo 'https_proxy'
		echo $https_proxy
	        echo 'Please input argument: on,off'
	fi
	
	```
	
3. run `. proxy_swtich.sh on` to enjoy C ~





