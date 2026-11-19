
# AI行业资讯5.19

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/XDp2doWGnoBJMJxg8EJc1OXonff](https://qqs7y1hozd1.feishu.cn/docx/XDp2doWGnoBJMJxg8EJc1OXonff)

## DeepSeek公开`V3`大模型降本方法

5月15日，DeepSeek团队发布新论文，详细介绍了`DeepSeek-V3`在训练和推理过程中解决硬件瓶颈的方法。论文揭示了四项核心创新技术，使`DeepSeek-V3`能够在仅使用2048块`H800` GPU的情况下，达到与数万块GPU相当的训练效果。

### 四项创新技术

+   内存优化：采用多头潜在注意力（`MLA`）技术，减少键值缓存（`KV Cache`）的内存占用。相比传统方法，`DeepSeek-V3`的`KV Cache`大小每token仅需70 KB，是传统方法的1/7到1/4，大幅降低显存压力。

+   计算优化：结合混合专家模型（`MoE`）和`FP8`低精度训练。`MoE`通过将模型参数分成多个专家，每次只激活部分专家处理输入，显著减少实际计算量。`FP8`低精度训练通过精细量化保持精度，训练成本降低50%，精度损失小于0.25%。

+   通信优化：采用多层胖树网络（`Multi-Plane Fat-Tree`），将集群网络分为多个平面，避免不同任务的流量冲突。相比传统三层网络，两层结构成本降低40%，延迟减少30%，支持上万GPU扩展。

+   推理加速：采用多token预测（`MTP`）方法，通过轻量级子模型并行预测多个候选token，验证后选择最优结果。生成速度提升1.8倍，每秒生成token数从10个增至18个，同时保持准确率在80%-90%。

### 未来展望

论文还对未来AI硬件的发展提出了五大维度的展望，包括低精度计算支持、扩展与扩展融合、网络拓扑优化、内存系统优化和鲁棒性与容错。这些展望旨在从“被动适配”逐步过渡到“主动设计”，以更好地支持大模型训练和推理。

论文地址：`https://arxiv.org/pdf/2505.09343`

![](img/1193e5d9bda7b92eedec3d264b8ab70f.png)

## OpenAI发布最强编程智能体`Codex`，集成至`ChatGPT`

5月17日，OpenAI正式发布基于云端运行的软件工程智能体`Codex`，集成至`ChatGPT`。`Codex`能够并行处理多个开发任务，协助开发者高效完成编程工作。它提供简洁的`UI`设计，包括一个输入框和两个按钮——“`Ask`”（提问）和“`Code`”（编码）。用户只需明确描述任务，`Codex`即可开始执行。

`Codex`的核心模型为`codex-1`，是OpenAI `o3`系列中专为软件工程微调的版本。基准测试显示，`codex-1`在`SWE-Bench`的得分为72.1%，优于`Claude 3.7`和`o3-high`。`Codex`通过在真实开发环境中进行强化学习训练，生成符合人类编码风格的代码，并严格遵循指令执行。

`Codex`能够理解代码结构、回答代码库相关问题、修复bug，甚至直接提交可审查的拉取请求。每个任务都在云端独立的沙箱环境中执行，自动加载用户代码仓库，运行时长从1分钟到30分钟不等，用户可实时查看进度。任务完成后，`Codex`会输出完整的执行结果，包括终端日志和测试记录。

安全性方面，`Codex`仅访问用户提供的代码库和预设依赖，不能联网或访问外部`API`。它经过专门训练，能够识别并拒绝与恶意开发相关的请求，避免被用于开发恶意软件。

目前，`Codex`已向`ChatGPT` Pro、Enterprise和Team用户开放，未来几周内将支持Plus和Edu用户。OpenAI还推出了专为命令行优化的轻量模型`codex-mini-latest`，支持开发者在本地环境快速接入AI。`Codex CLI`登录流程简洁，可直接使用`ChatGPT`账号接入，并享受基础的免费`API`配额。

OpenAI对`Codex`的长线愿景是将其打造成未来协作模式的雏形，内嵌进`IDE`、`Git`工具甚至`Slack`中，成为开发者真正的“副驾驶”。目前`Codex`仍处于研究预览阶段，图像输入等高级功能尚未开放，但其已将AI编程助手的畅想变为可用工具，真正融入实际工作流。

![](img/d5fc6b4901778ef3ba8dbc9971c4a002.png)

## GPT-4.1正式上线`ChatGPT`

5月17日，OpenAI宣布`GPT-4.1`模型正式上线`ChatGPT`。`GPT-4.1`系列包含三个版本：`GPT-4.1`、`GPT-4.1 mini`和`GPT-4.1 nano`，均支持高达100万token的上下文窗口，相比`GPT-4o`的12.8万token实现了大幅提升。

`GPT-4.1`系列在代码生成、指令遵循和长文本理解等方面表现突出，尤其在`SWE-bench Verified`基准测试中，`GPT-4.1`得分54.6%，较`GPT-4o`提升21.4%，较`GPT-4.5`提升26.6%。

此外，`GPT-4.1 mini`已替代原有的`GPT-4o mini`，向所有用户全面开放。

![](img/968c2d34b5c8e5985ba7ae08b075b41d.png)

## MiniMax发布`MiniMax Speech 02`

5月15日，MiniMax稀宇科技宣布其基于`AR Transformer`模型研发的高质量`TTS`系统「`MiniMax Speech 02`」正式发布。

该系统具有强大的泛化能力，能够轻松驾驭32种语言、不同口音和不同情绪的人声。在性能表现上，`MiniMax Speech 02`在两项全球权威语音基准测评榜单（`Artificial Analysis Speech Arena`和`Hugging Face TTS Arena`）中，超越了OpenAI、`ElevenLabs`等全球性能优异的模型，双双位列第一。

根据`Arena`榜单的`ELO`评分结果，`MiniMax Speech 02`在用户体验上表现更加优异，听感更加自然。同时，该模型系统在成本上也实现了突破，价格仅为`ElevenLabs Flash V2.5`的一半和`Multilingual V2`的四分之一。

目前，`MiniMax Speech 02`已上架`MiniMax Audio`，并公布了技术细节、实验对比数据、开源的多语言测试集以及技术报告。

![](img/0b4584f9edcc1f2a90dd98628da5addc.png)
