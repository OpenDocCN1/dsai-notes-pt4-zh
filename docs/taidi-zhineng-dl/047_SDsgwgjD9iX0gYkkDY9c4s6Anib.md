# CUDA Swizzle & bank conflict

> 来源：[https://learning.feishu.cn/docx/CjoQdWyfDoxDPhxnXRxcZVwbnvg](https://learning.feishu.cn/docx/CjoQdWyfDoxDPhxnXRxcZVwbnvg)

# Summary

掌握要点：

1.  shared memory 的多 bank 存储结构

1.  swizzle 原理 **- **行列 ID 的异或 (XOR)操作

1.  PTX ldmatrix 指令对逻辑空间和存储空间的要求

1.  cute 和 cutlass 的 swizzle 实现

1.  Triton Compiler 的 shared layout & swizzling 实现

简要总结：

1.  bank conflict: 多个 thread 同时访问相同 bank 时，只能串行，latency 增加。（broadcast 例外）

1.  swizzle: 解决 bank conflict 的主流方法。改变 logic address 和 physical address 的 mapping 关系，把相同 Bank 的地址映射到不同的物理地址，但对于用户来说逻辑地址依然为原来的地址。代价是 runtime 的一次 xor 计算。

1.  针对 shared memory: conflict 一定是同一个 warp 内的 thread => 一定发生在 shared memory 上

如果访问的是相同 address，有的硬件或许可以按 broadcast 处理，也不会发生 bank conflict。

但，broadcast 本身，可能也是有额外的代价的？应该不是 zero overhead。

bank conflict 典型场景：

1.  Cuda Core，发生 stride access 时。stride=2，则 2-way bank conflict。

1.  TensorCore。以 tiled 模型从 GMEM 加载到 SMEM 以后，一次 MMA 所需的数据，是 tiled 块中的多行，可能分布在 1 个 bank 上的多行里。

1.  cache line 也会引起 bank conflict。thread block 访问的数据大小，没有对齐 cache line，在 SMEM 中没有 bank conflict，中了 cache 以后，可能在同一个 cache line 的同一个 bank 里面。

主要的解法：

+   padding。

+   转置存储（矩阵乘法的优化措施中，其中的一个典型操作是将从global memory读取的数据转置存储到shared memory中，本文中不做详细解释）。

+   swizzling 机制。

+   random，hash，xxx etc

Thread Block Swizzle。swizzle 的另一个功能，利用局部性原理，重映射block id改变其发射顺序，提高L2 cache命中率

## Cuda Core 的 bank conflict

如果数据访问是连续的 (stride = 1) ，一般没有 bank conflict。

发生 stride access 时。stride=N，则 N-way bank conflict。

但如果是带 stride 的访问，则容易 bank conflict。

stride = 2，容易发生 2-way bank conflict。

stride = 8，就是 8-way bank conflict。

stride = N 意味着，连续 N 个 SRAM bank，只有 1 个会工作，N 个 Thead 的数据，都在这里。


![](img/ba91ed3d7db6744b35ffadfcb7af325b.png)

# 硬件背景

## Bank

Memory 都是多 bank 的。

每个bank都是可以独立寻址的存储空间，bank之间可以并行的读写数据，相互之间不会影响。

Nvidia 的 shared memory 是 32 个 bank，带宽 32bit / bank （4byte）。

每个bank为黑框所包含的单元，用户看到的地址空间为箭头所示的方向，即相邻的4byte占用不同的bank

![](img/74e06a0a0120f942ba68cdb82f3a77dd.png)

## SRAM & DRAM 硬件原理

[内存 - SRAM / DRAM / HBM](https://deep-learning.feishu.cn/wiki/R0cow7MLHiiRpQkXZkEciTD5nQh?fromScene=spaceOverview)

GPU 上的 bank conflict，讨论的一般是 Shared memory。

物理上是 SRAM，与 DRAM 是有区别的。但，从 bank 往上，硬件原理都差不多。

server CPU 上插的，一般是 DIMM，双通道的 2 颗 DDR 为一组 （channel 通道？）。

GPU 上的 HBM，就是 3d 堆叠封装的 DRAM。

如果只看单层的 DRAM，特别是里面的多个 Bank，HBM 和 DDR 没有明显区别。

https://www.youtube.com/playlist?list=PLZU5hLL_713ygweO3b_9KiZUJuEI7I5yK

完整笔记 [SoC 101](https://deep-learning.feishu.cn/wiki/Pb3UwpSK7i62L1kNik7ckzewnJg?fromScene=spaceOverview)

读写 SRAM 和 register 都是 1 个 cycle 的 latency。实际是用了 single clock edge。

但，SRAM 是独占。而 register 可以与 compute 共享一个 cycle。

一个 cycle 可以读写多个 register，但 SRAM 必须是相同地址。（可以用多个 bank 提高 bandwidth）


RISC 里，必须用 load/store 才能访问 memory。在 CISC 里，有 instruction 可以直接访问 SRAM。

GPU 里面，H100 新增的 wgmma 可能是可以直接访问 SRAM 的。

![](img/30595d37e45f659c7dceee5fcef4b2ba.png)


hierarchy 的搞出来了 banks



![](img/a489894ca866e63adabef90e716f66a8.png)

## Bank Conflict

一个 warp 内，如果有 2 个 thread 同时访问同一 bank 的同一行，就会 bank conflict。

如果多个 thread 访问相同的 bank, 但是访问的是 bank 内的同一连续地址空间，也不会有 bank conflict，这种情况下，如果都为读操作，则会广播给访问线程们，如果是写，则只有一个线程的写会成功，具体是哪个线程是未定义行为。

在某些情况下，bank conflict 的发生是在warp中处于同一个phase的不同threads之间，

这里的phase是指warp的32个线程操作shared memory时，分多个phase，每个phase的参与线程不一样。

比如ldmatrix指令 ldmatrix.sync.aligned.x4.m8n8.shared.b16，从共享内存加载数据到线程寄存器，操作分4个phase，在phase0阶段，thread0~7操作共享内存，在phase1阶段，thread8~15操作共享内存，以此类推。

当发生bank conflict时，warp需要额外的一个cycle来重新提交shared memory的访问指令到LSU单元，该指令需要在MIO中排队，这种排队会导致访问延迟增加，此时warp可能处于等待数据返回的状态，warp state标识为Stall Short Scoreboard。如果MIO队列满，此时warp先需要等待MIO队列处于非空的状态，此时warp state标识为Stall MIO Throttle。

# Reducing bank conflict

![](img/105aea11388875ac9370ae85a19c848b.png)

## Padding

Padding: https://developer.nvidia.com/blog/efficient-matrix-transpose-cuda-cc/ Feb 18, 2013

Naive Row-Major Layout


按行访问时，没有 bank conflict。

Padding Row-Major Layout

![](img/1680ce9017c0af177064e9532161bc9b.png)

引入 offset，每一行都 shift 一下。

弊端是，会产生未对齐的地址，从而干扰需要对齐地址的快速指令。

```
half* padding_layout(half* data, int r, int c)
{
    return &amp;(data[r * (columns + 1) + c]);
}
```

思路

![](img/0a0cf3f6f434163aba90bfefd2925872.png)

padding的缺点有：

+   可能降低SM的occupancy。由于每个SM的可使用的shared memory有限，如果每个block使用的共享内存增加，则SM内最大可并发的block数目减少，导致资源不能被充分利用，一些计算资源被闲置。

+   地址访问对齐问题。需要仔细考虑padding的大小来避免地址不对齐的问题，比如访问shared memory时可能是向量化的访问，比如int4访问，也就是每次访问4个int，即16字节，那每次访问的地址必须是16字节对齐的，对于int s_data[32][33]这种padding方式，第二行元素的起始地址就是非16字节对齐，会导致kernel执行出错。

## swizzle 原理

**swizzle** 是在更复杂访问模式下，确保每个线程不会访问到相同的memory bank。

如果矩阵是row-major的且读取是连续的，那么可能无需swizzle操作就能避免bank conflict。

但如果存在交错访问或者更复杂的访问模式，则swizzle是有必要的，用以确保bank conflict free。

大部分的代码中，逻辑位置和物理位置是相同值，而在swizzling机制中，这两者不一样，存在一个映射关系：

(xp,yp)=f(xl,yl)

上面的公式中 (xp,yp) 表示物理存储坐标， (xl,yl) 表示逻辑坐标， x 和 y 取值都是整数。这个映射必须满足：

+   映射是一一对应的关系，即不能是多个逻辑坐标映射到同一个物理坐标或者一个逻辑坐标映射到多个物理坐标，否则就可能存在数据丢失或者重复的问题。

+   映射后的 x 和 y 的取值范围分别与映射前一致，否则可能会导致需要更多的shared memory容量，比如通过乘法映射也能满足一一映射关系，但映射后的值空间远大于映射前的范围。

![](img/b4e0a3de6e07b7d608490bc056f929c1.png)

性质

1.  一对一映射

1.  映射前后的取值范围保持不变

1.  列内任意两行在映射后的 x 值不同

## memory coalesce

在 NVIDIA GPU 的 CUDA 编程模型中，Cacheline 大小（128 Bytes） 和 向量化访问（128-bit, 16 Bytes） 是性能优化中需要理解和结合的两个关键概念

L2 缓存和部分 L1 缓存的 Cacheline 大小是 128 Bytes。

向量化访问，数据结构对齐到 16 Bytes 边界，确保 __align__(16) 属性

memory coalesce 也可以，用 向量化的读写指令。

如以[128bi](https://zhida.zhihu.com/search?content_id=237334060&content_type=Article&match_order=1&q=phase&zhida_source=entity)t的形式读写共享[内存，此时线程需要访问的单位数](https://zhida.zhihu.com/search?content_id=237334060&content_type=Article&match_order=1&q=shared+memory%E7%A9%BA%E9%97%B4&zhida_source=entity)据[量为16byte，32个线程](https://forums.developer.nvidia.com/t/how-to-understand-the-bank-conflict-of-shared-mem/260900)需要访问的数据量为16byte x 32 = 512byte。完整的512byte需要4个phase才能完成访问，第一phase，T0-T7无bank conflict的访问所有bank，第二phase，T8-T15无bank conflict的访问所有bank，第三phase，T16-T23无bank conflict的访问所有bank，第四phase，T24-T31无bank conflict的访问所有的bank。这种情况也可以看作是：shared memory基本单元为16byte，总bank数为8，冲突与否的分析不在是32线程，而变成4个phase中的不同线程。如果采用64bit的访问形式，则相应的基本单元可以看作是8byte，总bank数目为16，冲突与否的条件变成两个phase内的线程是否冲突。整体上shared memory空间可以看作二维存储空间，其中列方向表示bank情况，行方向表示自由定义的大小。值得注意但是冲突与否是通过内存访问事务级别来判定的，具体的可以参考NVidia开发者论坛的讨论。

# Case Study

## Matrix Transpose

有些算法就是要求你既按列读也按行读，比如Matrix Transpose。一般的做法是把Matrix分块，一块块从GMEM搬进SMEM，在SMEM中做transpose，再从SMEM中搬回GMEM。如果按照GMEM中的layout原样搬进SMEM，做行列变换时肯定会有bank conflit的问题。解决的思路有两个：

1.  padding

1.  Swizzling: https://research.colfax-intl.com/tutorial-matrix-transpose-in-cutlass/ 2024

Padding会额外占用更多的内存，所以如果能用swizzling解决，会更优雅。

### naive 实现

基于shared memory进行矩阵（矩阵大小：1028*2048）转置

```
const int M = 1024; //矩阵行
const int N = 2048; //矩阵列
const dim3 block_size(32, 32);
const dim3 grid_size(N/32, M/32);
matrix_trans_shm&lt;&lt;&lt;grid_size, block_size&gt;&gt;&gt;(dev_A, M, N, dev_B);
```

kernel code

```
// 转置前的矩阵存储在dev_A中，矩阵大小为M*N，转置后的数据存储在dev_B中
__global__ void matrix_trans_shm(int* dev_A, int M, int N, int* dev_B) {
  int row = blockIdx.y * blockDim.y + threadIdx.y;
  int col = blockIdx.x * blockDim.x + threadIdx.x;
  
  // 每个block处理32*32的矩阵块
  __shared__ int s_data[32][32];

  if (row &lt; M &amp;&amp; col &lt; N) {
    // 从全局内存中加载数据，转置后写到共享内存中
    s_data[threadIdx.x][threadIdx.y] = dev_A[row * N + col];
    __syncthreads();
    int n_col = blockIdx.y * blockDim.y + threadIdx.x;
    int n_row = blockIdx.x * blockDim.x + threadIdx.y;
    if (n_col &lt; M &amp;&amp; n_row &lt; N) {
      // 从转置后的共享内存按行写到全局内存结果中
      dev_B[n_row * M + n_col] = s_data[threadIdx.y][threadIdx.x];
    }
  }
}
```

上述代码中声明的shared memory数组s_data[32][32]，其中每个元素大小为4字节，刚好对应一个bank，而一行32个元素刚好对应完整的32个bank。数组中每个元素的对应的bank编号如下图所示：

![](img/1f16a4c78bc48fd6423868deaea6c8a0.png)

warp层面的实际的操作是：

+   warp先从global memory读取连续的32个元素（Colased access to global memory）。

+   warp将读取的元素转置后写入到shared memory的同一列中。

可以看到，warp内的32个线程在写数据到shared memory数组中时，对应的同一列，这些地址全部落到相同的一个bank中，也就是32-way bank conflicts。通过profile的数据，可以看到 shared store的bank conflicts达到了2,032,616次。

![](img/ab6c1f301ba0ae951b540575a4efca51.png)

说明：将转置后的数据从shared memory写入到输出内存时，是按行进行的load操作，理论上warp不应该存在bank conflicts，但上面的截图显示仍然有1473次冲突，推测这个load冲突跟我们的这里的读取逻辑无关。

这个2,032,616次冲突的计算如下：

+   每个warp的32个线程，第一个线程的写不会冲突，其他的31个线程的写都会冲突，因此每个warp的bank conflicts的次数为32-1=31。

+   每个block有32个warp，总的block数为(M*N)/(32*32)= (1024*2048)/(32*32) = 2048。

+   所有block累计的bank conflicts为2048(block)*32(warp)*31(conflicts) = 2031616。

类似的wavefronts的数值为2,097,152的计算如下：

+   由于bank 冲突，32个线程的写都需要在MIO中排队，也就是需要分32次访问，每次访问对应一个wavefront(cycle)，如果没有冲突，32个线程只需要在MIO中排队一次，此时耗费的wavefronts数为1。

+   所有block累计的wavefronts = 2048(block)*32(warp)*32(wavefronts) = 2097152。

关于wavefront的概念：

A wavefront is the maximum unit that can pass through that pipeline stage per cycle. If not all cache lines or sectors can be accessed in a single wavefront, multiple wavefronts are created and sent for processing one by one, i.e. in a serialized manner.

A wavefront is described as a (work) package that can be processed at once, i.e. there is a notion of processing one wavefront per cycle in L1TEX. Wavefronts therefore represent the number of cycles required to process the requests, while the number of sectors per request is a property of the access pattern of the memory instruction for all participating threads. For example, it is possible to have a memory instruction that requires 4 sectors per request in 1 wavefront. However, you can also have memory instruction having 4 sectors per request, but requiring 2 or more wavefronts.

### 使用 Padding

思路

![](img/6e78d430a888ef874bc9e309428bac24.png)

coding

```
__global__ void matrix_trans_shm_padding(int* dev_A, int M, int N, int* dev_B) {
  int row = blockIdx.y * blockDim.y + threadIdx.y;
  int col = blockIdx.x * blockDim.x + threadIdx.x;

  // 每个block处理32*32的矩阵块，尾部padding来避免bank conflict
  __shared__ int s_data[32][33];

  if (row &lt; M &amp;&amp; col &lt; N) {
    s_data[threadIdx.x][threadIdx.y] = dev_A[row * N + col];
    __syncthreads();
    int n_col = blockIdx.y * blockDim.y + threadIdx.x;
    int n_row = blockIdx.x * blockDim.x + threadIdx.y;
    if (n_col &lt; M &amp;&amp; n_row &lt; N) {
      dev_B[n_row * M + n_col] = s_data[threadIdx.y][threadIdx.x];
    }
  }
}
```

profile的结果如下图所示，可以看出shared store的bank conflicts的数目变为0

![](img/3ac3f73979cbf6960babab417ea8cef5.png)

padding的缺点有：

+   可能降低SM的occupancy。由于每个SM的可使用的shared memory有限，如果每个block使用的共享内存增加，则SM内最大可并发的block数目减少，导致资源不能被充分利用，一些计算资源被闲置。

+   地址访问对齐问题。需要仔细考虑padding的大小来避免地址不对齐的问题，比如访问shared memory时可能是向量化的访问，比如int4访问，也就是每次访问4个int，即16字节，那每次访问的地址必须是16字节对齐的，对于int s_data[32][33]这种padding方式，第二行元素的起始地址就是非16字节对齐，会导致kernel执行出错。

### swizzling 实现

swizzling是在不额外分配内存的情况下，通过将shared memory的数据进行重排来避免bank conflict。下面是重排后的一个示例：

![](img/83edb3820c51e21b008c7b5a74bd2c79.png)

在上面的图中，每个元素有唯一的位置信息 (x,y) ， x 和 y 分别表示列和行，并且假设从0开始。

重排后，每一行内的数据集合保持不变，但行内元素的相对位置进行重排，比如对于 y=1 ，x原来的索引顺序是[0, 1, 2, 3, ......, 30, 31]，而新的索引顺序是[1, 0, 3, 2, 5, 4, ......, 31, 30]。

从上图可以看到，每一行/列的各个元素的bank值不再一样，这样就避免了shared memory在load/store操作时的bank冲突。

```
__global__ void matrix_trans_swizzling(int* dev_A, int M, int N, int* dev_B) {
  int row = blockIdx.y * blockDim.y + threadIdx.y;
  int col = blockIdx.x * blockDim.x + threadIdx.x;

  __shared__ int s_data[32][32];

  if (row &lt; M &amp;&amp; col &lt; N) {
    // 从全局内存读取数据写入共享内存的逻辑坐标(row=x,col=y)
    // 其映射的物理存储位置位置(row=x,col=x^y)
    s_data[threadIdx.x][threadIdx.x ^ threadIdx.y] = dev_A[row * N + col];
    __syncthreads();
    int n_col = blockIdx.y * blockDim.y + threadIdx.x;
    int n_row = blockIdx.x * blockDim.x + threadIdx.y;
    if (n_row &lt; N &amp;&amp; n_col &lt; M) {
      // 从共享内存的逻辑坐标(row=y,col=x)读取数据
      // 其映射的物理存储位置(row=y,col=x^y)
      dev_B[n_row * M + n_col] = s_data[threadIdx.y][threadIdx.x ^ threadIdx.y];
    }
  }
}
```

代码中关于使用swizzling机制的代码逻辑如下：

+   块中的每个线程从global memory读取一个元素后，在转置前，对应shared memory的逻辑为坐标(x = threadIdx.x, y = threadIdx.y)，进行转置后存储的逻辑坐标为(x = threadIdx.y, y = threadIdx.x)。

+   对于一个warp内的32个线程，threadIdx.y值一样，threadIdx.x不同，导致存储到shared memory中的位置是相同列不同行中，从而导致bank conflict。

+   此时，应用swizzling进行映射后的，转置存储时，行保持不变仍然为threadIdx.x，新的列为 threadIdx.x ^ threadIdx.y。

+   读取时采用类似的机制，将逻辑坐标转换为物理存储坐标，再从shared memory的对应物理位置去读。

Profile 结果如下：

![](img/8841f7b3ba7653cfdc8b8ee321e67d1c.png)

图中显示shared memory store时bank conflicts=0，但load时还有bank conflict，推测跟swizzling无关。

## MMA Bank Conflict

为了兼容 Tensor Core 的 Warp Level 计算属性，

从 Shared Memory加载的数据是以 block 的形式进行操作的，

并且该Warp Level Memory块中可能有多个 sub block 是在同一个Bank上，

这个与mma.sync的指令一般是m16n8k16类型的2的幂次方形状有关，

由于Shared Memory是有限的所以Shared Memory重用率非常高，导致如果出现Bank Conflict后，可能会极大的影响Kernel的性能。

![](img/a956e73d9795004aa085f1497b28cc6d.png)

选用 32 x 64 的Block大小，数据类型为half类型。

Naive Row-Major Layout


按行访问时，没有 bank conflict。

但，按 TensorCore 场景，按 tile / block 加载时，就要访问一个 bank 的多行了。

```
half* naive_layout(half* data, int r, int c) 
{
    return &amp;(data[r * columns + c]);
}
```

Padding Row-Major Layout

![](img/14f1776ab37cf851528a19b6af6bf133.png)

引入 offset，每一行都 shift 一下。

弊端是，会产生未对齐的地址，从而干扰需要对齐地址的快速指令。

```
half* padding_layout(half* data, int r, int c)
{
    return &amp;(data[r * (columns + 1) + c]);
}
```

Naive Swizzled Layout

![](img/b69bc1404ebc6d222631e141003b66f5.png)

“S[wiz](https://zhida.zhihu.com/search?content_id=701901940&content_type=Answer&match_order=1&q=TMA&zhida_source=entity)zle”内存，其中渐进的行被重新洗牌以改变Banking的排列方式。这种布局通过将索引与行进行异或来实现这一点，从而消除了存储体冲突并对齐了地址。然而，这种布局缺乏对 HGMMA 和 TMA 指令的硬件支持，这对于 H100 GPU 实现高性能尤为重要。

```
half* row_swizzled_layout(half* data, int r, int c)
{
    auto address = (uint64_t)&amp;data[r * columns + c];
    return reinterpret_cast&lt;half *&gt;(address ^ (r &lt;&lt; 2));
}
```

[矩阵计算](https://zhida.zhihu.com/search?content_id=237334060&content_type=Article&match_order=1&q=%E7%9F%A9%E9%98%B5%E8%AE%A1%E7%AE%97&zhida_source=entity)中的数据加载是从全局内存到共享内存然后到寄存器。共享内存作为中间的媒介可以[减少矩](https://zhida.zhihu.com/search?content_id=237334060&content_type=Article&match_order=1&q=%E5%B9%B6%E8%A1%8C%E6%80%A7&zhida_source=entity)阵计算时对全局内存的访问数据量，从而提升计算访存比。共享内存为了提升访问的并行性采用多bank结构，这也造成了编程时的困难，cute通过提供swizzle抽象简化了逻辑空间和多bank存储空间的映射的复杂度。

最早是 WMMA (warp level MMA)

Hopper 引入了 WGMMA (Warpgroup MMA)，可以直接读取 SMEM 中的数据进行计算。不用经过 register file。

wmma 封装了 Shared Memory -> MMA -> Shared Memory这一层的过程。

包括 matrix A/B/C 的 fragment、fragment 的加载（load_matrix_sync），

结果就是很难规避 shared memory bank conflict。

因为，丧失了一些灵活性，例如想做swizzle，改变shared memory -> mma这部分的layout，或者做一些fuse，把elementwise的op直接fuse到mma累加完的寄存上，WMMA就不太好做了。

sm80 以上，Hopper(sm90) 之前，mma.sync 使用较多，且，可以达到更好的性能

wmma这个由每个线程去组织数据时比较令人头疼，而针对mma，会用ldmatrix这条指令就够了，所以尽管mma是通过一个warp去调度实现的，但是还是粒度为线程级别时更容易编写。

假设用TensorCore直接计算一个16x8x16xfp16大小的矩阵乘法，针对从shared memory加载到register file然后送进mma进行计算的过程，用PTX指令大致描述如下：

```
// [load A f](https://zhida.zhihu.com/search?content_id=245984318&amp;content_type=Article&amp;match_order=2&amp;q=ldmatrix&amp;zhida_source=entity)rom shared memory to register file, d0/1/2/3 is b32 register
ldmatrix.sync.aligned.m8n8.x4.shared.b16 {d0, d1, d2, d3}, [a_addr];
// load B from shared memory to register file, d4/5 is b32 register
ldmatrix.sync.aligned.m8n8.x2.trans.shared.b16 {d4, d5}, [b_addr]; 

// d = a x b + c
mma.sync.aligned.m16n8k16.row.col.f32.bf16.bf16.f32 d, a, b, c;
```

ldmatrix指令以一个warp的形式负责从A、B矩阵shared memory中加载mma指令需要大小的数据（分别为A——16x8x16xfp16 with row-marjor、B——16x8xfp16 with col-major），然后交给mma指令进行计算。

对于矩阵A，按照[ISA](https://zhida.zhihu.com/search?content_id=245984318&content_type=Article&match_order=1&q=ISA&zhida_source=entity)的描述，ldmatrix负责读取4个8x8xfp16矩阵，为了用一[个warp](https://zhida.zhihu.com/search?content_id=245984318&content_type=Article&match_order=1&q=phase&zhida_source=entity)完成这个过程，这里需要分成4个phase，其中T0-7为phase 0、T8-15为phase 1、T16-23为phase 2、T24-31为phase 4，每个phase读取一个8x8xfp16矩阵，每个phase（8个线程）中的每个线程负责读取一行的8xfp16=16bytes=128bit（vector）=4bank数据。注意，这一行的4bank数据必须连续，每个线程之间的4bank数据则不要求连续，满足对齐要求即可。因此，这里对bank冲突的讨论将从一个warp变成一个phase（8个线程），只需要保证这8个线程读取的时候不会发生bank冲突即可。

可以发现，每个线程将自己读取到的4bank数据，拆成4份，然后发给指定的线程，例如T0 4个32bit寄存器（d0、d1、d2、d3）将会收到T0、T8、T16、T24的第一份数据，依次类推。一张图显示ldmatrix指令的加载过程：

## Ampere - ldmatrix 指令

从 SMEM 到 register 加载数据。cache line 会导致 bank conflict

一个线程会加载 8 个 fp16 数据并将其分散到寄存器文件的四个角上面

![](img/47644f384c8bf2f095a7b6692a9bb923.png)

当 8 个线程对第一个 8×8 的矩阵块进行ldmatrix将会造成 Bank Conflict。

如下图所示，一个 16×16 的矩阵块在物理角度看来是一个 32×8 的块，其中一行为一个 Cache line 的大小。因此当 T0 与 T4 进行数据访问的时候将会发生 Bank Conflict，同理 T1 与 T5，T2 与 T6，T3 与 T7 也是一样的。

因此，为了避免 Bank Conflict 的情况，需要避免 8 个线程对相同的 Bank 进行访问的情况。

在上面我们了解到 Bank Conflict 总是发生在一个事务内，因此接下来我们只需考虑 T0-T7 这个过程中的 Bank Conflict 的问题即可，而不必关注整个Warp 内的所有线程，同时 T8-T15，T16-T23，T24-T31 同理。

![](img/851bdc44226b8421a95ba9d500ccff3a.png)

一种可[行的方法是使用 Pa](https://zhida.zhihu.com/search?content_id=703022015&content_type=Answer&match_order=5&q=Cache+line&zhida_source=entity)dding，通过向内存中填充无用的空白的数据来使得内存错位，从而避免 Bank Conflict 的问题。例如，下图中通过向共享内存中第一行 Cache line 末尾插入了一段空白的内存，在这种情况下，原本又 T19 访问的数据被转移到了下一个 Cache line 中，从而使得 T0/T4，T1/T5，T2/T6，T3/T7 能够在不同 Bank 进行访问，避免了 Bank Conflict。然而，Padding 的缺点在于会过多占用内存，同时也会产生未对齐地址。

![](img/25e22a95625467db4af0271b02a9975d.png)

## Hopper TMA 的 swizzling

Nvidia hopper中的tensorcore在做矩阵乘法计算时，支持从SMEM中直接读取A矩阵和B矩阵，对于不同shape的输入，有可能会涉及到需要做特殊的32-byte/64-byte/128-byte swizzling

https://docs.nvidia.com/cuda/parallel-thread-execution/#asynchronous-warpgroup-level-matrix-shared-memory-layout-swizzling-modes

矩阵乘法中，为了减少内存访问指令数目，通常通过向量化的访问指令来实现，比如int2/int4，float2/float4等，即一次访问8字节或16个字节。这种情况下，数据chunk的大小就不是4字节，跟单个bank的大小就不一样，此时仍然有bank冲突的现象存在，只是发生冲突的bank号不是连续分布，而是间隔分布，比如bank为0，4，8等等。为了避免冲突，可以按chunk为单位进行swizzling，其核心的处理思想仍然是使用异或操作，下图是一个示例：

![](img/cfc127970a9143cbf384ab1b6cf23533.png)

下面说明TMA中进行swizzling的处理过程：

+   TMA以128字节单位为1个segment，以16字节为1个chunk。

+   通过对segment内的chunk进行swizzling，来实现不同segment间的bank conflict free。

TMA 假设使用的shared memory的数组的形式是T array[][NX]（T可以是1/2/4Bytes的数据类型），并且满足：

+   NX * sizeof(T) == SWIZZLE_SIZE。

+   SWIZZLE_SIZE 必须是32，64或者128三个值之一。

下面的计算过程假设SWIZZLE_SIZE=128字节，对于数组中逻辑坐标是[y][x]的元素，通过TMA映射后，在物理存储数组中的坐标为[y_swz][x_swz]。跟之前的映射类似，物理坐标y_swz与逻辑坐标y保持一致，x_swz的计算过程如下：

+   计算16字节的chunk块的索引

+   i16 = (y * NX + x) * sizeof(T) / 16。

+   y16 = i16 / 8。

+   x16 = i16 % 8。

+   计算这16字节的chunk块的swizzling的索引

+   y16_swz = y16，即chunk块的行保持不变。

+   x16_swz = y16^x16，即chunk块的列为逻辑行和逻辑列的异或值。

+   计算chunk块的某个x元素的映射后的索引

+   先计算chunk块在行内的偏移，然后计算chunk块中的元素在chunk块中的偏移，两者之和即为偏移。

+   x_swz = x16_swz * 16 / sizeof(T) % NX + x % (16 / sizeof(T))。

对于不同的SWIZZLE_SIZE进行坐标重排后的其效果如下：

![](img/cd955e630c18643fa9999c0fefe1cc1a.png)

由于SWIZZLE_SIZE为128字节时，刚好对应了32个bank的大小，此时在chunk层面的进行的swizzling映射不会改变chunk所在的行，只会改变列。如果SWIZZLE_SIZE不为128字节时，映射后有可能改变行，比如对于float4 array[15][4]这样的数组，SWIZZLING_SIZE = 64字节，对于数组中超过第8行的那些数据，第8行后的chunk块的前4个和后4个的相对位置已经发生变化，如下图所示：

![](img/0004f907d28dc7b442ec67f03d45917b.png)

对SWIZZLING_SIZE不为128字节的情况，chunk的处理逻辑与上面的处理过程类似，但从chunk到单个元素的转换时，需要做些调整。

## swizzle in CUTLASS

swizzle 原理：通过重映射的方式，能够将本来在相同 Bank 的内存映射到不同的物理地址，但对于用户来说逻辑地址依然为原来的地址。

通用的 Swizzle 计算方法，定义了 B，M，S 三个符号并通过一系列变换公式来对 Layout 进行重映射。

+   M 表示为一个 Swizzle 重映射的最小单元，在 2^M × 16 bits 的数据中的所有数据之间的顺序不会产生变化，这是基于 NVIDIA 中向量化访问考虑的，一次向量化访问最大可访问 128 bits。

+   S 表示基于 M 构成的进行一次内存重映射的元素个数，表示对 2S2^S 个块在一行内进行重排序。

+   B 表示构成整个 Swizzle 二维空间的行数。

例如 Swizzle<B, M, S> = Swizzle<3, 3, 3> 表示每 128 bits 组成一个块，一行有 8 个块，共有 8 行。

当规定完成后，即可对共享内存中二维数组的编号进行重映射，在 Swizzle 中采用行列坐标进行异或的方式进行物理内存重映射。

![](img/15b3510a3fb192a678c97a368ef99c05.png)

经过 Swizzle 后的内存映射如下图所示，此时可以见 T0/T4, T1/T5, T2/T6, T3/T7 之间的内存访问完全把 Bank 分开进行访问，此时前八个线程的内存访问达到了 Bank Conflict free。其他线程的访问同理可以避免 Bank Conflict，在这种情况下 Swizzle 可以实现 Bank Conflict free。在此时尽管对于物理地址的访问发生了变化，但对于用户来说逻辑地址依然是不变的。


作为对比，padding 后的效果

![](img/c8c6c04c0f1f8b1bdf6e266b5484a9be.png)

原始排布

![](img/20585b7c4aa0ee631cdd4071267427aa.png)

Cutlass 使用tensor core的进行矩阵乘法时，比如对于m16n8k16的fp16的矩阵乘法，需要分4个phase从shared memory分别加载4个8x8的fp16的矩阵数据到线程寄存器中，此时的bank conflict只局限同phase的8个线程内，可以只考虑针对这8个线程需要访问的shared memory来进行swizzling，从而避免bank conflict，感兴趣的读者可以去找资料学习。

![](img/13b81d1de1a6e6462388d0f9a6cb9168.png)

# PTX - Swizzling Modes

SMEM 中的数据，需要考虑 Swizzling 问题。几种 mode

1.  No swizzle mode

1.  32 byte swizzle mode

1.  64 byte swizzle mode

1.  128 byte swizzle mode

![](img/8c3f5a0709659344fc63b13f9edfee14.png)

## 32 byte swizzle mode

256/16 = 16

![](img/5c5c01082f5b01146fd396a5a51d8400.png)

![](img/698bd78248a3630b09bf8960f367b019.png)

![](img/8a3bf0c6b0e7b5b64ef62eae7f31ac8b.png)

figure 13 的 fragment，也就是 source layout。

![](img/a7ca61d0c169a893f938dff10d73b685.png)

![](img/be5ca27433d1d3cc3f2bfa80bd300eae.png)

## 64 byte swizzle mode

![](img/0441559892851b602d9b051d5539f93e.png)

![](img/398068a8a90e39bb29be2f381cf34fa1.png)

![](img/eb64a3d3d14159abad864ce5c6f257e9.png)

![](img/80fa02177b0cc99a2b73774719b8e060.png)

## 128 byte swizzle mode

![](img/fc3a84c4dc557893fbfe0e93d8dba5b4.png)

![](img/120091217427fddd273ecfd4ca2aff25.png)

![](img/f3e68ebdc32d4ea6f5ff3b14b4d7b5a1.png)

![](img/811062ecb55d602a7a944f1724716a01.png)

![](img/af2ed6090bb9226083fa5421b887ef3e.png)

# Triton - Shared Layout swizzling

多个 different cuda threads 可能 同时访问 的 tensor data。

GPGPU 上，一定发生在 shared memory 上。

核心是研究 swizzle 问题，以避免 bank conflicts 问题。

In other words,

for all indices  ,  = {0, 1, ..., 32*num_warps - 1}

In order to avoid shared memory bank conflicts, elements may be swizzled.

Here are some examples.  In all cases, the input tensor is [0, 1, ..., n-1].

## Basic swizzling

#shared<{vec=1, perPhase=1, maxPhase=4, order=[1,0]}>

[ 0,  1,  2,  3],  // xor with 0

[ 5,  4,  7,  6],  // xor with 1

[10, 11,  8,  9],  // xor with 2

[15, 14, 13, 12]   // xor with 3

Here elements of row r are xor'ed with r (or more properly, in[r][c] -> out[r][c^r]).

## Multiple rows per phase

#shared<{vec=1, perPhase=2, maxPhase=4, order=[1,0]}>

[ 0,  1,  2,  3],  // phase 0 (xor with 0)

[ 4,  5,  6,  7],

[ 9,  8, 11, 10],  // phase 1 (xor with 1)

[13, 12, 15, 14]

Elements of row r are xor'ed with r/2.

In other words, perPhase=2 means that pairs of 2 rows get the same swizzling.

## Max-phase applied

$shared<{vec=1, perPhase=1, maxPhase=2, order=[1,0]}>

[ 0,  1,  2,  3],  // phase 0 (xor with 0)

[ 5,  4,  7,  6],  // phase 1 (xor with 1)

[ 8,  9, 10, 11],  // phase 0

[13, 12, 15, 14],  // phase 1

[16, 17, 18, 19],  // ...

[21, 20, 23, 22],

[24, 25, 26, 27],

[29, 28, 31, 30]

Elements of row r are xor'ed with (r/2) % 2.  In other words, maxPhase=m has the

effect of limiting the maximum value of the xor to m-1.

## Max-phase and per-phase

#shared<{vec=1, perPhase=2, maxPhase=2, order=[1,0]}>

[ 0,  1,  2,  3],  // phase 0 (xor with 0)

[ 4,  5,  6,  7],  // phase 0

[ 9,  8, 11, 10],  // phase 1 (xor with 1)

[13, 12, 15, 14],  // phase 1

[16, 17, 18, 19],  // phase 0

[20, 21, 22, 23],  // phase 0

[25, 24, 27, 26],  // phase 1

[29, 28, 31, 30]]  // phase 1

Here the xor value (the "phase", I guess?) changes every perPhase rows, up to a

maximum value of maxPhase-1.  In other words, elements of row r are xor'ed with

(r/2) % 2.

## Adding vec

#shared<{vec=2, perPhase=1, maxPhase=4, order=[1,0]}>

[ 0,  1,  2,  3,  4,  5,  6,  7],

[10, 11,  8,  9, 14, 15, 12, 13],

[20, 21, 22, 23, 16, 17, 18, 19],

[30, 31, 28, 29, 26, 27, 24, 25]

When vec=2, elements are swizzled in pairs of 2.  In other words, the element at

(r,c) has value

((c / 2) ^ r) * 2 + (c % 2).

For MMAv3 eg Hopper GMMA, hasLeadingOffset should be true. In this case,

when the matrix is stored in shared memory, there will be an offset not

only in the stride dimension, but also in the leading dimension. For example,

a matrix of size 16x128 and data type I8 is stored in the shared memory with

64B-swizzle mode. The offset of the element with index (0, 64) will be 16*64,

compared to 1*64 when the hasLeadingOffset is false.

# reference

cutlass 的 swizzle 实现，500 行代码。

https://github.com/NVIDIA/cutlass/blob/main/include/cute/swizzle.hpp

PTX ISA v8.5, PDF Page 59

https://docs.nvidia.com/cuda/parallel-thread-execution/

cute 之 Swizzle - reed

https://zhuanlan.zhihu.com/p/671419093

How to understand the bank conflict of shared_mem

https://forums.developer.nvidia.com/t/how-to-understand-the-bank-conflict-of-shared-mem/260900

皓月争曦 - cuda的swizzle是怎么实现bank conflict free的？

https://www.zhihu.com/question/667972067/answer/43935974172

KuangjuX - cuda的swizzle是怎么实现bank conflict free的？

https://www.zhihu.com/question/667972067/answer/49533645340

CUDA shared memory避免bank conflict的swizzling机制解析

https://zhuanlan.zhihu.com/p/4746910252

https://docs.nvidia.com/cuda/parallel-thread-execution/#asynchronous-warpgroup-level-matrix-shared-memory-layout-swizzling-modes

GTC 2024

https://www.nvidia.com/en-us/on-demand/session/gtc24-s62192/

![](img/c223e04cc585b50f93a42e4c2e118bf7.png)