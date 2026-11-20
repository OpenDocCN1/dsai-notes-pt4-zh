
# 通义千问-Qwen技术报告细节分享

> 来源：[https://geekbang.feishu.cn/docx/CEWhdVJ3uoOq7YxRprYcbzgXnEg](https://geekbang.feishu.cn/docx/CEWhdVJ3uoOq7YxRprYcbzgXnEg)
>
> 👨🏻 作者：刘聪NLP
>
> 📃 作者介绍：AIGC前沿知识分享&落地经验总结
>
> 🛖 知乎主页：`https://www.zhihu.com/people/LiuCongNLP`
>
> ⏰ 收录时间：2024-05-23 11:28:42
>
> 🔗 原文链接：h[ttps://mp.weixin.q](https://mp.weixin.qq.com/s/3MQdDabjX0rIX0OozxYk3g)q.com/s/3MQdDabj...

![](img/6defe1db1c691b7935a6d1755fa0e128.png)

关注作者👸号，获取更多AI知识

## 写在前面

大家好，我是刘聪NLP。

阿里在很早前就开源了`Qwen-7B`模型，但不知道为什么又下架了。就在昨天阿里又开源了`Qwen-14B`模型（原来的`7B`模型也放出来了），同时还放出了`Qwen`的技术报告内容。今天特此来给大家分享一下。

PS：现在国内的开源大模型也开始陆陆续续的放出了技术报告，都给我卷起来！！！

```
Report: https://qianwen-res.oss-cn-beijing.aliyuncs.com/QWEN_TECHNICAL_REPORT.pdf
GitHub: https://github.com/QwenLM/Qwen
```

技术报告中介绍了整个`Qwen`系列的模型，有`Base`模型、`RM`模型、`Chat`模型、`Code`模型、`Math`模型、多模态模型。由于`Code`模型和`Math`模型暂时没有开源，多模态`Qwen-VL`模型本身有自己的论文，本次分享对三种模型就不做介绍了，感兴趣的同学自行查阅。

![](img/f393b863ba9b1226fc4dbfe421bccd80.png)

先说结论，`Qwen-14B`模型效果从12个数据集（涉及语言理解、知识、推理等多个领域）上进行均优于现有同等级的`13B`，但仍落后于`GPT-3.5`和`GPT-4`。

![](img/ba1e833e3795ad7a86f22fd0365910a1.png)

## 预训练

### 数据

预训练数据共`3TB`，主要涉及公共网络文档、百科全书、书籍、代码等，数据涉及多语言，但以中文和英文为主。为了保证数据质量，制定了一套全面的预处理程序。

+ `Web`数据需要从`HTML`中提取文本内容，并采用语言识别工具确定语种；

+ 通过重复数据删除技术增加数据的多样性，包括规范化后的精确匹配重复数据删除方法和使用`MinHash`和`LSH`算法的模糊重复数据删除方法；

+ 结合规则和机器学习的方法过滤低质量数据，即通过多个模型对内容进行评分，包括语言模型、文本质量评分模型以及用于识别潜在冒犯性模型；

+ 从各种来源数据中手动采样并进行审查，以确保其质量；

+ 有选择地对来自某些来源的数据进行采样，以确保模型在各种高质量内容上进行训练。

### Tokenizer

词表大小影响者模型的训练效率和下游任务效果，`Qwen`采用开源快速`BPE`分词器-`tiktoken`，以`cl100k`为基础词库，增加了常用的中文字词以及其他语言的词汇，并把数字字符串拆成单个数字，最终词表大小为`152K`。

从不同语言上对比不同模型的压缩率，如下图所示，`Qwen`在绝大多少语言上都优于`LLaMA-7B`、`Baichuan-7B`、`ChatGLM-6B`、`InternLM-7B`模型。

![](img/5a892e890563bdb430f2d803798b00af.png)

PS：不知道为啥没有对比`Baichuan2`模型。

### 模型

模型采用`Transformer`框架，主要做了以下修改：

+ `Embedding` and `output projection`：对于`embedding`层和`lm_head`层不进行权重共享，是两个单独的权重。

+ `Positional embedding`：采用`RoPE`为位置编码，并选择使用`FP32`精确度的逆频率矩阵。

+ `Bias`：在`QKV`注意力层中添加了偏差，以增强模型的外推能力。

+ `Pre-Norm` & `RMSNorm`：采用预归一化提高训练稳定性，并将传统归一化方法替换为`RMSNorm`。

+ `Activation function`：采用`SwiGLU`激活函数，不同于传统`FFN`的2个矩阵，`SwiGLU`有三个矩阵，因此缩小了隐藏层维度，由原来的4倍变成`8/3`倍。

### 外推能力扩展

`Transformer`模型的注意力机制在上下文长度上有很大的限制，模型会随着上下文长度的增加，计算成本和内存会成倍增加。`Qwen`模型利用了简单地非训练计算，在推理过程中扩展上下文长度。

+ 动态`NTK`感知插值，即对序列长度的增加动态缩放位置信息。

+ `LogN-Scaling`，根据上下文长度与训练长度的比率，对`Q`和`V`的点积进行重新缩放，确保注意力值的熵随着上下文长度的增长而保持稳定。

+ `Window attention`，将注意力限制在一个上下文窗口内，防止模型关注到太远的内容。并在不同层采用不同的窗口大小，较低的层使用较短的窗口，而较高的层使用较长的窗口。

![](img/34cbe2f23f7da6bfb2299bf3b80bb5b6.png)

### 训练

+ 遵循自回归语言建模的标准方法，通过前面`Token`的内容预测下一个`Token`；

+ 模型预训练时最大长度为`2048`，为了构建批次数据，对文本内容进行随机打乱及合并，再讲其截断到指定长度。

+ 注意力模块采用`Flash Attention`技术，提高训练速度；

+ 优化器采用`AdamW`，超参数`β1`、`β2`和`ϵ`为别为`0.9`、`0.95`和`10−8`；

+ 采用余弦学习率计划，学习率会衰减到峰值的`10%`；

+ 采用`BFloat16`进行混合精度训练。

### 预训练效果

`QWEN`模型在同等级参数下表现优异，即使是更大的型号如`LLaMA2-70B`，在`3`个任务中也被`QWEN-14B`超越。

![](img/b7647962c909bcc55767359a53d54a7f.png)

## 对齐

### 有监督微调SFT

为了提高有监督微调数据集的能力，对多种风格的对话进行了标注，来关注不同任务的自然语言生成，进一步提高模型的有用性。并且大小训练方法也会影响模型行了，`Qwen`采用`ChatML`样式的格式来进行模型训练。`ChatML`格式可以时模型有效区分各类信息，包括系统质量、用户输入、模型输出等，可以增强模型对复杂会话的处理分析能力。

![](img/899ecb063e15436c862d602c32a3189b.png)

+ 优化器采用`AdamW`，超参数`β1`、`β2`和`ϵ`为别为`0.9`、`0.95`和`1e−8`；

+ 模型最大输入长度`2048`；

+ 训练批次大小为`128`；

+ 模型共训练`4000`步，在前`1430`步中，学习率逐渐增加，达到`2e−6`的峰值。

+ 为了防止过拟合，权重衰减的值设置为`0.1`，`dropout`设置为`0.1`，梯度裁剪的限制为`1.0`。

### RM模型

在奖励模型的构建上，先采用大量数据进行偏好模型预训练（`preference model pretraining`，`PMP`），在经过高质量偏好数据进行奖励模型精调。高质量偏好数据通过`6600`详细标签的分类系统平衡采样获取，为保证数据的多样性和复杂性。

奖励模型是由同等大小`Qwen`模型+`池化层`得来，用特殊的句子结束标记映射值作为模型奖励值。

模型在训练过程中，学习率恒为`3e−6`，批次大小为`64`，最大长度为`2048`，训练一个`epoch`。

![](img/46fdb63311c207b32d5531863a66d396.png)

### 强化学习PPO

`PPO`阶段共包含四个模型：`policy`模型、`value`模型、`reference`模型、`reward`模型。训练过程中，先对`policy`模型训练`50`步预热，这样保证了`value`模型能够有效地适应不同的奖励模型。在`PPO`过程中，对每个`query`会同时采样两个`response`，`KL`散度系数设为`0.04`，并根据平均值对奖励进行归一化处理。

`policy`模型和`value`模型的学习率分别为`1e−6`和`5e−6`。为了增强训练的稳定性，裁剪值`0.15`。在进行推理时，生成策略的`top-p`值设置为`0.9`。

### 对齐结果

`Qwen`的效果优于相同规模的其他开源模型，如`LLaMA2`、`ChatGLM2`、`InternLM`、`Baichuan2`。

![](img/c845e5d0aecd9c7aa8eca7bc914765db.png)

并且构造一个包含广泛主题的测试数据集，用于人工评测，比较了`Qwen-7B-Chat`（`SFT`）、`Qwen-14B-Chat`（`SFT`）、`Qwen-14B-Chat`（`RLHF`）、`GPT4`在对话上与`GPT3.5`的差异。可以看出`RLHF`模型明显优于`SFT`模型，说明`RLHF`可以生成更受人类喜爱的回答。

![](img/45734bb6ae5254d96c4f1e7fca9318ac.png)

### 工具使用

`Qwen`模型具有工具使用能力：

+ 可以通过`ReAct`提示进行使用未见的工具；

+ 使用`Python`解释器增强数学推理、数据分析等能力；

+ 作为代理，与人类交互过程中，可以访问`HuggingFace`中大量多模态模型集合。

PS：高质量数据`2000`条-`React`格式数据。

```
如何用 ReAct Prompting 技术命令千问使用工具
https://github.com/QwenLM/Qwen/blob/main/examples/react_prompt.md
```

## 总结

大模型现在已经不仅仅是卷开源了，也开始卷技术报告了~

往期推荐：

+ 如何从数据集中自动识别高质量的指令数据

+ `BaiChuan2`技术报告细节分享&个人想法

+ 大模型`LLM`微调经验总结&项目更新

+ [**浅谈LLM的长度外推**](https://mp.weixin.qq.com/s?__biz=Mzg5MTU1NTE1OQ==&mid=2247486596&idx=1&sn=b329fe7aa5b0cead6900175fd0663594&scene=21#wechat_redirect)

+ [**打造LLM界的Web UI**](https://mp.weixin.qq.com/s?__biz=Mzg5MTU1NTE1OQ==&mid=2247486578&idx=1&sn=8fc505e81e2a479bfaf9b5b15d120808&scene=21#wechat_redirect)

+ 是我们在训练大模型，还是大模型在训练我们？

+ [**Llama2技术细节&开源影响**](https://mp.weixin.qq.com/s?__biz=Mzg5MTU1NTE1OQ==&mid=2247486512&idx=1&sn=73d2b6dd73051e85bf6b9f630919f3d9&scene=21#wechat_redirect)

+ [**大模型时代-行业落地再思考**](https://mp.weixin.qq.com/s?__biz=Mzg5MTU1NTE1OQ==&mid=2247486493&idx=1&sn=d58830d103a21e141a20d308f2ad3a83&scene=21#wechat_redirect)

+ [**大模型幻觉问题调研**](https://mp.weixin.qq.com/s?__biz=Mzg5MTU1NTE1OQ==&mid=2247486489&idx=1&sn=a74f92ab19a3d7f82cf2cd527c5c8df4&scene=21#wechat_redirect)

+ 垂直领域大模型的一些思考及开源模型汇总

+ 如何评估大模型-`LLMs`的好坏？

+ 总结|`Prompt`在`NER`场景的应用
