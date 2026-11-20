
# 支持多种社区上的在线体验

> 来源：[https://docs.feishu.cn/docx/WgybdswGcoI0hLxwdmucBGAznPd](https://docs.feishu.cn/docx/WgybdswGcoI0hLxwdmucBGAznPd)

项目地址：https://github.com/thomas-yanxin/LangChain-ChatGLM-Webui

HUggingFace社区在线体验：https://huggingface.co/spaces/thomas-yanxin/LangChain-ChatLLM

另外也支持ModelScope魔搭社区、飞桨AIStudio社区等在线体验

1. 下载项目

```
git clone https://github.com/thomas-yanxin/LangChain-ChatGLM-Webui.git
```

1. 进入目录

```
cd LangChain-ChatGLM-Webui
```

1. 安装所需的包

```
pip install -r requirements.txt
pip install gradio==3.10
```

1. 修改`config.py`

```
init_llm = &quot;ChatGLM-6B&quot;

llm_model_dict = {
    &quot;chatglm&quot;: {
        &quot;ChatGLM-6B&quot;: &quot;/data/sim_chatgpt/chatglm-6b&quot;,
```

1. 修改`app.py`文件，将`launch`函数中的`share`设置为`True`，`inbrowser`设置为`True`

执行`webui.py`文件

```
python webui.py
```

![](img/bca565818537fc3b85779857391bfc2c.png)

显存占用约13G
