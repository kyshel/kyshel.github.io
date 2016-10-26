---
layout: default
---

# Install PHP 5.6 by yum on CentOS7

**1.Install EPEL** 

    rpm -Uvh http://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-8.noarch.rpm

**2.Install REMI**  

    rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-7.rpm

**3.Enabling the Repo**   
We need to make sure the repo is enabled and select which version you want to install. We need to head over to /etc/yum.repos.d you should inside see a file called remi.repo.  
Open the file in your favourite editor (Nano, Pico, Vi etc), you’ll see a number of sections. We need to make sure that the first section [remi] is enabled:


    [remi]
    name=Les RPM de remi pour Enterprise Linux 6 - $basearch
    #baseurl=http://rpms.famillecollet.com/enterprise/6/remi/$basearch/
    mirrorlist=http://rpms.famillecollet.com/enterprise/6/remi/mirror
    enabled=1
    gpgcheck=1
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-remi


Note the line enabled=1 make sure this is set! Now technically you can actually go ahead and install PHP, but you will only get PHP 5.4.*. Which might be want to you want is so skip ahead to the next section!

If we want PHP 5.5 or PHP 5.6 we need to do a bit more work, further down in the repo.repo file you will see two additional sections [remi-php55] and [remi-php56], decide which PHP version you want to install and then enable the correct. So for PHP 5.6 we would change to:

    [remi-php56]
    name=Les RPM de remi de PHP 5.6 pour Enterprise Linux 6 - $basearch
    #baseurl=http://rpms.famillecollet.com/enterprise/6/php56/$basearch/
    mirrorlist=http://rpms.famillecollet.com/enterprise/6/php56/mirror
    # WARNING: If you enable this repository, you must also enable "remi"
    enabled=1
    gpgcheck=1
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-remi

Once you made your changes save your modified file and quit your editor.

**4.Installing PHP**

    yum install php

5.**Verify**

	php -v
