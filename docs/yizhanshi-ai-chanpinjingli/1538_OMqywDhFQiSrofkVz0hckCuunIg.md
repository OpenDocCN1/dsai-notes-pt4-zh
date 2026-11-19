
# Jake：大模型应用层

> 来源：[`https://v11enp9ok1h.feishu.cn/docx/TdHBd12wJohkrLxr5kKcmwgJnJh`](https://v11enp9ok1h.feishu.cn/docx/TdHBd12wJohkrLxr5kKcmwgJnJh)

`https://v11enp9ok1h.feishu.cn/sync/PaIIdslbxsIJUIbkTfPcsMCFnph`

## Transformer决定边界

在CES 2024，李飞飞在争论`LLM`和`AIGC`名称不能混用，吴恩达觉得在公众传播没关系。李飞飞觉得难以接受，个人猜测是它模糊了大模型的本质。

在公众传播层面：

+   `AIGC`：指用`Stable Diffusion`或`Midjourney`生成图像内容，后来泛指用AI生成音乐、图像、视频等内容。

+   `LLM`：指`NLP`领域的大语言模型，如`ChatGPT`。

+   `GenAI`：生成式人工智能模型，国内官方政策文件使用这个词相对科学，涵盖了`LLM`和`AIGC`。

+   `AGI`：指通用人工智能，部分人觉得`LLM`具有`AGI`潜力，`LeCun`反对。

公众传播一般会混用上述名词，但底层是`transformer`结构。

（`stable diffusion`原采用`LDM`+`UNet`，后来改为`DiT`）

而`transformer`底层是`function loss`损失函数

+   `Transformer`是一个大参数（千亿级别）的回归方程。

+   回归方程的`Function loss`拟合`A to B mapping`关系，实现数据集的压缩与还原。

+   `Transformer`是在一定`prompt condition`情况下，`repeat`曾经出现过的数据内容，实现“生成”能力。

+   大语言模型的智能体验在两个数据集压缩后，能解释两个数据集之间地带的“连续”能力。（`Ilya`）

所以大语言模型是一个`perfect memory`，`repeat`曾经出现的内容。它与`Alpha Go`差异：

+   `Alpha Go`是一个增强学习模型，学习结果会调整模型自身参数

+   `Alpha Go`有推理能力，但大语言模型这块很弱。

> `Transformer`决定`LLM`是一个生成式模型。

## 先看工具框架再算AI价值

投资者曾经认为在基座模型基础上有应用层机会和产品，行业人员反馈说`Model as an application`，模型本身就是应用。更严谨的说法是，生成式模型能解决一系列任务，而这些任务价值由生成场景（`value`）、生成效率（`cost`）构成。

| AIGC场景 | 构建元素 | 应用 | 商业价值 | 工程门槛 | 代表作 |

| --- | --- | --- | --- | --- | --- |

| 视频制作 | 剧本、分镜、旁白、音乐、字幕 | 视频生成 | ⭐️⭐️⭐️⭐️⭐️ | ⭐️⭐️⭐⭐️️⭐️ | `SORA` |

| 图片设计 | `Lora` | 商品拍摄/海报设计 | ⭐️⭐️ | ⭐️⭐️ | 妙鸭相机 |

| 图文撰写 | 视频 | 抖音：视频+音乐+旁白+字幕 | ⭐️⭐️ | ⭐️ |  |

| NLP场景 | 输入 | 应用 | 商业价值 | 工程门槛 | 代表作 |

| 代码编程 | 前端：交互界面 `prompt` | `Vscode` + `Copilot` | ⭐️⭐️⭐️⭐️⭐️ | ⭐️⭐️⭐️⭐️ | `Devin` |

| 指令控制 | `Input` | 室内场景：智能音箱 | ⭐️⭐️ | ⭐️⭐️⭐️ | `One` |

| 情感表达 | `Character` | 虚拟游戏：`Character.AI` | ⭐️⭐️ | ⭐️⭐️⭐️ | `Character.AI` |

| 知识问答 | `RAG` | 研究助手（`Consensus`） | ⭐️⭐️⭐️ | ⭐️⭐️⭐️ |  |

所以生成模式应用层价值评估标准是：先场景，后`AI`。

1.  生成工具（编程、视频制作、写真拍摄等）场景价值

1.  生成工具能否有场景数据，`Finetune`基座模型

以`Build Android App`为例，这是一个高门槛高价值的工作。高门槛体现在涉及`Java`，`XML`，`OpenCV`，`Gradle`等专业性，各个版本有各种默认值，又有各种冲突。假如能拿最新的`android`工程代码去`finetune`基座模型，将会提高整个`android`工程编写效率，大大降低`APP`开发成本。

## 留意是否过度包装

### `Agent`智能体

`Agent`是实现特定功能人工智能个体，多个`Agent`形成一个`workflow`。但

1.  `AI`模型本身有误差，多个模型协作会出现累计误差。

1.  `Agent`的`ReAct`并没有学习能力修正误差，成功经验没有调整模型参数（如建立`Qlora`）。每次执行都是重新开始试错，效率低下。

### 新交互范式

手机界提到`ALL IN AI`，新能源汽车提到汽车大模型，`XR`行业提到`AI+XR`新交互方式，就要留意是否过度包装。它们是利用生成式模型什么能力？目前我了解到

+   手机通话中同步翻译（三星`S25`手机）。

+   新能源汽车用到语音交互+`function call`能力，便捷调用第三方`API`。

+   `AI+XR`用到是`GPT4V`和语音交互能力。

+   甚至出现新的`Form factor`，例如`Rabbit R1`，`AI Pin`。

这些能力都一定程度上优化交互效率，非革命性突破。`LLM`通过`function call`转为指令再去执行甚至`ReAct`，效率还是蛮低。在视觉层面，`Gaussian Splatting`的空间重建 + `RL`增强学习，个人觉得更有意思。

`AI`可以改变硬件形态，但可能不是`transformer`。
