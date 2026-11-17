
# 第五天：大模型自动评估理论和实战--LLM Automatic Evaluation

> 来源：[https://seigy6zzam.feishu.cn/docx/A3radouS4oI9p3xgqL8c3L7Mnxg](https://seigy6zzam.feishu.cn/docx/A3radouS4oI9p3xgqL8c3L7Mnxg)

> 🔗 原文链接：https://mp.weixin.qq.com/s?__biz=MzkxNTM5NTg2OA==&mid=2247490037&idx=1&sn=696b9d95263b1589a02728e8abdc015b&chksm=c15e9d86f62914901f99782d9b1bb29200c61738b6a93219a7b45eb364114b068e6b16ee5a89&mpshare=1&scene=1&srcid=0224RMRZtploPu6iYux2d0nh&sharer_shareinfo=56eb99a252d4b7851dcf93fb099faa6d&sharer_shareinfo_first=56eb99a252d4b7851dcf93fb099faa6d&exportkey=n_ChQIAhIQNLEGb7CRCeGYuNv8rKXklxKZAgIE97dBBAEAAAAAALCNADybmg8AAAAOpnltbLcz9gKNyK89dVj0i44wQJT9zJm8OHzAmQaffj3UI2hBnBvtkoZcqU5NZg8j7m%2FrUtYivIsS6kKo3udXAJhn2SospJSWRrsQOmxkhqM2%2BTQo9D6Yb1pR2NJ%2BN2ieq76cikcr6t%2Bz52%2BO6gwxq3Tm8UFacvTLIMsUC5Zo9%2Bxj5Nyf37H%2BFRPPCy2NvQGUK%2BBwRVZzOOCOmt2B%2BRvV58zvE0Rl5WF9rKGtkAEx3%2FFp89VRVmyms2SowC9sVu7FNtGZrfRy8Zf0UwvK%2FTdiZdW7OiAKJTy09H6vnPWlwINrCTVcLN0NxPpPRAS4kmtXGqY8jLVQOiqA%2BNYPeTiBOlsE&acctmode=0&pass_ticket=sqEw1AixJuTL4PxgBfg1Y1Ten2SuvmVvzfs0fJ7JRBVbTBbOoWC7hjmUChJPIEhRQF2pR%2FyJsQfZfLrqH9%2FJnQ%3D%3D&wx_header=0#rd

> ⏰ 剪存时间：2024-02-24 04:32:56 (UTC+8)

> ✂️ 本文档由 [飞书剪存](https://www.feishu.cn/hc/zh-CN/articles/606278856233?from=in_ccm_clip_doc) 一键生成

原创 魔搭官方 魔搭ModelScope社区 2024-02-13 18:00 广西

![](img/212142e4b08841cfc087b3874040ec44.png)

## 引言

### 背景

大语言模型（LLM）评测是LLM开发和应用中的关键环节。目前评测方法可以分为人工评测和自动评测，其中，自动评测技术相比人工评测来讲，具有效率高、一致性好、可复现、鲁棒性好等特点，逐渐成为业界研究的重点。

模型的自动评测技术可以分为 `rule-based` 和 `model-based` 两大类：

- `rule-based` 方法：
  - benchmark 以客观题为主，例如多选题，被测的 LLM 通过理解 context/question，来指定最佳答案
  - 解析 LLM 的 response，与标准答案做对比
  - 计算 metric（accuracy、rouge、bleu 等）

- `model-based` 方法：
  - 裁判员模型（e.g. GPT-4、Claude、Expert Models/Reward models）
  - LLM Peer-examination

### 内容提要

- LLM 自动评估理论
- 如何评估一个 LLM
- 自动评估的方法
- 常用的 benchmark
- LLM 评估面临的问题和挑战
- LLM 自动评估实战
- LLMuses 自动评测框架介绍
- 基于客观题 benchmark 自动评估
- 基于专家模型的自动评估
- LLM 推理性能评估

## LLM 评估的方法论

![](img/b6c434779613aa50116ce97c0ce843c5.png)

### 1.1 如何评估一个 LLM

哪些维度？

- 语义理解（Understanding）
- 知识推理（Reasoning）
- 专业能力（e.g. coding、math）
- 应用能力（MedicalApps、AgentApps、AI-FOR-SCI ...）
- 指令跟随（Instruction Following）
- 鲁棒性（Robustness）
- 偏见（Bias）
- 幻觉（Hallucinations）
- 安全性（Safety）

![](img/d73f7ca733da6790aa0077e0fba2f986.png)

> 例：GPT-4 vs LLaMA2-7B 能力维度对比评测

### 1.2 自动评估方法

#### 1.2.1 模型效果评估

- 基准和指标（Benchmarks & Metrics）

| 数据集 | 描述 | 评价指标 | 样例 |
| --- | --- | --- | --- |
| MMLU | Massive Multitask Language Understanding | Accuracy | Question: In 2016, about how many people in the United States were homeless? |
| TriviaQA | 阅读理解数据集，包含超过 65 万个问题-答案-证据三元组。其包括 95K 个问答对，由冷知识爱好者提供 + 独立收集的事实性文档撰写 | EM (Exact Match) | |
| MATH | 12500 道数学题，每道包含 step-by-step solution | Accuracy | |
| HumanEval | HumanEval (Hand-Written Evaluation Set) | pass@k | { |

#### Rule-based 自动评测

> 基本 流程

![](img/39d97152dc4ed0637f59e72e2ae642c0.png)

- 根据数据集原始 question 来构建 prompt

> 示例 (few-shot)

![](img/267e57a21ebe6f497d9054d84b1cb3c9.png)

> 示例：few-shot with CoT

```
# Examples in BBH
Evaluate the result of a random Boolean expression.
Q: not ( ( not not True ) ) is A: Let's think step by step. Remember that (i) expressions inside brackets are always evaluated first and that (ii) the order of operations from highest priority to lowest priority is "not", "and", "or", respectively. We first simplify this expression "Z" as follows: "Z = not ( ( not not True ) ) = not ( ( A ) )" where "A = not not True". Let's evaluate A: A = not not True = not (not True) = not False = True. Plugging in A, we get: Z = not ( ( A ) ) = not ( ( True ) ) = not True = False. So the answer is False.
Q: True and False and not True and True is A: Let's think step by step. Remember that (i) expressions inside brackets are always evaluated first and that (ii) the order of operations from highest priority to lowest priority is "not", "and", "or", respectively. We first simplify this expression "Z" as follows: "Z = True and False and not True and True = A and B" where "A = True and False" and "B = not True and True". Let's evaluate A: A = True and False = False. Let's evaluate B: B = not True and True = not (True and True) = not (True) = False. Plugging in A and B, we get: Z = A and B = False and False = False. So the answer is False.
```

#### 模型预测

> Generate

```
# Demo -- model_generate 直接生成 response
def model_generate(query: str, infer_cfg: dict) -> str:
    inputs = tokenizer.encode(query)
    input_ids = inputs['input_ids']
    ...
    # Process infer cfg (do_sample, top_k, top_p, temperature, special_tokens ...)
    generation_config = process_cfg(args)
    ...
    # Run inference
    output_ids = model.generate(
        input_ids=input_ids,
        attention_mask=attention_mask,
        generation_config=generation_config,
    )
    response = tokenizer.decode(output_ids, **decode_kwargs)
    return response
```

> Likelihood

```
# Demo -- model_call 方式计算 loglikelihood
# context + continuation 拼接，示例：# Question：法国的首都是哪里？# Choices：A.北京  B.巴黎  C.汉堡  D.纽约  # pair-1: (ctx, cont) = (法国的首都是哪里？，A.北京)  # pair-2: (ctx, cont) = (法国的首都是哪里？，B.巴黎)  # pair-3: (ctx, cont) = (法国的首都是哪里？，C.汉堡)  # pair-4: (ctx, cont) = (法国的首都是哪里？，D.纽约) # Logits -->
def loglikelihood(self, inputs: list, infer_cfg: dict = None) -> list:
    # To predict one doc
    doc_ele_pred = []
    for ctx, continuation in inputs:
        # ctx_enc shape: [context_tok_len]  cont_enc shape: [continuation_tok_len]
        ctx_enc, cont_enc = self._encode_pair(ctx, continuation)
        inputs_tokens = torch.tensor(
            (ctx_enc.tolist() + cont_enc.tolist())[-(self.max_length + 1):][:-1],
            dtype=torch.long,
            device=self.model.device).unsqueeze(0)
        logits = self.model(inputs_tokens)[0]
        logits = torch.nn.functional.log_softmax(logits.float(), dim=-1)
        logits = logits[:, -len(cont_enc):, :]
        cont_enc = cont_enc.unsqueeze(0).unsqueeze(-1)
        logits = torch.gather(logits.cpu(), 2, cont_enc.cpu()).squeeze(-1)
        choice_score = float(logits.sum())
        doc_ele_pred.append(choice_score)
    # e.g. [-2.3, 1.1, -12.9, -9.2], length=len(choices)
    return doc_ele_pred
```

#### 评价指标（Metrics）

- `WeightedAverageAccuracy` 加权平均准确率
- `Perplexity` 困惑度
- `Rouge` (Recall-Oriented Understudy for Gisting Evaluation)
- `Bleu` (Bilingual evaluation understudy)
- `ELO Rating System`
- `PASS@K`

#### Model-based 自动评测

![](img/e05bc924d069d4556d46a46d5b4e9b9d.png)

- 中心化评测
- 中心化评测模式下，裁判员模型只有一个，可靠性高，但容易收到裁判员模型的 bias 影响
- 去中心化评测
- 去中心化评测方式，要求模型之间做 peer-examination
- 特点是公平性好，但计算量大，且鲁棒性不高

> 裁判员模型

- `GPT-4`、`Claude`、`Qwen-Max` 等（产品 APIs）
- `PandLM`、`Auto-J` (tuned from LLM, like `LLaMA`)
- `Reward models` (Ranking learning)
- `Chatbot Arena` - 竞技场模式
- `Battle count of each combination of models`, from `LMSYS`

![](img/927e613fed5fc8434a3ae92117c6e7db.png)

- `Fraction of Model A wins for all non-tied A vs. B battles`, from `LMSYS`

![](img/b1c7d84b45d759edb93915fe43526f8a.png)

#### LLM 指令攻防

- 指令诱导（诱导模型输出目标答案，from SuperCLUE）

![](img/964c1c46d241bf59edd51876d4ffb484.png)

- 有害指令注入（将真实有害意图注入到 prompt 中，from SuperCLUE）

![](img/cf8d7bc4951a0973fd9b1dac54ac916f.png)

#### 1.2.2 模型性能评估

> model serving performance evaluation

| 指标名称 | 说明 |
| --- | --- |
| Time | 测试总时间（时间单位均为秒） |
| Expected number of requests | 期望发送的请求数，和 prompt 文件以及期望 number 有关 |
| concurrency | 并发数 |
| completed | 完成的请求数 |
| succeed | 成功请求数 |
| failed | 失败请求数 |
| qps | 平均 qps |



| latency | 平均latency |

| time to first token | 平均首包延迟 |

| throughput | output tokens / seconds |

| time per output token | 平均生成一个token需要的时间 |

| package per request | 平均每个请求的包数 |

| time per package | 平均每包时间 |

| input tokens per request | 平均每个请求的输入token数 |

| output tokens per request | 平均每个请求输出token数 |

## 1.3 问题和挑战

### 1.3.1 基准失效&数据泄露

+   静态数据集与快速演进的LLM能力形成GAP，导致基准失效

+   公开的benchmark被泄露到LLM的开发PT、CPT、SFT等开发环节

> 解决思路：动态数据集

![](img/ce1729e5998aa922181a499cd4cfd294.png)

![](img/9ecfc0843e1fc427cc96f9e7c16c160a.png)

### 1.3.2 裁判员模型的能力上限

+   裁判员模型的存在明显的能力边界，很难胜任更多场景、更强模型的评测工作

+   泛化性问题

+   LLM幻觉的诊断问题

# LLM评估实战

`LLMuses`框架--轻量化、端到端的大模型自动评估框架

**GitHub:**

`https://github.com/modelscope/llmuses`

### 框架特性

+   预置常用的测试基准，包括：MMLU、C-Eval、GSM8K、ARC、HellaSwag、TruthfulQA、MATH、HumanEval、BBH、GeneralQA等

+   常用评估指标（`metrics`）的实现

+   统一`model`接入，兼容多个系列模型的`generate`、`chat`接口

+   客观题自动评估

+   使用专家模型实现复杂任务的自动评估

+   竞技场模式(`Arena`)

+   评估报告生成与可视化

+   LLM性能评测（Performance Evaluation）

### 环境安装

```
# 1. 代码下载git clone git@github.com:modelscope/llmuses.git
# 2. 安装依赖cd llmuses/pip install -r requirements/requirements.txtpip install -e .
```

## 2.1 简单评测

```
# 指定模型和数据集python llmuses/run.py --model ZhipuAI/chatglm3-6b --datasets ceval --outputs ./outputs/test --limit 10
```

+   `--model`: ModelScope模型id

+   (`https://modelscope.cn/models/ZhipuAI/chatglm3-6b/summary`)，也可以是模型的本地路径

+   `--datasets`: 数据集的id

+   `--limit`: （每个sub-task）最大评测样本数

关闭

**观看更多**

更多

退出全屏

切换到横屏模式

[🎦 点击观看视频](https://mpvideo.qpic.cn/0bc3ruaccaaapiali6wlmbsvbdodeggqaiia.f10002.mp4?dis_k=276dc5c998f6333495ab1b561536cf03&dis_t=1708777877&play_scene=10120&auth_info=Dfae8qQEagI2+qeKu2gDAHhuBWsVQ2BaczhxR05qfkM2fTJKdgw3bVQPLWYRSRQ6cg==&auth_key=ac74a2821a3333a0afadf6106a486981&vid=wxv_3326062012199714818&format_id=10002&support_redirect=0&mmversion=false)

> 无法获取音频链接

视频详情

## 2.2 带参数评测

```
python llmuses/run.py --model ZhipuAI/chatglm3-6b --outputs ./outputs/test2 --model-args revision=v1.0.2,precision=torch.float16,device_map=auto --datasets arc --limit 10
```

+   `--model-args`: 模型参数，以逗号分隔，`key=value`形式

+   `--datasets`: 数据集名称，参考下文`数据集列表`章节

+   `--mem-cache`: 是否使用内存缓存，若开启，则已经跑过的数据会自动缓存，并持久化到本地磁盘

+   `--limit`: 每个subset最大评估数据量

关闭

**观看更多**

更多

退出全屏

切换到横屏模式

[🎦 点击观看视频](https://mpvideo.qpic.cn/0bc3zeackaaanmalhdwlgbsvbsodexeqajia.f10002.mp4?dis_k=0a4e8de4e95fd0dc3513b827f9b9a645&dis_t=1708777877&play_scene=10120&auth_info=Vvqguzc+WDH8oI28P1MGdWlZZkgcYw8kOyEdSD4tQTZwNkwlBmM3UwkqYRYeRDx/&auth_key=3c450589ccf57087aed0309c1c20c836&vid=wxv_3326064879107162114&format_id=10002&support_redirect=0&mmversion=false)

> 无法获取音频链接

视频详情

## 2.3 竞技场模式--Single mode

Single mode，使用专家模型（GPT-4）对待测LLM进行打分

```
# Examplepython llmuses/run_arena.py --c registry/config/cfg_single.yaml --dry-run
```

关闭

**观看更多**

更多

退出全屏

切换到横屏模式

[🎦 点击观看视频](https://mpvideo.qpic.cn/0bc3pqaccaaavaafp7glmjsva7gdef6aaiia.f10002.mp4?dis_k=3d66ddd2da8b12995a5919784538c4f5&dis_t=1708777877&play_scene=10120&auth_info=VrGzj9MDZAJkrKGL6GhRBylpDjZMHTRYcjJ8Sxo+JUNtfzxPdgw5bQZZK2dCSUY9Iw==&auth_key=771928acc71cab07a6669695a411977a&vid=wxv_3325928269971570692&format_id=10002&support_redirect=0&mmversion=false)

> 无法获取音频链接

视频详情

## 2.4 竞技场模式--Baseline mode

Baseline mode，选定baseline模型，其它待测LLM与该模型进行对比

```
# Examplepython llmuses/run_arena.py --dry-run --c registry/config/cfg_pairwise_baseline.yaml
```

关闭

**观看更多**

更多

退出全屏

切换到横屏模式

[🎦 点击观看视频](https://mpvideo.qpic.cn/0bc3syaciaaavaafu6wlavsvbfwdeslaajaa.f10002.mp4?dis_k=3d62132f5419aaa4e7c87d64daf6c7c7&dis_t=1708777877&play_scene=10120&auth_info=C/yB4rJRagJnqaOL6GhVUn4/WmcZFG8IIWpxG0xoJEUwfjAfIlk3bQVcKWdCSUJodA==&auth_key=b4be2e6006a0399d95c06578b507b3fa&vid=wxv_3325960124116615169&format_id=10002&support_redirect=0&mmversion=false)

> 无法获取音频链接

视频详情

## 2.5 竞技场模式--Pairwise mode

Pairwise mode，待测LLM两两组合进行对弈

```
python llmuses/run_arena.py -c registry/config/cfg_arena.yaml --dry-run
```

关闭

**观看更多**

更多

退出全屏

切换到横屏模式

[🎦 点击观看视频](https://mpvideo.qpic.cn/0bc334acmaaa4yaf4v6lfvsvbx6de3pqajqa.f10002.mp4?dis_k=72f2645f49c039aa1704c03a1e556cbf&dis_t=1708777877&play_scene=10120&auth_info=X8frm/0EaVczqPKPvjhVVXk4WmYURmZaeWokT0w7KBFke2VKJgo0OFFdeGMUGUJvcw==&auth_key=4901e5806e04d8c89cf94cd47bed6a82&vid=wxv_3325934272842678276&format_id=10002&support_redirect=0&mmversion=false)

> 无法获取音频链接

视频详情

![](img/65ac32d381b1bcb44e24c6f729c77ef6.png)

## 2.6 效果评测报告

按照预定格式存放数据，使用`streamlit`启动可视化服务

```
# Usage:streamlit run viz.py -- --review-file llmuses/registry/data/qa_browser/battle.jsonl --category-file llmuses/registry/data/qa_browser/category_mapping.yaml
```

+   **报告可视化**

![](img/a7b6b9b63ce4d4d229f5001977196f3a.png)

+   **Leaderboard:**

+   `https://modelscope.cn/leaderboard/58/ranking?type=free`

![](img/8f83b562999760bdc72fc3992ddea51d.png)

## 2.7 模型性能评测（Perf Eval）

> 性能评测报告示例

![](img/71b6d1728a176440597a60b130817895.png)

![](img/cdc6ae18616697b5f192bd7518089d03.png)

**预告：**

**第六天：大模型量化及低成本部署最佳实践**

点击 [**阅读原文**](https://www.bilibili.com/video/BV1W6421g7tP/)，直达本期课程视频学习
