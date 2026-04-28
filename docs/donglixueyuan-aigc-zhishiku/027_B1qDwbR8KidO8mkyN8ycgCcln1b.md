# 03 | 训练微调openai模型

> 来源：[https://docs.feishu.cn/docx/B76jdMf8IowZHKx2RZscqVNonLc](https://docs.feishu.cn/docx/B76jdMf8IowZHKx2RZscqVNonLc)

# **微调**

了解如何为应用程序自定义模型。

## **介绍**

通过微调您可以通过提供以下内容从通过 API 提供的模型中获得更多收益

1.  比提示设计更高质量的结果

1.  能够训练比提示所能容纳的更多示例

1.  由于提示时间较短可以节省token

1.  更低的延迟请求

GPT-3 已经对来自开放互联网的大量文本进行了预训练。当给出一个只有几个例子的提示时它通常可以直观地判断你正在尝试执行什么任务并生成一个合理的完成。这通常被称为“少镜头学习”。

微调通过训练比提示所能容纳的更多的示例来改进少数镜头学习让您在大量任务上获得更好的结果。对模型进行微调后无需再在提示中提供示例。这样可以节省成本并实现更低的延迟请求。

概括地说微调涉及以下步骤

1.  准备和上传训练数据

1.  训练新的微调模型

1.  使用微调模型

请访问我们的[定价页面](https://openai.com/api/pricing)详细了解如何对微调的模型训练和使用计费。

## **哪些模型可以微调**

微调目前仅适用于以下基本型号davinci  curie  babbage  ada   text-davinci-003。

## **安装**

我们建议使用我们的 OpenAI 命令行界面 (CLI)。要安装它请运行

```
pip install --upgrade openai
```

(以下说明适用于版本 **0.9.4** 及更高版本。此外OpenAI CLI 需要 python 3。

通过在 shell 初始化脚本(例如 .bashrc、zshrc 等)中添加以下行或在微调命令之前的命令行中运行它来设置环境变量OPENAI_API_KEY

## **准备****训练数据**

训练数据是你教 GPT-3去说。

您的数据必须是 [JSONL](https://jsonlines.org/)[ 文档其中每行都是对](https://platform.openai.com/docs/guides/fine-tuning/cli-data-preparation-tool)应于训练示例的一对提示完成。您可以使用我们的 CLI 数据准备工具轻松地将数据转换为此文件格式。

```
{&quot;prompt&quot;: &quot;&lt;prompt text&gt;&quot;, &quot;completion&quot;: &quot;&lt;ideal generated text&gt;&quot;}
{&quot;prompt&quot;: &quot;&lt;prompt text&gt;&quot;, &quot;completion&quot;: &quot;&lt;ideal generated text&gt;&quot;}
{&quot;prompt&quot;: &quot;&lt;prompt text&gt;&quot;, &quot;completion&quot;: &quot;&lt;ideal generated text&gt;&quot;}
...
```

### **CLI**** 数据准备工具**

我们开发了一个工具来验证、提供建议和重新格式化您的数据

```
openai tools fine_tunes.prepare_data -f &lt;LOCAL_FILE&gt;
```

**此工具****接****受不同****的格式文件唯****一要求它**们包含**提示和完成**列/键。您可以传递 CSV、TSV、XLSX、JSON 或 JSONL 文件它会在指导您完成建议的更改过程后将输出保存到 JSONL 文件中以便进行微调。

## **创建微调模型**

以下假设你已按照上述说明准备了训练数据。

使用 OpenAI CLI 启动微调作业

```
openai api fine_tunes.create -t &lt;TRAIN_FILE_ID_OR_PATH&gt; -m &lt;BASE_MODEL&gt;
```

BAS[E_M](https://platform.openai.com/docs/guides/fine-tuning/customize-your-model-name)ODEL是基础模型的名称(ada, babbage, curie, davinci)。您可以使用后缀参数自定义你的微调模型的名称。

运行上述命令会执行以下几项操作

1.  使用文件 API 上传文件(或使用已上传的文件)

1.  创建微调作业

1.  流式传输事件直到作业完成(这通常需要几分钟但如果队列中有许多作业或数据集很大则可能需要数小时)

每个微调作业都从基础模型开始默认是curie。模型的选择会影响模型的性能和运行微调模型的成本。

开始微调作业后可能需要一些时间才能完成。你的作业可能排在我们系统上的其他作业后面训练我们的模型可能需要几分钟或几小时具体取决于模型和数据集大小。如果事件流因任何原因中断您可以通过运行以下命令来恢复它

```
openai api fine_tunes.follow -i &lt;YOUR_FINE_TUNE_JOB_ID&gt;
```

作业完成后它会显示微调模型的名称。

除了创建微调作业之外您还可以列出现有作业、检索作业状态或取消作业。

```
#List all created fine-tunes
openai api fine_tunes.list

# Retrieve the state of a fine-tune. The resulting object includes
# job status (which can be one of pending, running, succeeded, or failed)
# and other information
openai api fine_tunes.get -i &lt;YOUR_FINE_TUNE_JOB_ID&gt;

# Cancel a job
openai api fine_tunes.cancel -i &lt;YOUR_FINE_TUNE_JOB_ID&gt;
```

## **使用微调模型**

OpenAI CLI

```
openai api completions.create -m &lt;FINE_TUNED_MODEL&gt; -p &lt;YOUR_PROMPT&gt;
```

curl

```
curl https://api.openai.com/v1/completions \
  -H &quot;Authorization: Bearer $OPENAI_API_KEY&quot; \
  -H &quot;Content-Type: application/json&quot; \
  -d &#x27;{&quot;prompt&quot;: YOUR_PROMPT, &quot;model&quot;: FINE_TUNED_MODEL}&#x27;
```

Python:

```
import openai
openai.Completion.create(
    model=FINE_TUNED_MODEL,
    prompt=YOUR_PROMPT)
```

Node.js

```
const response = await openai.createCompletion({
  model: FINE_TUNED_MODEL
  prompt: YOUR_PROMPT,
});
```