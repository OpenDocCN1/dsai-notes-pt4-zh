
# RAG：Langchain中使用自己的LLM大模型

> 来源：[https://agijuejin.feishu.cn/docx/RbWWdlqMyoFG9Kx8jhLccsWtnDV](https://agijuejin.feishu.cn/docx/RbWWdlqMyoFG9Kx8jhLccsWtnDV)

> 作者：哎呀AIYA

> 原文：https://mp.weixin.qq.com/s/p_-pWbBguLbm7LjreCH3pA

hi！这里是一个专注大模型应用的频道，介绍大模型基础、创作、RAG、Agent，每天学习一小点LLM不再迷惑，欢迎关注！

在使用Langchain做RAG和Agent项目中，智能核心是LLM大语言模型；配置好模型是整个流程跑起来的关键。然而很多人初学者在为没有`OpenAI`的`api-key`而苦恼，在寻找代理或者替代品中花费了很多的时间。

本篇文章将为大家介绍在`langchain`中使用自己大模型的方法，帮助大家扫清障碍快速搭建RAG和Agent流程。

## 环境依赖

```
langchain_huggingface, langchain_core, transformer, torch
```

## 模型选择

我们选择Qwen最新发布的`Qwen2-0.5B-Instruct`模型，模型可以提前下载好，也可以加载的时候下载。下载代码如下：

```
from huggingface_hub import snapshot_download

snapshot_download(repo_id='Qwen/Qwen2-0.5B-Instruct', 
                  local_dir="Qwen/Qwen2-0.5B-Instruct",
                  local_dir_use_symlinks=False, resume_download=True)
```

## 示例代码

话不多说，直接上代码：

```
from langchain_huggingface import HuggingFacePipeline
from langchain_core.messages import (
    HumanMessage,
    SystemMessage,
)
from langchain_huggingface import ChatHuggingFace

# 加载模型
llm = HuggingFacePipeline.from_model_id(
    model_id="Qwen/Qwen2-0.5B-Instruct",
    task="text-generation",
    pipeline_kwargs=dict(
        max_new_tokens=512,
        do_sample=False,
    ),
)
# 封装模型
chat_model = ChatHuggingFace(llm=llm)

# 跑个例子
messages = [
    SystemMessage(content="You are a helpful assistant."),
    HumanMessage(
        content="你好"
    ),
]

print(chat_model.invoke(messages))
```

上面的例子一般可以正常运行，但是也可能会报如下错误：

```
huggingface_hub.errors.LocalTokenNotFoundError: Token is required (`token=True`), but no token found. You need to provide a token or be logged in to Hugging Face with `huggingface-cli login` or `huggingface_hub.login`. See https://huggingface.co/settings/tokens.
```

根据报错信息可以看到是没有提供`token`，`token`就是在huggingface官网上每个账号的标识，根据`token`可以追溯到账号。

如何提供`token`呢？首先我们去huggingface官网注册一个账号（如果打不开网页需要使用VPN），然后在用户头像那里，点击Settings，再点击Access Tokens，在里面设置。

不能🪜的朋友们不用担心，可以使用下面方案一。

## 问题修复

### 方案一：更改 `langchain_huggingface` 源码

文件地址：`langchain_huggingface/chat_models/huggingface.py`

![](img/4598b7aac6082763a237ce2fd30b5a69.png)

注释第一个红框标红的代码，在第二个红框输入对应内容，就完美解决啦。

### 方案二：设置 `token`

命令行输入：

```
from huggingface_hub import notebook_login
notebook_login()
```

然后填入huggingface的`token`就可以了。

如果对内容有什么疑问和建议可以私信和留言，也可以添加我加入大模型交流群，一起讨论大模型在创作、RAG和agent中的应用。

好了，这就是我今天想分享的内容。如果你对构建大模型应用感兴趣，别忘了点赞、关注噢~

## 往期推荐

+   详解GPT-4o的收费设计，为什么输出`token`贵？
+   大模型LLM的定位器Attention自注意力|兼看Qwen2参数构成
+   大模型`token`的漫游之旅：Word Embedding
