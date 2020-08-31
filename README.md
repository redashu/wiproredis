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

