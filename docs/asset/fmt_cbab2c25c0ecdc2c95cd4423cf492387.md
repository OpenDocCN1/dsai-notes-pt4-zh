
文章探讨了 `Transformer` 模型在自然语言处理和人工智能领域的应用，特别是它与动态规划之间的联系。作者通过详细的数学推导，展示了 `Transformer` 中的注意力机制如何与动态规划的状态转移方程相对应。文章进一步分析了 `Transformer` 在计算生物学、物理学、几何学和图论等领域中的动态规划应用。此外，文章从微观复杂度的角度探讨了 `Mamba` 和 `RWKV` 等结构如何在特定任务中替代 `Transformer`，并提出了 “`Transformer-RWKV` 假设”，指出对于某些复杂度较高的问题，标准的 `Transformer` 可能更为适用。同时，文章指出在时间序列和 `RNA/DNA` 问题上，高效 `Transformer` 和状态空间模型可能更优。最后，文章质疑了 `next token prediction` 是否能有效学习动态规划，并探讨了可能的解决方案，如结合探索、链式思考和强化学习。

![](img/94ce804bf079557c8a1440a4048c1a1e.png)

## 揭秘`C++`：虚假的零成本抽象

贡献人：

`https://zhuanlan.zhihu.com/p/700751030`

文章深入探讨了 `C++` 中所谓的 “零成本抽象” 特性，揭示了其在实际应用中可能存在的额外运行时开销。具体分析了`std::unique_ptr`、`std::forward_list`和`std::tuple`这三个例子，指出它们虽然设计初衷是为了提供便捷的抽象而不增加性能负担，但实际上由于自定义的拷贝或移动构造函数，以及 `ABI` 的要求，它们在函数传递等场景下会导致性能损失。文章强调，尽管这些特性带来的额外成本在一般情况下可以忽略不计，但在追求极致性能的场景中，开发者应该注意这些潜在的开销，并在必要时寻找替代的实现方式以达到真正的零成本抽象。

![](img/609285ff3fa7102699feb37e9f057532.png)

## [ICML 2024] 高效架构`SLAB`: `Attention`简化，无需`LayerNorm`，视觉/语言任务精度无损效率提升

贡献人：

`https://zhuanlan.zhihu.com/p/700170686`

本文介绍了 `SLAB`，一种高效的 `Transformer` 架构，它通过简化线性注意力机制和采用渐进式重参数化 `BatchNorm` 策略，优化了模型性能。在技术细节上，`SLAB` 使用了 `ReLU` 算子作为相似度函数，并融入了深度可分离卷积来增强局部特征提取。渐进式重参数化 `BatchNorm`（`PRepBN`）策略通过引入可学习参数 `η` 和超参数 `γ`，实现了从 `LayerNorm` 到 `BN` 的平滑过渡，提高了推理速度，同时保持了模型精度。实验结果显示，`SLAB` 在 `ImageNet1k`、`COCO` 和 `Wikitext-103` 等数据集上的应用，能够在精度无损甚至提升的情况下，显著降低模型推理的时延。

![](img/bbf33e7db101aee93cb4be075a901054.png)

## 原理&图解`vLLM` Automatic Prefix Cache(`RadixAttention`): 首`Token`时延优化

贡献人：

`https://zhuanlan.zhihu.com/p/693556044`

文章深入探讨了 `vLLM` 自动前缀缓存机制（`RadixAttention`），旨在优化大型语言模型（`LLM`）中首个 `Token` 的处理时延。该机制通过构建基数树（`Radix Tree`）来自动识别和缓存序列中的共同前缀，实现对 `KV Cache` 的重用。文章详细阐述了 `Radix Tree` 的动态变化过程，以及如何利用哈希码作为物理 `KV Block` 的唯一标识，以简化工程实现。在处理多轮对话时，`vLLM` 能够缓存 `Prefix` 和 `Generated KV Cache`，从而在新的对话轮次中减少重复计算，提升性能。此外，文章还讨论了在边界情况下的缓存策略，以及如何通过 `LRU` 算法在显存不足时进行缓存块的逐出。最终，通过实际应用实践和代码分析，验证了 `vLLM` 自动前缀缓存机制在提升 `LLM` 推理效率方面的实用性和效果。

![](img/ee15572f2188efa0ce3a8cc77c82e177.png)

## 创建、设计和部署机器人应用程序：使用新的`NVIDIA Isaac`基础模型和工作流程

贡献人:

`https://developer.nvidia.com/blog/create-design-and-deploy-robotics-applications-using-new-nvidia-isaac-foundation-models-and-workflows/`

`NVIDIA` 推出了新的 `NVIDIA Isaac` 基础模型和工作流程，以促进机器人应用的创建、设计和部署。其中包括：

+   `Isaac Perceptor`：为自主移动机器人（`AMR`）和自动导引车辆（`AGV`）提供了一个新的参考工作流程，支持多相机 `AI` 深度感知、多相机视觉惯性里程计（`VIO`）、实时多相机体素网格映射等功能，以帮助机器人在复杂环境中进行导航和障碍物避让。

+   `Isaac Manipulator`：提供了加速库和 `AI` 模型，用于赋予机器人臂 `AI` 加速能力，包括：

+   `cuMotion`：`GPU` 加速的运动规划器，减少周期时间。

+   `FoundationPose`：用于单次 6D 姿态估计和跟踪的统一基础模型。

+   `SyntheticaDETR`：基于 `DETR` 的模型，用于单次图像空间中的物体检测，可在 `NVIDIA Omniverse` 生成的合成数据上进行训练。

+   `JetPack 6.0`：支持 `Isaac ROS 3.0`，加速 `AI` 和机器人应用的开发。

+   `Isaac Sim 4.0`：提供了一个轻量级的参考应用程序 `Isaac Lab`，用于机器人基础模型的训练，支持强化学习、模仿学习和迁移学习，并增加了对多款机器人的支持。

![](img/a5977d035b1b481e242c822660aca4ac.png)

## 银河通用王鹤：让具身智能机器人“言出法随”，需攻克两大局限性丨`GenAICon 2024`

贡献人：

`https://mp.weixin.qq.com/s/EK2KheSholTnr7DEqU6SoQ`

银河通用的王鹤项目旨在开发具身智能机器人，使其能够理解并执行复杂指令。该项目面临两大技术挑战：认知局限性和行动局限性。认知局限性体现在机器人难以准确解析人类语言和上下文信息，限制了其理解和适应复杂场景的能力。行动局限性则涉及机器人在实际环境中执行任务的精确度和适应性，特别是在动态变化的环境中进行精确控制和操作的能力。为了克服这些挑战，`GenAICon 2024` 将提供一个平台，汇聚专家共同探讨如何通过先进的自然语言处理、机器学习、传感器融合和机械设计等技术，提升机器人的认知和行动能力，推动具身智能机器人技术的进步。

![](img/dd3761738a0a8a65681fc20e0d821d73.png)

## 借着`triton inference server`聊一下各种batching方法

贡献人：

`https://mp.weixin.qq.com/s/Y-3ZMvn_gZK1u5I6_woq_g`

`Triton Inference Server` 支持多种批处理方法，优化推理过程。同步批处理要求客户端等待形成批次，有助于提升 `GPU` 资源利用率，但可能增加延迟。异步批处理允许客户端立即发送请求，减少延迟，但可能不利于 `GPU` 资源的最大化利用。动态批处理结合了前两者的优点，自动调整批处理大小，兼顾延迟和资源利用。`Triton` 还提供了自适应批处理大小调整、请求优先级处理和 `Instance Group` 等高级特性，以适应不同的推理场景，实现性能最优化。这些技术细节体现了 `Triton` 在提升推理效率和灵活性方面的优势。

![](img/de3b8c4a2de7290682e671fa8519ae25.png)

# 声明

本文档仅供学习交流使用，版权归原作者所有，若涉侵权，请联系Jack Jin 15101136166

##

（欢迎在下方反馈您宝贵的意见！整理不易，觉得有用给AI日报小组点个赞👍吧！）

`ChatGPT`；Angela；Zijia Liu；Alex；Zeqi Tan；Tiezhen（`HuggingFace`）；Naixu；Lizhi；Peter；Suoman；Jack

封面生成——右脑科技

如果你有大模型相关的创业想法，欢迎来找我们聊聊～🛰️：15101136166

> 快邀请你的小伙伴们一起来读大模型日报吧！

> `【日报知识库】`这里是近期日报内容的汇总,欢迎大家查阅

> [大模型日报知识库(分享版)](https://miracleplus.feishu.cn/wiki/KapVwXfkriOBiskB0B2cUHxjnTh)

>

> 欢迎大家多多帮我们分享,转发~~

> 如果大家有任何的建议或者吐槽,欢迎随时找我聊天~

![](img/3d49a79ae684efcfbfe4f1d2c81a2d9b.png)
