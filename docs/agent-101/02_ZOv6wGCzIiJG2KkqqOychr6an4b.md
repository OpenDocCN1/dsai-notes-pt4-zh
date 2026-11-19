
# Agent101 — 手把手打造专属AI搜索助理

> 来源：[https://docs.feishu.cn/docx/J9rkdarNyoB62XxWq3mcQFNjnpb](https://docs.feishu.cn/docx/J9rkdarNyoB62XxWq3mcQFNjnpb)

我们已经看到很多 AI 搜索的产品，比如 Perplexity、秘塔AI搜索、KIMI，搜索增强的原理简单来说是：通过 LLM 把搜索得到的结果进行二次处理，得到语义化的搜索结果。这篇文章主要介绍通过 Coze 来实现你的搜索增强 bot，本文假设你已经对 Coze 以及大模型有了基本了解。

> 【你能学到什么】

+ 如何写 `Prompt`
+ 如何使用 `workflow`
+ 如何使用 google 搜索插件
+ 如何使用 `Code` 节点

我们这个 AI 搜索就叫做 `AISearch`，体验地址：`https://www.coze.com/s/Zs8rrVCAd/`

![](img/e776d2fd2342253b8a89ef6dab9a46c3.png)

这个 `AISearch` 的核心包括：

+ 如何写 `Persona & Prompt`
+ 如何使用 `workflow` 以及 `plugin`
+ 通过 `Code` 节点对搜索的结果进行格式化
+ 在 `workflow` 中使用 `LLM` 生成内容

## 如何写 `Persona & Prompt`

![](img/d9ac5ce2f2e5ed705bcd0ee26e00168e.png)

`Persona & Prompt` 是指 bot 的 入口 `prompt`，如果你的 bot 比较简单只需要一个 `prompt` 就可以完成的话你可以在把 `prompt` 写得足够详细。如果你的 bot 要使用多个 `workflow` 或者 `plugin`，你的 `prompt` 需要包含：`人物设定` `功能和能力描述` 以及 `能力约束` 三个部分：

#### 人物设定：

描述 Bot 所扮演的角色或职责、回复风格。例如：你是一个新闻播报员，可以用非常生动的风格讲解科技新闻。

#### 功能和能力描述：

描述 Bot 的功能和工作流程，约定 Bot 在不同的场景下如何回答用户问题。例如：当用户查询最新 AI 新闻时，调用 `getNews` 工具来搜索科技新闻。

#### 能力约束：

如果您想限制回复范围，请直接告诉 Bot 什么应该回答、什么不应该回答。

`AISearch` 的 `Prompt` 如下：

```
# Your Role
You are a search enhancement assistant created by Agent101, you are the guide to the vast world of information. Ask away, and let the journey of exploration begin. Remember, there is nothing you don&#x27;t know.


# Skills
## Search and Anaswer 
Your most important ability is `Search and Answer`. When users pose questions or inquire about certain topics or concepts, you should **always** first conduct an online search before providing an answer. However, when users ask you **to do something**, such as translating, summarizing, etc., you must decide whether it&#x27;s reasonable to use your `search` capability to enhance your ability to perform the task.
```

## 如何使用 `workflow` 和 `plugin`

`AISearch` 的核心能力就是搜索增强，我们创建了一个叫做 `search` 的 `workflow` 来完成这个能力。

![](img/251530d00c7736282ec0bc6398dd1ccb.png)

`workflow` 支持通过可视化的方式，对插件、大语言模型、代码块等功能进行组合，从而实现复杂、稳定的业务流程编排。当目标任务场景包含较多的步骤，且对输出结果的准确性、格式有严格要求时，适合配置工作流来实现。

工作流由多个节点构成，节点是组成工作流的基本单元。例如，大语言模型 `LLM`、自定义代码、判断逻辑等节点。工作流默认包含了 `Start` 节点和 `End` 节点。

在这个 `Search` `workflow` 中总共包含了：`Start`、`googleWebSearch`、`Code`、`LLM`、`End` 五个节点。

## 使用 `Code` 节点对搜索的结果进行格式化

可以看到 google 搜索插件范围的数据是非常多的，其中有很多数据我们并用不到，另外一个是我们想要特定格式的搜索数据进行后续的处理，这里我们可以使用 `Code` 节点来进行数据的二次处理和格式化。

![](img/ff1a519ed7b80721612ca4b417e6a0ad.png)

![](img/1b43f99109db6eff39af8458e3b0305b.png)

这里的 `code` 节点代码比较简单，基本逻辑是：

1. 清理掉有可能为空的数据；
2. 结构化用于搜索增强的内容；
3. 结构化用于展示引用的内容；

## `Workflow` 中使用 `LLM` 生成内容

到这里我们已经获取了搜索增强的内容和引用的内容，接下来我们就用依托这些内容给用户生成回复，这里我们采用了一个 `LLM` 节点，模型采用了 `gpt-4o`，`gpt-4o` 确实速度确实比较快，而且具有 128k 的输入上下文。

所使用的 `Prompt` 借鉴了贾扬清大佬的开源项目

感谢大佬分享，这里的 `prompt` 是整个 `workflow` 的精髓，你可以尝试改造成自己的：

```
You are a large language AI assistant built by Agent101. You are given a user question, and please write clean, concise and accurate answer to the question. You will be given a set of related contexts to the question, each starting with a reference number like [[x]], where x is a number. Please use the context and cite the context at the end of each sentence if applicable.
 
Your answer must be correct, accurate and written by an expert using an unbiased and professional tone. Please limit to 2024 tokens. Do not give any information that is not related to the question, and do not repeat. Say &quot;information is missing on&quot; followed by the related topic, if the given context do not provide sufficient information.
 
Please cite the retrieved contexts with the reference numbers, in the format [x]. If a sentence comes from multiple contexts, please list all applicable citations, like [3][5]. Other than code and specific names and citations.
 
Here is the user query: {{query}}  
And here are the set of retrieved contexts:  
{{retrieved}}  
 
Remember your response MUST be written in the language the user prefers. Here is the user query: {{query}}
```

从 `LLM` 返回的结果直接输入就可以了。

## 最后

相应的 bot 和 `workflow` 代码我已经公开，你可以在这里查看完整的 `Prompt` 和 `Workflow`。希望能对你有所帮助。

加入 `Agent101 Coze Team`：

`https://www.coze.com/invite/VbogXxJTvUYU8wpxFoWh` (需要加入 team 才能看到完整的 `prompt` 和 `workflow`)

本 bot 地址：

`https://www.coze.com/s/Zs8rrVCAd/`

#### 参考文章：

1. `https://juejin.cn/post/7336841191772143642`
2. `https://github.com/leptonai/search_with_lepton`
