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
## Installing PYthon redis module

```
[root@ip-172-31-76-56 ~]# pip3  install  redis 
WARNING: Running pip install with root privileges is generally not a good idea. Try `pip3 install --user` instead.
Collecting redis
  Downloading https://files.pythonhosted.org/packages/a7/7c/24fb0511df653cf1a5d938d8f5d19802a88cef255706fdda242ff97e91b7/redis-3.5.3-py2.py3-none-any.whl (72kB)
    100% |################################| 81kB 7.1MB/s 
    
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

# Changing default port 

```
[root@ip-172-31-71-19 ~]# grep -in port  /etc/redis.conf 
83:# Accept connections on the specified port, default is 6379 (IANA #815344).
84:# If port 0 is specified Redis will not listen on a TCP socket.
85:port 6379
158:# warning (only very important / critical messages are logged)

---

[root@ip-172-31-71-19 ~]# vi +85  /etc/redis.conf 
[root@ip-172-31-71-19 ~]# grep -in port  /etc/redis.conf 
83:# Accept connections on the specified port, default is 6379 (IANA #815344).
84:# If port 0 is specified Redis will not listen on a TCP socket.
85:port 6300


```

## changing port with Selinux managed

```
[root@ip-172-31-71-19 ~]# getenforce 
Enforcing
[root@ip-172-31-71-19 ~]# setenforce  0
[root@ip-172-31-71-19 ~]# 
[root@ip-172-31-71-19 ~]# getenforce 
Permissive
[root@ip-172-31-71-19 ~]# systemctl  restart  redis
[root@ip-172-31-71-19 ~]# systemctl status redis
● redis.service - Redis persistent key-value database
   Loaded: loaded (/usr/lib/systemd/system/redis.service; enabled; vendor preset: disabled)
  Drop-In: /etc/systemd/system/redis.service.d
           └─limit.conf
   Active: active (running) since Mon 2020-08-31 09:49:48 UTC; 3s ago
  Process: 18558 ExecStop=/usr/libexec/redis-shutdown (code=exited, status=0/SUCCESS)
 Main PID: 18593 (redis-server)
   CGroup: /system.slice/redis.service
           └─18593 /usr/bin/redis-server 0.0.0.0:6381

Aug 31 09:49:48 ip-172-31-71-19.ec2.internal systemd[1]: Starting Redis persistent key-value database...
Aug 31 09:49:48 ip-172-31-71-19.ec2.internal systemd[1]: Started Redis persistent key-value database.
[root@ip-172-31-71-19 ~]# setenforce 1
[root@ip-172-31-71-19 ~]# getenforce 
Enforcing
[root@ip-172-31-71-19 ~]# systemctl  restart  redis
[root@ip-172-31-71-19 ~]# systemctl status redis
● redis.service - Redis persistent key-value database
   Loaded: loaded (/usr/lib/systemd/system/redis.service; enabled; vendor preset: disabled)
  Drop-In: /etc/systemd/system/redis.service.d
           └─limit.conf
   Active: failed (Result: exit-code) since Mon 2020-08-31 09:50:03 UTC; 3s ago
  Process: 18605 ExecStop=/usr/libexec/redis-shutdown (code=exited, status=0/SUCCESS)
  Process: 18620 ExecStart=/usr/bin/redis-server /etc/redis.conf --supervised systemd (code=exited, status=1/FAILURE)
 Main PID: 18620 (code=exited, status=1/FAILURE)

Aug 31 09:50:03 ip-172-31-71-19.ec2.internal systemd[1]: Stopped Redis persistent key-value database.
Aug 31 09:50:03 ip-172-31-71-19.ec2.internal systemd[1]: Starting Redis persistent key-value database...
Aug 31 09:50:03 ip-172-31-71-19.ec2.internal systemd[1]: Started Redis persistent key-value database.
Aug 31 09:50:03 ip-172-31-71-19.ec2.internal systemd[1]: redis.service: main process exited, code=exited, status=1/FAILURE
Aug 31 09:50:03 ip-172-31-71-19.ec2.internal systemd[1]: Unit redis.service entered failed state.
Aug 31 09:50:03 ip-172-31-71-19.ec2.internal systemd[1]: redis.service failed.

```
## selinux allowed Port 

```
[root@ip-172-31-71-19 ~]# semanage  port -l    |    grep -i redis
redis_port_t                   tcp      6379, 16379, 26379
[root@ip-172-31-71-19 ~]# 


===
[root@ip-172-31-71-19 ~]# semanage port -a -t redis_port_t -p tcp 6381
[root@ip-172-31-71-19 ~]# semanage  port -l    |    grep -i redis
redis_port_t                   tcp      6381, 6379, 16379, 26379

```

# cross check

```
[root@ip-172-31-71-19 ~]# getenforce 
Enforcing
[root@ip-172-31-71-19 ~]# systemctl restart  redis
[root@ip-172-31-71-19 ~]# netstat -nlpt
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 0.0.0.0:6381            0.0.0.0:*               LISTEN      18642/redis-server  
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      19923/sshd          
tcp        0      0 127.0.0.1:25            0.0.0.0:*               LISTEN      20036/master        
tcp6       0      0 :::22                   :::*                    LISTEN      19923/sshd          
tcp6       0      0 ::1:25                  :::*                    LISTEN      20036/master        
[root@ip-172-31-71-19 ~]# systemctl status redis
● redis.service - Redis persistent key-value database
   Loaded: loaded (/usr/lib/systemd/system/redis.service; enabled; vendor preset: disabled)
  Drop-In: /etc/systemd/system/redis.service.d
           └─limit.conf
   Active: active (running) since Mon 2020-08-31 09:55:01 UTC; 14s ago
  Process: 18605 ExecStop=/usr/libexec/redis-shutdown (code=exited, status=0/SUCCESS)
  
  ```
  
## checking lenght or associated values with key 

```
[root@ip-172-31-71-19 ~]# redis-cli  -h 172.31.71.19  -p 6381 
172.31.71.19:6381> 
172.31.71.19:6381> 
172.31.71.19:6381> auth Fallpass099
OK
172.31.71.19:6381> keys * 
1) "name00"
172.31.71.19:6381> keys na* 
1) "name00"
172.31.71.19:6381> set name1  ashu
OK
172.31.71.19:6381> keys na* 
1) "name1"
2) "name00"
172.31.71.19:6381> set  y  "hello world this is my name "
OK
172.31.71.19:6381> keys na* 
1) "name1"
2) "name00"
172.31.71.19:6381> keys *
1) "y"
2) "name1"
3) "name00"
172.31.71.19:6381> 
172.31.71.19:6381> STRLEN name1
(integer) 4
172.31.71.19:6381> STRLEN y
(integer) 28

```

## some maths operation 

```
172.31.71.19:6381> MSET x 100 y 200 z  5
OK
172.31.71.19:6381> keys * 
1) "z"
2) "name1"
3) "y"
4) "name00"
5) "x"
172.31.71.19:6381> get x
"100"
172.31.71.19:6381> INCR x
(integer) 101
172.31.71.19:6381> INCR x
(integer) 102
172.31.71.19:6381> INCR x
(integer) 103
172.31.71.19:6381> INCR x
(integer) 104
172.31.71.19:6381> get  x
"104"
172.31.71.19:6381> DECR x
(integer) 103
172.31.71.19:6381> DECR x
(integer) 102
172.31.71.19:6381> DECR x
(integer) 101
172.31.71.19:6381> DECR x
(integer) 100
172.31.71.19:6381> DECR x
(integer) 99
172.31.71.19:6381> get x
"99"
172.31.71.19:6381> INCRBY x  200
(integer) 299
172.31.71.19:6381> get x
"299"
172.31.71.19:6381> DECRBY x 100
(integer) 199
172.31.71.19:6381> get x
"199"
172.31.71.19:6381> 
172.31.71.19:6381> set  email  ashutoshh@
OK
172.31.71.19:6381> 
172.31.71.19:6381> get  email
"ashutoshh@"
172.31.71.19:6381> APPEND email  linux.com
(integer) 19
172.31.71.19:6381> get  email
"ashutoshh@linux.com"
172.31.71.19:6381> 
172.31.71.19:6381> 
172.31.71.19:6381> set name  ashutoshh
OK
172.31.71.19:6381> APPEND name  " singh"
(integer) 15
172.31.71.19:6381> get name
"ashutoshh singh"

```
