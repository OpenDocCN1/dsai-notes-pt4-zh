# 手把手教你实现大模型RAG[3]-UI实现

> 来源：[https://agijuejin.feishu.cn/docx/Kd5Md1CcBoRUiMxDn3ncI1Ernwc](https://agijuejin.feishu.cn/docx/Kd5Md1CcBoRUiMxDn3ncI1Ernwc)

> 作者：aC大

> 原文：[https://zhuanlan.z](https://zhuanlan.zhihu.com/p/692447827)hihu.com/p/692447...

### 前言

《手把手教你实现大模型RAG》系列文章分享的是从0到1基于langchain实现大模型结合本地知识库的问答系统。主要内容包括：

+   1. 大模型应用技术介绍

+   2. RAG原理及流程的系统介绍

+   3. 基于langchain手把手教你实现大模型RAG的整个流程

+   4. 使用第三方的大模型服务实现RAG

+   5. 使用开源大模型实现标准的模型服务部署，并基于部署的大模型实现RAG

+   6. 使用gradio教你快速实现RAG的体验UI，能够快速测试RAG的效果

+   7. 支持多种本地文档格式的上传和切割，并实现文本向量化

+   [8. 实现](https://www.zhihu.com/search?q=%E6%96%87%E6%9C%AC%E5%90%91%E9%87%8F&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A692447827%7D)基于langchain的 文本向量 的检索服务手把手教你实现大模型RAG

本系列 [文章会 ](https://www.zhihu.com/search?q=%E6%96%87%E7%AB%A0%E4%BC%9A&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A692447827%7D)逐一分享上述内容，同时作者也出了该系列的视频课程，见 B站《手把手教你实现大模型RAG-acedar讲师》 , 文章和视频结合学习效果更佳。

### UI功能介绍

实现了大模型的RAG之后，需要体验其效果就需要有一个可以快速体验效果的UI，为此本文使用gradio实现UI。其实现的UI页面如下：

![](img/e031194f5677c93d297a2dcafbb4b198.png)

基于gradio实现的UI包括5个部分

+   RAG文本对话tab，这里主要是方便后续扩展到多模型的问答，将纯文本的问答作为一个tab，基于图像的多模态问答可以新建一个tab集成到该项目下。

+   模型选择，本项目支持调用第三方平台的模型，比如百度的文心一言，也支持自己部署开源或微调大模型的调用，所以支持了模型的选择。

+   聊天历史展示，展示过往了历史聊天

+   问答输入，通过该 [文本框 ](https://www.zhihu.com/search?q=%E6%96%87%E6%9C%AC%E6%A1%86&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A692447827%7D)[输入问题](https://www.zhihu.com/search?q=%E7%9F%A5%E8%AF%86%E5%BA%93&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A692447827%7D)，后台会根据问题先从本地 知识库 中检索，然后调用大模型综合回答问题。

+   本地文件上传，本地知识库可以通过项目启动初始化，也支持用户聊天过程中上传。

### UI代码实现

使用gradio实现的UI代码如下：

```
import os
import time
import gradio as gr

def add_text(history, text):
    print(&quot;add_text&quot;)
    history = history + [(text, None)]
    return history, gr.update(value=&#x27;&#x27;, interactive=True)

def upload_file(history, file):
    print(&quot;upload file&quot;)
    &quot;&quot;&quot;
        todo:
        1. 上传文档
        2. 文档切割
        3. 文档向量化
        4. 检索
    &quot;&quot;&quot;
    history = history + [((file.name, ), None)]
    print(&quot;upload_file,&quot;, history)
    return history


def chat(history):
    print(&quot;chat&quot;)
    print(&quot;upload_file,&quot;, history)

    msg = history[-1][0]
    &quot;&quot;&quot;
    # todo:
        1. 根据用户的提问，调用检索服务，获取与问题相关本地知识库信息
        2. 调用大模型，对信息进行整合
        3. 解析大模型结果，并返回
    &quot;&quot;&quot;
    ans = &#x27;&#x27;

    history[-1][1] = &#x27;&#x27;
    for ch in ans:
        history[-1][1] += ch
        time.sleep(0.01)
        yield history[-1:]


with gr.Blocks() as demo:
    with gr.Tab(&quot;RAG文本对话&quot;):
        model_list = [&quot;文心一言大模型&quot;, &quot;chatglm3本地部署大模型&quot;]
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

### gradio中使用到的重要函数介绍

+   gr.Blocks()。开启一个新的页面（也称为session），在该session下的层级关系(缩进)设计会直接体现到UI页面上。

+   gr.Tab("RAG文本对话"), 在新session新增一个tab，如果需要再次新增tab，只需要在其并列的位置重复调用。 如：

```
with gr.Blocks() as demo:
    with gr.Tab(&quot;RAG文本对话&quot;):
        pass
    with gr.Tab(&quot;多模态对话&quot;):
        pass
```

+   [gr.Chatbot ](https://www.zhihu.com/search?q=gr.Chatbot&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A692447827%7D)([], elem_id='chatbot')，是创建一个聊天模板，第一个参数是聊天的历史信息，其他参数是配置聊天框的设置，可以根据实际情况修改。 在处理聊天信息时，创建时返回的chatbot的值，存储的就是聊天记录信息，根据添加的格式解析就可以。

+   gr.Row()。在当前的session下新增一行(这里的session为gr.Tab("RAG文本对话"))，因为是使用 with gr.row() 创建，所以可以在该session下增加其他功能。 这里新增了一个文本框 gr.Textbox() 和一个文件上传按钮 gr.UploadButton

+   t[xt.su](https://www.zhihu.com/search?q=%E9%80%BB%E8%BE%91%E5%87%BD%E6%95%B0&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A692447827%7D)[bmit/](https://www.zhihu.com/search?q=%E5%8A%A8%E4%BD%9C%E5%87%BD%E6%95%B0&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A692447827%7D)btn.upload。这两个调用是对文本框和文件上传按钮的预处理的实现，第一个参数是触发动作的 逻辑函数 ，第二个参数是一个list，表示触发 动作函数 的输入参数，第三个参数是一个list，表示触发动作函数的输出。 .then()是预处理之后的逻辑实现，第一个参数是逻辑实现函数，第二个参数是输入，第三个参数是输出。

+   demo.launch() 是启动gradio实现的UI。

更多相关内容，感兴趣的朋友可以同步关注B站(视频)：acedar-C大，知乎&👸号：aC大，也可加v进群交流：aa836304831

发布于 2024-04-26 15:31 ・IP 属地广东