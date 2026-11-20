
# 让Claude模仿任何写作风格。

> 来源：[https://my.feishu.cn/docx/ANB7dhQBBoa61Jx5f8mczCnfnif](https://my.feishu.cn/docx/ANB7dhQBBoa61Jx5f8mczCnfnif)

## 第 1 步：收集并复制一些写作样本。

## 第 2 步：打开Claude并使用此提示。

1. 先让大模型了解自己的角色和任务（模仿风格写作）；

2. 第二步让大模型持续吸收和学习用户上传的文章实例；

3. 第三步让大模型根据要求模仿风格并输出内容。

### 英文提示词

```
I&#x27;m going to provide you with my own written material, and your task will be to understand and mimic its style.

You&#x27;ll start this exercise by saying &quot;BEGIN.&quot; After, I&#x27;ll present an example text, to which you&#x27;ll respond, &quot;CONTINUE&quot;. The process will continue similarly with another piece of writing and then with further examples. I&#x27;ll give you unlimited examples. Your response will only be &quot;CONTINUE.&quot; You&#x27;re only permitted to change your response when I tell you &quot;FINISHED&quot;.

After this, you&#x27;ll explore and understand the tone, style, and characteristics of my writing based on the samples I&#x27;ve given. Finally, I&#x27;ll prompt you to craft a new piece of writing on a specified topic, emulating my distinctive writing style
```

### 中文版

```
我将为你提供我自己的书面材料，你的任务是理解并模仿其风格。

你将通过说&quot;开始&quot;来开始这个练习。之后，我会呈现一个示例文本，你要回应&quot;继续&quot;。这个过程将以类似的方式继续进行，包括另一段写作和更多的例子。我会给你无限的例子。你的回应只能是&quot;继续&quot;。只有当我说&quot;结束&quot;时，你才被允许改变你的回应。

在此之后，你将根据我给出的样本探索并理解我写作的语气、风格和特点。最后，我会提示你就指定的主题创作一篇新的文章，模仿我独特的写作风格。
```

## 第 3 步：向Claude提供你的写作样本。

提交提示词后，确保Claude说“`开始`”。

然后分别粘贴准备好的文章示例，并等待Claude的“`继续`”回复，然后再粘贴更多内容。

最少提交两次示例来训练，并且提供的越多，输出就越好。

## 第 4 步：完成后输入“`结束`”。

一旦你给 Claude 提供了足够的写作内容（建议至少 5 篇），请输入“`结束`”让 Claude 知道你已经完成了。

## 第 5 步：根据要求输出对应风格的内容

提交了5次示例内容后，输入新的写作任务，让Claude来执行。

内容参考来源：https://linux.do/t/topic/128683/1

---

## 作者简介：

我是文森策，一个10年+品牌策划人，AI提示词&提效玩家。

现阶段热衷于AI辅助内容营销探索，欢迎交流分享AI辅助创作思考/技法/实用工作流，一起用AI打开更多创作可能。

⭐ **提交反馈与交流合作请直接戳我**👇
