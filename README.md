# Redis for QA using Docker 

## Installing Docker  

### yum-uitls install
```
[root@ip-172-31-79-183 ~]# sudo yum install -y yum-utils

```

### confgiure yum and install it 

```
  40  sudo yum-config-manager     --add-repo     https://download.docker.com/linux/centos/docker-ce.repo
 
   42  yum  install http://mirror.centos.org/centos/7/extras/x86_64/Packages/container-selinux-2.107-1.el7_6.noarch.rpm
   43  sudo yum install docker-ce docker-ce-cli containerd.io
   
 ```
 
 ### checking from root user
 
 ```
   45  systemctl start  docker 
   46  systemctl status docker
   47  docker  version  

```

## adding nonroot user into docker group
```
 usermod -a -G docker  wipro
 ```
 
 ## creating storage directories 
 
 ```
 [wipro@ip-172-31-79-183 ~]$ whoami
wipro
[wipro@ip-172-31-79-183 ~]$ pwd
/home/wipro
[wipro@ip-172-31-79-183 ~]$ mkdir  ashu
[wipro@ip-172-31-79-183 ~]$ ls
ashu

```

## creating particular redis instance 

```
[wipro@ip-172-31-79-183 ~]$ docker  images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
[wipro@ip-172-31-79-183 ~]$ 
[wipro@ip-172-31-79-183 ~]$ 
[wipro@ip-172-31-79-183 ~]$ 
[wipro@ip-172-31-79-183 ~]$ docker  pull  redis 
Using default tag: latest
latest: Pulling from library/redis
bf5952930446: Pull complete 
911b8422b695: Pull complete 
093b947e0ade: Pull complete 
c7616728f575: Pull complete 
61a55f107028: Pull complete 
0ee3e0cb4e07: Pull complete 
Digest: sha256:933c6c01829165885ea8468d87f71127b1cb76a711311e6c63708097e92ee3d1
Status: Downloaded newer image for redis:latest
docker.io/library/redis:latest
[wipro@ip-172-31-79-183 ~]$ docker  images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
redis               latest              41de2cc0b30e        5 days ago          104MB
[wipro@ip-172-31-79-183 ~]$ 

```

## launching redis container 

```
[wipro@ip-172-31-79-183 ~]$ docker run  --name  ashuredis1  -itd -v  /home/wipro/ashu/:/var/lib/redis   redis 
834f842037afacf8d0f64a788c46291b85604da3e465d6ffaa31535fef4d62b1

```


## check it now 

```
[wipro@ip-172-31-79-183 ~]$ docker  ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
335c75c6e371        redis               "docker-entrypoint.s…"   19 seconds ago      Up 19 seconds       6379/tcp            sairedis_1
834f842037af        redis               "docker-entrypoint.s…"   53 seconds ago      Up 52 seconds       6379/tcp            ashuredis1

```

## inserting data into redis container

```
[wipro@ip-172-31-79-183 ~]$ docker exec -it  ashuredis1  bash
root@834f842037af:/data# 
root@834f842037af:/data# 
root@834f842037af:/data# 
root@834f842037af:/data# redis-cli 
127.0.0.1:6379> set name ashutoshh
OK
127.0.0.1:6379> HSET  employee  name ashutoshh email ashutoshh@linux.com contact 9509957594
(integer) 3
127.0.0.1:6379> 
127.0.0.1:6379> keys * 
1) "employee"
2) "name"
127.0.0.1:6379> 

```

## creating docker image of Redis

```
  43  docker pull centos:7
   44  docker run -it  ashuredis2  bash 
   45  docker run -it  --name ashuredis2 centos:7  bash 
   46  history 
   47  docker  ps
   48  docker ps  -a
   49  docker commit ashuredis2   redis:wiprov1
   50  docker  images

```

