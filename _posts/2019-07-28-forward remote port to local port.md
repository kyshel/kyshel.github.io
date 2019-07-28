# Background
Imagine a scene: there is a server running in friend's house, U want to ssh to that machine. But the LAN break your dream.

Is there anyway to achieve this? The answer is YES, as long as U have **another server that running on cloud**.


# Let's do it

the solution is very simple, just one-line command needs to be executed on friends server, as below:
```
ssh  -N -R *:11111:localhost:22 root@CLOUD_SERVER_IP
```

>>>

    -R [bind_address:]port:host:hostport
             Specifies that the given port on the remote
             (server) host is to be forwarded to the given host
             and port on the local side.  This works by allocat‐
             ing a socket to listen to port on the remote side,
             and whenever a connection is made to this port, the
             connection is forwarded over the secure channel,
             and a connection is made to host port hostport from
             the local machine.
             Specifying a remote bind_address will only
             succeed if the server's GatewayPorts option is
             enabled (see sshd_config(5)).

     -N      Do not execute a remote command.  This is useful
             for just forwarding ports (protocol version 2
             only).


>>>

After the command executed, the data will  be forwarded in this way:    
my_pc --> CLOUD_SERVER --> friend's server

Now run below in my_pc (USERNAME is login-able user that on friend's server )
`ssh  USERNAME@CLOUD_SERVER_IP  -p 11111 `

Bingo~

# Troubleshooting
- Like manual doc said, `GatewayPorts` option must be enabled on CLOUD_SERVER
- if U feel the password prompt appears slow,  try set `UserDNS no` and `GSSAPIAuthentication no` on CLOUD_SERVER
- add `-vvv` parameter to ssh command will show the detail info that may helpful
- execute  'netstat -anplut' on CLOUD_SERVER will show the connection info, to ensure that the port has been start listening
- Example use port 11111  on CLOUD_SERVER, please make sure this port is not used by other process. 

# The end
ssh is powerful, isn't it?
