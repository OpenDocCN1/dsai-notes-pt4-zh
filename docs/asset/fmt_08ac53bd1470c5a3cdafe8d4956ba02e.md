
1. **降低**缓存**未命中的惩罚时间**——多级缓存、关键字优先、先读后写、合并写缓冲区和牺牲缓存等技术。
1. **降低**缓存**未命中率**——更大的块大小、更大的缓存容量、更高的关联度、路预测和伪关联度以及编译器优化等技术。
1. **缩短缓存命中时间**——小而简单的缓存、避免地址转换、流水线式的缓存访问和跟踪缓存等技术。
1. 通过**并行处理**降低**未命中的惩罚时间和未命中率**——非阻塞缓存、硬件预取和编译器预取等技术。

![](img/b1be3efd597451d7be3322cb263596e5.png)

![](img/1ddaf66fc6e724c2956bdb28c53dbd78.png)

### 3.4 Cache Coherence

> 不同处理器同时访问一个`Cache`

> `Coherence`：defines what values can be returned by a read

+   当一个处理器的一读一写接连发生时，不能有另外一个处理器的写插在中间

> 主要在`cache`

> `Consistency`：determines when a written value will be returned by a read

> 主要在`memory`

两种

+   Migration：

+   A data item can be moved to a local cache and used there in a transparent fashion. Migration reduces both the latency to access a shared data item that is allocated remotely and the bandwidth demand on the shared memory.

+   Replication：

+   When shared data are being simultaneously read, the caches make a copy of the data item in the local cache. Replication reduces both latency of access and contention for a read shared data item.

+   Causes of Cache coherence problems

+   In modern parallel computers, processors often have `Cache`. Memory data may have multiple copies in the entire system. This leads to the cache coherence problem.

> 以下两个都是串行的原子的操作

+   Cache coherence protocol

+   A set of rules implemented by `Cache`, `CPU`, and memory to prevent different versions of the same data from appearing in multiple Caches forms a cache coherence protocol.

+   UMA：Bus snooping protocol 总线监督

+   总线连了一个共同的`memory`，每个`CPU`还有一个私有的`cache`和`memory`

+   In the snoopy coherence protocols, all processors snoop the bus. When a processor modifies the data in the private cache, it broadcasts invalid information or updated data on the bus to invalidate or update other copies.

![](img/101fc82b50107e61ea39a6c8edfcf229.png)

+   Write-through cache coherency protocol

+   While writing the data in the cache line, the content in the corresponding memory is also modified, and the data in the memory is kept up to date at any time.

![](img/550c9775a053bcc1baabb2731d62c34b.png)

![](img/37457ecff0a6118fc0238dbf193faf3b.png)

+   Write Invalidation Protocol

+   MSI protocol

+   Invalid

+   Shared

+   Modified

+   indicates that the block has been updated in the private cache; implies that the block is exclusive

![](img/6cef04e34b18df0dca139881f678a0d7.png)

![](img/84ab8605a32d238d78156b858a08218f.png)

> +   MESI protocol：再把`exclusive`分成`read`和`write`

> +   `exclusive`->`read` by others->`shared` `exclusive`->`write`->`modified`

+   Write-back cache consistency protocol

+   The write operation does not directly write to the memory. On the contrary, when the cache line is modified, a certain bit in the cache is set to indicate that the data in the cache line is correct but the data in the memory is out of date. Of course, the line will eventually be written back to memory, but it may be after multiple write operations.

![](img/ec572abb9819d6694d1ee46aec08370d.png)

+   MESI protocol

+   Invalid: The data contained in the cache item is invalid.

+   Shared: This row of data exists in multiple cache items, and the data  in the memory is the latest.

+   Exclusive: No other cache items include this row of data, and the  data in memory is the latest.

+   有好多正在改，会只有一个成为`Exclusive`

+   Modified: The data of the item is valid, but the data in the memory is invalid, and there is no copy of the data in other cache items.当前修改了，只有本地是对的

+   让其他变成`invalid`

+   `load` `store`只需要`buffer`，不太需要`CPU`，减少`CPU`的开销

![](img/8fd6402985efbe165332840b48e7d8ef.png)

+   例子：

![](img/f477a0985a6103243a593241d9f673d3.png)

+   MOESI

+   增加了`owned`: indicates that the associated block is owned by that cache and out-of-date in memory

+   `Modified` -> `Owned` without writing the shared block to memory

+   现在还有一种情况：

+   `x`和`y`是同一条cache线的，但是会导致比如我改`x`的时候，`y`也被迫`invalid`了，导致效率很低，其实可以分成`x`和`y`两条线

![](img/e77d4e7bde908841bb8046d6df5dc1b6.png)

![](img/a0d74947489f3e3b4eb56e6430acda2b.png)

+   NUMA：Directory based protocol 目录

+   牺牲空间选择时间

+   每个`CPU`都有一个比较近的`memory`

+   The directory protocol uses a directory to record which processors in the system have copies of certain storage blocks in the cache. When a processor wants to write a shared block, it sends an invalid signal to those processors that have copies of the block through the directory in a "point-to-point” way, so that all other copies are invalidated

![](img/6242fd53a35bee5eed34d03366a73f97.png)

### 3.5 CPU vulnerabilities

#### 3.5.1 漏洞介绍

![](img/d96675e19de68bb0f96843e872429b52.png)

> 缓存侧信道攻击

> 有防御措施：但是都很影响性能

#### 3.5.2 Meltdown

利用芯片硬件层面执行加速机制的实现缺陷，实施侧信道攻击，间接通过`CPU`缓存读取系统内存数据；利用了Intel `CPU`的乱序执行技术，通过对内存的响应时间差来建立一个侧信道攻击，破坏了位于用户和操作系统之间的基本隔离，“熔化”了由硬件来实现的安全边界

![](img/b3d64ce5fd63a77d467456e31f01a5f2.png)

![](img/0e8a1303b9c25f5e09f4dd8a5de160c9.png)

> 缓解措施

+   `KAISER`：一种内核修改，无需内核映射到用户空间

+   问题：存在一些限制。由于x86架构的设计，需要在用户空间中映射多个权限内存位置。这会留下Meltdown的残留攻击面，即，这些存储位置仍然可以从用户空间读取。

#### 3.5.3 Spectre

`Spectre`则是破坏了不同应用程序之间的隔离。问题的根源在于预测执行（speculative execution），这是一种优化技术，处理器会推测在未来有用的数据并执行计算。这种技术的目的在于提前准备好计算结果，当这些数据被需要时可立即使用。在此过程中，Intel没有很好地将低权限的应用程序与访问内核内存分开，这意味着攻击者可以使用恶意应用程序来获取应该被隔离的私有数据。

Spectre攻击主要分为三个阶段：

+   训练`CPU`的分支预测单元（BPU），使其在运行漏洞利用代码时会进行特定的预测执行；

+   预测执行使得`CPU`将要访问的地址的内容读取到`CPU Cache`中；

+   通过缓存测信道攻击，可以知道哪一个数组元素被访问过，也即对应的内存页存放在`CPU Cache`中，从而推测出地址的内容。

缓解措施

+   1）序列化指令：限制预测执行。 问题：无法在所有`CPU`或系统配置中工作。

+   2）插入推测执行阻止指令：在每个条件分支及其目标之后的指令处插入此指令。 问题：严重降低性能。

+   3）微代码修复现有处理器 问题：修补程序可能会阻止推测执行或阻止推测内存读取，但这会极大地破坏性能。

### 3.6 Storage Systems

> `Disk`,`RAID`,`Dependability`（可靠性）

1960-1980：没有存储，不断电的时候有`memory`

1990-：需要更稳定、更可靠的存储

文本很脆弱，容易受损，乱码

![](img/5fe94dd5a14e3e9809e22c8bd8821645.png)

> 写保护

#### 3.6.1 Disk

![](img/36b1a89cad30e93cd028ac6805443e2a.png)

![](img/830126f6601c69b8c4e67d347bf0e22e.png)

![](img/692fc085147c29e777ea815c731ab45b.png)

+   磁头

+   盘片

>

+   performance

+   positioning time定位时间/random access time

+   seek time

+   rotational latency

+   transmission time

> Disk Capacity

+   Areal Density

+   in 2011, the highest density

+   400 billion bits/inch^2

+   Costs per gigabyte

+   between 1983 and 2011,

+   improved by almost

+   a factor of 1,000,000

> Disk vs DRAM

+   Cost:`Disk`便宜

+   Access time:`DRAM`短

> Disk’s Competitor

+   Flash Memory

+   non-volatile semiconductor memory;

+   same bandwidth as disks;

+   100 to 1000 times faster;

+   15 to 25 times higher cost/gigabyte;

+   Wear out

+   limited to 1 million writes

+   Popular in cell phones,

+   but not in desktop and server

> Disk Power

+   Power by disk motor

+   ≈  x  x No. of platters

+   RPM: Revolutions Per Minute rotation speed

+   Smaller platters, slower rotation, and fewer platters reduce disk motor power

> 在磁盘中存放是连续的，记录`start`和`size`

> 不连续的话，用链表的形式连接起来

![](img/6e7baecb095a3e653bf76dd9fd9b5bf2.png)

#### 3.6.2 固态硬盘

非易失性存储

`SSD`

+   更快

+   电源消耗少

+   容量大/寿命短

#### 3.6.3 Disk scheduling

> minimize seek time

+   `FCFS`（First-Come First Served）

+   除了这个都会starvation

+   `SSTF`（Shortest-Seek-Time-First）

+   可能会starvation

+   `SCAN`

+   电梯算法，从一端到另一端，再从另一端到一端

+   `C-SCAN`

+   循环扫描

+   `LOCK`/`C-LOCK`

+   在`SCAN`和`C-SCAN`基础上，只需要移到一个方向的当前`queue`里面最远请求为止

#### 3.6.4 Disk Management

`mount`

> bootloader

> boot disk

Swap-space management

物理内存的数量达到临界低点，并且进程从内存移到交换空间以便释放内存空间时，就会出现交换。现实中，很少这样执行，相反，系统将交换与虚拟内存技术和交换页面结合。

#### 3.6.5 Disk Array：RAID

> 磁盘冗余阵列

> 冗余换取有效性

> `RAID 0`没有可靠性

+   Disk arrays with redundant disks to tolerate faults

+   冗余来容错

+   If a single disk fails, the lost information is reconstructed from redundant information

+   Striping: simply spreading data over multiple disks

+   RAID: redundant array of inexpensive/independent disks

![](img/8dca3e85341dcbfaee4378e224f86b22.png)

> `RAID[0]`：Nonredundant striped

![](img/c3f21ffa977b7c72cb285e800b7342aa.png)

+   可以同时访问0123，并行，读写是之前的`n`倍

+   没有校验，容错机制

+   但是性能很好

> `RAID[1]`

![](img/222081de10cda93820ff5273e44fa3dc.png)

+   镜像，安全，可同时跑

+   有点浪费

> 01结合

![](img/f7f42420bdca924d6d3d0e8520807a0b.png)
