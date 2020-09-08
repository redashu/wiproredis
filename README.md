# Redis Cluster 

##  Status

### 1 master and N replication server 

# Steps : 

<ol>
  <li> check network and DNS  connection  </li>
  <li> install Redis every where  </li>
  <li> Configure Redis Master </li>
  <li> Configure redis replication server. </li>
   <li> Manage firewall and SElinux </li>
</ol>


## Master Node 

```
yum install redis -y
yum reinstall redis -y

systemctl stop firewalld
systemctl disable firewalld
  
```

### config backup 
```
cp /etc/redis.conf /etc/redis.bak
```

## ONly on master node 
```
