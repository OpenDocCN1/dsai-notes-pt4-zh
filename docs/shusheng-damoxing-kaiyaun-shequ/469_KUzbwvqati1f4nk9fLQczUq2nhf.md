# 各模型框架工具调用姿势（范式）调研

> 来源：[https://aicarrier.feishu.cn/docx/NGTZdCau3oog33xSLsEcopJPnTb](https://aicarrier.feishu.cn/docx/NGTZdCau3oog33xSLsEcopJPnTb)

作者：书生·浦语开源社区贡献者——樊奇，内容转载请联系浦语小助手～，如果错误请指正～

**调研目标：**

+   通过调研各种模型框架 Function Calling 的调用姿势，搞清楚 LMDeploy 应该支持以什么样的姿势进行部署支持，以及 XTuner 工具调用能力的增强应该需要如何支持！

+   给书生·浦语社区小伙伴带来智能体方面认知上的真知灼见！

**Q1：**如果 XTuner 支持工具调用能力增强的微调，是哪种范式下的微调？

**Q2：**LMDeploy 应该支持以什么样的姿势进行部署支持？

# 通用智能体范式

通**用智能体范式，即模型不需要**通过 special token 来描述工具以及参数的起止点。模型只需要通过强大的指令跟随、推理等能力，遵循 system prompt 中所要求的格式进行回复。

此种方式的优势：

此种方式的劣势：

## ReAct

## ReWoo

# 模型特化智能体范式

模型特化智能体范式，即模型需要通过 special token 来描述工具以及参数的起止。如 InternLM2 需要通过 <|action_start|> 和 <|action_end|> 来描述，这往往与模型的 Tokenizer 中的 special token 强相关。

此种方式的优势：

此种方式的劣势：

## InternLM2

https://github.com/InternLM/InternLM/tree/main/agent

智能体相关特殊 token：

+   <|plugin|>

+   <|interpreter|>

+   <|action_start|>

+   <|action_end|>

## Octopus

# 社区 Function Calling 碰到的问题

## Q1

我通过lmdeploy部署了qwen1.5-32B。我是通过langchain的openai格式访问的模型，模型可以正常访问message内容。但是如何传递工具（bindtools或者设这fucntionls）呢？我一直卡在这里，有没有佬有这方面的经验，使用本地部署的模型调用工具成功的方法。不使用langchain也可以，其他框架也行。现在很多agent框架都是以调用api为、来使用模型的，或者有无本地模型调用工具的一些经验。希望本地是使用了lmdeploy或者vllm加速后的模型能够实现工具或者function call的功能。