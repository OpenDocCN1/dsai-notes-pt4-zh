
# 《如何向ChatGPT提问并获得高质量的答案》

> 来源：[https://diwfpqo78m.feishu.cn/docx/HzmRdo2BhoBuzBxtAaScf0Vhnub](https://diwfpqo78m.feishu.cn/docx/HzmRdo2BhoBuzBxtAaScf0Vhnub)

## 简介

> 非常高兴您阅读我的新书《如何向ChatGPT提问并获得高质量的答案——提示技术的完整指南》。
>
> 本书是一本综合性的指南，目的是帮助您理解和利用各种提示技术，以便从ChatGPT中获得高质量的答案。
>
> 我们将探索如何用不同的提示技术去完成不同的目的。
>
> ChatGPT 是目前最先进的、能够生成类似人类文本的语言模型。
>
> 然而，了解向ChatGPT提问的正确方式，以获得我们所期望的高质量答案，是至关重要的。
>
> 这就是本书的目的：无论你是一个普通人、研究员、开发者，或者是仅仅想把ChatGPT当成自己工作的个人助理，这本书都适合你。
>
> 书中使用通俗易懂的语言解释，并且辅以实例和提问技巧的公式。
>
> 通过这本书，你将学会如何使用提示技巧来控制ChatGPT的输出，让其按照你的需求生成文本。
>
> 在本书中，我还提供了一些如何综合利用不同提示技巧，以达到特定目的的例子。
>
> 希望这本书能给你丰富的信息，希望你在阅读时，能像我写书时那样开心。

## 第一章：提示工程技术简介

### 什么是提示工程？

提示工程是创建提示、要求或指示的过程，用来引导ChatGPT等语言模型的输出。它允许用户控制模型的输出，生成符合他们特定需求的文本。

ChatGPT是一种最先进的语言模型，能够生成类似人类的文本。它建立在transformer架构上，这使它能够处理大量的数据并生成高质量的文本。

为了从ChatGPT获得最好的结果，了解如何正确使用提示模型是很重要的。

提示允许用户控制模型的输出，生成相关、准确和高质量的文本。

在使用ChatGPT时，了解它的能力和限制是很重要的。

该模型能够生成类似人类的文本，但如果没有适当的引导，它输出的内容，可能不是我们所期望的。

这就是提示工程的用武之地：通过提供清晰而具体的说明，您可以指导模型输出，确保它是相关的。

提示公式是提示的具体格式，它一般由3个要素组成:

- 任务：对模型生成内容的清晰、简洁的陈述。
- 说明：模型生成文本时应遵循的指令。
- 角色：模型在生成文本时应承担的角色。

在本书中，我们将探讨可用于ChatGPT的各种提示工程技巧。我们将讨论不同类型的提示，以及如何使用这些提示来实现你想要的特定目标。

## 第二章：说明提示技术（Instructions Prompt Technique）

现在，让我们开始探索“说明提示技术”，以及如何用它来从ChatGPT生成高质量的文本。

说明提示技术是一种指导ChatGPT输出的方法，它为模型提供具体的指令。这种技术在确保输出内容的相关性、和高质量方面，非常有用。

要使用说明提示技术，你需要为模型提供一个清晰简明的任务，和可以遵循的具体指令。

举个例子，假如你要生成客服的回答。首先要提供一个任务，如“生成客户咨询的回复（generate responses to customer inquiries）”，以及说明：回答应该是专业的并提供准确的信息（responses should be professional and provide accurate information）

提示（Prompt）公式：“按照这些指示生成[任务]：[说明]”

### 举例

#### 生成客服回复

+ 任务：生成对客户咨询的回复（Generate responses to customer inquiries）
+ 说明：回答应该是专业的并提供准确的信息（responses should be professional and provide accurate information）
+ 提示（Prompt）公式：“生成对客户咨询的回复：回答应该是专业的并提供准确的信息”
+ （Generate professional and accurate responses to customer inquiries following these instructions: The responses should be professional and provide accurate information）

#### 生成一份法律文件

+ 任务：生成一份法律文件
+ 说明：该文件应符合相关法律和法规的规定
+ 提示（Prompt）公式：“按照这些指令，生成一份符合相关法律和法规的法律文件：该文件应符合相关法律和法规。”
+ （Generate a legal document that is compliant with relevant laws and regulations following these instructions: The document should be in compliance with relevant laws and regulations）

在使用指令提示技术时，重点是：指令应该是清晰、具体的。

这将会确保输出内容具有相关性、和高质量。指令提示技术可以和下一章中将解释的“角色提示”和“种子词提示”结合起来，提高ChatGPT的输出质量。

## 第三章：角色提示（role prompting technique）

角色提示技术（role prompting technique），是通过为模型提供特定角色来引导ChatGPT输出的一种方法。这种技术对于生成针对特定环境或受众的文本很有用。

要使用角色提示技术，您需要为模型提供一个明确而具体的角色。

例如，如果你正在生成客户服务回应，你将提供一个角色，如“客户服务代表”。

提示公式：“生成[任务]作为一个[角色]”

### 举例

#### 生成客户服务回复

+ 任务：生成对客户咨询的回复（Generate responses to customer inquiries）
+ 角色：客服
+ 提示公式：“作为客服，生成对客户咨询的答复。”
+ （Generate responses to customer inquiries as a customer service representative）

#### 生成一份法律文件

+ 任务：生成一份法律文件
+ 角色：律师
+ 提示公式：“作为律师生成一份法律文件。”
+ （Generate a legal document as a lawyer）

使用带有指令提示和种子词提示的角色提示技术将增强ChatGPT的输出质量。

下面是一个如何将指令提示、角色提示和种子词提示技术相结合的示例：

+ 任务：为新智能手机生成产品描述。
+ 指令：该描述应具有信息性、说服力，并强调智能手机的独特功能。
+ 角色：营销代表
+ 种子词：“创新”
+ 提示公式：“作为营销代表，生成一个信息量大，有说服力的产品描述，突出新智能手机的创新功能。该智能手机具有以下特点[插入你的特点]”
+ （As a marketing representative, generate an informative, persuasive product description that highlights the innovative features of the new smartphone. The smartphone has the following features [insert your features]）

在这个例子中，指令提示被用来确保产品描述具有信息性和说服力，角色提示用于确保以营销代表的角度编写描述，种子词提示用于确保描述侧重于智能手机的创新功能。

## 第四章：标准提示（Standard Prompts）

标准提示是引导ChatGPT输出的一个简单方法，它提供了一个具体的任务让模型完成。

例如，如果你想生成一篇新闻的摘要，你会提供一个任务，如“总结这篇新闻”。

提示公式：“生成[任务]”（Generate a [task]）

### 举例

#### 生成新闻文章摘要

+ 任务：总结这篇新闻文章
+ 提示公式：“生成这篇新闻文章的摘要”
+ （Generate a summary of this news article）

#### 生成产品评论

+ 任务：撰写有关新智能手机的评论
+ 提示公式：“生成对这款新智能手机的评论”
+ （Generate a review of this new smartphone）

此外，标准提示可以与其他技术相结合，如角色提示和种子词提示，以增强ChatGPT的输出质量。

下面是一个如何将指令提示、角色提示和种子词提示技术相结合的示例：

+ 任务：为新笔记本电脑生成产品评论
+ 说明：评论应该是客观的，信息丰富的，并突出笔记本电脑的独特功能
+ 角色：技术专家
+ 种子词：“ 强大的”
+ 提示公式：“作为一名技术专家，生成一份客观且信息丰富的产品评论，突出新笔记本电脑的强大功能。”
+ (As a tech expert, generate an objective and informative product review that highlights the powerful features of the new laptop)

在此示例中，使用标准提示技术来确保模型生成产品评论，角色提示技术用于确保评论是从技术专家的角度撰写的，使用种子词提示技术来确保评论集中在笔记本电脑的强大功能上。

## 第五章：零、单个和小样本提示（Zero, One and Few Shot Prompting）

零提示、单个提示和小样本提示是用于从ChatGPT中生成文本的技术，只有极少或没有示例可以参考。这些技术通常用于下列情况：当前任务的可用数据有限、任务是全新的、任务定义不明确。

当没有可用于任务的范例时，使用零样本提示技术。向模型提供一个普通的任务，它会根据对任务的理解生成文本。

当任务只有一个范例可用时，可以使用单样本提示技术。提供了一个范例给模型，模型根据对该范例的理解生成文本。

当可用于任务的范例数量有限时，使用小样本提示技术。提供了少量范例给模型，模型根据对该范例的理解生成文本。

提示公式：“基于[数量]的例子生成文本（Generate text based on [number] examples）”

### 举例

#### 为一个新产品生成产品描述，没有可用的例子。

+ 任务：为新智能手机生成产品描述。
+ 提示公式：“为这个新的智能手表生成一个产品描述，没有范例”
+ （Generate a product description for this new smartwatch with zero examples）

#### 为这个产品生成产品比较，只有一个范例可用。（Generating a product comparison with one example available）

+ 任务：将一款新的智能手机与最新的iPhone进行比较
+ 提示公式：“生成这个新智能手机的产品比较，有一个例子（最新的iPhone）”
+ （Generate a product comparison of this new smartphone with one example (latest iPhone)）

#### 生成一个产品评论，可用的例子很少。

+ 任务：写一篇新电子阅读器的评论
+ 提示公式：“用几个例子（其他 3 个电子阅读器）生成对这个新电子阅读器的评论”
+ （Generate a review of this new e-reader with few examples (3 other e-readers)）

这些技术可用于：根据模型对任务或所提供范例的理解来生成文本。

## 第六章：“让我们思考这个”提示（"Let’s think about this" prompt）

“让我们思考这个”提示是一种用于鼓励ChatGPT生成反思性、沉思性文本的技术。这种技术对于写作散文，诗歌或创造性写作等任务很有用。

### 使用方法：Let's think about this：主题

### 举例

#### 生成一篇反思性文章

+ 任务：写一篇关于个人成长主题的反思性文章
+ 提示公式：“让我们思考这个：个人成长”
+ （"Let’s think about this: personal growth）

#### 生成一首诗

+ 任务：写一首关于季节变化的诗
+ 提示公式：“让我们想想这个：不断变化的季节”
+ （Let’s think about this: the changing seasons）

此提示要求就特定主题或想法进行对话或讨论。演讲者邀请ChatGPT就手头的主题进行对话。

该模型提供了一个提示，作为对话或文本生成的起点。

然后，该模型使用其训练数据和算法来生成与提示相关的响应。该技术允许ChatGPT基于提供的提示生成上下文适当且连贯的文本。

要在ChatGPT中使用“让我们思考这个”技术，您可以按照以下步骤操作：

1. 确定您要讨论的主题或想法。
2. 制定一个提示，清楚地说明主题或想法，并开始对话或文本生成。
3. 在提示前面加上“让我们思考”或“让我们讨论”，表明您正在发起对话或讨论。

以下是使用此技术的一些提示示例：

+ 提示：“让我们思考一下气候变化对农业的影响”
+ 提示：“让我们讨论一下人工智能的现状”
+ 提示：“让我们谈谈远程工作的好处和缺点”

您还可以添加一个开放式问题、语句或一段文本，希望模型继续或构建。

提供提示后，模型将使用其训练数据和算法生成与提示相关的响应，并以连贯的方式继续对话。

这个独特的提示，帮助ChatGPT以不同的视角和角度给出答案，从而产生更具动态性和信息性的段落。

使用提示的步骤很简单，易于遵循，它可以真正改变你的写作。自己试试看

## 第七章：自我一致性提示（Self-Consistency Prompt）

自我一致性提示是一种技术，用于确保ChatGPT的输出与提供的输入一致。这种技术对于诸如事实核查、数据验证或文本生成中的一致性检查等任务很有用。

自我一致性提示的提示公式是输入文本后，说明“请确保以下文本是自我一致的（Please ensure the following text is self-consistent）”。

或者，可以提示模型生成与提供的输入一致的文本。

### 提示示例及其公式

#### 示例1：文本生成

+ 任务：生成产品评论
+ 指令：评论应与输入中提供的产品信息一致
+ 提示公式：“生成与以下产品信息[插入产品信息]一致的产品评论”
+ （Generate a product review that is consistent with the following product information [insert product information]）

#### 示例2：文本摘要

+ 任务：总结这篇新闻文章
+ 指令：摘要应与本条所提供的信息保持一致
+ 提示公式：“以符合所提供信息的方式，总结以下新闻文章[插入新闻文章]”
+ （Summarize the following news article in a way that is consistent with the information provided [insert news article]）

#### 示例3：文本完成（Text Completion）

+ 任务：写一个句子
+ 指令：完成的句子，应与输入中提供的背景相一致
+ 提示公式：“以符合所提供上下文的方式完成以下句子[插入句子]” （Complete the following sentence in a way that is consistent with the context provided [insert sentence]）

#### 示例4

##### 1. 事实核查

+ 任务：检查某篇新闻文章的一致性
+ 输入文本：“这篇文章说这个城市的人口是500万，但后来，它说人口是700万。”
+ 提示公式：“请确保下面的文字是自洽的。（Please ensure the following text is self-consistent）文章说该城市的人口是500万，但后来又说人口是700万。”

##### 2. 数据验证

+ 任务：检查给定数据集中的一致性
+ 输入文本：“数据显示，7月份的平均气温为30度，但最低气温记录为20度。”
+ 提示公式：“请确保下面的文字是自洽的（Please ensure the following text is self-consistent）：数据显示，7月份的平均气温为30度，但最低气温记录为20度。”

## 第8章：种子词提示（Seed-word Prompt）

种子词提示是一种技术，通过为ChatGPT提供特定的种子词或短语，来控制ChatGPT的输出。

种子词提示的提示公式是：“请根据以下种子词生成文本”的指令后跟着种子词或短语。

### 举例

#### 文本生成

+ 任务：生成一个关于龙的故事
+ 种子词：“龙”
+ 提示公式：“请根据以下种子词生成文本（Please generate text based on the following seed-word）:龙”

#### 语言翻译

+ 任务：将句子从英语翻译成西班牙语
+ 种子词：“您好”
+ 提示公式：“请根据以下种子词生成文本（Please generate text based on the following seed-word）:您好”

这种技术允许模型生成与种子词相关的文本并对其进行扩展。
