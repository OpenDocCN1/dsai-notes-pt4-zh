
# GraphRAG新增文件会重新索引所有文件吗？探究缓存何时生效

> 来源：[https://agijuejin.feishu.cn/docx/JrmjdExPaonzdFxHZ0Ic0zhvnJg](https://agijuejin.feishu.cn/docx/JrmjdExPaonzdFxHZ0Ic0zhvnJg)

> 作者：还就可 | 深入LLM Agent应用开发

> 原文： [https://mp.weixin.](https://mp.weixin.qq.com/s/YAlI2Do_WZhMH4IMzY6dGQ)qq.com/s/YAlI2Do_...

GraphRAG索引消耗Token巨大，所以你可能会问？新增文件后，我之前的文件会不会重新索引一次啊？什么情况下会利用缓存呢？本文带你一探究竟。

GraphRAG默认使用文件缓存，配置如下。当然你也可以考虑使用Azure云端的缓存。

```
cache:
  type: file # or blob
  base_dir: "cache"
  # connection_string: <azure_blob_storage_connection_string>
  # container_name: <azure_blob_storage_container_name>
```

缓存放在`cache`目录下，并根据流程分为实体提取、总结描述、社区报告和文本嵌入四个部分，所以缓存基本涵盖了所有涉及LLM调用或者Embedding调用的部分。

![](img/7579f8c2ca93c2a8ed17eeabb7131955.png)

缓存的文件名类似`chat-0a0edbed8a583fd6ef2647112f565bd9`、`create_community_report-chat-v2-0a8d6dcd76f6d92edb9388681da40d1d`，`tag`和`hash`值组合而成。缓存的内容是什么呢？其实就LLM的请求输入和响应输出，或者是embedding API的调用回复。

```
{
    "result": "(\"entity\"<|>\"Retrieval-Augmented Generation\"<|>\"technique\"<|>...<|COMPLETE|>",
    "input": "-Goal-\nGiven a text document ...",
    "parameters": {
        "model": "deepseek-chat",
        "temperature": 0,
        "frequency_penalty": 0,
        "presence_penalty": 0,
        "top_p": 0.99,
        "max_tokens": 4096,
        "n": 1
    }
}
```

那么何时缓存会生效？修改哪些部分会影响缓存呢？让我们看GraphRAG中如下代码，它是为缓存文件生成文件名，也就是上文看到文件名。

```
def create_hash_key(operation: str, prompt: str, parameters: dict) -> str:
    """Compute cache key from prompt and associated model and settings.

    Args:
        prompt (str): The prompt run through the language model.
        llm_string (str): The language model version and settings.

    Returns
    -------
        str: The cache key.
    """
    llm_string = _llm_string(parameters)
    return f"{operation}-{_hash(prompt + llm_string)}"
```

从这份代码上，我们能够了解到，有两个影响`hash`值的生成

+   输入的`Prompt`，包含用户输入

+   LLM的参数

这是实际运行过程中打印的参数

```
tag: chat
llm_string: {'model': 'llama3-70b-8192', 'temperature': 0.0, 'frequency_penalty': 0.0, 'presence_penalty': 0.0, 'top_p': 0.99, 'max_tokens': 2000, 'n': 1}
[('frequency_penalty', 0.0), ('max_tokens', 2000), ('model', 'llama3-70b-8192', 'n', 1), ('presence_penalty', 0.0), ('temperature', 0.0, 'top_p', 0.99)]
```

所以当你

+   换一个LLM，或者修改LLM的参数，都会导致重新索引

+   加入新的文件，只会索引该文件，已经索引的文件不会重新索引。

+   修改现有文件，可能导致整个文件重新索引，或者是部分，因为修改文件，如果只是部分`chunk`改变，那么只会是这部分`chunk`需要重新索引。

因此，如果你的单个文档较大，并且可能会经常修改，可以考虑将其拆分，以充分利用缓存，节省你的Money。
