# REdis final day

## Comparasion b/w Redis 3.x & Redis 6.x 

<b> Labs are based out of RHEL 7.0 / 7.5 / 7.6 / 8.X  </b>

### Rhel 7.0 --->Redis 3.x  will be fine 
### RHEl 7.5+ / 8.x ---> Redis 6.x  will be fine 

# Redis offerings 

<img src="redisoffer.png">

## Redis modules and core product

<img src="redismodules.png">

# Redis cluster status

<ul>
  <li> general cluster  </li>
  <li> Cluster with Master Node HA  </li>
  <li> CLuster with sentinal </li>
  
 </ul>
 
 ## General cluster 
 
 <img src="general.png">
 
 ## CLuster with master Node HA 
 
 <img src="ha.png">
 
# CLuster setup

## Steps to perform in all Nodes 

### change bind address
```
bind 0.0.0.0
```

### protected mode no
```
protected-mode no
```

### cluster enable yes 

```
cluster-enabled yes
```

### cluster config 

```
cluster-config-file nodes-6379.conf 
```

### Node timeout value

```
cluster-node-timeout 15000
```

### enable AOF

```
appendonly yes
```

### start redis service
```
systemctl start redis
systemctl enable redis
```
