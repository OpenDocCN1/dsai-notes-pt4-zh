# PDF2Audio：超强开源 AI 工具，打开了阅读 PDF 的新方式！

> 来源：[https://agispace.feishu.cn/docx/EbmtdsHNiolozLxZxENcz5J9n1d](https://agispace.feishu.cn/docx/EbmtdsHNiolozLxZxENcz5J9n1d)

> 原创：王平安

> 公众号：AI工具派

> 原文链接：https://mp.weixin.qq.com/s/vqzN33YXc_ZLVDzjoH1pYA

在**数字化时代，信息的**获取和传播方式正经历着前所未有的变革。随着人工智能技术的飞速发展，我们迎来了一个全新的工具 —— PDF2Audio，它将彻底改变我们阅读和理解 PDF 文件的方式。

有了它，我们不再需要盯着屏幕，而是让信息以声音的形式流淌进你的耳朵，无论是在通勤的路上、健身时，还是任何需要解放双手的场合。

![](img/6ada451791e0ae04cea911dec3e4f7b7.png)

## 近期热文

+   Y Combinator 投资的开源 Cursor 替代品来了！

+   [**超强 AI 背景移除：在线批量处理，支持 WebG**](https://mp.weixin.qq.com/s?__biz=MzA5NDMwMTU3OA==&mid=2247485533&idx=1&sn=0eb299b25ce8a88ca884ab3228c72dea&scene=21#wechat_redirect)PU 加速，无需登录，可无限次免费使用！

+   [**爆火的与文档聊天的 RAG 开源工具，支持 Gra**](https://mp.weixin.qq.com/s?__biz=MzA5NDMwMTU3OA==&mid=2247485570&idx=1&sn=e6d84f1379a127f7ce1d865163098f81&scene=21#wechat_redirect)phRAG 、多模态问答和多种复杂推理！

## PDF2Audio 主要特点

+   支持处理多个 PDF 文件

+   支持选择不同的指令模板（播客、讲座、摘要等）

+   自定义文本生成和音频模型

+   支持为演讲者选择不同的声音

+   对文稿的编辑和对模型的具体反馈进行草稿迭代改进

## PDF2Audio 操作界面

PDF2Audio 的操作界面很简洁，操作步骤如下：

1.  上传一个或多个 PDF 文件

1.  选择所需的指令模板

![](img/941dab7efd22a8c35bbe16de7105c04f.png)

1.  如有需要，可自定义指令模板

1.  点击 “生成音频” 按钮以创建音频内容

![](img/3ba9d32d80ffd36f3522712e5b30e6f1.png)

## PDF2Audio 快速上手

### 在线使用

你可以通过 Huggingface 和 Colab 两个平台来在线使用 PDF2Audio 的功能。

lamm-mit/PDF2Audio

> https://huggingface.co/spaces/lamm-mit/PDF2Audio

**PDF2Audio.ipynb**

> https://colab.research.google.com/github/lamm-mit/PDF2Audio/blob/main/PDF2Audio.ipynb

### 本地运行

1.  克隆仓库

```
git clone https://github.com/lamm-mit/PDF2Audio.git
cd PDF2Audio
```

1.  创建 Conda 环境

```
conda create -n pdf2audio python=3.9
```

1.  激活已创建的 Conda 环境

```
conda activate pdf2audio
```

1.  安装依赖

```
pip install -r requirements.txt
```

1.  创建 .env 文件，配置 OpenAI Key

```
OPENAI_API_KEY=your_api_key_here
```

1.  运行 app.py 启动应用

```
python app.py
```

> https://github.com/lamm-mit/PDF2Audio