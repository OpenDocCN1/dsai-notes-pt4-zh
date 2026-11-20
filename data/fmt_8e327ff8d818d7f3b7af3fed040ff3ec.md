
```
#include <stdio.h>
#include <iostream>

__global__ void helloFromGPU (void)
{
  __shared__ uint32_t aTile[4*8*4];

  int tidx = threadIdx.x + blockDim.x * threadIdx.y;
  // 下面的代码是把smem中的4*8*4的矩阵，初始化数值！
  if (tidx == 0) {
    for (int i = 0; i < 4*8*4; ++i) {
        aTile[i] = i;
    }
  }
  __syncthreads();

  int aTile_index = tidx % 16 * 8 + tidx / 16 * 4;
  uint32_t a[4];
  uint32_t smem = __cvta_generic_to_shared(aTile+aTile_index);
  asm("ldmatrix.sync.aligned.m8n8.x4.shared.b16 { %0, %1, %2, %3 }, [ %4 ];\n"
  : "=r"(a[0]), "=r"(a[1]), "=r"(a[2]), "=r"(a[3]) 
  : "r"(smem)
  );

  if (tidx == 1) {
    printf("%d \n", (a[0])); printf("%d \n", (a[1]));
    printf("%d \n", (a[2])); printf("%d \n", (a[3]));
  }
}

int main(void) {
  uint3 grid = {1,1,1};
  uint3 block = {32,1,1};
  helloFromGPU <<<grid, block>>>();

  cudaDeviceReset();
  return 0;
}
```

+ 编译命令是 `nvcc A.cu -arch sm_80`。
+ 下面的代码就是让一个warp中的32个线程，从shared memory中加载4个8*8的矩阵，但是我这个里面写的是`uint32_t`元素类型，因此其实是4个8*4的矩阵！
+ 下面代码打印出线程1占据的四个32位寄存器的值！
+ 上面代码中`int aTile_index = tidx % 16 * 8 + tidx / 16 * 4;`这个代码是让
+ 每个线程分别指向每行的首地址！
+ 其中`tidx % 16`表示的是行id，
+ `tidx / 16 * 4`表示的是列id，
+ 从按照`b16`的角度来看，其实列id应该是`tidx / 16 * 8`，
+ 但是我代码里面写的是`uint32`类型，所以就是`tidx / 16 * 4`了！
+ 最后打印出来的四个数字如下图所示哦！

![](img/f9bb7e5bf582231115f96769c6318e3b.png)

+ 上面的使用例子我的数据单元设置成`uint32_t`了，用户可能不太好直接理解，下面我补充一个例子，直接针对`fp16`元素进行`ldmatrix`指令的例子。
+ 下面例子的代码我起名字叫`B.cu`，编译命令是 `nvcc B.cu -arch sm_80`。

```
#include <stdio.h>
#include <iostream>
#include "cublas_v2.h"

__global__ void helloFromGPU (void) {
  __shared__ half aTile[4*8*8];

  int tidx = threadIdx.x + blockDim.x * threadIdx.y;
  // 下面的代码是把smem中的4*8*8的矩阵，初始化数值！
  if (tidx == 0) {
    for (int i = 0; i < 4*8*8; ++i) {
        aTile[i] = i;
    }
  }
  __syncthreads();

  int aTile_index = tidx % 16 * 16 + tidx / 16 * 8;
  uint32_t my_register[4];
  uint32_t smem = __cvta_generic_to_shared(aTile+aTile_index);
  asm("ldmatrix.sync.aligned.m8n8.x4.shared.b16 { %0, %1, %2, %3 }, [ %4 ];\n"
  : "=r"(my_register[0]), "=r"(my_register[1]), "=r"(my_register[2]), "=r"(my_register[3]) 
  : "r"(smem)
  );

  if (tidx == 1) {
    for (int i = 0; i < 4; i++) {
      half * tmp = (half*)(&(my_register[i]));
      printf("%f\n", (float)(tmp[0]));
      printf("%f\n", (float)(tmp[1]));
    }
  }
}

int main(void) {
  uint3 block = {32,1,1};
  uint3 grid = {1,1,1};
  helloFromGPU <<<grid, block>>>();

  cudaDeviceReset();
  return 0;
}
```

打印出来的就是下面的8个数字啦

![](img/50172e9b126cb999a57ddaf24d4f6560.png)

# H100 - TMA 引擎

TMA 引擎的 API，见章节：`Data Movement and Conversion Instructions: cp.async.bulk`

https://docs.nvidia.com/cuda/hopper-tuning-guide/index.html#tensor-memory-accelerator

# 附录

## Compute Capability Sheet

![](img/12f8dfca9ba21e6af231443ff4dc5efb.png)

![](img/be2ad7757e3ecce841c2a3d0a90c0fbc.png)

Compute Capability 查看方法: https://developer.nvidia.com/cuda-gpus

![](img/f257e3d621c3a16a13842d9248b96e63.png)

## PTX ISA version history

![](img/4c7c1c8f8cd9c4b71a79a14a81393f07.png)

![](img/da2c85357a749fb127e94a15e54c61aa.png)

![](img/56c3b9f37c5e04a399b67113ebed6391.png)

## 直接访问 SMEM 的指令

GPU 的运算单元可以直接操作共享内存的数据而不加载到寄存器中吗

`sm90` 增加的 `TMA`, `WGMMA` 指令可以, `TMA` 还带了 reduction 功能

https://docs.nvidia.com/cuda/parallel-thread-execution/index.html#register-fragments-and-shared-memory-matrix-layouts

![](img/300f9d43a687482454975843b7356b64.png)

## cuda 硬件约束

https://docs.nvidia.com/cuda/cuda-c-programming-guide/index.html#features-and-technical-specifications

| 开始支持 | Compute Capability | PTX ISA version | max | max | max | max |  |

| --- | --- | --- | --- | --- | --- | --- | --- |

| A100 | `sm_80` | `PTX 7.0` | 2048 | 64 | 32 | 2048 / 32 = 64 |  |

| RTX 4090 | `sm_89` | `PTX 7.8` | 1536 | 48 | 24 | 1536 / 24 = 64 |  |

| H100 | `sm_90a` | `PTX 8.0` | 2048 | 64 | 32 | 64 |  |

| B200 | `sm_100` | `PTX ISA 8.6` | 2048 | 64 | 32 | 64 |  |

| 5090 | `sm_120` | `PTX ISA 8.7` | 1536 | 48 | 32 |  |  |

![](img/c4875bbfe5764bcbe75317ae1362ff44.png)

## deviceQuery

H100 PCIe

5120/8 * 1.593 * 2 /1000 = 2.039 TB/s

![](img/13378e824802ff75118d56e18570ea38.png)

A100-SXM4-40GB

5120/8 * 1.215 * 2 /1000 = 1.55 GB/s

![](img/bdce3f02718eca020baf46b58d1ecbd0.png)

# PTX vs SASS

https://zhuanlan.zhihu.com/p/707409842

开心的小福 - 从PTX到SASS：编译器视角

2024.07

`PTX`指令明确地使用了寄存器，支持 Predicated Execution，也有五花八门的操作符(例如`ld`的各种cache控制)。

但到`SASS`仍旧路途遥远，一些是保证程序正确性的转换，更多的则是优化。

`PTX` 是为了兼容，到 `SASS` 不全是一对一映射。

## 功能性转换

### 数据依赖

CPU 和 GPU 中都有 记分牌 (score board)，区别在于，

CPU的记分牌是由硬件在指令发射时分配和自动跟踪的，

GPU不自动跟踪，需要编译器或手工设置。

有些论文就是通过手工修改指令流的二进制编码来改变记分牌（或其他控制功能），进而提高性能。

例如，对于一个 `RAW` (read after write) 依赖，

编译器必须设置 源指令 在完成后更新`记分牌A`的状态，

而目标指令必须等待`记分牌A`。

在执行时，若`记分牌A`的状态标识源指令未完成，则不发射目标指令。有点类似于OS课程中的信号量概念。

抛砖引玉，读者们可以思考如何解决跨基本块的依赖且性能损失最小。

这就是一个 `producer-consumer` 的 computing model。

### branch divergent & barrier

在Independent thread scheduling 时代(`V100` 引入，改变了 `warp-synchronicity` 的行为)，

如果不进行同步，在汇合处，走不同分支的线程将会继续不同步地执行下去，严重影响资源使用率和性能。

编译器会自动插入 `barrier` 同步指令，解决此问题。

在性能方面也有一些考量，比如没必要在每对`divergent branch`和`rejoin point`都插入同步，一指令数增加，二是可能对性能影响不大。

## 优化

大多数人心中的疑问是：`PTX`已经够底层了，到`SASS`还能有什么优化？

其实所有的平台相关优化都是在这一层做的，包括但不限于大家耳熟能详的后端三板斧：指令选择，指令调度和寄存器分配。

熟悉`LLVM/NVPTX`的朋友又会问，这些不都已经做过了吗？我简要说两点原因:

1.  最重要的指令调度和寄存器分配均未达到生产要求。小插曲：有一次我误改了指令调度的一个方法，导致某`kernel`性能下降了22%，我要表达的是这些家伙需要精雕细琢，因地制宜。

1.  `PTX` 毕竟是为了兼容而生，到 `SASS` 不全是一对一映射，可能会产生新的指令甚至新的基本块，因此在`PTX`层次可能无法看到最终的程序。

Lowering过程也会产生别的各类优化机会，最简单地如窥孔优化。但其实在GPU编译器里，就连窥孔优化也不全是简单地识别-转换即可，因为延长某寄存器的生命期可能会导致溢出，而回报可能只是少执行了一条延迟不高的指令。

由此可见，藏在 `PTX` 层底下的，实际上是一个庞大的分析和优化 pipeline。

理论上包含平台相关的优化足矣，就是那些类似于 `LLVM/CodeGen` 目录底下的。但点开Transforms底下的PASS也可以看到，大家或多或少都使用了Target hook来查阅信息并决策，因此这个pipeline是非常巨大的，甚至也囊括了CFG变换和循环优化。

（谈及循环优化，上周令我感到惊奇的事是，一个很长的程序在经过循环展开后，指令数反而大大减少，经仔细研读，发现是暴露的优化机会几乎全被充分利用了）

循环里有根据归纳变量的值走不同分支，那么展开后可以只保留走下去的分支的基本块。循环本身的判断，跳转指令也都可以省掉。

展开后，访存指令也可以向量化，运算指令可以做strength reduction。还有就是我们的循环优化比较早期，每次迭代都用不少公共运算，`reassociate`, `cse`后，让后续不公共的运算也简单了。

访存指令的 地质模式其实也能fold不少

由于GPU和CPU的结构差异，那些在CPU编译器中的同类PASS会有较大改动。譬如，为了追求并行度，可以忍受更多溢出，但为了折衷就得增加新的分析。

下文简要阐述两个GPU专有的优化，基本都和寄存器相关，毕竟寄存器的使用量直接影响了并行度和延迟，从而重塑了几乎所有的优化。另一个重要影响因素是针对纹理的优化，但本文是给写算子的CUDA用户扫盲，所以懒得写了。

### 使用 Uniform Register

对于WARP里所有线程的值都一样的变量，如线程块ID，可以放在`uniform register`中存储和运算，使用这类寄存器可以显著降低读操作数的延迟，以及更高效地利用功能部件。

第一步是通过数据流分析，从程序中找出所有线程都一致的值（也包括分支指令）。甚至某些不一致的值也能装到`uniform register`中运算。

鉴于`Uniform Register`的诸多好处，在不破坏各线程对值的“可见性”前提下，我们也会尽力分析一些复杂的控制流。

分析跑完后，将选择将哪些值放到`uniform register`里，毕竟寄存器总是有限的，而且频繁地在两类寄存器中移动数据可能得不偿失，大家可以理解为一种寄存器溢出。

决策过程的影响因素比较多，记不住，就先不说了。

### 溢出到 共享内存

动机是访存延迟，但过量使用共享内存会降低线程块间的并行度。针对该优化没什么可讲的，道理就是这么简单。诚然，`NVPTX`后端也可以完成类似优化，但如上文所述，此时的程序可能是不精确的。

与其他可以在`NVPTX`层实现，但未实现的优化一样，如果和`SASS`层各优化一次，那么使用非 `NVCC` 的用户可能要骂为什么编译这么慢了。

## 内联 PTX 有用吗？

既然到`SASS`过程里，`PTX`只是 `IR` 的角色，那内联“汇编”还有用吗？

老衲在23岁时候优化过一个CUDA程序，就是通过用加载指令+特定的cache操作提升了性能。

我不太清楚CUDA里有没有提供C++的接口，但这种情况下，编译器无法分析得出，可以考虑使用内联汇编。

## PTX 之前

有很多非常重要的优化（我最近就学习了一个），但我确实不熟，而且有些历史原因我不太熟悉，就不评论了。

其他 case

![](img/cd6b71140c95915bee8c565a2000294c.png)
