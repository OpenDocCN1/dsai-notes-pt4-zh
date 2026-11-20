
最近大语言模型（LLMs）的进展展示了令人印象深刻的代码生成能力，主要通过语言到代码的基准测试进行评估。然而，这些基准测试可能并不能完全捕捉模型对代码的理解能力。我们引入了一个新颖的基准测试CodeJudge-Eval（CJ-Eval），旨在从代码判断的角度而非代码生成的角度评估LLMs的代码理解能力。CJ-Eval挑战模型确定提供的代码解决方案的正确性，涵盖各种错误类型和编译问题。通过利用多样化的问题和精细化的判断系统，CJ-Eval解决了传统基准测试的局限，包括潜在的解决方案记忆问题。在CJ-Eval上对12个知名LLMs的评估显示，即使是最先进的模型也面临困难，突显了该基准测试深入探究模型代码理解能力的能力。我们的基准测试将在`https://github.com/CodeLLM-Research/CodeJudge-Eval`上提供。

![](img/8508d3bf889a72e6d26170f6626f5d41.png)

## Transfusion：用一个多模型模型预测下一个 token 并传播图像

贡献人：

链接：http://arxiv.org/abs/2408.11039v1

我们介绍了Transfusion，这是一个用于训练多模态模型的方法，可以处理离散和连续数据。Transfusion将语言建模损失函数（下一个token的预测）与扩散结合起来，用于训练混合模态序列上的单个Transformer。我们从零开始在混合文本和图像数据上预训练了多个Transfusion模型，达到了7B参数，建立了与各种单模和交叉模态基准的比例律。我们的实验表明，Transfusion比量化图像并在离散图像token上训练语言模型要好得多。通过引入模态特定的编码和解码层，我们可以进一步提高Transfusion模型的性能，甚至将每个图像压缩到仅16个补丁。我们进一步证明，将我们的Transfusion方法扩展到7B参数和2T多模态token会产生一个能够生成图像和文本的模型，与类似规模的扩散模型和语言模型相媲美，获得了两个世界的好处。

![](img/3de34f7a9166ffdcd8a4345c6e686a8d.png)

![](img/46d6605c40d9cfd715ca6d00862a4960.png)

# 产品

## MolyPix.AI

贡献人：

`https://molypix.ai/`

在 MolyPix 可以轻松生成精美的“海报”设计，只需要一句话就能生成设计稿件，所有文字和图片都支持灵活编辑

除此之外，还提供很多 prompt 模板（包含 posters、invitations、postcards、birthday cards … ），让创作更高效、更轻松！

![](img/b5497aa877ac89af4d0ad7282b9f0fa7.png)

## AgentQL

贡献人：

`https://www.agentql.com/`

AgentQL 是一款基于 AI 的网页数据提取和自动化工具，允许用户通过自然语言查询来轻松获取网页数据，避免了传统 `XPath` 和 `DOM` 选择器的脆弱性。它希望提高数据抓取的准确性和灵活性，适应网站的结构变化，提升用户的工作效率。

![](img/1e6879644a8f0c171a68968e1c6a125a.png)

## Fleak

贡献人：

`https://fleak.ai/`

Fleak AI 工作流是一款无服务器 API 构建平台，希望可以简化数据团队的工作流程。它与主要的云数据仓库和向量数据库集成，允许用户快速构建复杂的模型链数据工作流，帮助团队专注于业务影响，而无需处理繁琐的基础设施。

![](img/c1213a527821da693769151ef998c84c.png)

# HuggingFace & Github

## Lerobot

贡献人：

`https://github.com/huggingface/lerobot`

LeRobot 是一个希望提供用于真实世界机器人的模型、数据集和工具的 `PyTorch` 项目。它的目标是降低机器人领域的准入门槛，让每个人都可以贡献和从共享数据集和预训练模型中获益。

![](img/28be943a87d21839af46413705b924a8.png)

## Qwen2-Math-Demo

贡献人：

`https://huggingface.co/spaces/Qwen/Qwen2-Math-Demo`

通义天问放出 Demo，支持通过文本输入数学问题进行解答，如果公式等文本输入困难，可以通过图像或截图输入。

![](img/b25defa5401159710c6a2dd3d34e3d33.png)

# 投融资

## Defcon AI完成4400万美元种子轮融资，助力解决军事物流问题

贡献人：

`https://techcrunch.com/2024/08/20/defcon-ai-raises-44m-seed-to-help-solve-military-logistics/`

Defcon AI，这家成立于2022年的初创公司，已成功完成了由Bessemer Venture Partners领投的4400万美元种子轮融资，其他参与方包括Fifth Growth Fund和Red Cell Partners等。公司专注于开发用于军事物流的复杂软件系统，目前已获得约1500万美元的政府合同，并已将产品投入实际操作中。Defcon AI计划扩大其软件的应用范围，并继续与国防部密切合作，以进一步提升产品的竞争力。

公司官网：`https://www.defconai.com/`

![](img/410ee52d5dc1bb0a8cd5c5bd1d47b8d3.png)

## Eppo完成2800万美元B轮融资，扩大AI实验业务

`https://techcrunch.com/2024/08/20/eppo-lands-new-cash-to-grow-its-app-website-and-ai-experimentation-business/`

Eppo是一家专注于应用、网站和AI实验平台的初创公司，近日完成了由Innovation Endeavors领投的2800万美元B轮融资，参与者还包括Icon Ventures、Amplify Partners和Menlo Ventures。本轮融资使公司估值达到1.38亿美元。Eppo计划利用这笔资金增强其AI实验能力、改进分析工具，并扩大市场推广。自2022年成立以来，Eppo已经吸引了数百家企业客户，包括Twitch、SurveyMonkey和Coinbase等。

公司官网：`https://www.geteppo.com/`

![](img/cfbcf2e3ea6dc1bd83385b75d13f11e3.png)

## Trunk Tools完成3000万美元融资，加速建筑行业文档自动化

`https://techcrunch.com/2024/08/20/trunk-tools-raises-30m-to-help-construction-companies-get-answers-from-documents/`

Trunk Tools是一家专注于建筑行业文档自动化的AI工具公司，近日完成了3000万美元的融资，其中包括由Redpoint领投的2000万美元A轮融资。本次融资将用于扩大公司在纽约的30人团队，并开发新服务，如最近推出的建筑工人激励计划。Trunk Tools的技术通过自然语言处理，帮助建筑团队快速处理大量项目文档，从而提高项目效率并减少因文件管理不善造成的延误与返工。

公司官网：`https://trunktools.com/`

![](img/39ed558722054dade0bc6b3c6bc3a023.png)

## SleekFlow获得700万美元融资，推动AI能力扩展

`https://techcrunch.com/2024/08/20/omnichannel-platform-sleekflow-gets-7m-to-propel-expansion-ai-capabilities/`

SleekFlow是一家总部位于新加坡和香港的社交电商平台，专注于为亚洲市场打造对话式AI套件，近日获得了由Atinum Investment领投的700万美元融资。本轮融资将用于继续开发其AI技术，并进一步拓展东南亚、中东及欧洲市场。此前，SleekFlow已累计融资1500万美元，且年经常性收入（ARR）在2024年6月已增至800万至900万美元。此外，公司团队已扩展至160人，全球客户数量超过5000家。

公司官网：`https://sleekflow.io/`

![](img/8e666c05a126bb0e632388a4a44ddbf6.png)

# 学习

## [Phi-3CookBook](https://github.com/microsoft/Phi-3CookBook)

贡献人：

`https://github.com/microsoft/Phi-3CookBook`

`microsoft/Phi-3CookBook` 仓库是一个专门为了帮助开发者学习如何使用微软的开源 AI 模型 `Phi-3` 而设立的资源库。`Phi-3` 模型是一系列高效且成本效益高的小型语言模型（SLMs），在多种语言、推理、编程和数学基准测试中表现优于同类大小和更大尺寸的模型。该资源库包含了关于设置开发环境、`Phi-3` 模型家族介绍、关键技术理解、AI 安全性、硬件支持、模型可用性等内容的介绍。此外，还提供了快速开始的指南，包括在 `GitHub` 模型目录、`Hugging Face`、`OpenAI SDK`、`HTTP` 请求、`Azure AI Studio`、`Azure MaaS` 或 `MaaP`、`Ollama`、`LM Studio`、`VSCode AI` 工具包、`LiteLLM` 等多个平台上使用 `Phi-3` 的方法。仓库还包含了模型推理、微调、评估以及端到端（E2E）样例的详细教程，涵盖了不同硬件设备和应用场景。用户可以通过 `Azure AI Studio` 的 playground 体验 `Phi-3` 模型，并为自己的场景定制化。

![](img/ba63f53f5b40ac6ecc95d666a0aa24cf.png)

## LM-class

`https://lm-class.org/`

`LM-class` 是一个面向现代语言模型的教育资源库，适合具有机器学习和神经网络基础知识的研究生和高级本科生。该课程强调技术深度而非广度，包括讲座和作业，旨在为学生提供坚实的基础，并提供工具让他们能够自主扩展和更新知识。这些资料最初是为了康奈尔大学 Tech 的 `CS 5740` 自然语言处理课程开发的，并在 2024 年春季首次使用。`LM-class` 由 Yoav Artzi、Anne Wu 和 Omer Gul 创建，并且在每个讲座和作业中都有感谢页。资料采用 CC BY-NC 4.0 许可证分发，希望能够得到广泛多样化的使用。`LM-class` 目前仍在进一步开发中，有许多待完成的内容，计划在未来的修订中添加更多主题，如混合专家模型、近似微调和推断、状态空间模型、`RNN` 和 `CNN` 等。同时，还考虑增加一个专注于 `PyTorch` 的教程，涵盖实验设置、数据处理、参数扫描和基本调试技术。

`https://lm-class.org/lectures`

![](img/d9331953b3467ad8e69eb6516fc2c84a.png)

## 使用`Claude 3.5 Sonnet`处理音频数据

`https://www.assemblyai.com/blog/claude-3-5-sonnet-with-audio-data-python/`

AssemblyAI 发布了一篇教程，指导如何使用 `Claude 3.5 Sonnet` 等模型处理音频和视频数据。`Claude 3.5 Sonnet` 是由 Anthropic 提供的大型语言模型，在语言理解和生成方面表现出色。教程详细介绍了如何通过 `AssemblyAI Python SDK` 安装和配置这些模型，以及如何将其应用于音频和视频文件的转录、总结和信息提取。`LeMUR` 框架作为一个强大的工具，使得开发者能够在几行代码中实现语音识别和语言模型的结合。

![](img/cdc0b2788f1c2c8dee35d04b4a191268.png)

## 使用基础模型构建自己的合成数据生成器。

`https://docs.covalent.xyz/docs/cloud/tutorials-cloud/data_generator/`

本文介绍了如何利用 Covalent Cloud 平台和基础模型（如 `Llama 3` 8 Billion）构建合成数据生成器。技术细节包括：选择 `A100` GPU 进行快速数据生成；使用`covalent_cloud`和`covalent` Python 库创建和管理计算环境；定义数据生成函数，利用`vllm`库与基础模型交互，生成符合特定任务和格式的数据批次；通过`@ct.electron`和`@ct.lattice`装饰器构建可扩展的数据生成工作流；以及通过`cc.dispatch`函数异步执行数据生成任务。此外，文章还展示了如何为不同的应用场景（如金融情感分析、文本到键值对转换、垃圾邮件检测）生成定制化的数据集，并提供了成本和时间效益的具体数据。

![](img/82cdffafa5780945ce9901f8247251d2.png)

## 分布式并行笔记（CP: Context Parallel）

`https://zhuanlan.zhihu.com/p/713516721?utm_psn=1809578499201187841`

文章深入探讨了分布式并行计算中的上下文并行（CP）技术，它通过优化 `attention` 机制，提高了性能和负载均衡。CP 技术利用最新的 OSS 和 `cuDNN` `flash attention kernels`，移除了因低三角形掩码带来的冗余计算。在 `Megatron` 框架中，CP 通过`get_batch_on_this_cp_rank`函数对输入数据进行分片，确保每个 GPU 处理不同的数据块，从而解决因因果掩码导致的计算不均衡。在`AttnFuncWithCP`中实现的前向传播，通过对 `q`,`k`,`v` 的重塑以及循环处理不同数据块，优化了 `attention` 计算。此外，通过丢弃低三角形中无意义计算，进一步提升了效率。CP 技术的并行实现包括了 `attention` 前向传播与通信 `isend`/`irecv` 的并行，以及与 `lse correction` 的并行，这些并行操作有助于提高整体计算效率。

![](img/168e64094b54f6dc100e943ea850cea4.png)

## 面向AI大模型开发和应用的大规模数据处理套件

`https://zhuanlan.zhihu.com/p/715651607?utm_psn=1809578649860575232`

文章介绍了一个针对 AI 大模型的大规模数据处理套件，旨在提高数据处理效率和模型开发质量。该套件支持多种数据类型，并能与现有 AI 平台集成，解决了数据清洗、标注、存储和安全等技术挑战。通过优化数据流程，它确保了大规模数据的高效处理，为 AI 模型的训练和应用提供了强有力的技术支撑。

![](img/3a7d0ec932cab48c81841057cb5781e8.png)

## (MoE) Part 2: Loading Balance & Gating

`https://zhuanlan.zhihu.com/p/715408974?utm_psn=1809554842273853440`

MoE通过引入多个专家网络和门控机制，提高了深度学习模型的灵活性和性能。负载均衡确保每个专家网络都能平均参与训练，避免资源浪费。门控机制则通过精细的路由决策，将输入样本有效地分配到最合适的专家网络，从而提升模型的泛化能力。在 MoE 中，硬门控（hard gating）和软门控（soft gating）是两种主要的路由策略，它们各自适用于不同的应用场景。硬门控通过选择单一专家简化决策过程，而软门控通过组合多个专家的输出，提供更加丰富的信息表示。此外，通过优化损失函数，例如加入负载均衡损失和门控损失，可以进一步提升模型在复杂任务中的表现

![](img/6f99dddd049ce21c7b3dd1ad4af09121.png)

# 🚀招聘公告

---

![](img/e59fad016dbcf13b42358ebb19520e2c.png)

奇绩创坛由陆奇博士于2019年创立，使命是在源头最大化驱动创新。奇绩研究组将研究模型理论、到投研战略分析、市场策划等不同背景的同学聚在一起，搭建GenAI时代独特的跨界人才社群。

### F24实习岗位

#### [`行研-学术科研人才交流组实习生`](https://miracleplus.feishu.cn/docx/Yn6rdf2iOoY2Rcx8yVNcUBYRnch#part-QkMcdeJmuob9uvxvV3qcLSzHn8g)

#### 行研-GenAI研究岗位-战略分析（偏产品）

#### Web / AI 全栈开发工程师实习生

### 火热开放中

#### 行研-GenAI研究岗位-前沿学术探索

#### 行研-GenAI研究岗位-战略分析（偏咨询）

#### 创新活动产品运营 （大模型系列活动产品运营）

> 点击相关岗位可查看对应岗位需求【JD】；相关岗位投递请添加🛰️💻【`sunjiajunspace`】

> ‼️欢迎推荐/自荐，期待在奇绩创坛遇见更好的你‼️

---

# 声明

本文档仅供学习交流使用，版权归原作者所有，若涉侵权，请联系Jack Jin 15101136166

（欢迎在下方反馈您宝贵的意见！整理不易，觉得有用给AI日报小组点个赞👍吧！）

ChatGPT；Angela；Zijia Liu；Alex；Zeqi Tan；Tiezhen（HuggingFace）；Naixu；Lizhi；Peter；Suoman；Jack
