# REdis final day

## Comparasion b/w Redis 3.x & Redis 6.x 

<b> Labs are based out of RHEL 7.0 / 7.5 / 7.6 / 8.X  </b>

### Rhel 7.0 --->Redis 3.x  will be fine 
### RHEl 7.5+ / 8.x ---> Redis 6.x  will be fine 

# Redis offerings 

<img src="redisoffer.png">

## Redis modules and core product

<img src="redismodules.png">

# Redis cluster status

<ul>
  <li> general cluster  </li>
  <li> Cluster with Master Node HA  </li>
  <li> CLuster with sentinal </li>
  
 </ul>
 
 ## General cluster 
 
 <img src="general.png">
 
 ## CLuster with master Node HA 
 
 <img src="ha.png">
 
# CLuster setup

## Steps to perform in all Nodes 

### change bind address
```
bind 0.0.0.0
```

### protected mode no
```
protected-mode no
```

### cluster enable yes 

```
cluster-enabled yes
```

### cluster config 

```
cluster-config-file nodes-6379.conf 
```

### Node timeout value

```
cluster-node-timeout 15000
```

### enable AOF

```
appendonly yes
```

### start redis service
```
systemctl start redis
systemctl enable redis
```


## Now from any of the Node you want to configure as Master Hit this command from There 

```
[root@ip-172-31-66-52 ~]# redis-cli  --cluster create  172.31.66.52:6379  172.31.66.219:6379  172.31.73.40:6379   172.31.66.189:6379  172.31.73.247:6379  172.31.76.78:6379  --cluster-replicas 1 
>>> Performing hash slots allocation on 6 nodes...
Master[0] -> Slots 0 - 5460
Master[1] -> Slots 5461 - 10922
Master[2] -> Slots 10923 - 16383
Adding replica 172.31.73.247:6379 to 172.31.66.52:6379
Adding replica 172.31.76.78:6379 to 172.31.66.219:6379
Adding replica 172.31.66.189:6379 to 172.31.73.40:6379
M: 402a42de17f965b71ac6bf6d582c74c642be9179 172.31.66.52:6379
   slots:[0-5460] (5461 slots) master
M: abe130a3866404465420b2cb01a4a97780953401 172.31.66.219:6379
   slots:[5461-10922] (5462 slots) master
M: cdbaa05a23eef98f191eeba216c0bfd5d6e28d44 172.31.73.40:6379
   slots:[10923-16383] (5461 slots) master
S: 2d2f08b10a65b31ac9e2459740f1dbbd1aa4ef0c 172.31.66.189:6379
   replicates cdbaa05a23eef98f191eeba216c0bfd5d6e28d44
S: ec1364d7f1694ee1c1892d3b6b09e0fe0496d237 172.31.73.247:6379
   replicates 402a42de17f965b71ac6bf6d582c74c642be9179
S: bc933f3e22a1a03d3cd8ba5f74f1a09c49c02fc6 172.31.76.78:6379
   replicates abe130a3866404465420b2cb01a4a97780953401
Can I set the above configuration? (type 'yes' to accept): yes
>>> Nodes configuration updated
>>> Assign a different config epoch to each node
>>> Sending CLUSTER MEET messages to join the cluster
Waiting for the cluster to join
.
>>> Performing Cluster Check (using node 172.31.66.52:6379)
M: 402a42de17f965b71ac6bf6d582c74c642be9179 172.31.66.52:6379
   slots:[0-5460] (5461 slots) master
   1 additional replica(s)
M: abe130a3866404465420b2cb01a4a97780953401 172.31.66.219:6379
   slots:[5461-10922] (5462 slots) master
   1 additional replica(s)
S: 2d2f08b10a65b31ac9e2459740f1dbbd1aa4ef0c 172.31.66.189:6379
   slots: (0 slots) slave
   replicates cdbaa05a23eef98f191eeba216c0bfd5d6e28d44
M: cdbaa05a23eef98f191eeba216c0bfd5d6e28d44 172.31.73.40:6379
   slots:[10923-16383] (5461 slots) master
   1 additional replica(s)
S: bc933f3e22a1a03d3cd8ba5f74f1a09c49c02fc6 172.31.76.78:6379
   slots: (0 slots) slave
   replicates abe130a3866404465420b2cb01a4a97780953401
S: ec1364d7f1694ee1c1892d3b6b09e0fe0496d237 172.31.73.247:6379
   slots: (0 slots) slave
   replicates 402a42de17f965b71ac6bf6d582c74c642be9179
[OK] All nodes agree about slots configuration.
>>> Check for open slots...
>>> Check slots coverage...
[OK] All 16384 slots covered.

```

## checking all master Nodes

```
[root@ip-172-31-66-52 ~]# redis-cli  -h  172.31.66.52  -p 6379   cluster  nodes     | grep -i master
abe130a3866404465420b2cb01a4a97780953401 172.31.66.219:6379@16379 master - 0 1599722993278 2 connected 5461-10922
cdbaa05a23eef98f191eeba216c0bfd5d6e28d44 172.31.73.40:6379@16379 master - 0 1599722994280 3 connected 10923-16383
402a42de17f965b71ac6bf6d582c74c642be9179 172.31.66.52:6379@16379 myself,master - 0 1599722994000 1 connected 0-5460

```

## check all slave nodes

```
[root@ip-172-31-66-52 ~]# redis-cli  -h  172.31.66.52  -p 6379   cluster  nodes     | grep -i slave
2d2f08b10a65b31ac9e2459740f1dbbd1aa4ef0c 172.31.66.189:6379@16379 slave cdbaa05a23eef98f191eeba216c0bfd5d6e28d44 0 1599722977232 3 connected
bc933f3e22a1a03d3cd8ba5f74f1a09c49c02fc6 172.31.76.78:6379@16379 slave abe130a3866404465420b2cb01a4a97780953401 0 1599722978236 2 connected
ec1364d7f1694ee1c1892d3b6b09e0fe0496d237 172.31.73.247:6379@16379 slave 402a42de17f965b71ac6bf6d582c74c642be9179 0 1599722977000 1 connected

```


# Redis Graph module 

## COnfigure redis on network 

```
[root@ip-172-31-66-52 ~]# grep  -in bind  /etc/redis.conf
48:# By default, if no "bind" configuration directive is specified, Redis listens
51:# the "bind" configuration directive, followed by one or more IP addresses.
55:# bind 192.168.1.100 10.0.0.1
56:# bind 127.0.0.1 ::1
59:# internet, binding to all the interfaces is dangerous and will expose the
61:# following bind directive, that will force Redis to listen only into
69:bind 127.0.0.1
76:# 1) The server is not binding explicitly to a set of addresses using the
77:#    "bind" directive.
87:# are explicitly listed using the "bind" directive.
[root@ip-172-31-66-52 ~]# 
[root@ip-172-31-66-52 ~]# sed  -i  '69s/127.0.0.1/0.0.0.0/'   /etc/redis.conf 
[root@ip-172-31-66-52 ~]# 
[root@ip-172-31-66-52 ~]# grep  -in bind  /etc/redis.conf
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
[root@ip-172-31-66-52 ~]# systemctl restart  redis 
[root@ip-172-31-66-52 ~]# netstat -nlpt
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 0.0.0.0:6379            0.0.0.0:*               LISTEN      21984/redis-server  
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      4433/sshd           
tcp        0      0 127.0.0.1:25            0.0.0.0:*               LISTEN      4133/master         
tcp6       0      0 :::22                   :::*                    LISTEN      4433/sshd           
tcp6       0      0 ::1:25                  :::*                    LISTEN      4133/master    

```

## Now setup Redis INsight 

```
