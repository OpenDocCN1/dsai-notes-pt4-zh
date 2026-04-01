# Hugging Face全面拥抱LangChain：全新官方合作包

> 来源：[https://agijuejin.feishu.cn/docx/PlUAdxCrEoJeX9xNqx8cuv24nRc](https://agijuejin.feishu.cn/docx/PlUAdxCrEoJeX9xNqx8cuv24nRc)

> 作者：AI小智

> 原文： [https://mp.weixin.](https://mp.weixin.qq.com/s/aZF_fbldRBJ5wZHADQvURw)qq.com/s/aZF_fbld...

![](img/99aea2ba7999151add37f3c0d6e84c40.png)

> **❝**

> 最近 **Hugging Face **官宣发布 langchain_huggingface ，这是一个由 Hugging Face 和 LangChain 共同维护的 LangChain 合作伙伴包。这个新的 Python 包旨在将 Hugging Face 最新功能引入 LangChain 并保持同步。

通过 **Hugging Face ****官方包的加持，开发小**伙伴们通过简**单的api调用就能在 la**ngchain 中轻松使用 Hugging Face 上各类流行的开源大语言模型以及各类AI工具。

以下是笔者在测试使用后，总结的在LangChain中无缝安装和使用Hugging Face最佳实践。

###  **如何安装 **

要使用 **Hugging Face **平台的功能，首先需要安装 langchain-h**uggingface 包，**这是一个专门为 Hugging Face 平台集成的Python包。

```
pip install langchain-huggingface
```

###  **大型语言模型(LLM) **

**LLM是Langchain****对大语言模型的核心抽象， **Hugging Face 中则将类似能力称之为Pipeline。因而 Hugging Face 平台使用 HuggingFacePipeline 类来启动开源大语言模型的本地启动

```
from langchain_huggingface.llms import HuggingFacePipeline

llm = HuggingFacePipeline.from_model_id(
    model_id=&quot;gpt2&quot;,
    task=&quot;text-generation&quot;,
    pipeline_kwargs={&quot;max_new_tokens&quot;: 10},
)
```

+   使用示例：https://python.langchain.com/v0.2/docs/integrations/llms/huggingface_pipelines/

+   API参考文档：https://api.python.langchain.com/en/latest/llms/langchain_huggingface.llms.huggingface_pipeline.HuggingFacePipeline.html

###  聊天模型(Chat modal)

**Chat作为Langcha**in集成与交互的核心入口， Hugging Face 平台也提供了多种开源聊天模型作为对应支持。你可以直接使用 ChatHuggingFace 类来快速集成开源LLM。

```
from langchain_core.messages import (
    HumanMessage,
    SystemMessage,
)
from langchain_huggingface import ChatHuggingFace

messages = [
    SystemMessage(content=&quot;You&#x27;re a helpful assistant&quot;),
    HumanMessage(
        content=&quot;What happens when an unstoppable force meets an immovable object?&quot;
    ),
]

chat_model = ChatHuggingFace(llm=llm)
```

+   使用示例：https://python.langchain.com/v0.2/docs/integrations/chat/huggingface/

+   API参考文档：https://api.python.langchain.com/en/latest/chat_models/langchain_huggingface.chat_models.huggingface.ChatHuggingFace.html

###  嵌入模型(embedding)

**RAG ****应用开发过程中必不可少的e**mbedding模型， Hugging Face 也提供了各类开源嵌入模型的选择，包括：

####   HuggingFaceEmbeddings

+   通用开源embedding模型加载

```
from langchain_huggingface import HuggingFaceEmbeddings

model_name = &quot;sentence-transformers/all-mpnet-base-v2&quot;
model_kwargs = {&#x27;device&#x27;: &#x27;cpu&#x27;}
encode_kwargs = {&#x27;normalize_embeddings&#x27;: False}
hf = HuggingFaceEmbeddings(
    model_name=model_name,
    model_kwargs=model_kwargs,
    encode_kwargs=encode_kwargs
)
```

+   使用示例：https://python.langchain.com/v0.2/docs/integrations/text_embedding/huggingfacehub/

+   API参考文档：https://api.python.langchain.com/en/latest/embeddings/langchain_huggingface.embeddings.huggingface.HuggingFaceEmbeddings.html

####   HuggingFaceInstructEmbeddings

+   指令型开源embedding模型加载

```
from langchain_community.embeddings import HuggingFaceInstructEmbeddings

model_name = &quot;hkunlp/instructor-large&quot;
model_kwargs = {&#x27;device&#x27;: &#x27;cpu&#x27;}
encode_kwargs = {&#x27;normalize_embeddings&#x27;: True}
hf = HuggingFaceInstructEmbeddings(
    model_name=model_name,
    model_kwargs=model_kwargs,
    encode_kwargs=encode_kwargs
)
```

+   使用示例：https://python.langchain.com/v0.2/docs/integrations/text_embedding/instruct_embeddings/

+   API参考文档：https://api.python.langchain.com/en/latest/embeddings/langchain_community.embeddings.huggingface.HuggingFaceInstructEmbeddings.html

####   HuggingFaceBgeEmbeddings

由北京人工智能研究院(BAAI)创建的BGE模型，是开源嵌入模型中的佼佼者。

```
from langchain_community.embeddings import HuggingFaceBgeEmbeddings

model_name = &quot;BAAI/bge-small-en&quot;
model_kwargs = {&quot;device&quot;: &quot;cpu&quot;}
encode_kwargs = {&quot;normalize_embeddings&quot;: True}
hf = HuggingFaceBgeEmbeddings(
    model_name=model_name, model_kwargs=model_kwargs, encode_kwargs=encode_kwargs
)
```

+   使用示例：https://python.langchain.com/v0.2/docs/integrations/text_embedding/bge_huggingface/

+   API参考文档：https://api.python.langchain.com/en/latest/embeddings/langchain_community.embeddings.huggingface.HuggingFaceBgeEmbeddings.html

####   **Hugging Face Text Embeddi**ngs Inference (TEI)

Hugging Face文本嵌入推理(TEI) 是一个部署和提供开源文本嵌入和序列分类模型的工具包，支持包括 FlagEmbedding 、 Ember 、 GTE 和 E5 在内的流行模型。部署后，可以通过本地的http端口来调用Embedding能力。

```
pip install huggingface-hub

model=BAAI/bge-large-en-v1.5
revision=refs/pr/5
volume=$PWD/data # share a volume with the Docker container to avoid downloading weights every run

docker run --gpus all -p 8080:80 -v $volume:/data --pull always ghcr.io/huggingface/text-embeddings-inference:0.6 --model-id $model --revision $revision
```

```
from langchain_huggingface.embeddings import HuggingFaceEndpointEmbeddings

embeddings = HuggingFaceEndpointEmbeddings(model=&quot;http://localhost:8080&quot;)
```

+   使用示例：https://python.langchain.com/v0.2/docs/integrations/text_embedding/text_embeddings_inference/

+   API参考文档：https://api.python.langchain.com/en/latest/embeddings/langchain_community.embeddings.huggingface_hub.HuggingFaceHubEmbeddings.html

###  **文档加载器 **

**Hugging Face **平台拥有超过75,000个数据集，涵盖100多种语言，适用于NLP、计算机视觉和音频领域的多种任务。通过HuggingFaceDatasetLoader，我们可以直接将这些数据集应用于LangChain系统的构建与测试。

####   Hugging Face Dataset

```
from langchain_community.document_loaders.hugging_face_dataset import HuggingFaceDatasetLoader

dataset_name = &quot;imdb&quot;
page_content_column = &quot;text&quot;


loader = HuggingFaceDatasetLoader(dataset_name, page_content_column)
```

+   使用示例：https://python.langchain.com/v0.2/docs/integrations/document_loaders/hugging_face_dataset/

+   API参考文档：https://api.python.langchain.com/en/latest/document_loaders/langchain_community.document_loaders.hugging_face_dataset.HuggingFaceDatasetLoader.html

###  **工具 **

####   Hugging Face Hub Tools

Hugging Face工具 支持文本输入输出，可以通过 load_huggingface_tool 函数加载。

```
pip install transformers huggingface_hub
```

```
from langchain.agents import load_huggingface_tool

tool = load_huggingface_tool(&quot;lysandre/hf-model-downloads&quot;)

print(f&quot;{tool.name}: {tool.description}&quot;)
```

+   使用示例：https://python.langchain.com/v0.2/docs/integrations/tools/huggingface_tools/

+   API参考文档：https://api.python.langchain.com/en/latest/agent_toolkits/langchain_community.agent_toolkits.load_tools.load_huggingface_tool.html

###  **总结 **

**通过上述介绍，相信你已经可****以轻松地在你的LangCh**ain项目中集成 Hugging Face 平台的强大功能。无论是聊天机器人、文本嵌入还是数据集加载， Hugging Face 平台都能为你提供更多的开源模型的支持。

今天的内容就到这里，如果老铁觉得还行，可以来一波三连，感谢！

**PS ： **A**I小智 **技 术交流群（技术交流、摸鱼、白嫖课程为主）又不定时开放了，感兴趣的朋友，可以在下方公号内回复： 666 ，即可进入。

**老规矩 **，道友们还记得么， **右下角的 “在看” 点一下 **， 如果感觉文章内容不错的话，记得分享朋友圈让更多的人知道！