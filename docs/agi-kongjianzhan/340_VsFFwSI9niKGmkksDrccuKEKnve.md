# 阿里 Qwen2 模型开源，教你如何将 Qwen2 扩展到百万级上下文

> 来源：[https://agispace.feishu.cn/docx/CuIhdKaKnoZyQKxFPOSc7cNunLe](https://agispace.feishu.cn/docx/CuIhdKaKnoZyQKxFPOSc7cNunLe)

> 原创：是莫尔索

> 公众号：莫尔索随笔

> 原文链接：https://mp.weixin.qq.com/s/YAgS9UtEy6aI_2jsLK43jg

![](img/c720aa2f9e93987a5dab19436201ade5.png)

莫尔索随笔

跟踪LLM(大语言模型)最新动态，分享第一手LLM 实践经验，致力于打造个人LLM 应用开发全栈指南！

74篇原创内容

公众号

Qwen2 和 Qwen1.5 的模型结构基本一致，主要是模型预训练数据有所增加（大约在 7T 以上），Qwen2-0.5B、Qwen2-1.5B 模型支持最大上下文长度为 32K；Qwen2-57B-A14B MoE 模型支持最大上下文为 64K；Qwen2-7B、Qwen2-72B 模型支持最大上下文为 128K，代码和数学能力显著提升。

在 Qwen1.5 系列中，只有 32B 和 110B 的模型使用了 GQA，Qwen2 所有尺寸的模型都使用了 GQA，GQA 可以显著加速推理，降低显存占用。

### **模型效果**

Qwen2 系列模型效果不光整体超过 Qwen1.5 系列，相对于其他同级别参数开源模型也很亮眼，下面是指令微调模型 Qwen2-72B-Instruct 和 Qwen2-7B-Instruct 和常见开源 SOTA() 模型比较。

Qwen2-72B-Instruct 在多项指标超过 Llama-3-70B-Instruct，特别是中文领域，大幅领先。

Qwen2-72B-Instruct 在多项指标超过 Llama-3-70B-Instruct

代码和数学方面都超过了 Llama-3-70B-Instruct

代码和数学方面都超过了 Llama-3-70B-Instruct

Qwen2-7B-Instruct 和 智谱最近开源的 GLM-4-9B-Chat水平相当。

Qwen2-7B-Instruct

最后说下开源 License，除了 Qwen2-72B 使用 Qianwen License（有使用范围限制）其余模型 4 个尺寸模型均采用 Apache 2.0 的许可。

更多详细内容，请前往官网博客查看 https://qwenlm.github.io/zh/blog/qwen2/

### 借助 Qwen-Agent 实现长文本理解

这个也是阿里开源的，特别是在本地使用 Qwen2-0.5B、Qwen2-1.5B 这类上下文长度有限的模型时，通过 Qwen-Agent 框架，能够把处理的上下文扩展到 1M，整体采用的是代理式 RAG（Agentic RAG）思路，具体的做法分为三步。

#### **第一步查询转换**

查询转换

将文本分成每块不超过 512 字短块，保留最相关在 8k 上下文，采用查询转换的方法：

+   步骤 1：引导模型分离用户查询中的指令信息与非指令信息。例如，将用户查询转为{"信息": ["自行车是什么时候发明的"], "指令": ["回答时用 2000 字", "尽量详尽", "用英文回复"]}。

+   步骤 2：从信息部分提取多语言关键词。例如，"自行车是什么时候发明的"转为{"关键词英文": ["bicycles", "invented", "when"], "关键词中文": ["自行车", "发明", "时间"]}。

+   步骤 3：利用 BM25 基于关键词的检索，找出最相关的块。

这也是比较成熟的方案，详细可以看我半年前介绍的完整工程化实现使用这个工具后，我将 RAG 的准确性和召回率都提高了两倍！

#### **第二步 查询重写**

在相关块与用户查询关键词重叠程度不足时，可能导致遗漏相关内容未被检索到，采用二次查询的方式：

步骤 1：对于每个 512 字块，让模型先评估其与用户查询的相关性。若认为不相关，则输出"无"；若相关，则输出相关句子。

步骤 2：筛选出相关句子，将其用作搜索查询词，通过 BM25 检索出最相关的块（检索结果长度控制在 8k 上下文限制内）。

步骤 3：基于检索到的上下文生成最终答案。

查询重写

#### 第三步 自问提示(Self-Ask)

当遇到问题：“与第五交响曲创作于同一世纪的交通工具是什么？”模型需先回答子问题：“第五交响曲创作于哪个世纪？”即 19 世纪，接着，才能识别到包含信息“自行车于 19 世纪发明”与原问题相关。

通过让 LLM 自问自答生成多跳问题与答案，然后再生成最终的答案。

> 向 LLM1 提出一个问题？

> while (LLM1 无法根据其记忆回答问题) {

> LLM1 提出一个新的子问题待解答。

> LLM1 向 LLM2 提问这个子问题。

> 将 LLM2 的回答添加到 LLM1 的记忆中。

> }

> LLM1 提供原始问题的最终答案。

通过这种方式就可以让较弱上下文长度的开源模型具备长上下文能力，对于构建完全本地化的 RAG 应用十分有益，下节我将结合 Ollama 在本地部署 Qwen2-7B，构建隐私安全的完全本地化笔记搜索软件。

![](img/619e55b9cc7c283c594a21c4a6e17c6d.png)

莫尔索随笔

跟踪LLM(大语言模型)最新动态，分享第一手LLM 实践经验，致力于打造个人LLM 应用开发全栈指南！

74篇原创内容

公众号

### LangChain() 相关文章合集

+   从零开始学 LangChain（1）：入门介绍

+   [**从零开始学 LangChain（2）：Model I/O 模块**](https://mp.weixin.qq.com/s?__biz=Mzg2OTk1NDQ4Ng==&mid=2247483866&idx=1&sn=3b50f450f67367c3024c1193223a88a0&scene=21#wechat_redirect)和 Data Connection 模块

+   [**从零开始学 LangChain（3）**](https://mp.weixin.qq.com/s?__biz=Mzg2OTk1NDQ4Ng==&mid=2247483890&idx=1&sn=f1bc87d4e4087df24121c32a7e6a85ee&scene=21#wechat_redirect)：Memory 模块和 Chain 模块

+   [**从零开始学 LangChain（4）：Age**](https://mp.weixin.qq.com/s?__biz=Mzg2OTk1NDQ4Ng==&mid=2247483894&idx=1&sn=bb3ab52b45ffe3c6d18fce41ded748a4&scene=21#wechat_redirect)nts 模块和 Callbacks 模块

+   [**LLM 应用开发框架 Semantic Kerne**](https://mp.weixin.qq.com/s?__biz=Mzg2OTk1NDQ4Ng==&mid=2247484170&idx=1&sn=044f341d858934e7e4b8343fdba53cff&scene=21#wechat_redirect)l 和 LangChain 比较（上篇）

+   [**LLM 应用开发框架 Semantic Kerne**](https://mp.weixin.qq.com/s?__biz=Mzg2OTk1NDQ4Ng==&mid=2247484176&idx=1&sn=f1e46b697571c195fc3bb499cb390449&scene=21#wechat_redirect)l 和 LangChain 比较（下篇）

+   轻装上阵，加速商业化！LangChain 0.1 预发布看点！

+   [**LangChain 在多轮对话中让**](https://mp.weixin.qq.com/s?__biz=Mzg2OTk1NDQ4Ng==&mid=2247484590&idx=1&sn=7b5acda8ed4cef7a593c02a694470539&scene=21#wechat_redirect) Agent 保持长期记忆的 8 种方式

+   如何使用 LangChain 实现 Agent 的 ReAct 模式

+   LangChain 创始人最新访谈：开源 LLM 中间件的未来与挑战

+   [**刚刚！LangChain 宣布推出 L**](https://mp.weixin.qq.com/s?__biz=Mzg2OTk1NDQ4Ng==&mid=2247485046&idx=1&sn=b0e8e3c0dbee8c3ebf21c50c329ce74e&scene=21#wechat_redirect)angChain v0.2 预发布版本！

+   [**使用 DeepSeek-V2+LangGraph**](https://mp.weixin.qq.com/s?__biz=Mzg2OTk1NDQ4Ng==&mid=2247485039&idx=1&sn=cddc45babe6dfba911e7971d2c0a0d08&scene=21#wechat_redirect) 编写了一个编码助手类 AI Agent