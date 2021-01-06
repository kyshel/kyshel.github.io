zabbix是一个监控平台，这里记录下deploy的过程。

# zabbix架构
zabbix类似服务端+客户端的结构，server作为服务端，汇总监控数据并展示，agent作为客户端，收集被监控主机的信息，并送给server。但是这里有点不同的是，server和agent通信有两种方式，active和passive，这个主动被动的角度是从agent端来看的。大多数通信都是active，即agent主动向server端发起请求，但有时候server可能想立刻获得被监控主机的信息，这时候就是server向agent发起请求了，这就是passive方式。


# server部署方式
打开官网界面，服务端部署方式很多，包括from packages、云镜像、容器镜像、现成的appliance、源码部署等。但它支持的os是主流Linux，包括rhel、centos、oracle linux、ubuntu、debian、suse、raspbian，暂不支持winserver系统。

部署方式比较如下：  
- from packages部署适用于prodcution环境，依赖网络
- 云镜像需要有aws云服务器，
- 容器镜像是个好选择，后续会通过容器做出离线部署的方式，
- appliance依赖较高的vm版本，
- 源码部署需要安装编译环境并且时间较长。

此处以from packages部署为例，简单写下部署过程。
1. 首选选择平台，笔者选的是5.0 LTS的zabbix，os是CentO S7，数据库选的postgresql，web服务器选的nginx
    ```
    yum install postgres postgresql-server 
    sudo postgresql-setup initdb
    sudo systemctl start postgresql
    sudo passwd postgres
    ```
2. bash内配置
    安装zabbix仓库

    ```
    rpm -Uvh https://repo.zabbix.com/zabbix/5.0/rhel/7/x86_64/zabbix-release-5.0-1.el7.noarch.rpm
    yum clean all

    yum install zabbix-server-pgsql zabbix-agent
    yum install centos-release-scl
    ```

    Edit file /etc/yum.repos.d/zabbix.repo and enable zabbix-frontend repository.

    ```
    [zabbix-frontend]
    ...
    enabled=1
    ...
    ```

    继续

    ```
    yum install zabbix-web-pgsql-scl zabbix-nginx-conf-scl
    sudo -u postgres createuser --pwprompt zabbix
    sudo -u postgres createdb -O zabbix zabbix
    zcat /usr/share/doc/zabbix-server-pgsql*/create.sql.gz | sudo -u zabbix psql zabbix

    ```

    Edit file /etc/zabbix/zabbix_server.conf  `DBPassword=password`

    Edit file /etc/opt/rh/rh-nginx116/nginx/conf.d/zabbix.conf, uncomment and set 'listen' and 'server_name' directives. **server_name没有域名的话要换成ip**

    ```
    # listen 80;
    # server_name example.com;

    ```

    Edit file /etc/opt/rh/rh-php72/php-fpm.d/zabbix.conf, add nginx to listen.acl_users directive.  ` listen.acl_users = apache,nginx`

    to check the timezone `timedatectl`   
    uncomment and set the right timezone  ` ; php_value[date.timezone] = Europe/Riga`


    编辑pg_hba.conf，将这两行的METHOD设为md5   `vi /var/lib/pgsql/data/pg_hba.conf`
    ```
    # "local" is for Unix domain socket connections only
    local   all             all                                     md5
    # IPv4 local connections:
    host    all             all             127.0.0.1/32            md5
    ```



    Start Zabbix server and agent processes and make it start at system boot.

    ```
    systemctl restart zabbix-server zabbix-agent rh-nginx116-nginx rh-php72-php-fpm
    systemctl enable zabbix-server zabbix-agent rh-nginx116-nginx rh-php72-php-fpm
    ```

    

1. 浏览器打开`http://server_ip_or_name`，按提示配置就可以，到配置数据库连接时，Database TLS encryption的勾去掉。注意如果连数据库时报identication failed错误，修改pg_hba.conf。最后登录时，默认的用户名密码为 Admin:zabbix  如果部署在公网上，请第一时间修改密码







# agent部署方式
agent支持的os就多了，包括windows、linux、moacOS、aix、freebsd等等。以windows为例，只需根据系统位数下载对应的zip包，然后通过exe起服务的形式就能启动agent。zabbix_agentd.exe只有800k，且都是调用的系统接口，不需第三方依赖。更多的内容，manual中有详细明了的解释。

# reference

-  https://www.zabbix.com/download?zabbix=5.0&os_distribution=centos&os_version=7&db=postgresql&ws=nginx
-  https://www.zabbix.com/documentation/current/manual/installation/install#installing_frontend
-  https://www.postgresql.org/docs/9.2/auth-pg-hba-conf.html
-  https://linuxize.com/post/how-to-install-postgresql-on-centos-7/
