
# 231210 - 1204｜Google 发布 Gemini；H100 最强竞品 AMD 发布新产品；微软 Copilot 再进化；全球首个开源多模态医疗基础模型；Meta 推出 AI 图像生成器｜LLM 内部可视化；VSCode 中使用端侧 Copilot，使用 DeepSeek 模型

> 来源：[https://leonzhang.feishu.cn/docx/FT1KdErYSo9HZpxp32Icw507njL](https://leonzhang.feishu.cn/docx/FT1KdErYSo9HZpxp32Icw507njL)

![](img/4b57e996ea1f5416bfe1728da16e8b41.png)

```
an illustration of a christmas tree with computers and stacked folders under it --ar 16:9
```

## 上周二三事

### GPT Store 上线时间推迟至明年

OpenAI 宣布将推迟 GPT 商店的推迟至明年，邮件中表示“意外的事情让我们很忙”。原计划在 12 月推出，比先前承诺的时间晚。OpenA! 将向 GPT 构建者发送更新，强调 ChatGPT 的一些改进，并宣布对定制 GPT 平台进行了功能改进。GPT 商店将成为 GPT 构建者的市场，用户可销售和共享其构建的 GPT。

![](img/f301385653a1f7139a3e638652a82215.png)

### [让谷歌再次伟大？Gemini 超越 GPT](https://mp.weixin.qq.com/s/WnbouvQ16i5Pq-JdLAWx8A)-4 问鼎最强大模型，Pro 版今日可用

谷歌发布 Gemini 大模型，超越 GPT-4，但技术报告引发质疑。Gemini Pro 版本已上线，对比 GPT-4 显示 Gemini 在编程方面表现出色，但在图形推理上存在改进空间。Gemini 支持多模态能力，对生活场景的识别较好。专家建议放弃 ChatGPT 的免费版，Gemini 在 AlphaGo 能力升级等方面仍待关注。技术报告未透露详细参数和训练数据。

![](img/6bd38d8683a672c383e95dd76de4fde4.png)

### [H100 最强竞品正式发布！推理快 1.6](https://mp.weixin.qq.com/s/KRt-oGRRjY9AxLvzKZYnTQ) 倍，内存高达 192GB，来自 AMD

AMD 推出 Instinct MI300X GPU，直接对标英伟达 H100。拥有史上最大芯片、1530 亿晶体管，AI 推理性能超英伟达 H100 1.6 倍，内存 192GB。

MI300A 是全球首个数据中心 APU，CPU 和 GPU 融合，内含 24 线程 CPU 核心、228 个 CDNA.3 计算单元，用于生成式 Al 领域。2024 年 Al 芯片市场将竞争激烈，苏妈预测 2027 年总市场达 4000 亿美元。

+   [AM](https://www.tomshardware.com/pc-components/cpus/amd-unveils-instinct-mi300x-gpu-and-mi300a-apu-claims-up-to-16x-lead-over-nvidias-competing-gpus)D unveils Instinct MI300X GPU and MI300A APU, claims up to 1.6X lead over Nvidia’s competing GPUs

![](img/a120583375eda7a1ac8e190ad9e9008c.png)

### [微软 Copilot 进化完全体，代码解释器、DA](https://mp.weixin.qq.com/s/3LJCrIi8PZ_JJL2LuxwPRg)LL·E 3，ChatGPT 有的它都有

微软 Copilot 迎来重磅更新，支持 128k 上下文，集成 GPT-4 Turbo 模型，新增代码解释器、升级 DALLE 3 绘图功能。搜索方面，结合多模态实现更准确回答，深度搜索消除歧义。Edge 浏览器专属功能包括对 YouTube 视频理解和文本生成的改进。更新标志着 Copilot 结束预览阶段，正式版本免费推出。

+   https://www.theverge.com/2023/12/5/23989052/microsoft-copilot-gpt-4-turbo-openai-models-code-interpreter-feature

+   https://blogs.microsoft.com/blog/2023/12/05/celebrating-the-first-year-of-copilot-with-significant-new-innovations/

![](img/92de0f3c15b4592a7d3e0b43d83c6f8f.png)

### [2023 年度 AI 设计实践报告： ](https://www.geekpark.net/news/328586)AI 将在营销领域大放异彩 | 极客公园

12 月 5 日，《2023 年度 Al 设计实践报告》发布，调查 5034 人，包括专业设计师、新媒体运营等。报告指出 Al 设计工具个人普及速度超预期，但 30%未使用，企业仅 8.4%采用。AI 在平面设计率先落地，国产 AI 发展潜力大。调查发现三大问题：控制不精准、生成有瑕疵、版权有争议。商业案例包括珠宝设计、音乐专辑、宣发营销。

报告预测 2024 年 AI 视频生成爆发，国产工具或超越海外。AI 改变设计能力结构，未来设计师需提升美学、创意能力，跨界思考，将经验沉淀为模型。

![](img/aaeb7fb94aebd337d8b6f0f9ae3be525.png)

### [全球首个开源多模态医疗基础模型：人工打分平均超越 ](https://mp.weixin.qq.com/s/a_yorRIxq9gUQbtzIk_Zjg)GPT-4V、支持 2D/3D 放射影像

上海交大与上海 Al Lab 发布了 Radiology Foundation Model （RadFM），这是一个开源的 14B 多模态医疗基础模型，首次支持 2D/3D 放射影像输入。研究聚焦于构建放射学领域的医学通用基础模型，提供了全球最大规模的医疗多模态数据集 MedMD&RadMD，以及开源的 RadFM 模型，支持 2D/3D、图像/文本混合输入。

+   论文链接：https://arxiv.org/abs/2308.02463

+   代码链接：https://github.com/chaoyi-wu/RadFM

+   PMC-Inline Dataset:  https://huggingface.co/datasets/chaoyi-wu/PMC-Inline

+   PMC-Casereport Dataset:  https://huggingface.co/datasets/chaoyi-wu/PMC-CaseReport

+   Huggingface Model: https://huggingface.co/chaoyi-wu/RadFM

![](img/e201d1f1f6d349d8009fca1830b9dbfa.png)

### [Meta 推出独立的 AI 图像生成器，目](https://www.ithome.com/0/737/571.htm)前免费但只支持英文提示词 - IT 之家

Meta 公司发布全新 AI 图像生成器 Imagine with Meta，允许用户通过自然语言描述创建图像。该生成器采用 Meta 的 Emu 图像生成模型，支持高分辨率图像。为解决种族偏见问题，Meta 将在生成图像中添加由 AI 生成的隐形水印，并由相应模型检测，提高内容透明度。之前，Meta 推出基于 AI 的图像编辑工具 Emu Edit 和 Emu Video。

Emu Edit 使用庞大的合成数据集进行训练，提供更精准的图像编辑能力。

![](img/1719e4fa088627418d119d2f30ff2f90.png)

## 📱 工具推荐

### LLM 可视化：令人惊叹的动画交互式教程，了解 GPT 内部结构

+   https://bbycroft.net/llm

![](img/87df72c5471f307f8ccd743f5db29f30.png)

### Excalidraw 把他们的文本到图表的功能开源了

[@Excalidraw](https://twitter.com/Excalidraw) 是一个虚拟白板应用，专门用于绘制类似手绘的图表。它提供了一个无限的、基于画布的白板，具有手绘风格，支持多种功能。新更新允许用户输入文本描述，将其自动转换为相应的图表或图形。

+   GitHub: [https://github.com/e](https://t.co/V2XJ8XHOdy)[x](https://t.co/V2XJ8XHOdy)calidraw/excalidraw…

+   在线体验：https://excalidraw.com

![](img/c39d8936c8ac736fd9f4a68fd35c4f7f.png)

### [免费并且完全私密地在 VSCode 中使用 Co](https://twitter.com/dani_avila7/status/1731344666824523882?s=20)pilot，使用 DeepSeek 模型

1.  下载 VSCode 的 CodeGPT 扩展：https://marketplace.visualstudio.com/items?itemName=DanielSanMedium.dscodegpt&ssr=false#overview

1.  下载@Ollama_ai: https://ollama.ai

1.  安装完成后，在终端运行以下命令：`ollama run deepseek-coder`

1.  在 VSCode 的 CodeGPT 设置中，选择 Ollama 作为提供者，模型选择 `deepseek-coder`。

1.  完成设置！

## 💰 上周融资

### [悠桦林完成近亿元 C+轮融资，巩固](https://www.36kr.com/p/2544467896395524) AI 智能决策行业领先地位 -36 氪

中国 AI 智能决策技术领军企业—-悠桦林完成近亿元人民币的 C+轮融资，由上汽恒旭领投。新资金将加强在运筹优化领域的技术实力，招募顶级人才，深耕智能供应链计划与排程优化平台，提升技术领先优势，加速商业化步伐。悠桦林通过海量数据驱动 AI 智能决策，以一体化智能供应链计划与排程优化平台解决制造业难题，实现显著业务增长
