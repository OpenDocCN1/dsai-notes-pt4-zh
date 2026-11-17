
# week1_part1_foundations

> 来源：[https://gotoaiworld.feishu.cn/docx/McMYdeJCOonBiLxuO6ncwW97nMb](https://gotoaiworld.feishu.cn/docx/McMYdeJCOonBiLxuO6ncwW97nMb)

## [Week 1, Part 1] Applied LLM Foundations and Real World Use Cases

[Jan](https://forms.gle/353sQMRvS951jDYu7) 15 2024 You can register here to receive course content and other resources

## ETMI5: Explain to Me in 5

In this part of the course, we delve into the intricacies of Large Language Models (LLMs). We start off by exploring the historical context and fundamental concepts of artificial intelligence (AI), machine learning (ML), neural networks (NNs), and generative AI (GenAI). We then examine the core attributes of LLMs, focusing on their scale, extensive training on diverse datasets, and the role of model parameters. Then we go over the types of challenges associated with using LLMs.

In the next section, we explore practical applications of LLMs across various domains, emphasizing their versatility in areas like content generation, language translation, text summarization, question answering etc. The section concludes with an analysis of the challenges encountered in deploying LLMs, covering essential aspects such as scalability, latency, monitoring etc.

In summary, this part of the course provides a practical and informative exploration of Large Language Models, offering insights into their evolution, functionality, applications, challenges, and real-world impact.

## History and Background

```
               Image Source: [https://medium.com/womenintechnology/ai-c3412c5aa0ac](https://medium.com/womenintechnology/ai-c3412c5aa0ac)
```

The terms mentioned in the image above have likely come up in conversations about ChatGPT. The visual representation offers a broad overview of how they fit into a hierarchy. AI is a comprehensive domain, where LLMs constitute a specific subdomain, and ChatGPT exemplifies an LLM in this context.

In summary, Artificial Intelligence (AI) is a branch of computer science that involves creating machines with human-like thinking and behavior. Machine Learning (ML), a subfield of AI, allows computers to learn patterns from data and make predictions without explicit programming. Neural Networks (NNs), a subset of ML, mimic the human brain's structure and are crucial in deep learning algorithms. Deep Learning (DL), a subset of NN, is effective for complex problem-solving, as seen in image recognition and language translation technologies. Generative AI (GenAI), a subset of DL, can create diverse content based on learned patterns. Large Language Models (LLMs), a form of GenAI, specialize in generating human-like text by learning from extensive textual data.

Generative AI and Large Language Models (LLMs) have revolutionized the field of artificial intelligence, allowing machines to create diverse content such as text, images, music, audio, and videos. Unlike discriminative models that classify, generative AI models generate new content by learning patterns and relationships from human-created datasets.

At the core of generative AI are foundation models which essentially refer to large AI models capable of multi-tasking, performing tasks like summarization, Q&A, and classification out-of-the-box. These models, like the popular one that everyone’s heard of-ChatGPT, can adapt to specific use cases with minimal training and generate content with minimal example data.

The training of generative AI often involves supervised learning, where the model is provided with human-created content and corresponding labels. By learning from this data, the model becomes proficient in generating content similar to the training set.

Generative AI is not a new concept. One notable example of early generative AI is the Markov chain, a statistical model introduced by Russian mathematician Andrey Markov in 1906. Markov models were initially used for tasks like next-word prediction, but their simplicity limited their ability to generate plausible text.

The landscape has significantly changed over the years with the advent of more powerful architectures and larger datasets. In 2014, generative adversarial networks (GANs) emerged, using two models working together—one generating output and the other discriminating real data from the generated output. This approach, exemplified by models like StyleGAN, significantly improved the realism of generated content.

A year later, diffusion models were introduced, refining their output iteratively to generate new data samples resembling the training dataset. This innovation, as seen in Stable Diffusion, contributed to the creation of realistic-looking images.

In 2017, Google introduced the transformer architecture, a breakthrough in natural language processing. Transformers encode each word as a token, generating an attention map that captures relationships between tokens. This attention to context enhances the model's ability to generate coherent text, exemplified by large language models like ChatGPT.

The generative AI boom owes its momentum not only to larger datasets but also to diverse research advances. These approaches, including GANs, diffusion models, and transformers, showcase the breadth of methods contributing to the exciting field of generative AI.

## Enter LLMs

The term "Large" in Large Language Models (LLMs) refers to the sheer scale of these models—both in terms of the size of their architecture and the vast amount of data they are trained on. The size matters because it allows them to capture more complex patterns and relationships within language. Popular LLMs like GPT-3, Gemini, Claude etc. have thousands of billion model parameters. In the context of machine learning, model parameters are like the knobs and switches that the algorithm tunes during training to make accurate predictions or generate meaningful outputs.

Now, let's break down what "Language Models" mean in this context. Language models are essentially algorithms or systems that are trained to understand and generate human-like text. They serve as a representation of how language works, learning from diverse datasets to predict what words or sequences of words are likely to come next in a given context.

The "Large" aspect amplifies their capabilities. Traditional language models, especially those from the past, were smaller in scale and couldn't capture the intricacies of language as effectively. With advancements in technology and the availability of massive computing power, we've been able to build much larger models. These Large Language Models, like ChatGPT, have billions of parameters, which are essentially the variables the model uses to make sense of language.

Take a look at the infographic from “Information is beautiful” below [to see](https://informationisbeautiful.net/visualizations/the-rise-of-generative-ai-large-language-models-llms-like-chatgpt/) how many parameters recent LLMs have. You can view the live visualization here

Image source: [https://informationisbeautiful.net/visualizations/the-rise-of-generative-ai-large-language-models-llms-like-chatgpt/](https://informationisbeautiful.net/visualizations/the-rise-of-generative-ai-large-language-models-llms-like-chatgpt/)

## Training LLMs

Training LLMs is a complex process that involves instructing the model to comprehend and produce human-like text. Here's a simplified breakdown of how LLM training works:

1. Providing Input Text:
   - LLMs are initially exposed to extensive text data, encompassing various sources such as books, articles, and websites.
   - The model's task during training is to predict the next word or token in a sequence based on the context provided. It learns patterns and relationships within the text data.

2. Optimizing Model Weights:
   - The model comprises different weights associated with its parameters, reflecting the significance of various features.
