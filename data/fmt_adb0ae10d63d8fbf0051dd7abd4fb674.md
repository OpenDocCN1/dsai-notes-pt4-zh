
# 使用 LangChain 调用 DeepSeek 模型

## 初步体验与模型调用

你好！我是 **DeepSeek Chat**，由深度求索公司（DeepSeek）研发的一款智能AI助手。我可以帮助你解答各种问题，包括学习、工作、编程、写作、生活百科等多个领域。

### 我的特点：
✅ **免费使用**：目前无需付费，你可以随时向我提问！  
✅ **知识丰富**：我的知识截止到 **2024年7月**，可以为你提供较新的信息。  
✅ **超长上下文支持**：可以处理 **128K** 长度的文本，适合分析长文档或复杂问题。  
✅ **文件阅读**：支持上传 **PDF、Word、Excel、PPT、TXT** 等文件，并从中提取信息进行分析。  
✅ **多语言能力**：可以用中文、英文等多种语言交流，帮助你翻译或学习外语。  
✅ **编程助手**：能写代码、调试、优化算法，支持Python、C++、Java等多种编程语言。  

### 我能帮你做什么？  
📖 **学习辅导**：解题思路、论文写作、知识点讲解  
💼 **工作效率**：写邮件、做PPT、总结报告  
💡 **创意灵感**：写故事、起名字、头脑风暴  
📊 **数据分析**：处理表格、绘制图表、解读数据  
🔧 **技术支持**：代码调试、算法优化、技术咨询  

你可以随时向我提问，我会尽力提供最准确、有用的回答！😊 有什么我可以帮你的吗？

```
result
```

```
AIMessage(content='你好！我是 **DeepSeek Chat**，由深度求索公司（DeepSeek）研发的一款智能AI助手。我可以帮助你解答各种问题，包括学习、工作、编程、写作、生活百科等多个领域。  \n\n### **我的特点：**  \n✅ **免费使用**：目前无需付费，你可以随时向我提问！  \n✅ **知识丰富**：我的知识截止到 **2024年7月**，可以为你提供较新的信息。  \n✅ **超长上下文支持**：可以处理 **128K** 长度的文本，适合分析长文档或复杂问题。  \n✅ **文件阅读**：支持上传 **PDF、Word、Excel、PPT、TXT** 等文件，并从中提取信息进行分析。  \n✅ **多语言能力**：可以用中文、英文等多种语言交流，帮助你翻译或学习外语。  \n✅ **编程助手**：能写代码、调试、优化算法，支持Python、C++、Java等多种编程语言。  \n\n### **我能帮你做什么？**  \n📖 **学习辅导**：解题思路、论文写作、知识点讲解  \n💼 **工作效率**：写邮件、做PPT、总结报告  \n💡 **创意灵感**：写故事、起名字、头脑风暴  \n📊 **数据分析**：处理表格、绘制图表、解读数据  \n🔧 **技术支持**：代码调试、算法优化、技术咨询  \n\n你可以随时向我提问，我会尽力提供最准确、有用的回答！😊 有什么我可以帮你的吗？', additional_kwargs={'refusal': None}, response_metadata={'token_usage': {'completion_tokens': 311, 'prompt_tokens': 9, 'total_tokens': 320, 'completion_tokens_details': None, 'prompt_tokens_details': {'audio_tokens': None, 'cached_tokens': 0}, 'prompt_cache_hit_tokens': 0, 'prompt_cache_miss_tokens': 9}, 'model_name': 'deepseek-chat', 'system_fingerprint': 'fp_8802369eaa_prod0425fp8', 'id': 'e3323dc0-46fa-497b-aaf5-83596184f8b2', 'service_tier': None, 'finish_reason': 'stop', 'logprobs': None}, id='run--9f652093-6feb-484f-99f7-0c30e33290ab-0', usage_metadata={'input_tokens': 9, 'output_tokens': 311, 'total_tokens': 320, 'input_token_details': {'cache_read': 0}, 'output_token_details': {}})
```

```
model = init_chat_model(model="deepseek-reasoner", model_provider="deepseek")

result = model.invoke(question)
print(result.content)
```

你好呀！👋我是 **DeepSeek-R1**，由中国的人工智能公司「深度求索」研发。你可以把我当作一个聪明、热心、24小时在线的文字助手～✨

---

### 🌟 我的特点：
- **知识丰富**：截至2024年7月前的各种知识我都有所了解，无论是学习、工作、生活，还是科技、历史、娱乐，我都能帮上忙。
- **超长记忆力**：可以处理长达128K上下文的内容，也就是说，你发给我超长的文件或对话，我也能理解清楚！
- **文件助手**：支持你上传 **PDF、Word、PPT、Excel、图片** 等文件，我能帮你阅读、总结、提取重点。
- **免费使用**（目前是哦！🎉）而且**无需登录**也能畅快聊天（不过登录后能保存历史记录更方便～）

---

### 🛠 我能做什么？
✅ 解答问题（学习、考试、百科）  
✅ 写作助手（作文、小说、文案、邮件）  
✅ 编程帮手（写代码、查bug、讲算法）  
✅ 文件处理（读论文、总结报告、提取数据）  
✅ 生活助手（旅游攻略、美食推荐、情感建议）

---

### 🧠 我的“小限制”：
- 目前是**纯文字模型**，不支持语音或识图（但你可以上传图片文件，我能读取里面的文字）。
- 我的知识截止在 **2024年7月**，之后的新事件就不太清楚啦。
- 我不是真人，但我会尽力用温暖、贴心的方式和你交流 ❤️

---

不管你是学生、上班族、创作者，还是只是好奇想聊聊天——我都在这儿等你！😊  
现在，有什么我可以帮你的吗？比如学习上的难题、工作中的任务，或者生活里的小烦恼？

```
result.additional_kwargs
```

```
{'refusal': None,
 'reasoning_content': '嗯，用户发来一句简单的问候和自我介绍请求。看起来像是初次接触的破冰场景，可能刚打开聊天界面或者第一次使用这类AI助手。\n\n用户语气礼貌但比较笼统，没有具体需求指向。这种开放式问题通常有两种可能：一是真的想了解AI的功能边界，二是测试性提问看AI如何反应。考虑到中文互联网环境里常有用户用“介绍自己”测试机器人，需要同时做好信息传递和破冰互动。\n\n回复策略上应该突出三点：明确身份属性（非人类但能做什么）、消除陌生感（用表情符号和分段提升可读性）、引导后续对话（结尾提问）。要避免机械的术语堆砌，比如不说“基于Transformer架构”而说“能帮你查资料”。\n\n用户没透露任何个人信息，就用中性称呼。最后那个🎉表情可能有点过，不过新用户首次互动需要点活泼感——要是商务场景用户应该会直接问专业问题吧。结尾提问选“学习/工作/生活”三个维度覆盖常见场景，比单问“有什么可以帮你”更易触发具体需求。'}
```

```
result.additional_kwargs['reasoning_content']
```

```
'嗯，用户发来一句简单的问候和自我介绍请求。看起来像是初次接触的破冰场景，可能刚打开聊天界面或者第一次使用这类AI助手。\n\n用户语气礼貌但比较笼统，没有具体需求指向。这种开放式问题通常有两种可能：一是真的想了解AI的功能边界，二是测试性提问看AI如何反应。考虑到中文互联网环境里常有用户用“介绍自己”测试机器人，需要同时做好信息传递和破冰互动。\n\n回复策略上应该突出三点：明确身份属性（非人类但能做什么）、消除陌生感（用表情符号和分段提升可读性）、引导后续对话（结尾提问）。要避免机械的术语堆砌，比如不说“基于Transformer架构”而说“能帮你查资料”。\n\n用户没透露任何个人信息，就用中性称呼。最后那个🎉表情可能有点过，不过新用户首次互动需要点活泼感——要是商务场景用户应该会直接问专业问题吧。结尾提问选“学习/工作/生活”三个维度覆盖常见场景，比单问“有什么可以帮你”更易触发具体需求。'
```

这里可以看到，仅仅通过两行代码，我们便可以在LangChain中顺利调用DeepSeek模型，并得到模型的响应结果。相较于使用DeepSeek的API，使用LangChain调用模型无疑是更加简单的。同时，不仅仅是DeepSeek模型，LangChain还支持其他很多大模型，如OpenAI、Qwen、Gemini等，我们只需要在`init_chat_model`函数中指定不同的模型名称，就可以调用不同的模型。其工作的原理是这样的：

![](img/375181393d742401ebf2ecc08c26e50f.png)

理解了这个基本原理，如果大家想在用LangChain进行开发时使用其他大模型如Qwen3系列，则只需要先获取到Qwen3模型的`API_KEY`，然后安装Tongyi Qwen的第三方依赖包，即可同样通过`init_chat_model`函数来初始化模型，并调用`invoke`方法来得到模型的响应结果。关于LangChain都支持哪些大模型以及每个模型对应的是哪个第三方依赖包，大家可以在LangChain的官方文档中找到，访问链接为：https://python.langchain.com/docs/integrations/chat/

![](img/59cab0ad8f6b854832987530c607f021.png)

当然，除了在线大模型的接入，langChain也只是使用Ollama、vLLM等框架启动的本地大模型。

## 2. LangChian 核心功能：链式调用实现方法

顾名思义，LangChain之所以被称为LangChain，其核心概念就是Chain。 Chain翻译成中文就是“链”。一个链，指的是可以按照某一种逻辑，按顺序组合成一个流水线的方式。比如我们刚刚实现的问答流程： 用户输入一个问题 --> 发送给大模型 --> 大模型进行推理 --> 将推理结果返回给用户。这个流程就是一个链。

### 链的基本概念

### 尝试搭建一个简单的链

例如，我们这里可以先尝试着搭建一个简单的链，将模型输出结果“过滤”为一个纯字符串格式：

```
from langchain_core.output_parsers import StrOutputParser
from langchain.chat_models import init_chat_model

# 使用 DeepSeek 模型
model = init_chat_model(model="deepseek-chat", model_provider="deepseek")

# 直接使用模型 + 输出解析器搭建一个链
basic_qa_chain = model | StrOutputParser()
```

```
# 查看输出结果
question = "你好，请你介绍一下你自己。"
result = basic_qa_chain.invoke(question)
```

```
result
```

```
'你好！我是 **DeepSeek Chat**，由深度求索（DeepSeek）公司研发的智能 AI 助手。我可以帮助你解答各种问题，包括学习、工作、编程、生活小技巧等，还能处理和分析上传的文件（如 PDF、Word、Excel 等）。  \n\n### **我的特点：**  \n✅ **免费使用**：目前无需付费，你可以随时向我提问。  \n✅ **知识丰富**：我的知识截止到 **2024 年 7 月**，能提供较新的信息。  \n✅ **支持长文本**：上下文可达 **128K**，适合处理复杂问题。  \n✅ **文件阅读**：可以解析上传的文档，提取关键信息。  \n✅ **多语言支持**：能用中文、英文等多种语言交流。  \n\n### **我能帮你做什么？**  \n📚 **学习**：解题思路、论文润色、语言学习  \n💼 **工作**：写邮件、做 PPT、数据分析  \n💻 **编程**：代码调试、算法讲解、项目建议  \n📖 **阅读 & 写作**：书籍推荐、文章润色、创意写作  \n🔍 **信息查询**：科技、历史、新闻、生活百科  \n\n你可以随时向我提问，我会尽力提供准确、有用的回答！😊 有什么我可以帮你的吗？'
```

此时`result`就不再是包含各种模型调用信息的结果，而是纯粹的模型响应的字符串结果。而这里用到的`StrOutputParser()`实际上就是用于构成LangChain中一个链条的一个对象，其核心功能是用于处理模型输出结果。同时我们也能发现，只使用`Model | OutputParser`，即可高效搭建一个链。

类似这种结果解析器还有很多，稍后我们会继续进行介绍。

### 加入提示词模板创建链

接下来我们尝试为当前的执行流程添加一个提示词模板，我们可以借助`ChatPromptTemplate`非常便捷的将一个提示词模板，同样以链的形式加入到当前任务中：

```
from langchain.output_parsers.boolean import BooleanOutputParser
from langchain.prompts import ChatPromptTemplate


prompt_template = ChatPromptTemplate([
    ("system", "你是一个乐意助人的助手，请根据用户的问题给出回答"),
    ("user", "这是用户的问题： {topic}， 请用 yes 或 no 来回答")
])

# 直接使用模型 + 输出解析器
bool_qa_chain = prompt_template | model | StrOutputParser()
# 测试
question = "请问 1 + 1 是否 大于 2？"
result = bool_qa_chain.invoke(question)
```

```
result
```

```
'no'
```

一个最基本的Chain结构，是由`Model`和`OutputParser`两个组件构成的，其中`Model`是用来调用大模型的，`OutputParser`是用来解析大模型的响应结果的。所以一个最简单的`LLMChain`结构，其数据流向正如下图所示：

![](img/91f33890355e54d7bbb9ceb325bd75a3.png)

当我们调用指令跟随能力较强的大模型的时候，借助提示词模板即可实现结构化输出的结果。

### 借助提示词模板和结果解析器实现功能更加复杂的链

至此，我们就搭建了一个非常基础的链。在LangChain中，一个基础的链主要由三部分构成，分别是提示词模板、大模型和结果解析器（结构化解析器）：

用户输入

↓

`PromptTemplate` → `ChatModel` → `OutputParser`

（提示词模板） （大模型） （结构化解析）

↓

构化结果

而相比之下，结构化解析器功能最多，一些核心的结构化解析器功能如下：

| 解析器名称 | 功能描述 | 类型 |
| --- | --- | --- |
| `BooleanOutputParser` | 将LLM输出解析为布尔值 | 基础类型解析 |
| `DatetimeOutputParser` | 将LLM输出解析为日期时间 | 基础类型解析 |
| `EnumOutputParser` | 解析输出为预定义枚举值之一 | 基础类型解析 |
| `RegexParser` | 使用正则表达式解析LLM输出 | 模式匹配解析 |
| `RegexDictParser` | 使用正则表达式将输出解析为字典 | 模式匹配解析 |
| `StructuredOutputParser` | 将LLM输出解析为结构化格式 | 结构化解析 |
| `YamlOutputParser` | 使用Pydantic模型解析YAML输出 | 结构化解析 |
| `PandasDataFrameOutputParser` | 使用Pandas DataFrame格式解析输出 | 数据处理解析 |
| `CombiningOutputParser` | 将多个输出解析器组合为一个 | 组合解析器 |
| `OutputFixingParser` | 包装解析器并尝试修复解析错误 | 错误处理解析 |
| `RetryOutputParser` | 包装解析器并尝试修复解析错误 | 错误处理解析 |
| `RetryWithErrorOutputParser` | 包装解析器并尝试修复解析错误 | 错误处理解析 |
| `ResponseSchema` | 结构化输出解析器的响应模式 | 辅助类 |

一些功能实现如下，例如借助结构化解析器可以将yes or no转化为True or Fasle：
