# CUDA Reduction 加速 - TODO

> 来源：[https://learning.feishu.cn/docx/RG06dfdxBoYfeLxy4A9cK3h0nqd](https://learning.feishu.cn/docx/RG06dfdxBoYfeLxy4A9cK3h0nqd)

Reduction 问题的经典介绍: https://developer.download.nvidia.cn/assets/cuda/files/reduction.pdf

大部分优化手段，已不适合最新的 Nvidia GPU。但，思想和理念，熠熠生辉。

我的测试代码: https://github.com/JackonYang/paper-reading/tree/master/02-tutorial-code/03-hands-on-cuda

![](img/a2933ebb52df212ea8aba6eeeff3da50.png)

最简单的 reduction 实现，就是 sum，max。

如果在 GPU 上，用 N thread 并行，最少需要  轮。

# H100 - 手写 reduction 测试

H100 PCIe 的 HBM bandwidth 是 2TB/s。

来自：https://www.nvidia.com/content/dam/en-zz/Solutions/gtcs22/data-center/h100/PB-11133-001_v01.pdf

![](img/19e2ed55306cb2801e01065a93163d36.png)

## 功能版本

n = 4 * 1024 * 1024，即 4M。耗时 100307us

```
__global__ void SerialKernel(const float* input, float* output, size_t n) {
  float sum = 0.0f;
  for (size_t i = 0; i &lt; n; ++i) {
    sum += input[i];
  }
  *output = sum;
}

void ReduceBySerial(const float* input, float* output, size_t n) {
  SerialKernel&lt;&lt;&lt;1, 1&gt;&gt;&gt;(input, output, n);
}
```

## Baseline 性能

```
#define N (1024 * 1024 * 1024)  // 1G elements

// Naive reduction kernel
__global__ void reduce_naive(float* d_in, float* d_out) {
    // Shared memory declaration
    // - Dynamic shared memory allocation using extern keyword
    // - Visible to all threads in the block
    extern __shared__ float sdata[];
    
    // each thread loads one element GMEM -&gt; SMEM
    // sync to Ensures all threads finish loading before computation
    unsigned int tid = threadIdx.x;  // Thread ID within block
    unsigned int i = blockIdx.x * blockDim.x + tid;  // Global index
    sdata[tid] = (i &lt; N) ? d_in[i] : 0.0f;
    __syncthreads();

    // do reduction in shared mem
    // - Sequential addressing: Stride starts at half block size
    // - Avoids thread divergence within warps (tid &lt; s condition)
    for(unsigned int s = blockDim.x/2; s &gt; 0; s &gt;&gt;= 1) {
        if (tid &lt; s) {
            sdata[tid] += sdata[tid + s];
        }
        // Prevents race conditions during shared memory access
        __syncthreads();
    }

    // write result for this block to global mem &amp; acc using atomicAdd
    if (tid == 0) atomicAdd(d_out, sdata[0]);
}
```

性能测试结果：HBM Bandwidth: 609.44 GB/s

```
N: 1024M, grid_size: 4194304, block_size: 256
diff:  0, CPU: 1073741824.0, GPU: 1073741824.0.
Success! time cost: 8.956 ms, HBM Bandwidth: 479.56 GB/s

N: 1024M, grid_size: 2097152, block_size: 512
diff:  0, CPU: 1073741824.0, GPU: 1073741824.0.
Success! time cost: 7.047 ms, HBM Bandwidth: 609.44 GB/s

N: 1024M, grid_size: 1048576, block_size: 1024
diff:  0, CPU: 1073741824.0, GPU: 1073741824.0.
Success! time cost: 8.598 ms, HBM Bandwidth: 499.50 GB/s
```

## Memory Coalesce

Memory Coalesce，即，引入了 elementsPerThread。从 1 测试到 16 找最佳。

elementsPerThread = 8 时，bandwidth 跑到  1.86T。达到极限的 93.3% 了。

只用 elementsPerThread = 8，然后 atomicAdd。别的优化技巧都没用。

得到的经验：

很多沉淀多年的 cuda 优化技巧，实测下来，在现代硬件 + 认知下，已经意义不大了。

比如，reduce 的时候，最后一个 warp 要手动 unroll。

源码

```
float sum = 0.0f;
#pragma unroll
for (size_t ii = 0; ii &lt; elementsPerThread; ii++) {
    int gtid = i + ii;
    if (gtid &lt; N) {
        sum += d_in[gtid];
    }
}
sdata[tid] = sum;
__syncthreads();
```

优化措施：

1.  减少不必要的 SMEM 的访问。

1.  每个 thread 内的累加，从写入 SMEM 改成用 thread 临时变量。

测试记录：

![](img/5411fa7e221218e351fb86ac59218e37.png)

```
N: 1024M, grid_size: 2097152, block_size: 512, elementsPerThread: 1
diff:  0, CPU: 1073741824.0, GPU: 1073741824.0.
Success! time cost: 7.039 ms, HBM Bandwidth: 610.14 GB/s

N: 1024M, grid_size: 1048576, block_size: 512, elementsPerThread: 2
diff:  0, CPU: 1073741824.0, GPU: 1073741824.0.
Success! time cost: 3.832 ms, HBM Bandwidth: 1120.81 GB/s

N: 1024M, grid_size: 524288, block_size: 512, elementsPerThread: 4
diff:  0, CPU: 1073741824.0, GPU: 1073741824.0.
Success! time cost: 2.693 ms, HBM Bandwidth: 1594.91 GB/s

Build successful. Executing...
N: 1024M, grid_size: 262144, block_size: 512, elementsPerThread: 8
diff:  0, CPU: 1073741824.0, GPU: 1073741824.0.
Success! time cost: 2.400 ms, HBM Bandwidth: 1789.52 GB/s

N: 1024M, grid_size: 131072, block_size: 512, elementsPerThread: 16
diff:  0, CPU: 1073741824.0, GPU: 1073741824.0.
Success! time cost: 3.347 ms, HBM Bandwidth: 1283.24 GB/s
```

增大 或 减少 elementsPerThread，bandwidth 都下降。

```
N: 1024M, grid_size: 262144, block_size: 512, elementsPerThread: 8
diff:  0, CPU: 1073741824.0, GPU: 1073741824.0.
Success! time cost: 2.300 ms, HBM Bandwidth: 1867.12 GB/s

N: 1024M, grid_size: 131072, block_size: 512, elementsPerThread: 16
diff:  0, CPU: 1073741824.0, GPU: 1073741824.0.
Success! time cost: 3.185 ms, HBM Bandwidth: 1348.60 GB/s

N: 1024M, grid_size: 65536, block_size: 512, elementsPerThread: 32
diff:  0, CPU: 1073741824.0, GPU: 1073741824.0.
Success! time cost: 5.649 ms, HBM Bandwidth: 760.24 GB/s

N: 1024M, grid_size: 524288, block_size: 512, elementsPerThread: 4
diff:  0, CPU: 1073741824.0, GPU: 1073741824.0.
Success! time cost: 2.556 ms, HBM Bandwidth: 1680.05 GB/s
```

## warpReduce

也就是 shfl 指令。在 register 内即可完成，不用经过 shared memory。

以下代码，可以得到 wap 内 指定 thread 变量的 reduce_sum

```
template &lt;typename T&gt;
__inline__ __device__
T warpReduceSum(T val)
{
  for(int mask = 16; mask &gt; 0; mask &gt;&gt;= 1)
    val += __shfl_xor_sync(FINAL_MASK, val, mask, 32);
  return val;
}
```

## blockReduce

首先通过束内规约求出当前线程所在 warp 的规约值，存入 shared 中，然后把 warpSum 赋值给 threadIdx.x 小于 32 的线程内的变量 val，这 32 个线程正好也在一个线程束内，然后再执行一次束内规约就得到块内规约值，计算思路非常巧妙。

典型的实现代码

```
template &lt;typename T&gt;
__inline__ __device__
T blockReduceSum(T val)
{
  static __shared__ T shared[32]; 
  int lane = threadIdx.x &amp; 0x1f; 
  int wid = threadIdx.x &gt;&gt; 5;  

  val = warpReduceSum&lt;T&gt;(val);

  if(lane == 0)
    shared[wid] = val;
  __syncthreads();
  
  val = (threadIdx.x &lt; (blockDim.x &gt;&gt; 5 )) ? shared[lane] : (T)0.0f;
  val = warpReduceSum(val);
  return val;
}
```

# 理论性能推演

SIMT 怎么做，优化空间，典型 case。

SIMD 怎么做，优化空间，典型 case。

用 triton 跑一个 GPGPU 上的 reduce max，看生成的 code 是什么水平，特别是与 memory layout 和 access coalesce 相关的 pass。

访存优化

GPU也支持 32/64 字节大小的 CacheLine, 详情参考:

https://docs.nvidia.com/cuda/cuda-c-programming-guide/index.html#device-memory-accesses

![](img/187a3f002f645424ad701dfc9b41327c.png)

![](img/66e55c746922f7a86b7ab8d532ff616f.png)

## Roofline 模型分析

以 fp32 精度的 reduce_sum 为例，

假设其元素数量等于 n，则运算量为 n-1 次加法，内存读取比特数为 4n，

因此算术强度大约为 0.25。

以 RTX 4050 Mobile 笔记本作为测试平台进行分析，首先可以查到它的峰值 FP32 算力为 8.986 TFLOPS，显存带宽为 192 GB/s

这里的 46.80 来自于 8986 / 192。其中黑点标明的位置就是 Reduction 操作在当前 GPU 上的性能峰值，也就是 100% 利用显存带宽的情况下能够达到的运算吞吐量，大约为 48 GFLOPS。

![](img/eb29138bbd8030968549b1d5bc16f399.png)

# Benchmark 复现

涉及 3 款 GPU，复现的现象不一样。2024.04.02, based on exp01 - exp12

1.  K80 是 2012 年的 Kepler 架构。slides 的原始 arch。

1.  GTX 1080 是 2016 年的 Pascal 架构，与 P4 是同一代。

1.  kernel 4 达到极限 throughput。240GB/s of 320 GB/s，约 75%。

1.  后续优化无明显提升。

1.  RTX 4090。

1.  kernel 6 加速效果明显，400GB/s -> 850GB/s of 1008GB/s，约 85%。

1.  kernel 6 的加速方法：Multiple Adds per Thread。加速原理分析：

1.  计算分 2 个 round。先 Multiple Adds per Thread 完成大部分计算，然后 loop unroll 完成尾块的计算。

1.  加速空间，主要来自 first round，相比于 single add per round，减少了 thread 调度和切换的代价，一个 thread 先把能做的计算做到底。

1.  又说明，在 RTX 4090 上，因为 bandwidth & compute capacity 的增加，bottleneck 已经是 thread schedule 了。其他的，都被 software + hardware 的 co-design 自动全解决了。

1.  其他 kernel 没有看到明显加速效果。

对我们的启发

1.  hardware / nvcc compiler / driver or compiler inside driver。

1.  bandwidth vs compute。

1.  有指令同时做 2 个 reduction。

1.  Reduction 指令之间的 hardware IDLE 气泡挤压。

## Profile 命令

```
git clone git@github.com:NVIDIA/cuda-samples.git

cd cuda-samples/Samples/2_Concepts_and_Techniques/reduction

mkdir build &amp;&amp; cd build
cmake ..
make -j$(nproc)

export CUDA_PATH=/usr/local/cuda-12.5/
# nvcc compile
make build
# binary run
make testrun
make run
```

```
cd cuda-samples/Samples/2_Concepts_and_Techniques/reduction

export CUDA_PATH=/usr/local/cuda-12.5/
# nvcc compile
make build

# binary run
make run

# gdb
gdb --args ./reduction -kernel=11

nvidia-smi -l 1


cat smi.log | grep -A 1 &#x27;0  NVIDIA GeForce&#x27; | grep &#x27;%&#x27;
cat qiu.log | grep -A 1 &#x27;1  NVIDIA GeForce&#x27; | grep &#x27;%&#x27;

# 拿利用率，并输出到 1 行
cat perf-yan.log | cut -d &#x27;|&#x27; -f 4 | awk &#x27;{print $1}&#x27;
# 用 xargs 比较一组文件的利用率
ls perf-*.log | xargs -I{} bash -c &quot;cat {} | cut -d &#x27;|&#x27; -f 4 | cut -d &#x27;%&#x27; -f 1 | xargs echo&quot;
0 0 0 1 67 44 80 35 78 11 57 95 37 89 43 85 46 87 51 72 96 63 94 11 0 0 0 0 0 0
0 0 0 0 70 75 95 95 94 95 91 94 97 96 98 95 96 96 97 96 96 96 94 96 0 0 0 0 0 0 0
0 0 0 0 70 48 95 56 94 70 91 88 43 96 60 95 52 96 55 93 65 96 81 96 0 0 0 0 0 0 0
```

RTX 4090 card

Software versions:

```
$ nvcc --version
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2023 NVIDIA Corporation
Built on Tue_Jun_13_19:16:58_PDT_2023
Cuda compilation tools, release 12.2, V12.2.91
Build cuda_12.2.r12.2/compiler.32965470_0

$ cat /proc/driver/nvidia/version
NVRM version: NVIDIA UNIX x86_64 Kernel Module  535.113.01  Tue Sep 12 19:41:24 UTC 2023
GCC version:  gcc version 12.3.0 (Ubuntu 12.3.0-1ubuntu1~22.04)
```

## exp01 - baseline

Key info:

1.  Code commit: 3559ca4d088e12db33d6918621cab5c998ccecf1

![](img/3657120c650e033819b992bd866382db.png)

Result:

有较大的波动，629 GB/s, 851 GB/s。

白皮书 spec 是 1008 GB/s，达到了 80%

![](img/a2b5e56ef8f0772f3af09a852035f846.png)

## exp12 - grid size

实验对于kernel 7 中的优化，就grid_size的变化，记录对throughput的影响：

Grid size 太小和太大，都不行。by default 时，用的是 2^6，是比较极限的 high performance。

| Grid Size | GB/s card:0 | GB/s card:1 |
| --- | --- | --- |
| 2^5 | 454.2277 | 453.1159 |
| 2^6 | 788.6683 | 846.3871 |
| 2^7 | 787.6002 | 816.1968 |
| 2^8 | 805.9321 | 853.8864 |
| 2^9 | 807.8441 | 811.9284 |
| 2^10 | 411.0989 | 419.6223 |

# Reference

Nvidia Reduction PDF: https://developer.download.nvidia.cn/assets/cuda/files/reduction.pdf

reduce优化学习笔记 https://github.com/BBuf/how-to-optim-algorithm-in-cuda/blob/master/reduce/README.md

【CUDA编程】束内规约与块内规约问题 https://mp.weixin.qq.com/s/gXima2dpby-0jTee3wPVdw