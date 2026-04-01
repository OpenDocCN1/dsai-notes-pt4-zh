# Task4：上手体验大模型解决科学问题

> 来源：[https://datawhaler.feishu.cn/docx/Qne5dfaAXo7n1ix6OGQcXER1nwg](https://datawhaler.feishu.cn/docx/Qne5dfaAXo7n1ix6OGQcXER1nwg)

> **教程贡献者说：**

> 本次Task4我们将使用大模型来尝试解决本赛题。需要说明的是，大模型非常擅长有限空间内的分类问题，并已经在多个数据集上超越了传统深度学习模型。然而，大模型并不擅长处理本赛题的回归问题，即无穷连续值预测。目前一些研究中把产率预测回归问题转变为“高产率”和“低产率”分类问题，大大提高了大模型的预测精度。

> 针对大模型的应用主要有两个方面：Prompt工程和微调。本次夏令营我们先来尝试使用一下Prompt工程，利用大模型上下文学习的能力来预测输出。

# 必知概念入门

> **本小节目标：**

> 知道大模型+化学有哪些综述性研究论文可以参考

> 知道大模型如何在化学领域应用，各种任务下的效果如何

> 知道大模型的局限性，擅长做什么，不擅长做什么

> 知道提示词工程是什么

## 大模型+化学产率预测方向可以参考的论文

[1] ChemLLM: A Chemical Large Language Model https://arxiv.org/pdf/2402.06852

[2] What can Large Language Models do in chemistry? A comprehensive benchmark on eight tasks https://arxiv.org/pdf/2305.18365【简直是豪华阵容】

![](img/9f6863da98ae7151db23adbd61e3fe28.png)

## 化学为什么需要大模型？大模型的应用会遇到哪些问题？[2]

大模型可以简单理解为一个已经预训练好的深度神经网络，我们通过提示词即可引导其结合之前已经学会的知识输出结果。大型预训练语言模型的最新发展已经展示了跨不同任务的令人印象深刻的少量学习能力，在各种分类场景下甚至超越了传统深度学习的SOTA方案。下图(b)说明了化学领域需要特定的大模型。

> 试想一下，为了一个预测产率的任务就用这么多数据费时这么久从头训练一个新模型，简直太不值得了。我们为什么不尝试利用大模型已经掌握的知识直接解决问题呢？（PS. 的确有部分任务是大模型不擅长的，此时还是得自己训练传统模型）

我们要知道**大模型只是一个语言模型，其训练时用的**数据集就是文本，输入是文本，输出也是文本。然而在很多科学领域，待检测数据并非是序列类型，例如药物分子、社交网络等。为了使用大模型，我们需要使用各种方法将其转换为序列格式才能输入大模型，如下图(a)。本赛题使用的就是药物的SMILES序列，非常方便直接输入给LLM。

![](img/3bb3587405f89cdb6929a9aefb03fe34.png)

**大模**型在不同化**学问题**上都有应用，如下图所示 [1]，我们针对不同化学任务将输入数据转换为不同的序列格式，构造提示词并输入大模型中，然后从大模型的输出中处获得结果。

> 提示工程（Prompt Engineering）是一门较新的学科，关注提示词开发和优化，帮助用户将大语言模型（Large Language Model, LLM）用于各场景和研究领域。 掌握了提示工程相关技能将有助于用户更好地了解大型语言模型的能力和局限性。

> 研究人员可利用提示工程来提升大语言模型处理复杂任务场景的能力，如问答和算术推理能力。开发人员可通过提示工程设计、研发强大的工程技术，实现和大语言模型或其他生态工具的高效接轨。

> 提示工程不仅仅是关于设计和研发提示词。它包含了与大语言模型交互和研发的各种技能和技术。提示工程在实现和大语言模型交互、对接，以及理解大语言模型能力方面都起着重要作用。用户可以通过提示工程来提高大语言模型的安全性，也可以赋能大语言模型，比如借助专业领域知识和外部工具来增强大语言模型能力。

![](img/3ab7aa4fa579fa717d3bff72e593ee05.png)

**

**需要说明的是，目前化学反应预测的SOTA方法是UAGNN，感兴趣的同学可以自行了解并实现 [1]。本教程只关注大模型的实现 。

![](img/6f5a93eb33b961be7ee31e38f0aaf4e3.png)

## 大模型擅长什么？不擅长什么？

需要注意的是，LLM不擅长回归任务，更擅长分类任务，目前的论文中使用大模型预测产率把问题转换为了预测高产率（大于70%）和低产率的二分类问题 [1]。

![](img/17d68d3d3f87e6f349f8043a67e0df19.png)

下面是论文中产率预测（二分类简化版）使用的Prompt，大模型预测是Yes和No的二分类结果 [1]。

![](img/b987fd46a434b27bdeaa375c28b69a65.png)

**我们可以看到不同大模型在不同化学任务**上的表现 [2]，有趣的是Yield Prediction产率预测任务是大模型表现最差的几个任务之一了。此外，我们也能看出来在化学数据集上微调过的ChemLLM的表现更好。

![](img/27d62f88d8dc48d24c9b1a7c8b5259c3.png)

# 引导思考

本次任务我们使用两种方式实践大模型：

1.  手动下载Qwen-7b-instruct模型（大概需要下载15-16G）

1.  调用大模型的API

当然，感兴趣且有能力的同学可以试试本地部署ChemLLM，这是一个在化学数据集上微调后的大模型：https://huggingface.co/AI4Chem/ChemLLM-20B-Chat-DPO。

**我们面临的主要问题就是：**

1.  如何调用大模型API？

1.  如何下载大模型？

1.  如何构造Prompt？

希望通过本任务的学习，大家能学会基础的大模型使用方法。

# 正文内容

## 如何使用大模型API

目**前有很多国产**大模型都提供了API接口，本次我们可以使用阿里的Qwen模型，其API申请教程可以参考（注意：Qwen的免费额度仅有100w）：

[从零入门 AI 逻辑推理](https://datawhaler.feishu.cn/wiki/LSaUwltKAiZxuXk12gBcABCmnfd)

这个教程的baseline使用了Qwen-7b-instruct API，关键代码如下：

```
from http import HTTPStatus
import dashscope

MODEL_NAME = &#x27;qwen2-7b-instruct&#x27;
dashscope.api_key=&quot;sk-xxxxx&quot;

def get_completions(query, MODEL_NAME=&#x27;qwen2-7b-instruct&#x27;):
    max_retries = 5
    retry_delay = 60  # in seconds
    attempts = 0
    while attempts &lt; max_retries:
        try:
            return call_qwen_api(MODEL_NAME, query)
        except Exception as e:
            attempts += 1   
            if attempts &lt; max_retries:
                logger.warning(f&quot;Attempt {attempts} failed for text: {query}. Retrying in {retry_delay} seconds...&quot;)
                time.sleep(retry_delay)
            else:
                logger.error(f&quot;All {max_retries} attempts failed for text: {query}. Error: {e}&quot;)
                raise
                
   def call_qwen_api(MODEL_NAME, query):
    # 这里采用dashscope的api调用模型推理，通过http传输的json封装返回结果
    messages = [
        {&#x27;role&#x27;: &#x27;user&#x27;, &#x27;content&#x27;: query}]
    response = dashscope.Generation.call(
        MODEL_NAME,
        messages=messages,
        result_format=&#x27;message&#x27;,  # set the result is message format.
    )
    if response.status_code == HTTPStatus.OK:
        # print(response)
        return response[&#x27;output&#x27;][&#x27;choices&#x27;][0][&#x27;message&#x27;][&#x27;content&#x27;]
    else:
        print(&#x27;Request id: %s, Status code: %s, error code: %s, error message: %s&#x27; % (
            response.request_id, response.status_code,
            response.code, response.message
        ))
        raise Exception()
```

具体细节可以参考下面文件，上面的代码是从下面文件中截取的：

**（注意：这是另一个赛题的baseline，仅供参考如何**使用Qwen的API，并非本赛题代码！）

![](img/3e7922a66bf6d338693ee2bca2582395.png)

[baseline01.ipynb](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/QvUcbtQMXoagvaxyHsRcJN9qncb)

此外，我们也可以使用讯飞的星火大模型，其API申请**教程**可以参考（讯飞的免费token额度有1亿）：

[零基础入门大模型技术竞赛](https://datawhaler.feishu.cn/wiki/VIy8ws47ii2N79kOt9zcXnbXnuS)

本次实验测试集较大（上万条数据），因此本教程使用Spark Max API（星火大模型）的一亿免费token。

使用大模型的API是最简单的方式之一，只需要一段代码就能把LLM封装为一个“黑箱函数”：

```
from sparkai.llm.llm import ChatSparkLLM, ChunkPrintHandler
from sparkai.core.messages import ChatMessage

#星火认知大模型Spark3.5 Max的URL值，其他版本大模型URL值请前往文档（https://www.xfyun.cn/doc/spark/Web.html）查看
SPARKAI_URL = &#x27;wss://spark-api.xf-yun.com/v3.5/chat&#x27;
#星火认知大模型调用秘钥信息，请前往讯飞开放平台控制台（https://console.xfyun.cn/services/bm35）查看
SPARKAI_APP_ID=&quot;xxxx&quot;
SPARKAI_API_SECRET=&quot;xxxx&quot;
SPARKAI_API_KEY=&quot;xxxx&quot;
#星火认知大模型Spark3.5 Max的domain值，其他版本大模型domain值请前往文档（https://www.xfyun.cn/doc/spark/Web.html）查看
SPARKAI_DOMAIN = &#x27;generalv3.5&#x27;

def get_completions(text):
    messages = [ChatMessage(
        role=&quot;user&quot;,
        content=text
    )]
    spark = ChatSparkLLM(
        spark_api_url=SPARKAI_URL,
        spark_app_id=SPARKAI_APP_ID,
        spark_api_key=SPARKAI_API_KEY,
        spark_api_secret=SPARKAI_API_SECRET,
        spark_llm_domain=SPARKAI_DOMAIN,
        streaming=False,
    )
    handler = ChunkPrintHandler()
    a = spark.generate([messages], callbacks=[handler])
    return a.generations[0][0].text

# 测试模型配置是否正确
text = &quot;你是谁&quot;
get_completions(text)
```

**不**同品牌的大模型有不同的秘钥和调用方式，具体细节大家可以参考上面提供的两个教程（分别使用Qwen API和星火API）

## 如何下载大模型

我们使用Hugging Face来下载模型，Qwen2-7B-Instruct大概在13-16G。

```
import os
os.environ[&#x27;HF_ENDPOINT&#x27;] = &#x27;https://hf-mirror.com&#x27;

from transformers import AutoModelForCausalLM, AutoTokenizer

# 设置设备
device = &quot;cuda&quot; if torch.cuda.is_available() else &quot;cpu&quot;

# 预训练的开源大模型，如果本地之前已经下载过，可以直接加载而无需重新下载
# model = AutoModelForCausalLM.from_pretrained(
#     &quot;/mnt/data/qwen/Qwen2-7B-Instruct&quot;,
#     torch_dtype=&quot;auto&quot;,
#     device_map=&quot;auto&quot;
# )

# 首次运行，需要先下载模型；
# 下载中断报错不要怕，重新运行即可，下载会自动从中断处开始
model = AutoModelForCausalLM.from_pretrained(
    &quot;Qwen/Qwen2-7B-Instruct&quot;,
    torch_dtype=torch.bfloat16,
    device_map=&quot;auto&quot;
)

tokenizer = AutoTokenizer.from_pretrained(&quot;Qwen/Qwen2-7B-Instruct&quot;)
```

下载完模型后，我们也可以将其封装为方便调用的黑箱：

```
def get_completions(prompt):

    messages = [
        {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: prompt}
    ]

    # print(messages)
    
    text = tokenizer.apply_chat_template(
        messages,
        tokenize=False,
        add_generation_prompt=True
    )
    model_inputs = tokenizer([text], return_tensors=&quot;pt&quot;).to(device)

    generated_ids = model.generate(
        model_inputs.input_ids,
        max_new_tokens=512
    )
    generated_ids = [
        output_ids[len(input_ids):] for input_ids, output_ids in zip(model_inputs.input_ids, generated_ids)
    ]
    
    return tokenizer.batch_decode(generated_ids, skip_special_tokens=True)[0]

# 测试模型配置是否正确
text = &quot;你是谁&quot;
get_completions(text)
```

## 如何构造Prompt

本赛题我们可以尝试使用下面的Prompt，包括角色描述、任务描述、格式要求、示例参考、待预测样本等信息。

> 值**得注意的是，为**了从大模型输出的内容中提取出需要的预测值，我们使用了@{0.7823}这样的特殊符号来标记产率，后面可以使用正则表达式提取出产率。

```
# 创建Prompt模板
chem_prompt = &quot;&quot;&quot;
你精通预测药物合成反应的产率，你的任务是根据给定的反应数据准确预测反应的产率(Yield)。请仔细阅读以下说明并严格遵循:

***** 任务描述：*****
1. 你将获得一个相应物质的SMILES字符串字段组成的催化合成反应数据，包含Reactant1(反应物1)、Reactant2(反应物2)、Product(产物)、Additive(添加剂)、Solvent(溶剂)。
2. 数据为药物合成中常用的碳氮键形成反应。
3. 待预测的Yield是目标字段是归一化为0到1之间的4位浮点数，表示反应的产率。

***** 你的任务：*****
1. 仔细分析提供的示例数据，理解反应与Yield之间的关系。
2. 根据待预测样本的数据，预测其反应产率Yield。
3. 输出预测的Yield值，精确到小数点后四位。

***** 输出格式要求：*****
1. 仅输出预测的Yield值，不要包含任何其他解释或评论。
2. 使用以下格式输出你的预测：@{{预测的Yield值}}
   例如：@{{0.7823}}

***** 注意事项：*****
1. 你必须输出预测的产率预测结果，不能输出不是预测产率的内容。
2. 相似SMILES的反应产率可能也会有很大差异。
2. 你确保你的预测是合理的，介于0.0000到1.0000之间。
2. 不要遗漏小数点后的零，始终保持四位小数的格式。
3. 不要在输出中包含任何额外的空格或换行符。

以下是几个示例数据，供你参考：

***** 示例样本 *****
{examples}

现在，请基于以上示例和说明，预测以下反应的产率：

**** 待预测样本 ****
Reactant1: {test_reactant1}  
Reactant2: {test_reactant2}  
Product: {test_product}  
Additive: {test_additive} 
Solvent: {test_solvent}  
Yield: @{{}}

请仅输出产率预测值，格式如下：
@{{预测的Yield值}}
&quot;&quot;&quot;
```

**此外，我们希望在Pro****mpt中加**入一些和待检测样本相似的样本，让大模型更清楚这个领域的知识，进而输出更好的结果（这就是上下文学习）。

下面我们使用一个函数向Prompt中填入待检测测试样本，以及训练集中的相似样本。这里使用的是python中字符串的format函数，可以快速把外部变量赋值给字符串中"{var}"部分。只要向generate_prompt函数中传入待填入的Prompt模板、待检测样本、待检测样本相似的样本，即可返回最终完整的Prompt。

```
def generate_prompt(prompt_template, test_sample, top_5_samples):
    examples = &quot;\n\n&quot;.join([
        # f&quot;rxnid: {row[&#x27;rxnid&#x27;]}  \n&quot;
        f&quot;Reactant1: {row[&#x27;Reactant1&#x27;]}  \n&quot;
        f&quot;Reactant2: {row[&#x27;Reactant2&#x27;]}  \n&quot;
        f&quot;Product: {row[&#x27;Product&#x27;]}  \n&quot;
        f&quot;Additive: {row[&#x27;Additive&#x27;]}  \n&quot;
        f&quot;Solvent: {row[&#x27;Solvent&#x27;]}  \n&quot;
        f&quot;Yield: {row[&#x27;Yield&#x27;]}&quot;
        for _, row in top_5_samples.iterrows()
    ])
    
    return prompt_template.format(
        examples=examples,
        # test_rxnid=test_sample[&#x27;rxnid&#x27;],
        test_reactant1=test_sample[&#x27;Reactant1&#x27;],
        test_reactant2=test_sample[&#x27;Reactant2&#x27;],
        test_product=test_sample[&#x27;Product&#x27;],
        test_additive=test_sample[&#x27;Additive&#x27;],
        test_solvent=test_sample[&#x27;Solvent&#x27;]
    )
```

下面是一个填充过后完整的Prompt：

```
你精通预测药物合成反应的产率，你的任务是根据给定的反应数据准确预测反应的产率(Yield)。请仔细阅读以下说明并严格遵循:

***** 任务描述：*****
1. 你将获得一个相应物质的SMILES字符串字段组成的催化合成反应数据，包含Reactant1(反应物1)、Reactant2(反应物2)、Product(产物)、Additive(添加剂)、Solvent(溶剂)。
2. 数据为药物合成中常用的碳氮键形成反应。
3. 待预测的Yield是目标字段是归一化为0到1之间的4位浮点数，表示反应的产率。

***** 你的任务：*****
1. 仔细分析提供的示例数据，理解反应与Yield之间的关系。
2. 根据待预测样本的数据，预测其反应产率Yield。
3. 输出预测的Yield值，精确到小数点后四位。

***** 输出格式要求：*****
1. 仅输出预测的Yield值，不要包含任何其他解释或评论。
2. 使用以下格式输出你的预测：@{预测的Yield值}
   例如：@{0.7823}

***** 注意事项：*****
1. 即使你是一个人工智能模型，但是你有能力直接预测反应的产率，请你一定输出预测的产率，不要回避这个问题。
2. 相似SMILES的反应产率可能也会有很大差异。
2. 你确保你的预测是合理的，介于0.0000到1.0000之间。
2. 不要遗漏小数点后的零，始终保持四位小数的格式。
3. 不要在输出中包含任何额外的空格或换行符。

以下是几个示例数据，供你参考：

***** 示例样本 *****
Reactant1: c1cc2cc[nH]c2cn1  
Reactant2: Clc1ccc(I)cc1  
Product: Clc1ccc(-n2ccc3ccncc32)cc1  
Additive: CN(C)CCN.O=P([O-])([O-])[O-].[Cu+2].[I-].[I-].[K+].[K+].[K+]  
Solvent: CN(C)C=O  
Yield: 0.8860

Reactant1: c1cc2cc[nH]c2cn1  
Reactant2: Clc1ccc(I)cc1  
Product: Clc1ccc(-n2ccc3ccncc32)cc1  
Additive: CN(C)[C@@H]1CCCC[C@H]1N(C)C.O=P([O-])([O-])[O-].[Cu+2].[I-].[I-].[K+].[K+].[K+]  
Solvent: Cc1ccccc1  
Yield: 0.7200

Reactant1: c1cc2cc[nH]c2cn1  
Reactant2: O=[N+]([O-])c1ccc(F)nc1  
Product: O=[N+]([O-])c1ccc(-n2ccc3ccncc32)nc1  
Additive: O=C([O-])[O-].[Cs+].[Cs+]  
Solvent: CN(C)C=O  
Yield: 0.6400

现在，请基于以上示例和说明，预测以下反应的产率：

**** 待预测样本 ****
Reactant1: c1cc2cc[nH]c2cn1  
Reactant2: Clc1ccc(I)cc1  
Product: Clc1ccc(-n2ccc3ccncc32)cc1  
Additive: CNCCNC.O=P([O-])([O-])[O-].[Cu+2].[I-].[I-].[K+].[K+].[K+] 
Solvent: CN(C)C=O  
Yield: @{}

请仅输出产率预测值，格式如下：
@{预测的Yield值}
```

相信你此时会有一个疑惑：**如何获得相似样本呢？**

一种简单的思路就是把原始的文本数据转换为特征向量，然后计算两个向量的相似度。

**本教程使用最简单的TF-IDF方法，combined_features里面就存放了**处理后的特征向量，后面我们可以通过余弦相似度计算任何两个样本特征向量的相似程度：

```
logger.info(&quot;开始特征提取...&quot;)

# 创建TF-IDF向量化器
tfidf = TfidfVectorizer()

# 将所有文本特征组合成一个字符串
def combine_features(row):
    return &#x27; &#x27;.join([str(row[&#x27;Reactant1&#x27;]), str(row[&#x27;Reactant2&#x27;]), str(row[&#x27;Product&#x27;]), str(row[&#x27;Additive&#x27;]), str(row[&#x27;Solvent&#x27;])])

train_df[&#x27;combined_features&#x27;] = train_df.apply(combine_features, axis=1)
test_df[&#x27;combined_features&#x27;] = test_df.apply(combine_features, axis=1)
train_tfidf = tfidf.fit_transform(train_df[&#x27;combined_features&#x27;])

logger.info(&quot;特征提取完成&quot;)
```

对于任何一个待测试样本，我们都可以通过下面的方式计算其和所有训练集样本的相似度：

```
# 对测试样本进行TF-IDF转换
test_tfidf = tfidf.transform([test_sample[&#x27;combined_features&#x27;]])

# 计算与训练集的相似度
similarities = cosine_similarity(test_tfidf, train_tfidf).flatten()

# 获取最相似的K个样本的索引
top_K_indices = similarities.argsort()[-k:][::-1]
```

最**后，我们把使用并行计算的方法**调用前面封装好的大模型调用函数，构造Prompt，并从大模型的输出中抽取目标结果。这里我使用了进度打印、时间预测、错误重试等优化手段，感兴趣的同学可以学习一下，下面的方法适用于大部分大模型API调用场景的封装。

```
# 设置日志
import backoff
import logging

logging.basicConfig(level=logging.INFO, format=&#x27;%(asctime)s - %(levelname)s - %(message)s&#x27;)
logger = logging.getLogger(__name__)

# 使用 backoff 装饰器来处理 API 调用的重试
@backoff.on_exception(backoff.expo,
                      Exception,
                      max_tries=5,
                      max_time=300)

# 处理单个样本
def process_single_sample(args):
    test_sample, train_df, train_tfidf, tfidf = args
    try:
        test_tfidf = tfidf.transform([test_sample[&#x27;combined_features&#x27;]])
        similarities = cosine_similarity(test_tfidf, train_tfidf).flatten()
        top_k_indices = similarities.argsort()[-3:][::-1]
        top_k_samples = train_df.iloc[top_k_indices]
        prompt = generate_prompt(chem_prompt, test_sample, top_k_samples)
        # print(prompt)
        prediction = get_completions(prompt)
        # print(prediction)
        yield_value = extract_yield(prediction)
        # print(yield_value)
        if yield_value is None:
            raise ValueError(f&quot;无法从预测结果中提取有效的产率值&quot;)
        return test_sample[&#x27;rxnid&#x27;], yield_value, None
    except Exception as e:
        return test_sample[&#x27;rxnid&#x27;], None, str(e)

def extract_yield(prediction):
    yield_match = re.search(r&#x27;@{(.+?)}&#x27;, prediction)
    if yield_match:
        yield_value = yield_match.group(1)
        try:
            float_yield = float(yield_value)
            if 0 &lt;= float_yield &lt;= 1:
                return f&quot;{float_yield:.4f}&quot;
            else:
                logger.warning(f&quot;提取的产率值 {float_yield} 不在有效范围内&quot;)
        except ValueError:
            logger.warning(f&quot;无法将提取的值 &#x27;{yield_value}&#x27; 转换为浮点数&quot;)
    else:
        logger.warning(f&quot;无法从预测结果中提取产率值。&quot;) # 完整响应：{prediction}
    return None

# 并行处理样本
def process_samples_parallel(test_df, train_df, train_tfidf, tfidf, max_workers=None, batch_size=100):
    results = {}
    error_indices = []
    total_samples = len(test_df)

    logger.info(f&quot;开始并行处理 {total_samples} 个测试样本&quot;)

    # 如果没有指定max_workers，API最大支持2的并行
    if max_workers is None:
        max_workers = 2

    # 将数据分成批次
    batches = [test_df[i:i+batch_size] for i in range(0, total_samples, batch_size)]

    with tqdm(total=total_samples, desc=&quot;处理测试样本&quot;, unit=&quot;sample&quot;) as pbar:
        for batch in batches:
            with concurrent.futures.ThreadPoolExecutor(max_workers=max_workers) as executor:
                futures = [executor.submit(process_single_sample, (row, train_df, train_tfidf, tfidf)) 
                           for _, row in batch.iterrows()]
                
                for future in concurrent.futures.as_completed(futures):
                    rxnid, yield_value, error = future.result()
                    if error:
                        logger.error(f&quot;处理样本 {rxnid} 时出错: {error}&quot;)
                        error_indices.append(rxnid)
                    else:
                        results[rxnid] = yield_value
                    pbar.update(1)
                    
                    # 更新预计完成时间
                    elapsed_time = pbar.format_dict[&#x27;elapsed&#x27;]
                    rate = pbar.format_dict[&#x27;rate&#x27;]
                    if rate and rate &gt; 0:
                        remaining_time = (total_samples - pbar.n) / rate
                        eta = time.strftime(&quot;%H:%M:%S&quot;, time.gmtime(remaining_time))
                        pbar.set_postfix({&#x27;ETA&#x27;: eta}, refresh=True)

    return results, error_indices
```

> 需要注意的是，星火大模型和Qwen2 API的最大线程数量是2，大家可以结合官方文档设置线程数量。

> **经验：**测试集很大的情况下，使用大模型推理也需要花费很多时间。为了实时了解大模型的预测情况，我们往往打印出进度条，或者加上完成时间预测功能。

最后函数返回的results中是正确提取的样本，而err**or_indices中是无法提取的样本。我们**后面需要对error_indices重新处理，然后把不完整的results补全。

为什么需要这么严格的错误捕捉呢？

**因为大模型有时并不能**按照我们的要求输出，例如下面都是LLM预测错误内容：

```
无法从预测结果中提取产率值。完整响应：由于我是一个人工智能模型，我不能直接进行化学实验或预测实际的化学反应产率。我是基于大量的数据和已有的知识来提供帮助和建议的。对于具体的化学反应产率预测，通常需要专业的化学知识和实验数据来进行分析和预测。然而，我可以为您提供一些关于如何进行反应产率预测的一般性指导：
1. 收集数据：首先，您需要收集相关的反应数据，包括反应条件、反应物、产物、催化剂等。这些数据可以来自文献、实验记录或其他可靠的来源。
2. 特征提取：从收集到的数据中提取有用的特征，例如反应物的分子结构、反应条件（如温度、压力、时间）等。这些特征可以帮助建立模型并预测产率。
3. 建立模型：使用机器学习算法（如回归模型）来建立反应产率预测模型。您可以选择适合您数据的模型类型，如线性回归、决策树回归、支持向量机等。
4. 训练和验证：将数据集分为训练集和测试集，用训练集来训练模型，并用测试集来评估模型的性能。确保模型具有良好的泛化能力，即在未见过的数据上也能做出合理的预测。
5. 预测产率：一旦模型训练完成，您可以使用它来预测新的反应样本的产率。输入新样本的特征值，模型会输出一个预测的产率值。


由于我是一个AI，我不能直接进行化学实验或预测化学反应的产率。我的能力是基于已有的数据和信息进行分析和推断。对于你提供的待预测样本，我无法提供准确的预测结果，因为我没有实际的反应数据来支持我的预测。如果你有具体的反应数据和模型，我可以帮助你分析这些数据并给出可能的预测结果。但是，基于你给出的示例数据，我们没有足够的信息来做出可靠的预测。我建议你咨询专业的化学家或使用专门的化学软件来进行这样的预测。


由于我是一个人工智能模型，我不能直接进行化学实验或预测实际的化学反应产率。我是基于大量的数据和已有的知识来提供帮助和建议的。对于你提供的待预测样本，我可以提供一个基于已有数据的一般性分析，但请注意这只是一个估计，并不能保证准确性。
根据已有的数据样本，我们可以观察到以下几点：
1. 反应条件（Reactant1、Reactant2、Additive、Solvent）对产率有一定的影响。
2. 某些特定的反应物组合可能具有较高的产率，而其他组合则较低。
3. 不同的溶剂可能会影响反应的产率。
然而，要准确预测一个特定反应的产率，我们需要更详细的信息，例如反应的具体机理、温度、压力等。此外，机器学习模型的训练也需要大量的数据和特征工程，以建立一个可靠的预测模型。
因此，我无法直接给出待预测样本的产率预测值。如果你有具体的化学反应机理和实验条件，我建议你咨询专业的化学家或使用专门的化学软件来进行更准确的预测。
```

无法从大模型输出中提取预测怎么办？下面给出两种方案：

1.  重新生成

1.  填充默认值

代码中同时使用了下面两种方案，针对前面处理过程中出错的error_indices，我们重新让大模型预测几次，如果还出错，我们就直接用默认值填充了。

```
# 处理出错的样本
if error_indices:
    logger.info(f&quot;有 {len(error_indices)} 个样本处理出错，正在重新处理...&quot;)
    error_df = test_df[test_df[&#x27;rxnid&#x27;].isin(error_indices)]
    retry_results, retry_error_indices = process_samples_parallel(error_df, train_df, train_tfidf, tfidf)
    results.update(retry_results)

    if retry_error_indices:
        logger.warning(f&quot;仍有 {len(retry_error_indices)} 个样本处理失败&quot;)
        # retry_error_idx = [int(item[4:]) - 1 for item in retry_error_indices]
        for rxnid in retry_error_indices:
            results[rxnid] = default_yield[int(rxnid[4:]) - 1]  # 对于始终无法处理的样本，设置一个默认值
```

如何获得default_yield默认值？

本教程使用的方式是：对于一个待检测样本，其默认填充值为与其相似度最大的三个测试集样本的加权产率均值

具体的实现如下：

```
# 利用相似样本的均值填补大模型无法预测的空缺值

default_yield = []

for index, test_sample in tqdm(test_df.iterrows(), total=len(test_df), desc=&quot;处理测试样本&quot;):
        # 对测试样本进行TF-IDF转换
        test_tfidf = tfidf.transform([test_sample[&#x27;combined_features&#x27;]])
        
        # 计算与训练集的相似度
        similarities = cosine_similarity(test_tfidf, train_tfidf).flatten()
        
        # 获取最相似的k个样本的索引
        top_k_indices = similarities.argsort()[-3:][::-1]
    
        # 获取最相似的k个样本的产率
        top_k_yields = train_df.iloc[top_k_indices][&#x27;Yield&#x27;].astype(float).values
        
        # 获取相似度权重
        weights = similarities[top_k_indices]
        # 对权重进行归一化
        weights = weights / weights.sum()
        
        # 计算这k个samples的产率加权平均
        weighted_yield = np.dot(top_k_yields, weights) 
        
        default_yield.append(weighted_yield)
```

**最后，基于大模型API预测产率的完整代**码如下（简单快捷，但是需要申请API）：

**此方案可以在CPU环境中尝试**

![](img/535d95749eaf70e2448740a663acf06b.png)

[chemLLM_SparkAPI.ipynb](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/VNPxbPI2toyTKExqmMNcgR99nVc)

基于手动下载大模型预测差率的完整代码如下（耗时较久，但是免费）：

此方案建议大家魔搭中下载，并在GPU环境进行推理：https://www.modelscope.cn/my/mynotebook/preset

（不过需要注意的是，魔搭每次关闭都会删除下载大模型，每次都需要重新下载比较费事）

![](img/c1c96f7f35586505796eecb36425e6c3.png)

[ChemLLM_Qwen2-7B-Instruct.ipynb](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/DZEdbPxSSoQh9QxOctDcmHhnnsg)

## 如何微调

微调不是本教程的重点，大家可以参考第一期夏令营中关于微调的讲解：

[Task3：进阶 baseline2【微调方向】 + 知识点讲解](https://datawhaler.feishu.cn/wiki/Q48xwICyHiV0O2kSwjccuTE1nrb)

在本小节，我们可以使用下面的方式来生成微调数据，微调的输入就是填充后的Prompt，微调的输出就是带预测产率：

```

import heapq
import json
# 准备输出文件
output_file = &#x27;finetune_dataset.jsonl&#x27;
total_samples = len(train_df)
similarity_threshold = 0.7
null_count = 0
    
print(f&quot;开始生成微调数据集，共 {total_samples} 个样本...&quot;)
with open(output_file, &#x27;w&#x27;, encoding=&#x27;utf-8&#x27;) as f:
    for index, test_sample in tqdm(train_df.iterrows(), total=total_samples, desc=&quot;处理样本&quot;):
        # 对测试样本进行TF-IDF转换
        test_tfidf = tfidf.transform([test_sample[&#x27;combined_features&#x27;]])
        
        # 计算与训练集的相似度
        similarities = cosine_similarity(test_tfidf, train_tfidf).flatten()
        
        # 找到topk的相似度（使用堆优化此过程）
        top_k = 3  # 可以根据需要调整
        top_indices = heapq.nlargest(top_k, range(len(similarities)), key=similarities.__getitem__)
        top_similarities = similarities[top_indices]

        # 过滤，把上面的相似度中低于similarity_threshold的去除
        filtered_indices = [idx for idx, sim in zip(top_indices, top_similarities) 
                            if sim &gt;= similarity_threshold and sim &lt; 1.0]  # 排除自身

        # 如果去除后的结果为空，此时只向Prompt中加入相似度最大的样本
        if not filtered_indices:
            selected_indices = [top_indices[1]]  # 选择第二相似的样本（第一个是自身）
            null_count += 1
            
        # 获取选中的样本
        top_samples = train_df.iloc[selected_indices]
        
        # 生成Prompt
        prompt = generate_prompt(ft_prompt, test_sample, top_samples)
        
        # print(prompt)
        # 构造一个样本并写入文件
        sample = {
            &quot;input&quot;: prompt,
            &quot;target&quot;: f&quot;@{{{test_sample[&#x27;Yield&#x27;]}}}&quot;
        }
        json.dump(sample, f, ensure_ascii=False)
        f.write(&#x27;\n&#x27;)

print(f&quot;微调数据集已生成并保存到 {output_file}&quot;)
```

获得微调数据集后，大家就可以按照上面教程的方案在线微调，或者微调下载的Qwen模型，欢迎感兴趣的小伙伴尝试使用微调的方法。

# 来个小总结

如果仔细学习上面两个版本（调用API和手动下载大模型）的代码，相信你已经掌握了应用大模型的关键技术。

在此过程中我们也会遇到一些困难，例如大模型串行输出慢、大模型不按照要求输出等。此时我们可以使用多线程加速数据处理，并引入严格的错误捕捉和重试机制，如果大模型还是无法正确预测某些样本，我们可以使用默认值填充，或者选用其他深度学习模型来预测。

最后，下面提供一写关于Prompt的优化思路，欢迎大家思考和尝试：

## 基于Prompt的方案的优化思路

1.  大模型不擅长回归问题，因此我们可以把回归预测转换为离散的分类预测。把[0,1]的连续值拆分为[0.000,0.001,0.002,0.003.....,0.999, 1.000]，虽然最终结果可能会有偏差，但是或许会有更好的预测结果。

1.  调整Prompt中样例的顺序，相似度大的样例放到后面，因为大模型更关注后面的内容。此外，Prompt不同部分的顺序也会影响大模型的输出，可以多试几次看看效果。

# ❀ 恭喜看完全部知识教程！

<https://datawhaler.feishu.cn/sync/TdhvdKG8psUuznbIsOzcJsSon56>

## 你可以随时回顾学习内容~

<https://datawhaler.feishu.cn/sync/XGMQd4cS3s7L0mbC5lmcHttvnRb>

### 🎉合影留念墙

你居然连选学的Task4都学了！！！快来给自己点个赞！

期待下期夏令营还能与你再次相遇~

一起合个影 :)

> 本教程主要由   写作贡献，

> 融合了多位助教的内测体验和宝贵建议。

> 如果你觉得对你有价值和启发，请在下面点赞和评论哟~

>

### 另外别忘了写笔记打卡呀！！~

### 及时思考、沉淀和产出能让你的学习事半功倍！

>

> **注：本文采用 ****署名—非商业性使用**—相同方式共享 4.0 协议国际版许可协议进行许可。