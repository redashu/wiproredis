# Redis 

## Key value stored. Nosql DB 

# Installation of Redis on Single Node 

## checking OS name

```
[root@ip-172-31-71-19 ~]# cat  /etc/os-release 
NAME="Red Hat Enterprise Linux Server"
VERSION="7.6 (Maipo)"
ID="rhel"
ID_LIKE="fedora"
VARIANT="Server"
VARIANT_ID="server"
VERSION_ID="7.6"
PRETTY_NAME="Red Hat Enterprise Linux Server 7.6 (Maipo)"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:redhat:enterprise_linux:7.6:GA:server"
HOME_URL="https://www.redhat.com/"
BUG_REPORT_URL="https://bugzilla.redhat.com/"

REDHAT_BUGZILLA_PRODUCT="Red Hat Enterprise Linux 7"
REDHAT_BUGZILLA_PRODUCT_VERSION=7.6
REDHAT_SUPPORT_PRODUCT="Red Hat Enterprise Linux"
REDHAT_SUPPORT_PRODUCT_VERSION="7.6"

```

## RAM and CPU check 

```
[root@ip-172-31-71-19 ~]# free  -m
              total        used        free      shared  buff/cache   available
Mem:           3787          91        3477          16         219        3450
Swap:             0           0           0


----

[root@ip-172-31-71-19 ~]# lscpu 
Architecture:          x86_64
CPU op-mode(s):        32-bit, 64-bit
Byte Order:            Little Endian
CPU(s):                2
On-line CPU(s) list:   0,1
Thread(s) per core:    1
Core(s) per socket:    2
Socket(s):             1
NUMA node(s):          1
Vendor ID:             GenuineIntel
CPU family:            6
Model:                 79
Model name:            Intel(R) Xeon(R) CPU E5-2686 v4 @ 2.30GHz
Stepping:              1
CPU MHz:               2300.219
BogoMIPS:              4600.03
Hypervisor vendor:     Xen

```

## check mounted storage

```
[root@ip-172-31-71-19 ~]# df  -h 
Filesystem      Size  Used Avail Use% Mounted on
/dev/xvda2       50G  935M   50G   2% /
devtmpfs        1.9G     0  1.9G   0% /dev
tmpfs           1.9G     0  1.9G   0% /dev/shm
tmpfs           1.9G   17M  1.9G   1% /run
tmpfs           1.9G     0  1.9G   0% /sys/fs/cgroup
tmpfs           379M     0  379M   0% /run/user/1000

```

## checking existing installation 

```
[root@ip-172-31-71-19 ~]# rpm  -qa    |    grep -i  redis
[root@ip-172-31-71-19 ~]# 
[root@ip-172-31-71-19 ~]# 
[root@ip-172-31-71-19 ~]# 
[root@ip-172-31-71-19 ~]# rpm  -qa  redis* 
[root@ip-172-31-71-19 ~]# 

```

## Configure Yum offline / online. Repo 

### yum udpate if you have internet connected

```
[root@ip-172-31-71-19 ~]# yum  update  -y

```

### chaning directory and create yum client 

```
[root@ip-172-31-71-19 ~]# cd   /etc/yum.repos.d/

[root@ip-172-31-71-19 yum.repos.d]# cat  ashu.repo 
[redis]
baseurl=http://172.16.52.25/Redis
gpgcheck=0

```

## Installing  Redis

```
yum  install  redis

---

[root@ip-172-31-71-19 ~]# rpm   -q    redis 
redis-3.2.12-2.el7.x86_64
[root@ip-172-31-71-19 ~]# rpm  -qi  redis 
Name        : redis
Version     : 3.2.12
Release     : 2.el7
Architecture: x86_64
Install Date: Mon Aug 31 05:55:04 2020
Group       : Unspecified
Size        : 1424583
License     : BSD
Signature   : RSA/SHA256, Fri Oct 26 07:10:06 2018, Key ID 6a2faea2352c64e5
Source RPM  : redis-3.2.12-2.el7.src.rpm
Build Date  : Fri Oct 26 07:05:47 2018
Build Host  : buildhw-10.phx2.fedoraproject.org
Relocations : (not relocatable)
Packager    : Fedora Project
Vendor      : Fedora Project
URL         : http://redis.io
Bug URL     : https://bugz.fedoraproject.org/redis
Summary     : A persistent key-value database
Description :


```

## Starting Redis DB Engine 

```
[root@ip-172-31-71-19 ~]# systemctl   start  redis
[root@ip-172-31-71-19 ~]# systemctl   status redis
● redis.service - Redis persistent key-value database
   Loaded: loaded (/usr/lib/systemd/system/redis.service; disabled; vendor preset: disabled)
  Drop-In: /etc/systemd/system/redis.service.d
           └─limit.conf
   Active: active (running) since Mon 2020-08-31 06:25:04 UTC; 8s ago
 Main PID: 17992 (redis-server)
   CGroup: /system.slice/redis.service
           └─17992 /usr/bin/redis-server 127.0.0.1:6379

Aug 31 06:25:04 ip-172-31-71-19.ec2.internal systemd[1]: Starting Redis persistent key-value database...
Aug 31 06:25:04 ip-172-31-71-19.ec2.internal systemd[1]: Started Redis persistent key-value database.

```

## auto start on reboot

```
[root@ip-172-31-71-19 ~]# systemctl   enable  redis
Created symlink from /etc/systemd/system/multi-user.target.wants/redis.service to /usr/lib/systemd/system/redis.service.
```

## checking port binding for Redis

```
[root@ip-172-31-71-19 ~]# netstat  -nlpt
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 127.0.0.1:6379          0.0.0.0:*               LISTEN      17992/redis-server  

```

# Redis client connect

## Local client 

```
[root@ip-172-31-71-19 ~]# redis-cli  
127.0.0.1:6379> 
127.0.0.1:6379> 
127.0.0.1:6379> 


```


# Hashes 

```
3.236.140.200:6381> HMSET  emp  name jack email jack@gmail.com  contact 893556245 
OK
3.236.140.200:6381> keys * 
1) "email"
2) "emp"
3) "name"
4) "z"
5) "name1"
6) "y"
7) "name00"
8) "x"
3.236.140.200:6381> HGET  emp  email
"jack@gmail.com"
3.236.140.200:6381> HGET  emp  name
"jack"
3.236.140.200:6381> HGETALL  emp
1) "name"
2) "jack"
3) "email"
4) "jack@gmail.com"
5) "contact"
6) "893556245"
3.236.140.200:6381> HKEYS emp
1) "name"
2) "email"
3) "contact"
3.236.140.200:6381> HVALS  emp
1) "jack"
2) "jack@gmail.com"
3) "893556245"

```
