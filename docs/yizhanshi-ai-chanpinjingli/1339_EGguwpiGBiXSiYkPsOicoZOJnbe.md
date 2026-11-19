
# Langchain —— 工具/函数调用

> 来源：[https://v11enp9ok1h.feishu.cn/docx/FjTgdszEtome3rxcg7ecqw19nch](https://v11enp9ok1h.feishu.cn/docx/FjTgdszEtome3rxcg7ecqw19nch)

LLM 的一个令人兴奋的用例是为其他“工具”构建自然语言接口，无论是 API、函数、数据库等。LangChain 非常适合构建此类接口，因为它具有：

+ 良好的模型输出解析，可以轻松地从模型输出中提取 JSON、XML、OpenAI 函数调用等。

+ 大量内置[工具](https://python.langchain.com/v0.1/docs/integrations/tools/)。

+ 在调用这些工具的方式上提供了很大的灵活性。

使用工具主要有两种方式：[链和](https://python.langchain.com/v0.1/docs/modules/chains/)[代理](https://python.langchain.com/v0.1/docs/modules/agents/)。

链允许您创建预定义的工具用法序列。

![](img/3978ae0611a8fff7f7deeba4cb115fb1.png)

代理让模型在循环中使用工具，以便它可以决定使用工具的次数。

![](img/0ac939e0edb30ffbe90843eed58aef84.png)

## 快速入门

我们需要为本指南安装以下软件包：

```
%pip install --upgrade --quiet langchain
```

如果您想在 [LangSmith](https://python.langchain.com/v0.1/docs/langsmith/) 中跟踪您的运行，请取消注释并设置以下环境变量：

```
import getpass
import os

# os.environ["LANGCHAIN_TRACING_V2"] = "true"
# os.environ["LANGCHAIN_API_KEY"] = getpass.getpass()
```
