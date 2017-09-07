1. Install Cygwin with OpenSSH and VIM
1. RIGHT MOUSE ON "Cygwin terminal" AND "RUN AS ADMINISTRATOR"
1. `$ ssh-host-config` , follow steps. In step of "set value for sshd Daemon", type `ntsec`
1. Open control panel, Create a new account with administrator rights, set a password for this new account
1. `$ /bin/mkpasswd.exe -l -u [new_username] >> /etc/passwd`    
(for example `/bin/mkpasswd.exe -l -u kyshel >> /etc/passwd`)
1. Open Putty, type 127.0.0.1 in ip box, Open, type new added username and password 
1. Enjoy~
