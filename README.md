# SQL to redis

## python code to load data from MYsql to Redis 

```
pip install mysql-connector-python --user
pip install redis

```

## code 

```
[wipro@ip-172-31-79-183 ~]$ cat ashumysql_redis.py 
import  mysql.connector 

mydb=mysql.connector.connect(host='127.0.0.1',user='root',password='1234',database='mysql')

#  connecting 
sqlcursor=mydb.cursor()
#  so above  sqlcursor wll connect to mysql db 
q="select  user,password,host  from user"
# query we want to run 
sqlcursor.execute(q)
data=sqlcursor.fetchall()

print(data)



## now  preparing  data  for Redis 
#  and that format will be Hash 

import hashlib

# here creating a hash key 

hash=hashlib.sha224(q).hexdigest()
print("key got created ",hash)

#  we need to create data that must be supported by Redis 
import  cPickle
#  converting it
newdata=cPickle.dumps(data)

print(newdata)

#  Now  connecting  to Redis 
import  redis

R_SERVER=redis.Redis(host='localhost')

# now putting  data 
R_SERVER.set(hash,newdata)
# setting  TTL for 60 minutes 
R_SERVER.expire(hash,60)


```

## cross check 

```
[wipro@ip-172-31-79-183 ~]$ python  ashumysql_redis.py 
[(u'root', u'*A4B6157319038724E3560894F7F932C8886EBFCF', u'localhost'), (u'root', u'*A4B6157319038724E3560894F7F932C8886EBFCF', u'ip-172-31-79-183.ec2.internal'), (u'root', u'*A4B6157319038724E3560894F7F932C8886EBFCF', u'127.0.0.1'), (u'root', u'*A4B6157319038724E3560894F7F932C8886EBFCF', u'::1')]
('key got created ', '882dc5439b78067761e27b9b810c5c32832c232cb42c87f36c820d12')
(lp1
(Vroot
V*A4B6157319038724E3560894F7F932C8886EBFCF
Vlocalhost
tp2
a(Vroot
V*A4B6157319038724E3560894F7F932C8886EBFCF
Vip-172-31-79-183.ec2.internal
tp3
a(Vroot
V*A4B6157319038724E3560894F7F932C8886EBFCF
V127.0.0.1
tp4
a(Vroot
V*A4B6157319038724E3560894F7F932C8886EBFCF
V::1
tp5
a.
[wipro@ip-172-31-79-183 ~]$ 
[wipro@ip-172-31-79-183 ~]$ redis-cli 
127.0.0.1:6379> keys * 
1) "882dc5439b78067761e27b9b810c5c32832c232cb42c87f36c820d12"
127.0.0.1:6379> 

```

