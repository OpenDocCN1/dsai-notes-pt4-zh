
# 图解Vllm Executor-Workers架构

> 来源：[https://v11enp9ok1h.feishu.cn/docx/IkUZdzjLqoVvnaxqrsbc1Rz1nee](https://v11enp9ok1h.feishu.cn/docx/IkUZdzjLqoVvnaxqrsbc1Rz1nee)

## 一、Executor的类型

在`vllm`中，`Executor`一共有4种类型，由配置参数`--distributed-executor-backend`决定，相关的代码和文档参见：

对于`--distributed-executor-backend`，默认情况下为`None`，你当然也可以手动指定。在默认情况下，`vllm`会根据你的分布式配置（`world_size`）和你所使用的平台特征（`cuda`、`neuron`、是否安装/初始化/配置了`ray`等）来自动决定`--distributed-executor-backend`的取值。

我们来简单介绍下这4种类型的`Executor`。

（1）`mp`：`MultiprocExecutor`

+   适用场景：单机多卡。当单机的卡数满足分布式配置，且没有正在运行的`ray pg`时，默认使用`mp`

+   在`mp`的情况下，`Executor`成为一个主进程，其下的若干个`workers`构成了它的子进程们

（2）`ray`：`RayDistributedExecutor`

+   适用场景：多机多卡

+   在`ray`的情况下，`Executor`成为一个`ray driver process`，其下管控着若干`worker process`

（3）`uni`：`UniProcExecutor`

+   适用场景：单卡或 `Neuron`环境

（4）`external_launcher`：`ExecutorWithExternalLauncher`

+   适用场景：想要用自定义的外部工具（例如`Slurm`）来做分布式管理

注意：以上的“适用场景”描述的是一般情况，更具体的细节，请参见“决定`executor`类型”的相关代码。

在本文中，我们将以`mp: MultiProcExecutor`进行讲解，并假设这里的分布式配置仅用了`tp`，其余的细节留给用户自行阅读。

## 二、Executor -> Workers

### 2.1 整体架构-官网版

![](img/39a741daede03e1c7c73a437678a743b.png)

上图右侧刻画了`V1`的架构：

+   `Scheduler`和`Executor`都位于`EngineCoreProc`所在的进程上。如本文第一章`offline batching`的流程图所示，`Scheduler`决定单次调度步骤中要送去推理的请求，并将这些请求发送给`Executor`。

+   一个`Executor`下管理着若干`workers`，每个`workers`位于独立的进程上，可以理解成一个`workers`占据着一张卡

+   `Executor`负责把请求`broadcast`到各个`workers`上

+   各个`workers`接收到请求，负责执行实际的推理过程，并将推理结果返回给`Executor`。

相比于`V0`，`V1`这种架构设计的优势在于：在`V0`中，`worker0`既要负责调度和数据分发、又要负责实际的推理计算。如此一来，各个`workers`间存在负载不均的问题，而`worker0`将成为性能的瓶颈。而`V1`通过拆分【调度】和【计算】过程解决了这个问题。

### 2.2 整体架构-细节版

现在我们已经通过`vllm`官方的示例图，初步了解了`V1`下`Executor-Workers`的架构，现在我们扩展这张架构图，来看更多的细节，为了画图简明，这里我们只展示了其中1个`worker`，没有画出全部`workers`：

![](img/987297e3003eafe66a0b345ceb91db2a.png)

上图展示的是使用`MultiprocExecutor`下的架构，如前文所说，该类型的`Executor`常被用于单机多卡的推理场景，我们按照从上到下，从左到右的顺序来解读上图。

1、`MultiprocExecutor`和`Scheduler`都位于`EngineCoreProc`所在的进程中。`Scheduler`负责决定单次调度步骤中，要送去推理的`reqs`，并将这些`reqs`传递给`MultiprocExecutor`。

2、在`MultiprocExecutor`上，我们将创建一个`rpc_broadcast_mq`队列：

+   该队列存储着`Executor`要`broadcast`给各个`workers`的【小数据（<=10MB）】，而【大数据（>10MB）】则不会进入此队列，而是通过`zmq socket`进行传输

+   每条数据可以被粗糙理解成是`(method, data)`的形式，`data` = 数据本身，`method` = 你期望`worker`上调用什么样的方法来处理这条数据。

+   针对这个队列、以及大小数据的传输细节，我们将在本文第三部分详细介绍。

3、在`MultiProcExecutor`上，通过`make_worker_process`创建子进程：

+   每个进程占据一张卡，每个进程上运行着一个`worker`实例

+   在创建每个子进程时，我们会将`rpc_broadcast_mq_handler`，也就是输入队列的句柄也传递给子进程，这里你可以粗糙将“`handler`（句柄）”理解成是一个“地址”，有了这个地址，每个子进程才知道要去哪里找到并【连接】这个队列，以此读到队列中的数据。相关细节我们同样在后文给出。

4、每个`Worker`实例又由以下几部分组成：

+   `WorkerWrapper`，每个`Worker`实例都有属于自己的`WorkerWrapper`。你可以将它形象理解成是一个`worker`的`manager`，它负责管控一个`worker`的生命周期（创建->销毁）、所占资源、扩展功能（例如在`rlhf`场景下的一些功能）等等。

+   `Worker`，真正的`Worker`实例，它上面维护着两个重要队列：

+   `rpc_broadcast_mq`：正如3中所说，单个`worker`通过`rpc_broadcast_mq_handler`这个句柄，连接上了`Executor`的`rpc_broadcast_mq`队列，这样它就能从这个队列中读取`(method, data)`数据。注意，这里说的是【连接】而不是创建，为了强调这一点，图中单`worker`上的该队列用虚线表示。

+   `worker_response_mq`：单个`worker`【创建】的、用于存放这个`worker`上推理输出结果的队列。同样也会产出`worker_response_mq_handler`这个句柄。后续这个句柄将通过`zmq socket`传送给`Executor`，让`Executor`可以连接上这个队列，这样`Executor`就可以获得每个`worker`的输出结果。

+   `ModelRunner`，一个`worker`实例下维护着一个`ModelRunner`实例，这个实例上维护着模型权重分片（`model weights sharding`）、这块卡上的`kv_caches`、`attn_backend`等一系列的具体模型信息，它将最终负责模型权重的加载（`load_model`），并执行实际的推理过程。

5、连接`Executor`和`Worker`的`ZMQ sockets`：

+   `Executor`和`Worker`分属不同的进程，这里依然采用`ZMQ sockets`做进程间的通信。

+   这里其实创建了多个不同`socket`（为了表达简便，我统一画成`ZMQ sockets`），每个`socket`会用于不同内容的通信，例如：

+   `ready_socket`：`worker`进程向`Executor`发送`ready`信号 + `worker_broadcast_mq_handler`

+   `local_socket`：如前文所说，除了使用上述的2个队列做`Executor->Worker`间的输入输出通信外，我们还会直接使用`local_socket`做输入输出通信。前者用于单机内快速通信较小的数据（<=10MB），后者用于通信大数据（>10MB）。我们会在后文细说这一点。

+   等等

6、`worker_busy_loop()`：在`worker`上启动`busy loop`，持续监听`Executor`发送的数据、做推理、并将推理结果持续返回给`Executor`。这样一来，这个`worker`就无限运转起来了，除非收到用户信号，显式终止掉这个`worker`，否则这个`busy loop`不会停止。

---

> 到此为止，我们简单总结一下在`Executor->Workers`的初始化环节都做了什么事：

+   首先，按照上图所示，创建了`Executor->Workers`架构，特别注意上述2个输入输出队列的初始化和连接。

+   对于每个`Worker`，我们通过`init_device()`，将它绑到指定的卡上，并对它做分布式环境初始化（即制定它的分布式通信`group`）

+   对于每个`worker`，我们通过`load_model()`，当`ModelRunner`实际去加载这个`worker`所要的模型分片

+   在每个`worker`上启动`run_busy_loop()`，让`worker`持续不断地运转起来。 更多的细节，请大家自行阅读源码。

接下来，我们着重来讨论这`rpc_broadcast_mq`和`worker_response_mq`这两个输入输出队列。

## 三、Executor与Worker间的数据传输机制

我们先快速回顾一下上文的内容：

> （1）在我们的例子中，`Executor`的具体类型是`MultiprocExecutor`，它一般适用于单机多卡推理。

> （2）`Executor`和`Worker`分属不同的进程，`Executor`需要把输入数据`broadcast`到`Worker`上，`Worker`需要把推理的输出结果返回给`Executor`。

> （3）对于小数据（<=10MB），`vllm`使用`rpc_broadcast_mq`和`worker_response_mq`来做数据传输，这两个队列的本质是`ShmRingBuffer`（环形共享缓存），其中`Shm`即我们熟知的`shared_memory`，而`ring`是使用环形的方式往`shm`中读写数据（看不懂也没关系，我们马上来说细节）。

> （4）对于大数据（>10MB），`vllm`使用`zmq socket`来做数据传输。

为什么要设计2种不同的进程间通信机制，来分别处理【小数据】和【大数据】呢？这里简单说几个我能想到的原因：

> （1）首先，通过`shm`的方式读写数据时，不同的进程都从同一块共享内存（`shm`）上直接读取，这样数据不需要从一个进程的地址空间复制到另一个进程的地址空间，也就是可以实现数据的“零拷贝访问”

> （2）其次，通过`shm`的方式读写数据时，可以避免网络协议栈和数据重复写入的开销，可以实现更高效、更快的数据访问。

> （3）那么，既然`shm`这么好，为什么只让【小数据】使用它，而让【大数据】走`zmq socket`呢？这是因为`shm`是一块固定的内存大小，一旦预分配好，就不能被改变了。在实际使用场景中，可能需要传输的数据量本身就不大，只是会偶发出现一些【大数据】传输的情况，因此我们没必要预留更大的`shm`空间，来应对这些只是偶发情况，这样会造成内存的浪费。所以我们额外使用`zmq socket`来处理这些偶发情况。

### 3.1 `ShmRingBuffer`（共享环形缓存）

假设我们现在只使用`tp`，即一个`Executor`下有若干`tp workers`，那么：

+   `Scheduler`生成单次调度的结果，将这些结果传递给`Executor`，我们称单次调度的结果为一个`chunk`

+   `Executor`拿到单次调度的结果，写入`rpc_broadcast_mq`（本质是`ShmRingBuffer`）中

+   这些`tp workers`都需要从`rpc_broadcast_mq`读取这个`chunk`（每个`tp worker`的输入是相同的）

+   各个`tp workers`执行推理，并将推理结果写入各自维护的`worker_broadcast_mq`（本质是`ShmRingBuffer`）中。

+   `Scheduler`继续生成单次调度结果（`chunk`），重复以上步骤。

+   不难发现，对于一个`chunk`，我们总有1个`writer`，和1个或若干个`readers`，例如：

+   `rpc_broadcast_mq`的`chunk`中，`writer` = `Executor`，`readers` = `tp workers`

+   `worker_broadcast_mq`的`chunk`中，`writer` = 某个`tp worker`，`reader` = `Executor`

现在，让我们将`ShmRingBuffer`想象成是一个存储站：

+   这个存储站中有`max_chunks`个柜子，每个柜子用于存储一块数据（`chunk`），`max_chunk`默认值为10

+   每个柜子的最大数据存储量为`max_chunk_bytes`，该值当前默认为10MB

+   每个柜子上有一个面板（`metadata`），这个面板上有`1 + n_reader`个指示灯。其中`1`这个指示灯代表`written_flag`，即用于指示`writer`是否把`chunk`塞进了柜子（写入完毕），`n_reader`个指示灯代表`reader_flags`，分别表示这些`readers`是否已经将这个`chunk`读取完毕。

+   由此可知，对于一个柜子，只有当`writer`写入完毕后，`readers`才可以去读。只有当所有`readers`都读取完毕后，这个柜子里的`chunk`才可以被“废弃”，也就是这个柜子才可以重新回到“可写入”的状态，让`writer`写入新数据。

+   `Scheduler`在做一轮又一轮的调度，产出一个又一个的`chunk`，那么这些`chunk`就按照顺序，依次装入这些柜子中，当这10个柜子的数据都被轮番用过以后，下一次再来新`chunk`前，就从0号柜开始复用起（当然要按照上条所说的，检查该柜子是否达到可复用状态），这种环形使用的方式，称之为“ring”。

有了以上这个形象的理解，现在我们再回过头来看`vllm`代码中的这部分注释，就不难读懂了，而关于代码的更多细节，请大家自行阅读源码：

![](img/70e1ecc30fde9ccfe56b9988b476a9da.png)

### 3.2 `zmq socket`

正如前文所说，在`Executor`和`Worker`间做大数据（>10MB）的传输时，可以使用`zmq socket`，这块就是传统的`zmq socket`构建流程了，没有太多可说的。这里我们想结合`worker.worker_busy_loop()`（也就是一个`worker`持续读取输入、进行推理、写入输出）的过程，来具体看一下`shm`和`zmq socket`是如何配合运作的。

![](img/6e73c6fbe104bc8acc051782ef52103c.png)

从代码中我们发现一件有趣的事：这里好像只从`shm`上读取数据，并没有通过`zmq socket`呀！不要紧，我们现在深入`rpc_broadcast_mq.dequeue()`中一探究竟。

![](img/0c5c614ff465f98de42cee0068e7bb9d.png)

整体上说：

+   当一个`chunk`过来时，我们会先检查它的大小：

+   如果<=10MB，则装入`shm`对应的柜子中

+   如果 > 10MB，则先在`shm`对应的柜子中记上一笔（`buf[0]==1`），然后再通过`zmq sokcet`去`send`这份数据

+   以上过程不在所提供的代码截图中，需要大家自行找相关代码阅读

+   接着，我们会根据这个柜子的标志`buf[0]`是否为1，来检测对应的`chunk`是装在柜子里，还是通过`zmq socket`发送了。如果是前者，那么直接从`shm`读；如果是后者，那么就通过`zmq socket`做`receive`。

+   最后，如果当前`chunk`是大数据，虽然它不会装在对应的柜子里，但我们也会认为这个柜子已经被使用过。这样后一个`chunk`来的时候，它不会使用当前的柜子，而是使用下一个可用的柜子。

+   你可能会发现，在上述`dequeue`的代码中，存在2种`zmq socket`：`local_socket`和`remote_socket`，前者用于单机内的通信（`writer`和`readers`在一台`node`内），后者用于多机间的通信（`writer`和`readers`不在一台内）。由于我们当前都是以`MultiprocExecutor`这种单机场景为例的，所以我们提到的`zmq socket`都是指`local_socket`。

好，关于`Executor->Worker`架构的介绍就到这里了，大家可以配合本文，自行阅读源码，获取更多细节。在这个系列后面的文章中，我们还会看到更多`Executor-> Worker`配合运行的更多例子。
