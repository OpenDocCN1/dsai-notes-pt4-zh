# BaiChuan2 大模型生态介绍及本地私有化部署方案

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/P0M5dPXzUoNbZqx2Pc6c5Q8an9f](https://kq4b3vgg5b.feishu.cn/docx/P0M5dPXzUoNbZqx2Pc6c5Q8an9f)

# 本地部署开源大模型

## Ch.16 BaiChuan 2 大模型生态介绍及本地私有化部署方案

> **注意**：大模型技术正处于快速发展阶段，因此在实际使用中可能会遇到与教材内容不一致的情况。如遇到此类情况，请根据实际情况灵活调整，或及时与我们联系。我们在接到相关反馈后，将迅速更新教材内容以确保信息的准确性和实用性。

> 本期课件更新时间：2024年2月28日

百川智能公司，是由搜狗创始人王小川在2023年4月10日创建。在成立后不到100天的时间里，该公司分别于2023年6月15日和2023年7月11日发布了Baichuan-7B和Baichuan-13B两个参数量的预训练大模型。其中，**Baichuan 7B模型是我国国内首个开源且可商用的大模型，在多项国内外数据集评测中同级别参数量下均获得第一名，尤其在中文榜单上大幅领先，并在英文榜单上首度超越LlaMA。**这样的性能效果使得百川智能在很长一段时间内被认为是在国内开源大模型领域类OpenAI一样的存在，将引领国内开源模型的生态和快速发展。

+   Baichuan-7B中英文榜评测

Baichuan-7B在发布之初，相关评测的数据集及对应的评测指标如下：（官方说明地址：https://github.com/baichuan-inc/Baichuan-7B）

+   AGIEval：用来评估模型的认知和解决问题相关的任务中的一般能力，在实际评测中，只保留了其中的四选一单项选择题，随机划分后对所有模型进行了统一 5-shot 测试；

+   C-Eval：该数据集是一个全面的中文基础模型评测数据集，涵盖了 52 个学科和四个难度的级别。在实际评测中，使用该数据集的 dev 集作为 few-shot 的来源，在 test 集上进行了 5-shot 测试；

+   Gaokao：一个以中国高考题作为评测大语言模型能力的数据集，用以评估模型的语言能力和逻辑推理能力。 在实际评测中，只保留了其中的单项选择题，随机划分后对所有模型进行统一 5-shot 测试；

+   MMLU：包含 57 个多选任务的英文评测数据集，涵盖了初等数学、美国历史、计算机科学、法律等，难度覆盖高中水平到专家水平，是目前主流的LLM评测数据集。在实际评测中，采用了开源的评测方案；

Baichuan-7B模型基于标准的Transformer结构，采用了和LLaMA一样的模型设计，在约 1.2 万亿 tokens 上训练得到的 70 亿参数模型，使用自有的中英文双语语料进行训练，在中文上进行优化，从上述评测结果上看，Baichuan-7B模型在标准的中文和英文 benchmark（C-Eval/MMLU）上均取得同尺寸最好的效果。

> Baichuan更多的模型细节，可查看：https://huggingface.co/baichuan-inc/Baichuan-7B （注：需要开启科学上网访问）

在实际评测过程中，均使用5-shot 进行，baichuan-7B与同级别的其他模型评测效果对比如下：

![](img/f0e31ec1eacc29e7370cdec980e07c6d.png)

而其不同于LLaMA完全禁止商业使用，Baichuan-7B使用更宽松的开源协议，允许用于商业，至此也正式拉开了Baichuan模型的迅速迭代，同时也极大程度上推动了国内大模型开源生态的快速发展

接下来，我们就来详细的了解一下截止到2024年2月28日，Baichuan大模型的开源生态情况。

# 1. Baichuan在线大模型生态介绍

百川智能公司通过两个主要策略实现快速迭代和市场扩张：首先，它持续地开源新一代的基础模型及其对应的聊天模型，不断推进技术前沿。其次，通过构建在线API和开发者生态系统，Baichuan模型为开发者提供了丰富的应用开发资源，从而加速其技术的应用与普及。通过这种双轨并行的策略促进Baichuan模型的市场占有率和影响力的提升，而这种策略，也基本符合目前大模型公司的发展决策和态势。

**百川智能官网：https://www.**baichuan-ai.com/home

+   **在线开发者中心**

在这种模式下使用Baichuan模型的方式，就是我们已经非常熟悉的API调用开发形式，即与OpenAI、GLM4一样，按照实际使用的数据量（千tokens）收费：

> Baichuan API资费地址：https://platform.baichuan-ai.com/price

![](img/9baf685b480c4eca6f5d44457728ccfd.png)

一般情况下Baichuan2大模型1个token约等于1.5个中文汉字。新用户注册，可以领取8元的系统赠送金额用于体验Baichuan的API服务。领取及试用步骤如下：

+   **Step 1. 进入百川智能官网：https://www.**baichuan-ai.com/home

进入百川智能官网后，找到进入开发者中心按钮，点击进入。

![](img/0e2f394d238c289df5c70a90fce20056.png)

+   **Step 2. 注册登录**

当进入开发者中心后，先找到左上角登陆按钮，使用手机号进行注册并登录，然后便可以点击[新用户] 点击激发开发者账号，免费领100万Tokens位置，直接领取体验金额。

![](img/e31fbed3af83cd5bdb0f3702a45c446a.png)

+   **Step 3. 确认体验金金额**

接下来进入工作台，可以看到自己账户的一些基本情况，比如账户账单、API Key管理等配置内容。

![](img/aa2dde65589291c319880fce3a55cdfb.png)

+   Step 4. 构建API测试流程

在领取到体验金后，当前的账户就已经获得了一定数量的Tokens免费使用额度。接下来，我们可以尝试通过在线API调用的方式与Baichuan大模型进行交互。首先，需要先创建一个可以使用的API Key，创建方式如下：

![](img/79615d812cd77bd1b4a6f8f9fb6aee64.png)

在弹出的页面中，给当前的API Key取一个名称，点击创建。

![](img/3de0b3944cbe353af67dbbc8e90041e2.png)

**注**意：生成新API Key后，务必在此步骤复制并妥善保存当前的秘钥。一旦关闭页面，将无法再次复制该秘钥。

![](img/f3787896591e782673ef00c907b3d976.png)

+   Step 5. 调用Baichuan模型

通过API调用Baichuan模型的方式与OpenAI、Qwen等其它模型的调用方式相似，主要采用HTTP协议，并通过POST请求实现交互。具体的实现过程如下：

```
import requests
import json
```

> 如果导入报错，请执行 ! pip install requests 命令后，重启当前Jupyter Notebook后再次执行。

```
# 这里写入 Http请求的URL，是固定的
url = &quot;https://api.baichuan-ai.com/v1/chat/completions&quot;

# 这里写入在Step 4步骤中创建的API Key
api_key = &quot;sk-xxxx&quot;
```

构建HTTP请求的Header，Content-Type表示请求的数据格式，目前仅支持 JSON 格式，固定填写"application/json"，Authorization则是请求鉴权的 APIKey，由百川提供。

```
headers = {
    &quot;Content-Type&quot;: &quot;application/json&quot;,
    &quot;Authorization&quot;: &quot;Bearer &quot; + api_key
}
```

构建Messages，这里还是采用通用的Message对话输入格式：

```
messages = [
    {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;你好，请你详细的介绍一下你自己&quot;}
]
```

接下来构建请求体。首先在 model 参数中，选择想要使用的模型，目前Baichuan支持的在线模型如下:

| 模型类型 | 模型名称 | 价格（元/千tokens） |
| --- | --- | --- |
| 通用大模型 | Baichuan2-Turbo | 0.008 |
| 通用大模型 | Baichuan2-Turbo-192k | 0.016 |
| 通用大模型 | Baichuan2-53B | 0.01~0.002 |

这里选择在Baichuan2-Turbo进行测试，同时传入构建好的Messages。

```
data = {
    &quot;model&quot;: &quot;Baichuan2-Turbo&quot;,
    &quot;messages&quot;: messages,
}
```

除此之外，我们在上面提到了，在构建HTTP请求的Header时，目前仅支持 JSON 格式，所以对于输入的Data，我们需要使用json.dumps将 data 转化为 JSON格式。

```
json_data = json.dumps(data)
```

```
json_data
```

```
&#x27;{&quot;model&quot;: &quot;Baichuan2-Turbo&quot;, &quot;messages&quot;: [{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;\\u4f60\\u597d\\uff0c\\u8bf7\\u4f60\\u8be6\\u7ec6\\u7684\\u4ecb\\u7ecd\\u4e00\\u4e0b\\u4f60\\u81ea\\u5df1&quot;}]}&#x27;
```

最后，发送一个POST请求到我们前一步指定的URL，并携带自定义的头部信息和JSON格式的数据，从而实现与Baichuan模型的交互。

```
response = requests.post(
    url, 
    headers=headers,
    data=json_data,)
```

可以通过.text查看到返回的数据信息。

```
response.text
```

```
&#x27;{&quot;id&quot;:&quot;chatcmpl-Mf378012RBUpoGN&quot;,&quot;object&quot;:&quot;chat.completion&quot;,&quot;created&quot;:1709004651,&quot;model&quot;:&quot;Baichuan2-Turbo&quot;,&quot;choices&quot;:[{&quot;index&quot;:0,&quot;message&quot;:{&quot;role&quot;:&quot;assistant&quot;,&quot;content&quot;:&quot;你好！我是百川大模型，是由百川智能的工程师们创造的大语言模型，我可以和人类进行自然交流、解答问题、协助创作，帮助大众轻松、普惠的获得世界知识和专业服务。如果你有任何问题，可以随时向我提问。&quot;},&quot;finish_reason&quot;:&quot;stop&quot;}],&quot;usage&quot;:{&quot;prompt_tokens&quot;:8,&quot;completion_tokens&quot;:52,&quot;total_tokens&quot;:60}}&#x27;
```

如果仅需查看模型的推理结果，可简单的进行如下转化：

```
# 使用json.loads将字符串解析为字典
parsed_response = json.loads(response.text)

# 提取content的内容
content = parsed_response[&quot;choices&quot;][0][&quot;message&quot;][&quot;content&quot;]

content
```

```
&#x27;你好！我是百川大模型，是由百川智能的工程师们创造的大语言模型，我可以和人类进行自然交流、解答问题、协助创作，帮助大众轻松、普惠的获得世界知识和专业服务。如果你有任何问题，可以随时向我提问。&#x27;
```

查看此轮对话花费的Tokens：

```
# 提取usage的内容
usage = parsed_response[&quot;usage&quot;][&quot;total_tokens&quot;]
usage
```

```
60
```

经过上述的分步处理流程，我们已经成功的执行完了一轮的对话流程。接下来尝试编写一个chat_with_baichuan函数封装上述过程。只需要传入Prompt和选择调用的模型即可快速获取到推理结果。

```
import requests
import json

def chat_with_baichuan(user_message, model=&quot;Baichuan2-Turbo&quot;):
    &quot;&quot;&quot;
    使用百川大模型进行聊天。
    
    参数:
    - user_message: 用户发送的消息内容。
    
    返回:
    - content: 百川大模型的回复内容。
    &quot;&quot;&quot;
    # Http请求的URL
    url = &quot;https://api.baichuan-ai.com/v1/chat/completions&quot;

    # API Key
    api_key = &quot;sk-xxx&quot;

    # 设置请求头
    headers = {
        &quot;Content-Type&quot;: &quot;application/json&quot;,
        &quot;Authorization&quot;: &quot;Bearer &quot; + api_key
    }

    # 构造消息
    messages = [
        {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: user_message}
    ]

    # 构造请求数据
    data = {
        &quot;model&quot;: model,
        &quot;messages&quot;: messages,
    }

    # 将数据转换为JSON格式
    json_data = json.dumps(data)

    # 发送POST请求
    response = requests.post(url, headers=headers, data=json_data)

    # 解析响应数据
    parsed_response = json.loads(response.text)

    # 提取并返回回复内容
    content = parsed_response[&quot;choices&quot;][0][&quot;message&quot;][&quot;content&quot;]
    
    return content
```

完成函数封装后，尝试输入一条Prompt进行功能测试。

```
# 注意：此代码仅为示例，实际使用时需要替换api_key
chat_response = chat_with_baichuan(&quot;你好，请你详细的介绍一下你自己&quot;)
print(chat_response)
```

```
你好！我是百川大模型，是由百川智能的工程师们创造的大语言模型，我可以和人类进行自然交流、解答问题、协助创作，帮助大众轻松、普惠的获得世界知识和专业服务。如果你有任何问题，可以随时向我提问。
```

```
# 注意：此代码仅为示例，实际使用时需要替换api_key
chat_response = chat_with_baichuan(&quot;请问下，什么是机器学习？&quot;)
print(chat_response)
```

```
机器学习（Machine Learning）是一种人工智能（AI）技术，它使计算机能够在不进行明确编程的情况下从数据中学习。通过分析大量的数据，机器学习算法可以自动识别模式、建立预测模型并做出决策。这种技术在许多领域都有广泛的应用，如图像识别、语音识别、自然语言处理、推荐系统等。
```

除次之外，还可以通过model参数指定不同的Baichuan模型。

```
# 注意：此代码仅为示例，实际使用时需要替换api_key
chat_response = chat_with_baichuan(model=&#x27;Baichuan2-53B&#x27;, user_message=&quot;请问下，什么是机器学习？&quot;)
print(chat_response)
```

```
机器学习（Machine Learning）是一种人工智能（AI）技术，它使计算机能够在不进行明确编程的情况下从数据中学习。通过分析大量的数据，机器学习算法可以自动识别模式、建立预测模型并做出决策。这种技术在许多领域都有广泛的应用，如图像识别、自然语言处理、推荐系统等。机器学习可以分为监督学习、无监督学习和强化学习等类型。
```

以上就是在线调用Baichuan模型的开发流程和开发者文档的介绍，更多的调用示例和模型参数说明，大家可以根据自己想了解的方向，进入开发者中心进一步的了解，地址：https://platform.baichuan-ai.com/scene

![](img/d10d99ea9f5a02d8a43f1404ad7e0c61.png)

+   **在**线体验中心：https://www.baichuan-ai.com/chat?from=%2Fhome

目前Baichuan最新的版本为Baichuan3，是一个参数量超千亿的模型，根据官方的说明是：在众多权威通用及医疗评测中的中文表现超越GPT-4。大幅提升语义理解和生成能力。但目前未开源，且没有提供在线调用API的接口。目前可以通过如下链接进入体验。

![](img/108e580628306a34e1c5083cb95c17f2.png)

而关于Baichuan3的未来方向，究竟是继续沿袭开源路线，以利于开发者社区，还是转向仅提供在线API服务，如GLM4所做，这一决策将在不久后明朗化。

综上，在了解了百川智能公司的在线大模型生态后，我们将重点转向其开源生态，逐步的实现Baichuan最新一代开源的模型Baichuan2的本地私有化部署及使用。

---

📍更**多大模型技术内容学习**

**扫码添加助理****英英**，回复“大模型”，了解更多大模型技术详情哦👇

![](img/2ac6b9b86b9f7c517a95f253462fe900.png)

此外，**扫码回复“入群”**，即可加入**大模型技术社群****：海量硬核独家技术****干货内容****+无门槛****技术交流****！**