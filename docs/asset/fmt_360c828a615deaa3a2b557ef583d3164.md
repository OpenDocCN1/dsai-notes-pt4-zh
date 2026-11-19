
# 模型下载和安装

> 来源：[https://ai.feishu.cn/docx/BksUdaEemo2Irzxvlw2cJZZHnNd](https://ai.feishu.cn/docx/BksUdaEemo2Irzxvlw2cJZZHnNd)

## 免安装在线尝鲜

> 你将有一种办法免费，无需安装的尝鲜我们的模型，它是：

+   [官方体验地址](https://modelscope.cn/studios/ZhipuAI/Cogvlm2-llama3-chinese-chat-Demo/summary)

> 在这个demo中，你将能体验到完整的 CogVLM2 模型能力。

## 源码安装

下载模型

+   [下载](https://github.com/git-lfs/git-lfs?utm_source=gitlfs_site&utm_medium=installation_link&utm_campaign=gitlfs#installing)模型文件前请先确保`git lfs`命令已安装，安装教程请参考这里。
+   模型文件已上传至 Huggingface, Modelsope, WiseModel 三个平台，用户可以快速安装模型。
+   从 Huggingface 下载模型

```
# 图像理解英语模型
git clone https://huggingface.co/THUDM/cogvlm2-llama3-chat-19B.git
# 图像理解中英双语模型
git clone https://huggingface.co/THUDM/cogvlm2-llama3-chinese-chat-19B.git
# 图像理解英语模型 - INT4版本
git clone https://huggingface.co/THUDM/cogvlm2-llama3-chat-19B-int4.git
# 图像理解中英双语模型 - INT4版本
git clone https://huggingface.co/THUDM/cogvlm2-llama3-chinese-chat-19B-int4.git
# 视觉理解英语模型 - 单轮对话
git clone https://huggingface.co/THUDM/cogvlm2-video-llama3-chat
# 视觉理解英语模型 - 基座模型
git clone https://huggingface.co/THUDM/cogvlm2-video-llama3-base
```

+   从 ModelScope 魔搭社区 下载图像理解模型

```
# 图像理解英语模型
git clone https://modelscope.cn/models/ZhipuAI/cogvlm2-llama3-chat-19B.git
# 图像理解中英双语模型
git clone https://modelscope.cn/models/ZhipuAI/cogvlm2-llama3-chinese-chat-19B.git
# 图像理解英语模型 - INT4版本
git clone https://modelscope.cn/models/ZhipuAI/cogvlm2-llama3-chat-19B-int4.git
# 图像理解中英双语模型 - INT4版本
git clone https://modelscope.cn/models/ZhipuAI/cogvlm2-llama3-chinese-chat-19B-int4.git
# 视觉理解英语模型 - 单轮对话
git clone https://modelscope.cn/models/ZhipuAI/cogvlm2-video-llama3-chat
# 视觉理解英语模型 - 基座模型
git clone https://modelscope.cn/models/ZhipuAI/cogvlm2-video-llama3-base
```

+   下载示例代码

```
git clone https://github.com/THUDM/CogVLM2
```

## 最低配置要求

下表展现了模型推理和微调的最低要求，请根据你的情况进行选择

| Model Name | Type | 19B Series Model | Remarks |
| --- | --- | --- | --- |
| CogVLM2 | BF16 / FP16 Inference | 42GB | Tested with 2K dialogue text |
| CogVLM2 | Int4 Inference | 16GB | Tested with 2K dialogue text |
| CogVLM2 | BF16 Lora Tuning (Freeze Vision Expert Part) | 57GB | Training text length is 2K |
| CogVLM2 | BF16 Lora Tuning (With Vision Expert Part) | > 80GB | Single GPU cannot tune |
| CogVLM2-Video | BF16 / FP16 Inference | 28GB | Tested with 0.2K dialogue text |
