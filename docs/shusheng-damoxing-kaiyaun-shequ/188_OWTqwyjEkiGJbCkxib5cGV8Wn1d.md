
# 提示词工程课程

> 来源：[https://aicarrier.feishu.cn/docx/WIWHdAgGtoeEt5xSagAcI5E3nnf](https://aicarrier.feishu.cn/docx/WIWHdAgGtoeEt5xSagAcI5E3nnf)

## 引入

## 总体结构介绍

## 模块构成

## 内部元素设计

## 编写实战

## 通关要求

近期研究发现，LLM在对比数字时表现不佳，例如认为`13.8<13.11`：

![](img/61dcbbb50de6da0d302fe169f68bb30f.png)

要求：利用`LangGPT`优化提示词，使LLM输出正确结果。

### 5.1 能力提高

参考huggingface Open LLM Leaderboard (V1) 的任务设置进行任务划分，共6个任务：ARC Challenge、Hellaswag、MMLU、TruthfulQA、Winogrande和GSM8k。

+ AI2 Reasoning Challenge  (25-shot) - 一套小学级别的科学问题；
+ [HellaSwag ](https://arxiv.org/abs/1905.07830) (10-shot) - 常识推理测试，这对人类来说很容易，但对 SOTA 模型来说却具有挑战性。
+ [MMLU ](https://arxiv.org/abs/2009.03300)  (5-shot) - 是一项用于测量文本模型多任务准确性的测试。该测试涵盖 57 项任务，包括初等数学、美国历史、计算机科学、法律等。
+ [TruthfulQA ](https://arxiv.org/abs/2109.07958) (0-shot) - 是一项用于衡量模型复制网上常见虚假信息倾向的测试。
+ [Winogrande ](https://arxiv.org/abs/1907.10641) (5-shot) - 一个更具有对抗性，难度更高的常识推理测试。
+ [GSM8k ](https://arxiv.org/abs/2110.14168) (5-shot) - 多样化的小学数学单词问题，以衡量模型解决多步骤数学推理问题的能力。

通过抽签的方式分配任务，每人为其中1个任务编写提示词，基于`LangGPT`格式。使用`OpenCompass`进行评测，LLM为`internlm2.5-chat-7b`，编写的提示词作为系统提示。

达标要求：不使用系统提示的`internlm2.5-chat-7b`的任务表现性能作为`baseline`，提交的系统提示词要指导LLM表现出比`baseline`更高的性能。

提交文件：编写的6个任务的提示词；评测的结果文件（会根据提交的提示词进行复核，复核失败的组认定不通过）
