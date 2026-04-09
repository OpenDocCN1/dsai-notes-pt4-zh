# LLMs 配置

> 来源：[https://deepwisdom.feishu.cn/docx/PTCzdSJwyoKGKNxlh4EcnZOJnic](https://deepwisdom.feishu.cn/docx/PTCzdSJwyoKGKNxlh4EcnZOJnic)

## Warning: model not found.

```
Warning: model not found. Using cl100k_base encoding
```

A：对应 LLM 支持 openai 的 ap i兼容模式，但是因为对应不能从接口结果里得到 token 使用统计，直接使用 tokenizer 计算时，由于不是 openai 对应的模型名导致。使用单独的 token 统计逻辑。不影响后续进程推进，如果遇到进程问题，请关注后续具体 error 的内容

## openai.types.completion_usage.CompletionUsage() argument after ** must be a mapping, not NoneType

```
openai.types.completion_usage.CompletionUsage() argument after ** must be a mapping, not NoneType
```

A：https://github.com/geekan/MetaGPT/pull/1253（需要更新到最新的MG版本）