# LongWriter 技术文档

> 来源：[https://ai.feishu.cn/docx/IgNydF3F4ojWcexcwIRcHafXnLd](https://ai.feishu.cn/docx/IgNydF3F4ojWcexcwIRcHafXnLd)

+   作者：智谱团队

+   Github 官方仓库：https://github.com/THUDM/LongWriter

+   技术支持：请到 github issue 提问

# 模型介绍

长文本生成即使一直是众多语言模型追求极致的方法，我们公开了一个**完整的长上下文生成方案，在两个**评**估基准 LongBench-W**rite和LongWrite-Ruler 上，我们尝试了8B模型与9B模型。并且对9B进行了DPO相关的任务， 从而使模型生成的输出更符合用户需求

在很多实验中，我们发现当前的长上下文大语言模型 (LLMs) 可以处理多达 100,000 个标记的输入，但很难生成超过 2,000 个单词的适度长度的输出。 SFT 数据集在上下文是并没有合适的适配任务，为此我们引入了 AgentWriter并构建了一个完整的包含 6,000 个 SFT 数据的LongWriter-6k数据集，输出长度范围为 2k 到 32k 个单词。通过将该数据集合并到模型训练中，我们成功地将现有模型的输出长度扩展到超过 10,000 个单词

在模型输出质量上 我们设计了评估超长生成能力的综合基准 LongBench-Write，9B 参数模型在改基准上通过 DPO（Direct Preference Optimization，直接偏好优化）超越了更大的专有模型。

总之， **LongWrite是一套完整的模型**长文本解决方案，当您需要解锁此功能的时候

# 技术报告和论文

> 论文地址：

> [L](https://arxiv.org/pdf/2408.07055)ongWriter: Unleashing 10,000 +Word Generation From Long Context LLMs

您可以引用以下论文：

```
@article{bai2024longwriter,
  title={LongWriter: Unleashing 10,000+ Word Generation from Long Context LLMs}, 
  author={Yushi Bai and Jiajie Zhang and Xin Lv and Linzhi Zheng and Siqi Zhu and Lei Hou and Yuxiao Dong and Jie Tang and Juanzi Li},
  journal={arXiv preprint arXiv:2408.07055},
  year={2024}
}
```

# 快速阅览

我们在文档不同的位置进行了详细的介绍，您可以点击进入并查看具体的技术文档。