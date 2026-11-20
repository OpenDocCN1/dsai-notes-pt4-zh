
这块其实相对来说跟视频平台的创编流程紧密相关，很多短视频和中长视频平台公司都有布局，在其创编工具中逐步完善相关的功能，但是现在大部分还都是过程辅助阶段，完全自主的从无到有生成能力上还存在一定的技术瓶颈，有一些国外的创业明星产品，有一些突破，但是暂时看实用性并不是特别高，我觉得除了下面说的合规和易用性挑战外，最大的技术以及生态发展挑战来说，就是稳定可控性了。现在很多视频生成能力时长都比较短，生成的效果虽然可以做到尽量满足文字的描述，但是可能很多关键性的细节难以控制，如果只是玩玩或者简单视频还行，大型的工业化生产就还有提升空间，视频也需要文生图的`lora` + `controlnet`时刻

![](img/2051f15825a2b599c26a3b4d4fb6841d.png)

![](img/c04bc0239eae17000820e42ba458261c.png)

![](img/f9af4815ef329f8d1fb78af89f26718c.png)

![](img/9dfb7c140e9b037058fcd011ed2e7344.png)

![](img/b158e1fb28ed5f01f4ec721f20a605c0.png)

![](img/e1e4189430916b87fd6fa7ee82815918.png)

# 原理和案例

+   `Runway`: 这家公司参与了《瞬息全宇宙》的幕后制作，`Runway`的核心产品为视频编辑器`Gen-1`和`Gen-2`。`Gen-1`模型可以通过输入文本或图像，改变原视频的风格，例如将现实中的实拍镜头转换为卡通剪纸世界。`Gen-2`模型可以直接使用文本生成视频场景， 使用户可以使用文本提示生成“逼真的视频内容”并“自动剪辑视频”。

![](img/e68326c336301d8e2b058c79f9781f0f.png)

+   `Zeroscope`: 这是一款基于 `Modelscope` 的无水印视频模型，能够生成 1024 x 576 的高质量视频。该模型是使用 24 帧、1024x576 分辨率下的 9，923 个剪辑和 29，769 个标记帧，根据带有偏移噪声的原始权重进行训练的。主要是名为`text-to-video-synthesis`(文本生成视频大模型)，具有17亿参数数，由阿里达摩院提供。只需输入文本描述，就能生成对应的流畅视频。

![](img/c9faf6061cb57b5fe64d8472d2963447.png)

<https://fffiloni-zeroscope--qlggk.hf.space/file=/tmp/gradio/ebc612c3d979e65ef5a6ff0943a2e863eaacf9d5/tmpj43m67qj.mp4>

在草地上奔跑的狗狗：[`Zeroscope Text-To-Video`](https://huggingface.co/spaces/fffiloni/zeroscope) - a Hugging Face Space by fffiloni

+   `Pikalib`: https://www.pika.art/demo

<https://video.wixstatic.com/video/af70ef_cc6d03729d344f0b9fe8633ad81b9af1/360p/mp4/file.mp4>

<https://video.wixstatic.com/video/af70ef_caffbe10a41c4a848b49f6ac0af8a2df/360p/mp4/file.mp4>

# 三维模型应用

这块因为使用涉及比较少，仅提供一些易[观数据的](https://www.analysys.cn/article/detail/20021029)调研资料作为参考，更详细可以参考原始报告

![](img/6274c1ab78068e0eb058b607c0d62e27.png)

![](img/bee4ee0ace4ac2c2ed435d53dbf52692.png)

![](img/fd42dec6eac38f357ac9687741dcec85.png)

![](img/7082de462bcc61c674e87ef00d43fa36.png)

# 应用场景合集

|  |  | **创意生成（抽卡）** | **风格迁移** | **内容理解** | **局部调整** | **辅助工具** |

| --- | --- | --- | --- | --- | --- | --- |

| 文本 | 开源 | +   `text-generation-webui`：致力于打造文本生成领域的[`stable-difussion-webui`](https://www.bilibili.com/video/BV1fh4y1M7DX/)，但是感觉可能最大的制约主要是底模的使用门槛太高，虽然`SD`有一定门槛，大部分消费级显卡还是可以跑的，但是现在的`LLM`大部分还在`ChatGPT3.5`水平，对消费级显卡也不友好，最近看到一个视频讲的不错 | 类似图片也可以通过`lora`低成本实现，其实最早`lora`是用在文本生成领域的 | 基于数据源增强减少幻觉： | 基本`LLM`都是通过提示词完成的，比如常见的润色和纠错 | +   加速方案 |

|  | 免费工具 | `text-generation-webui`整合包（还在早期，相对于图像社区比较小众） | 基本`LLM`都是通过提示词完成的 | 基本`LLM`都是通过提示词完成的 | 基本`LLM`都是通过提示词完成的 |  |

|  | 用户产品 | +   [`Character.AI`](https://beta.character.ai/)：如果在基础模型能力上不会因为角色化变形，其实我觉得角色化可能是用户最有市场的场景，包括看到结合图像生成有更多的想象空间 | 基本`LLM`都是通过提示词完成的 | +   [`chatbase`](https://www.chatbase.co/)：使用任何在线或离线数据快速训练的聊天机器人，并将其作为`iframe`或支持小部件嵌入网站，从默认能力提供来看，也是跳出做通用，更偏向于做网站线索收集场景 | 基本`LLM`都是通过提示词完成的 | +   [`relevanceai.com`](https://relevanceai.com/)：构建端到端自定义`LLM`应用程序和代理，批量共享、使用和运行 |

| 图片 | 开源 | +   `stable-difussion-webui`：YYDS不解释，我觉得最核心还是开放的插件机制，以及丰富的插件生态 | 现在基本都是通过`lora`低成本实现 | 通用场景下，主要是识别图片中的大致内容以及生成描述 | +   `stable-diffusion-inpainting`：文本说明修改图片内容 | +   [`CodeFormer`](https://github.com/sczhou/CodeFormer)：图像超清修复，被集成到了`SD`-webui中 |

|  | 免费工具 | +   [`秋叶整合包`](https://www.bilibili.com/video/BV1iM4y1y7oA/)：基本是做的最容易上手的`SD`-webui整合包了，而且作者一直持续更新新功能，持续分享使用经验 | 现在基本都是通过`lora`低成本实现 | 暂未发现，感觉如果只是单纯的理解可能价值不大，更多跟应用目标结合，不过如果是`OCR`倒是有很多 | `SD`-webui的各种整合插件 |  |

|  | 用户产品 | +   [`Midjoureny`](https://www.midjourney.com/)：底模能力太强大了，除了需要付费以及没有独立软件这两个缺点外，因为大部分的效果都是可以通过提示词实现，所以相对`SD`门槛会低一些 | `Midjoureny`更倾向于使用提示词描述，包括绘画风格，类型还有作者画风 | 这块`OCR`的服[务](https://bobtranslate.com/service/ocr/volcengine.html)还是挺多的，就不一一列举了，可以参考这里 | +   [`改图鸭`](https://www.gaituya.com/)：大部分的图片处理工作都可以用这个工具完成 |  |

| 音频 | 开源 | +   [`Bark`](https://github.com/suno-ai/bark)：通过提示词生成真实的人声，包括音乐、背景噪音和简单的音效，不完全是`TTS`，基于`transformer`模型，更类似文生图在音频的场景（音色抽卡） | +   [`VITS`](https://github.com/jaywalnut310/vits)（`TTS`）：下面的歌声转换是基于该能力实现的，你也可以结合自己的需求使用`VITS-fast-fine-tuning`微调定制 | 其实音频的理解本质可以归为文字的理解，所以下面主要是`ASR`，因为`ASR`的精度决定了文字理解的上限 | 暂无，不过可以通过很多剪辑软件切分和合并音频轴 | +   [`AudioSep`](https://github.com/Audio-AGI/AudioSep)：可以使用简单自然语言指令从混合音频中分离出特定的声音 |

|  | 免费工具 | +   [`Bark`整合包](https://www.bilibili.com/video/BV1MP411m7cw/)：下载整合包之后需要先联网，生成`cache`文件的内容有检验，没有打包，因为打包也没用 | +   [`微软语音合成助手`](https://www.bilibili.com/video/BV1QW4y1R7oo)（`TTS`）：输入文本可以选择希望的声音来朗读 | 其实音频的理解本质可以归为文字的理解，所以下面主要是`ASR`，因为`ASR`的精度决定了文字理解的上限 | 暂无，不过可以通过很多剪辑软件切分和合并音频轴 |  |

|  | 用户产品 | [`ACE Studio`](https://ace-studio.timedomain.cn/c)：结合`AI`能力进行音乐编曲，工具做的体验很棒，但是也需要一些基础的乐理知识才能用。因为普通歌手不太容易赚到钱，不确定长期商业前景 | +   [`大饼AI变声器`](https://dubbing.tech/)：免费客户端，有免费限时的声源可选 | 其实音频的理解本质可以归为文字的理解，所以下面主要是`ASR`，因为`ASR`的精度决定了文字理解的上限 | 暂无，不过可以通过很多剪辑软件切分和合并音频轴 |  |

| 视频 | 开源 | +   [`Text2Video-Zero`](https://github.com/Picsart-AI-Research/Text2Video-Zero)：文本到图像扩散模型的视频生成器，这个抽卡比图片还不可控 | +   [`SimSwap`](https://github.com/neuralchen/SimSwap)：具有单个训练模型的图像和视频上的任意面部替换框架 | 暂无，不过其实可以抽取视频帧转化成图片理解 | +   [`FateZero`](https://github.com/ChenyangQiQi/FateZero)：通过文本描述调整视频中的内容 |  |

|  | 免费工具 | +   `sd-webui-text2video`：`SD`插件方便直接使用 `ModelScope` 或者 `VideoCrafter` 实现图片生成视频的效果  | +   [`CoDeF`](https://github.com/qiuyu96/CoDeF)：传统的视频处理方法通常需要逐帧分析。`CoDeF`通过仅在一个图像上部署算法，并通过时间变形字段将结果传播到整个视频，提高了效率，实现对于视频内容的风格迁移 | +   `Segment-and-Track-Anything`：直接通过文字提取视频中的目标物体，在各种场景中的分割和跟踪功能，例如街景、`AR`、单元格、动画、航拍等 | +   [`roop`](https://github.com/s0md3v/roop)：拍一段视频，用你选择的脸替换里面的脸。你只需要一张想要的脸的图像。没有数据集，没有训练 |  |

|  | 用户产品 | +   [`Runway Gen-2`](https://runwayml.com/)：`Runway`是一家应用人工智能研究公司 艺术、娱乐和人类创造力的下一个时代，在视频生成领域效果卓越 | 之前出现过然后被快速封杀了 | 暂无，不过其实可以抽取视频帧转化成图片理解 | +   [`Rask.AI`](https://rask.ai/)：翻译视频，其实本质应该还是提取音频，然后音频合成回去 |  |

| 代码 | 开源 | +   [`ChatDev`](https://github.com/OpenBMB/ChatDev)：单角色`SOP`生成代码 | +   [`gpt-migrate`](https://github.com/joshpxyne/gpt-migrate)：轻松将代码库从一种框架或语言迁移到另一种 | +   `screenshot-to-code`：通过截图直接生成前端代码 | +    | +   [`tldraw`](https://github.com/tldraw/tldraw)：白板工具，借助`GPT-Vision`能力可以转化代码或者其他产物 |

|  | 免费工具 | +    |  |  |  |  |

|  | 用户产品 | +   [`Github Copilot`](https://github.com/features/copilot)：全能型代码辅助工具，支持通过自然语言编程 |  |  |  |  |

| 3D | 开源 | +   [`shap-e`](https://github.com/openai/shap-e)：`OpenAI`出品，比较擅长对于抽象物体的生成，形象人物效果比较差 |  |  |  |  |

|  | 用户产品 | +   [`csm.ai`](https://www.csm.ai/)：暂未试用过，图片转`3D` |  |  |  |  |

+   [`ChatGLM2`](https://github.com/THUDM/ChatGLM2-6B)

+   [`FastChat`](https://github.com/lm-sys/FastChat)

+   [`BELLE`](https://github.com/LianjiaTech/BELLE)

+   [`stanford_alpaca`](https://github.com/tatsu-lab/stanford_alpaca)

+   [`qlora`](https://github.com/artidoro/qlora)：降低微调成本

+   [`AutoGPTQ`](https://github.com/PanQiWei/AutoGPTQ)：降低模型使用成本

+   [`CTranslate2`](https://github.com/OpenNMT/CTranslate2)

+   [`langchain`](https://github.com/langchain-ai/langchain)

+   [`MetaGPT`](https://github.com/geekan/MetaGPT)

+   [`llama_index`](https://github.com/jerryjliu/llama_index)

+   [`DemoGPT`](https://github.com/melih-unsal/DemoGPT)：只需使用句子即可创建 `LangChain` 应用程序

+   [`OpenLLM`](https://github.com/bentoml/OpenLLM) 如何快速将各种`LLM`大模型部署到生产环境中，且轻松微调、部署、应用、调用大语言模型`llm`？

+   [`LLM-Kit`](https://github.com/wpydcr/LLM-Kit) 各大语言模型的全流程工具 `WebUI` 整合包。支持主流大模型`API`接口和开源模型。支持知识库，角色扮演，`mj`文生图，`LoRA`和全参数微调等等

+   `Dify` - `LLMOps` platform: 开源`LLMOps`工具，体验和设计一流

+   [`OpenAgents`](https://github.com/xlang-ai/OpenAgents/blob/main/README_ZH.md)：开源`Agent`搭建平台，无需代码开发能力，支持本地部署

+   [`Agenta`](https://github.com/Agenta-AI/agenta)：用于构建强大`LLM`应用程序的`LLMOps`平台。轻松试验和评估不同的提示、模型和工作流程

+   [`FastGPT`](https://github.com/labring/FastGPT)：知识的问答系统，为数据处理、模型调用等提供开箱即用功能。通过使用`Flow`工作流编排实现复杂问答场景

+   [`botpress`](https://github.com/botpress/botpress)：构建和部署`GPT`/`LLM`智能体的开源市场

+   [`bisheng`](https://github.com/dataelement/bisheng)：开源大模型应用开发平台，给用户提供最佳实践的方案，包括分析报告、知识库问答，要素提取和对话等

+   [`AgentSims`](https://github.com/py499372727/AgentSims)：通过模拟沙箱环境，设定人物角色，可以模拟验证智能体特定能力

# 智能体开发

> 相比于如火如荼的百模大战，还有卷得不能再卷得基于模型`API`的上层应用开发，后续更有想象空间的方向可能使智能体的开发，外文中一般叫做 `agent`，翻译过来就是代理，比较容易引起歧义。所以一般叫做智能体会更加贴切一些。`ChatGPT`给的定义：在计算机科学和人工智能领域，`"Agent"` 通常指代一个能够感知环境、做出决策并执行操作的实体。这个实体可以是一个程序、一个软件机器人或者其他类似的系统。`"Agent"` 可以根据预定义的目标或目标函数来行动，以最大化某种性能指标。

![](img/1a3674027c2fe32eb5b34628bbd8a58e.png)

|  | `Agent` | 微调 | 知识库 | 提示词工程 |

| --- | --- | --- | --- | --- |

| 技术原理 | 有自主规划和反思能力，会利用工具 | 常见的是通过`Lora`，更详细参考[`PEFT`](https://github.com/huggingface/peft) | 向量化后基于语义搜索匹配 | 激活预训练知识，进行角色扮演 |
