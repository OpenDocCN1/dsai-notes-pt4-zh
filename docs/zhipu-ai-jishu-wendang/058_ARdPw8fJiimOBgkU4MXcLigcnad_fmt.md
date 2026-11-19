
# 模型列表

> 来源：[https://ai.feishu.cn/docx/YiBbdbPj4omYxAxmzKMcLQyjnyV](https://ai.feishu.cn/docx/YiBbdbPj4omYxAxmzKMcLQyjnyV)

我们在 Huggingface 仓库中提供了多个不同类型的模型，这些模型具有不同的作用，本章节将为开发者列出不同的模型列表的含义和信息。

下表展现了 `CogAgent` 系列模型列表:

下表展现了 `CogVLM` 系列模型列表:

您可以参考下述思维导图，决定使用哪个模型：

其中`1120*1120`比`490*490`分辨率的显存占用稍高（`INT4`模式下：`CogAgent(1120)` ~ `12.6GB`, `CogVLM(490)` ~ `11GB`），均高于`224*224`。

所有模型都可以进行微调。对应的微调脚本请到对应的章节进行查询。
