
# VLMEvalKit 评测InternVL2及Qwen2VL实践

> 来源：[https://aicarrier.feishu.cn/docx/XEvpdOn92oPhmVx3jP9cr6XQnqg](https://aicarrier.feishu.cn/docx/XEvpdOn92oPhmVx3jP9cr6XQnqg)

腾讯会议：640-900-694

时间：9 月 15 日 19:00

多模态兴趣小组加入门槛：输出一篇高质量 `InternVL2` 、`VLMEvalKit` 相关文章、完成第三期书生大模型实战营 `InternVL` 课程的任务，即可私聊浦语小助手（🛰️搜索：`InternLM`）即可加入多模态兴趣小组。

![](img/68702ff4606eca7f195681588672df77.png)

## 一、InternVL2简介

`InternVL2` 是由上海人工智能实验室 `OpenGVLab` 发布的一款多模态大模型，其中文名称为“书生·万象”。该模型在多学科问答（`MMMU`）任务上表现出色，成为国内首个在该任务上性能突破 60 的模型，被誉为开源多模态大模型性能的新标杆。

![](img/a3e4821c07675e79ead6a50c8b8f3f93.png)

### InternVL2主要功能

`InternVL 2.0` 具有各种指令调整模型，范围从 10 亿到 1080 亿个参数。与最先进的开源多模态大型语言模型相比，`InternVL 2.0` 超越了大多数开源模型。它在各种功能上展示了与专有商业模式相当的竞争性能，包括文档和图表理解、信息图表 `QA`、场景文本理解和 `OCR` 任务、科学和数学问题解决，以及文化理解和集成多模式功能。

![](img/fc247de59239784007812473156e8b76.png)

`InternVL 2.0` 使用 8k 上下文窗口进行训练，并利用由长文本、多张图像、医疗数据和视频组成的训练数据，与 `InternVL 1.5` 相比，它显著提高了处理这些类型输入的能力。

![](img/acb5d8de93b5af3f75e342d33378b0b7.png)

如图所示，`InternVL2` 采用与 `InternVL 1.5` 相同的架构，特别是各种现有研究中引用的 `ViT-MLP-LLM` 配置。

`InternVL2` 模型在多个基准测试中表现优异，例如在中文多模态大模型 `SuperCLUE-V` 基准测试中，`InternVL2-40B` 模型获得了 `70.59` 分，排名第二。这表明该模型在处理多模态数据和跨学科问题方面具有较高的能力。

其使用方法可以参考：[Welcome to InternVL’s tutorials! — internvl](https://internvl.readthedocs.io)

## 二、Qwen2VL简介

`Qwen2-VL` 是阿里巴巴达摩院开源的视觉多模态 `AI` 模型，具备高级图像和视频理解能力。`Qwen2-VL` 支持多种语言，能处理不同分辨率和长宽比的图片，实时分析动态视频内容。`Qwen2-VL` 在多语言文本理解、文档理解等任务上表现卓越，适用于多模态应用开发，推动了 `AI` 在视觉理解和内容生成领域的进步。

![](img/19568c192df6ac435861e36148eb9a66.png)

### Qwen2-VL主要增强功能

1. `SoTA` 理解各种分辨率和比例的图像：`Qwen2-VL` 在视觉理解基准上实现了先进的性能，包括 `MathVista`、`DocVQA`、`RealWorldQA`、`MTVQA` 等。

2. **理解 20 分钟以上的视频**：借助在线流媒体功能，`Qwen2-VL` 可以通过基于高质量视频的问答、对话、内容创作等来理解 20 分钟以上的视频。

3. **可以操作手机、机器人等的智能体**：`Qwen2-VL` 具有复杂的推理和决策能力，可以与手机、机器人等设备集成，基于视觉环境和文本指令进行自动操作。

4. **多语言支持**：为了服务全球用户，除了英语和中文之外，`Qwen2-VL` 现在支持图像内不同语言文本的理解，包括大多数欧洲语言、日语、韩语、阿拉伯语、越南语等。

通义千问团队以 Apache 2.0 协议开源了 `Qwen2-VL-2B` 和 `Qwen2-VL-7B`，并发布了 `Qwen2-VL-72B` 的 API。开源代码已集成到 `Hugging Face Transformers`、`vLLM` 和其他第三方框架中。

其使用方法可以参考：https://qwenlm.github.io/zh/blog/qwen2-vl/

## 三、VLMEvalKit简介

`VLMEvalKit` 是一个专为大型视觉语言模型评测设计的开源工具包。它支持在各种基准测试上对大型视觉语言模型进行一键评估，无需进行繁重的数据准备工作，使评估过程更加简便。`VLMEvalKit` 适用于图文多模态模型的评测，支持单对图文输入或是任意数量的图文交错输入。它通过实现 70 多个基准测试，覆盖了多种任务，包括但不限于图像描述、视觉问答、图像字幕生成等。

![](img/040211052a6e69b89878702f93cd80eb.png)

`VLMEvalKit` 的主要用途包括以下几个方面：

1. **多模态模型评估**：`VLMEvalKit` 旨在为研究人员和开发人员提供一个用户友好且全面的框架，以评估现有的多模态模型并发布可重复的评估结果。它支持多种多模态模型和评测集，包括闭源和开源模型，以及多个多模态基准数据集。

2. **模型性能比较**：通过 `VLMEvalKit`，用户可以轻松比较不同多模态模型在各种任务上的性能。该工具包提供了详细的评测结果，并支持将结果发布在 `OpenCompass` 的多模态整体榜单上。

3. **便捷的一站式评测**：`VLMEvalKit` 支持一键式评测，无需手动进行数据预处理。用户只需一条命令即可完成对多个多模态模型和评测集的评测。

4. **易于扩展**：`VLMEvalKit` 框架支持轻松添加新的多模态模型和评测集。用户可以根据需要添加新的模型或评测集，并确保原有的评测集和模型仍然适用于新的模型或评测集。

5. **支持多种模型和评测集**：`VLMEvalKit` 支持超过三十个开源多模态模型和十余个开源多模态评测集，包括 `GPT-4v`、`GeminiPro`、`QwenVLPlus` 等主流模型和 `MME`、`MMBench`、`SEEDBench` 等评测集。

6. **提供定量与定性结果**：`VLMEvalKit` 不仅提供定量评测结果，还支持对模型的定性分析，帮助用户了解模型在特定任务上的表现和不足之处。

## 四、使用 `VLMEvalKit` 评测 `InternVL2` 及 `Qwen2VL`

使用 `VLMEvalKit` 进行评测需要进行以下步骤：

### 环境准备

用以下命令安装依赖：

```
# 基于conda环境
conda create -n vlmeval python=3.10
conda activate vlmeval
conda install pytorch==2.0.1 torchvision==0.15.2 torchaudio==2.0.2 pytorch-cuda=11.7 -c pytorch -c nvidia -y

git clone https://github.com/open-compass/VLMEvalKit.git
cd VLMEvalKit
pip install -e .
```

> **注意：**
>
> 某些 VLM 可能无法在某些特定的 `transformers` 版本下运行，参考以下设置来评估对应的 VLM:
>
> + 请用 `transformers==4.33.0` 来运行: `Qwen series`, `Monkey series`, `InternLM-XComposer Series`, `mPLUG-Owl2`, `OpenFlamingo v2`, `IDEFICS series`, `VisualGLM`, `MMAlaya`, `ShareCaptioner`, `MiniGPT-4 series`, `InstructBLIP series`, `PandaGPT`, `VXVERSE`.
>
> + 请用 `transformers==4.37.0` 来运行: `LLaVA series`, `ShareGPT4V series`, `TransCore-M`, `LLaVA (XTuner)`, `CogVLM Series`, `EMU2 Series`, `Yi-VL Series`, `MiniCPM-[V1/V2]`, `OmniLMM-12B`, `DeepSeek-VL series`, `InternVL series`, `Cambrian Series`, `VILA Series`, `Llama-3-MixSenseV1_1`, `Parrot-7B`, `PLLaVA Series`.
>
> + 请用 `transformers==4.40.0` 来运行: `IDEFICS2`, `Bunny-Llama3`, `MiniCPM-Llama3-V2.5`, `360VL-70B`, `Phi-3-Vision`, `WeMM`.
>
> + 请用 `transformers==latest` 来运行: `LLaVA-Next series`, `PaliGemma-3B`, `Chame3leon series`, `Video-LLaVA-7B-HF`, `Ovis series`, `Mantis series`, `MiniCPM-V2.6`, `OmChat-v2.0-13B-sinlge-beta`, `Idefics-3`, `GLM-4v-9B`, `VideoChat2-HD`.

### 修改文件

在 `$VLMEvalKit/vlmeval/config.py` 文件中设置在 `VLMEvalKit` 中支持的 VLM 名称，以及模型路径。

如果你的电脑上面没有该模型的模型文件，则需要自己下载，然后更改模型路径，也可以不修改，在运行模型评测命令的时候会自动下载模型文件。

> 注：默认下载使用的是 `Huggingface`，需要进行🪜，也可以使用 `modelscope` 将模型下载到本地，然后更改路径。

![](img/b0cd4d018c1fcf002ff6cae31f24f222.png)

将所要评测的模型路径设置好以后就可以开始评测了。

### 模型评测

模型评测时可以使用 `python` 或 `torchrun` 来运行脚本，使用 `python` 运行时，只实例化一个 VLM，并且它可能使用多个 GPU。使用 `torchrun` 运行时，每个 GPU 上实例化一个 VLM 实例，这可以加快推理速度。

这里我们在 `MME` 上 进行推理和评估。

`MME` 涵盖了感知和认知能力的考察。除了 `OCR` 外，感知包括对粗粒度和细粒度对象的识别。前者识别对象的存在、数量、位置和颜色。后者识别电影海报、名人、场景、地标和艺术作品。认知包括常识推理、数值计算、文本翻译和代码推理。总共有 14 个子任务。

使用以下命令开始推理和评估：

```
torchrun --nproc-per-node=2 run.py --data MME --model qwen2vl con--verbose
```

```
python run.py --data MME --model Qwen2.5-VL-7B-Instruct --verbose --work-dir F:\lyf\results\vlmevalkit
```

> **参数解释：**
>
> + `--data` (list[str]): 设置在 `VLMEvalKit` 中支持的数据集名称（在 `vlmeval/utils/dataset_config.py` 中定义）
> + `--model` (list[str]): 设置在 `VLMEvalKit` 中支持的 VLM 名称（在 `vlmeval/config.py` 中的 `supported_VLM` 中定义）
> + `--mode` (str, 默认值为 'all', 可选值为 ['all', 'infer']): 当 `mode` 设置为 `"all"` 时，将执行推理和评估；当设置为 `"infer"` 时，只执行推理
> + `--nproc` (int, 默认值为 4): 调用 API 的线程数
> + `--work-dir` (str, default to '.'): 存放测试结果的目录
> + `--nframe` (int, default to 8): 从视频中采样的帧数，仅对视频多模态评测集适用
> + `--pack` (bool, store_true): 一个视频可能关联多个问题，如 `pack==True`，将会在一次询问中提问所有问题

命令运行以后大约需要半小时的时间完成评测，评估结果将作为日志打印出来。此外，结果文件也会在目录 `$YOUR_WORKING_DIRECTORY/{model_name}` 中生成。以 `.csv` 结尾的文件包含评估的指标。

`Qwen2VL` 的 `MME` 数据集的评估结果如下：

`InternVL2` 的 `MME` 数据集的评估结果如下：

### 4. 结果分析

通过对 `Qwen2-VL-2B-Instruct_MME_score.xlsx` 和 `InternVL2-2B_MME_score.xlsx` 两个表格中的大模型分数进行比较，我们可以观察到以下差异：

1. 整体分数对比：两个模型在各项能力上的平均分数存在细微差异，但整体趋势相似，均表现出较高的综合能力。两个模型在多项能力上均表现出较高的水平，但在具体任务上各有侧重。`Qwen2-VL-2B-Instruct` 在感知、名人识别和代码推理方面表现更佳，而 `InternVL2-2B` 则在 `OCR`、艺术作品识别和推理能力上有所领先。

2. 具体能力项差异：

   + `Perception`（感知能力）：`InternVL2-2B` 的分数（1452.86）略低于 `Qwen2-VL-2B-Instruct`（1467.74），表明在图像或文本等基础信息的理解能力上，后者略胜一筹。
   + `Reasoning`（推理能力）：`InternVL2-2B` 的分数（420.71）略高于 `Qwen2-VL-2B-Instruct`（411.79），这可能意味着在处理复杂逻辑和关系推理时，`InternVL2-2B` 更具优势。
   + `OCR`（光学字符识别）：`InternVL2-2B` 的分数（95）明显高于 `Qwen2-VL-2B-Instruct`（65），显示出更强的文本识别和提取能力。
   + `Artwork`（艺术作品识别）：`InternVL2-2B` 的分数（146.25）略高于 `Qwen2-VL-2B-Instruct`（142），表明在艺术作品的识别和理解上，前者稍有优势。
   + `Celebrity`（名人识别）：`Qwen2-VL-2B-Instruct` 的分数（140.59）高于 `InternVL2-2B`（118.53），反映出在名人识别能力上，后者稍显不足。
   + `Code Reasoning`（代码推理）：`Qwen2-VL-2B-Instruct` 的分数（92.5）略高于 `InternVL2-2B`（87.5），显示出在处理代码和逻辑推断上的微弱优势。
   + 其他能力项：如 `Commonsense Reasoning`（常识推理）、`Count`（计数）、`Existence`（存在性判断）、`Landmark`（地标识别）、`Numerical Calculation`（数值计算）、`Position`（位置识别）、`Posters`（海报识别）、`Scene`（场景识别）和 `Text Translation`（文本翻译）等，两个模型各有优势，但差异不大。

## 结语

多模态技术的突破，正在改变我们理解和交互世界的方式。无论是强大的感知能力、复杂的推理分析，还是图文融合的创新应用，`InternVL2` 与 `Qwen2-VL` 展现了大模型的无限可能。如果你也对前沿多模态技术充满兴趣，欢迎加入我们的多模态兴趣小组，与志同道合的伙伴共同探索未来的 AI 世界！

更多详细内容请看：

`VLMEvalKit`：用于评估大型视觉语言模型的 🏆 工具包：https://github.com/open-compass/VLMEvalKit

`InternVL` 系列模型使用教程：https://internvl.readthedocs.io

`Qwen2VL` 系列模型使用教程：https://qwenlm.github.io/zh/blog/qwen2-vl/

如果你想查看更多的 VLM 评估结果，可以到 `OpenVLM` 排行榜进行查看：https://huggingface.co/spaces/opencompass/open_vlm_leaderboard
