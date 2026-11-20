
# 开源实习-昇思MindSpore开源实习书生专辑

> 来源：`https://aicarrier.feishu.cn/docx/JSqDdJDX0oz4bWxEkePcVgqtn9b`

链接：`https://gitee.com/mindspore/community/issues`

## 开源实习计划介绍

昇思`MindSpore`开源实习介绍：`https://www.mindspore.cn/internship`

![](img/4340854556e667d9ea8a43638fab19a4.png)

## 本批任务方向介绍

### 任务目标：

1. 扩充`MindSpore`的三方生态，包含书生工具链，目前`OpenCompass`已验证，`LMDeploy`等正在验证中；

1. 扩充`MindSpore`模型库，包含`InternLM`，`InternVL`和`Intern-S1`；

### 任务前置知识：

1. `vLLM-MindSpore`：`https://gitee.com/mindspore/vllm-mindspore`

1. `vLLM-MindSpore`模型列表（可参考）：`https://www.mindspore.cn/vllm_mindspore/docs/zh-CN/master/user_guide/supported_models/models_list/models_list.html`

1. `MindSpore Transformers`大模型使能教程：`https://docs.qq.com/aio/DZG5FZGZkeFVBQnVH`

1. `MindSpore Transformers`模型列表（可参考）：`https://gitee.com/mindspore/mindformers`

## 任务清单（正在发布中）

| 活动类型  选填 | 领域  必填 | 任务类型  必填 | 任务名称  必填 | 任务描述 |  |  |  |  | 任务难度  （低、中、高）  必填 | 任务技术要求  必填 | 任务成果仓库  必填 | 分数 | 任务链接（更新中） |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|  |  |  |  | 背景描述  选填 | 需求描述  必填 | 数据集  选填 | 参考资料  选填 | 验收标准  必填 |  |  |  |  |  |
| 开源实习 | `vllm-mindspore` | 模型迁移 | 联手书生社区，打造`vllm-mindspore`和`OpenCompass`的对接，输出样例和文档。 | `OpenCompass` 是面向大模型评测的一站式平台。由开源可复现的评测框架 `CompassKit` 、共建共享的评测基准社区 `CompassHub`、官方搭建的榜单和精选的垂直领域榜单 `CompassRank` 组成，被`Mete`，`HF`等多家伙伴收录推荐；`vLLM` `Mindspore`插件（`vllm-mindspore`）是一个由`MindSpore`社区孵化的`vLLM`后端插件。其将基于`MindSpore`构建的大模型推理能力接入`vLLM`，从而有机整合`MindSpore`和`vLLM`的技术优势，提供全栈开源、高性能、易用的大模型推理解决方案。 | 1.打通`vllm-mindspore`和`OpenCompass`部署安装和评测使用，形成完善的样例，文档等（包含代码，环境等），并纳入`OpenCompass`  2.基于`vllm-mindspore`实现`Qwen`、`InternLM`模型推理服务化在`OpenCompass`上进行`GSM8k`数据集评测，在精度误差内，推理时间优于`Huggingface`标准，形成完善的样例，文档等（包含代码，环境等）；  3.合入`https://gitee.com/mindspore/vllm-mindsporer`分支  2.提交代码到`OpenCompass`代码仓上或者文档上； | 不限 |  | 1.提交应用的代码至 `https://gitee.com/mindspore/vllm-mindsporer`分支  2.提交代码到`OpenCompass`代码仓上或者文档上； | 中 | 硬件：`Atlas 800I A2`推理服务器，或`Atlas 800T A2`推理服务器，已安装必要的驱动程序，并可连接至互联网  操作系统：`openEuler`或`Ubuntu Linux`  软件：  `Python` >= 3.9, < 3.12  `CANN` >= 8.0.0.beta1  `MindSpore`:2.7.0  `vLLM`:0.9.1 | `vllm-mindspore` master 分支：`https://gitee.com/mindspore/vllm-mindspore`； | 50 |  |
| 开源实习 | `MindSpore Transformers` | 模型迁移 | 联手书生社区，基于`MindSpore Transformers`套件迁移`InternLM2.5`模型到`MindSpore Transformers`仓库 | `InternLM2.5`由上海AI实验室等单位推出。作为书生·浦语系列模型的最新版本，书生·浦语2.5具备1M词元（`Token`）超长文本窗口及开源模型中领先的推理能力，并支持自主规划和在线信息整合，成为助力复杂问题高效解决的得力`AI`助手。 | 1.迁移`InternLM2.5`到`MindSpore Transformers`上， 实现微调，推理流程，脚本与`mindformers`仓库的`Qwen3`一致； | 不限 |  | 1.提交应用的代码至 `https://gitee.cn/mindspore/mindformers` master  2.提交代码到`InternLM`代码仓上或者文档上；  3.提供1个推理和微调案例； | 高 | 硬件：`Atlas 800I A2`推理服务器，或`Atlas 800T A2`推理服务器，已安装必要的驱动程序，并可连接至互联网  操作系统：`openEuler`或`Ubuntu Linux`  软件：  `Python` >= 3.11.4  `CANN` >= 8.2.RC1  `MindSpore`:2.7.0  `MindSpore Transformers`=1.6.0 | `MindSpore Transformers` (master)：`https://gitee.com/mindspore/mindformers` | 50 |  |
| 开源实习 | `MindSpore Transformers` | 模型迁移 | 联手书生社区，基于`MindSpore Transformers`套件迁移`InternLM3`模型到`MindSpore Transformers`仓库 | `InternLM3`，即书生·浦语大模型第3代，开源了80亿参数，面向通用使用与高阶推理的指令模型（`InternLM3-8B-Instruct`）。模型具备以下特点：  更低的代价取得更高的性能: 在推理、知识类任务上取得同量级最优性能，超过`Llama3.1-8B`和`Qwen2.5-7B`。值得关注的是`InternLM3`只用了4万亿词元进行训练，对比同级别模型训练成本节省75%以上。  深度思考能力: `InternLM3`支持通过长思维链求解复杂推理任务的深度思考模式，同时还兼顾了用户体验更流畅的通用回复模式。 | 1.迁移`InternLM3`到`MindSpore Transformers`上， 实现微调，推理流程，脚本与`mindformers`仓库的`Qwen3`一致； | 不限 |  | 1.提交应用的代码至 `https://gitee.cn/mindspore/mindformers` master  2.提交代码到`InternLM`代码仓上或者文档上；  3.提供1个推理和微调案例； | 高 | 硬件：`Atlas 800I A2`推理服务器，或`Atlas 800T A2`推理服务器，已安装必要的驱动程序，并可连接至互联网  操作系统：`openEuler`或`Ubuntu Linux`  软件：  `Python` >= 3.11.4  `CANN` >= 8.2.RC1  `MindSpore`:2.7.0  `MindSpore Transformers`=1.6.0 | `MindSpore Transformers` (master)：`https://gitee.com/mindspore/mindformers` | 50 |  |
| 刷新中 |  |  | `InternVL2.5` |  |  |  |  |  |  |  |  |  |  |
| 刷新中 |  |  | `InternVL3` |  |  |  |  |  |  |  |  |  |  |
| 刷新中 |  |  | `InternVL3.5` |  |  |  |  |  |  |  |  |  |  |
| 刷新中 |  |  | `Intern-SI` |  |  |  |  |  |  |  |  |  |  |
| 刷新中 |  |  | `Intern-SI-mini` |  |  |  |  |  |  |  |  |  |  |
