
# 如何突破英伟达垄断

> 来源：[https://agijuejin.feishu.cn/docx/Swf3d2fBBoR70Tx1d3ecwMJwnze](https://agijuejin.feishu.cn/docx/Swf3d2fBBoR70Tx1d3ecwMJwnze)

> 共识粉碎机｜[阅读原文](https://mp.weixin.qq.com/s/YHGngJ7ZQi3uYEu0ZUo_8A)

> 转载请联系原作者取得授权

> 本期讨论会参与者：

> 李乐丁老师，北极光高级顾问，前百度云主任架构师。

> Eric老师，清醒异构首席架构师，有丰富的AMD落地训练/推理经验。

> 王健飞老师，NPU架构师，知乎@王健飞。

> 以及一位资深的一线ASIC厂商战略负责人。

![](img/baae1ac4b7ee299dfa86d29c9b37d833.png)

## 1 从PyTorch2展开谈CUDA壁垒

---

PyTorch2整体上有一系列重大更新，虽然进展稍慢，但不影响终局

+ PyTorch2是2022年下半年发布的，在2021年就开始了宣传，其中 `Torch Dynamo`（注1） 是Pytorch2.0中最重要的工作。

+ 2.0中最大的功能是`torch.compile()`，这个功能相当于在用户的Python程序和厂商所需要的图上数据流计算构建了之间的桥梁。在更早的时候，NV可能不需要这样的表示也能保证性能。但随着算力需求不断扩张，NV也开始需要类似的能力。因此，Torch把这部分提上了官方日程。这对所有的ASIC/DSA厂商来说都是利好，因为这部分工作量相当于Torch自己承担了，并会对用户形成一种广泛的教育。

+ 从目前情况来看进展比预期稍慢，但已经有初步落地效果，进展慢的原因包括`Torch Expert`等功能还未发布；以及并不是100%的模型结构都能稳定翻译出来，仍然会有corner case。

+ 技术上没有风险，预估到2024年会使整个软件栈做得更加鲁棒。目前`Torch`提供的两套IR（Intermediate Representation）算子（即`ATen`算子库和`Prims`算子库）尚未完全定型，以及`Torch Dynamo`的`Compile`系统仍需一年时间的打磨。

+ 注1：`Torch Dynamo`：Python-level JIT compiler designed to make unmodified PyTorch programs faster. `TorchDynamo` hooks into the frame evaluation API in CPython ( PEP 523 ) to dynamically modify Python bytecode right before it is executed. It rewrites Python bytecode in order to extract sequences of PyTorch operations into an `FX Graph` which is then just-in-time compiled with a customizable backend. It creates this `FX Graph` through bytecode analysis and is designed to mix Python execution with compiled backends to get the best of both worlds — usability and performance.`TorchDynamo` makes it easy to experiment with different compiler backends to make PyTorch code faster with a single line decorator `torch._dynamo.optimize()` which is wrapped for convenience by `torch.compile()`

+ Source: https://pytorch.org/docs/stable/dynamo/index.html

### PyTorch2 推出的 `Prims` 新算子库与 Triton 结合可以削弱 CUDA 壁垒

+ `Torch`之前的算子库，叫`ATen`。这个算子库早时是按照需要添加，没有明确的算子集合的概念。可以说`Torch`在Python层面是一致的，但在算子层面内部没有一个良好的定义。因此，非NV厂商想要对接，会发现这些算子定义不够稳定，可能会在一段时间内发生变化，跟踪起来会很麻烦，只能pin到`Torch`的某个稳定版本上进行迭代，这显然不符合客户的预期。如果是仅在内部使用，无需定义IR，则并不会影响当前的使用。

+ `Torch2.0` 提供了另一套名为`Prims`的算子库。这套算子库将原来的~2,000多个算子拆解成更原子的算子。`Prims`算子的优势在于，因为它的数量较少，所以更容易得到厂商更完善的支持。它的缺点在于，原本是一个宏观的算子，如`Attention`这样的算子，在`Prims`算子上会被拆成很多小算子。拆完后，原来对`Attention`整个算子有加速设计，现在需要把小算子再拼回来，相比更加麻烦。因此，`Torch`希望可以用`Prims`来做保底。如果有一些额外设计，则可以从`Attention`上接入。

+ `Torch2`同时添加了新的后端`Triton`（注2），大致可以理解为类似于Python领域专用的语言，对中间表示进行处理，接着通过这些中间表示进行优化。最终用IR与GPU相关的中间表示生成代码。如果以`CUDA`为例，写一个矩阵相乘，如果使用`Triton`的方法，在某些硬件上能够达到`CUDA`的90%。若要支持新算子，`Torch`也有编译的开源栈，能够把一些算子变成`Triton`的中间表示。实际上，对于`AMD`与`ASIC/DSA`来说，只需要添加一个在`Triton`上的后端代码生成过程。因此，`Torch2`更好地支持了模型编译，这也削弱了`CUDA`的壁垒。

+ 注2：`Triton` 是一种类似于 Python 的开源编程语言，它使没有 `CUDA` 经验的研究人员能够编写高效的 GPU 代码——大多数时候与专家编写的代码相当。`Triton` 可以用相对较少的努力达到最高的硬件性能；例如，它可用于编写 `FP16` 矩阵乘法内核，其性能可与 `cuBLAS` 的性能相媲美——这是许多 GPU 程序员无法做到的——不到 25 行代码。相关研究人员已经使用它来生成比等效的 `Torch` 实现效率高出 2 倍的内核，官方还是很愿意与社区合作，让每个人都能更轻松地进行 GPU 编程。Source： https://zhuanlan.zhihu.com/p/617731357

![](img/69cc3b33e342c88a8a357234cfd91909.png)

## AMD 与 ASIC/DSA 如何通过 PyTorch2 追赶 CUDA

+ 从ASIC的角度来说，现在就应该接入`Torch`。长得像GPU架构的 keyi 可以 从`Triton`层面接入 可能更适合长得像GPU架构的，工作量 会 小一些。

+ 若是其他`DSA`（Domain Specific Architecture，领域专用架构，注2），例如国外的`Graphcore`、`Tostorent`和特斯拉等平台，还需要在 `Triton`上（可能是一个比`CUDA`更好 上 的媒介），但还需要 做 一定 扩展，接下来需要 减少一些功能增加一些功能，比如增加空间计算能力，如`mesh`结构的空间计算，才能用得更好。或者 直接接入 `Graph` 自己做编译。

+ `ASIC/DSA` 何时能做得好，实际取决于`ASIC/DSA` 厂商的能力和努力程度，是个工作量问题。如果投入足够的人员，看准这个道路并打磨体验，可能在一两年内就能做得较好。但这个工作量永远在，追赶的厂商需要平衡现有的使用和未来的发展。

+ 但无论是生态层还是整个上下游的连接上，当`Torch`进行任何更新时，都必须确保`CUDA`接好。这样一来，即使是框架层，它仍在为`CUDA`适配。那么新厂商永远面临适配框架的问题，因此追赶会相当辛苦。从方向上看，这确实有利，但最终可能是资源、时间和架构技术方面的问题。

+ 看`NV`的`Datasheet`，当`V100`时就有`Tensor Core`了。如果看到最新的`A100`或`H100`，会标出`CUDA`的`FP32`和`Tensor`的`FP16`。`NV`也在不断增强通用计算部分的`CUDA Core`计算能力。最新的`A100`、`H100`做的`Transformer`或者各种深度学习网络，加速更好的是在它的`DSA-like`的架构上。`H100`、`H800`上面表的数字很高的，1个`P`左右算力都是专用的，不是`CUDA Core`那种通用算力。

+ 注3：`DSA`针对特定应用场景定制处理引擎甚至芯片，支持部分软件可编程。`DSA`与`ASIC`在同等晶体管资源下性能接近，两者最大的不同在于是否可软件编程。`ASIC`由于其功能确定，软件只能通过一些简单的配置控制硬件运行，其功能比较单一。而`DSA`则支持一些可编程能力，使得其功能覆盖的领域范围相比`ASIC`要大很多。`Google`的`TPU`是一种典型的`DSA`产品。 Source: DSA芯片到底是什么？ - 知乎 (zhihu.com)

![](img/6e9780a7c8c43eb01dc249bb2a8f19b1.png)

## 如何量化芯片厂商的工作量

+ 会分成几种挡位。如果架构偏向`GPU`，无论是`NV GPU`还是`AMD GPU`都会更好，例如`Triton`的那种情况。假设有几十个人就可以完成。如果偏向`DSA`，以华为为首，需要铺设很多人才行，例如华为整个软件栈，有1000人以上的规模。

+ 因此，对于非`GPU`或非`NV`公司来说，工作量本身就是一个现实壁垒。即使技术都完善，但要达到`NV`或`CUDA`的水平，仍需要时间去追赶。相比`NV`，还需要保持加速度。

+ `NV`宣称自己是一 家软件公司。当客户量大的时候，确实需要庞大的软件支撑团队。一方面要打好自己的基础能力，另一方面要帮助客户做适配。因为客户一开始可能不会掏钱或者付出工作量来适配你的平台。作为厂商，需要帮客户将工作量迁移到系统上，无论难易，这个过程一定需要很多软件团队的人。

+ 软件团队一方面需要打好基础，例如编译系统等需要资深领域专家参与。在国内，这些团队的数量可能不是特别大。另一方面，例如华为可能有上千人，帮助客户适配模型或算子，如算子加速优化或一类网络模型优化。现在大家都听到 `FlashAttention`（注4）这种结构，这是一种针对子图的优化方法。然而，客户并不仅仅是这一种结构，还有许多其他的奇特网络结构，全依赖自动来实现的方案并不成熟，实操中仍需要大量人员开发，如`FAE`等，帮助客户完成这个任务。因此，这些工作确实非常烧人。

+ 举个例子，如果今天写一个矩阵乘法，基本上所有人写出来都没有`NV`的速度快。不是因为算法不够好，而是`NV`已经根据不同矩阵尺寸和 芯片 代际来分别设计不同的算法。这些算法的参数是`NV`一遍遍测出来的，这是一个 巨大的 工程工作量。因此，这一块需要大家在一段时间内不断积累工作量才能 追赶和超越 。

+ 注4：`FlashAttention` 是一种重新排序注意力计算的算法，它无需任何近似即可加速 注意力计算并减少内存占用。所以作为目前`LLM`的模型加速它是一个非常好的解决方案。 Source: https://zhuanlan.zhihu.com/p/626079753?utm_id=0

![](img/99941535a12ef3fa4618cdfc24c5a0f7.png)

## 为什么 PyTorch 会主动与 AMD 等加强合作

+ `Torch`与`NV`的适配 不需要双方主动做什 么，整个社区生态自然而然就会发生。

+ 但`Torch`要面向更广大的开发者，与各种`GPU`都对接好，就要尽量多的加入硬件加速器成员。与`AMD`加强合作，也符合`Torch`框架的自身发展。

+ 从`AMD`的角度来看 ，`Torch`一直在努力适配`AMD`，大约在 `Version 1.8`左右能够全面支持`AMD`。因此 ，若接下来`Torch`继续发展 ，不仅要支持`CUDA`，还要支持`AMD`的`ROCm`，或者通过编译手段添 加新的算子的方式。

## PyTorch 与 ROCm 的合作进度怎样了

+ 从用户体验的角度来看，现在使用`PyTorch + ROCm`和`PyTorch + CUDA`并没有什么特别的差别。例如，原来底层接的是`CUDA`，将某个`Tensor`放到`CUDA`上跑通。当使用`AMD`卡时，代码几乎无需改变。从使用体验来看，`Torch2.0 + ROCm`的感觉越来越好。

+ 从性能角度来看，硬件本身的算力可能存在一些差距。例如，`A100`和`MI210`左右的显卡对比，`A100`的`FP16`的计算性能，实际上要比`MI210`好很多。但是从软件角度来看，差距没有那么大。

## 当用户最终需要使用一些算法时，算法性能如何？

+ 这与软件肯定有很大关系。例如，对某一个特定的算子，如矩阵乘，优化的程度是多少，这可能是一个限制。从目前使用`ROCm`的角度来看，例如测试矩阵层和内存开销的算子，与`CUDA`同级别下的算力卡，差距可能达到`A100`的80%，访存方面可能达到80%。计算`FP32`可能达到提供的理论算力的90%，`FP16`可能达到80%。

## `PyTorch + CUDA` 和 `PyTorch + ROCm` 之间的差距是动态变化的

在整个过程中，两者之间在类似任务上的差距是逐渐缩小的，还是有所增大？

+ 从`LLM`的角度来看。在市场上，较最好的模型（`GPT4.0`）尚未出现特别大的革新, 故在算子，如矩阵乘，这些算子并没有特别多的新需求。

+ 如果从这个角度来考虑，可能`ROCm`和`CUDA`的距离会更逐渐接近，因此最终制约两个卡的效率的主要因素可能还是在硬件上，可能不在软件这一层。

+ 实际上也要关注两个硬件的速度和架构演进程度。例如，`NV`最新一代增加了许多`DSA`能力，如`TMA`等（`Tensor Core and Memory Accelerator`，注5）。最早的`CUDA`实际上连`Tensor Torch`都没有，只有最基础的`Vector`运算。`Tensor Torch`也不是第一个`DSA`能力，随后增加诸如`TMA`这样的数据搬运能力，以及多个`SM`共享的协作计算的能力。这些能力并非基于最原始的`SIMT`编程模型所有的东西。`DSA`技术发展后，这些更加不标准这种趋势非常明显，拥有更大的算力以及提高更好的算力利用率，必须也要做这些事情。这也是各种`ASIC`或`DSA`能够弯道超车的机会，厂商如果做得比英伟达更好，可能有机会弯道超车。但英伟达自己也在做这些事情， 所以如果其他厂商也不能只单采取跟随策略，而需要跑的比 `NV` 更快。

+ 注5：相关`DSA`特性包括：`TMA`、`Async warpgroup-level MMA`、`Distributed Shared Memory`、`Async transaction barrier`，其中，`TMA`支持`img2col`，`multicast`，`shared memory swizzle`等。 Source: NVIDIA 的「老黄刀法」究竟是个什么梗? - 知乎 (zhihu.com)

![](img/a7e8e8ccb35ed33cd7465d69746b1c70.png)

## 现在已经看到 AMD 生态在追赶英伟达的趋势了吗？

+ `ROCm`达到`CUDA`的能力，还需要做大量工程方面的工作，另外 对于硬件来说，硬件卡的算力并非是最困难的地方，最难的地方还是在`I/O`。 一方面是等显存，另一方面是等卡间通信。这其中耗费了大量时间，因此大家会看到很多卡利用率仅为30%~50%。要充分利用这些资源，不仅需要利用显存，还要关注缓存。今年新出的`Hopper`或`4090`，花了大量芯片面积放置`L2`缓存。而`AMD`选择的是`Inifinite Fabric`，这实际上也是一种加快缓存的方式。若再仔细观察，`SIMT` （`Sing-Instruction，Multi-Treads`，注6）里面也是缓存。

+ 因此，无论是`TPU`论文还是`Infinite`等论文，大量篇幅都在讨论如何设计缓存问题。实际上，面临的问题与当年`CPU`面临的问题非常相似。计算`Core`速度远超过`I/O`能力，中间会有大量缓存，开发者需要面临大量`Cache Line`对齐等细节问题。这些都需要`AI`框架或像`ROCm`这样的计算库来解决和克服。

+ 今天有时候`NV`做得很快，或者`PyTorch`做得很快是出于这样的原因：同样的算子，最终映射到不同卡的算子不一样，而不同卡上的算子在不同参数大小和规格下的性能表现都不一样。尽管现在`PyTorch2.0`迈出了重要一步，中间有`IR`，但`IR`只是代表可以从`API`的角度涵盖不同卡，但`IR`不能解决不同卡在不同算子上的性能表现不一样的问题。 因此，仍需大量工作来解决不同卡在不同算子上的性能表现不一样，否则会看到像`AMD`的卡在纸面性能上非常高，尤其是新出的`MI300`，纸面规格比`H100`还要强。但实际应用效能可能不如`H100`。 最近`MosaicML`（注7），移植后发现`MI 250`的实际应用效能是`A100`的80%。如果真正挖掘还有大量性能潜力隐藏在具体细节中，这方面的工程量仍然很大。

+ 从过去的实践经验来看，这不仅仅是技术问题，更多的是企业生态建设的决心问题。如果只依赖像`AMD`这一家公司逐步改进`ROCm`，很可能永远追不上`CUDA`。可需要像`Google`或`Meta`这样的企业，能够从更上层，比如说`PyTorch`这一层，或者更往上的层次一起支撑。例如更多地使用`Deep-speed`（注8）这样的框架，并用自己的生态改进，它的速度可能会很快，就像`Google`一样。如果真的下决心，它的`TPU`表现会非常好。从宏观角度来看，同一代`TPU`效能不如同一代的`NV GPU`。但`Google`手中有`AI`框架，有`Tensorflow/JAX`等，有自己的领域模型，可以在`DSA`领域进行专项优化，从而提高效能。因此， 这是整个产业链的事情，不能将压力全部压到像`AMD`这样的公司上。

+ 具象来看`AMD`的话，计算上的瓶颈可能不大，瓶颈会出现在显存带宽上。前段时间关注到，`AMD`现在有`Chiplet` （注9）技术，在片上集成能力可能会更强一些。而`NV`走的路线是卡之间的互通能力特别强。`AMD`的策略更倾向于在单卡上将显存做得特别大，这也是`AMD`可能存在的某种优势。
