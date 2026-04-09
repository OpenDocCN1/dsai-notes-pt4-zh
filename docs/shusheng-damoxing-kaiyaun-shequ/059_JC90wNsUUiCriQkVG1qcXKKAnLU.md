# 开源实习-昇思MindSpore开源实习书生专辑

> 来源：[https://aicarrier.feishu.cn/docx/JSqDdJDX0oz4bWxEkePcVgqtn9b](https://aicarrier.feishu.cn/docx/JSqDdJDX0oz4bWxEkePcVgqtn9b)

链接：https://gitee.com/mindspore/community/issues

# 开源实习计划介绍

昇思MindSpore开源实习介绍：https://www.mindspore.cn/internship

![](img/4340854556e667d9ea8a43638fab19a4.png)

# 本批任务方向介绍

任务目标：

1.  扩充MindSpore的三方生态，包含书生工具链，目前OpenCompass已验证，LMDeploy等正在验证中；

1.  扩充MindSpore模型库，包含InternLM，InternVL和Intern-S1；

任务前置知识：

1.  vLLM-MindSpore：https://gitee.com/mindspore/vllm-mindspore

1.  vLLM-MindSpore模型列表（可参考）：https://www.mindspore.cn/vllm_mindspore/docs/zh-CN/master/user_guide/supported_models/models_list/models_list.html

1.  MindSpore Transformers大模型使能教程：https://docs.qq.com/aio/DZG5FZGZkeFVBQnVH

1.  MindSpore Transformers模型列表（可参考）：https://gitee.com/mindspore/mindformers

# 任务清单（正在发布中）

| 活动类型  选填 | 领域  必填 | 任务类型  必填 | 任务名称  必填 | 任务描述 |  |  |  |  | 任务难度  （低、中、高）  必填 | 任务技术要求  必填 | 任务成果仓库  必填 | 分数 | 任务链接（更新中） |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|  |  |  |  | 背景描述  选填 | 需求描述  必填 | 数据集  选填 | 参考资料  选填 | 验收标准  必填 |  |  |  |  |  |
| 开源实习 | vllm-mindspore | 模型迁移 | 联手书生社区，打造vllm-mindspore和OpenCompass的对接，输出样例和文档。 | OpenCompass 是面向大模型评测的一站式平台。由开源可复现的评测框架 CompassKit 、共建共享的评测基准社区 CompassHub、官方搭建的榜单和精选的垂直领域榜单 CompassRank 组成，被Mete，HF等多家伙伴收录推荐；vLLM Mindspore插件（vllm-mindspore）是一个由MindSpore社区孵化的vLLM后端插件。其将基于MindSpore构建的大模型推理能力接入vLLM，从而有机整合MindSpore和vLLM的技术优势，提供全栈开源、高性能、易用的大模型推理解决方案。 | 1.打通vllm-mindspore和OpenCompass部署安装和评测使用，形成完善的样例，文档等（包含代码，环境等），并纳入OpenCompass  2.基于vllm-mindspore实现Qwen、InternLM模型推理服务化在OpenCompass上进行GSM8k数据集评测，在精度误差内，推理时间优于Huggingface标准，形成完善的样例，文档等（包含代码，环境等）；  3.合入https://gitee.com/mindspore/vllm-mindspore； | 不限 |  | 1.提交应用的代码至 https://gitee.com/mindspore/vllm-mindsporer分支  2.提交代码到OpenCompass代码仓上或者文档上； | 中 | 硬件：Atlas 800I A2推理服务器，或Atlas 800T A2推理服务器，已安装必要的驱动程序，并可连接至互联网  操作系统：openEuler或Ubuntu Linux  软件：  Python &gt;= 3.9, &lt; 3.12  CANN &gt;= 8.0.0.beta1  MindSpore:2.7.0  vLLM:0.9.1 | vllm-mindspore master 分支：https://gitee.com/mindspore/vllm-mindspore； | 50 |  |
| 开源实习 | MindSpore Transformers | 模型迁移 | 联手书生社区，基于MindSpore Transformers套件迁移InternLM2.5模型到MindSpore Transformers仓库 | InternLM2.5由上海AI实验室等单位推出。作为书生·浦语系列模型的最新版本，书生·浦语2.5具备1M词元（Token）超长文本窗口及开源模型中领先的推理能力，并支持自主规划和在线信息整合，成为助力复杂问题高效解决的得力AI助手。 | 1.迁移InternLM2.5到MindSpore Transformers上， 实现微调，推理流程，脚本与mindformers仓库的Qwen3一致； | 不限 |  | 1.提交应用的代码至 https://gitee.cn/mindspore/mindformers master  2.提交代码到InternLM代码仓上或者文档上；  3.提供1个推理和微调案例； | 高 | 硬件：Atlas 800I A2推理服务器，或Atlas 800T A2推理服务器，已安装必要的驱动程序，并可连接至互联网  操作系统：openEuler或Ubuntu Linux  软件：  Python &gt;= 3.11.4  CANN &gt;= 8.2.RC1  MindSpore:2.7.0  MindSpore Transformers=1.6.0 | MindSpore Transformers (master)：https://gitee.com/mindspore/mindformers | 50 |  |
| 开源实习 | MindSpore Transformers | 模型迁移 | 联手书生社区，基于MindSpore Transformers套件迁移InternLM3模型到MindSpore Transformers仓库 | InternLM3，即书生·浦语大模型第3代，开源了80亿参数，面向通用使用与高阶推理的指令模型（InternLM3-8B-Instruct）。模型具备以下特点：  更低的代价取得更高的性能: 在推理、知识类任务上取得同量级最优性能，超过Llama3.1-8B和Qwen2.5-7B。值得关注的是InternLM3只用了4万亿词元进行训练，对比同级别模型训练成本节省75%以上。  深度思考能力: InternLM3支持通过长思维链求解复杂推理任务的深度思考模式，同时还兼顾了用户体验更流畅的通用回复模式。 | 1.迁移InternLM3到MindSpore Transformers上， 实现微调，推理流程，脚本与mindformers仓库的Qwen3一致； | 不限 |  | 1.提交应用的代码至 https://gitee.cn/mindspore/mindformers master  2.提交代码到InternLM代码仓上或者文档上；  3.提供1个推理和微调案例； | 高 | 硬件：Atlas 800I A2推理服务器，或Atlas 800T A2推理服务器，已安装必要的驱动程序，并可连接至互联网  操作系统：openEuler或Ubuntu Linux  软件：  Python &gt;= 3.11.4  CANN &gt;= 8.2.RC1  MindSpore:2.7.0  MindSpore Transformers=1.6.0 | MindSpore Transformers (master)：https://gitee.com/mindspore/mindformers | 50 |  |
| 刷新中 |  |  | InternVL2.5 |  |  |  |  |  |  |  |  |  |  |
| 刷新中 |  |  | InternVL3 |  |  |  |  |  |  |  |  |  |  |
| 刷新中 |  |  | InternVL3.5 |  |  |  |  |  |  |  |  |  |  |
| 刷新中 |  |  | Intern-SI |  |  |  |  |  |  |  |  |  |  |
| 刷新中 |  |  | Intern-SI-mini |  |  |  |  |  |  |  |  |  |  |