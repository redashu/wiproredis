# security 

## password security 

### securing contaienr 

```
[root@ip-172-31-79-183 ~]# docker  exec -it  ashuredis1  bash 
root@8cdd17646253:/data# redis-cli 
127.0.0.1:6379> CONFIG set  requirepass 8cd07f3a5ff98f2a78cfc366c13fb123eb8d29c1ca37c79df190425d5b9e424d
OK
127.0.0.1:6379> save
OK
127.0.0.1:6379> 
root@8cdd17646253:/data# redis-cli 
127.0.0.1:6379> keys * 
(error) NOAUTH Authentication required.
127.0.0.1:6379> auth 8cd07f3a5ff98f2a78cfc366c13fb123eb8d29c1ca37c79df190425d5b9e424d
OK
127.0.0.1:6379> keys * 
(empty array)
127.0.0.1:6379> 
127.0.0.1:6379> 

```

## rename dangerious commands

```
[root@ip-172-31-79-183 ~]# grep -in  rename  /etc/redis.conf  
299:# security of read only slaves using 'rename-command' to shadow all the
485:# environment. For instance the CONFIG command may be renamed into something
491:# rename-command CONFIG b840fc02d524045429941cc15f59e41cb7be6c52

494:rename-command  CONFIG  ashucfg 

```

## Few more commands torenme


```
[root@ip-172-31-79-183 ~]# grep -in  rename  /etc/redis.conf
[root@ip-172-31-79-183 ~]# grep -in  rename  /etc/redis.conf
299:# security of read only slaves using 'rename-command' to shadow all the
485:# environment. For instance the CONFIG command may be renamed into something
491:# rename-command CONFIG b840fc02d524045429941cc15f59e41cb7be6c52
494:rename-command  CONFIG  ashucfg 
495:rename-command  FLUSHALL   ""
496:rename-command  FLUSHDB   ""
503:# rename-command CONFIG ""
886:#  g     Generic commands (non-type specific) like DEL, EXPIRE, RENAME, ...


```

