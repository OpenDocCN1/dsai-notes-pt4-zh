
# 🖌Stable Diffusion学习指南

> 来源：[https://chengchaungai.feishu.cn/docx/PoCjdPJs3o4NgzxtSScc0WzBnwg](https://chengchaungai.feishu.cn/docx/PoCjdPJs3o4NgzxtSScc0WzBnwg)

![](img/c3c7f4fa37ceae3a4e9c8e60b96b45b5.png)

————选择人工智能，链接无限未来————

本文编辑：JUGO 👸号：橙创AI

![](img/c61a1e5a5d8cf83904fc87d4dc4546ad.png)

![](img/8341af787cb2dbf8176702f78b9214de.png)

**进入👉 [🍊橙创AI知识库（首页）](https://ix9q8jjj4ug.feishu.cn/wiki/DOsXwmRDaibJVUkDSHKcYbhmn0f) 获取更多AIGC内容👈**

> 查阅、整理和输出教程属实[不易](https://ki6j1b0d92h.feishu.cn/wiki/wikcnOTLuqHXX6ZJ1oWEBGbd9zf?from=from_copylink)，觉得这篇教程对你有所帮助的话，不妨分享给更多需要的人呗😊

## 子页面目录

## 入门文档

| 2023-5-18 | [✍喂饭级教程：自主搭建Stable Diffusion和GUI](https://chengchaungai.feishu.cn/wiki/SV5ZwDNdeiyhihkRH4NcC443nIc) |

| --- | --- |

## 交流群

| 加入交流群，🛰️请加JUGO737Y |

| --- |

## 基本介绍

Stable Diffusion是2022年发布的深度学习文本到图像生成模型。它主要用于根据文本的描述产生详细图像，尽管它也可以应用于其他任务，如内补绘制、外补绘制，以及在提示词（英语）指导下产生图生图的翻译。

Stable Diffusion许多开源用户界面实现之一的示例，此用户界面已汉化。

## 模型介绍

[精通Stable Diffusion画图，理解LoRA、Dreambooth、Hypernetworks四大模型差异](https://id8ex69uxy.feishu.cn/wiki/wikcn8NdCwKOqFzG9G0gg0KXggb)

### LoRA

Low-Rank Adaptation of Large Language Models 是微软研究员引入的一项新技术，主要用于处理大模型微调的问题。目前超过数十亿以上参数的具有强能力的大模型 (例如 GPT-3) 通常在为了适应其下游任务的微调中会呈现出巨大开销。LoRA 建议冻结预训练模型的权重并在每个 `Transformer` 块中注入可训练层 (秩-分解矩阵)。因为不需要为大多数模型权重计算梯度，所以大大减少了需要训练参数的数量并且降低了 GPU 的内存要求。

简单来说，lora模型为我们提供了更便捷更自由的微调模型，能够使我们在底模的基础上，进一步指定整体风格、指定人脸等等，而且lora模型非常的小，大部分都只有几十MB，非常的方便！

## 安装模型

在网上下载了模型了，如何使用模型呢？下面介绍一下各个文件的应用方法：

### 基础模型/大模型

这是训练过后的模型包，文件名类似`***.safetensors`，而且文件体积特别大，一般都超过1G，可以通过这些来判断是否为基础模型。此模型安装方法就是将这个文件放在`models`下面的`sable-diffusion`目录下面：

![](img/6ebcd192c802311a977edb45cf2e3bd8.png)

## 相关文档

> 关注👸号，加入AI实战交流群，掌握全面专业的行业资讯、工具分享、变现指南和实战教程，开启你的AI智慧之旅👇👇

联系🛰️JUGO737Y（备注来意）

![](img/ae8741d19a988efee6c62cdc6ffce774.png)

👸号：橙创AI

![](img/5cbde2af958667b24bc8b70336810138.png)

群聊二维码：

![](img/044a7f25ed3fb166813b0f8507fbf2fb.png)
