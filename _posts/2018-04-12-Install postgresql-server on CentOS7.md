### Require
    yum install httpd php vim -y


### Install 
    yum install postgresql-server phpPgAdmin
    postgresql-setup initdb
    service postgresql start && chkconfig postgresql on

### Config
    su postgres
    psql
    /passwd
    ^d ^d

### Allow Local Connection
    vim /usr/share/phpPgAdmin/conf/config.inc.php
    change $conf['extra_login_security'] = true; to  $conf['extra_login_security'] = false;
    vim /var/lib/pgsql/data/pg_hba.conf
    find line: local   all             all                                      peer
    change to: local   all             all                                      md5
    service postgresql restart
    
### Verify
input host.address in browser and check it
