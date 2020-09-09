# Redis Cluster 

##  Status using 3.x version of Redis

### 1 master and N replication server 

# Steps : 

<ol>
  <li> check network and DNS  connection  </li>
  <li> install Redis every where  </li>
  <li> Configure Redis Master </li>
  <li> Configure redis replication server. </li>
   <li> Manage firewall and SElinux </li>
</ol>


## Master Node 

```
yum install redis -y
yum reinstall redis -y

systemctl stop firewalld
systemctl disable firewalld
  
```

### config backup 
```
cp /etc/redis.conf /etc/redis.bak
```

# ONly on master node 

### chaning bind address

```
[root@ip-172-31-78-206 ~]# grep -in bind  /etc/redis.conf 
48:# By default, if no "bind" configuration directive is specified, Redis listens
51:# the "bind" configuration directive, followed by one or more IP addresses.
55:# bind 192.168.1.100 10.0.0.1
56:# bind 127.0.0.1 ::1
59:# internet, binding to all the interfaces is dangerous and will expose the
61:# following bind directive, that will force Redis to listen only into
69:bind 127.0.0.1  

```

## bind changed

```
bind 0.0.0.0
```

## to Allow communication with Replicas 

```
[root@ip-172-31-78-206 ~]# grep -in protected-mode  /etc/redis.conf 

90:protected-mode yes
[root@ip-172-31-78-206 ~]# vi +90  /etc/redis.conf 
[root@ip-172-31-78-206 ~]# grep -in protected-mode  /etc/redis.conf 
90:protected-mode no

```

## TO secure client / application write / Read operation 

```
[root@ip-172-31-78-206 ~]# grep -in requirepass  /etc/redis.conf  
392:# If the master is password protected (using the "requirepass" configuration
785:# IMPORTANT NOTE: starting with Redis 6 "requirepass" is just a compatiblity
791:# requirepass foobared
1521:# So use the 'requirepass' option to protect your instance.
[root@ip-172-31-78-206 ~]# 
[root@ip-172-31-78-206 ~]# vi +791  /etc/redis.conf 
[root@ip-172-31-78-206 ~]# grep -in requirepass  /etc/redis.conf  
392:# If the master is password protected (using the "requirepass" configuration
785:# IMPORTANT NOTE: starting with Redis 6 "requirepass" is just a compatiblity
791: requirepass Wipro@99cool
1521:# So use the 'requirepass' option to protect your instance.

```

## setting Supervision  as per OS boot process

```
[root@ip-172-31-78-206 ~]# grep -in supervised  /etc/redis.conf 
231:#   supervised no      - no supervision interaction
232:#   supervised upstart - signal upstart by putting Redis into SIGSTOP mode
233:#   supervised systemd - signal systemd by writing READY=1 to $NOTIFY_SOCKET
234:#   supervised auto    - detect upstart or systemd method based on
238:supervised no
```

### updated

```
[root@ip-172-31-78-206 ~]# grep -in supervised  /etc/redis.conf 
231:#   supervised no      - no supervision interaction
232:#   supervised upstart - signal upstart by putting Redis into SIGSTOP mode
233:#   supervised systemd - signal systemd by writing READY=1 to $NOTIFY_SOCKET
234:#   supervised auto    - detect upstart or systemd method based on
238:supervised systemd

```

## applying changes 

```
[root@ip-172-31-78-206 ~]# systemctl daemon-reload
[root@ip-172-31-78-206 ~]# systemctl restart redis
[root@ip-172-31-78-206 ~]# systemctl status redis
● redis.service - Redis persistent key-value database
   Loaded: loaded (/usr/lib/systemd/system/redis.service; disabled; vendor preset: disabled)
  Drop-In: /etc/systemd/system/redis.service.d
           └─limit.conf
   Active: active (running) since Wed 2020-09-09 04:53:12 UTC; 7s ago
  Process: 8841 ExecStop=/usr/libexec/redis-shutdown (code=exited, status=0/SUCCESS)
 Main PID: 8856 (redis-server)
   Status: "Ready to accept connections"
   CGroup: /system.slice/redis.service
           └─8856 /usr/bin/redis-server 0.0.0.0:6379

Sep 09 04:53:12 ip-172-31-78-206.ec2.internal systemd[1]: Stopped Redis persistent key-value da...e.
Sep 09 04:53:12 ip-172-31-78-206.ec2.internal systemd[1]: Starting Redis persistent key-value d.....
Sep 09 04:53:12 ip-172-31-78-206.ec2.internal systemd[1]: Started Redis persistent key-value da...e.
Hint: Some lines were ellipsized, use -l to show in full.

```

# Slave / Replica configuration 

## Install and start Redis 

```
yum install redis -y
systemctl start redis 

```

## change bInd address to 0.0.0.0

```
[root@ip-172-31-74-34 ~]# grep -in bind  /etc/redis.conf 
48:# By default, if no "bind" configuration directive is specified, Redis listens
51:# the "bind" configuration directive, followed by one or more IP addresses.
55:# bind 192.168.1.100 10.0.0.1
56:# bind 127.0.0.1 ::1
59:# internet, binding to all the interfaces is dangerous and will expose the
61:# following bind directive, that will force Redis to listen only into
69:bind 0.0.0.0
76:# 1) The server is not binding explicitly to a set of addresses using the
77:#    "bind" directive.
87:# are explicitly listed using the "bind" directive.
```

## connect to Master 

### if Redis 3.x 

```
[root@ip-172-31-74-34 ~]# grep -i slaveof  /etc/redis.conf  
# Master-Replica replication. Use replicaof to make a Redis instance a copy of
# slaveof <masterip> <masterport>
#    but to INFO, replicaOF, AUTH, PING, SHUTDOWN, REPLCONF, ROLE, CONFIG,

```

### if Redis is 5.x or alter 

```
[root@ip-172-31-74-34 ~]# grep -i replicaof  /etc/redis.conf  
# Master-Replica replication. Use replicaof to make a Redis instance a copy of
# replicaof <masterip> <masterport>
#    but to INFO, replicaOF, AUTH, PING, SHUTDOWN, REPLCONF, ROLE, CONFIG,

```

### updating master password

```
#
 replicaof 172.31.78.206  6379

# If the master is password protected (using the "requirepass" configuration
# directive below) it is possible to tell the replica to authenticate before
# starting the replication synchronization process, otherwise the master will
# refuse the replica request.
#
masterauth Wipro@99cool

```

## Now starting SErvice 
```
[root@ip-172-31-74-34 ~]# systemctl restart redis

```


