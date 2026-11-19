
# 采样

> 来源：[https://v11enp9ok1h.feishu.cn/docx/YQiedMiHHokX04xXszvcrExKnCb](https://v11enp9ok1h.feishu.cn/docx/YQiedMiHHokX04xXszvcrExKnCb)

采样是SWIFT新支持的重要能力之一，这部分可以理解为test-time compute的落地实现。同时，该能力对RFT（强化微调）的实现也至关重要。

## 能力介绍

SWIFT的`sample`能力可以使用下面的例子进行：

```bash
swift sample --model LLM-Research/Meta-Llama-3.1-8B-Instruct --sampler_engine pt --num_return_sequences 5 --dataset AI-ModelScope/alpaca-gpt4-data-zh#5
```

在当前文件夹的`sample_output`目录下，会生成以时间戳为文件名的`jsonl`文件，该文件应该包含25行，每一行都是一个完整`messages`格式的数据。

采样的参数列表请参考[这里](https://modelscope.cn/docs/llm-training-and-inference/user-guide/%E5%91%BD%E4%BB%A4%E8%A1%8C%E5%8F%82%E6%95%B0.md)。

## 环境准备

```bash
pip install ms-swift[llm] -U
```

或从源代码安装：

```bash
git clone https://github.com/modelscope/ms-swift.git
cd ms-swift
pip install -e '.[llm]'
```

## 使用PRM和ORM进行结果过滤

采样重要的能力就是对过程和结果进行监督，这可以通过设置额外参数来支持。

```bash
swift sample --model LLM-Research/Meta-Llama-3.1-8B-Instruct --sampler_engine lmdeploy --num_return_sequences 5 --n_best_to_keep 2 --dataset tastelikefeet/competition_math#5 --prm_model AI-ModelScope/GRM-llama3.2-3B-rewardmodel-ft --orm_model math
```

在当前文件夹的`sample_output`目录下，会生成以时间戳为文件名的`jsonl`文件，该文件至多包含10行，每一行都是一个完整`messages`格式的数据。

> 之所以至多包含10行，是因为虽然设置了共处理5个数据，每个数据保留2个（`n_best_to_keep`），但是`orm`可能会校验失败，失败数据不会保留到文件中。 另外，增加了`--prm_model`或`--orm_model`后文件格式有所不同，包含了`rejected_response` key，内容来自于`prm`评分最低的行。

## 自定义PRM或ORM

PRM和ORM的自定义可以在`plugin`中按照现有代码增加一个新的实现。例如：

```python
class CustomPRM:

    # 构造需要是无参的
    def __init__(self):
        # init here
        pass

    def __call__(self, infer_requests: List[InferRequest], ground_truths: List[str], **kwargs) -> List[Union[float, List[float]]]:
        ...


prms = {'custom': CustomPRM}
```

之后在命令行中使用`--prm_model custom`即可。

## 显存控制

如果被采样模型和PRM共同加载进显存，则可能出现`OOM`的问题。因此采样可以分为两段进行：

+ 第一段指定`--model`和`--sampler_engine`，同时不指定`--orm_model`和`--prm_model`，仅进行采样，并存储为文件

+ 第二段指定`--sampler_engine no`，指定`--orm_model`和`--prm_model`，并同时指定`--cache_files`，仅进行`RM`数据过滤，不重新采样

通过两段方式可以每次仅加载一个模型，防止`OOM`。

## 实际例子

请参考[强化微调脚本](https://github.com/modelscope/ms-swift/tree/main/examples/train/rft/rft.py)。该脚本给出了使用采样进行强化微调的实际例子。

> 注意：该脚本的实际效果和模型、数据、`RM`的质量强相关，因此仅作为样例出现，用户请自行修改该脚本并训练自己的`RM`和`generator`模型。

## 大模型蒸馏采样

SWIFT的`sample`支持使用OpenAI API的方式，用大模型蒸馏数据，如下示例：

```bash
OPENAI_API_KEY="your_api_key" \
swift sample \
    --sampler_type distill \
    --sampler_engine client \
    --model deepseek-r1 \
    --stream true \
    --dataset tastelikefeet/competition_math#5 \
    --num_return_sequences 1 \
    --temperature 0.6 \
    --top_p 0.95 \
    --engine_kwargs '{"base_url":"https://dashscope.aliyuncs.com/compatible-mode/v1"}'
```

在以上示例中，`base_url`和`model`分别是`api`地址和模型名称，`stream`表示发起请求的`stream`参数。

注意，对于`Deepseek-R1`系列模型，输出会被格式化为：`<think>{reasoning_content}</think>\n\n<answer>{content}</answer>`。
