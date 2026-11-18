
如果`grid_size`和`block_size`都用`1d`的表示，类似一个如下的`2d Tensor`。

另外一种`2d`的画法，如下的画法，可以方便的改成`3d`情况，每一个小格子是一个`multi-dim`的`tensor`。

![](img/d765069ef01ed881d644aa303ae56ad4.png)

### 计算`block_size` & `grid_size`的典型方法

```
int block_size = 256;
int grid_size = (N + block_size - 1) / block_size;
```

其中，`grid_size`本质是`ceil(N/block_size)`，上面是不用`ceil`函数的常见`trick`。

`block_size * grid_size`有上限`limit` => 单个`grid`的`thread`数限制。

当`kernel`的`element`数量超过`grid limit`时，一种思路是：

每个`thread`做多个`element`。代码复杂度增加，但效率是很高的。

支持任意`element per thread`的一种思路，用`while loop` + `tid`递增。

在`deep learning`任务中，如果`N`是编译时已知的，可以循环展开。也避免`while loop`引入分支分化的可能性。

![](img/c2927764dcacf048edc8c33a24378456.png)

如果`kernel`能支持任意数量的`element per thread`，

那么，选一个超过`SM`数量的`grid_size`似乎也没有意义了。

可以直接写`constant`，比如`128`，或者常见`GPU arch SM`数量的最小公倍数。

## Shared memory & Sync (Reduction?)

`splitting blocks into threads`，

可以认为是一种`working around`，适配`hardware`有最大`block number`（`grid_size`）的限制。

`__shared__`修饰符，声明分配在`SMEM`里面的`variable`。

每个`block`在`launch block`的时候，都会有一份`copy`。数据是`private to each block`的。

在`block`内做`data dependency` & `sync`，缩小了`barrier`的代价。

`fast barrier synchronization`也是`SIMT`模型的3大前提之一。

`SIMT`执行模型，关键前提：

1. `fast barrier synchronization`

1. `lightweight thread creation`

1. `zero schedule overhead`

# Reduction

先`element wise`的每个`thread`处理`n`个`element`。

然后把所有`thread`的结果，`reduce`到`1`个`element`。

做`reduction`的时候，如果`input`规模比较小，

为了用满`SM`算力而设置很大的`block number`，不一定是性能最优的。增加了`sync`的代价。

`naive`的实现中，有`2`个`sync`代码。第二行`sync`在`loop`里。

实际的`sync`代价是

![](img/6185b356c445860aa2ae46d7133a746c.png)

`reduction`之前，注意做好`thread`间的`sync`。确保别的`thread`写完再去`read`他的结果。

```
// synchronize threads in this block 
 __syncthreads();
```

This call guarantees that every `thread` in the `block` has completed instructions prior to the `__syncthreads()` before the `hardware` will execute the next instruction on any `thread`.

例子

![](img/15efa7d7b9d204436cebb7cf03046589.png)

`code`

![](img/9c0a882867060e5dd3b1580cd6649ed5.png)

## SMEM

## sync & barrier

# GPU 编程的挑战

`single thread`很简单的事情，但`cuda`里面就很复杂了

1. `atomic`。`i++`指令，用`AtomicAdd`

1. `stream`

1. 带`control flow`支持的汇编

# matmul

## Code

例子代码写的不太典型，缺少`tuning`空间。换成`GEMM`的例子。

```
#include <stdio.h>
#include <cuda_runtime.h>
#include <iostream>


static void CheckCudaErrorAux (const char *, unsigned, const char *, cudaError_t);
#define CUDA_CHECK_RETURN(value) CheckCudaErrorAux(__FILE__,__LINE__, #value, value)


/**
 * Check the return value of the CUDA runtime API call and exit
 * the application if the call has failed.
 */
static void CheckCudaErrorAux (const char *file, unsigned line, const char *statement, cudaError_t err){
        if (err == cudaSuccess)
                return;
        std::cerr << statement<< " returned " << cudaGetErrorString(err) << "("<<err<< ") at "<<file<<":"<<line << std::endl;
        exit (1);
}


void generate_matrix(float *data, int size, float init_val){
  for (int i = 0; i < size; i++) {
    data[i] = init_val;
  }
}


__global__ void maxtrix_mul(float* a, float* b, float *c, int dim){
  int id = threadIdx.x;
  int x = id % dim; //每一个线程负载c[y][x]的结果计算
  int y = id / dim;

  #define idx(y,x) y*dim + x
  #pragma unroll //不用管
  for(int i = 0; i < dim; i++) {
    c[idx(y,x)] += a[idx(y,i)]*b[idx(i,x)];//计算c[y][x]
  }
}

int main(int argc, const char *argv[]){
  const int dim = 10;
  int size = dim*dim;
  float* a_host = new(float[size]);
  float* b_host = new(float[size]);
  float* c_host = new(float[size]);

  generate_matrix(a_host, size, 1);
  generate_matrix(b_host, size, 2);


  float *a_device, *b_device, *c_device;
  CUDA_CHECK_RETURN(cudaMalloc((void **) &a_device, size*sizeof(float))); //在GPU上为矩阵分配内存
  CUDA_CHECK_RETURN(cudaMalloc((void **) &b_device, size*sizeof(float)));
  CUDA_CHECK_RETURN(cudaMalloc((void **) &c_device, size*sizeof(float)));

  CUDA_CHECK_RETURN(cudaMemcpy(a_device, a_host, size*sizeof(float), cudaMemcpyHostToDevice)); //把a,b矩阵copy到GPU上
  CUDA_CHECK_RETURN(cudaMemcpy(b_device, b_host, size*sizeof(float), cudaMemcpyHostToDevice));

  maxtrix_mul<<<1,size>>>(a_device, b_device, c_device, dim); //让GPU计算，<<<>>>内指定启动size个线程，这种函数称之为kernel函数。

  CUDA_CHECK_RETURN(cudaDeviceSynchronize()); //暂时不用管
  CUDA_CHECK_RETURN(cudaMemcpy(c_host, c_device, size*sizeof(float), cudaMemcpyDeviceToHost)); //把计算结果c矩阵取回主机内存

  printf("maxtrix multiply finished! c[0][0] = %f", c_host[0]);

  delete(a_host);
  delete(b_host);
  delete(c_host);
  cudaFree(a_device);
  cudaFree(b_device);
  cudaFree(c_device);
  return 0;
}
```

### run

```
nvcc matrix_mul.cu -o run
./run
```

因此保证`GPU`程序就是保证别让`GPU`的`Core`和`CPU`闲着，高效方式主要有两条：

+ 保证并发度，即尽量多用`Core`

+ 用上的`Core`尽量别等待，比如说等`IO`

### 计算`Computation`

如何填满一个`Warp`（32 `thread`）也有一定的讲究（一家人就要整整齐齐的）。因此，这个层面的性能分析往往是：

1. 我用的`Core`够不够。

1. `Warp`的利用率高不高。

### `IO`

其实上图中已经出现了`Register`、`Shared Memory`（`CUDA`可控）、`Cache`这样的组件。那么我们简单介绍一下`IO`构架。

+ 首先`System memory`会通过`PCIe3`连接`GPU`，把数据传入`Device Memory`，这里的带宽上限大概是`12GB/s`。

+ 然后每个`Core`会发起对`Device Memory`读取操作，检查是否命中`Cache`，若没有命中，则由`SM`整体对`Memory Controller`发起读取操作，因此这里最好多个`Core`每次读一段连续的地址，有助于提高`Cache line`的命中从而减少读写`Device Memory`的次数

+ 对于热数据，用户合一自定义使用`Shared memory`进行加速减少`IO`瓶颈。

也就是说，在优化`performance`时，我们需要尽量减少`GPU Core`由于`IO`操作引起的空转。

![](img/f67316bc77c54a2db52bd691d83edc26.png)

### `GPU`的基本内存构架

![](img/296dd7a7afcb1143ef9c9c67fdda25ca.png)

1. 通常我们使用`global memory`，使用`cudaMemcpy`将数据从`host memory`拷贝到`GPU`上。

1. `GPU`有两级`cache`，其中`L1 cache`每个`SM`都有一个，但是比较小。看`GPU`架构图

1. 我们通常将频繁访问的数组放入`shared memory`中，它具有接近`register`的速度。（硬件上，每个`SM`有一个`shared memory`。）

1. 软件上，所有一个`block`内的`thread`共享一个`shared memory`的数据。

## GPU 线程规划

```
...

global
 void maxtrix_mul(float* a, float* b, float *c, int dim){
  int id = threadIdx.x;
  int x = id % dim; //每一个线程负载c[y][x]的结果计算
  int y = id / dim;
 
....
  maxtrix_mul<<<1,size>>>(a_device, b_device, c_device, dim); //让GPU计算，<<<>>>内指定启动size个线程，这种函数称之为kernel函数。
...
```

我们在`kernel`函数的`<<<gird, block>>>`指定`GPU`启动线程的数量和其在`GPU`上的大致分布。

其中`block`和硬件层面的`SM`对应。（请牢记这句话）。

实际上，为了更好的和程序结合，`CUDA`的线程可以成三维排列。虽然没有很大的用处，但是可以减少用户把`thread`的`uid`自己重新在空间中排列的工作。

我们这里简单以一个二维线程分布举例。假设我们在一个`kernel`启动的时候采用了这样的代码：

```
two_d_kernel<<<dim3(2,2),dim3(3,3)>>>(a_device, b_device, c_device, dim);
```

![https://s3-us-west-2.amazonaws.com/secure.notion-](img/fc362a400ab208d8f0e3feabc5698efa.png)static.com/55a7b597-8de5-485b-8507-09fecca70da5/Untitled.png

```
#include <stdio.h>

global
 void two_d_kernel(){
  printf("gridDim.x:%d, gridDim.y:%d, blockDim.x:%d, blockDim.y:%d ***** blockIdx.x:%d, blockIdx.y:%d, threadIdx.x:%d, threadIdx.y:%d\n",gridDim.x,gridDim.y,blockDim.x,blockDim.y,blockIdx.x,blockIdx.y,threadIdx.x,threadIdx.y);
}
int main(){
  two_d_kernel<<<dim3(2,2),dim3(3,3)>>>();
  cudaDeviceSynchronize();
  return 0;
}
```

# Optimization & Deeper Look

`goal`: 加速矩阵乘

`CUDA`程序加速可以总结为两个方向：

1. `IO`优化（提高`cache`命中、使用`share memory`、`register`等）

1. 并行规划（让计算与`IO`、`IO`与`IO`、计算与计算互相`overlap`）

本节需要一点背景知识：

1. `cache`的基本原理。

1. 阻塞和非阻塞，异步和同步的概念。

1. 多线程的基本原理。

### `IO`优化

程序优化的`2`个常见优化点：

1. `cache`友好的访问模式。

1. 使用`shared memory`暂存频繁读写的数据。

### `cache`友好的访问模式

`GPU`的`cache`与传统的`cache`相同，也存在`cache line`的概念。

`cache line`解释：Why software developers should care about CPU caches https://medium.com/software-design/why-software-developers-should-care-about-cpu-caches-8da04355bb8a

Each processor core sports two levels of cache:

+ `2 to 64 KB Level 1 (L1) cache` very high speed cache

+ `~256 KB Level 2 (L2) cache` medium speed cache

All cores also share a `Level 3 (L3) cache`. The `L3 cache` tends to be around `8 MB`

+ `L1 cache` access latency: `4` cycles

+ `L2 cache` access latency: `11` cycles

+ `L3 cache` access latency: `39` cycles

+ `Main memory` access latency: `107` cycles
