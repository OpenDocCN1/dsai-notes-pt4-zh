
`message`空间用来report带内的`message`和`event`。例如`error message`、电源管理消息等

# PCIe Transaction Model

## PIO

`Programmed I/O`，可编程输入输出

数据传送由CPU执行I/O端口指令来按照字节或更大的数据单位来处理，占用大量的CPU资源，数据传输速度也大大低于DMA模式。

![](img/07b468e7fb99bd7a68c496bd4901cb33.png)

## DMA

`Direct Memory Access`，直接内存访问。

数据传送由 DMA 控制器完成，占用极少的 CPU 资源。

![](img/5ca40451f6d44fd98024263e1f9d63f7.png)

## P2P

`Peer-to-Peer`。

一个`endpoint`发送`packet`给另一个`endpoint`。

`P2P`路由在`Switch`中是必需的，但对于`RC`来说是可选的。

![](img/98e9b42ea6955df38ea696762d235c0a.png)

# PCIe 基本概念

重要`feature`

1. `Point-to-Point bus`

2. `Bi-directional bus`

3. `Scalability of data rates`

4. `Backwards compatibility`

5. `Wide adoption across many markets`

## P2P 双工

PCIe的总线并不是我们传统意义上共享线路的总线（`Bus`），而是一个点对点的网络

1. `Point-to-Point bus`

2. `Bi-directional bus`

![](img/66eba771ec2a9f22c256ec852e03ba60.png)

## Data Link & Lane

一组`lane`是一个`date link`。

`Lane`数量可变 -> `Scalable`

![](img/626d6f4e0bad9f18488021c34a9d899d.png)

![](img/e9aa45bc6c132a46881109df2b64d128.png)

## Root Complex

广义来说，`Root Complex`可以认为是 CPU 和 PCIe 拓扑之间的接口。或者说，PCIe 树的根节点。

所有的数据，都要经过`Root Complex`。

`Root Complex`集成在 CPU 内，或作为 CPU 外的一个单独 component。

从广义上讲，`RC`可以理解为CPU前端总线和PCIe总线之间的桥，主要负责将CPU的访问事务转换为PCIe总线上的访问事务。PCIe总线是以报文的形式交换信息或者传输数据的，因此，`RC`负责根据CPU的访问事务产生对应的报文，并转发给下游PCIe设备，同时也要解析下游PCIe设备上报的报文，并根据报文内容，将信息或者数据通知CPU。

在早期的CPU上，`Root Complex`其实是放在了北桥（MCU）上 [5]，后来随着技术的发展，现在已经都集成进了CPU内部了 [8]。（注意下图的`System Agent`的部分，他就是`PCIe Root Complex`所在的位置。）

![](img/0a19e00dcd9c189231f8ecfe7937c05c.png)

虽然是根节点，但是系统里面可以存在不只一个`Root Complex`。随着PCIe Lane的增加，PCIe控制器和`Root Complex`的数量也随之增加。比如，我的台式机的CPU是i9-10980xe，上面就有4个`Root Complex`，而我的笔记本是i7-9750H，上面就只有一个`Root Complex`。我们在Windows上可以通过设备管理器来查看：

| `i9-11900K` | `i7-9750H` |

| --- | --- |

| ![](img/abebbcdda7cd2df5f387f315b85b3757.png) | ![](img/7f10e619e0a14fbced8f73947bd2daa2.png) |

用的CPU是EPYC 7742，可以很明显的看到`PEG P0-3`，对应着4个PCIe Controller和`Root Complex`

https://www.supermicro.org.cn/en/products/motherboard/H12DSi-Nt6

![](img/9cc9910eed128d88b4d63d3455454d7a.png)

一个`RC`可以包括多个`RP`（`Root Port`），例如一个16条`lane`的PCIe `RC`可以包括4个`RP`（4个x4的），或者8个`RP`（8个x2的）等等。

与PCI总线的中`Host bridge`有些类似，是CPU和PCIe总线直接的接口。

从软件的角度来看，`RC` 像是一组虚拟的 `PCI-PCI` 桥。

1. 完成存储器域到 PCIe 总线域的地址转换，

2. 随着虚拟化技术的引入，`RC`的功能也越来越复杂。

3. `RC`把来自 CPU 的`request`转化成PCIe的4种不同的`requests`(`configuration`， `Memory`， `I/O`， `Message`)并发送给接在它下面的设备。

4. `Root Complex` 可以访问内存

`RC`支持的主要功能有：

1. 配置空间管理：`RC`管理 PCIe 总线上所有设备的配置空间。每个 PCIe 设备都有一些配置寄存器，这些寄存器存储设备的特定信息，如设备ID、厂商ID。

2. 地址映射：`RC`负责分配 PCIe 总线上各个设备的地址。这包括内存映射和I/O映射，以确保不同设备之间的数据传输能够正确进行。

3. 中断管理：PCIe设备可能会生成中断信号，`RC`负责管理这些中断，确保它们正确地传递到相应的处理器核心。

4. 通信与数据传输：`RC`与其他 PCIe 设备之间通过 PCIe 链路进行通信。PCIe 链路分为一到多个通道，每个通道包含一个上行和一个下行通道，用于双向数据传输。

![](img/532efab71bda6e01bd4a2340a92f458d.png)

### downstream/upstream port

与`RC`直接或者间接相连的端口称之为上游端口。

在PCIe总线中，`RC`的位置一般在上方，这也是上游端口的由来。

除上游端口之外的其它端口就是下游端口。

![](img/f92b32683d4e9f80effe04b964ed78a8.png)

### Reference Clock

嵌入式时钟的技术（`Embedded Clock`），即发送端不再向接收端发送时钟，但是接收端可以通过`8b/10b`，`128b/130b`的编码从数据`Lane`中恢复出时钟。

![](img/240bc41368bd2311f752e11163792801.png)

### Wide adoption

PCIe可拓展性强，可以支持的设备有：显卡、固态硬盘（PCIe接口形式）、无线网卡、有线网卡、声卡、视频采集卡、PCIe转接M.2接口、PCIe转接USB接口、PCIe转接Tpye-C接口等

![](img/0f80615ae0a84fc912d436cbdc125b7e.png)

# PCIe layout guidelines

https://www.youtube.com/watch?v=kTtE3vQsbsw

`PCIe layout guidelines`

# Control & Data Flow

![](img/ad6e9817667c6504d11aab362758341c.png)

![](img/cfa4c1acaadd17f0811a43dc2009c599.png)

![](img/a4fac8214f228307c951a2e516a0fcf4.png)

![](img/f9e7476de170cd0c6fa90e1eb0e0147e.png)

![](img/e8b678dd2e8931f8eb1602fe7bd27c86.png)

# 协议参数

https://zhuanlan.zhihu.com/p/676301958

## Max payload size

有些`TLP`含有`Data Payload`，如存储器写请求、存储器读完成`TLP`等。`TLP`包含的`Data Payload` 大小与`Max_Payload_Size` 、`Max_Read_Request_Size`和 `RCB` 参数相关。下面将分别介绍这些参数的使用。

PCIe spec规定TLP里的数据有效负载的最大值为4KB，但是PCIe设备并不一定能发送这么大的数据报文。PCIe设备含有`Max_Payload_Size`和`Max_Payload_Size Supported`参数，这两个参数分别在`Device Control`寄存器和`Device Capability`寄存器中定义。

![](img/853b1c3c6272772bfeaec4084e7d9d22.png)

`Max_Payload_Size Supported`参数：存放PCIe设备TLP有效负载的最大值，该参数由PCIe设备的硬件逻辑确定，系统软件不能改写该参数。

而`Max_Payload_Size`参数存放PCIe设备实际使用的TLP有效负载的最大值。该参数由PCIe链路两端的设备协商决定，是PCIe设备进行数据传送时实际使用的参数。

PCIe设备发送数据报文时，使用`Max_Payload_Size`参数决定TLP的最大有效负载。当PCIe设备所传送的数据大小超过`Max_Payload_Size`参数时，这段数据将被分割为多个TLP进行发送。当PCIe设备接收TLP时，该TLP的最大有效负载也不能超过`Max_Payload_Size`参数，如果接收的TLP，其`Length`字段超过`Max_Payload_Size`参数，该PCIe设备将认为该TLP非法。

`RC`或者`EP`在发送存储器读完成TLP时，该TLP的最大`Payload`也不能超过`Max_Payload_Size`参数，如果超过，PCIe设备需要发送多个读完成报文。值得注意的是，这些读完成报文需要满足`RCB`参数的要求，有关`RCB`参数的详细说明见下文。

在实际应用中，尽管有些PCIe设备的`Max_Payload_Size Supported`参数可以为256B、512B、1024B或者更高，但是如果PCIe链路的对端设备可以支持的`Max_Payload_Size`为128B时，系统软件将使用对端设备的`Max_Payload_Size Supported`参数，初始化该设备的`Max_Payload_Size`参数，即选用PCIe链路两端最小的`Max_Payload_Size Supported`参数初始化`Max_Payload_Size`参数。

在多数x86处理器系统的MCH或者ICH中，`Max_Payload_Size Supported`参数为128B。这也意味着在x86处理器中，与MCH或者ICH直接相连的PCIe设备进行DMA读写时，数据的有效负载不能超过128B。而在PowerPC处理器系统中，该参数大多为256B。

目前在大多数`EP`中，`Max_Payload_Size Supported`参数不大于512B，因为在大多数处理器系统的`RC`中，`Max_Payload_Size Supported`也不大于512B。即便`EP`支持较大的`Max_Payload_Size Supported`参数，并不会提高数据传送效率。

`Max_Payload_Size`参数的大小与PCIe链路的传送效率成正比，该参数越大，PCIe链路带宽的利用率越高，该参数越小，PCIe链路带宽的利用率越低。

PCIe总线规范规定，对于实时性要求较高的PCIe设备，`Max_Payload_Size`参数不应设置过大，因此这个参数有时会低于PCIe链路允许使用的最大值。

## `Max_Read_Request_Size`参数

`Max_Read_Request_Size`参数由PCIe设备决定，该参数规定了PCIe设备一次能从目标设备读取多少数据。

`Max_Read_Request_Size`参数在`Device Control`寄存器中定义。该参数与存储器读请求TLP的`Length`字段相关，`Length`字段不能大于`Max_Read_Request_Size`参数。在存储器读请求TLP中，`Length`字段表示需要从目标设备读取多少数据。

值得注意的是，`Max_Read_Request_Size`与`Max_Payload_Size`参数间没有直接联系，`Max_Payload_Size`参数仅与存储器写请求和存储器读完成报文相关。

PCIe总线规定存储器读请求，其读取的数据长度不能超过`Max_Read_Request_Size`参数，即存储器读TLP中的`Length`字段不能大于这个参数。如果一次存储器读操作需要读取的数据范围大于`Max_Read_Request_Size`参数时，该PCIe设备需要向目标设备发送多个存储器读请求TLP。

PCIe总线规定`Max_Read_Request_Size`参数的最大值为4KB，但是系统软件需要根据硬件特性决定该参数的值。因为PCIe总线规定`EP`在进行存储器读请求时，需要具有足够大的缓冲接收来自目标设备的数据。

如果一个`EP`的`Max_Read_Request_Size`参数被设置为4KB，而且这个`EP`每发出一个4KB大小存储器读请求时，`EP`都需要准备一个4KB大小的缓冲。这对于绝大多数`EP`，这都是一个相当苛刻的条件。为此在实际设计中，一个`EP`会对`Max_Read_Request_Size`参数的大小进行限制。

## `RCB`参数

`RCB`位在`Link Control`寄存器中定义。`RCB`位决定了`RCB`参数的值，`RCB`的大小为64B或者128B，如果PCIe设备没有设置`RCB`的大小，则`RC`的`RCB`参数缺省值为64B，而其他PCIe设备的`RCB`参数的缺省值为128B。PCIe总线规定`RC`的`RCB`参数的值为64B或者128B，其他PCIe设备的`RCB`参数为128B。

![](img/3c160469584bd8de1d9daf620d44c9db.png)

![](img/015be9a4679bc2b19c07d909435bc1dc.png)

在PCIe总线中，一个存储器读请求TLP可能收到目标设备发出的多个完成报文后，才能完成一次存储器读操作。因为在PCIe总线中，一个存储器读请求最多可以请求4KB大小的数据报文，而目标设备可能会使用多个存储器读完成TLP才能将数据传递完毕。

当一个`EP`向`RC`或者其他`EP`读取数据时，这个`EP`首先向`RC`或者其他`EP`发送存储器读请求TLP；之后由`RC`或者其他`EP`发送存储器读完成TLP，将数据传递给这个`EP`。

如果存储器读完成报文所传递数据的地址范围没有跨越`RCB`参数的边界，那么数据发送端只能使用一个存储器完成报文将数据传递给请求方，否则可以使用一个或多个存储器读完成TLP。

假定一个`EP`向地址范围为`0xFFFF_0000~0xFFFF_0010`这段区域进行DMA读操作，`RC`收到这个存储器读请求TLP后，将组织存储器读完成TLP，由于这段区域并没有跨越`RCB`边界，因此`RC`只能使用一个存储器读完成TLP完成数据传递。

如果存储器读完成报文所传递数据的地址范围跨越了`RCB`边界，那么数据发送端可以使用一个或者多个完成报文进行数据传递。数据发送端使用多个存储器读完成报文完成数据传递时，需要遵循以下原则。

+ 第一个完成报文所传送的数据，其起始地址与要求的起始地址相同。其结束地址或者为要求的结束地址(使用一个完成报文传递所有数据)，或者为`RCB`参数的整数倍(使用多个完成报文传递数据)。

+ 最后一个完成报文的起始地址或者为要求的起始地址(使用一个完成报文传递所有数据)，或者为`RCB`参数的整数倍(使用多个完成报文传递数据)。其结束地址必须为要求的结束地址。

+ 中间的完成报文的起始地址和结束地址必须为`RCB`参数的整数倍。

当`RC`或者`EP`需要使用多个存储器读完成报文将`0xFFFE_FFF0~0xFFFF_00C7`之间的数据发送给数据请求方时，可以将这些完成报文按照表5‑9方式组织。

表5‑9 存储器读完成报文的拆分方法

上表提供的方式仅供参考，目标设备还可以使用其他拆分方法发送存储器读完成TLP。PCIe总线使用多个完成报文实现一次数据读请求的主要原因是考虑`Cache`行长度和流量控制。在多数x86处理器系统中，存储器读完成报文的数据长度为一个`Cache`行，即一次传送64B。除此之外，较短的数据完成报文占用流量控制的资源较少，而且可以有效避免数据拥塞。

# ROCKm over PCIe

https://rocm.docs.amd.com/en/latest/conceptual/More-about-how-ROCm-uses-PCIe-Atomics.html

uses `PCIe 3.0` features for atomic read-modify-write transactions which extends inter-processor synchronization mechanisms to IO to support the defined set of `HSA` capabilities needed for queuing and signaling memory operations.

The new PCIe atomic operations operate as completers for `CAS` (`Compare and Swap`), `FetchADD`, `SWAP` atomics. The atomic operations are initiated by the I/O device which support `32-bit`, `64-bit` and `128-bit` operand which target address have to be naturally aligned to operation sizes.

Here is how our base address register (`BAR`) works on `GFX 8` GPUs with `40 bit` Physical Address Limit
