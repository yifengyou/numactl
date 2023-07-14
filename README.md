# numactl - Rocky8.6 numactl解析


## 目录

* [numactl使用](docs/numactl使用.md)
    * [numactl](docs/numactl使用/numactl.md)
    * [numastat](docs/numactl使用/numastat.md)
    * [numademo](docs/numactl使用/numademo.md)
    * [migratepages](docs/numactl使用/migratepages.md)
    * [memhog](docs/numactl使用/memhog.md)
    * [migspeed](docs/numactl使用/migspeed.md)
* [numactl源码解析](docs/numactl源码解析.md)



## numactl信息

```
Name         : numactl
Version      : 2.0.12
Release      : 13.el8
Architecture : x86_64
Size         : 161 k
Source       : numactl-2.0.12-13.el8.src.rpm
Repository   : @System
From repo    : baseos
Summary      : Library for tuning for Non Uniform Memory Access machines
URL          : https://github.com/numactl/numactl
License      : GPLv2
Description  : Simple NUMA policy support. It consists of a numactl program to run
             : other programs with a specific NUMA policy.

```

```
# rpm -ql numactl
/usr/bin/memhog
/usr/bin/migratepages
/usr/bin/migspeed
/usr/bin/numactl
/usr/bin/numademo
/usr/bin/numastat

/usr/share/doc/numactl
/usr/share/doc/numactl/README.md
/usr/share/man/man8/memhog.8.gz
/usr/share/man/man8/migratepages.8.gz
/usr/share/man/man8/migspeed.8.gz
/usr/share/man/man8/numactl.8.gz
/usr/share/man/man8/numastat.8.gz
```





---
