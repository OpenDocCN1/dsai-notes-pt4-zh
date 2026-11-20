
# 20240422 - RAG模块提示词修改

> 来源：[https://deepwisdom.feishu.cn/docx/LO4AdT7meoLGEGxoCOCckRgrnif](https://deepwisdom.feishu.cn/docx/LO4AdT7meoLGEGxoCOCckRgrnif)

> 状态：讨论中

## QA：

Q: 请问，关于`rag`模块，那个`aquery`函数背后的提示词可以修改吗

A: 提示词模版吗

Q: 对，我可以仅用`rag`去获取搜索得到的块吗？或者我可以修改最后输出回答的模版

A: 获取搜索得到的块，可以使用`aretrieve`

Q: 是`engine`的方法吗？

Q: 那`simpleengine`可以有方法改`aquery`的提示词吗

A: `https://github.com/geekan/MetaGPT/blob/main/metagpt/rag/engines/simple.py#L171`

Q: 哦哦，这个是直接取回，那可以改`aquery`背后的提示词模版吗

A: **可以，`engine`有个属性**`response_synthesizer`，默认值是`get_response_synthesizer(llm=llm)`，有个参数`text_qa_template`对应提示词模版。

## 相关文档：

[RAG模块](https://deepwisdom.feishu.cn/wiki/BNRiwEilUizWTokKG06c4CM2njg)
