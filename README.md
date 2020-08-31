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

