# memhog - Allocates memory with policy for testing

memhog 是一个用于测试内存分配和使用的工具。它可以在给定的大小和策略下，使用 mmap 分配一块内存区域，并使用 memset 对其进行更新。它可以帮助您模拟不同的内存压力和场景，以及观察系统的内存行为和性能。

memhog 中的 hog 不是一个单词的缩写，而是一个英文单词，意思是“贪婪地占用或使用”。在这里，它表示 memhog 这个工具可以贪婪地占用或使用系统的内存，以达到测试的目的。memhog 的名字可以理解为“内存贪婪者”的意思。

memhog 测试后没有任何报告，是因为它是一个简单的工具，只是用于分配和更新内存区域，而不是用于测量或评估内存性能或效果。如果想知道测试结果的性能或效果，需要使用其他的工具或方法来监控和分析系统的内存使用情况和行为。

可以使用以下一些工具或方法：

- 使用 top 或 free 命令来查看系统的总体内存使用情况和空闲内存量。
- 使用 vmstat 或 sar 命令来查看系统的内存活动和压力，包括页面交换和缺页中断等指标。
- 使用 numastat 命令来查看每个 NUMA 节点的内存分配和使用情况，以及 NUMA 命中和失效等统计信息。
- 使用 perf 或 oprofile 等性能分析工具来收集和分析系统或进程的内存相关的性能事件和数据。
- 使用 /proc/meminfo 或 /sys/devices/system/node/node*/meminfo 等文件来获取更多的内存信息和细节。


## 帮助信息

```
memhog 的基本语法是：

memhog [ -r<NUM> ] [ size kmg ] [ policy nodeset ] [ -f<filename> ]
```

```

# memhog

memhog [-rNUM] size[kmg] [policy [nodeset]]
-rNUM repeat memset NUM times
      指定使用 memset 更新内存区域的次数

-H disable transparent hugepages

size kmg：指定分配的内存区域的大小，可以使用 k（千字节），m（兆字节）或 g（吉字节）作为后缀

policy nodeset：指定分配内存时使用的 NUMA 策略和节点集合。支持的策略有
  --interleave（交错）
  --membind（绑定）
  --preferred（优先）
  default（默认）

-f<filename>：指定使用 mmap 时的文件名，如果不指定则使用匿名映射

```


# 举例

```
memhog -r3 1G
```
分配 1G 的内存区域，使用默认策略，重复更新 3 次。

```
memhog -r5 512M --interleave 0-3
```
分配 512M 的内存区域，使用交错策略，在节点 0 到 3 之间轮流分配，重复更新 5 次。


```
memhog -r10 256M --membind 1
```
分配 256M 的内存区域，使用绑定策略，只在节点 1 上分配，重复更新 10 次。

```
memhog -r8 128M --preferred 2
```
分配 128M 的内存区域，使用优先策略，优先在节点 2 上分配，如果不够则回退到其他节点，重复更新 8 次。

```
memhog -r6 64M -f memhog.mmap
```
分配 64M 的内存区域，使用默认策略，使用 memhog.mmap 文件作为映射源，重复更新 6 次。








---




























---
