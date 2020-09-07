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

