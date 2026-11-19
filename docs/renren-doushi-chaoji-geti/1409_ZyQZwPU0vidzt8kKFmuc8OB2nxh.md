
# 2023-08-23重磅！GPT-3.5 Turbo推出微调功能，可以打造专属ChatGPT啦！

> 来源：[https://xuqiwei1986.feishu.cn/docx/Dbhyd3YQ3omFt4xMQcjcz0JJnOc](https://xuqiwei1986.feishu.cn/docx/Dbhyd3YQ3omFt4xMQcjcz0JJnOc)

> 🔗 原文链接：h[ttps://baijiahao.b](https://baijiahao.baidu.com/s?id=1774981780470751272&wfr=spider&for=pc)aidu.com/s?id=17...

重磅！GPT-3.5 Turbo推出微调功能，可以打造专属ChatGPT啦！

8月23日凌晨，OpenAI在官网宣布，推出GPT-3.5 Turbo微调功能并更新API，使企业、开发人员可以使用自己的数据，结合业务用例构建专属ChatGPT。GPT-4的微调功能将于今年秋天推出。

OpenAI表示，从早期测试数据来看，GPT-3.5 Turbo 的微调版本在某些特定任务上，其性能可以匹配甚至超越GPT-4 的基本功能。（详细说明文档：`https://platform.openai.com/docs/guides/fine-tuning`）

微调功能是目前企业应用大语言模型的主要方法，例如，法律领域的Spellbook、律商联讯、Litera、Casetext等，他们通过自己积累的海量法律数据在GPT-4模型上进行微调、预训练构建法律领域的专属ChatGPT，使其回答的内容更加聚焦、安全、准确。

![](img/5adaca9c1130937264c045ed4d21cbc2.png)

自GPT-3.5 Turbo发布以来，开发人员和企业希望能够开发自定义大模型，为用户创造独特、差异化的产品体验。现在，OpenAI终于开放了此功能，使得人人都能打造独一无二的专属ChatGPT。

## 什么是微调

大语言模型的微调（Fine-tuning）是一种在预训练模型的基础上，对特定任务进行深度训练的方法。

微调的基本思想是，先在大规模文本数据上预训练一个大型的语言模型，例如，GPT-3.5，然后，使用特定任务的数据集（如法律、医疗），进一步对模型进行训练，以适应特定的任务。在这个过程中，模型的参数会进行微小的调整，使其在特定业务场景上的性能更好。

例如，如果我们有一个预训练的GPT3.5模型，想让它在法律业务上的表现更好、更专业，可以用海量法律数据集对模型进行微调。

经过微调，模型学习到如何更好地解读、生成和预测法律问题。

## 微调用例

根据OpenAI的官方微调说明文档，列出了以下常见用例：

+ 设置风格、基调、格式等；
+ 提高输出的可靠性；
+ 纠正未能遵循复杂提示的问题；
+ 以特定方式处理多个边缘内容；
+ 改进可操纵性；
+ 执行难以在提示中阐明的新技能或任务。

例如，改进可操纵性，微调功能允许企业使模型更好地遵循指令，使输出简洁或始终以特定语言进行响应。例如，开发人员可以使用微调来确保模型，在提示使用德语时始终以德语进行响应。

除了提高性能之外，微调使企业能够缩短提示时间同时保证性能。GPT-3.5-Turbo微调还可以处理4k tokens，是OpenAI之前微调模型的两倍。

早期测试人员通过对模型本身的指令进行微调，将提示大小减少了高达 90%，从而加快了每个API调用的速度并降低了成本。

## 如何进行微调

### 1，准备数据

创建一组多样化的演示对话，类似于要求模型在实际输出中的对话。数据集中的每个示例都应该与OpenAI的聊天完成 API 格式相同的对话，特别是消息列表，其中每条消息都有角色、内容和可选名称。

![](img/d4dccfdf79573ecfa2b62ddc76c0a990.png)

### 2，上传文件

![](img/daa0dd26ffc756e9dd8e0a547307ad1f.png)

### 3，创建微调作业

使用 OpenAI SDK开始进行大规模数据训练、微调。训练模型可能需要几分钟或几小时，具体取决于模型和数据集大小。模型训练完成后，创建微调作业的用户将收到一封确认电子邮件。

![](img/a1c461b04dc68845e86ab190e97f1682.png)

除了创建微调作业外，开发者还可以列出现有作业、检索作业状态或取消作业。

![](img/27457718b9df2c060462c411abde8daf.png)

### 4，使用微调模型

微调作业完成后模型可以投入使用了。

![](img/85fd7b0fa707efd3cd9f83cc6e3cbae4.png)

在某些情况下，用户的微调模型可能需要几分钟才能准备好处理请求。如果对模型的请求超时或找不到模型名称，可能是因为模型仍在加载中，可在几分钟后重试。

![](img/8974ba6948aa3d02e66b9e40b9a13a91.png)

## 微调定价

微调分为初始训练成本和使用成本两大部分。

训练：0.008 美元/1K tokens。

使用输入：0.012 美元/1K tokens。

使用输出：0.016 美元/1K tokens。

例如，一个GPT-3.5 Turbo包含 100,000 个okens的训练文件并训练3个epoch的微调作业的预期成本为2.40美元。

本文来源：AIGC开放社区，原文标题：《重磅！GPT-3.5 Turbo推出微调功能，可以打造专属ChatGPT啦！》

举报/反馈

大家都在搜

[gp turbo ](https://baidu.com/s?word=gp%20turbo&rsv_dl=feed_landingpage_rs&from=1020853i&rsf=)[GPT模式 ](https://baidu.com/s?word=GPT%E6%A8%A1%E5%BC%8F&rsv_dl=feed_landingpage_rs&from=1020853i&rsf=)[gigabyte开启vt](https://baidu.com/s?word=%E6%8A%80%E5%98%89%E4%B8%BB%E6%9D%B板gpt%E8%AE%BE%E7%BD%AE&rsv_dl=feed_landingpage_rs&from=1020853i&rsf=)[虚拟化 技嘉主板gpt](https://baidu.com/s?word=gpt%20bios%E8%AE%BE%E7%BD%AE&rsv_dl=feed_landingpage_rs&from=1020853i&rsf=)[设置方法 g](https://baidu.com/s?word=GPT3%E4%BB%8B%E7%BB%8D&rsv_dl=feed_landingpage_rs&from=1020853i&rsf=)pt bios设置 GPT3介绍
