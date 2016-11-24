---
layout: post_ks
---

# Install Lavarel5 On CentOS7

**1.Requirement**

- PHP >= 5.6.4  
[Install PHP 5.6 by yum on CentOS7](http://kyshel.com/blog/install-php-5-6-by-yum-on-centos7/)
- Mbstring PHP Extension  
`yum install php-mbstring`
- Others  
`yum install php-xml`

**2.Install Composer**

    php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
    php -r "if (hash_file('SHA384', 'composer-setup.php') === 'e115a8dc7871f15d853148a7fbac7da27d6c0030b848d9b3dc09e2a0388afed865e6a3d6b3c0fad45c48e2b5fc1196ae') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
    php composer-setup.php
    php -r "unlink('composer-setup.php');"

**3.Global Composer**

    mv composer.phar /usr/local/bin/composer

**4.Download Laravel-Installer**

    composer global require "laravel/installer"

**5.Global Laravel-Installer**

    ln -s ~/.config/composer/vendor/bin/laravel /usr/local/bin/

**6.Create a fresh Laravel installation**

    laravel new blog

**7.Config Permission**

	cd blog
    chown apache:apache storage/ -R

**8.Open [localhost/blog/public](localhost/blog/public) , and enjoy~**

![Lavarel_install_ok](https://kyshel.github.io/file/blog/Lavarel_install_kyshel.png)


