# 昆仑万维携手南洋理工大学抢发Q*算法：百倍提升7B模型推理能力

> 来源：[https://agijuejin.feishu.cn/docx/XSxNdHEoTorpomx7DllcK4OBn4g](https://agijuejin.feishu.cn/docx/XSxNdHEoTorpomx7DllcK4OBn4g)

> 机器之心｜[阅读原文](https://www.jiqizhixin.com/articles/2024-06-25-3)

> 转载请联系原作者取得授权

自 OpenAI 的 Q* 项目曝光后，引发业内众多讨论。据现有信息汇总，Q* 项目被视作 OpenAI 在探索人工通用智能（Artificial General Intelligence, AGI）道路上的一次重大尝试，有望在包括数学问题解决能力、自主学习和自我改进等多个层面对 人工智能 技术带来革新性突破。

![](img/6fa1c2f61c25c39a5ad0b220ac6bbed9.png)

![](img/44e37beb9c62ea8359d5f6dc4de312d7.png)

英伟达科学家 Jim Fan、图灵奖得主 Yann LeCun 等参与讨论 OpenAI 的 Q* 实现方式

![](img/15856ddb5bcf413795c2aa1646e1c3ea.png)

Meta 科学家 田渊栋 则认为 Q* 是 Q-learning 和 A* 的结合，且天然地适合推理任务，尤其在数学推理方面

不过迄今为止 OpenAI 没有公开关于 Q* 算法的具体细节，其效果究竟如何我们并不得而知。

昆仑万维自 Q* 项目曝光以来，一直密切关注 Q* 的动向，且在第一时间就成立研究小组尝试开发自己的 Q* 算法，希望打破 OpenAI 的封锁，提升现有开源模型的推理能力。

经过数月的尝试，昆仑万维携手新加坡南洋理工大学成功开发了一个名为 Q* 的算法，能够显著提升现有大模型的推理能力。在 GSM8K 数据集上，Q* 帮助 Llama-2-7b 提升至 80.8% 的 准确率 ，超越了 ChatGPT；在 MATH 数据集上，Q* 帮助 DeepSeek-Math-7b 提升至 55.4% 的 准确率 ，超越了 Gemini Ultra；在 MBPP 数据集上，Q* 帮助 CodeQwen1.5-7b-Chat 提升至 77.0% 的 准确率 ，缩小了与 GPT-4 的编程水平差距。

![](img/58637ec6b9427c35ac9e1ae589df623d.png)

+   论文：Q*: Improving Multi-step Reasoning for LLMs with Deliberative Planning

+   论文链接：https://arxiv.org/abs/2406.14283

Q* 能够帮助小模型达到 参数 量比其大数十倍、甚至上百倍模型的推理能力，这一算法不仅大幅提升了小模型的性能，还显著降低了计算资源的需求，为 人工智能 的广泛应用带来了全新可能，开创了高效智能的新纪元。

**复杂推理任务全盘 规划**

在《Q*: Improving Multi-step Reasoning for LLMs with Deliberative Planning》论文中，研究人员首先将大 语言模型 的推理轨迹分解为若干个状态，对于每一个状态，参考 DeepCubeA 中的设计，通过将定义 Path Cost 的 g (s_t) 函数和定义 Accumulated Reward 的 Q*(s_t, a_t) 集成到同一个 f (s_t) 函数内，实现了对历史状态收益和未来期望收益的综合考虑。最后利用 A* 搜索算法对状态进行 最佳优先搜索 ，实现了对复杂推理任务的全盘 规划 ，从而提升开源模型在推理任务上的性能。

![](img/f749cfb787c0d278348ef4998d1a34fd.png)

其中 g (s_t) 表示当前轨迹中的多个历史状态，既 {s1,...,s_t}，的聚合收益。

![](img/1ff782605d8d134de8adcf8b17d86703.png)

具体 g (s_t) 的函数形式可以通过人为定义，例如判断当前代码是否符合语法规则等，或者通过构建 Process Reward Model (PRM) 进行 监督学习 得到；g (s_t) 中的聚合方式可以为求和，最大值，最小值等。

![](img/68ce0e315fe864401703fbf8334a7bf6.png)

![](img/33b2b8ff9438fb225866654891fe33a4.png)

![](img/47696a204a41b54bae0a0a994b2b8a68.png)

实验结果表明，昆仑万维本次所提出的 Q* 框架，可以显著地提升 LLM 的推理能力，在 GSM8K 数据集上，Q* 帮助 Llama-2-7b 提升至 80.8% 的 准确率 ，超越了 ChatGPT；在 MATH 数据集上，Q* 帮助 DeepSeek-Math-7b 提升至 55.4% 的 准确率 ，超越了 Gemini Ultra; 在 MBPP 数据集上，Q* 帮助 CodeQwen1.5-7b-Chat 提升至 77.0% 的 准确率 ，缩小了与 GPT-4 的编程水平差距。

![](img/0c533aee121765edcbe1596c66e7b5f9.png)

![](img/f9e3cb79c362986a620bf210f3406367.png)

![](img/ce83b759664bb66a449687179d85e132.png)

研究证明，Q* 能够帮助 参数 量仅为 7b 的小模型达到 参数 量比其大数十倍甚至百倍模型的推理能力，大幅提升模型的性能，并显著降低了计算资源的需求。目前，Q* 的研究尚在初级阶段，算法在各个环节还有进一步的改进空间。未来，昆仑万维会继续深入此项研究，不断提升国产开源模型推理能力，打破 OpenAI 闭源封锁，为 人工智能 前沿技术发展带来全新可能。