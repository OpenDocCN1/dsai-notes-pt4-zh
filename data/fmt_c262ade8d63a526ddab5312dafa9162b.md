
# 03 | 训练微调openai模型

> 来源：[https://docs.feishu.cn/docx/B76jdMf8IowZHKx2RZscqVNonLc](https://docs.feishu.cn/docx/B76jdMf8IowZHKx2RZscqVNonLc)

## 微调

### 介绍

通过微调您可以通过提供以下内容从通过 API 提供的模型中获得更多收益：

1. 比提示设计更高质量的结果
1. 能够训练比提示所能容纳的更多示例
1. 由于提示时间较短可以节省 token
1. 更低的延迟请求

GPT-3 已经对来自开放互联网的大量文本进行了预训练。当给出一个只有几个例子的提示时，它通常可以直观地判断你正在尝试执行什么任务并生成一个合理的完成。这通常被称为“少镜头学习”。

微调通过训练比提示所能容纳的更多的示例来改进少数镜头学习，让您在大量任务上获得更好的结果。对模型进行微调后，无需再在提示中提供示例。这样可以节省成本并实现更低的延迟请求。

概括地说，微调涉及以下步骤：

1. 准备和上传训练数据
1. 训练新的微调模型
1. 使用微调模型

请访问我们的[定价页面](https://openai.com/api/pricing)详细了解如何对微调的模型训练和使用计费。

### 哪些模型可以微调

微调目前仅适用于以下基本型号：`davinci`、`curie`、`babbage`、`ada`、`text-davinci-003`。

### 安装

我们建议使用我们的 `OpenAI CLI`。要安装它，请运行

```
pip install --upgrade openai
```

(以下说明适用于版本 `0.9.4` 及更高版本。此外 `OpenAI CLI` 需要 `python` 3。

通过在 shell 初始化脚本(例如 `.bashrc`、`zshrc` 等)中添加以下行，或在微调命令之前的命令行中运行它，来设置环境变量 `OPENAI_API_KEY`。

### 准备训练数据

训练数据是你教 GPT-3 去说。

您的数据必须是 [JSONL](https://jsonlines.org/) 文档，其中每行都是一对提示完成。您可以使用我们的 [CLI 数据准备工具](https://platform.openai.com/docs/guides/fine-tuning/cli-data-preparation-tool)轻松地将数据转换为此文件格式。

```
{"prompt": "<prompt text>", "completion": "<ideal generated text>"}
{"prompt": "<prompt text>", "completion": "<ideal generated text>"}
{"prompt": "<prompt text>", "completion": "<ideal generated text>"}
...
```

#### CLI 数据准备工具

我们开发了一个工具来验证、提供建议和重新格式化您的数据

```
openai tools fine_tunes.prepare_data -f <LOCAL_FILE>
```

此工具接受不同的格式文件，唯一要求它们包含提示和完成列/键。您可以传递 CSV、TSV、XLSX、JSON 或 JSONL 文件，它会在指导您完成建议的更改过程后，将输出保存到 JSONL 文件中以便进行微调。

### 创建微调模型

以下假设你已按照上述说明准备了训练数据。

使用 `OpenAI CLI` 启动微调作业

```
openai api fine_tunes.create -t <TRAIN_FILE_ID_OR_PATH> -m <BASE_MODEL>
```

[`BASE_MODEL`](https://platform.openai.com/docs/guides/fine-tuning/customize-your-model-name) 是基础模型的名称(`ada`、`babbage`、`curie`、`davinci`)。您可以使用后缀参数自定义你的微调模型的名称。

运行上述命令会执行以下几项操作：

1. 使用文件 API 上传文件（或使用已上传的文件）
1. 创建微调作业
1. 流式传输事件直到作业完成（这通常需要几分钟，但如果队列中有许多作业或数据集很大，则可能需要数小时）

每个微调作业都从基础模型开始，默认是 `curie`。模型的选择会影响模型的性能和运行微调模型的成本。

开始微调作业后可能需要一些时间才能完成。你的作业可能排在系统上的其他作业后面，训练模型可能需要几分钟或几小时，具体取决于模型和数据集大小。如果事件流因任何原因中断，您可以通过运行以下命令来恢复它

```
openai api fine_tunes.follow -i <YOUR_FINE_TUNE_JOB_ID>
```

作业完成后，它会显示微调模型的名称。

除了创建微调作业之外，您还可以列出现有作业、检索作业状态或取消作业。

```
# List all created fine-tunes
openai api fine_tunes.list

# Retrieve the state of a fine-tune. The resulting object includes
# job status (which can be one of pending, running, succeeded, or failed)
# and other information
openai api fine_tunes.get -i <YOUR_FINE_TUNE_JOB_ID>

# Cancel a job
openai api fine_tunes.cancel -i <YOUR_FINE_TUNE_JOB_ID>
```

### 使用微调模型

`OpenAI CLI`

```
openai api completions.create -m <FINE_TUNED_MODEL> -p <YOUR_PROMPT>
```

`curl`

```
curl https://api.openai.com/v1/completions \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"prompt": YOUR_PROMPT, "model": FINE_TUNED_MODEL}'
```

`Python`:

```
import openai
openai.Completion.create(
    model=FINE_TUNED_MODEL,
    prompt=YOUR_PROMPT)
```

`Node.js`:

```
const response = await openai.createCompletion({
  model: FINE_TUNED_MODEL,
  prompt: YOUR_PROMPT,
});
```
