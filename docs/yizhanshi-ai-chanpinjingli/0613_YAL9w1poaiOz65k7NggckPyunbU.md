# 评测

> 来源：[https://v11enp9ok1h.feishu.cn/docx/EQtGdulhcoKsHDxvt3fcbp5rnEc](https://v11enp9ok1h.feishu.cn/docx/EQtGdulhcoKsHDxvt3fcbp5rnEc)

# 评测

SWIFT支持了eval（评测）能力，用于对原始模型和训练后的模型给出标准化的评测指标。

## 能力介绍

[SWIFT的eval能力使](https://github.com/modelscope/eval-scope)用了魔搭社区评测框架EvalScope，并进行了高级封装以支持各类模型的评测需求。

> [注意：Eval](https://evalscope.readthedocs.io/zh-cn/latest/user_guides/stress_test/quick_start.html)Scope支持许多其他的复杂能力，例如模型的性能评测，请直接使用EvalScope框架。

目前我们支持了标准评测集的评测流程，以及用户自定义评测集的评测流程。其中标准评测集由三个评测后端提供支持：

[下面展示所支持的](https://evalscope.readthedocs.io/zh-cn/latest/get_started/supported_dataset.html)数据集名称，若需了解数据集的详细信息，请参考所有支持的数据集

1.  Native（默认）：

主要支持纯文本评测，同时支持评测结果可视化

```
&#x27;arc&#x27;, &#x27;bbh&#x27;, &#x27;ceval&#x27;, &#x27;cmmlu&#x27;, &#x27;competition_math&#x27;,
&#x27;general_qa&#x27;, &#x27;gpqa&#x27;, &#x27;gsm8k&#x27;, &#x27;hellaswag&#x27;, &#x27;humaneval&#x27;,
&#x27;ifeval&#x27;, &#x27;iquiz&#x27;, &#x27;mmlu&#x27;, &#x27;mmlu_pro&#x27;,
&#x27;race&#x27;, &#x27;trivia_qa&#x27;, &#x27;truthful_qa&#x27;
```

1.  OpenCompass：

主要支持纯文本评测，暂不支持评测结果可视化

```
&#x27;obqa&#x27;, &#x27;cmb&#x27;, &#x27;AX_b&#x27;, &#x27;siqa&#x27;, &#x27;nq&#x27;, &#x27;mbpp&#x27;, &#x27;winogrande&#x27;, &#x27;mmlu&#x27;, &#x27;BoolQ&#x27;, &#x27;cluewsc&#x27;, &#x27;ocnli&#x27;, &#x27;lambada&#x27;,
&#x27;CMRC&#x27;, &#x27;ceval&#x27;, &#x27;csl&#x27;, &#x27;cmnli&#x27;, &#x27;bbh&#x27;, &#x27;ReCoRD&#x27;, &#x27;math&#x27;, &#x27;humaneval&#x27;, &#x27;eprstmt&#x27;, &#x27;WSC&#x27;, &#x27;storycloze&#x27;,
&#x27;MultiRC&#x27;, &#x27;RTE&#x27;, &#x27;chid&#x27;, &#x27;gsm8k&#x27;, &#x27;AX_g&#x27;, &#x27;bustm&#x27;, &#x27;afqmc&#x27;, &#x27;piqa&#x27;, &#x27;lcsts&#x27;, &#x27;strategyqa&#x27;, &#x27;Xsum&#x27;, &#x27;agieval&#x27;,
&#x27;ocnli_fc&#x27;, &#x27;C3&#x27;, &#x27;tnews&#x27;, &#x27;race&#x27;, &#x27;triviaqa&#x27;, &#x27;CB&#x27;, &#x27;WiC&#x27;, &#x27;hellaswag&#x27;, &#x27;summedits&#x27;, &#x27;GaokaoBench&#x27;,
&#x27;ARC_e&#x27;, &#x27;COPA&#x27;, &#x27;ARC_c&#x27;, &#x27;DRCD&#x27;
```

1.  VLMEvalKit:

主要支持多模态评测，暂不支持评测结果可视化

```
&#x27;COCO_VAL&#x27;, &#x27;MME&#x27;, &#x27;HallusionBench&#x27;, &#x27;POPE&#x27;, &#x27;MMBench_DEV_EN&#x27;, &#x27;MMBench_TEST_EN&#x27;, &#x27;MMBench_DEV_CN&#x27;, &#x27;MMBench_TEST_CN&#x27;,
&#x27;MMBench&#x27;, &#x27;MMBench_CN&#x27;, &#x27;MMBench_DEV_EN_V11&#x27;, &#x27;MMBench_TEST_EN_V11&#x27;, &#x27;MMBench_DEV_CN_V11&#x27;,
&#x27;MMBench_TEST_CN_V11&#x27;, &#x27;MMBench_V11&#x27;, &#x27;MMBench_CN_V11&#x27;, &#x27;SEEDBench_IMG&#x27;, &#x27;SEEDBench2&#x27;,
&#x27;SEEDBench2_Plus&#x27;, &#x27;ScienceQA_VAL&#x27;, &#x27;ScienceQA_TEST&#x27;, &#x27;MMT-Bench_ALL_MI&#x27;, &#x27;MMT-Bench_ALL&#x27;,
&#x27;MMT-Bench_VAL_MI&#x27;, &#x27;MMT-Bench_VAL&#x27;, &#x27;AesBench_VAL&#x27;, &#x27;AesBench_TEST&#x27;, &#x27;CCBench&#x27;, &#x27;AI2D_TEST&#x27;, &#x27;MMStar&#x27;,
&#x27;RealWorldQA&#x27;, &#x27;MLLMGuard_DS&#x27;, &#x27;BLINK&#x27;, &#x27;OCRVQA_TEST&#x27;, &#x27;OCRVQA_TESTCORE&#x27;, &#x27;TextVQA_VAL&#x27;, &#x27;DocVQA_VAL&#x27;,
&#x27;DocVQA_TEST&#x27;, &#x27;InfoVQA_VAL&#x27;, &#x27;InfoVQA_TEST&#x27;, &#x27;ChartQA_TEST&#x27;, &#x27;MathVision&#x27;, &#x27;MathVision_MINI&#x27;,
&#x27;MMMU_DEV_VAL&#x27;, &#x27;MMMU_TEST&#x27;, &#x27;OCRBench&#x27;, &#x27;MathVista_MINI&#x27;, &#x27;LLaVABench&#x27;, &#x27;MMVet&#x27;, &#x27;MTVQA_TEST&#x27;,
&#x27;MMLongBench_DOC&#x27;, &#x27;VCR_EN_EASY_500&#x27;, &#x27;VCR_EN_EASY_100&#x27;, &#x27;VCR_EN_EASY_ALL&#x27;, &#x27;VCR_EN_HARD_500&#x27;,
&#x27;VCR_EN_HARD_100&#x27;, &#x27;VCR_EN_HARD_ALL&#x27;, &#x27;VCR_ZH_EASY_500&#x27;, &#x27;VCR_ZH_EASY_100&#x27;, &#x27;VCR_ZH_EASY_ALL&#x27;,
&#x27;VCR_ZH_HARD_500&#x27;, &#x27;VCR_ZH_HARD_100&#x27;, &#x27;VCR_ZH_HARD_ALL&#x27;, &#x27;MMDU&#x27;, &#x27;MMBench-Video&#x27;, &#x27;Video-MME&#x27;
```

## 环境准备

```
pip install ms-swift[eval] -U
```

或从源代码安装：

```
git clone https://github.com/modelscope/ms-swift.git
cd ms-swift
pip install -e &#x27;.[eval]&#x27;
```

## 评测#

支持纯文本评测、多模态评测、url评测、自定义数据集评测四种方式

基本示例

```
CUDA_VISIBLE_DEVICES=0 \
swift eval \
    --model Qwen/Qwen2.5-0.5B-Instruct \
    --eval_backend Native \
    --infer_backend pt \
    --eval_limit 10 \
    --eval_dataset gsm8k
```

其中:

+   model: 可指定本地模型路径或者modelscope上的模型ID

+   eval_backend: 可选 Native, OpenCompass, VLMEvalKit，默认为 Native

+   infer_backend: 可选 pt, vllm, lmdeploy，默认为 pt

+   eval_limit: 每个评测集的采样数，默认为None，表示使用全部数据，可用于快速验证

+   eval_dataset: 评测数据集，可设置多个数据集，用空格分割

具体评测的参数列表可以参考[这里](https://modelscope.cn/docs/llm-training-and-inference/user-guide/%E5%91%BD%E4%BB%A4%E8%A1%8C%E5%8F%82%E6%95%B0.md#%E8%AF%84%E6%B5%8B%E5%8F%82%E6%95%B0)。

## 训练中评测#

SWIFT支持在训练过程中使用EvalScope对当前的模型进行评测，以便及时了解模型的训练效果。

基本示例

```
CUDA_VISIBLE_DEVICES=0 \
swift sft \
  --model &quot;Qwen/Qwen2.5-0.5B-Instruct&quot; \
  --train_type &quot;lora&quot; \
  --dataset &quot;AI-ModelScope/alpaca-gpt4-data-zh#100&quot; \
  --torch_dtype &quot;bfloat16&quot; \
  --num_train_epochs &quot;1&quot; \
  --per_device_train_batch_size &quot;1&quot; \
  --learning_rate &quot;1e-4&quot; \
  --lora_rank &quot;8&quot; \
  --lora_alpha &quot;32&quot; \
  --target_modules &quot;all-linear&quot; \
  --gradient_accumulation_steps &quot;16&quot; \
  --save_steps &quot;50&quot; \
  --save_total_limit &quot;5&quot; \
  --logging_steps &quot;5&quot; \
  --max_length &quot;2048&quot; \
  --eval_strategy &quot;steps&quot; \
  --eval_steps &quot;5&quot; \
  --per_device_eval_batch_size &quot;5&quot; \
  --eval_use_evalscope \
  --eval_datasets &quot;gsm8k&quot; \
  --eval_datasets_args &#x27;{&quot;gsm8k&quot;: {&quot;few_shot_num&quot;: 0}}&#x27; \
  --eval_limit &quot;10&quot;
```

注意启动命令为sft，其中eval相关的参数有：

+   eval_strategy: 评估策略。默认为None，跟随save_strategy的策略

+   eval_steps: 默认为None，如果存在评估数据集，则跟随save_steps的策略

+   eval_use_evalscope: 是否使用evalscope进行评测，需要设置该参数来开启评测

+   eval_datasets: 评测数据集，可设置多个数据集，用空格分割

+   eval_datasets_args: 评测数据集参数，json格式，可设置多个数据集的参数

+   eval_limit: 评测数据集采样数

+   eval_generation_config: 评测时模型推理配置，json格式，默认为{'max_tokens': 512}

更多评测的样例可以参考[examples](https://github.com/modelscope/ms-swift/tree/main/examples/eval)

## 自定义评测集#

本框架支持选择题和问答题，两种预定义的数据集格式，使用流程如下：

注意：使用自定义评测时，eval_backend参数必须为Native

### 选择题格式（MCQ）#

适合用户是选择题的场景，评测指标为准确率（accuracy）。

数据准备

准备选择题格式的csv文件，该目录结构如下：

```
mcq/
├── example_dev.csv  # （可选）文件名组成为
{subset_name}_dev.csv
，用于fewshot评测
└── example_val.csv  # 文件名组成为{subset_name}_val.csv，用于实际评测的数据
```

其中csv文件需要为下面的格式：

```
id,question,A,B,C,D,answer
1,通常来说，组成动物蛋白质的氨基酸有____,4种,22种,20种,19种,C
2,血液内存在的下列物质中，不属于代谢终产物的是____。,尿素,尿酸,丙酮酸,二氧化碳,C
```

其中：

+   id是序号（可选）

+   question是问题

+   A, B, C, D等是可选项，最大支持10个选项

+   answer是正确选项

启动评测

运行下面的命令：

```
CUDA_VISIBLE_DEVICES=0 \
swift eval \
    --model Qwen/Qwen2.5-0.5B-Instruct \
    --eval_backend Native \
    --infer_backend pt \
    --eval_dataset general_mcq \
    --dataset_args &#x27;{&quot;general_mcq&quot;: {&quot;local_path&quot;: &quot;/path/to/mcq&quot;, &quot;subset_list&quot;: [&quot;example&quot;]}}&#x27;
```

其中：

+   eval_dataset 需要设置为 general_mcq

+   dataset_args 需要设置

+   local_path 自定义数据集文件夹路径

+   subset_list 评测数据集名称，上述 *_dev.csv 中的 *

运行结果

```
+---------------------+-------------+-----------------+----------+-------+---------+---------+
| Model               | Dataset     | Metric          | Subset   |   Num |   Score | Cat.0   |
+=====================+=============+=================+==========+=======+=========+=========+
| Qwen2-0.5B-Instruct | general_mcq | AverageAccuracy | example  |    12 |  0.5833 | default |
+---------------------+-------------+-----------------+----------+-------+---------+---------+
```

## 问答题格式（QA）#

适合用户是问答题的场景，评测指标是ROUGE和BLEU。

数据准备

准备一个问答题格式的jsonline文件，该目录包含了一个文件：

```
qa/
└── example.jsonl
```

该jsonline文件需要为下面的格式：

```
{&quot;query&quot;: &quot;中国的首都是哪里？&quot;, &quot;response&quot;: &quot;中国的首都是北京&quot;}
{&quot;query&quot;: &quot;世界上最高的山是哪座山？&quot;, &quot;response&quot;: &quot;是珠穆朗玛峰&quot;}
{&quot;query&quot;: &quot;为什么北极见不到企鹅？&quot;, &quot;response&quot;: &quot;因为企鹅大多生活在南极&quot;}
```

启动评测

运行下面的命令：

```
CUDA_VISIBLE_DEVICES=0 \
swift eval \
    --model Qwen/Qwen2.5-0.5B-Instruct \
    --eval_backend Native \
    --infer_backend pt \
    --eval_dataset general_qa \
    --dataset_args &#x27;{&quot;general_qa&quot;: {&quot;local_path&quot;: &quot;/path/to/qa&quot;, &quot;subset_list&quot;: [&quot;example&quot;]}}&#x27;
```

其中：

+   eval_dataset 需要设置为 general_qa

+   dataset_args 是一个json字符串，需要设置：

+   local_path 自定义数据集文件夹路径

+   subset_list 评测数据集名称，上述 *.jsonl 中的 *

运行结果

```
+---------------------+-------------+-----------------+----------+-------+---------+---------+
| Model               | Dataset     | Metric          | Subset   |   Num |   Score | Cat.0   |
+=====================+=============+=================+==========+=======+=========+=========+
| Qwen2-0.5B-Instruct | general_qa  | bleu-1          | default  |    12 |  0.2324 | default |
+---------------------+-------------+-----------------+----------+-------+---------+---------+
| Qwen2-0.5B-Instruct | general_qa  | bleu-2          | default  |    12 |  0.1451 | default |
+---------------------+-------------+-----------------+----------+-------+---------+---------+
| Qwen2-0.5B-Instruct | general_qa  | bleu-3          | default  |    12 |  0.0625 | default |
+---------------------+-------------+-----------------+----------+-------+---------+---------+
| Qwen2-0.5B-Instruct | general_qa  | bleu-4          | default  |    12 |  0.0556 | default |
+---------------------+-------------+-----------------+----------+-------+---------+---------+
| Qwen2-0.5B-Instruct | general_qa  | rouge-1-f       | default  |    12 |  0.3441 | default |
+---------------------+-------------+-----------------+----------+-------+---------+---------+
| Qwen2-0.5B-Instruct | general_qa  | rouge-1-p       | default  |    12 |  0.2393 | default |
+---------------------+-------------+-----------------+----------+-------+---------+---------+
| Qwen2-0.5B-Instruct | general_qa  | rouge-1-r       | default  |    12 |  0.8889 | default |
+---------------------+-------------+-----------------+----------+-------+---------+---------+
| Qwen2-0.5B-Instruct | general_qa  | rouge-2-f       | default  |    12 |  0.2062 | default |
+---------------------+-------------+-----------------+----------+-------+---------+---------+
| Qwen2-0.5B-Instruct | general_qa  | rouge-2-p       | default  |    12 |  0.1453 | default |
+---------------------+-------------+-----------------+----------+-------+---------+---------+
| Qwen2-0.5B-Instruct | general_qa  | rouge-2-r       | default  |    12 |  0.6167 | default |
+---------------------+-------------+-----------------+----------+-------+---------+---------+
| Qwen2-0.5B-Instruct | general_qa  | rouge-l-f       | default  |    12 |  0.333  | default |
+---------------------+-------------+-----------------+----------+-------+---------+---------+
| Qwen2-0.5B-Instruct | general_qa  | rouge-l-p       | default  |    12 |  0.2324 | default |
+---------------------+-------------+-----------------+----------+-------+---------+---------+
| Qwen2-0.5B-Instruct | general_qa  | rouge-l-r       | default  |    12 |  0.8889 | default |
+---------------------+-------------+-----------------+----------+-------+---------+---------+
```