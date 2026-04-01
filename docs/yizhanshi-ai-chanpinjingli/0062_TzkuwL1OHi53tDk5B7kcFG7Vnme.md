# 常见问题

> 来源：[https://v11enp9ok1h.feishu.cn/docx/WfG3dxRzfowDe3xMJEAcxv2cnyd](https://v11enp9ok1h.feishu.cn/docx/WfG3dxRzfowDe3xMJEAcxv2cnyd)

<https://v11enp9ok1h.feishu.cn/sync/VztedzIoUstVPDb6RNMctK8Unie>

### 在提出问题之前

请先查看：

https://github.com/THUDM/CogVLM/issues

和

https://github.com/THUDM/CogVLM/discussions

寻找相关的问题集

## Q 模型有没有Int4版本

1.  目前没有，官方正在规划中，但是你可以使用 int4来加载模型

## Q 模型能否拆分到多张卡进行使用

1.  可以，但是我们暂时没有进行多张显卡的深度尝试和压力测试。

## Q 是否有支持中文的模型

1.  没有，目前的 Cog 全系列模型仅支持英语

## Q 模型的视觉编码器 / tokenizer 能否更换

1.  理论可以，我们没有进行尝试