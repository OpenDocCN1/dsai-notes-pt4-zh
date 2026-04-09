# 1.Intro to prompting 提示简介

> 来源：[https://waytoagi.feishu.cn/docx/Ao68dR0AuoGwwYxvTFnc5LpJnMg](https://waytoagi.feishu.cn/docx/Ao68dR0AuoGwwYxvTFnc5LpJnMg)

Claude is trained to be a helpful, honest, and harmless assistant. It is used to speaking in dialogue, and you can instruct it in regular natural language requests as if you were making requests of a human.

克劳德经过训练，成为一个乐于助人、诚实可靠、无害的助手。它习惯于进行对话，并且您可以用常规自然语言请求指示它，就像您在向一个人提出请求一样。

The quality of the

instructions you give Claude can have a large effect on the quality of its outputs, especially for complex tasks.

你给克劳德的指示质量会对其输出的质量产生很大影响，特别是对于复杂任务。

# What is a prompt? 什么是提示？

A prompt is the text that you give Claude to elicit, or "prompt," a relevant output. A prompt is usually in the form of a question or instructions. For example:

提示是您给 Claude 的文本，用于引发相关输出。提示通常以问题或指示的形式出现。例如：

The text that Claude responds with is called a "response", sometimes also called an "output" or "completion."

Claude 回答的文本被称为“响应”，有时也被称为“输出”或“完成”。

## Claude works by sequence prediction Claude 通过序列预测工作

Claude is a conversational assistant, based on a large language model (LLM). The model uses all the text that you have sent it (your prompt) and all the text it has generated so far to predict the next token that would be most helpful.

Claude 是一个基于大型语言模型（LLM）的对话助手。该模型使用您发送给它的所有文本（您的提示）以及它迄今为止生成的所有文本来预测下一个最有帮助的标记。

This means that Claude constructs its responses one set of characters at a time, in order. It cannot go back and edit its response after it has written it unless you give it a chance to do so in a subsequent prompt.

这意味着 Claude 逐个字符地构建其响应。它在写完响应后不能返回并编辑响应，除非您在随后的提示中给它这样的机会。

Claude can also only see (and make predictions on) what is in its context window. It can't remember previous conversations unless you put them in the prompt, and it can't open links.

Claude 也只能看到（并对其上下文窗口中的内容进行预测）。除非将它们放在提示中，否则它无法记住以前的对话，也无法打开链接。

### Prompt length 提示长度

The maximum prompt length that Claude can see is its context window. See our model comparison table for a list of context window sizes per model.

Claude 可以看到的最大提示长度是其上下文窗口。请查看我们的模型比较表，了解每个模型的上下文窗口大小列表。