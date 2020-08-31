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

