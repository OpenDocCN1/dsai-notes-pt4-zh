
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
