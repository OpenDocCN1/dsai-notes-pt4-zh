# 1.2 模型 · CogVLM 系列开源模型

> 来源：[https://ai.feishu.cn/docx/CTvwdNOxwoBX8PxEU47cbf6JnVh](https://ai.feishu.cn/docx/CTvwdNOxwoBX8PxEU47cbf6JnVh)

### 简介

CogVLM 视觉语言模型目前迭代至CogVLM2版本，以不牺牲 NLP 任务推理性能前提决定采用可训练的视觉专家添加到语言模型中，完美的解决了从NLP任务基座模型开始训练挑战性难题。我们 CogVLM V1版本模型深度集成了视觉和语言特征，同时保留了预训练大型语言模型的全部功能，在CogVLM的能力之上，我们探索了一个拥有了GUI图像Agent的模型CogAgent，开源版本的CogAgent-18B 在9个经典跨模态基准测试中实现了最先进的通用性能。CogVLM-17B 由 Vicuna-7B 训练而来，在 17 个经典的跨模态基准测试中取得了最先进的成绩，新一代的 CogVLM2 系列模型并开源了两款基于 Meta-Llama-3-8B-Instruct 开源模型，当前版本模型在TextVQA, DocVQA任务相比之前有所提升，支持 8K 文本长度，支持高达 1344 * 1344 的图像分辨率，中英文双语的开源模型版本。

### 技术报告和论文

> 论文地址：

> [C](https://arxiv.org/abs/2311.03079)ogVLM: Visual Expert for Pretrained Language Models

> [CogAgent: A Visual Language ](https://arxiv.org/abs/2312.08914)Model for GUI Agents

您可以引用以下论文：

```
@misc{wang2023cogvlm,
      title={CogVLM: Visual Expert for Pretrained Language Models}, 
      author={Weihan Wang and Qingsong Lv and Wenmeng Yu and Wenyi Hong and Ji Qi and Yan Wang and Junhui Ji and Zhuoyi Yang and Lei Zhao and Xixuan Song and Jiazheng Xu and Bin Xu and Juanzi Li and Yuxiao Dong and Ming Ding and Jie Tang},
      year={2023},
      eprint={2311.03079},
      archivePrefix={arXiv},
      primaryClass={cs.CV}
}
```

```

@misc{hong2023cogagent,
      title={CogAgent: A Visual Language Model for GUI Agents}, 
      author={Wenyi Hong and Weihan Wang and Qingsong Lv and Jiazheng Xu and Wenmeng Yu and Junhui Ji and Yan Wang and Zihan Wang and Yuxiao Dong and Ming Ding and Jie Tang},
      year={2023},
      eprint={2312.08914},
      archivePrefix={arXiv},
      primaryClass={cs.CV}
}
```

# 快速阅读

<https://zhipu-ai.feishu.cn/sync/I447dldoQsFFP6bcGUYcAaOVnQc>