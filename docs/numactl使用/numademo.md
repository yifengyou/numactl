# numademo


numademo是一个用于测试和演示libnuma库功能的程序，它可以用来评估不同的NUMA内存分配策略对系统性能的影响。

它可以在不同的NUMA节点上运行一些内存密集型的操作，比如memcpy，然后报告吞吐量和延迟等指标。它还可以模拟不同的内存分配策略，比如本地分配，交叉分配，首选节点分配等，并观察它们对性能的影响。

NUMA是一种非一致性内存访问架构，它将内存划分为多个节点，每个节点与一组CPU核心相连，这样可以提高内存访问的局部性和效率。

## 帮助信息

```
# numademo
usage: numademo [-S] [-f] [-c] [-e] [-t] msize[kmg] {tests}
No tests means run all.
-c output CSV data. -f run even without NUMA API. -S run stupid tests. -e exit on error
-t regression test; do not run all node combinations
valid tests: memset memcpy forward backward stream random2 ptrchase
```



## numademo的基本用法

```bash
numademo [-S] [-f] [-c] [-e] [-t] msize[kmg] {tests}
```

其中：

- `-S`表示在每次测试之前清空系统缓存。
- `-f`表示即使没有NUMA支持也强制运行。
- `-c`表示以CSV格式输出数据。
- `-e`表示在每次测试之后打印错误信息。
- `-t`表示设置测试时间（秒）。
- `msize`表示设置每个线程分配的内存大小，可以用k、m、g为单位。
- `{tests}`表示选择要运行的测试，如果省略则运行所有测试。

numademo支持以下测试：

- `local`：在本地节点上分配和访问内存。
- `interleave`：在所有节点上交叉分配和访问内存。
- `nodeX`：在指定节点X上分配和访问内存。
- `interleaveX,Y,...`：在指定节点X、Y等上交叉分配和访问内存。
- `preferredX`：设置首选节点为X，并根据系统策略分配和访问内存。
- `manualinterleave`：手动设置交叉分配策略，并访问内存。
- `manualinterleaveX,Y,...`：手动设置交叉分配策略为指定节点X、Y等，并访问内存。

## 如何解释numademo输出？

numademo的输出格式如下：

```
{test name} {memory policy} memcpy Avg {average throughput} MB/s Max {maximum throughput} MB/s Min {minimum throughput} MB/s
```

其中：

- `{test name}`表示测试的名称，比如local、interleave、node0等。
- `{memory policy}`表示测试时使用的内存分配策略，比如without policy、interleaved on all nodes、on node 0等。
- `{average throughput}`表示测试时memcpy操作的平均吞吐量（MB/s）。
- `{maximum throughput}`表示测试时memcpy操作的最大吞吐量（MB/s）。
- `{minimum throughput}`表示测试时memcpy操作的最小吞吐量（MB/s）。

一般来说，吞吐量越高，表示性能越好。不同的测试和策略可能会导致不同的吞吐量。一些影响因素有：

- NUMA节点之间的距离（distance）：不同节点之间访问内存的延迟可能不同，距离越远，延迟越高，吞吐量越低。可以用`numactl --hardware`命令查看节点之间的距离矩阵。
- NUMA节点之间的负载（load）：不同节点之间访问内存的带宽可能不同，负载越高，带宽越低，吞吐量越低。可以用`numastat`命令查看节点之间的负载情况。
- NUMA节点的内存大小（size）：不同节点的可用内存大小可能不同，内存越小，分配越困难，吞吐量越低。可以用`numactl --hardware`命令查看节点的内存大小。
- NUMA节点的内存分配策略（policy）：不同的内存分配策略可能会影响性能，比如本地分配（local）、交叉分配（interleave）、首选节点分配（preferred）等。一般来说，本地分配会比交叉分配有更高的吞吐量，因为减少了跨节点访问的延迟。首选节点分配会根据系统的默认策略来分配内存，比如最近优先（localalloc）、最少使用优先（bind）等。可以用`numactl --show`命令查看系统的默认策略。

## 常用例子

下面给出一些常用的numademo例子：

- 在一个双节点的NUMA系统上，运行所有测试，每个线程分配1G内存，每个测试持续10秒，并清空缓存：

```bash
numademo -S -t 10 1g
```

- 在一个四节点的NUMA系统上，只运行本地分配和交叉分配两种测试，每个线程分配512M内存，每个测试持续5秒，并以CSV格式输出数据：

```bash
numademo -c -t 5 512m local interleave
```

- 在一个单节点的NUMA系统上，只运行在节点0上分配和访问内存的测试，每个线程分配256M内存，每个测试持续3秒，并打印错误信息：

```bash
numademo -e -t 3 256m node0
```

- 使用1GB的内存来测试memset操作，不指定任何NUMA策略。
```
numademo -m 1G memset
```


- 使用1GB的内存来测试memset操作，并且只使用节点0的内存。
```
numademo -m 1G -N 0 memset
```

- 使用1GB的内存来测试memset操作，并且在节点0和节点1之间交错分布内存。
```
numademo -m 1G -i 0,1 memset
```


- 使用1GB的内存来测试memset操作，并且优先分配节点0的内存。
```
numademo -m 1G -p 0 memset
```

- 使用1GB的内存来测试memset操作，并且手动分配节点0的内存。
```
numademo -m 1G -a 0 memset
```

- 使用1GB的内存来测试memcpy操作，并且在核心0和核心1之间交替运行。
```
numademo -m 1G -c 0,1 memcpy
```
















































---
