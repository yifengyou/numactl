# migspeed

## 帮助信息

```
# migspeed -h
usage migspeed [-p pages] [-h] [-v] from-nodes to-nodes
      from and to nodes may specified in form N or N-N
      -p pages  number of pages to try (defaults to 1000)
      -v        verbose
      -h        usage

```


## 用例及字段解析


```
# migspeed -v -p  1000 0 1
From: 0
To: 1
Allocating 1000 pages of 4096 bytes of memory
Dirtying memory....
Starting test
7fb7ef978000 bind:0 anon=1000 dirty=1000 active=0 N0=1000 kernelpagesize_kB=4
7fb7ef978000 bind:1 anon=1000 dirty=1000 active=0 N1=1000 kernelpagesize_kB=4
3.9 Mbyte migrated in 0.00 secs. 1332.2 Mbytes/second
```





---
