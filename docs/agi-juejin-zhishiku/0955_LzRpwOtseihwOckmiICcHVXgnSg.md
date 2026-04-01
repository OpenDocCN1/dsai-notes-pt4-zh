# 手把手教你实现大模型RAG[4]-RAG类的实现与介绍

> 来源：[https://agijuejin.feishu.cn/docx/EIPOdcuKWo2IyAxKqzkcDwwwnyl](https://agijuejin.feishu.cn/docx/EIPOdcuKWo2IyAxKqzkcDwwwnyl)

> 作者：aC大

> 原文：[https://zhuanlan.z](https://zhuanlan.zhihu.com/p/695643553)hihu.com/p/695643...

### 前言：

《手把手教你实现大模型RAG》系列文章分享的是从0到1基于langchain实现大模型结合本地知识库的问答系统。主要内容包括：

+   [1]-手把手教你实现大模型RAG-大模型应用技术介绍

+   [2]-手把手教你实现大模型RAG-RAG流程的系统介绍

+   [3]-手把手教你实现大模型RAG-UI实现

+   [4]-手把手教你实现大模型RAG-RAG类的实现

+   [5]-手把手教你实现大模型RAG-基于langchain的文本切割

+   [6]-手把手教你实现大模型RAG-基于langchain的文本向量化

+   [7]-手把手教你实现大模型RAG-第三方大模型接口接入

+   [8]-手把手教你实现大模型RAG-chatglm3本地部署openai标准接口及接入

+   [[9]-手](https://www.zhihu.com/search?q=%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A695643553%7D)把手教你实现大模型RAG- 环境变量 的配置和加载

本系列文章会逐一分享上述内容，同时作者也出了该系列的视频课程，见 B站《手把手教你实现大模型RAG-acedar讲师》 , 文章和视频结合学习效果更佳。

### UI实现的补充

继上一篇介绍的UI实现，现补充UI实现中内部的调用，我们将主要的调用[封装到RA](https://www.zhihu.com/search?q=%E5%87%BD%E6%95%B0%E8%B0%83%E7%94%A8&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A695643553%7D)G类中，在UI实现的web_main.py文件中添加RAG的实例化及 函数调用 ，增加完整的RAG相关的函数调用后的完整代码如下：

```
import time[
imp](https://www.zhihu.com/search?q=%E4%B8%8A%E4%BC%A0%E6%96%87%E6%A1%A3&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A695643553%7D)ort gradio as gr
fr[om ](https://www.zhihu.com/search?q=%E7%9F%A5%E8%AF%86%E5%BA%93&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A695643553%7D)llm_proxy.langchain_proxy import RAG

rag = RAG()

def add_text(history, text):
    print(&quot;add_text&quot;)
    history = history + [(text, None)]
    return history, gr.update(value=&#x27;&#x27;, interactive=True)


def upload_file(history, file):
    print(&quot;upload file&quot;)
    &quot;&quot;&quot;
        1. 上传文档        2. 文档切割
        3. 文档向量化
        4. 检索
    &quot;&quot;&quot;
    rag.db.load_dir(file.name)
    history = history + [((file.name, ), None)]
    print(&quot;upload_file,&quot;, history)
    return history


def chat(history):
    print(&quot;chat&quot;)
    print(&quot;upload_file,&quot;, history)

    msg = history[-1][0]
    if isinstance(msg, tuple):
        ans = &#x27;文件上传成功！&#x27;
    else:

        &quot;&quot;&quot;
            # todo:
            1. 根据用户的提问，调用检索服务，获取与问题相关本地知识库信息
            2. 调用大模型，对信息进行整合
            3. 解析大模型结果，并返回
        &quot;&quot;&quot;
        response = rag.as_retrival_qa()({&quot;query&quot;: msg})
        ans = &#x27;&#x27;
        try:
            ans = response[&#x27;result&#x27;]
            # ans = &#x27;待调用检索服务及大模型，敬请期待&#x27;
        except Exception as err:
            print(&quot;err:{}, ans: {}&quot;.format(err, response))

    history[-1][1] = &#x27;&#x27;
    for ch in ans:
        history[-1][1] += ch
        time.sleep(0.01)
        yield history[-1:]


with gr.Blocks() as demo:
    with gr.Tab(&quot;RAG文本对话&quot;):
        model_list = [&quot;百度零一34b大模型&quot;, &quot;chatglm3&quot;]
        input_model_list = gr.Dropdown(choices=model_list, label=&#x27;模型选择&#x27;)

        chatbot = gr.Chatbot(
            [],
            elem_id=&#x27;chatbot&#x27;,
            bubble_full_width=False
        )

        with gr.Row():
            txt = gr.Textbox(
                scale=4,
                show_label=False,
                placeholder=&#x27;输入文本并回车，后者上传本地文件&#x27;,
                container=False
            )
            btn = gr.UploadButton(&quot;文件&quot;, file_types=[])

        txt_msg = txt.submit(add_text, [chatbot, txt], [chatbot, txt], queue=False).then(
            chat, chatbot, chatbot
        )
        file_msg = btn.upload(upload_file, [chatbot, btn], [chatbot], queue=False).then(
            chat, chatbot, chatbot
        )

demo.queue()

if __name__ == &quot;__main__&quot;:
    demo.launch()
```

相关函数介绍： - rag = RAG()：实例化RAG类，为了使用的方便，与UI交互的函数均封装在该接口中 - rag.db.load_dir(file.name)： 该项目基于大模型+本地知识库的问答，需要知识文件的上传。load_dir是处理上传文件的，这里支持目录的上传，也支持单个文件的加载。如果是目录，在会依次解析目录下的每个文件，按照不同的文件格式加载。 - response = rag.as_retrival_qa()({"query": msg})： 该接口是直接调用大模型的接口，msg是用户输入的问题。该函数内部会先从上传的本地知识库中检索相关的文档，接着按prompt模板生成调用大模型的数据，并请求大模型。

### RAG类的实现

```
import os
from langchain.chains import RetrievalQA
from langchain.prompts import PromptTemplate
from langchain_community.llms.chatglm3 import ChatGLM3
from local_data_retrieval.text_retrieval import Text2embedStore
from llm_proxy.llm import BaiduLlm


qa_prompt = PromptTemplate.from_template(&quot;&quot;&quot;
根据给出的上下文信息回答问题，如果从上下文信息中找不到答案就诚实的说不知道。
上下文信息: {context}
问题: {question}
&quot;&quot;&quot;)


class RAG(object):

    def __init__(self):
        self.model_name = os.environ.get(&quot;MODEL_NAME&quot;)

        self.model_init()

    def model_init(self):
        # 向量相关的
        embed_model_name = os.environ.get(&quot;EMBED_MODEL_NAME&quot;)
        self.db = Text2embedStore(embed_model_name)

        if self.model_name == &#x27;baidu-yi_34b_chat&#x27;:
            model_name = self.model_name.replace(&#x27;baidu-&#x27;, &#x27;&#x27;)
            llm = BaiduLlm(model_name=model_name)
        elif self.model_name == &#x27;chatglm3&#x27;:
            endpoint_url = os.environ.get(&#x27;ENDPOINT_URL&#x27;)
            max_tokens = int(os.environ.get(&quot;MAX_TOKENS&quot;, 1024))
            top_p = float(os.environ.get(&quot;TOP_P&quot;, 0.9))

            if not endpoint_url:
                raise ValueError(&#x27;endpoint_url not right,url:{}&#x27;.format(endpoint_url))
            llm = ChatGLM3(
                endpoint_url=endpoint_url,
                max_tokens=max_tokens,
                top_p=top_p,
                prefix_messages=[]
            )
        else:
            raise ValueError(&quot;no llm to call&quot;)
        self.qa = RetrievalQA.from_chain_type(
            llm=llm,
            retriever=self.db.as_retriever(),
            verbose=True,
            chain_type_kwargs={&quot;prompt&quot;: qa_prompt}
        )

    def as_retrival_qa(self):
        return self.qa
```

RAG类[的实现的关](https://www.zhihu.com/search?q=%E6%A8%A1%E5%9E%8B%E8%B7%AF%E5%BE%84&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A695643553%7D)键点介绍 - qa_prompt=PromptTemplate.from_template(""): 使用langchain提供的prompt模板管理工具，配置上下文信息: {context} 和问题: {question} - self.model_n[ame =](https://www.zhihu.com/search?q=%E7%9F%A5%E8%AF%86%E9%97%AE%E7%AD%94&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A695643553%7D) os.environ.get("MODEL_NAME")：模型名或 模型路径 获取，这里计划将所有的与模型相关的配置均从环境变量获取的方式配置，后面会介绍如何统一管理环境变量的配置。 - model_init-Text2embedStore： 模型初始化，包括了embedding模型的初始化（如果是调用第三方的embedding接口则可以不初始化），这个定义了核心的类Text2embedStore，用于管理本地知识文档上传的解析、分割、向量化和持久化操作。 - model_init-大模型配置：这里根据环境变量读取的模型名称，初始化baidullm或chatglm3类，本项目是使用langchain来做 知识问答 的，所以这里定义的类要么直接使用langchain提供的，要么继承langchain的基类，并实现其几个核心的方法，后续介绍模型类的实现时会介绍。 - model_init-RetrievalQA：这里同样使用langchain的chains的检索类实现模型类，检索器和模板的封装，该类中实现了先调用检索，然后将检索结果和用户问题按prompt模板组装，最后调用大模型的整个流程。具体chains模块的介绍，可以看我的系列文章 langchain源码剖析系统文章 或系列视频 langchain源码剖析系统视频 - as_retrival_qa(): 将RetrievalQA()实例返回，方便在UI中调用。

更多相关内容，感兴趣的朋友可以同步关注B站(视频)：acedar-C大，知乎&公众号：aC大，也可加v进群交流：aa836304831

发布于 2024-05-01 22:57 ・IP 属地广东