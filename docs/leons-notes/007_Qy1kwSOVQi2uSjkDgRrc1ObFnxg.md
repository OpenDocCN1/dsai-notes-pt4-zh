
# Notes｜Deep Dive into LLMs like ChatGPT｜Andrej Karpathy

> 来源：`https://leonzhang.feishu.cn/docx/P3xSdH8FqoU7zXxbXSJc9KMsnQP`

> This is a general audience deep dive into the Large Language Model (LLM) AI technology that powers ChatGPT and related products. It is covers the full training stack of how the models are developed, along with mental models of how to think about their "psychology", and how to get the best use them in practical applications. I have one "Intro to LLMs" video already from ~year ago, but that is just a re-recording of a random talk, so I wanted to loop around and do a lot more comprehensive version.

> 这是一篇面向普通观众的深入介绍，介绍了支持 ChatGPT 和相关产品的大型语言模型 (LLM) AI 技术。它涵盖了模型开发的完整训练堆栈，以及如何思考其“心理学”的心理模型，以及如何在实际应用中充分利用它们。我已经有了一个大约一年前的“LLMs 简介”视频，但那只是随机谈话的重新录制，所以我想循环播放并制作一个更全面的版本。

> Instructor 讲师

> Andrej was a founding member at OpenAI (2015) and then Sr. Director of AI at Tesla (2017-2022), and is now a founder at Eureka Labs, which is building an AI-native school. His goal in this video is to raise knowledge and understanding of the state of the art in AI, and empower people to effectively use the latest and greatest in their work.

> Andrej 是 OpenAI 的创始成员（2015 年），后来担任特斯拉的 AI 高级总监（2017-2022 年），现在是 Eureka Labs 的创始人，该实验室正在建立一所 AI 原生学校。他在这段视频中的目标是提高人们对 AI 最新技术的了解和理解，并让人们能够在工作中有效地使用最新和最好的技术。

## 章节目录

> [Chapters](https://www.youtube.com/watch?v=7xTGNNLPyMI)

- 00:00:00 introduction
- 00:01:00 pretraining data (internet)
- 00:07:47 tokenization
- 00:14:27 neural network I/O
- 00:20:11 neural network internals
- 00:26:01 inference
- 00:31:09 GPT-2: training and inference
- 00:42:52 Llama 3.1 base model inference
- 00:59:23 pretraining to post-training
- 01:01:06 post-training data (conversations)
- 01:20:32 hallucinations, tool use, knowledge/working memory
- 01:41:46 knowledge of self
- 01:46:56 models need tokens to think
- 02:01:11 tokenization revisited: models struggle with spelling
- 02:04:53 jagged intelligence
- 02:07:28 supervised finetuning to reinforcement learning
- 02:14:42 reinforcement learning
- 02:27:47 DeepSeek-R1
- 02:42:07 AlphaGo
- 02:48:26 reinforcement learning from human feedback (RLHF)
- 03:09:39 preview of things to come
- 03:15:15 keeping track of LLMs
- 03:18:34 where to find LLMs
- 03:21:46 grand summary

进度：`https://www.bilibili.com/video/BV16cNEeXEer?t=1462.9`

## 扩展内容

1. 【安德烈·卡帕西：深入探索像ChatGPT这样的大语言模型｜Andrej Karpathy】`https://www.bilibili.com/video/BV16cNEeXEer/`

1. 原视频链接：`https://www.youtube.com/watch?v=7xTGNNLPyMI`

1. 视频中提到的链接（Links）

+   ChatGPT `https://chatgpt.com/`

+   [FineWeb (pretraining dataset)](https://huggingface.co/spaces/HuggingFaceFW/blogpost-fineweb-v1): `https://huggingface.co/spaces/Hugging...`

+   Tiktokenizer: `https://tiktokenizer.vercel.app/`

+   Transformer Neural Net 3D visualizer: `https://bbycroft.net/llm`

+   [llm.c Let's Reproduce GPT-2](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbW9GcEwzV1pkWVBvTk01Z2ZUQi1Ib09LeGpoQXxBQ3Jtc0ttMlZqd2Yyd3lPT2tRUC1ZRlczc0F1cHNKc2JKUG5yM2tKQ09pM2poVlJyWFdVREltMXhYTjhNMVNsSkV5X2w5V1pDeWwyV0FTcWp4VW83M0pSZk1FU09mUTg2MkpUX2dWVkxZTTZ2QzFuVGVveXgyVQ&q=https%3A%2F%2Fgithub.com%2Fkarpathy%2Fllm.c%2Fdiscussions%2F677&v=7xTGNNLPyMI) `https://github.com/karpathy/llm.c/dis...`

+   Llama 3 paper from Meta: `https://arxiv.org/abs/2407.21783`

+   Hyperbolic, for inference of base model: `https://app.hyperbolic.xyz/`

+   InstructGPT paper on SFT: `https://arxiv.org/abs/2203.02155`

+   [HuggingFace inference playground](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqa2JhRFpqMWJ2TWQ1dm9GNGpFNTdsMktTN0hWUXxBQ3Jtc0ttRTV5aHFESlVRZGZoUjFDbktjSzhtLXoySUE3V3NEOEZPZmtydFduQ3I1R3pENTBPOHAyWE9PQUZQUHc0akF4MXB6MldUbDFVTE5wRGZMSjlOenpweGN5dmhmQ3JFamZBTTRoSndvWVZTdzc1M241aw&q=https%3A%2F%2Fhuggingface.co%2Fspaces%2Fhuggingface%2Finference-playground&v=7xTGNNLPyMI) `https://huggingface.co/spaces/hugging...`

+   DeepSeek-R1 paper: `https://arxiv.org/abs/2501.12948`

+   TogetherAI Playground for open model inference: `https://api.together.xyz/playground`

+   [AlphaGo paper (PDF)](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqa1lsMnBacUlVbXM0TzY5UVZNYW00aFZpSkJ4Z3xBQ3Jtc0ttNFpkeDhxT3pObDZPTVVRWU9rRmFSQzExdWhKRjlSZkJ4X1dFQ3RrZ3NYRW5kMGZBaWtkN2UycFNXNFdiV2ZpbElXaUNrYXpkcGJUa0gyS0t1S1NvbHNhS2I4OGhEQ0FKb2xiaVFMS2F1SVBMbkE1RQ&q=https%3A%2F%2Fdiscovery.ucl.ac.uk%2Fid%2Feprint%2F10045895%2F1%2Fagz_unformatted_nature.pdf&v=7xTGNNLPyMI) `https://discovery.ucl.ac.uk/id/eprint...`

+   [AlphaGo Move 37 video](https://www.youtube.com/watch?v=HT-UZkiOLv8) Lee Sedol vs AlphaGo Move 37 reaction...

+   LM Arena for model rankings: `https://lmarena.ai/`

+   AI News Newsletter: `https://buttondown.com/ainews`

+   LMStudio for local inference `https://lmstudio.ai/`

+   The visualization UI I was using in the video: `https://excalidraw.com/`

+   [The specific file of Excalidraw we built up](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbmVNSVJMenlvVTdqNlJsRzdLY0RCc29CREVEQXxBQ3Jtc0ttQUZCd1JrNVI3Z3JKbkRmYVZKbWZtNEstcngxa3NYOFROUWxVd253Nm9TWmxiR2NXXzFaSTFtaVc4LTNua3VkTmhuXzJ5djEyVGhTN2lwN1ZPLUNqbHA5SWxZd2FhSjlMM1NuLS03b1d2SkV6SVNjNA&q=https%3A%2F%2Fdrive.google.com%2Ffile%2Fd%2F1EZh5hNDzxMMy05uLhVryk061QYQGTxiN%2Fview%3Fusp%3Dsharing&v=7xTGNNLPyMI) `https://drive.google.com/file/d/1EZh5...`

+   Discord channel for Eureka Labs and this video: `/ discord` `https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbndBY20tdFpPMHlOQ0RYeTl0Vk5xb0h0WDFMd3xBQ3Jtc0trUzBEcDFNODl1MTM2NzY1MDdJTDRIejFWU1lGbGYtZUg0ZGIxeUJRMDdjcmlEQnFNODFQT1JtX21TMTJLNzRCUTd6SHhxcFJSWkt1aTdPZmFEdWhvVkJLVnFVQVBIdEV5VmJrRUo5UjE5UjZ1MGVyOA&q=https%3A%2F%2Fdiscord.gg%2F3zy8kqD9Cp&v=7xTGNNLPyMI`
