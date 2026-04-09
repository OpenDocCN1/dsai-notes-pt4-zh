# Prompt Principle 提示原则

> 来源：[https://vxc3hj17dym.feishu.cn/docx/MH4zdpzVWoeyurxEQ3Uc7SZ2nTd](https://vxc3hj17dym.feishu.cn/docx/MH4zdpzVWoeyurxEQ3Uc7SZ2nTd)

> If you prefer more concise answers, no need to be polite with LLM so there is no need to add phrases like “please", "if you don't mind", "thank you", "I would like to", etc., and get straight to the point.

如果你喜欢更简洁的回答，没有必要礼貌地使用LLM，所以没有必要添加诸如“请”，“如果你不介意”，“谢谢”，“我想”等短语，直奔主题。

Integrate the intended audience in the prompt, e.g., the audience is an expert in the field.

在提示中整合目标听众，例如，听众是该领域的专家。

Break down complex tasks into a sequence of simpler prompts in an interactive conversation.

将复杂任务分解为一系列更简单的提示，在互动对话中进行。

Employ affirmative directives such as "do," while steering clear of negative language like "don't".

使用肯定的指令，如“做”，同时避免使用如“不要”这样的否定语言。

When you need clarity or a deeper understanding of a topic, idea, or any piece of information, utilize the following prompts:

+   Explain [insert specific topic] in simple terms.

+   Explain to me like I'm 11 years old.

+   Explain to me as if I'm a beginner in [field].

+   Explain to me as if I'm an expert in [field].

+   "Write the [essay/text/paragraph] using simple English like you're explaining something to a 5-year-old".

当你需要清晰理解或深入了解一个主题、想法或任何信息时，使用以下提示：

+   用简单的术语解释[插入具体主题]。

+   把我当成11岁的孩子一样向我解释。

+   把我当[领域]初学者一样向我解释。

+   把我当[领域]专家一样向我解释。

+   “使用简单的英语写[文章/文本/段落]，就像你在向一个5岁的孩子解释某事”。

Add "I'm going to tip $xxx for a better solution".

添加“我将为更好的解决方案提供$xxx的小费”。

Implement example-driven prompting (Use few-shot prompting).

实施示例驱动的提示（使用少量示例提示）。

When formatting your prompt, start with '###Instruction###', followed by either '###Example###' or '###Question###' if relevant. Subsequently, present your content. Use one or more line breaks to separate instructions, examples, questions, context, and input data.

在格式化提示时，首先以’###Instruction###‘开始，（如果相关），接着是’###Example###‘或’###Question###'。然后呈现您的内容。使用一个或多个换行符来分隔指令、示例、问题、上下文和输入数据。

Incorporate the following phrases: "Your task is" and "You MUST".

使用以下短语：“你的任务是”和“你必须”。

Incorporate the following phrases: "You will be penalized".

使用以下短语：“如果你不这样做，你将受到惩罚”。

Use the phrase "Answer a question given in a natural, human-like manner" in your prompts.

在提示中使用“以自然、类人的方式回答问题”的短语。

Use Leading words like writing "think step by step".

使用引导词，如“一步步思考”。

Add to your prompt the following phrase "Ensure that your answer is unbiased and avoids relying on stereotypes."

在提示中添加以下短语“确保你的回答没有偏见，并避免依赖刻板印象”。

Allow the model to elicit precise details and requirements from you by asking you questions until it has enough information to provide the needed output (for example, "From now on, I would like you to ask me questions to...").

允许模型通过向你提问直到它拥有足够的信息来提供所需的输出来引出精确的细节和要求（例如，“从现在开始，我想让你问我问题，直到你拥有足够的信息来创建一个个性化的健身计划”）。

To inquire about a specific topic or idea or any information and you want to test your understanding, you can use the following phrase: "Teach me the [Any theorem / topic / rule name] and include a test at the end, but don't give me the answers and then tell me if I got the answer right when I respond".

为了询问特定的主题或想法或任何信息，并且你想测试你的理解，你可以使用以下短语：“教我[任何定理/主题/规则名称]，并在最后包含一个测试，但不要给我答案，然后告诉我，当我回答后，我的答案是否正确”。

Assign a role to the Large Language Models (LLMs).

为大型语言模型（LLM）分配角色。

Use Delimiters.

使用分隔符。

Repeat a specific word or phrase multiple times within a prompt.

在提示中多次重复特定的单词或短语。

Combine Chain-of-thought (Cot) with few-Shot prompts.

将Chain-of-thought（Cot）与少量示例提示结合使用。

Use output primers, which involve concluding your prompt with the beginning of the desired output. Utilize output primers by ending your prompt with the start of the anticipated response.

使用输出启动器，这涉及到以期望的输出的开始结束你的提示。利用输出启动器，通过期望的响应来结束你的提示。

To write an [essay / text paragraph / article] or any type of text that should be detailed: "Write a detailed [essay / text / paragraph] for me on [topic] in detail by adding all the information necessary".

为了写一个[文章/文本段落/文章]或任何类型的文本，应该是详细的：“为我详细写一个关于[主题]的详细[文章/文本/段落]，并添加所有必要的信息”。

To correct / change specific text without changing its style: "Try to revise every paragraph sent by users. You should only improve the user’s grammar and vocabulary and make sure it sounds natural. You should maintain the original writing style, ensuring that a formal paragraph remains formal".

为了纠正/更改特定的文本而不改变其风格：“尝试修改用户发送的每个段落。你只能改善用户的语法和词汇，并确保它听起来自然。你应该保持原文的写作风格，确保正式的段落保持正式。”

When you have a complex coding prompt that may be in different files: "From now and on whenever you generate code that spans more than one file, generate a [programming language] script that can be run to automatically create the specified files or make changes to existing files to insert the generated code. [your question]."

当您有一个可能分布在多个文件中的复杂编码提示时：“从现在开始，每当你生成跨越多个文件的代码时，生成一个[编程语言]脚本，可以运行来自动创建指定的文件，或对现有文件进行更改以插入生成的代码。[你的问题]。”

When you want to initiate or continue a text using specific words, phrases, or sentences, utilize the following prompt:

+   I'm providing you with the beginning [song lyrics / story / paragraph / essay...]: [Insert lyrics / words / sentence]. Finish it based on the words provided. Keep the flow consistent.

当您想要使用特定的单词、短语或句子来启动或继续文本时，请使用以下提示：

我为您提供了一首[歌词/故事/段落/文章…]的开头：[插入歌词/单词/句子]。根据提供的词语来完成它。保持流畅一致。

Clearly state the requirements that the model must follow in order to produce content, in the form of keywords, regulations, hints, or instructions.

明确指出模型必须遵循的要求，以产生内容，形式为关键词、规则、提示或指令。

To write any text, such as an essay or paragraph, that is intended to be similar to a provided sample, include the following instructions:

+   "Use the same language based on the provided paragraph[ / title / text / essay / answer]".

为了写任何文本，例如文章或段落，旨在与提供的示例相似，请包含以下说明：

“根据提供的段落[ /标题/文本/文章/答案]使用相同的语言”。