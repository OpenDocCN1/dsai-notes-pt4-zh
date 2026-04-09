# TVM meetup Highlights - 2023.12

> 来源：[https://learning.feishu.cn/docx/LaOGdIvibopwL9x0C0ec1Ko4nif](https://learning.feishu.cn/docx/LaOGdIvibopwL9x0C0ec1Ko4nif)

# Insights from the talks

TVM Unity 的 frontend

1.  不再支持 Tensorflow 了。大模型时代，没人用了。

1.  PyTorch Model，直接或用 Dynamo 转成 Torch FX。

1.  进入图编译 (Relax) 之前，针对各个 frontend 先做了一次 shape deduction

![](img/f672657372d3506aad53acab4ddbdad6.png)

一次 lowering 搞不定的，就 loop 反复 lowering 多次。

以前 lowering 前后的 IR 目标清晰，相互解耦。现在，撤回设计，只要啥都能存，就能支持反复 loop lowering。

-- 我们的设计就是没有设计。大道无形。

![](img/1b291b5e4165c3442c391b5ea60fafde.png)

TVM Unity 把 KVCache 和 distributed 放在了 runtime 里面来支持。

这感觉历史包袱还是太重了，非常不面向未来的 llm。

![](img/2c8175b342a0978a6c701f17e62d9dfb.png)

OctoML 的 project

把 KV Cache 和 IO 作为 Effect 单独抽象。是个思路，但又感觉有点僵硬，就像没抽象一样。

![](img/791df31aa9d8b1100f6d55cbc28651b8.png)

TVM 的一贯做法 - model 和 weight 分离。

在大模型时代，可能确实是个好主意。省了很多事。

量化主要就是怼着 weight 搞。

![](img/bdde3bb120a9bfd59de337baf8f59142.png)

字节 ByteIR，torch 的 这个 demo，思路跟我们的接近。

他们验证过了，还能做 training。

先抄完再说，有问题再优化。即使错了，也是字节 AML 水平的错误。

![](img/714f3abf2648d10893a6f034136e7a5d.png)

Forward 和 backward 的图分离，获得额外的加速空间。

![](img/7864acc2255e0a1966cd594e91466e5e.png)

![](img/a4e23b52436f0b5f4987bda3a7f69591.png)

高性能算子，用了 AI Template。看起来比 torch compiler 原生的快一些。

![](img/d27be262fd4a0a6b161ae1db0e366e92.png)

![](img/b6913c3d745a71e3a530d1bddf5d6910.png)

Compiler 直接以 backend 接入 PyTorch compiler，效果不错。

ByteIR 的 compile，inference & training demos，API 设计的感觉不错

compile

![](img/70536088ea26c5247b8c7093a829fee8.png)

inference

![](img/a4af6fa2583c6c0d92c98b84c5eeb4d3.png)

训练 + inference 的 demo

![](img/e616f73a211a83c7506a27bbc12e5fe9.png)

# Talk 1 - TVM Unity

## 编译挑战: Steep Lowering Curve

![](img/0f15932c8a6fddf2b37253700633b1e3.png)

![](img/10f3ff025ee6caf7faa7b05b7d7a65e8.png)

![](img/52f7a06a730f87eb323a36f26da2811e.png)

## TVM Unity 方案: Partial Lowering

![](img/e28c8091c5829f7dea4f9e49ae535cff.png)

![](img/7e095ee0a7b8fb13d5d766fd27e76ff1.png)

![](img/87ee95f66af230424fe65200b9b0de50.png)

## Compilation workflow

![](img/290a6c9884be615a0ec4fd7bcedf3cb1.png)

![](img/0819cd36b24757c8bc79cae672db5bad.png)

![](img/848a0ab68903376b8467c55e0bb74cff.png)

![](img/2d038dd329ed3acd923758a6b1884e89.png)

![](img/70f56d80d4615db382ba9eed09da743f.png)

# Talk 2 - **OctoML **的一些 projects

## Torchy

KVCache

![](img/6ec6ec02f3f835a2755f75e7904caef2.png)

IOEffect

![](img/e1e6b8defdfdac618c3088ab7289118b.png)

## AutoLLM: 统一的量化框架

其实就是个胶水代码，所以才能统一各种算法。

价值：整理的各种量化算法的通用 pattern 和 workflow。

可能是 1 个人不到 3 个月的工程工作。

![](img/c3bb283df807dbf7b4dc6a2d86c5623d.png)

![](img/e78cf87540c794cdd752ed94b45277e5.png)

![](img/723a24b0d4ea28bd6d21614356fb95ba.png)

# Talk 3 - 美团 TVM 的实践

一个老生常谈的话题了，讲了很多年了。没变过。

![](img/fb774588766f9ff69558c8dbcf4616c9.png)

![](img/8cd1fe8123bc50558b651a50cb4ae148.png)

# Talk 4 - 字节 ByteIR

![](img/f3cfb8e4c148832b6ff764e0163d5954.png)

## ByteIR 架构

![](img/e41563b09590ce02e7c0083aa6bb5e84.png)

![](img/686b28207104816e70c7d2c1aef3d4b1.png)

![](img/fe55e38b7515feb72ad257f18323657b.png)

## PyTorch 的例子

![](img/0be5f041e88e1ad9793764fc4b400c66.png)

![](img/dc5b36887e55bf3e99d820731752396d.png)

## FW / BW 的 graph 分离

效果很好，特别是针对 backward 图，优化空间变多了。

![](img/4008c9687bc775795b89112cde08bac5.png)

![](img/031400bae04ca0d5627b7cd6ce2038a1.png)

## 接 AI-template 算子

![](img/a55b778530167d0ea6ad0bb9c7bae9b0.png)

![](img/2376c8fa95a9c174bcdd74912d6ef6ac.png)

## Linalg 上的 tiling 和 fusion 优化

![](img/3a1962485a61100d9f4b5ba9609f899a.png)

![](img/d7045469a69aa500f8401dff4977be2c.png)

![](img/0a5d1241918ab3fc7aee4d212e9b1b3e.png)

![](img/a4a82e00c919f59e6c01fc7547c6ed74.png)

## Pytorch Example

编译

![](img/f2380ed2ce6cf740432a467ad641c365.png)

inference

![](img/eea21e83d8eb1732400f7f872851bce9.png)

训练 + inference 的 demo

![](img/b2418e375f3aa463374f3b3007ac220a.png)

![](img/d9abe8fb6d42002d1405e3324237c0d6.png)