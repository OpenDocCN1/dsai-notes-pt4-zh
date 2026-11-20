
# 共享内存 Bank Conflict 与 Swizzling 技术详解

## 从 SMEM 到 Register 加载数据与 Bank Conflict

从 `SMEM` 到 `register` 加载数据。`cache line` 会导致 bank conflict

一个线程会加载 8 个 `fp16` 数据并将其分散到寄存器文件的四个角上面

![](img/47644f384c8bf2f095a7b6692a9bb923.png)

当 8 个线程对第一个 8×8 的矩阵块进行 `ldmatrix` 将会造成 Bank Conflict。

如下图所示，一个 16×16 的矩阵块在物理角度看来是一个 32×8 的块，其中一行为一个 `cache line` 的大小。因此当 `T0` 与 `T4` 进行数据访问的时候将会发生 Bank Conflict，同理 `T1` 与 `T5`，`T2` 与 `T6`，`T3` 与 `T7` 也是一样的。

因此，为了避免 Bank Conflict 的情况，需要避免 8 个线程对相同的 Bank 进行访问的情况。

在上面我们了解到 Bank Conflict 总是发生在一个事务内，因此接下来我们只需考虑 `T0`-`T7` 这个过程中的 Bank Conflict 的问题即可，而不必关注整个 `Warp` 内的所有线程，同时 `T8`-`T15`，`T16`-`T23`，`T24`-`T31` 同理。

![](img/851bdc44226b8421a95ba9d500ccff3a.png)

一种可[行的方法是使用 Pa](https://zhida.zhihu.com/search?content_id=703022015&content_type=Answer&match_order=5&q=Cache+line&zhida_source=entity)dding，通过向内存中填充无用的空白的数据来使得内存错位，从而避免 Bank Conflict 的问题。例如，下图中通过向共享内存中第一行 `cache line` 末尾插入了一段空白的内存，在这种情况下，原本又 `T19` 访问的数据被转移到了下一个 `cache line` 中，从而使得 `T0`/`T4`，`T1`/`T5`，`T2`/`T6`，`T3`/`T7` 能够在不同 Bank 进行访问，避免了 Bank Conflict。然而，`Padding` 的缺点在于会过多占用内存，同时也会产生未对齐地址。

![](img/25e22a95625467db4af0271b02a9975d.png)

## Hopper TMA 的 swizzling

Nvidia hopper 中的 `tensorcore` 在做矩阵乘法计算时，支持从 `SMEM` 中直接读取 `A` 矩阵和 `B` 矩阵，对于不同 shape 的输入，有可能会涉及到需要做特殊的 32-byte/64-byte/128-byte swizzling

https://docs.nvidia.com/cuda/parallel-thread-execution/#asynchronous-warpgroup-level-matrix-shared-memory-layout-swizzling-modes

矩阵乘法中，为了减少内存访问指令数目，通常通过向量化的访问指令来实现，比如 `int2`/`int4`，`float2`/`float4` 等，即一次访问 8 字节或 16 个字节。这种情况下，数据 chunk 的大小就不是 4 字节，跟单个 bank 的大小就不一样，此时仍然有 bank 冲突的现象存在，只是发生冲突的 bank 号不是连续分布，而是间隔分布，比如 bank 为 0，4，8 等等。为了避免冲突，可以按 chunk 为单位进行 swizzling，其核心的处理思想仍然是使用异或操作，下图是一个示例：

![](img/cfc127970a9143cbf384ab1b6cf23533.png)

下面说明 TMA 中进行 swizzling 的处理过程：

+   TMA 以 128 字节单位为 1 个 segment，以 16 字节为 1 个 chunk。

+   通过对 segment 内的 chunk 进行 swizzling，来实现不同 segment 间的 bank conflict free。

TMA 假设使用的 shared memory 的数组的形式是 `T array[][NX]`（`T` 可以是 1/2/4 Bytes 的数据类型），并且满足：

+   `NX * sizeof(T) == SWIZZLE_SIZE`。

+   `SWIZZLE_SIZE` 必须是 32，64 或者 128 三个值之一。

下面的计算过程假设 `SWIZZLE_SIZE`=128 字节，对于数组中逻辑坐标是 `[y][x]` 的元素，通过 TMA 映射后，在物理存储数组中的坐标为 `[y_swz][x_swz]`。跟之前的映射类似，物理坐标 `y_swz` 与逻辑坐标 `y` 保持一致，`x_swz` 的计算过程如下：

+   计算 16 字节的 chunk 块的索引

+   `i16 = (y * NX + x) * sizeof(T) / 16`。

+   `y16 = i16 / 8`。

+   `x16 = i16 % 8`。

+   计算这 16 字节的 chunk 块的 swizzling 的索引

+   `y16_swz = y16`，即 chunk 块的行保持不变。

+   `x16_swz = y16^x16`，即 chunk 块的列为逻辑行和逻辑列的异或值。

+   计算 chunk 块的某个 `x` 元素的映射后的索引

+   先计算 chunk 块在行内的偏移，然后计算 chunk 块中的元素在 chunk 块中的偏移，两者之和即为偏移。

+   `x_swz = x16_swz * 16 / sizeof(T) % NX + x % (16 / sizeof(T))`。

对于不同的 `SWIZZLE_SIZE` 进行坐标重排后的其效果如下：

![](img/cd955e630c18643fa9999c0fefe1cc1a.png)

由于 `SWIZZLE_SIZE` 为 128 字节时，刚好对应了 32 个 bank 的大小，此时在 chunk 层面的进行的 swizzling 映射不会改变 chunk 所在的行，只会改变列。如果 `SWIZZLE_SIZE` 不为 128 字节时，映射后有可能改变行，比如对于 `float4 array[15][4]` 这样的数组，`SWIZZLING_SIZE` = 64 字节，对于数组中超过第 8 行的那些数据，第 8 行后的 chunk 块的前 4 个和后 4 个的相对位置已经发生变化，如下图所示：

![](img/0004f907d28dc7b442ec67f03d45917b.png)

对 `SWIZZLING_SIZE` 不为 128 字节的情况，chunk 的处理逻辑与上面的处理过程类似，但从 chunk 到单个元素的转换时，需要做些调整。

## swizzle in CUTLASS

swizzle 原理：通过重映射的方式，能够将本来在相同 Bank 的内存映射到不同的物理地址，但对于用户来说逻辑地址依然为原来的地址。

通用的 Swizzle 计算方法，定义了 `B`，`M`，`S` 三个符号并通过一系列变换公式来对 Layout 进行重映射。

+   `M` 表示为一个 Swizzle 重映射的最小单元，在 `2^M × 16 bits` 的数据中的所有数据之间的顺序不会产生变化，这是基于 NVIDIA 中向量化访问考虑的，一次向量化访问最大可访问 128 bits。

+   `S` 表示基于 `M` 构成的进行一次内存重映射的元素个数，表示对 `2^S` 个块在一行内进行重排序。

+   `B` 表示构成整个 Swizzle 二维空间的行数。

例如 `Swizzle<B, M, S>` = `Swizzle<3, 3, 3>` 表示每 128 bits 组成一个块，一行有 8 个块，共有 8 行。

当规定完成后，即可对共享内存中二维数组的编号进行重映射，在 Swizzle 中采用行列坐标进行异或的方式进行物理内存重映射。

![](img/15b3510a3fb192a678c97a368ef99c05.png)

经过 Swizzle 后的内存映射如下图所示，此时可以见 `T0`/`T4`, `T1`/`T5`, `T2`/`T6`, `T3`/`T7` 之间的内存访问完全把 Bank 分开进行访问，此时前八个线程的内存访问达到了 Bank Conflict free。其他线程的访问同理可以避免 Bank Conflict，在这种情况下 Swizzle 可以实现 Bank Conflict free。在此时尽管对于物理地址的访问发生了变化，但对于用户来说逻辑地址依然是不变的。

作为对比，`Padding` 后的效果

![](img/c8c6c04c0f1f8b1bdf6e266b5484a9be.png)

原始排布

![](img/20585b7c4aa0ee631cdd4071267427aa.png)

Cutlass 使用 `tensor core` 的进行矩阵乘法时，比如对于 `m16n8k16` 的 `fp16` 的矩阵乘法，需要分 4 个 phase 从 shared memory 分别加载 4 个 8×8 的 `fp16` 的矩阵数据到线程寄存器中，此时的 bank conflict 只局限同 phase 的 8 个线程内，可以只考虑针对这 8 个线程需要访问的 shared memory 来进行 swizzling，从而避免 bank conflict，感兴趣的读者可以去找资料学习。

![](img/13b81d1de1a6e6462388d0f9a6cb9168.png)

## PTX - Swizzling Modes

`SMEM` 中的数据，需要考虑 Swizzling 问题。几种 mode

1.  `No swizzle mode`

1.  `32 byte swizzle mode`

1.  `64 byte swizzle mode`

1.  `128 byte swizzle mode`

![](img/8c3f5a0709659344fc63b13f9edfee14.png)

### 32 byte swizzle mode

256/16 = 16

![](img/5c5c01082f5b01146fd396a5a51d8400.png)

![](img/698bd78248a3630b09bf8960f367b019.png)

![](img/8a3bf0c6b0e7b5b64ef62eae7f31ac8b.png)

figure 13 的 fragment，也就是 source layout。

![](img/a7ca61d0c169a893f938dff10d73b685.png)

![](img/be5ca27433d1d3cc3f2bfa80bd300eae.png)

### 64 byte swizzle mode

![](img/0441559892851b602d9b051d5539f93e.png)

![](img/398068a8a90e39bb29be2f381cf34fa1.png)

![](img/eb64a3d3d14159abad864ce5c6f257e9.png)

![](img/80fa02177b0cc99a2b73774719b8e060.png)

### 128 byte swizzle mode

![](img/fc3a84c4dc557893fbfe0e93d8dba5b4.png)

![](img/120091217427fddd273ecfd4ca2aff25.png)

![](img/f3e68ebdc32d4ea6f5ff3b14b4d7b5a1.png)

![](img/811062ecb55d602a7a944f1724716a01.png)

![](img/af2ed6090bb9226083fa5421b887ef3e.png)

## Triton - Shared Layout swizzling

多个 different cuda threads 可能 同时访问 的 tensor data。

GPGPU 上，一定发生在 shared memory 上。

核心是研究 swizzle 问题，以避免 bank conflicts 问题。

In other words,

for all indices  ,  = {0, 1, ..., 32*`num_warps` - 1}

In order to avoid shared memory bank conflicts, elements may be swizzled.

Here are some examples. In all cases, the input tensor is [0, 1, ..., n-1].

### Basic swizzling

`#shared<{vec=1, perPhase=1, maxPhase=4, order=[1,0]}>`

```
[ 0,  1,  2,  3],  // xor with 0
[ 5,  4,  7,  6],  // xor with 1
[10, 11,  8,  9],  // xor with 2
[15, 14, 13, 12]   // xor with 3
```

Here elements of row r are xor'ed with r (or more properly, `in[r][c] -> out[r][c^r]`).

### Multiple rows per phase

`#shared<{vec=1, perPhase=2, maxPhase=4, order=[1,0]}>`

```
[ 0,  1,  2,  3],  // phase 0 (xor with 0)
[ 4,  5,  6,  7],
[ 9,  8, 11, 10],  // phase 1 (xor with 1)
[13, 12, 15, 14]
```

Elements of row r are xor'ed with r/2.

In other words, `perPhase=2` means that pairs of 2 rows get the same swizzling.

### Max-phase applied

`#shared<{vec=1, perPhase=1, maxPhase=2, order=[1,0]}>`

```
[ 0,  1,  2,  3],  // phase 0 (xor with 0)
[ 5,  4,  7,  6],  // phase 1 (xor with 1)
[ 8,  9, 10, 11],  // phase 0
[13, 12, 15, 14],  // phase 1
[16, 17, 18, 19],  // ...
[21, 20, 23, 22],
[24, 25, 26, 27],
[29, 28, 31, 30]
```

Elements of row r are xor'ed with `(r/2) % 2`. In other words, `maxPhase=m` has the effect of limiting the maximum value of the xor to `m-1`.

### Max-phase and per-phase

`#shared<{vec=1, perPhase=2, maxPhase=2, order=[1,0]}>`

```
[ 0,  1,  2,  3],  // phase 0 (xor with 0)
[ 4,  5,  6,  7],  // phase 0
[ 9,  8, 11, 10],  // phase 1 (xor with 1)
[13, 12, 15, 14],  // phase 1
[16, 17, 18, 19],  // phase 0
[20, 21, 22, 23],  // phase 0
[25, 24, 27, 26],  // phase 1
[29, 28, 31, 30]  // phase 1
```

Here the xor value (the "phase", I guess?) changes every perPhase rows, up to a maximum value of maxPhase-1. In other words, elements of row r are xor'ed with `(r/2) % 2`.

### Adding vec

`#shared<{vec=2, perPhase=1, maxPhase=4, order=[1,0]}>`

```
[ 0,  1,  2,  3,  4,  5,  6,  7],
[10, 11,  8,  9, 14, 15, 12, 13],
[20, 21, 22, 23, 16, 17, 18, 19],
[30, 31, 28, 29, 26, 27, 24, 25]
```

When `vec=2`, elements are swizzled in pairs of 2. In other words, the element at `(r,c)` has value

`((c / 2) ^ r) * 2 + (c % 2)`.

For MMAv3 eg Hopper GMMA, `hasLeadingOffset` should be true. In this case,

when the matrix is stored in shared memory, there will be an offset not

only in the stride dimension, but also in the leading dimension. For example,

a matrix of size 16x128 and data type `I8` is stored in the shared memory with

64B-swizzle mode. The offset of the element with index `(0, 64)` will be 16*64,

compared to 1*64 when the `hasLeadingOffset` is false.

## reference

cutlass 的 swizzle 实现，500 行代码。

https://github.com/NVIDIA/cutlass/blob/main/include/cute/swizzle.hpp

PTX ISA v8.5, PDF Page 59

https://docs.nvidia.com/cuda/parallel-thread-execution/

cute 之 Swizzle - reed

https://zhuanlan.zhihu.com/p/671419093

How to understand the bank conflict of shared_mem
