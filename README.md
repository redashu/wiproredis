# snapshot 

## taking backup auto matically 

```
[root@ip-172-31-78-206 ~]# grep -in snapshot   /etc/redis.conf 
180:################################ SNAPSHOTTING  ################################
206:# By default Redis will stop accepting writes if RDB snapshots are enabled
613:# some data loss consider the default persistence mode that's snapshotting),
[root@ip-172-31-78-206 ~]# 
[root@ip-172-31-78-206 ~]# 
[root@ip-172-31-78-206 ~]# cp  /etc/redis.conf    /etc/redis.bak
[root@ip-172-31-78-206 ~]# vi  +180  /etc/redis.conf 


```

## in snapshot section 

```
################################ SNAPSHOTTING  ################################
#
# Save the DB on disk:
#
#   save <seconds> <changes>
#
#   Will save the DB if both the given number of seconds and the given
#   number of write operations against the DB occurred.
#
#   In the example below the behaviour will be to save:
#   after 900 sec (15 min) if at least 1 key changed
#   after 300 sec (5 min) if at least 10 keys changed
#   after 60 sec if at least 10000 keys changed
#
#   Note: you can disable saving completely by commenting out all "save" lines.
#
#   It is also possible to remove all the previously configured save
#   points by adding a save directive with a single empty string argument
#   like in the following example:
#
#   save ""

save 200 2
save 100 10
save 30 100
save 5  3
save 1 1    #  every second we are doing  automatic save 

```

## restarting 

```
[root@ip-172-31-78-206 ~]# systemctl  restart  redis

```

# Note : snapshot is not durable so we are enabling AOF 

## Appendonly File 

```
[root@ip-172-31-78-206 ~]# grep -in  appendonly  /etc/redis.conf 
595:appendonly no # change this to yes
597:# The name of the append only file (default: "appendonly.aof")
599:appendfilename "appendonly.aof"

```


## checking aof


```

[root@ip-172-31-78-206 ~]# systemctl  restart  redis 
[root@ip-172-31-78-206 ~]# 
[root@ip-172-31-78-206 ~]# 
[root@ip-172-31-78-206 ~]# ls   /var/lib/redis/
appendonly.aof  dump.rdb
[root@ip-172-31-78-206 ~]# cat  /var/lib/redis/appendonly.aof 

```

## now making changes 

```
127.0.0.1:6379> set  name  helloworld
OK
127.0.0.1:6379> HSET  emp  name  google email google@gmail.com 
(error) ERR wrong number of arguments for 'hset' command
127.0.0.1:6379> HSET  emp  name  google 
(integer) 1
127.0.0.1:6379> keys * 
1) "emp"
2) "name"
127.0.0.1:6379> 

```

## Fixing AOF 


```
[root@ip-172-31-78-206 redis]# redis-check-aof   --fix  /var/lib/redis/appendonly.aof 
AOF analyzed: size=108, ok_up_to=108, diff=0
AOF is valid

```

## Setting file descriptor in Linux 

```
[root@ip-172-31-78-206 redis]# cat  /etc/sysctl.conf 
# sysctl settings are defined through files in
# /usr/lib/sysctl.d/, /run/sysctl.d/, and /etc/sysctl.d/.
#
# Vendors settings live in /usr/lib/sysctl.d/.
# To override a whole file, create a new file with the same in
# /etc/sysctl.d/ and put new settings there. To override
# only specific settings, add a file with a lexically later
# name in /etc/sysctl.d/ and put new settings there.
#
# For more information, see sysctl.conf(5) and sysctl.d(5).

fs.file-max=100000

----
sysctl -p

```

## setting max allowed client in Redis as well

```
[root@ip-172-31-78-206 redis]# grep -in maxclient  /etc/redis.conf 
514: maxclients 100000


--
systemctl restart redis

```
