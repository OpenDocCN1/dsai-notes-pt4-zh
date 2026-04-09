# SIMD Processing

> 来源：[https://learning.feishu.cn/docx/CnUSdCCk1oqFTGxa3Zhcz2GnnKr](https://learning.feishu.cn/docx/CnUSdCCk1oqFTGxa3Zhcz2GnnKr)

| Links |
| --- |
| https://www.youtube.com/watch?v=XaW_O9nKPe0 |
| https://www.youtube.com/watch?v=P0SEyu0bYO8 |
| https://www.youtube.com/watch?v=P0SEyu0bYO8 |

如果有机会在工作中研究 SIMD 和 GPGPU 的统一架构，很不错。

## SIMD 的 3 个流派

SISD, SIMD, MISD, MIMD

SIMD 的 3 个主要流派：

1.  Vector Processor ( pipeline processor ), 每个 unit 在时间维度上，做相同 instruction 的不同 data。

1.  SIMD extension: MMX, AVX, SSE 等。

1.  GPGPU (SIMT): 每个 thread，每个 cycle 做相同 instruction。

说明：

1.  Array Processor 和 Vector Processor，比较抽象，但有助于 uArch 的融会贯通。

1.  GPGPU 在 warp level 是 FGMT (Fine-Gained Multithreading)。FGMT 很重要。

1.  理解 GPGPU 时，注意区分 programming 和 hardware model。

1.  此处都是 Regular  (Data) Parallelism。非规则的，用 gather / scatter 预处理下。

## Vector Processor - TODO

[Vector Processor](https://dwpl6xgouw.feishu.cn/wiki/IFStwfZYrig8XfkjCPqcg8tAnHg?fromScene=spaceOverview)

# SIMD Processing

背景介绍: [Flynn's Taxonomy 费林分类法](https://dwpl6xgouw.feishu.cn/wiki/CQA8w0fL9ip30Kk6HawcdnipnVd?fromScene=spaceOverview)

与 MIMD ( multi-core )  比，SIMD 的 instruction 依旧是 sequential 的，更符合开发者的直觉，易于 debug。

提到向量 SIMD 处理器时，需要注意其流行普及的两类关键技术：

1.  操作数重组（component swizzling）：能够将源运算对象的单个操作数重定向到单个处理单元，类似地，将单个输出操作数重定向到目标操作数

1.  操作数屏蔽（component masking）：能够丢弃单个输出操作数（或类似地禁用单个处理单元）

## Data Parallelism

SIMD 是 same operator on different(multiple) data。不需要通过 multi-thread 就可以。

Dataflow 是 **different** operations in parallel。

thread parallelism 是 different thread in parallel。

multiple data elements, 可以是 in time or in space。

对应的，是 array processor 或 vector processor。

需要 vector registers 存数据。

![](img/a173d819516aa2f0e2d0ee9e0bbeec7a.png)

SIMD v.s. VLIW

+   SIMD 是 same operation。

+   VLIW 是 multiple operation pack 在一起。

## SIMD ISA Extensions

![](img/6c695f1c4ba24f1be26bc2f4693a0d5f.png)

### Intel Pentium MMX Operations

![](img/37b2fc99dc1ff495961b3c08a4361e16.png)

![](img/97f9563b2ab009f3877193f13d3f6043.png)

![](img/9d7eecb86f22518456d1d878b969aded.png)

![](img/ff6fad03fbdec4af7bfad9b8b768c2aa.png)

### SIMD in ML accelerator

![](img/4039b0766dc8eee98fb6e3533932fe8a.png)

![](img/814035eece6e412194f527921ef83dd9.png)

### MIMD Machine with SIMD Processors

bottom-up: Tile -> Die -> Wafer

每一个 Tile 是就一个 SIMD processor。

![](img/5270ea65807eea6b5c1202dff0ac942f.png)

4-wary 的 SIMD processor。

![](img/f4c152f8da43c76f0620b787dec3aa13.png)