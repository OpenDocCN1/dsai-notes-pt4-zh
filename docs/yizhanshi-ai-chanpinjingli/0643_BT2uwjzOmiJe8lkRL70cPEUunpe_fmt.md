
# 大模型推理加速技术的学习路线

> 来源：[https://v11enp9ok1h.feishu.cn/docx/Tz1fdjuPbowOBPxxxfKcCKApneb](https://v11enp9ok1h.feishu.cn/docx/Tz1fdjuPbowOBPxxxfKcCKApneb)

## 以LLama2-7B 为例，端侧推理所需DDR空间

1. `Weights` 参数：`4bit`表示，所需空间为3.5GB

1. `KV cache`：以`prefill` 阶段超长上下文，8K～32K token，用`INT8`表示。

1. 计算公式为 `2（K+V）* bsdl`；

1. 其中 `b` 代表`batch size`，端侧`batch size`都等于1；`s`是`sequence length`，这里是8K～32K；`d` 是`embedding size`，`llama2-7B`模型是4096，`l`是`layer`的层数，`llama2-7B`是32个。

1. 那么根据公式可以计算出所需`KV cache`空间为：2GB～8GB（惊呆了！）

1. 也就是按照最少的上下文需求也是3.5GB+2GB = 5.5GB（在没有开始生成的一个output token的情况下）

### 推理带宽及理论上最大推理速度

> 1. `decoding`过程一次所需带宽是：5.5GBs

> 1. 按照`LPDDR 8533` 70%的有效效率计算（如果`AP`上的`NPU`可以完全看到的话）是58GBs。

> 1. 最大推理速度是58/5.5 = 10.5 token/s。实际推理会低于该值。这么大的`KV cache`在`NPU`推理过程中，所需的`ram`已经远远大于`tcm`，肯定会和`ddr`交互，速度会更慢。

### 本文的推理加速优化手段

核心就是为什么要把所有`V cache`都加载参与计算呢，因为并不是`V cache`都是重要的，可以被忽略。具体流程如下：

#### Inference with KV cache：

+ `TopN`

+ `Softmax`后取出`TopN`（该`N`值是可以设定的），所以就不需要所有的`V`参与计算，而是可以根据`TopN`选择出对应的`V cache`进行计算。

![](img/a37b74130f390dfc03d758403d34eb89.png)

这个方案，虽然看上去合理。但是实际的有效性仍然需要验证：

> 之所以说有效，是因为有类似于`sliding window attention`的工作已经被认证，确实并不是所有的`token`都重要，有一些`token`可以被扔掉。

>

> 本工作相当于`sliding window`的自适应版本，自动选择`topN`，但问题就在于这个`N`时动态的，每一层`layer`都是不同的。可能`layer 1`的`top N` 对应的是 1～N，但是`layer`的`topN`对应的`N+1～2N`。那么所有的`V cache`就都需要在`ddr` hold住。完全起不到节省内存的目标。

>

> 当然也存在一种可能，就是`decoding`过程中生成的长文本，发现对上文中的前部分依赖变少，直接压到`flash`中去。但这么做`runtime`做的太复杂， 不划算。

>

> 实际带宽需求会减少，但是推理时间却有可能无法加速。因为`TopN`在`DDR` 地址上的不连续，要跳着去取`V cache`，极限的情况下需要`DMA`配100个地址（完成`gather`的操作）

所以对于手机端侧部署是一篇看上去很美的方案。想用起来，need more study。

## 另外一篇 PowerInfer-2 看似很美的推理加速框架

`PowerInfer-2`从架构原理上，和前面的工作`LLM in a flash`（苹果作品）很像，或者说从基础逻辑上一摸一样。架构图如下：

![](img/af8ca30c9d2613beee7be281d10f91d8.png)

主要的技术点有 或者是模型要求：

> **模型的稀疏化**。如果`MOE`模型是大颗粒的稀疏，通过路由的方式选择哪个`FFN`分支被加载。那么`PowerInfer2`（背后的那篇`Turbo Sparse`，就是`FFN`中参数级别的稀疏）

>

> 当然这个稀疏是通过对于`FFN`中`up projector`和 `down poojector`中间的激活函数进行改造，从而推理过程中获得很多0。那么`up projector`中对应的列数据，`down poojector`对应的行数据 实际就没有任何意义。因为要么参与计算的结果，通过激活函数变成0；要么就是输入是0，计算的输出也必然是0.

>

> 以上是需要有一个`predictor`的，需要通过基于`attention`层输出的`embedding` 去预测 应该读`up projector`和`down projector`的哪些列和行。当然这里的`predictor`是有一个准确率的，它相当于一个更小的模型或者分支。

>

> 当然并不一定要用`turbo sparse`的激活函数去对模型改造，就像苹果也没有一样的函数。核心在于稀疏，唯一的区别就是稀疏率。

以上是模型基本要求、其次是对于推理框架的要求：

1. 常用的`FFN` 列和行放在`DDR`里，不常用的消失在`DDR`，但一直在`flash`里面。这是它文章里面的`hot neurons`

1. 有需要的时候，再从`flash`里加载到`ddr`，然后再参与计算。本文非常扎实的验证了不同大小从`flash`读出的带宽数据（点个赞）

1. 文章里面`encoding`用了`npu`，`decoding`用了`cpu`。这是跟真正落地有区别的。区别在于`decoding`落地也用的是`npu`，并且是`W8A16`的精度，并不是`fp16`的精度。

### 为什么这个工作看似很美

举个具体的数据，来换算一下：

1. 假设我们使用的是7B model，`INT8`表达 那么需要7GB的`DDR`空间（为了简单计算，忽略`KV cache`的使用）。`LPDDR5X` 有效带宽在42GBs。那么最高推理速度是6Token/s

1. 7B model 7GB中，2.3GB 是 `attention` 部分，4.7GB是`FFN` 部分。（为了简单计算，请忽略`embedding`部分）

1. 假设`FFN`部分通过修改激活，实际每次推理，只需要20%的参数，那么就只需要4.7GB * 20% = 0.94GB的数据

1. 但是同样，从`DDR`读到`AP`来测算，从原来的连续地址读操作，变成了随机读操作。有效带宽会变为原来的 **%（先忽略整个系统`flash`的参与，而只有`ddr`参数。`flash`是一个带宽低，单位功耗高的存在），这里的 ** % 和 随机读的大小（行列捆绑的大小和`block size`有关） 和 搜寻空间范围有关

1. 那么每一册推理需要的数据load时间就变为： 2.3 GB / 42GBs + 0.94GB / （42GBs * %），这个 %就很重要了，因为影响到每一个load数据到底是比原来的`dense model` 多，还是少？

1. 随机跳地址读，问题又来了，是`CPU`读，还是`NPU`里面的`DMA`读。这里又有了一个效率问题。

1. 就算数据都读过来，因为下一步的相当于`sparse ffn`要交给`npu`去计算，那么就涉及到 数据的重新排布。

1. 又如果每一层的`ffn`实际稀疏化是不一致的，那么就要造成每一层的`shape`都是不一样的，就需要额外的`padding`到同一尺寸。

这里面有太多的未知量，会影响到实际的推理性能有没有收益了。（`dense model`简单，一张计算图，一个起始`ddr`地址，一个`call`完成）

所以个人才说这工作看起来很美，当然也不是说它就不美。只是还有太多工作是没有完成和没有数据的了。

还是希望交大校友们可以继续做下去。因为这工作也真的很棒，针对手机系统和手机`AP`，做的推理加速工作。也考虑到了很多实际手机上的限制！还是要大大的感谢！

## 新增一篇，对于encoding的优化，苹果大作

> `LazyLLM` 非常精妙的负载转移方案，抛开`paper`里面的数据不看，该思想确实是非常非常精妙。

> 为什么这么说 是非常精妙的？其实从端到端的角度上来讲（`prefill` + `decoding` 两个阶段，比如 `encoding` 1024个 token，`decoding`生成512 token），在使用该方案的前提下，总计算量是没有发生改变的。只是把 `prefill`过程中的计算量 部分转移到了 `decoding`过程中了。

那效果会像论文里一样有效吗？我觉得：有机会。原因：

1. `TTFT`过程中，需要的计算量实打实变少了，减少的计算量转移到了`decoding`的过程。在端侧（以下所有思考逻辑，都是围绕这个方案如何落地手机 ），如果先不考虑峰值内存的因素（系统关系，峰值内存过大会杀后台进程），`prefill`过程是一个明确的`compute bound`，算力是影响`TTFT`的唯一因素。那么`prefill`阶段总计算量变少了，相应的`TTFT`就大概率会变少。

1. 被减少的计算量被转移到`decoding`的过程，实际受到的影响其实会是很小的。why？因为`decoding`是典型的`memory bound`过程（当前现在实际在工程落地的过程中，发现了其实还是算子级别的`compute bound`），简单的可以认为 `NPU` 的`mac ratio` 是非常低的。也就是计算在等数据，那么将部分计算转移到`decoding`过程，其实影响是会很小的。

以上2点，是核心的逻辑，为什么是有机会。具体行不行呢？我们再详细一点来拆开看OK不OK？

### encoding阶段：

> 核心是做到如何对`token` 进行`pruning`，论文的方法其实也很简单就是利用 `attention map`。假设`prefill prompt` 是 1024个`token`，那么 `attention map` 的大小 就是 1024*1024 = 1048576 个 `fp16`的 `score`，假设有 32个 `head`，那么就需要：

1. 16 * 1024 * 1024 * 32 / 8 / 1024 / 1024 = 64MB 的空间用来存储（运大于`NPU`内的`TCM`大小，需要`offload`到`DDR`里面），如果在计算过程中就可以加权平均（如`paper`里面介绍的），那么就是2MB，就可以存储在`NPU`内的`TCM`吗？那么相关的排序也就可以使用`NPU`内的`MCU`去完成。

1. 看看`attention map`的样子，如何做`pruning`？

1. 根据`attention map` 进行计算，不管是`top k`，还是其他的算法，基本的逻辑就是通过对于1024*1024（实际大小跟输入的`sequence—length`有关）

1. 根据`pruned algorithm`算法决定，有哪些`token`继续参加后续`transformers`的计算，不参与计算当前的`hidden states`进行标记保存。

![](img/f541e5da79069aec1f4e19f67992cffa.png)

### Decoding 阶段：

其实这里我是有疑问的：

1. 对于生成第一个`token`，也就是`prefill`阶段，通过`pruned` 是可以减少`token`的，按照`paper`里面介绍的，是从比较靠后的`transformer block`开始进行`pruned`。那么前面的`KV cache`其实是没有变化的（当然我们也可以自己做实验，看从哪里开始`pruned`比较合适）。问题在于生成第二个`token`，第二个`token`需要 去和前面所有的 `prefill` + first token做`attention`，其实第二个`token` 做完，也基本生成了全部的`kv cache`，当然也存在下面的一个问题。

1. 比如：

1. 第一层`block` 做了 1，2，3，4，5，6，7，8，9，10 `token`（`prefill`）的`kv cache`

1. 第二层`block` 因为第一层进行了裁剪 只有 1，3，5，6，7，9，10 `token`（`prefill`）的`kv cache`

1. 第三层`block` 因为第二层又进行了裁剪 只有 1，5，7，9，10 的 `kv cache`

1. 那么`decoding`的时候，第二层可以通过1中保存的`hidden state`重新生成出2，4，8的`kv cache`，这里相当于和待生成`token`和 这一层`block`的`wq`，`wk`，`wv`做并行计算。

1. 那么`decoding` 第三层`block`的时候怎么办呢？这个2，4，8在`hidden state`是没有对应的，这一层的`hidden state`只有3和6.如果用第一层的`hidden state`就相当于 穿透了 第二层。

1. 精度怎么保证？？

### 方案的意见：

1. `pruned algorithm`需要在`NPU`里面的`mcu`实现，不然`rpc`的时间太长。

1. 动态`shape`的问题，因为`prefill`阶段，存在每一层`block`需要`encoding`的`token`数目不一致的问题。目前的解决方案是，通过`weights sharing` + 多个计算图的方式实现。这会带来的影响是 内存`footprint`问题

1. 接上那个，因为`pruned`的可能性太多了，计算图需要按照档位去做，那么就会出现`padding`的问题，需要`DMA`实现，不然效率太低了。

1. 当然还有`pruned algorithm`的实验，和 `decoding`过程中 `kv cache`如何补的问题。

好在这个实现，可以完全脱离对算法伙伴的依赖。部署的伙伴，可以单独尝试。
