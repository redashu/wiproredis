# Storing data in Redis using Data structure

<ol>
  
  <li> string </li>
  <li> sets </li>
  <li> hashes </li>
  <li> order sets </li>
  <li> hyperlogics </li>
  
 </ol>
 
 ## connecting and storing 
 
 ```
 [root@ip-172-31-71-19 ~]# redis-cli  
127.0.0.1:6379> 
127.0.0.1:6379> 
127.0.0.1:6379> set  name  "ashutoshh singh"
OK
127.0.0.1:6379> set  x  200
OK
127.0.0.1:6379> set  y 67.4
OK
127.0.0.1:6379> 
127.0.0.1:6379> keys  * 
1) "x"
2) "y"
3) "name"


```

## getting values
```
127.0.0.1:6379> get  y
"67.4"
127.0.0.1:6379> get  x
"200"
127.0.0.1:6379> get  name
"ashutoshh singh"
127.0.0.1:6379> 

```

## check and update if not present 

```
127.0.0.1:6379> set  accno  87744
OK
127.0.0.1:6379> 
127.0.0.1:6379> keys * 
1) "x"
2) "accno"
3) "y"
4) "name"
127.0.0.1:6379> get  accno
"87744"
127.0.0.1:6379> setnx  accno 90000
(integer) 0
127.0.0.1:6379> get  accno
"87744"
127.0.0.1:6379> setnx  accno1 90000
(integer) 1
127.0.0.1:6379> get  accno1
"90000"

```

## deletion of keys

```
127.0.0.1:6379> del x 
(integer) 1
127.0.0.1:6379> keys *
1) "accno1"
2) "accno"
3) "name"
4) "y"


```
## string data with TTL 

```
127.0.0.1:6379> SETEX  passcheck 10 wipro1233
OK
127.0.0.1:6379> keys * 
1) "accno1"
2) "passcheck"
3) "accno"
4) "name"
5) "y"
127.0.0.1:6379> get passcheck
(nil)
127.0.0.1:6379> keys * 
1) "accno1"
2) "accno"
3) "name"
4) "y"
127.0.0.1:6379> SETEX  passcheck 10 wipro1233
OK
127.0.0.1:6379> 
127.0.0.1:6379> ttl  passcheck 
(integer) 3
127.0.0.1:6379> ttl  passcheck 
(integer) 2
127.0.0.1:6379> ttl  passcheck 
(integer) 1
127.0.0.1:6379> ttl  passcheck 
(integer) -2
127.0.0.1:6379> keys * 
1) "accno1"
2) "accno"
3) "name"
4) "y"

```

# Configure Redis 

## running redis on all interfaces

### checking configuration file

```
127.0.0.1:6379> INFO  server 
# Server
redis_version:3.2.12
redis_git_sha1:00000000
redis_git_dirty:0
redis_build_id:7897e7d0e13773f
redis_mode:standalone
os:Linux 3.10.0-957.5.1.el7.x86_64 x86_64
arch_bits:64
multiplexing_api:epoll
gcc_version:4.8.5
process_id:17992
run_id:f0bcdc0f467732136dc64f2f9a64b85de7356324
tcp_port:6379
uptime_in_seconds:2856
uptime_in_days:0
hz:10
lru_clock:5022440
executable:/usr/bin/redis-server
config_file:/etc/redis.conf

```

### another method 
```
[root@ip-172-31-71-19 ~]# rpm  -qc  redis
/etc/logrotate.d/redis
/etc/redis-sentinel.conf
/etc/redis.conf
/etc/systemd/system/redis-sentinel.service.d/limit.conf
/etc/systemd/system/redis.service.d/limit.conf

```

###   changi

```
vim /etc/redis.conf 
# ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
bind 0.0.0.0

```

## starting redis service

```
[root@ip-172-31-71-19 ~]# systemctl  restart  redis

[root@ip-172-31-71-19 ~]# netstat -nlpt
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 0.0.0.0:6379            0.0.0.0:*               LISTEN     


```
  
## Connecting. from Remote Client

```

root@80218152457b:/data# redis-cli   -h  3.236.140.200
3.236.140.200:6379> 
3.236.140.200:6379> 
3.236.140.200:6379> keys * 
1) "accno"
2) "name"
3) "accno1"
4) "y"

3.236.140.200:6379> set  product  "hello world"
OK

```


# Remote client connection 

```
[root@ip-172-31-76-56 ~]# redis-cli  -h 3.236.140.200 
3.236.140.200:6379> INFO  clients
# Clients
connected_clients:3
client_longest_output_list:0
client_biggest_input_buf:0
blocked_clients:0
3.236.140.200:6379> INFO  server
# Server
redis_version:3.2.12
redis_git_sha1:00000000
redis_git_dirty:0
redis_build_id:7897e7d0e13773f
redis_mode:standalone
os:Linux 3.10.0-957.5.1.el7.x86_64 x86_64
arch_bits:64
multiplexing_api:epoll
gcc_version:4.8.5
process_id:18150
run_id:353e0522136731bfad08ec1340f764fcbef9ac20
tcp_port:6379
uptime_in_seconds:5535
uptime_in_days:0
hz:10
lru_clock:5028337
executable:/usr/bin/redis-server
config_file:/etc/redis.conf

```

## setting multiple key and values 

```
3.236.140.200:6379> MSET  name ashu age 29 email ashutoshh@linux.com
OK
3.236.140.200:6379> keys * 
1) "age"
2) "1"
3) "name"
4) "email"

```

## SEcuring Server with Password

```
[root@ip-172-31-71-19 ~]# grep -in  requirepass  /etc/redis.conf 
268:# If the master is password protected (using the "requirepass" configuration
481:# requirepass foobared

---

[root@ip-172-31-71-19 ~]# vi +481  /etc/redis.conf 
[root@ip-172-31-71-19 ~]# grep -in  requirepass  /etc/redis.conf 
268:# If the master is password protected (using the "requirepass" configuration
481: requirepass Fallpass099

--

[root@ip-172-31-71-19 ~]# systemctl  restart  redis
[root@ip-172-31-71-19 ~]# systemctl  status  redis
● redis.service - Redis persistent key-value database
   Loaded: loaded (/usr/lib/systemd/system/redis.service; enabled; vendor preset: disabled)
  Drop-In: /etc/systemd/system/redis.service.d
           └─limit.conf
   Active: active (running) since Mon 2020-08-31 09:03:26 UTC; 11s ago
  Process: 18380 ExecStop=/usr/libexec/redis-shutdown (code=exited, status=0/SUCCESS)
 Main PID: 18395 (redis-server)
   CGroup: /system.slice/redis.service
           └─18395 /usr/bin/redis-server 0.0.0.0:6379

Aug 31 09:03:26 ip-172-31-71-19.ec2.internal systemd[1]: Starting Redis persistent key-value database...
Aug 31 09:03:26 ip-172-31-71-19.ec2.internal syst

```

## checking authentication 

```
[root@ip-172-31-76-56 ~]# redis-cli  -h 3.236.140.200 
3.236.140.200:6379> 
3.236.140.200:6379> 
3.236.140.200:6379> keys * 
(error) NOAUTH Authentication required.
3.236.140.200:6379> set ookk  1000
(error) NOAUTH Authentication required.
3.236.140.200:6379> auth 
(error) ERR wrong number of arguments for 'auth' command
3.236.140.200:6379> auth Fallpass099
OK

```

# Redis via python 

## installing python On your client machine  (windows/linux/mac)

```
[root@ip-172-31-76-56 ~]# yum  install python3
Failed to set locale, defaulting to C
Loaded plugins: amazon-id, rhui-lb, search-disabled-repos
Resolving Dependencies

```

```
[root@ip-172-31-76-56 ~]# cat  ashuredis.py 
import   redis
import  time

#  loading  driver in python to connect or load Redis

# connecting to Remote Redis


r_connect=redis.Redis(host='3.236.140.200',port=6379,password='Fallpass099')
r_connect.set('name11',"ashutoshh singh")
time.sleep(3)
value=r_connect.get('name11')
print(value)

```
