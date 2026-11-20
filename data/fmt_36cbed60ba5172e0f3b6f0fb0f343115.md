
# 微学习25 | DeepSeek+Ollama：在英特尔显卡部署

> 来源：[https://datawhaler.feishu.cn/docx/ZdftdoDtIo3NMmxp5nJcygfgnhf](https://datawhaler.feishu.cn/docx/ZdftdoDtIo3NMmxp5nJcygfgnhf)

> 本内容由 同济子豪兄投稿，

> 原内容标题：游戏显卡B580也能本地部署DeepSeek R1？英特尔优化版ollama实测

<https://player.bilibili.com/player.html?bvid=1KZXeY2EGj>

在英特尔显卡上，本地部署DeepSeek R1大模型。创造出你自己的端侧知识库问答助手、AI客服、简历筛选、代码辅助，甚至决策智能体。

只要是英特尔的显卡，都可以按这个教程操作。包括集成显卡（例如酷睿ultra处理器的AI PC、哪吒开发板）和独立显卡（例如B580）。

英特尔优化版`ollama`背后用了`IPEX-LLM`这个加速库，方便在英特尔的CPU GPU NPU上推理大语言模型。

## 本地部署开源大模型有八大好处

除了隐私和数据安全之外，不用花钱买`token`，端侧无限调用。不用害怕卡脖子，真正自主可控。不用担心版本变，甚至账号被封。高自由度定制化，随便微调蒸馏。

## 详细步骤

1.在`设备管理器-显示适配器`中查看你的显卡是不是英特尔显卡

2.更新英特尔显卡驱动

3.从魔搭社区下载英特尔优化版`ollama`

4.启动`ollama`服务，执行`ollama run`命令

5.下载DeepSeek R1模型，和模型对话

6.在聊天机器人接入`ollama` API
