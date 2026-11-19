
# 图解Vllm V1系列1：整体流程

> 来源：[https://v11enp9ok1h.feishu.cn/docx/FFSodQCExoElTvxXz1JcUwizn3d](https://v11enp9ok1h.feishu.cn/docx/FFSodQCExoElTvxXz1JcUwizn3d)

## 一、整体流程

Offline batching下的整体流程如下图所示，在后文中，我们称请求为`req`：

![](img/ddbaab6504872e1f98c381b404e91cd8.png)

我们先忽略图中七七八八的细节，从大面上来看一下这个流程：

+ 首先，观察到我们采用2个不同的进程（`process0`，`process1`）来完成整个推理。

+ `process0`上维护的核心对象是`SyncMPClient`（Sync MultiProcessing Client），我们简称其为客户端。`process1`上维护的核心对象是`EngineCoreProc`，我们简称其为`EngineCore`。

+ `vllm`使用`ZMQ`来负责不同进程间的数据收发（不了解`ZMQ`的朋友，到此为止只要先了解它的主要功能是这个即可，后续再慢慢了解它的其余细节）

+ 观察到：
  + 对于`req`的输入输出处理(`processor`、`output_processor`)是由客户端执行的；而`req`的实际推理过程(`Scheduler` -> `ModelExecutor`)则由`EngineCore`来执行。
  + 也就是说，`vllm v1`将cpu和gpu上的调度拆成2个进程进行处理，这样可以更好实现cpu和gpu的overlap。举例来说，在先前`vllm`版本的推理中，我们经常会发现`detokenize`（`tokenid` -> 文本）这个步骤会成为推理瓶颈，特别是遇到长序列的情况。在等待序列做`detokenize`的过程中，gpu是空转的。现在通过这种拆解，让原来是串行的事情并行化。

最后，客户端的类型有很多种，由于本节我们讨论的是offline batching，所以展示的是`SyncMPClient`。在下一节中，我们会来看其余类型的客户端。

有了以上大体上的感知，现在我们可以来介绍图中细节了：

1、`add req to`：LLM端接收`req`，并将`req`发送至`LLMEngine`的`processor`。

2、`process req`：原始`req` -> `EngineCoreRequest`，`processor`的作用是对`req`进行预处理，具体包括：

+ `Tokenize`

+ 验证输入参数的合法性

+ 将`req`封装成特定形式(`EngineCoreRequest`)等等

3、`encode & send req`：

+ 将`EngineCoreRequest`发送给`SyncMPClient`

+ `SyncMPClient`会对其做`encode`操作：`EngineCoreRequest` -> `pickle EngineCoreRequest`

+ 通过`zmq` `input_socket`，将`pickle EngineCoreRequest` 发送给 `EngineCoreProc`（这里`SyncMPClient`和`EngineCoreProc`分别运行在2个不同的进程上，`vllm`选择使用`ZMQ`来负责进程间的数据通信）

------------------ 进程分割线（Client -> EngineCoreProc） ------------------

4、`recv & decode req`：

+ 在`EngineCoreProc`所在的进程上，会启动一个线程，在该线程中执行`process_input_socket()`函数，这个函数具体做了如下事情：

+ 持续监听通过`input_socket`传来的`pickle EngineCoreRequest`

+ `decode`：（`pickle EngineCoreRequest` -> `EngineCoreRequest`）

+ 将 `EngineCoreRequest` 装入 `input_queue` 队列中

5、`add req to scheduler`：`EngineCoreRequest` 被添加进 `Scheduler`的`waiting`队列中

6、`one step inference`

+ `Scheduler`执行一步调度，从`waiting`和`running`队列中选择`req`进行推理，输出一步推理后的结果(`EngineCoreOutputs`)。这边的细节我们暂时略过，留到后文细说。

7、`put output to output_queue`：将一步推理结果放入 `output_queue`队列中这里我们会重复执行5/6/7步骤，即重复执行【一步调度->一步推理】的过程，直到 `input_queue` 和 `scheduler`中再无`req`存量。

8、`encode & send req`:

+ 在`EngineCoreProc`所在的进程上，会启动一个线程，在该线程中执行`process_output_socket()`函数，这个函数具体做了如下事情：

+ 持续监听`output_queue`中装入的一步推理输出结果(`EngineCoreOutputs`)

+ `encode`：`EngineCoreOutputs` -> `pickle EngineCoreOutputs`

+ 使用`zmq` `output_socket`作为通信管道，将`pickle EngineCoreOutputs`发送给`SyncMPClient`

-------------- 进程分割线（EngineCoreProc -> Client） -------------

9、`recv & decode req`

+ 在`SyncMPClient`所在进程上，会启动一个线程，在该线程中执行`process_output_socket()`函数，这个函数具体做了如下事情：

+ 持续监听通过`output_socket`传递来的`pickle EngineCoreOutputs`

+ `decode`：`pickle EngineCoreOutputs` -> `EngineCoreOutputs`

+ 将 `EngineCoreOutputs` 装入 `output_queue` 中

10、`process_output`，对`EngineCoreOutputs`做一些后置处理，例如`detokenize`等操作。

11、`output until all reqs are finished`：LLM回去检测每一条`req`是否已做完推理(`req.finished`的状态)，由于目前我们是offline batching的模式，所以等到全部的`req`都完成推理后，LLM会一起输出推理结果。

## 二、Online Serving

### 2.1 调用方式

这里我[假设](https://github.com/vllm-project/vllm/blob/4283a28c2fb7595c5d72d3edc7ad356c87273d94/vllm/entrypoints/openai/api_server.py)起的online serving是这份脚本：脚本

### 2.2 整体流程

![](img/6618a664b7fd32b5611bbc005734660d.png)

我们还是先从整体上来感知online serving的运作流程：

+ 和offline batching相比，`EngineCoreProc`部分的逻辑没有变

+ `Client`和`EngineCoreProc`交互的逻辑没有变

+ 所以现在，我们只需要关注`Client`的实现细节

Online Serving的`Client`实现细节如下：

1、`async per req generate`：用户发送`req`给`vllm`的API server，server对单`req`发起`generate`操作。这里会首先将`req`发送给`AsyncLLM`

2、`create asynico.queue() for each req`和`register the queue to output_processor`：

+ 这两个步骤都在`AsyncLLM`上执行

+ `create asynico.queue() for each req`：会针对每一个`req`创建一个异步队列，这个异步队列将用于存储这个`req`的输出结果。

+ `register the queue to output_processor`：会把这个`req`的异步队列注册到`output_processor`中。

+ 为什么每个`req`需要一个单独的队列：

  + 在online serving中，`req`的输出结果是流式的（一块一块地把结果返回给用户）

  + 在online serving中，用户时常有多轮对话的需求等等

  + 基于这样的考虑，相比于offline batching对所有`req`的一次性统一输出，online serving下更有必要将各个`req`的输出隔离开来，因此才单独针对每个`req`创建一个用于盛放输出结果的异步队列。

3、`prcocess req` 和 `4、encode & send req`和offline batching逻辑基本一致，这里不再赘述

-------------------- 进程分割线（Client -> EngineCoreProc） -------------------

`EngineCorePro`会实际执行`req`的推理过程，5~9的步骤我们在offline batching中详细解释过，这里不再赘述

在`AsyncLLM`上，我们会启动一个异步任务`asyncio.create_task(_run_output_handler())`，这个异步任务上会执行`_run_output_handler()`函数，这个函数的主要作用是异步接受、处理来自`EngineCore`的模型输出结果，具体流程如下：

10、`AsyncMPClient`上发起`get_output_async()`：

+ 持续监听通过`output_socket`传递来的`pickle EngineCoreOutputs`

+ `decode`：`pickle EngineCoreOutputs` -> `EngineCoreOutputs`

+ 将 `EngineCoreOutputs` 装入 `output_queue` 中

11、`split output into slices`：

+ 将`output_queue`中的数据切成`slices`，方便后续做流式输出

+ 将`slices`数据装入这个`req`注册在`output_processor`中、只属于这个`req`的异步队列

+ `output_processor`将会以`slices`为维度，对输出数据做诸如`detokenize`等的操作。

12、`continously yield current output`：

+ 对于一条请求，`AsyncLLM`将会持续从`output_processor`中获取它当前的输出结果，返回给用户，直到这条请求推理完毕

此外，`Vllm`官方文档中，也给出了一张online serving的架构图上面的内容可以说是这张架构图的细节扩展，大家可以比对进行理解：

![](img/24c18d7c9c290371d9eb5bdf83de1c41.png)

到此为止，我们就把V1的大致运作流程介绍完了，在后面的文章中，我们会来看关于调度器、`KV Cache`管理和分布式模型执行的更多细节。
