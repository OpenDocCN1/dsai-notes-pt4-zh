# 语音生文｜Whisper

> 来源：[https://geekbang.feishu.cn/docx/OKQtdlW6YoEzc4xdp9NcvxVWnQg](https://geekbang.feishu.cn/docx/OKQtdlW6YoEzc4xdp9NcvxVWnQg)

# 基本介绍

## Whisper是什么

**Whisper**** 是 Open**A**I 的一种自动**最先进的语音识别系统，它已经接受了 680,000 **小时从网络收集**的多语言和多任务监督数据的训练。这个庞大而多样化的数据集提高了对口音、背景噪音和技术语言的鲁棒性。此外，它还支持多种语言的转录，以及将这些语言翻译成英语。与 DALLE-2 和 GPT-3 不同，Whisper 是一种免费的开源模型。OpenAI 发布了模型和代码，作为构建利用语音识别的有用应用程序的基础。

## Whisper的日常用途

whisper的核心功能语音识别，对于学生党和工作党来说，可以帮助我们更快捷地将会议、讲座、课堂录音整理成文字稿；对于影视爱好者，可以将无字幕的资源自动生成字幕，不用再苦苦等待各大字幕组的字幕资源；对于外语口语学习者，使用whisper翻译你的发音练习录音，可以很好地检验你的口语发音水平。

当然，我们知道一些商业公司提供语音识别服务，但是基本都是联网运行，个人隐私安全总是有隐患，而whisper完全不同，whisper完全在本地运行，无需联网，充分保障了个人隐私，且whisper识别准确率相当高。拿我自己录的一段音频举例，5min长度400多字，使用whisper的medium模式识别，只错了两个英文单词，那两个英文单词还是因为发音问题识别错误。

# 相关学习

Whisper与ChatGPT联手，轻松实现音频转录文本总结

windows11下whisper的安装使用

请查收使用OpenAI的Whisper进行语音识别的攻略