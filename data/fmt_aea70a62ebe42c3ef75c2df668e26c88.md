
# 2 | Standard & Few Shot Prompts

> 来源：[https://aigcreative.feishu.cn/docx/NMxIdv575ob1mHxY8KWcMRWens1](https://aigcreative.feishu.cn/docx/NMxIdv575ob1mHxY8KWcMRWens1)

Let's take a quick look at standard or regular prompts. We'll go through how prompting a chatbot works and then quickly move onto few shot prompts.

## Here's two regular prompt examples:

Regular Prompt

```
What's the highest mountain in the world?
```

Regular Prompt in Question and Answer format

```
Q: What's the highest mountain in the world?
A：
```

When you provide a regular prompt as an input to a large language model such as ChatGPT, it will generate a corresponding output or in prompt engineering terms, a completion. For example, this is illustrated below:

```
Q: What's the highest mountain in the world?
A: Mount Everest.
```

Now, there are also a few techniques you can employ to get better or more efficient outputs from a large language model such as Zero-shot, one-shot and few-shot prompting.

## Few Shot Prompts

Few-shot prompting refers to a technique that involves presenting a small number of examples, typically ranging from two to five, to a model, enabling it to quickly adapt to new examples of previously encountered objects.

### Two examples of few shot prompts:

Few Shot Prompt

```
What is the first colour of a rainbow?
Red

What is the second colour of a rainbow?
Yellow

What is the third colour of a rainbow?
```

### Few Shot Prompt in QA format

```
Q:What is the first colour of a rainbow?
A:Red

Q:What is the second colour of a rainbow?
A:Yellow

Q:What is the third colour of a rainbow?
A:
```

Breaking down your prompt into a 'few shot' prompt helps to structure and train your chosen large language model so it provides you with a better output. By providing it with previous examples, it is more likely to get the output you want.

https://aigcreative.feishu.cn/sync/K4Gvd9vUQsy9Oubwqaeclu0Fnbg
