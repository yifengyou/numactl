# numastat

## 帮助信息

```
# numastat --help
Usage: numastat [-c] [-m] [-n] [-p <PID>|<pattern>] [-s[<node>]] [-v] [-V] [-z] [ <PID>|<pattern>... ]
-c to minimize column widths
   压缩表格显示宽度，根据数据内容动态调整列宽度。
-m to show meminfo-like system-wide memory usage
   显示类似于 /proc/meminfo 的系统范围的内存使用信息。
-n to show the numastat statistics info
   显示原始统计信息，与默认行为相同，但单位是兆字节而不是页数。
-p <PID>|<pattern> to show process info
    显示指定进程或模式的每个节点的内存分配信息。
    如果参数是数字，则认为是进程 ID；
    如果参数包含非数字字符，则认为是进程命令行中要匹配的文本片段。
-s[<node>] to sort data by total column or <node>
    显示指定进程或模式在指定节点或所有节点上使用的共享内存段
-v to make some reports more verbose
    尽可能详细输出
-V to show the numastat code version
    显示版本
-z to skip rows and columns of zeros
    输出跳过空行
```


## 用例及字段解析

```
# numastat
                           node0           node1
numa_hit               122097701        34481596
numa_miss                4969235         5441250
numa_foreign             5441250         4969235
interleave_hit             39097           38715
local_node             122091355        34423780
other_node               4975579         5499066
```
显示默认的系统范围的 NUMA 统计信息。

```
# numastat -m

Per-node system memory usage (in MBs):
Token Node not in hash table.
Token Node not in hash table.
Token Node not in hash table.
Token Node not in hash table.
Token Node not in hash table.
Token Node not in hash table.
Token Node not in hash table.
Token Node not in hash table.
Token Node not in hash table.
Token Node not in hash table.
                          Node 0          Node 1           Total
                 --------------- --------------- ---------------
MemTotal                32089.32        32205.39        64294.71
MemFree                   228.83           85.59          314.42
MemUsed                 31860.49        32119.81        63980.30
Active                  21054.64        21957.28        43011.92
Inactive                 9536.50         8666.57        18203.07
Active(anon)            20769.06        21628.06        42397.12
Inactive(anon)           6391.26         5425.43        11816.70
Active(file)              285.58          329.22          614.80
Inactive(file)           3145.24         3241.14         6386.38
Unevictable                22.54          455.87          478.41
Mlocked                    22.54          455.87          478.41
Dirty                       0.70            0.77            1.46
Writeback                   0.00            0.00            0.00
FilePages                4040.83         4537.09         8577.92
Mapped                     56.33           46.08          102.41
AnonPages               26796.95        26694.32        53491.28
Shmem                       7.13            5.70           12.84
KernelStack                16.66           10.64           27.30
PageTables                 89.11           91.07          180.18
NFS_Unstable                0.00            0.00            0.00
Bounce                      0.00            0.00            0.00
WritebackTmp                0.00            0.00            0.00
Slab                      709.17          565.36         1274.53
SReclaimable              266.83          225.61          492.44
SUnreclaim                442.34          339.75          782.09
AnonHugePages           23222.00        21048.00        44270.00
HugePages_Total             0.00            0.00            0.00
HugePages_Free              0.00            0.00            0.00
HugePages_Surp              0.00            0.00            0.00

```
显示每个节点的 meminfo-like 内存使用信息。


```
# numastat -p 1

Per-node process memory usage (in MBs) for PID 1 (systemd)
                           Node 0          Node 1           Total
                  --------------- --------------- ---------------
Huge                         0.00            0.00            0.00
Heap                         1.46            0.75            2.21
Stack                        0.00            0.04            0.04
Private                      0.09            7.02            7.11
----------------  --------------- --------------- ---------------
Total                        1.56            7.81            9.37
```
显示进程 ID 为 1234 的进程的每个节点的内存分配信息。


```
# numastat -p sshd

Per-node process memory usage (in MBs)
PID                        Node 0          Node 1           Total
----------------  --------------- --------------- ---------------
1357 (sshd)                  2.03            4.76            6.79
2566 (sshd)                  2.82            4.80            7.63
2568 (sshd)                  2.91            4.71            7.62
3820 (sshd)                  2.93            4.89            7.82
9631 (sshd)                  2.51            6.41            8.92
9644 (sshd)                  3.13            5.51            8.64
----------------  --------------- --------------- ---------------
Total                       16.34           31.08           47.42
```
显示命令行中包含 java 的所有进程的每个节点的内存分配信息。


```
# numastat -s 1 sshd

Per-node process memory usage (in MBs)
PID                        Node 0          Node 1           Total
----------------  --------------- --------------- ---------------
1 (systemd)                  1.54            7.84            9.39
9631 (sshd)                  2.52            6.42            8.95
9644 (sshd)                  3.13            5.51            8.64
3820 (sshd)                  2.91            4.89            7.80
2566 (sshd)                  2.82            4.80            7.63
2568 (sshd)                  2.91            4.71            7.62
1357 (sshd)                  2.03            4.76            6.79
----------------  --------------- --------------- ---------------
Total                       17.88           38.93           56.81
```

显示进程 ID 为 1234 的进程在节点 0 上使用的共享内存段。








---
