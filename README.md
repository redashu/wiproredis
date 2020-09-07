## Monitoring 

```
[root@ip-172-31-79-183 ~]# redis-cli   monitor  
OK





1599476274.214601 [0 103.195.202.124:14365] "info" "server"
1599476297.414425 [0 103.195.202.124:14365] "keys" "*"
1599476312.770507 [0 103.195.202.124:14365] "set" "name" "helloworld" "120"
1599476317.517537 [0 103.195.202.124:14365] "set" "name" "helloworld"
1599476329.910884 [0 103.195.202.124:14365] "get" "name"
^C
[root@ip-172-31-79-183 ~]# redis-cli   monitor   >/tmp/redis.logs
^C
[root@ip-172-31-79-183 ~]# cat  /tmp/redis.logs 
OK
1599476359.798512 [0 103.195.202.124:14365] "info" "client"
1599476364.794091 [0 103.195.202.124:14365] "info" "memory"



```

## logs 

```
[root@ip-172-31-79-183 ~]# !106
tail  -f  /var/log/redis/redis.log 
      `-._    `-.__.-'    _.-'                                       
          `-._        _.-'                                           
              `-.__.-'                                               

10031:M 07 Sep 10:54:04.957 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
10031:M 07 Sep 10:54:04.957 # Server started, Redis version 3.2.12
10031:M 07 Sep 10:54:04.957 # WARNING overcommit_memory is set to 0! Background save may fail under low memory condition. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
10031:M 07 Sep 10:54:04.957 # WARNING you have Transparent Huge Pages (THP) support enabled in your kernel. This will create latency and memory usage issues with Redis. To fix this issue run the command 'echo never > /sys/kernel/mm/transparent_hugepage/enabled' as root, and add it to your /etc/rc.local in order to retain the setting after a reboot. Redis must be restarted after THP is disabled.
10031:M 07 Sep 10:54:04.957 * DB loaded from disk: 0.000 seconds
10031:M 07 Sep 10:54:04.957 * The server is now ready to accept connections on port 6379


```

