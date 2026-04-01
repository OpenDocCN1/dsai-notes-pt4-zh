# GLM-4-9B 技术文档

> 来源：[https://v11enp9ok1h.feishu.cn/docx/X1twdwQ6BoFXNaxxBWbcfqXqnle](https://v11enp9ok1h.feishu.cn/docx/X1twdwQ6BoFXNaxxBWbcfqXqnle)

<https://v11enp9ok1h.feishu.cn/sync/VztedzIoUstVPDb6RNMctK8Unie>

+   作者：智谱团队

+   Github 官方仓库：https://github.com/THUDM/GLM-4

+   相关B站讲解：https://www.bilibili.com/video/BV1mw4m1Y7eZ/?spm_id_from=333.337.search-card.all.click

+   技术支持：请到 [github issue](https://github.com/THUDM/GLM-4/issues) 提问

# 模型介绍

GLM-4-9B 是智谱 AI 推出的最新一代预训练模型 GLM-4 系列中的开源版本。 在语义、数学、推理、代码和知识等多方面的数据集测评中，GLM-4-9B 及其人类偏好对齐的版本 GLM-4-9B-Chat 均表现出超越 Llama-3-8B 的卓越性能。除了能进行多轮对话，GLM-4-9B-Chat 还具备网页浏览、代码执行、自定义工具调用（Function Call）和长文本推理（支持最大 128K 上下文）等高级功能。

本代模型增加了多语言支持，支持包括日语，韩语，德语在内的26 种语言。我们还推出了支持 1M 上下文长度（约 200 万中文字符）的 GLM-4-9B-Chat-1M 模型和基于 GLM-4-9B 的多模态模型，以及在语言理解、指令遵循、长文本处理等方面性能得到全面提升的 GLM-4-Plus。GLM-4V-9B 具备 1120 * 1120 高分辨率下的中英双语多轮对话能力，在中英文综合能力、感知推理、文字识别、图表理解等多方面多模态评测中，GLM-4V-9B 表现出超越 GPT-4-turbo-2024-04-09、Gemini 1.0 Pro、Qwen-VL-Max 和 Claude 3 Opus 的卓越性能。GLM-4-Plus 使用了大量模型辅助构造高质量合成数据以提升模型性能；利用 PPO 有效有效提升模型推理（数学、代码算法题等）表现，更好反应人类偏好。

# 技术报告和论文

> 论文地址：

> [ChatGLM: A Family of Large Language](https://arxiv.org/abs/2406.12793) Models from GLM-130B to GLM-4 All Tools

> [C](https://arxiv.org/abs/2311.03079)ogVLM: Visual Expert for Pretrained Language Models

您可以引用以下论文：

```
@misc{glm2024chatglm,
      title={ChatGLM: A Family of Large Language Models from GLM-130B to GLM-4 All Tools}, 
      author={Team GLM and Aohan Zeng and Bin Xu and Bowen Wang and Chenhui Zhang and Da Yin and Diego Rojas and Guanyu Feng and Hanlin Zhao and Hanyu Lai and Hao Yu and Hongning Wang and Jiadai Sun and Jiajie Zhang and Jiale Cheng and Jiayi Gui and Jie Tang and Jing Zhang and Juanzi Li and Lei Zhao and Lindong Wu and Lucen Zhong and Mingdao Liu and Minlie Huang and Peng Zhang and Qinkai Zheng and Rui Lu and Shuaiqi Duan and Shudan Zhang and Shulin Cao and Shuxun Yang and Weng Lam Tam and Wenyi Zhao and Xiao Liu and Xiao Xia and Xiaohan Zhang and Xiaotao Gu and Xin Lv and Xinghan Liu and Xinyi Liu and Xinyue Yang and Xixuan Song and Xunkai Zhang and Yifan An and Yifan Xu and Yilin Niu and Yuantao Yang and Yueyan Li and Yushi Bai and Yuxiao Dong and Zehan Qi and Zhaoyu Wang and Zhen Yang and Zhengxiao Du and Zhenyu Hou and Zihan Wang},
      year={2024},
      eprint={2406.12793},
      archivePrefix={arXiv},
      primaryClass={id=&#x27;cs.CL&#x27; full_name=&#x27;Computation and Language&#x27; is_active=True alt_name=&#x27;cmp-lg&#x27; in_archive=&#x27;cs&#x27; is_general=False description=&#x27;Covers natural language processing. Roughly includes material in ACM Subject Class I.2.7. Note that work on artificial languages (programming languages, logics, formal systems) that does not explicitly address natural-language issues broadly construed (natural-language processing, computational linguistics, speech, text retrieval, etc.) is not appropriate for this area.&#x27;}
}
```

如果您使用了 GLM-4V-9B，您还需要引用

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

# 快速阅览

我们在文档不同的位置进行了详细的介绍，您可以点击进入并查看具体的技术文档。

###### [评测结果](https://zhipu-ai.feishu.cn/wiki/CZeCwAalviWXsNkxNLxc85ZBnzc)

+   GLM-4 开源模型模型跑分，评测数据

+   GLM-4 开源模型模型技术报告和论文

###### [快速使用](https://zhipu-ai.feishu.cn/wiki/PCDWwC2Ndiuj39kryJZcsTDbnlg)

+   环境配置，模型下载，基础的模型推理

+   LongWriter：10k+ 输出模型

###### [进阶之](https://zhipu-ai.feishu.cn/wiki/Sehlww04xizsGDkBKrGcoD4snKc)[240904](https://zhipu-ai.feishu.cn/wiki/R1QDwhLSOivOc2kS8micaHJ4nGb)[路](https://zhipu-ai.feishu.cn/wiki/Sehlww04xizsGDkBKrGcoD4snKc)

+   OpenAI API 类 模型服务构建

+   模型微调

+   设备加速推理

+   模型提示词和构建

###### [常见问题](https://zhipu-ai.feishu.cn/wiki/U3nywnDpniLO6LkH31IcVnKFnTg)

+   使用 GLM-4-9B 开源模型遇见的常见问题。

+   如何为项目做出贡献。

+   项目更新计划

###

# 📚 信息汇总

官方网站：https://www.zhipuai.cn/

智谱清言：https://chatglm.cn

API开放平台：https://bigmodel.cn/

Github仓库：https://github.com/THUDM/GLM-4

# 👍 GLM-4 体验：  https://bigmodel.cn/trialcenter

---