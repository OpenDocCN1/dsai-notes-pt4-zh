
# LLM 工具使用、评估与 Prompt 设计指南

将 [工具使用 ](https://www.zhihu.com/search?q=%E5%B7%A5%E5%85%B7%E4%BD%BF%E7%94%A8&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A654041855%7D)API封装给LLM调用、ChatGPT的插件等都是工具的形式，验证LLM工作操作的能力一般会使用一些推理任务去评估。随着工具的增多，LLM有限的`context`导致其无法很好的利用这些工具反馈的信息，现在的做法是将这些信息存储为`embedding`使用。

## 基准和经验性评估

+ Comprehensive Evaluation Benchmarks
+ MMLU
+ BIG-bench、BIG-bench-Lite、BIG-bench hard
+ HELM
+ Human-level test benchmarks：AGIEval、MMCU、M3KE、C-Eval、Xiezhi
+ Empirical Ability Evaluation

论文作者通过在闭源大模型（ChatGPT, Claude, Davinci003 and Davinci002）和开源大模型（LLaMA 7B、Pythia 7B和12B、Falcon 7B，经过`instruction-tuned`的Vicuna 7B、Alpaca 7B、ChatGLM 6B）上做了一些实验评估，得到了一些结论

+ 闭源大模型相比开源大模型，效果更好，尤其是ChatGPT
+ ChatGPT 和 Davinci003 在与环境互动和工具操作任务方面表现更佳
+ 所有模型在难度很大的推理任务中都表现不佳
+ `instruction-tuned` model效果要优于base model
+ 这些小型开源模型在数学推理、与环境互动和工具操作任务方面表现不佳
+ 在`Human Alignment`任务上，开源模型的表现方差较大
+ 作为最新发布的型号，Falcon-7B 性能不俗，尤其是在语言生成任务方面

推荐阅读：

+ LLM Evaluation 如何评估一个大模型？
+ [A Survey on Evaluation of L](https://link.zhihu.com/?target=https%3A//github.com/MLGroupJLU/LLM-eval-survey)arge Language Models

## 一些Leaderboard

+ [MMLU](https://link.zhihu.com/?target=https%3A//paperswithcode.com/dataset/mmlu%23%3A~%3Atext%3DMMLU%2520%2528Massive%2520Multitask%2520Language%2520Understanding%2529%2520is%2520a%2520new%2Cevaluating%2520models%2520exclusively%2520in%2520zero-shot%2520and%2520few-shot%2520settings.)
+ [Chatbot Arena](https://link.zhihu.com/?target=https%3A//huggingface.co/spaces/lmsys/chatbot-arena-leaderboard)
+ [BIG-bench](https://link.zhihu.com/?target=https%3A//github.com/google/BIG-bench)
+ Open LLM Leaderboard
+ [SuperCLUE](https://link.zhihu.com/?target=https%3A//cluebenchmarks.com/superclue.html)
+ [C-Eval](https://link.zhihu.com/?target=https%3A//cevalbenchmark.com/static/leaderboard.html)
+ [GaoKao-Bench](https://link.zhihu.com/?target=https%3A//github.com/OpenLMLab/GAOKAO-Bench)
+ [AlpacaEval](https://link.zhihu.com/?target=https%3A//tatsu-lab.github.io/alpaca_eval/)
+ [OpenCompass](https://link.zhihu.com/?target=https%3A//github.com/open-compass/opencompass/blob/main/README_zh-CN.md)

## Prompt设计指南

原则：

① 清楚地表达任务目标；

② 分解成简单、详细的子任务；

③ 提供少量演示；

④ 使用模型友好格式

### 任务描述

+ 描述越细节越好；
+ 告诉LLM自己是某方面的专家；
+ 告诉LLM更多应该做什么而不是不应该做什么；如果不希望太长的输出，可以使用`Question: Short Answer: `或者`in a or a few words`，`in one of two sentences`。

### 输入数据

+ 对于要求提供事实性知识的问题，可以先通过搜索引擎检索相关文档，然后将其串联到提示语中作为参考；
+ 为了突出提示中的某些重要部分，请使用特殊标记，如引号（""）和换行符（`\n`）。您也可以同时使用这两种符号来强调

### 上下文信息

+ 对于复杂的任务，可以清楚地描述完成任务所需的中间步骤，例如：`Please answer the question step by step as: Step 1 - Decompose the question into several sub-questions, · · ·`
+ 如果您想让 LLM 为文本打分，则有必要详细说明评分标准，并提供示例作为参考
+ 当 LLM 根据上下文 [生成文本 ](https://www.zhihu.com/search?q=%E7%94%9F%E6%88%90%E6%96%87%E6%9C%AC&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A654041855%7D)时（例如，根据购买历史记录进行推荐），向它们解释根据上下文生成的结果有助于提高生成文本的质量。

### 示例（Demonstration）

+ 格式良好的上下文示例对指导 LLM 非常有用，尤其是在制作格式复杂的输出时。
+ 在进行`few-shot CoT`提示时，也可以使用 `Let’s think step-by-step` 的提示语，而且少量示例之间应该用`\n`分隔，而不是句号。
+ 检索与上下文相似的示例去补充LLM任务相关的领域知识，为了获取更多的示例，可以先搜索问题的答案，然后再把问题和答案拼在一起去检索
+ 提示中语境范例的多样性也很有用。如果不容易获得多样化的问题，也可以设法保持问题解决方案的多样性。
+ 在使用`chat-based LLM`时，可以将示例分解为多轮对话形式
+ 复杂且详细的示例可以帮助LLM回答复杂问题
+ 由于一个符号序列通常可分为多个片段（如 `i1`、`i2`、`i3` -→ `i1`、`i2` 和 `i2`、`i3`），因此前面的片段可作为上下文中的范例，引导 LLM 预测后面的片段，同时提供历史信息。
+ 上下文中的示例和提示组件的顺序很重要。对于很长的输入数据，问题的位置（第一个或最后一个）也会影响性能。
+ 如果无法从现有数据集中获取上下文示例，另一种方法是使用 LLM 本身生成的`zero-shot`示例

### 其他设计

+ 让LLM在输出的时候检测内容是否正确。如`Check whether the above solution is correct or not.`
+ 如果LLM不能很好的解决问题，可以让LLM使用外部API工具，如`function calling`能力
+ 提示语应自成一体，最好不要在上下文中使用代词（如 `it` 和 `they`）。
+ 在使用 LLM 比较两个或多个示例时，顺序对性能影响很大。
+ 在提示之前，为 LLM 分配一个角色非常有用，可以帮助它更好地完成以下任务指令
+ 与其他语言相比，OpenAI 模型可以更好地用英语执行任务。因此，首先将输入内容翻译成英语，然后将其输入 LLM 是非常有用的。
+ 对于多选题，限制 LLM 的输出空间是非常有用的。您可以使用更详细的解释，也可以只对对数施加限制。
+ 对于基于排序的任务（如推荐），我们可以为未排序的项目分配指标（如 `ABCD`），并指示 LLM 直接输出排序后的指标，而不是在排序后直接输出每个项目的完整文本
