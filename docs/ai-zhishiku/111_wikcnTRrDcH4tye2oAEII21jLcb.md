
# 【AutoGPT】安装教程

> 来源：[https://my.feishu.cn/docx/BHXNdBFBxosj1RxUqcocfw9zncf](https://my.feishu.cn/docx/BHXNdBFBxosj1RxUqcocfw9zncf)

## AutoGPT 官方安装流程

[github.com/Torantulino/Auto-GPT](https://github.com/Torantulino/Auto-GPT)

AgentGPT 体验地址：`agentgpt.reworkd.ai`

## AutoGPT 本地安装流程

1. 安装 Git for Windows，地址：`gitforwindows.org`
2. 安装 Python 3.11.3，地址：`www.python.org`
3. 在 `D` 盘新建 `autogpt` 文件夹
4. 进入 `github.com/Torantulino/Auto-GPT`
5. 在 `D:\autogpt` 文件夹上方对话框，调出 `CMD`，输入 `git clone https://github.com/Torantulino/Auto-GPT.git`
6. 在 `D:\autogpt\autogpt` 文件夹，调出 `CMD`，输入 `pip install -r requirements.txt`
7. 在 `D:\autogpt\autogpt` 文件夹中找到 `.env.template` 文件。
8. 进入 `Pinecone.io`，注册账号，生成一组 API Key。
9. 把 `your pinecone api key` 替换为刚才生成的 API Key。
10. 进入 `Pinecone.io`，复制 Environment 代码。把 `your pinecone region` 替换为 Environment 代码。
11. 进入 `platform.openai.com/account/api-keys`，获取 API Key。
12. 点击 `Create new secret key`，复制 API 值，替换原有的 `openai api key`。
13. 保存文本文件，并把 `.env.template` 文件替换为 `.env`。
14. 在 `D:\autogpt\autogpt` 运行 `python scripts/main.py`，执行 AutoGPT。
15. 在 `D:\autogpt\autogpt` 运行 `python scripts/main.py --speak`，可以播放语音。
