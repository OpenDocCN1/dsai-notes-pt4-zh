# 微调（Fine-tuning）

> 来源：[https://waytoagi.feishu.cn/docx/QhqudjuGtoZi8xxQS9ocuwCNn6d](https://waytoagi.feishu.cn/docx/QhqudjuGtoZi8xxQS9ocuwCNn6d)

了解如何为您的应用程序定制模型。

## 介绍

通过提供以下内容，微调可让您从 API 提供的模型中获得更多收益：

1.  比即时设计更高质量的结果

1.  能够训练比提示中更多的例子

1.  由于更短的提示而节省了Token

1.  更低的延迟请求

GPT-3 已经在来自开放互联网的大量文本上进行了预训练。当给出仅包含几个示例的提示时，它通常可以凭直觉判断出您要执行的任务并生成合理的完成。这通常称为“小样本学习”。

微调通过训练比提示中更多的示例来改进小样本学习，让您在大量任务中取得更好的结果。对模型进行微调后，您将不再需要在提示中提供示例。这样可以节省成本并实现更低延迟的请求。

在高层次上，微调涉及以下步骤：

1.  准备和上传训练数据

1.  训练新的微调模型

1.  使用您的微调模型

## 哪些模型可以微调？

微调目前仅适用于以下基础模型：davinci、curie、babbage和ada。这些是原始模型，在训练后没有任何说明（例如text-davinci-003）。您还可以继续微调微调模型以添加其他数据，而无需从头开始。

## 安装

我们建议使用我们的 OpenAI 命令行界面 (CLI)。要安装这个，运行

```
pip install --upgrade openai
```

（以下说明适用于**0.9.4**及更高版本。此外，OpenAI CLI 需要 python 3。）

OPENAI_API_KEY通过将以下行添加到您的 shell 初始化脚本（例如 .bashrc、zshrc 等）或在微调命令之前的命令行中运行它来设置您的环境变量：

```
export OPENAI_API_KEY=&quot;&lt;OPENAI_API_KEY&gt;&quot;
```

## 准备训练数据

训练数据是你如何教 GPT-3 你想让它说什么。

您的数据必须是JSONL文档，其中每一行都是一个提示完成对，对应于一个训练示例。您可以使用我们的CLI 数据准备工具轻松地将您的数据转换成这种文件格式。

```
{&quot;prompt&quot;: &quot;&lt;prompt text&gt;&quot;, &quot;completion&quot;: &quot;&lt;ideal generated text&gt;&quot;}
{&quot;prompt&quot;: &quot;&lt;prompt text&gt;&quot;, &quot;completion&quot;: &quot;&lt;ideal generated text&gt;&quot;}
{&quot;prompt&quot;: &quot;&lt;prompt text&gt;&quot;, &quot;completion&quot;: &quot;&lt;ideal generated text&gt;&quot;}
...
```

设计用于微调的提示和补全不同于设计用于我们的基本模型（Davinci、Curie、Babbage、Ada）的提示。特别是，虽然基础模型的提示通常包含多个示例（“小样本学习”），但对于微调，每个训练示例通常包含一个输入示例及其相关输出，无需给出详细说明或在同一提示中包含多个示例。

有关如何为各种任务准备训练数据的更多详细指导，请参阅我们准备数据集的最佳实践。

您拥有的训练示例越多越好。我们建议至少有几百个示例。一般来说，我们发现数据集大小每增加一倍都会导致模型质量线性增加。

### CLI 数据准备工具

我们开发了一个工具来验证、提供建议和重新格式化您的数据：

```
openai tools fine_tunes.prepare_data -f &lt;LOCAL_FILE&gt;
```

此**工具接受不**同的格式，唯一的要求是它们包含提示和完成列/键。您可以传递CSV、TSV、XLSX、JSON或JSONL文件，它会在指导您完成建议的更改过程后将输出保存到 JSONL 文件中以备微调。

## 创建微调模型

以下假设您已经按照上述说明准备了训练数据。

使用 OpenAI CLI 开始微调工作：

```
openai api fine_tunes.create -t &lt;TRAIN_FILE_ID_OR_PATH&gt; -m &lt;BASE_MODEL&gt;
```

您从哪里BASE_MODEL开始的基本模型的名称（ada、babbage、curie 或 davinci）。您可以使用后缀参数自定义微调模型的名称。

运行上面的命令会做几件事：

1.  使用文件 API上传文件（或使用已经上传的文件）

1.  创建微调作业

1.  流式传输事件直到作业完成（这通常需要几分钟，但如果队列中有很多作业或您的数据集很大，则可能需要数小时）

每个微调工作都从一个默认为curie的基本模型开始。模型的选择会影响模型的性能和运行微调模型的成本。您的模型可以是以下之一：ada、babbage、curie或davinci。请访问我们的定价页面，了解有关微调费率的详细信息。

开始微调作业后，可能需要一些时间才能完成。在我们的系统中，您的工作可能排在其他工作之后，训练我们的模型可能需要几分钟或几小时，具体取决于模型和数据集的大小。如果事件流因任何原因中断，您可以通过运行以下命令恢复它：

```
openai api fine_tunes.follow -i &lt;YOUR_FINE_TUNE_JOB_ID&gt;
```

工作完成后，它应该显示微调模型的名称。

除了创建微调作业外，您还可以列出现有作业、检索作业状态或取消作业。

```
# List all created fine-tunes
openai api fine_tunes.list

# Retrieve the state of a fine-tune. The resulting object includes
# job status (which can be one of pending, running, succeeded, or failed)
# and other information
openai api fine_tunes.get -i &lt;YOUR_FINE_TUNE_JOB_ID&gt;

# Cancel a job
openai api fine_tunes.cancel -i &lt;YOUR_FINE_TUNE_JOB_ID&gt;
```

## 使用微调模型

当作业成功时，该fine_tuned_model字段将填充模型名称。您现在可以将此模型指定为我们的 Completions API 的参数，并使用Playground向它发出请求。

在您的工作首次完成后，您的模型可能需要几分钟时间才能准备好处理请求。如果对您的模型的完成请求超时，可能是因为您的模型仍在加载中。如果发生这种情况，请在几分钟后重试。

您可以通过将模型名称作为model完成请求的参数传递来开始发出请求：

OpenAI 命令行界面：

```
openai api completions.create -m &lt;FINE_TUNED_MODEL&gt; -p &lt;YOUR_PROMPT&gt;
```

cURL：

```
curl https://api.openai.com/v1/completions \
  -H &quot;Authorization: Bearer $OPENAI_API_KEY&quot; \
  -H &quot;Content-Type: application/json&quot; \
  -d &#x27;{&quot;prompt&quot;: YOUR_PROMPT, &quot;model&quot;: FINE_TUNED_MODEL}&#x27;
```

Python：

```
import openai
openai.Completion.create(
    model=FINE_TUNED_MODEL,
    prompt=YOUR_PROMPT)
```

Node.js：

```
const response = await openai.createCompletion({
  model: FINE_TUNED_MODEL
  prompt: YOUR_PROMPT,
});
```

您可以继续使用所有其他完成参数，如temperature 等，对这些frequency_penalty请求presence_penalty进行微调模型。

## 删除微调模型

要删除微调模型，您必须在您的组织中被指定为“所有者”。

OpenAI 命令行界面：

```
openai api models.delete -i &lt;FINE_TUNED_MODEL&gt;
```

cURL：

```
curl -X &quot;DELETE&quot; https://api.openai.com/v1/models/&lt;FINE_TUNED_MODEL&gt; \
  -H &quot;Authorization: Bearer $OPENAI_API_KEY&quot;
```

Python：

```
import openai
openai.Model.delete(FINE_TUNED_MODEL)
```

## 准备数据集

**微调是一种**强大的技术，可用于创建特定于您的用例的新模型。在微调您的模型之前，我们强烈建议您阅读以下针对您的用例的最佳实践和具体指南。

## 数据格式化

要微调模型，您需要一组训练示例，每个训练示例都包含一个输入（“提示”）及其关联的输出（“完成”）。这与使用我们的基本模型明显不同，在基本模型中，您可能会在单个提示中输入详细说明或多个示例。

+   每个提示都应以固定分隔符结尾，以在提示结束和完成开始时通知模型。通常效果很好的简单分隔符是\n\n###\n\n. 分隔符不应出现在任何提示中的其他地方。

+   由于我们的token化，每个完成都应该以空格开头，它用前面的空格token大多数单词。

+   每次完成都应以固定的停止序列结束，以在完成结束时通知模型。停止序列可以是\n、###或任何其他未出现在任何完成中的token。

+   对于推理，您应该按照与创建训练数据集时相同的方式格式化提示，包括相同的分隔符。还指定相同的停止序列以正确截断完成。

## 一般最佳实践

使用更多高质量的示例进行微调效果更好。要微调一个比使用我们的基本模型使用高质量提示更好地执行的模型，您应该提供至少几百个高质量的示例，最好由人类专家审查。从那里开始，性能往往会随着示例数量的每增加一倍而线性增加。增加示例的数量通常是提高性能的最佳和最可靠的方法。

分类器是最容易上手的模型。对于分类问题，我们建议使用 ada，经过微调后，它通常只会比功能更强大的模型稍微差一点，同时速度更快，成本更低。

如果您要对预先存在的数据集进行微调，而不是从头开始编写提示，请务必在可能的情况下手动检查您的数据是否存在令人反感或不准确的内容，或者如果数据集很大，请检查尽可能多的随机样本。

## 具体指南

微调可以解决多种问题，最佳使用方式可能取决于您的具体用例。下面，我们列出了最常见的微调用例和相应的指南。

+   [分类](https://platform.openai.com/docs/guides/fine-tuning/classification)

+   [该模型是否做出了不真实的陈述？](https://platform.openai.com/docs/guides/fine-tuning/case-study-is-the-model-making-untrue-statements)

+   [情绪分析](https://platform.openai.com/docs/guides/fine-tuning/case-study-sentiment-analysis)

+   [电子邮件分类的分类](https://platform.openai.com/docs/guides/fine-tuning/case-study-categorization-for-email-triage)

+   [条件生成](https://platform.openai.com/docs/guides/fine-tuning/conditional-generation)

+   根据维基百科文章撰写引人入胜的广告

+   [实体提取](https://platform.openai.com/docs/guides/fine-tuning/case-study-entity-extraction)

+   [客户支持聊天机器人](https://platform.openai.com/docs/guides/fine-tuning/case-study-customer-support-chatbot)

+   [基于技术属性列表的产品描述](https://platform.openai.com/docs/guides/fine-tuning/case-study-product-description-based-on-a-technical-list-of-properties)

### 分类

在分类问题中，提示中的每个输入都应分类到预定义的类别之一。对于此类问题，我们建议：

+   在提示末尾使用分隔符，例如\n\n###\n\n. 当您最终向您的模型发出请求时，请记住还要附加此分隔符。

+   选择映射到单个token的类。在推理时，请指定max_tokens=1，因为您只需要第一个token进行分类。

+   确保提示+完成不超过 2048 个token，包括分隔符

+   目标是每班至少 ~100 个例子

+   logprobs=5要获得类日志概率，您可以在使用模型时指定（对于 5 个类）

+   确保用于微调的数据集在结构和任务类型上与模型将用于的数据集非常相似

#### 案例研究：模型是否做出了不真实的陈述？

假设您希望确保您网站上的广告文字提及正确的产品和公司。换句话说，您要确保模型没有胡编乱造。您可能想要微调过滤掉不正确广告的分类器。

数据集可能类似于以下内容：

```
{&quot;prompt&quot;:&quot;Company: BHFF insurance\nProduct: allround insurance\nAd:One stop shop for all your insurance needs!\nSupported:&quot;, &quot;completion&quot;:&quot; yes&quot;}
{&quot;prompt&quot;:&quot;Company: Loft conversion specialists\nProduct: -\nAd:Straight teeth in weeks!\nSupported:&quot;, &quot;completion&quot;:&quot; no&quot;}
```

在上面的示例中，我们使用了包含公司名称、产品和相关广告的结构化输入。作为分隔符，我们使用\nSupported:它清楚地将提示与完成分开。对于足够数量的示例，分隔符不会产生太大影响（通常小于 0.4%），只要它没有出现在提示或完成中即可。

对于这个用例，我们微调了一个 ada 模型，因为它会更快、更便宜，而且性能将与更大的模型相当，因为它是一个分类任务。

现在我们可以通过发出完成请求来查询我们的模型。

```
curl https://api.openai.com/v1/completions \
  -H &quot;Content-Type: application/json&quot; \
  -H &quot;Authorization: Bearer $OPENAI_API_KEY&quot; \
  -d &#x27;{
    &quot;prompt&quot;: &quot;Company: Reliable accountants Ltd\nProduct: Personal Tax help\nAd:Best advice in town!\nSupported:&quot;,
    &quot;max_tokens&quot;: 1,
    &quot;model&quot;: &quot;YOUR_FINE_TUNED_MODEL_NAME&quot;
  }&#x27;
```

哪个将返回 yesor no。

#### 案例研究：情绪分析

假设您想要了解特定推文的正面或负面程度。数据集可能类似于以下内容：

```
{&quot;prompt&quot;:&quot;Overjoyed with the new iPhone! -&gt;&quot;, &quot;completion&quot;:&quot; positive&quot;}
{&quot;prompt&quot;:&quot;@lakers disappoint for a third straight night https://t.co/38EFe43 -&gt;&quot;, &quot;completion&quot;:&quot; negative&quot;}
```

对模型进行微调后，您可以通过在logprobs=2完成请求上设置来取回第一个完成令牌的对数概率。正类别的概率越高，相对情绪就越高。

现在我们可以通过发出完成请求来查询我们的模型。

```
curl https://api.openai.com/v1/completions \
  -H &quot;Content-Type: application/json&quot; \
  -H &quot;Authorization: Bearer $OPENAI_API_KEY&quot; \
  -d &#x27;{
    &quot;prompt&quot;: &quot;https://t.co/f93xEd2 Excited to share my latest blog post! -&gt;&quot;,
    &quot;max_tokens&quot;: 1,
    &quot;model&quot;: &quot;YOUR_FINE_TUNED_MODEL_NAME&quot;
  }&#x27;
```

哪个将返回：

```
{
  &quot;id&quot;: &quot;cmpl-COMPLETION_ID&quot;,
  &quot;object&quot;: &quot;text_completion&quot;,
  &quot;created&quot;: 1589498378,
  &quot;model&quot;: &quot;YOUR_FINE_TUNED_MODEL_NAME&quot;,
  &quot;choices&quot;: [
    {
      &quot;logprobs&quot;: {
        &quot;text_offset&quot;: [
          19
        ],
        &quot;token_logprobs&quot;: [
          -0.03597255
        ],
        &quot;tokens&quot;: [
          &quot; positive&quot;
        ],
        &quot;top_logprobs&quot;: [
          {
            &quot; negative&quot;: -4.9785037,
            &quot; positive&quot;: -0.03597255
          }
        ]
      },

      &quot;text&quot;: &quot; positive&quot;,
      &quot;index&quot;: 0,
      &quot;finish_reason&quot;: &quot;length&quot;
    }
  ]
}
```

#### 案例研究：电子邮件分类的分类

假设您希望将收到的电子邮件归入大量预定义类别之一。对于大量类别的分类，我们建议您将这些类别转换为数字，最多可处理约 500 个类别。我们观察到，由于token化，在数字前添加一个空格有时会对性能略有帮助。您可能希望按如下方式构建训练数据：

```
{&quot;prompt&quot;:&quot;Subject: &lt;email_subject&gt;\nFrom:&lt;customer_name&gt;\nDate:&lt;date&gt;\nContent:&lt;email_body&gt;\n\n###\n\n&quot;, &quot;completion&quot;:&quot; &lt;numerical_category&gt;&quot;}
```

例如：

```
{&quot;prompt&quot;:&quot;Subject: Update my address\nFrom:Joe Doe\nTo:support@ourcompany.com\nDate:2021-06-03\nContent:Hi,\nI would like to update my billing address to match my delivery address.\n\nPlease let me know once done.\n\nThanks,\nJoe\n\n###\n\n&quot;, &quot;completion&quot;:&quot; 4&quot;}
```

在上面的示例中，我们使用了一封上限为 2043 个令牌的传入电子邮件作为输入。（这允许使用 4 个token分隔符和一个token完成，总计为 2048。）作为分隔符，我们使用并删除了电子邮件中\n\n###\n\n出现的所有内容。###

### 条件生成

条件生成是需要在给定某种输入的情况下生成内容的问题。这包括释义、总结、实体提取、编写给定规范的产品描述、聊天机器人等。对于此类问题，我们建议：

+   在提示末尾使用分隔符，例如\n\n###\n\n. 当您最终向您的模型发出请求时，请记住还要附加此分隔符。

+   在完成结束时使用结束token，例如 END

+   请记住在推理过程中将结束token添加为停止序列，例如stop=[" END"]

+   目标是至少 ~500 个示例

+   确保提示+完成不超过 2048 个token，包括分隔符

+   确保示例具有高质量并遵循相同的所需格式

+   确保用于微调的数据集在结构和任务类型上与模型将用于的数据集非常相似

+   使用较低的学习率和仅 1-2 个时期往往更适合这些用例

#### 案例研究：根据维基百科文章撰写引人入胜的广告

这是一个生成用例，因此您需要确保提供的样本具有最高质量，因为微调模型将尝试模仿给定示例的风格（和错误）。一个好的起点是大约 500 个示例。示例数据集可能如下所示：

```
{&quot;prompt&quot;:&quot;&lt;Product Name&gt;\n&lt;Wikipedia description&gt;\n\n###\n\n&quot;, &quot;completion&quot;:&quot; &lt;engaging ad&gt; END&quot;}
```

例如：

```
{&quot;prompt&quot;:&quot;Samsung Galaxy Feel\nThe Samsung Galaxy Feel is an Android smartphone developed by Samsung Electronics exclusively for the Japanese market. The phone was released in June 2017 and was sold by NTT Docomo. It runs on Android 7.0 (Nougat), has a 4.7 inch display, and a 3000 mAh battery.\nSoftware\nSamsung Galaxy Feel runs on Android 7.0 (Nougat), but can be later updated to Android 8.0 (Oreo).\nHardware\nSamsung Galaxy Feel has a 4.7 inch Super AMOLED HD display, 16 MP back facing and 5 MP front facing cameras. It has a 3000 mAh battery, a 1.6 GHz Octa-Core ARM Cortex-A53 CPU, and an ARM Mali-T830 MP1 700 MHz GPU. It comes with 32GB of internal storage, expandable to 256GB via microSD. Aside from its software and hardware specifications, Samsung also introduced a unique a hole in the phone&#x27;s shell to accommodate the Japanese perceived penchant for personalizing their mobile phones. The Galaxy Feel&#x27;s battery was also touted as a major selling point since the market favors handsets with longer battery life. The device is also waterproof and supports 1seg digital broadcasts using an antenna that is sold separately.\n\n###\n\n&quot;, &quot;completion&quot;:&quot;Looking for a smartphone that can do it all? Look no further than Samsung Galaxy Feel! With a slim and sleek design, our latest smartphone features high-quality picture and video capabilities, as well as an award winning battery life. END&quot;}
```

这里我们使用了多行分隔符，因为维基百科文章包含多个段落和标题。我们还使用了一个简单的结束token，以确保模型知道何时应该完成完成。

#### 案例研究：实体提取

这类似于语言转换任务。为了提高性能，最好按字母顺序或按照它们在原始文本中出现的相同顺序对不同的提取实体进行排序。这将有助于模型跟踪需要按顺序生成的所有实体。数据集可能如下所示：

```
{&quot;prompt&quot;:&quot;&lt;any text, for example news article&gt;\n\n###\n\n&quot;, &quot;completion&quot;:&quot; &lt;list of entities, separated by a newline&gt; END&quot;}
```

例如：

```
{&quot;prompt&quot;:&quot;Portugal will be removed from the UK&#x27;s green travel list from Tuesday, amid rising coronavirus cases and concern over a \&quot;Nepal mutation of the so-called Indian variant\&quot;. It will join the amber list, meaning holidaymakers should not visit and returnees must isolate for 10 days...\n\n###\n\n&quot;, &quot;completion&quot;:&quot; Portugal\nUK\nNepal mutation\nIndian variant END&quot;}
```

多行分隔符效果最好，因为文本可能包含多行。理想情况下，输入提示的类型会高度多样化（新闻文章、维基百科页面、推文、法律文件），这反映了提取实体时可能遇到的文本。

#### 案例研究：客户支持聊天机器人

聊天机器人通常会包含有关对话的相关上下文（订单详细信息）、到目前为止的对话摘要以及最近的消息。对于这个用例，相同的过去对话可以在数据集中生成多行，每次都有稍微不同的上下文，对于每个代理生成作为完成。这个用例将需要几千个示例，因为它可能会处理不同类型的请求和客户问题。为确保高质量的性能，我们建议审查对话样本以确保代理消息的质量。可以使用单独的文本转换微调模型生成摘要。数据集可能如下所示：

```
{&quot;prompt&quot;:&quot;Summary: &lt;summary of the interaction so far&gt;\n\nSpecific information:&lt;for example order details in natural language&gt;\n\n###\n\nCustomer: &lt;message1&gt;\nAgent: &lt;response1&gt;\nCustomer: &lt;message2&gt;\nAgent:&quot;, &quot;completion&quot;:&quot; &lt;response2&gt;\n&quot;}
{&quot;prompt&quot;:&quot;Summary: &lt;summary of the interaction so far&gt;\n\nSpecific information:&lt;for example order details in natural language&gt;\n\n###\n\nCustomer: &lt;message1&gt;\nAgent: &lt;response1&gt;\nCustomer: &lt;message2&gt;\nAgent: &lt;response2&gt;\nCustomer: &lt;message3&gt;\nAgent:&quot;, &quot;completion&quot;:&quot; &lt;response3&gt;\n&quot;}
```

在这里，我们有意分离不同类型的输入信息，但在提示和完成之间以相同的格式维护客户代理对话框。所有的完成都应该只由代理完成，我们可以\n在进行推理时用作停止序列。

#### 案例研究：基于技术属性列表的产品描述

在这里，将输入数据转换为自然语言很重要，这可能会带来卓越的性能。例如，以下格式：

```
{&quot;prompt&quot;:&quot;Item=handbag, Color=army_green, price=$99, size=S-&gt;&quot;, &quot;completion&quot;:&quot; This stylish small green handbag will add a unique touch to your look, without costing you a fortune.&quot;}
```

不会像以下那样工作：

```
{&quot;prompt&quot;:&quot;Item is a handbag. Colour is army green. Price is midrange. Size is small.-&gt;&quot;, &quot;completion&quot;:&quot; This stylish small green handbag will add a unique touch to your look, without costing you a fortune.&quot;}
```

为了获得高性能，请确保完成是基于所提供的描述。如果经常查阅外部内容，则以自动方式添加此类内容将提高性能。如果描述基于图像，则使用算法提取图像的文本描述可能会有所帮助。由于完成只有一个句子长，我们可以.在推理过程中用作停止序列。

## 高级用法

## 自定义您的模型名称

您可以使用后缀参数将最多 40 个字符的后缀添加到经过微调的模型名称中。

OpenAI 命令行界面：

```
openai api fine_tunes.create -t test.jsonl -m ada --suffix &quot;custom model name&quot;
```

结果名称将是：

```
ada:ft-your-org:custom-model-name-2022-02-15-04-21-04
```

## 分析您的微调模型

我们会在每个作业完成后附上一个结果文件。当您检索微调时以及查看微调中的事件时，将列出此结果文件 ID。您可以下载这些文件：

OpenAI 命令行界面：

```
openai api fine_tunes.results -i &lt;YOUR_FINE_TUNE_JOB_ID&gt;
```

cURL：

```
curl https://api.openai.com/v1/files/$RESULTS_FILE_ID/content \
  -H &quot;Authorization: Bearer $OPENAI_API_KEY&quot; &gt; results.csv
```

该_results.csv文件为每个训练步骤包含一行，其中一个步骤是指对一批数据的一次前向和反向传递。除步骤编号外，每行还包含与该步骤对应的以下字段：

+   **elapsed_tokens**：模型到目前为止已经看到的令牌数（包括重复）

+   elapsed_examples：模型到目前为止已经看到的示例数量（包括重复），其中一个示例是您的批次中的一个元素。例如，如果batch_size = 4，每一步将增加elapsed_examples4。

+   **training_loss**：训练批次的损失

+   training_sequence_accuracy ：训练批次**中模**型的预测token与真实完成token完全匹配的完成百分比。例如，如果 a 为batch_size3，如果您的数据包含补全 [ [1, 2] , [0, 5] , [4, 2] ] 和模型预测 [ [1, 1] , [0, 5] ，[4, 2] ]，这个准确度将是 2/3 = 0.67

+   training_token_accu**racy**：模型正确预测的训练批次中token的百分比。例如，如果 a 为batch_size3，如果您的数据包含补全 [ [1, 2] , [0, 5] , [4, 2] ] 和模型预测 [ [1, 1] , [0, 5] ，[4, 2] ]，这个准确度将是 5/6 = 0.83

### 分类特定指标

我们还提供了在结果文件中生成其他特定于分类的指标的选项，例如准确性和加权 F1 分数。这些指标是根据完整的验证集和微调结束时定期计算的。您将在结果文件中看到它们作为附加列。

要启用此功能，请设置参数--compute_classification_metrics。此外，您必须提供一个验证文件，并classification_n_classes为多类分类设置参数，或classification_positive_class为二元分类设置参数。

OpenAI 命令行界面：

```
# For multiclass classification
openai api fine_tunes.create \
  -t &lt;TRAIN_FILE_ID_OR_PATH&gt; \
  -v &lt;VALIDATION_FILE_OR_PATH&gt; \
  -m &lt;MODEL&gt; \
  --compute_classification_metrics \
  --classification_n_classes &lt;N_CLASSES&gt;

# For binary classification
openai api fine_tunes.create \
  -t &lt;TRAIN_FILE_ID_OR_PATH&gt; \
  -v &lt;VALIDATION_FILE_OR_PATH&gt; \
  -m &lt;MODEL&gt; \
  --compute_classification_metrics \
  --classification_n_classes 2 \
  --classification_positive_class &lt;POSITIVE_CLASS_FROM_DATASET&gt;
```

如果您设置以下指标将显示在您的结果文件--compute_classification_metrics中：

##### 对于多类分类

+   **分类/准确度**：准确度

+   classification/weighted_f1_score : 加权 F-1 分数

##### 对于二进制分类

以下指标基于 0.5 的分类阈值（即当概率 > 0.5 时，示例被分类为属于正类。）

+   **分类/准确度**

+   **分类/精度**

+   **分类/召回**

+   **分类/f{beta}**

+   **分类/auroc** - AUROC

+   **分类/auprc** - AUPRC

请注意，这些评估假设您正在为将token化为单个token的类使用文本标签，如上所述。如果这些条件不成立，您得到的数字很可能是错误的。

### 验证

您可以保留一些数据以供验证。验证文件与训练文件具有完全相同的格式，并且您的训练数据和验证数据应该互斥。

如果您在创建微调作业时包含验证文件，则生成的结果文件将包括对微调模型在训练期间定期对验证数据执行情况的评估。

OpenAI 命令行界面：

```
openai api fine_tunes.create -t &lt;TRAIN_FILE_ID_OR_PATH&gt; \
  -v &lt;VALIDATION_FILE_ID_OR_PATH&gt; \
  -m &lt;MODEL&gt;
```

如果您提供了验证文件，我们会在训练期间定期计算批量验证数据的指标。您将在结果文件中看到以下附加指标：

+   **validation_loss**：验证批次的损失

+   validation_sequence_accuracy：验证批次中模型的预测token与真实完成token完全匹配的完成百分比。例如，如果 a 为batch_size3，如果您的数据包含完成 [ [1, 2] , [0, 5] , [4, 2] ] 和模型预测 [ [1, 1] , [0, 5] ，[4, 2] ]，这个准确度将是 2/3 = 0.67

+   validation_token_accuracy：模型正确预测的验证批次中token的百分比。例如，如果 a 为batch_size3，如果您的数据包含完成 [ [1, 2] , [0, 5] , [4, 2] ] 和模型预测 [ [1, 1] , [0, 5] ，[4, 2] ]，这个准确度将是 5/6 = 0.83

## 超参数

我们选择了适用于一系列用例的默认超参数。唯一需要的参数是训练文件。

也就是说，调整用于微调的超参数通常可以产生产生更高质量输出的模型。特别是，您可能需要配置以下内容：

+   model：要微调的基本模型的名称。您可以选择“ada”、“babbage[”、](https://ywh1bkansf.feishu.cn/wiki/R70MwasSpik2tgkCr7dc9eTmn0o)“curie”或“davinci”之一。要了解有关这些模型的更多信息，请参阅模型文档。

+   n_epochs- 默认为 4。训练模型的时期数。一个纪元指的是训练数据集的一个完整周期。

+   batch_size- 默认为训练集中示例数量的 0.2%，上限为 256。批量大小是用于训练单个正向和反向传递的训练示例数。总的来说，我们发现更大的批次大小往往更适用于更大的数据集。

+   learning_rate_multiplier- 默认为 0.05、0.1 或 0.2，具体取决于 final batch_size。微调学习率是用于预训练的原始学习率乘以该乘数。我们建议使用 0.02 到 0.2 范围内的值进行试验，以查看产生最佳结果的值。根据经验，我们发现较大的学习率通常在较大的批量大小下表现更好。

+   compute_classification_metrics- 默认为假。如果为 True，为了对分类任务进行微调，在每个 epoch 结束时在验证集上计算特定于分类的指标（准确性、F-1 分数等）。

要配置这些额外的超参数，请通过 OpenAI CLI 上的命令行标志传递它们，例如：

```
openai api fine_tunes.create \
  -t file-JD89ePi5KMsB3Tayeli5ovfW \
  -m ada \
  --n_epochs 1
```

## 从微调模型继续微调

如果您已经为您的任务微调了一个模型，并且现在有您想要合并的额外训练数据，您可以从模型继续微调。这将创建一个从所有训练数据中学习的模型，而无需从头开始重新训练。

为此，请在创建新的微调作业时传入微调后的模型名称（例如-m curie:ft-<org>-<date>）。其他训练参数不必更改，但是如果您的新训练数据比以前的训练数据小得多，您可能会发现减少learning_rate_multiplier2 到 4 倍很有用。

## 权重和偏差

您可以将微调与权重和偏差同步以跟踪实验、模型和数据集。

要开始使用，您需要一个Weights & Biases帐户和一个付费的 OpenAI 计划。为确保您使用的是最新版本的openaiand wandb，请运行：

```
pip install --upgrade openai wandb
```

要将微调与权重和偏差同步，请运行：

您可以阅读[权重和偏差文档](https://wandb.me/openai-docs)以获取有关此集成的更多信息。