
说明：将转置后的数据从`shared memory`写入到输出内存时，是按行进行的`load`操作，理论上`warp`不应该存在`bank conflicts`，但上面的截图显示仍然有1473次冲突，推测这个`load`冲突跟我们的这里的读取逻辑无关。

这个`2,032,616`次冲突的计算如下：

+ 每个`warp`的32个线程，第一个线程的写不会冲突，其他的31个线程的写都会冲突，因此每个`warp`的`bank conflicts`的次数为`32-1=31`。

+ 每个`block`有32个`warp`，总的`block`数为`(M*N)/(32*32)` = `(1024*2048)/(32*32)` = `2048`。

+ 所有`block`累计的`bank conflicts`为`2048(block)*32(warp)*31(conflicts)` = `2031616`。

类似的`wavefronts`的数值为`2,097,152`的计算如下：

+ 由于`bank`冲突，32个线程的写都需要在`MIO`中排队，也就是需要分32次访问，每次访问对应一个`wavefront`(`cycle`)，如果没有冲突，32个线程只需要在`MIO`中排队一次，此时耗费的`wavefronts`数为`1`。

+ 所有`block`累计的`wavefronts` = `2048(block)*32(warp)*32(wavefronts)` = `2097152`。

关于`wavefront`的概念：

A `wavefront` is the maximum unit that can pass through that pipeline stage per cycle. If not all `cache lines` or `sectors` can be accessed in a single `wavefront`, multiple `wavefronts` are created and sent for processing one by one, i.e. in a serialized manner.

A `wavefront` is described as a (work) package that can be processed at once, i.e. there is a notion of processing one `wavefront` per cycle in `L1TEX`. `Wavefronts` therefore represent the number of cycles required to process the requests, while the number of `sectors` per request is a property of the access pattern of the memory instruction for all participating threads. For example, it is possible to have a memory instruction that requires 4 `sectors` per request in 1 `wavefront`. However, you can also have memory instruction having 4 `sectors` per request, but requiring 2 or more `wavefronts`。

## 使用 Padding

思路

![](img/6e78d430a888ef874bc9e309428bac24.png)

coding

```
__global__ void matrix_trans_shm_padding(int* dev_A, int M, int N, int* dev_B) {
  int row = blockIdx.y * blockDim.y + threadIdx.y;
  int col = blockIdx.x * blockDim.x + threadIdx.x;

  // 每个block处理32*32的矩阵块，尾部padding来避免bank conflict
  __shared__ int s_data[32][33];

  if (row < M && col < N) {
    s_data[threadIdx.x][threadIdx.y] = dev_A[row * N + col];
    __syncthreads();
    int n_col = blockIdx.y * blockDim.y + threadIdx.x;
    int n_row = blockIdx.x * blockDim.x + threadIdx.y;
    if (n_col < M && n_row < N) {
      dev_B[n_row * M + n_col] = s_data[threadIdx.y][threadIdx.x];
    }
  }
}
```

`profile`的结果如下图所示，可以看出`shared store`的`bank conflicts`的数目变为`0`

![](img/3ac3f73979cbf6960babab417ea8cef5.png)

`padding`的缺点有：

+ 可能降低`SM`的`occupancy`。由于每个`SM`的可使用的`shared memory`有限，如果每个`block`使用的共享内存增加，则`SM`内最大可并发的`block`数目减少，导致资源不能被充分利用，一些计算资源被闲置。

+ 地址访问对齐问题。需要仔细考虑`padding`的大小来避免地址不对齐的问题，比如访问`shared memory`时可能是向量化的访问，比如`int4`访问，也就是每次访问4个`int`，即16字节，那每次访问的地址必须是16字节对齐的，对于`int s_data[32][33]`这种`padding`方式，第二行元素的起始地址就是非16字节对齐，会导致`kernel`执行出错。

## swizzling 实现

`swizzling`是在不额外分配内存的情况下，通过将`shared memory`的数据进行重排来避免`bank conflict`。下面是重排后的一个示例：

![](img/83edb3820c51e21b008c7b5a74bd2c79.png)

在上面的图中，每个元素有唯一的位置信息 `(x,y)` ， `x` 和 `y` 分别表示列和行，并且假设从0开始。

重排后，每一行内的数据集合保持不变，但行内元素的相对位置进行重排，比如对于 `y=1` ，`x`原来的索引顺序是`[0, 1, 2, 3, ......, 30, 31]`，而新的索引顺序是`[1, 0, 3, 2, 5, 4, ......, 31, 30]`。

从上图可以看到，每一行/列的各个元素的`bank`值不再一样，这样就避免了`shared memory`在`load`/`store`操作时的`bank`冲突。

```
__global__ void matrix_trans_swizzling(int* dev_A, int M, int N, int* dev_B) {
  int row = blockIdx.y * blockDim.y + threadIdx.y;
  int col = blockIdx.x * blockDim.x + threadIdx.x;

  __shared__ int s_data[32][32];

  if (row < M && col < N) {
    // 从全局内存读取数据写入共享内存的逻辑坐标(row=x,col=y)
    // 其映射的物理存储位置位置(row=x,col=x^y)
    s_data[threadIdx.x][threadIdx.x ^ threadIdx.y] = dev_A[row * N + col];
    __syncthreads();
    int n_col = blockIdx.y * blockDim.y + threadIdx.x;
    int n_row = blockIdx.x * blockDim.x + threadIdx.y;
    if (n_row < N && n_col < M) {
      // 从共享内存的逻辑坐标(row=y,col=x)读取数据
      // 其映射的物理存储位置(row=y,col=x^y)
      dev_B[n_row * M + n_col] = s_data[threadIdx.y][threadIdx.x ^ threadIdx.y];
    }
  }
}
```

代码中关于使用`swizzling`机制的代码逻辑如下：

+ 块中的每个线程从`global memory`读取一个元素后，在转置前，对应`shared memory`的逻辑为坐标`(x = threadIdx.x, y = threadIdx.y)`，进行转置后存储的逻辑坐标为`(x = threadIdx.y, y = threadIdx.x)`。

+ 对于一个`warp`内的32个线程，`threadIdx.y`值一样，`threadIdx.x`不同，导致存储到`shared memory`中的位置是相同列不同行中，从而导致`bank conflict`。

+ 此时，应用`swizzling`进行映射后的，转置存储时，行保持不变仍然为`threadIdx.x`，新的列为 `threadIdx.x ^ threadIdx.y`。

+ 读取时采用类似的机制，将逻辑坐标转换为物理存储坐标，再从`shared memory`的对应物理位置去读。

`Profile`结果如下：

![](img/8841f7b3ba7653cfdc8b8ee321e67d1c.png)

图中显示`shared memory` `store`时`bank conflicts`=`0`，但`load`时还有`bank conflict`，推测跟`swizzling`无关。

## MMA Bank Conflict

为了兼容 `Tensor Core` 的 `Warp Level` 计算属性，

从 `Shared Memory`加载的数据是以 `block` 的形式进行操作的，

并且该`Warp Level Memory`块中可能有多个 `sub block` 是在同一个`Bank`上，

这个与`mma.sync`的指令一般是`m16n8k16`类型的2的幂次方形状有关，

由于`Shared Memory`是有限的所以`Shared Memory`重用率非常高，导致如果出现`Bank Conflict`后，可能会极大的影响`Kernel`的性能。

![](img/a956e73d9795004aa085f1497b28cc6d.png)

选用 `32 x 64` 的`Block`大小，数据类型为`half`类型。

`Naive Row-Major Layout`

按行访问时，没有 `bank conflict`。

但，按 `TensorCore` 场景，按 `tile / block` 加载时，就要访问一个 `bank` 的多行了。

```
half* naive_layout(half* data, int r, int c) 
{
    return &(data[r * columns + c]);
}
```

`Padding Row-Major Layout`

![](img/14f1776ab37cf851528a19b6af6bf133.png)

引入 `offset`，每一行都 `shift` 一下。

弊端是，会产生未对齐的地址，从而干扰需要对齐地址的快速指令。

```
half* padding_layout(half* data, int r, int c)
{
    return &(data[r * (columns + 1) + c]);
}
```

`Naive Swizzled Layout`

![](img/b69bc1404ebc6d222631e141003b66f5.png)

“S`[wiz](https://zhida.zhihu.com/search?content_id=701901940&content_type=Answer&match_order=1&q=TMA&zhida_source=entity)zle”内存，其中渐进的行被重新洗牌以改变`Banking`的排列方式。这种布局通过将索引与行进行异或来实现这一点，从而消除了存储体冲突并对齐了地址。然而，这种布局缺乏对 `HGMMA` 和 `TMA` 指令的硬件支持，这对于 `H100` GPU 实现高性能尤其重要。

```
half* row_swizzled_layout(half* data, int r, int c)
{
    auto address = (uint64_t)&data[r * columns + c];
    return reinterpret_cast<half *>(address ^ (r << 2));
}
```

[`矩阵计算`](https://zhida.zhihu.com/search?content_id=237334060&content_type=Article&match_order=1&q=%E7%9F%A9%E9%98%B5%E8%AE%A1%E7%AE%97&zhida_source=entity)中的数据加载是从全局内存到共享内存然后到寄存器。共享内存作为中间的媒介可以[`减少矩`](https://zhida.zhihu.com/search?content_id=237334060&content_type=Article&match_order=1&q=%E5%B9%B6%E8%A1%8C%E6%80%A7&zhida_source=entity)阵计算时对全局内存的访问数据量，从而提升计算访存比。共享内存为了提升访问的并行性采用多`bank`结构，这也造成了编程时的困难，`cute`通过提供`swizzle`抽象简化了逻辑空间和多`bank`存储空间的映射的复杂度。

最早是 `WMMA` (`warp level MMA`)

`Hopper` 引入了 `WGMMA` (`Warpgroup MMA`)，可以直接读取 `SMEM` 中的数据进行计算。不用经过 `register file`。

`wmma` 封装了 `Shared Memory` -> `MMA` -> `Shared Memory`这一层的过程。

包括 `matrix A/B/C` 的 `fragment`、`fragment` 的加载（`load_matrix_sync`），

结果就是很难规避 `shared memory bank conflict`。

因为，丧失了一些灵活性，例如想做`swizzle`，改变`shared memory` -> `mma`这部分的`layout`，或者做一些`fuse`，把`elementwise`的`op`直接`fuse`到`mma`累加完的寄存上，`WMMA`就不太好做了。

`sm80` 以上，`Hopper`(`sm90`) 之前，`mma.sync` 使用较多，且，可以达到更好的性能

`wmma`这个由每个线程去组织数据时比较令人头疼，而针对`mma`，会用`ldmatrix`这条指令就够了，所以尽管`mma`是通过一个`warp`去调度实现的，但是还是粒度为线程级别时更容易编写。

假设用`TensorCore`直接计算一个`16x8x16xfp16`大小的矩阵乘法，针对从`shared memory`加载到`register file`然后送进`mma`进行计算的过程，用`PTX`指令大致描述如下：

```
// [load A f](https://zhida.zhihu.com/search?content_id=245984318&content_type=Article&match_order=2&q=ldmatrix&zhida_source=entity)rom shared memory to register file, d0/1/2/3 is b32 register
ldmatrix.sync.aligned.m8n8.x4.shared.b16 {d0, d1, d2, d3}, [a_addr];
// load B from shared memory to register file, d4/5 is b32 register
ldmatrix.sync.aligned.m8n8.x2.trans.shared.b16 {d4, d5}, [b_addr]; 

// d = a x b + c
mma.sync.aligned.m16n8k16.row.col.f32.bf16.bf16.f32 d, a, b, c;
```

`ldmatrix`指令以一个`warp`的形式负责从`A`、`B`矩阵`shared memory`中加载`mma`指令需要大小的数据（分别为`A`——`16x8x16xfp16` with `row-marjor`、`B`——`16x8xfp16` with `col-major`），然后交给`mma`指令进行计算。

对于矩阵`A`，按照`ISA`的描述，`ldmatrix`负责读取4个`8x8xfp16`矩阵，为了用一个`warp`完成这个过程，这里需要分成4个`phase`，其中`T0-7`为`phase 0`、`T8-15`为`phase 1`、`T16-23`为`phase 2`、`T24-31`为`phase 4`，每个`phase`读取一个`8x8xfp16`矩阵，每个`phase`（8个线程）中的每个线程负责读取一行的`8xfp16`=`16bytes`=`128bit`（`vector`）=4`bank`数据。注意，这一行的4`bank`数据必须连续，每个线程之间的4`bank`数据则不要求连续，满足对齐要求即可。因此，这里对`bank`冲突的讨论将从一个`warp`变成一个`phase`（8个线程），只需要保证这8个线程读取的时候不会发生`bank`冲突即可。

可以发现，每个线程将自己读取到的4`bank`数据，拆成4份，然后发给指定的线程，例如`T0` 4个`32bit`寄存器（`d0`、`d1`、`d2`、`d3`）将会收到`T0`、`T8`、`T16`、`T24`的第一份数据，依次类推。一张图显示`ldmatrix`指令的加载过程：

## Ampere - ldmatrix 指令
