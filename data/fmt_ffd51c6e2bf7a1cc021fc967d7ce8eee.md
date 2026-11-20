
# CUDA 事件与流同步

```
cudaEvent_t event;//声明一个event
cudaEventCreate(cudaEvent_t* event);//命令GPU创建一个event
cudaEventRecord(cudaEvent_t event,cudaStream_t running_stream); //把这个stream 安排给某个stream,你可以把stream，看成一个先进先出的队列，event被塞进了这个队列，也就说等它前面已经声明的任务执行完了之后，event才会发生。
cudaEventSynchronize(cudaEvent_t event); //命令CPU主线程等待这个event发生，在event发生之前，CPU会阻塞在这行代码处。
cudaStreamWaitEvent(cudaStream_t waiting_stream,cudaEvent_t event, 0);//命令一个stream等待一个event发生，这个event可能被植入别的stream了，那么通过这种方式，两个stream就实现了交互和同步等操作
cudaEventDestroy(cudaStream_t event);//用完别忘说分手。

cudaDeviceSynchronize();现在我们理解这个cudaDeviceSynchronize()就是等所有stream中已经安排的任务跑完。
```

```
for(int i = 0; i < visitor_size; i++){
    if(i!=0){
        cudaStreamWaitEvent(stream[i],kernel_cal[i-1],0);
    }
    maxtrix_mul<<<grid,block,0,stream[i]>>>(i);
 
    cudaEventRecord(kernel_cal[i],stream[i]);
}
```

为什么`kernel`都可以互相overlap，但是`memcpy`不行呢？

没错GPU编程的一大优化思想就是把能overlap的东西尽量overlap掉。

本小节我们就介绍怎么让使用非阻塞的`MemcpyAsync`。

GPU端启动了很多`stream`，那么这些`stream`之间是怎么知道彼此运行到哪了，万一有程序这种需求呢，就和CPU的`thread`需要通讯一样，就和`block`里的`thread`需要`__syncthreads()`一样。

这种需求，我们通过`event`来解决。

`event`就和赛车的照相点一样。它把自己加入某个`stream`的赛道，然后你就可以通过它观测和控制某个`stream`的状态了。

具体的，他有以下几个API：

```
cudaEvent_t event;//声明一个event
cudaEventCreate(cudaEvent_t* event);//命令GPU创建一个event
cudaEventRecord(cudaEvent_t event,cudaStream_t running_stream); //把这个event 安排给某个stream,你可以把stream，看成一个先进先出的队列，event被塞进了这个队列，也就说等它前面已经声明的任务执行完了之后，event才会发生。
cudaEventSynchronize(cudaEvent_t event); //命令CPU主线程等待这个event发生，在event发生之前，CPU会阻塞在这行代码处。
cudaStreamWaitEvent(cudaStream_t waiting_stream,cudaEvent_t event, 0);//命令一个stream等待一个event发生，这个event可能被植入别的stream了，那么通过这种方式，两个stream就实现了交互和同步等操作
cudaEventDestroy(cudaStream_t event);//用完别忘说分手。

cudaDeviceSynchronize();现在我们理解这个cudaDeviceSynchronize()就是等所有stream中已经安排的任务跑完。
```

这样讲起来比较抽象，那么我们通过一个具体的例子来理解。

`vi event_test1.cu`

我们看到这样的代码：

```
for(int i = 0; i < visitor_size; i++){
    if(i!=0){
        cudaStreamWaitEvent(stream[i],kernel_cal[i-1],0);
    }
    maxtrix_mul<<<grid,block,0,stream[i]>>>(i);
    cudaEventRecord(kernel_cal[i],stream[i]);
}
```

![https://s3-us-west-2.amazonaws.com/secure.notion-](img/4ae504763b7b22781a1731bd14e17602.png)static.com/2aad1f56-325d-40e4-bd41-afb634269f76/Untitled.png

实际上`stream`的名称已经指出了它的设计理念：就是一个先进先出的任务队列，所以`event`就是在队列里埋下的标记点。

基于这种思路，我们重新回到`matrix_mul_multi_kernel_slow.cu`，换种思路，想一想我们不并行`kernel`，而是把每次的数据传输和上一次的`kernel`计算重叠起来，形成一个流水，像这样：

![https://s3-us-west-2.amazonaws.com/secure.notion-](img/a791bb1e03e57360f04f6d6a4ca6b6c2.png)static.com/5bb9eb37-de78-4612-94d0-420a49007a72/Untitled.png

同一个方向的`memcpy`不能互相overlap?因为PCIe虽然是双工的，但是GPU只有一个HtoD的copy引擎加一个DtoH的copy引擎啊。

我们按照这个思路实现了`matrix_mul_io_after_overlap.cu`

接下来我们通过实验看看速度提升：

```
nvcc matrix_mul_io_after_overlap.cu -o run_af
nvcc matrix_mul_io_before_overlap.cu -o run_bf
./run_af
./run_bf
```

实际上因为矩阵乘的`kernel`计算太慢了，显示不出来io被流水的效果，我们代码里使用了一点小trick。让`kernel`不真正计算而是使用这个函数休息一会(1秒左右)：

```
__device__
void sleep_awhile(){
    for(int i = 0; i < 1*1500; i ++ )
    {
        long long int start_clock = clock64();
        long long int clock_offset = 0;
        while (clock_offset < 1000000)
        {
            clock_offset = clock64() - start_clock;
        }
    }
}
```

请你对照代码理解`memcpyAsync`的工作方式，并为`matrix_mul_io_after_overlap`画出类似上图的时序示意图

你知道吗？在CUDA7之前，`default stream`上所有的操作，默认都是同步阻塞的，所以当你写了异步代码，但是它不work的时候，你可以看看这两个网页：

[如何重叠数据传输](https://devblogs.nvidia.com/how-overlap-data-transfers-cuda-cc/)

[GPU Pro Tip: CUDA 7 Streams Simplify Concurrency](https://devblogs.nvidia.com/gpu-pro-tip-cuda-7-streams-simplify-concurrency/)

### GPU 开发

```
__device__
void sleep_awhile(){
  ...
}
 

__global__
void kernel(){
    sleep_awhile();
}
```

这里涉及几个指示符：

1.  `__global__` 表示该函数是CPU调用GPU的接口`kernel`函数。

1.  `__device__` 表示该函数是GPU端代码，只能在`kernel`内执行。

1.  `__host__` 表示该该函数是CPU端代码，可省略。

但是有一种特殊情况：

```
__device__ __host__
void double_side_func(){
  ...
}

__global__
void kernel(){
    double_side_func();
}

int main(){
    kernel<<<1,1>>>();
    double_side_func();
}
```

带有`__device__` `__host__`标记的函数既可以在GPU上使用，又可以在CPU上使用。

## 自主练习题

编译执行`matrix_mul/test_device.cu`观察输出并学习代码。

请尝试解释`ds`对象是如何被传输到GPU上的，他的函数表又是怎样的。如果想不出来，请请教你的mentor。

# Profiling & Optimizing

搭建一个CUDA profiling知识树，以一个样例程序作为引导，使像我一样的初学者掌握CUDA profiling的以下7点内容：

1.  GPU程序性能分析的主要关注点是什么。

1.  GPU程序的性能分析流程是怎样的。

1.  GPU程序的并发是怎样的，在profiling的时候应该注意什么。

1.  `nvprof`(nvidia profiler)是什么，怎么用。

1.  `nvvp`（nvidia visual profiler）是什么，怎么用。

1.  `nsight compute`是什么，怎么用。

1.  要查拓展资料怎么查。

要做到这些，我们可能需要一些背景知识，包括：

+   `cuda`编程的基本概念（我默认读者有一定的基础）

+   NVIDIA GPU的构架（下文会给出简单的讲解）

## 配置要求

[Untitled](https://www.notion.so/2a33146075c04180a688cf68a56da7b1?pvs=21)

## 示例

```
which nvcc
/usr/local/cuda/bin/nvcc
cp /usr/local/cuda/samples ./
cd simples/0_Simple/vectorAdd
make
```

得到一个简单的向量加程序。

```
nvprof ./vectorAdd (./不要掉了)
```

![https://s3-us-west-2.amazonaws.com/secure.notion-](img/e4c7e0b2279acd2c312089b3b8fe2dad.png)static.com/f270c04f-3f01-487b-bde0-6154786658cb/Untitled.png

可以看到，`nvprof`执行成功后返回了2个信息：

1.  `Kernel`和`CUDA memcopy`的耗时。

1.  `CUDA API`的调用耗时。

1.  对于一个程序，因为`CUDA`中并发过程较多。所以总耗时并不能简单计算（因此提升性能也不是简单的优化耗时最多的地方）

好，我们现在跑通了第一个测试，终于进入正轨。接下来安心看一下框架吧。

### nvprof 框架

在了解GPU的构架之后，理解`nvprof`的框架有助于我们理解`nvprof`的工作原理。

实际上，GPU的性能报告之所以能被生成，有两个重要支持：

1.  GPU有自己的运行状态`metrics`。`nvprof`可以将其为对应的`kernel`提取出来。

1.  同时代码中也有各种自定义或者预设的`events`。代码运行时，`nvprof`会记录这些`events`发生的时刻和次数等信息，有助于我们理解具体代码段的执行情况。

1.  `nvprof`正是在执行的过程中生成`timeline`数据，不断地把`metrics`和`events`整理进去从而生成一个有很多信息的`timeline`，从而帮助我们理解程序运行过程的工具。

#### metrics

```
nvprof --query-metrics
// or
/usr/local/cuda/NsightCompute-2019.3/nv-nsight-cu-cli --query-metrics
```

可以看到会有各种`l2_utilization`、`shared_utilization`等统计信息。

![https://s3-us-west-2.amazonaws.com/secure.notion-](img/fdbba7948d3bc090950f8bc6607ac361.png)static.com/f147484d-f0f6-43a9-8d02-2c51481336b7/Untitled.png

如果报错如下，用 `nsight`

![https://s3-us-west-2.amazonaws.com/secure.notion-](img/99232f70731151fe6d0fa7572bff8330.png)static.com/6d8c7198-cb28-4cd5-8d25-f1f0561fca0a/Untitled.png

#### events

因为7.5以上的构架不支持`event`

![https://s3-us-west-2.amazonaws.com/secure.notion-](img/a5fde006213e03b662c4e40f7b23f067.png)static.com/90a70dd0-81db-4fc3-8f47-e7b9bb4ccb22/Untitled.png

### nvidia-smi

`nvidia-smi dmon -i 0`

`enc`(视频编码模块使用率)

`dec`（视频解码模块使用率）

`mclk`（内存频率，一般GPU有两个内存频率，分别对应idle和使用中）

`pclk`(processor clock, 忙时频率取决于超频设置)

从第6行开始，由于有程序加入，GPU的频率突然提升，耗电和温度都在上升。

![https://s3-us-west-2.amazonaws.com/secure.notion-](img/84cec62bc1f353245e9e4ee4b455a214.png)static.com/f3c96afa-7995-4f42-ab53-595e26120bf4/Untitled.png

有时GPU常常会因为设置不当、功率限制、过热保护等原因，无法充分发挥性能，这时候需要调研和确认，执行命令：

```
watch -n 1 -d nvidia-smi -q -d performance -i 0
```

![https://s3-us-west-2.amazonaws.com/secure.notion-](img/6167e38fa76c89c267663141d15e26a9.png)static.com/28f5c608-25dd-4971-bf2b-617cbc0e7eb8/Untitled.png

使用`tmux` 再开一个`pane`

```
cd samples/1_Utilities/bandwidthTest
make
./bandwidthTest
```

![https://s3-us-west-2.amazonaws.com/secure.notion-](img/ea47a36be19b6c2cd216b42cde3c2769.png)static.com/56a1e507-296c-41dc-8b79-901b6363373b/Untitled.png

发现`Performance`跳到了`P0`(`P12`最低，`P0`最高，如果没有到`P0`，你的`GPU`卡可能需要正确配置来提高性能)，`Idle`变成了`Not Active`。

如果任何`Slowdown`选项出现`active`，证明`GPU`因为温度（HW）或者功率原因出现了性能下降。请你虐待它。

### 超频

（有的`GPU`支持自动超频，因此你可以跳过这一步，如果上面两步显示出来跑程序的`clock`不是最高的，再做）

首先执行

```
nvidia-smi -q -d support_clocks -i 0 #0是你要超频的`GPU`编号，不是我随便填的
```

![https://s3-us-west-2.amazonaws.com/secure.notion-](img/7da2ed190b84a060fcb244583707ed88.png)static.com/7c3a5e39-4d99-42e7-8698-6a0a94946945/Untitled.png

本例中发现`T4`卡的最大内存频率为5001，最大处理器频率为1590。

因此执行以下命令进行超频：

```
sudo nvidia-smi -pm #保证任何设置长期有效
sudo nvidia-smi -acp 0 #`GPU`不限制应用性能
sudo nvidia-smi -ac memClock,processorClock #指定`GPU`频率
sudo nvidia-smi --auto-boost-default=0
```

检验超频是否成功：

```
sudo nvidia-smi -q -d clock -i 0
```

![https://s3-us-west-2.amazonaws.com/secure.notion-](img/85b42e6fd16bbb06e8f655b5acff97bb.png)static.com/0a432a0c-2a11-4f9e-9fcc-1ff0405de0dd/Untitled.png

## nvprof
