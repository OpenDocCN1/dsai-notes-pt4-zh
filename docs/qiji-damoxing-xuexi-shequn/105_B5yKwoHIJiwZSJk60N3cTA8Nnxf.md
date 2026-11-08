# 奇绩大模型日报（9月 7-8日）

> 来源：[https://miracleplus.feishu.cn/docx/Bp7ldQwgjoaijXxi1FlcxDeQn1f](https://miracleplus.feishu.cn/docx/Bp7ldQwgjoaijXxi1FlcxDeQn1f)

# 推特

### Anthropic AI 提示工程深入解析：由Cluade总结

https://x.com/omarsar0/status/1832143997512642635

Anthropic 最近的 AI 提示工程深入解析是必看内容！

以下是 Claude 对所提到的提示技术和技巧的总结：

1.  编写提示时要清晰、准确。清晰陈述任务并描述概念的能力至关重要。

1.  需要快速迭代，多次向模型发送提示。优秀的提示工程师能够适应不断的反复调整和微调。

1.  在设计提示时，考虑边缘案例和不寻常的场景。思考你的提示在非典型情况下可能会如何失败。

1.  使用不完美、现实的用户输入来测试你的提示。不要假设用户会提供完全格式正确或语法无误的查询。

1.  仔细阅读并分析模型输出。密切关注模型是否按预期遵循指令。

1.  去除所有假设，并清晰传达完成任务所需的完整信息。系统性地分解任务，确保包括所有必要细节。

1.  考虑模型的“心智理论”来编写提示。思考模型可能会以与预期不同的方式解读你的指令。

1.  使用版本控制并在工作时跟踪实验。将提示像代码一样对待，在管理和迭代中加以利用。

1.  让模型识别指令中的不明确部分或歧义。这可以帮助改进和优化你的提示。

1.  精确但不要过度复杂化。目标是清晰的任务描述，不要构建不必要的抽象。

1.  考虑典型案例和边缘案例之间的平衡。在处理边缘案例的同时，不要忽视主要使用场景。

1.  考虑提示如何集成到更大的系统中。考虑数据源、延迟和整体系统设计等因素。

1.  不要仅依赖写作技巧；提示工程需要清晰的沟通和系统化思考。优秀的写作者不一定是优秀的提示工程师，反之亦然。

1.  与客户合作时，帮助他们理解用户输入的实际情况。引导他们考虑现实世界的使用模式，而不是理想化的场景。

1.  广泛练习查看数据和模型输出。熟悉模型如何响应不同类型的提示和输入。

![](img/54783315a254315c579698e245e25f10.png)

### OpenAI 考虑为新大型语言模型设定每月 2000 美元的订阅价格

https://www.pymnts.com/artificial-intelligence-2/2024/report-openai-considers-2000-monthly-subscription-prices-for-new-llms

据报道，OpenAI 高管正在考虑为公司即将推出的大型语言模型（LLM）——如 Strawberry 和 Orion——设定高达每月 2000 美元的订阅价格。

据路透社 9 月 5 日星期四报道，引用了一篇基于匿名消息来源的《信息》付费文章，OpenAI 高管在关于这些 LLM 的早期内部讨论中讨论了更高的价格。

对于 PYMNTS 的置评请求，OpenAI 暂未立即回复。

![](img/f79b84c8804719a9f5357d7b84ac0d8c.png)

### 用于反思提示的系统提示示例

https://x.com/rohanpaul_ai/status/1832216833530712081

你是一名 AI 助手，设计目的是提供详细的、逐步的回应。

1.从一个 <thinking> 部分开始。

2.在 thinking 部分中：

a. 简要分析问题并概述你的方法。

b. 提出解决问题的清晰步骤计划。

c. 使用“思维链”推理过程（如有必要），将你的推理过程分解为多个步骤。

3.对于每个想法，包含一个 <reflection> 部分，你需要：

a. 审查你的推理。

b. 检查是否有潜在的错误或疏漏。

c. 如果必要，确认或调整你的结论。

4.确保关闭所有 reflection 部分。

5.用 </thinking> 关闭 thinking 部分。

6.在 <output> 部分中提供你的最终答案。

请始终在回答中使用这些标签。在解释中要彻底，确保回答清晰完整。

记住： <thinking> 和 <reflection> 必须是标签，且必须在答案中关闭。

确保所有 <tags> 都位于独立的行上，且不要包含其他文本。

![](img/55b049b23ac6ea34c8ee8f8bd5786c75.png)

### 本地 AI 只运行在一张 SD 卡上：Henkel分享一张运行 Linux 的无线 SD 卡

https://x.com/BrianRoemmele/status/1832390445961244755

Henkel：一张运行 Linux 的无线 SD 卡。开源的 SiP 非常适合 SD 卡的尺寸，还留有足够的空间用于放置 WiFi 芯片。

---

Brian Roemmele：你的本地 AI 在一张 SD 卡上。

想象一下这台运行 Linux 的计算机在一张 SD 卡上。

一个开源的 SiP 非常适合 SD 卡的尺寸。

我将在这张 SD 卡上运行一个本地和私密版本的 AI，无需连接到互联网。

（SiP 由 @QVHenkel 提供）

![](img/f2f255e86acac3d49f539ac8b79c720e.png)

### MFLUX：在 Mac 上本地运行 FLUX 模型

https://x.com/ivanfioravanti/status/1832399920328061207

Apple Silicon 用户现在快去安装 MFLUX！

+   在 http://fal.ai 上微调你的 Flux LORA

+   本地下载它们

+   感谢 @filipstrand 和 MLX，你可以在 Mac 上生成任意数量的图片！

你可以在 schnell 或 dev 上使用你的自定义 Lora。

---

在你的 Mac 上本地运行来自 Black Forest Labs 的强大 FLUX 模型！

MFLUX 是将 Huggingface Diffusers 库中 FLUX 实现逐行移植到 Apple MLX 的项目。MFLUX 刻意保持最小化和明确性——网络架构是硬编码的，除了 tokenizer 外不使用任何配置文件。其目标是拥有一个小型的代码库，专注于表达这些模型（因此避免过多的抽象）。虽然 MFLUX 优先考虑可读性而非通用性和性能，但它仍然可以非常快速，甚至在量化后速度更快。

![](img/357ac3cb632cb3e47cecfd0b3bb57725.png)

### AI Codex：Cursor AI 的自我改进系统

https://x.com/zbeyens/status/1832079140083687671

介绍 AI Codex：@cursor_ai 的自我改进系统。

◆ http://codex.md：错误和学习存储库。

◆ http://learn.md：自动保存新见解。

◆ http://split-codex.md：智能分类。

免费、开源、即用型模板。

![](img/3376d66c25ea32ed4331f6f077fce349.png)

[ssstwitter.com_1725785192636.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/HUf7b7Xs5owbxGxpnpCclAU8nwb)

# 资讯

### 直击国产AI芯片生存现状：GPU造血，TPU突袭，Chiplet成大势，网络卡脖子

https://mp.weixin.qq.com/s/g_tvwJvl6zH70WUZ-obY5Q

2024年全球AI芯片峰会（GACS 2024）于9月6日在北京开幕，吸引了大量观众并通过云直播吸引了120万人次观看。峰会以“智算纪元 共筑芯路”为主题，涵盖AI芯片、Chiplet、RISC-V、智算集群等前沿领域，邀请了50多位行业专家进行分享。

壁仞科技在大会上宣布其异构GPU协同训练方案HGCT取得突破，实现了三种及以上异构GPU共同训练大模型的能力，这是业界首次。清华大学教授尹首一探讨了高算力芯片的五条创新路径，包括数据流芯片、存算一体芯片等。

高通、AMD等国际企业也展示了AI芯片领域的最新成果，涵盖从边缘设备到数据中心的全方位产品线。国产AI芯片企业如爱芯元智、凌川科技等在低成本和高性能方向不断创新，推动了AI芯片在智慧城市、智能驾驶等领域的落地。

峰会还讨论了大模型训练的技术瓶颈及解决方案，Chiplet技术成为打破性能瓶颈的重要方向，推动了AI芯片行业的多元化发展。

![](img/3c9c39e08a2099a71cd8788e76e81191.png)

### 2024全球AI芯片峰会收官：架构创新群雄混战，边端较劲大模型，两大榜单公布

https://mp.weixin.qq.com/s/sWduYx8ta1IxUp_Wftyqnw

9月6日至7日，2024全球AI芯片峰会（GACS 2024）在北京举行，主题为“智算纪元 共筑芯路”。峰会聚焦AI芯片在算力、网络、存储等方面的最新技术和落地进展，吸引了50多位行业专家和超过1500位参会者。

技术亮点包括北京超弦存储器研究院首席科学家戴瑾的演讲，强调存算分离在处理大规模AI模型时的优势，同时介绍了SRAM、DRAM等传统与新兴存储介质的技术现状。珠海芯动力推出的可重构并行处理器（RPP）凭借高效算力和低功耗在边缘计算领域表现出色，基于14nm工艺与英伟达A100芯片竞争。

此外，亿铸科技提出存算一体架构，通过高速互连和Chiplet技术应对算力需求增长。时识科技展示了类脑智能的前景，类脑计算芯片在视觉处理和低功耗领域显示出显著优势。云天励飞、锋行致远等公司专注于边缘AI大模型，推出高性价比的存算一体解决方案和国产工艺芯片。

![](img/5fe6420b3e9b69c0690a2c1d3de0165e.png)

### Reflection 70B 模型争议

https://mp.weixin.qq.com/s/Ip1Muv-jC6W9iHTBauJshQ

最近，开源大模型社区因AI初创公司HyperWrite推出的Reflection 70B模型引发热议。该模型基于Meta的Llama 3.1 70B Instruct，使用Llama chat格式，确保了与现有工具和pipeline的兼容性。Reflection 70B在多个基准测试（如MMLU、MATH、IFEval、GSM8K）上表现超越GPT-4，甚至超过了Llama 3.1 405B，因而被誉为“开源大模型新王”。

Reflection 70B由两位开发者（HyperWrite CEO Matt Shumer 和Glaive AI创始人Sahil Chaudhary）仅用3周开发完成。然而，Artificial Analysis的独立评估表明，该模型的MMLU得分仅与Llama 3 70B相同，并低于Llama 3.1 70B。此外，科学推理与知识（GPQA）和定量推理（MATH）测试结果也未达到预期。

Reddit上的讨论显示，Reflection 70B可能是基于Llama 3而非Llama 3.1，并使用了LoRA（低秩近似）微调技术。部分用户通过代码分析验证了两者的权重差异，表明Reflection模型与Llama 3更为相似，尤其是在语言理解任务中反应不一致。此外，模型的权重问题也引发了社区质疑，开发者Matt Shumer解释称，Hugging Face平台上传的模型权重出现问题，并计划重新训练模型。

随着模型的更多测试结果曝光，社区对Reflection 70B的关注逐渐转向开发者的透明性和商业动机。开发者团队表示将尽快解决技术问题并重新上传训练后的模型，未来表现如何仍待观察。

![](img/1d9c91aa1aa6c47e0a7cef604c7a9832.png)

# 产品

### CX Genie

贡献人：

https://www.cxgenie.ai/

CX Genie 是一个无代码的 AI 驱动客户支持平台，可以帮助企业提升客户体验和优化内部运营。它提供了一系列功能，包括聊天机器人、工单管理和帮助台，能够执行自动化任务并个性化客户互动。

![](img/bea6f0e30a4945d34bd4bb0c6ac15607.png)

### Trupeer

贡献人：

https://www.trupeer.ai/

Trupeer 是一款基于人工智能的工具，可以将屏幕录制快速转换为高质量的产品视频和用户指南，用户只需简单录制，AI 就可以自动生成脚本、配音和视频效果，非常适合内容创作者和产品开发者。

![](img/d97ba727ed1192cd4a15bd1a2ff3969d.png)

### videoGen( YC 24)

贡献人：

https://videogen.io

VideoGen 是一个快速生成专业视频的工具，用户只需简单点击即可创建版权自由的视频，适合内容创作者和企业使用。它提供自定义选项，可以添加音乐和媒体资料，从而降低视频制作的成本和复杂性。

![](img/3814c25bd4d537ad96e20cc561242d62.png)

[rome.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/O8DrbSdwtoo2JsxsNSncPZ5Hnpc)

# HuggingFace&Github

### screenpipe

贡献人：

https://github.com/mediar-ai/screenpipe

screenpipe 是一个开源的屏幕和音频捕捉库，帮助开发者构建基于个人数据的定制化 AI 应用。它提供了一个可靠的数据流，用户只需点击一个按钮就可以在后台持续捕捉屏幕和音频输入/输出数据。

### code2prompt

贡献人：

https://github.com/raphaelmansuy/code2prompt

Code2Prompt 是一个强大的命令行工具，它可以将用户的整个代码库转换为一个全面 Markdown 格式，使得大型语言模型（LLM）更容易理解项目的上下文。它希望可以简化开发人员与 LLM 之间的交互，以进行代码分析、文档编写和改进任务。

# 投融资

### VidAU 完成 Pre-A 轮融资，推动 AI 视频模型发展

https://mp.weixin.qq.com/s/agsVxoak37GYz_ftO-Lmeg

跨境电商 AI 视频大模型 VidAU 近日宣布完成由诺辉资本领投的 Pre-A 轮融资，此轮资金将用于提升智能算法和模型研发，进一步巩固其技术壁垒。此前，VidAU 已获得 River Jin Technology Limited 的天使轮投资。

这一系列资金的注入，表明投资者对 VidAU 在视频营销领域的创新能力充满信心。VidAU 通过其 AI 驱动的平台，简化了视频制作流程，用户仅需输入简单信息便能生成复杂结构的视频，帮助全球电商企业提升效率并降低成本。此外，VidAU 的 AI 数字人功能能够通过多种语言和多样化的风格吸引观众，特别适用于 TikTok、YouTube 等社交平台的营销推广。

VidAU 的技术团队占公司总人数的 80% 以上，展现了其强大的研发实力。创始人 Joanna 表示，公司未来将持续优化模型，并与全球品牌深度合作。投资方诺辉资本则看好 VidAU 在全球市场的拓展潜力，认为 AI 将深度融入各行各业，推动行业发展与创新，为企业带来更大价值。

公司官网：https://www.vidau.ai/

![](img/52b46736263a8a31c7a1c995a05633a4.png)

# 学习

### LLM推理引擎基准测试常见错误

贡献人：

https://docs.google.com/document/u/0/d/1fEaaIQoRQLbevRu3pj1_ReOklHkoeE7ELqZJ3pnW-K4/mobilebasic?_immersive_translate_auto_translate=1#heading=h.ntrtei46qfj8

**基准测试的重要性**

在推理优化的发展与比较过程中，基准测试至关重要。vLLM v0.6的发布展示了令人瞩目的性能进展，但在复现其结果时，发现了一些基准设置问题，记录这些错误以避免未来出现相同问题。

错误1：忽视优化的权衡或未全面报告各项指标的影响

vLLM v0.6中的关键优化是多步调度器，通过将多个解码步骤分组以减少CPU开销，提升了吞吐量。但这显著增加了首字节时间（TTFT）和字节间延迟（ITL），对在线服务流体验造成不利影响。vLLM在ShareGPT数据集上的测试结果显示，TTFT增加了3倍，ITL增加了10倍，导致不流畅的流式体验。虽然vLLM整体E2E延迟略低，但未启用SGLang的平滑流式特性。

报告优化时应全面覆盖所有指标，vLLM博文仅提供TPOT指标，未准确反映流式体验，并忽视了ITL等关键指标。

**错误2：y轴比例设置不当**

vLLM博文中的图表y轴设定过大（8000ms），掩盖了低QPS情况下TTFT的显著差异，而这些差异对实际生产环境至关重要。在实际在线场景中，SGLang的TTFT降低了3倍，但图表未能反映这一结果。

**错误3：超参数调优不公平**

vLLM在基准测试中未使用默认参数，而是对自身参数进行了调优（如多步调度器、GPU内存使用），但未对其他基线进行同等调优。重新运行测试后，稍微调整SGLang的超参数，结果表明其离线峰值吞吐量优于vLLM。

相关链接：https://x.com/zhyncs42/status/1832158129846677632

### 安利LinFusion，能单卡生成16K超高清图像

https://zhuanlan.zhihu.com/p/718203486?utm_psn=1815733814737108992

[现代扩散模型，特别是那](https://zhida.zhihu.com/search?q=Transformer&zhida_source=entity&is_preview=1)些使用基于Transformer的UNet进行去噪的模型，严重依赖自注意力操作来处理复杂的空间关系，从而实现了出色的生成性能。然而，这种现有范式在生[成高分辨率视觉](https://zhida.zhihu.com/search?q=%E7%BA%BF%E6%80%A7%E5%A4%8D%E6%9D%82%E5%BA%A6%E6%A8%A1%E5%9E%8B&zhida_source=entity&is_preview=1)内[容时面临着显](https://zhida.zhihu.com/search?q=%E6%B3%A8%E6%84%8F%E5%8A%9B%E5%BD%92%E4%B8%80%E5%8C%96&zhida_source=entity&is_preview=1)著挑战，原因是其时间和内存复杂度随着空间令牌数[量的增加呈二次](https://zhida.zhihu.com/search?q=%E7%BA%BF%E6%80%A7%E6%B3%A8%E6%84%8F%E5%8A%9B%E8%8C%83%E5%BC%8F&zhida_source=entity&is_preview=1)增长。为了解决这一限制，我们在本文中提出了一种新的线性注意力机制作为替代方案。具体来说，我们从最近引入的线性复杂度模型开始探索，例如Mamba、[Mamba2和](https://zhida.zhihu.com/search?q=SD-v2.1&zhida_source=entity&is_preview=1)Gated Linear Attention，并识别出两个关[键特性——注](https://zhida.zhihu.com/search?q=16K%E5%88%86%E8%BE%A8%E7%8E%87&zhida_source=entity&is_preview=1)意力归一化和非因果推理，这些特性提高[了高分辨率视觉生成的](https://zhida.zhihu.com/search?q=IP-Adapter&zhida_source=entity&is_preview=1)性能。基于这些见解，我们引入了一种广义的线性注意力范式，它作为多种流行线性令牌混合器的低秩近似。为了节省训练成本并更好地利用预训练模型，我们初始化了我们的模型并从预训练的StableDiffusion (SD)中提炼知识。我们发现，经过少量训练后，提炼出的模型LinFusion的性能与原始SD相当甚至更优，同时大大降低了时间和内存复杂度。在SD-v1.5、SD-v2.1和SD-XL上的大量实验表明，LinFusion能够实现令人满意的零样本跨分辨率生成性能，能够生成如16K分辨率的高分辨率图像。此外，它与预训练的SD组件高度兼容，如ControlNet和IP-Adapter，无需进行适配。

![](img/1a7b3cfc4f75e7c466c3c562bacefc09.png)

### 基于 chunked prefill 理解 prefill 和 decode 的计算特性

https://zhuanlan.zhihu.com/p/718715866?utm_psn=1815729196112175104

在对 SGLang 服务器参数和功能的探索中，作者发现“分块预填充”（Chunked Prefilling）是 LLM 推理优化中的关键技术，尤其是在《SARATHI: Efficient LLM Inference by Piggybacking Decodes with Chunked Prefills》这篇论文中。分块预填充方法将不同长度的提示分割为相同长度的块，以提高 GPU 利用率。虽然像 GPT 这样的 AI 模型错误地理解了分块和非分块预填充在数学上的等价性，但你发现分块预填充能够在预填充过程中产生的空闲时间（气泡）中插入解码请求，从而优化资源使用。

从技术上讲，预填充阶段并行处理所有 token，GPU 利用率很高，但在解码阶段，每次只生成一个 token，导致 GPU 利用率较低，主要受到内存访问的限制。通过将提示分块，减少了因重复读取 KV 缓存而导致的内存开销，同时复用了模型参数，使得解码过程从内存受限操作转变为计算受限操作，大幅提升了性能，解码时间最多可以减少 90%。然而，这需要在块大小上进行权衡：较小的块可以提高解码效率，但可能由于频繁的 I/O 操作而降低预填充效率。

Transformer 架构在预填充和解码阶段保持一致，但分块预填充通过减少流水线气泡，改善了整体模型并行性，通过平衡预填充和解码的优化目标，提高了模型的运行效率。

![](img/cd6ce635b342379e1290c3dff8bc3b22.png)

### 更适合 flash attenion 体质的长上下文训练方案

https://zhuanlan.zhihu.com/p/718486708?utm_psn=1815729769813266432

技术上，zigzag ring attention 具备三个核心优势：

1.  显存占用不随上下文长度增长。

1.  计算与通信在各卡上基本平衡。

1.  计算与通信可并行化（overlap）。

然而，实际实现中面临了如下技术挑战：

+   **精度问题**：flash attention 输出的 bf16 精度较低，累计误差可达 1e-3，且修改为 fp32 需大规模源码改动。

+   **varlen 版本低效**：需将数据拆分为多份并进行 padding，导致 kernel 并行度下降，运行速度慢。

+   **通信效率低**：依赖点对点通信（p2p），而非更高效的集合通信（cc）。

**为解决这些问题，作者**参考了 llama3 团队的优化方案，采用 all-gather 模式先收集 key 和 value，再进行局部计算。这样减少了通信延迟并提高了 mask 适应性，虽然显存占用增多，但可以通过每次只 all-gather 一个 head 来缓解显存压力。

在 varlen 数据处理上，作者提出了基于 cu_seqlens 的改进方案，避免了 padding 和数据拆分，提高了效率。最后，反向计算采用 reduce scatter 操作，确保梯度计算的精度。

该方案已实现为 llama3_flash_attn_varlen_func，实现细节在 GitHub 上公开，具备较好的精度和可用性。

![](img/d6d255b00a15562d0fbbe77a94cbed2b.png)

### “闭门造车”之多模态思路浅谈（三）：位置编码

https://zhuanlan.zhihu.com/p/718711086?utm_psn=1815731087495802880

当前多模态LLM在位置编码方面尚未达成共识，不同模态（如文本、图像、视频）的位置表示维度不一致，导致难以统一处理。例如，文本是1D序列，图像是2D，视频是3D。尽管现有工作通常通过将所有模态压平并使用RoPE-1D解决问题，但这种方法缺乏优雅性，并且可能限制模型性能。

为了解决这一难题，作者提出了一种改进的RoPE-2D方案，称为RoPE-Tie-v2，能够在混合模态下统一使用二维位置编码，并在单模态下退化为RoPE-1D或RoPE-2D。同时，该方案确保了不同模态间的对称性和等价性，使得文本和图像的位置信息可以兼容处理。

RoPE-Tie-v2的核心思想是将文本的位置从1D提升到2D，与图像统一处理，同时保持向后兼容性，确保在单模态下能恢复原有的RoPE编码。对于视频等3D模态，该方案通过类似的扩展处理时间维度，但作者认为时间维度与空间维度不同，应该允许无约束的自回归生成视频，而不需提前确定帧数。

相比于先前的RoPE-Tie方案，RoPE-Tie-v2优化了图像内部位置的均匀性，并通过较大的位置跳跃实现模态隔离，使得多模态交互更加自然。该方案兼顾了对称性和等价性，但在实际效果上仍需进一步实验验证。

![](img/e05f0d6e7ad2965d71610814de8c93ab.png)

### CUDA-MODE课程笔记 第11课: Sparsity

https://mp.weixin.qq.com/s/28Ku4_EXm0H-ipJX9LKF6g

这节课主要介绍了PyTorch团队在稀疏性(Sparsity)方向的工作，特别是在GPU推理上的进展。稀疏性技术虽然冷门，但其核心思想是通过移除部分模型参数或者使用低比特数据类型，提升推理效率。PyTorch团队以前专注于CPU和边缘设备，现在随着生成式AI模型（如LLMs和Vision Transformers）规模的扩大，推理需要在GPU上进行。

Sparsity的关键流程包括模型训练、剪枝、精度恢复以及优化的Sparsity kernel的使用。技术核心是通过识别零值参数跳过不必要的计算。团队重点研究了几种稀疏性模式，包括非结构化稀疏性、半结构化稀疏性(如2:4模式)和块稀疏性。每种模式在性能与精度之间的平衡不同。GPU推理中，半结构化稀疏性能在保持相对精度的情况下提高计算速度。

课程还深入探讨了如何通过PyTorch结合稀疏性和量化来加速推理，特别是使用NVIDIA的cuSPARSELt库处理稀疏矩阵乘法。尽管理想情况下2倍加速，实际效果通常约为1.6倍，且当前在精度和性能之间仍面临挑战。

![](img/294e16bc69b843eebf3728312c195c95.png)

### 对话Andrej Karpathy，涉及自动驾驶、人形机器人、合成数据、AI认知、小模型、AI教育等话题

https://mp.weixin.qq.com/s/EAm8AYrW219JWB6Ea_OPpQ

**1、自动驾驶技术：**

+   自动驾驶技术的进步，从早期PPT到实际应用的过渡。

+   技术与监管之间的差距及挑战，以及如何克服这些挑战。

+   特斯拉和Waymo在自动驾驶技术发展上所采用的不同策略。

**2、人形机器人：**

+   在将人形机器人推向消费者市场之前，先在制造和仓库环境中进行sim2real的重要性。

+   当前机器人技术在落地过程中存在的挑战。

**3、AI技术：**

+   transformer在AI研究中的变革性作用，特别是在处理大量数据和可扩展性方面。

+   合成数据生成在训练稳健模型中的作用，以及如何防止模型崩溃。

+   小模型蒸馏技术落地。

**4、AI与人类合作：**

+   AI在特定任务中超越人类认知的可能性。

+   结合人类知识，增强AI认知的可能性。

+   关于人类与AI合作的未来以及这种合作可能带来的社会和技术影响。

**5、AI与教育：**

+   使用AI创造个性化学习体验的可能性，以及如何使教育更具吸引力和可接受性。

+   重新评估传统教育结构的必要性，以便更好地为个人应对未来的挑战。

+   讨论了基础学科（如数学、物理和计算机科学）在培养批判性思维技能方面的重要性。

+   最后强调了技术的持续演变，以及它对交通运输、机器人技术、认知增强和教育等生活各个方面的潜在重大影响。

![](img/4276a9b620e58159c8f40aa7f7a5bc46.png)

# 声明

本文档仅供学习交流使用，版权归原作者所有，若涉侵权，请联系Jack Jin 15101136166

##

（欢迎在下方反馈您宝贵的意见！整理不易，觉得有用给AI日报小组点个赞👍吧！）

ChatGPT；Angela；Zijia Liu；Alex；Zeqi Tan；Tiezhen（HuggingFace）；Naixu；Lizhi；Peter；Suoman；Jack

封面生成——右脑科技

如果你有大模型相关的创业想法，欢迎来找我们聊聊～🛰️：15101136166

> 快邀请你的小伙伴们一起来读大模型日报吧！

> **【日报****知识库****】**这里是近期日报内容的汇总,欢迎大家查阅

> [大模型日报知识库(分享版)](https://miracleplus.feishu.cn/wiki/KapVwXfkriOBiskB0B2cUHxjnTh)

>

> 欢迎大家多多帮我们分享,转发~~

> 如果大家有任何的建议或者吐槽,欢迎随时找我聊天~

![](img/14dd3ec26af3d77ca74fd03a12c130db.png)