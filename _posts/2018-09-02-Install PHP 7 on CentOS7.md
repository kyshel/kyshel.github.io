	yum install epel-release
	yum install remi-release
	yum install yum-utils
	yum-config-manager --enable remi-php70
	yum-config-manager --enable remi-php71
	yum-config-manager --enable remi-php72
	yum install php php-mcrypt php-cli php-gd php-curl php-mysql php-ldap php-zip php-fileinfo 
	php -v
