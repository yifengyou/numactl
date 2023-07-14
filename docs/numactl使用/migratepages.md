# migratepages

## 帮助信息

```
# migratepages -h
usage: migratepages pid from-nodes to-nodes

nodes is a comma delimited list of node numbers or A-B ranges or all.
```

- migratepages 是一个用于迁移进程页面的物理位置的工具。
- 它可以在不改变进程的虚拟地址空间的情况下，将进程的页面从一个 NUMA 节点移动到另一个 NUMA 节点。这样可以改变进程与其内存的距离，从而优化性能。
- 如果指定了多个节点，migratepages 会尽量保持每个页面在节点集合中的相对位置。

migratepages 的基本语法是：
```
migratepages pid from-nodes to-nodes
```

其中：

```
pid：指定要迁移的进程的 ID。
from-nodes：指定要从哪些节点迁移页面，可以使用以下格式：
all：所有节点
number：节点编号
number1,number2：节点编号1和节点编号2
number1-number2：从节点编号1到节点编号2的所有节点
! nodes：反选后面指定的节点
to-nodes：指定要迁移到哪些节点，格式同上。
```

## 举例

下面是一些 migratepages 的常用例子：
```
migratepages 1234 0 1
```
将进程 ID 为 1234 的进程的页面从节点 0 迁移到节点 1。

```
migratepages 5678 all 3
```
将进程 ID 为 5678 的进程的页面从所有节点迁移到节点 3。

```
migratepages 9012 0,2,4,6 1,3,5,7
```
将进程 ID 为 9012 的进程的页面从节点 0,2,4,6 迁移到节点 1,3,5,7，尽量保持相对位置不变。

```
migratepages 3456 !0 !1
```
将进程 ID 为 3456 的进程的页面从除了节点 0 外的所有节点迁移到除了节点 1 外的所有节点。





---
