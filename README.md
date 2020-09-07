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
