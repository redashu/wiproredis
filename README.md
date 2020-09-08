# Redis on QA and Production 

# option 

<ol>
  
  <li> Docker </li>
  <li> VMs/Bare-metal </li>
  <li> Kubernetes </li>
  
</ol>

## REdis labs production installaion 

### Download redislabs tar from offical website 

### Untar file 

```
[root@ip-172-31-64-234 ec2-user]# ls
redislabs-6.0.8-25-rhel7-x86_64.tar
[root@ip-172-31-64-234 ec2-user]# 
[root@ip-172-31-64-234 ec2-user]# 
[root@ip-172-31-64-234 ec2-user]# tar xvf  redislabs-6.0.8-25-rhel7-x86_64.tar 
install.sh
redislabs-6.0.8-25.rhel7.x86_64.rpm
rlec_install_utils_tmpdir/
rlec_install_utils_tmpdir/custom_installer.sh
rlec_install_utils_tmpdir/install_functions
rlec_install_utils_tmpdir/install_args.conf
rlec_install_utils_tmpdir/python_args.conf
rlec_rest_api.tar.gz
rlec_upgrade_tmpdir/
rlec_upgrade_tmpdir/upgrade_checks_error_codes.sh
rlec_upgrade_tmpdir/upgrade_checks.py
rlec_upgrade_tmpdir/leash.py
rlec_upgrade_tmpdir/sync_certificates.py

====
[root@ip-172-31-64-234 ec2-user]# ls
install.sh                           redislabs-6.0.8-25.rhel7.x86_64.rpm  rlec_rest_api.tar.gz
redislabs-6.0.8-25-rhel7-x86_64.tar  rlec_install_utils_tmpdir            rlec_upgrade_tmpdir


```

## Installation 

```
[root@ip-172-31-64-234 ec2-user]# ./install.sh 
2020-09-08 05:05:06 [.] Checking root access
2020-09-08 05:05:06 [!] Running as user root, sudo is not required.
2020-09-08 05:05:06 [.] Checking prerequisites
2020-09-08 05:05:06 [.] Checking hardware requirements...
2020-09-08 05:05:06 [!] The node's hardware meets the minimum requirements for a production system: 
The node has 4 cores and 15 GB RAM
2020-09-08 05:05:06 [.] Creating /etc/opt/redislabs/redislabs_env_config.sh
================================================================================
RedisLabs Enterprise Cluster installer.
================================================================================

2020-09-08 05:05:06 [.] Creating socket directory /var/opt/redislabs/run 
2020-09-08 05:05:06 [x] Port 80 is occupied.
2020-09-08 05:05:06 [.] Deleting RedisLabs debug package if exist
2020-09-08 05:05:06 [.] Installing RedisLabs packages
2020-09-08 05:05:07 [$] executing: 'yum install -y redislabs-6.0.8-25.rhel7.x86_64.rpm'

```

### Redis with RedisInsight 

```
  7  docker  pull  redis
    8  docker pull redislabs/redisinsight

```

## Redis Db create 

```
 docker run -itd --name ashudb1  -v  /home/wipro/ashudb/:/var/lib/redis -p 1234:6379 redis
 ```
 
 ## Redis Insight creation 
 
 ```
  docker  run  -itd --name ashuinsght -v /home/wipro/ashuinsight/:/db  -p 9999:8001  redislabs/redisinsight
  
 ```
 
 


