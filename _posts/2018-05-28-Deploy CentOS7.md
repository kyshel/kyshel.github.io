# Auto with .sh
``` sh
wget http://nixni.cc/chip/sh/deploy.sh
chmod u+x deploy.sh
. deploy.sh
```

# Manual
## basic tools
``` sh
yum -y install net-tools zip unzip vim elinks tree wget git curl
```

## ssh key login
``` sh
mkdir ~/.ssh ; vim ~/.ssh/authorized_keys
```

## date sync
``` sh
yum install ntp -y
chkconfig ntpd on
ntpdate time.apple.com
```

## Timezone
``` sh
timedatectl set-timezone Asia/Shanghai
```

## epel
``` sh
yum install -y epel
```

## Docker
``` sh
yum install -y yum-utils 
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
yum makecache fast
yum -y install docker-ce
systemctl start docker
docker run hello-world
```

## LAMP
``` sh
yum install -y httpd mysql mariadb-server php php-mysql
chkconfig httpd on && chkconfig mariadb on
service httpd start && service mariadb start
firewall-cmd --permanent --zone=public --add-service=http && firewall-cmd --reload
```

## Samba
	yum install samba

/etc/samba/smb.conf

	        security = user
	        passdb backend = tdbsam
	        # map to guest = bad user
	[html]
	        path = /var/www/html
	        browsable =yes
	        writable = yes
	        guest ok = yes
	        read only = no
	        force user = root
	        force group = root
		
---
	chkconfig smb on
	service smb start
	smbpasswd -a root


	firewall-cmd --permanent --zone=public --add-service=samba && firewall-cmd --reload


## LAMP + Samba config
	mkdir ~/config_files && cd config_files && ln -s /etc/httpd . && ln -s /etc/httpd/conf/httpd.conf . && ln -s /etc/samba/smb.conf .

## Selinux Apache
	chcon -R -t httpd_sys_content_t */
	
## sqlalchemy
``` sh
yum -y install phpmyadmin python36 python36-devel mysql-devel gcc
pip3 install mysqlclient
```
	
## Install All

``` sh
yum -y install net-tools zip unzip vim elinks tree wget git curl \
httpd mysql mariadb-server php php-mysql samba \
epel-release ntp nss libcurl
yum -y install phpmyadmin python36 \


```
