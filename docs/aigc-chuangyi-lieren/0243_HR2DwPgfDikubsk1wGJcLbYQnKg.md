# 6 | Zero Shot Chain of Thought

> 来源：[https://aigcreative.feishu.cn/docx/XBOSdU7UMoR8SFxVzBUc7AkfnJd](https://aigcreative.feishu.cn/docx/XBOSdU7UMoR8SFxVzBUc7AkfnJd)

### What is Zero Shot CoT?

Zero Shot Chain of Thought (Zero-shot-CoT) is a new prompting technique for generating more accurate answers to questions.

It involves appending the words "Let's think step by step." to the end of a question, which helps language models generate a chain of thought that answers the question.

The full Zero-shot-CoT process involves two separate prompts: the first generates a chain of thought, while the second extracts the answer from the chain of thought. Take a look at the first image in order to understand how the first prompt provides the 'chain of thought'. In the second image below, you can see the answer being extracted.

Zero-shot-CoT is effective in improving results on arithmetic, commonsense, and symbolic reasoning tasks, especially when obtaining few shot examples for CoT prompting is difficult.

Kojima et al. experimented with different Zero-shot-CoT prompts and found that "Let's think step by step" was the most effective for their chosen tasks.

<https://aigcreative.feishu.cn/sync/K4Gvd9vUQsy9Oubwqaeclu0Fnbg>