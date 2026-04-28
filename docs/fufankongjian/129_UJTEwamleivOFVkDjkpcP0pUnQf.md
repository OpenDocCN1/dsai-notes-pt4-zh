# Gemini API快速调用

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/EmllddoGkot1UhxaOF9clnPcnTf](https://kq4b3vgg5b.feishu.cn/docx/EmllddoGkot1UhxaOF9clnPcnTf)

## 通过API-Key调用Gemini模型

+   API调用流程测试

```
import google.generativeai as genai
from google.generativeai import GenerativeModel
```

```
genai.configure(api_key=&#x27;AIzaSyDX4AaXpLtsCFMmQSIdmnweCVp8Npbulf4&#x27;)
```

```
model = GenerativeModel(&#x27;gemini-pro&#x27;)
response = model.generate_content(&#x27;The opposite of hot is&#x27;)
print(response.text)
```

```
cold
```

+   多轮对话测试

```
from IPython.display import Markdown
from IPython.display import display
import pathlib
import textwrap

def to_markdown(text):
    text = text.replace(&#x27;•&#x27;, &#x27;  *&#x27;)
    return Markdown(textwrap.indent(text, &#x27;&gt; &#x27;, predicate=lambda _: True))
```

```
chat = model.start_chat()
```

```
to_markdown(chat.send_message(&quot;请问什么是机器学习？&quot;).text)
```

> 机器学习（Machine Learning，简称ML）是人工智能（Artificial Intelligence，简称AI）的一个子领域，它使计算机能够在没有被明确编程的情况下，通过经验学习和适应来执行任务。机器学习算法可以通过分析数据来创建模型，然后使用该模型来做出预测或决定。

> 机器学习有两种主要类型：

+   **监督学习（Supervised Learning）：**在监督学习中，计算机使用标记的数据（即包含正确答案的数据）来训练模型。然后，模型可以使用新数据来做出预测。

+   **无监督学习（Unsupervised Learning）：**在无监督学习中，计算机使用未标记的数据来训练模型。模型必须自己找出数据中的模式，然后使用这些模式来做出预测。

> 机器学习被用于许多领域，包括：

+   **图像识别（Image Recognition）：**计算机学习识别图像中的物体。

+   **自然语言处理（Natural Language Processing）：**计算机学习理解和生成人类语言。

+   **语音识别（Speech Recognition）：**计算机学习理解人类的语音。

+   **推荐系统（Recommendation Systems）：**计算机学习向用户推荐产品或服务。

+   **欺诈检测（Fraud Detection）：**计算机学习识别欺诈性交易。

+   **医学诊断（Medical Diagnosis）：**计算机学习识别疾病，并推荐治疗方案。

> 机器学习是一个快速发展的领域，新的算法和应用不断涌现。机器学习有望在未来几年内继续改变我们的世界。

> 希望这个回答对您有所帮助！如果您还有其他问题，请随时问我。

```
chat.send_message(&quot;我的上一个问题是？&quot;).text
```

```
&#x27;您的上一个问题是：“机器学习是什么？”\n\n您问这个问题是为了了解机器学习的基本概念和应用。我给出了一个全面的回答，解释了机器学习的定义、类型、算法和应用领域。希望我的回答对您有所帮助。\n\n如果您还有其他问题，请随时问我。&#x27;
```

+   多模态功能测试

```
import PIL.Image
image = PIL.Image.open(&#x27;image.jpg&#x27;)
```

```
image
```

![](img/bc577dbf430792c5ce5e5a227a9dec26.png)

```
multimodal_model = GenerativeModel(&#x27;gemini-pro-vision&#x27;)
response = multimodal_model.generate_content([&#x27;请帮我介绍下图片上展示的内容&#x27;, image])
print(response.text)
```

```
 图片上展示了一只可爱的小猫在草地上奔跑。小猫全身都是蓬松的毛发，大大的眼睛炯炯有神，尾巴高高的翘起，看起来非常高兴。它在草地上奔跑，享受着自由自在的感觉。
```

## 通过Vertex AI调用Gemini模型

```
# 安装google-cloud-aiplatform
!pip install google-cloud-aiplatform
```

```
# 导入生成式AI库
from vertexai.preview.generative_models import GenerativeModel, Image, Content, Part, Tool, FunctionDeclaration, GenerationConfig, HarmCategory, HarmBlockThreshold
```

```
from google.cloud import aiplatform
from google_auth_oauthlib.flow import InstalledAppFlow
from google.auth.transport.requests import Request
from google.oauth2.credentials import Credentials
import vertexai
#from vertexai.generative_models import GenerativeModel, Part

# 从本地文件中加载凭据
creds = Credentials.from_authorized_user_file(&#x27;token.json&#x27;)
```

```
aiplatform.init(credentials=creds, project=&#x27;rock-terra-407914&#x27;)
```

+   文本对话测试

```
model = GenerativeModel(&quot;gemini-pro&quot;)
```

```
response = model.generate_content(&#x27;The opposite of hot is&#x27;)
print(response.text)
```

```
Cold
```

+   多轮对话功能测试

```
from IPython.display import Markdown
from IPython.display import display
import pathlib
import textwrap

def to_markdown(text):
    text = text.replace(&#x27;•&#x27;, &#x27;  *&#x27;)
    return Markdown(textwrap.indent(text, &#x27;&gt; &#x27;, predicate=lambda _: True))
```

```
model = GenerativeModel(&#x27;gemini-pro&#x27;)
chat = model.start_chat()
```

```
to_markdown(chat.send_message(&quot;请问什么是深度学习？&quot;).text)
```

> 深度学习是机器学习的一个子领域，它使用多个隐藏层的神经网络来学习数据表示。深度学习模型可以用于各种任务，包括图像识别、自然语言处理和语音识别。

> 深度学习的工作原理是通过将数据输入到神经网络，然后让神经网络学习数据的表示。神经网络由多个隐藏层组成，隐藏层之间通过权重连接。神经网络学习数据的表示 путем调整权重，以使网络的输出与输入数据相匹配。

> 深度学习模型可以用于各种任务，包括：

+   图像识别：深度学习模型可以用于识别图像中的物体。这对于人脸识别、物体检测和自动驾驶等任务非常有用。

+   自然语言处理：深度学习模型可以用于处理自然语言，例如翻译语言、生成文本和回答问题。

+   语音识别：深度学习模型可以用于识别语音，例如语音转文本和语音控制。

+   医学成像：深度学习模型可以用于分析医学图像，例如检测癌症和诊断疾病。

+   金融欺诈检测：深度学习模型可以用于检测金融欺诈，例如信用卡欺诈和洗钱。

+   药物发现：深度学习模型可以用于发现新药，例如设计新分子和预测药物的副作用。

> 深度学习是一个快速发展的领域，它正在不断取得新的突破。深度学习模型在各种任务上都取得了令人印象深刻的结果，它有望在未来继续改变世界。

```
to_markdown(chat.send_message(&quot;请问我的上一个问题是？&quot;).text)
```

> 您的上一个问题是：“什么是深度学习？”

> 您询问了我关于深度学习的知识，我回答了深度学习的定义、工作原理以及它可以用于哪些任务。

> 我希望我的回答对您有所帮助。如果您还有其他问题，请随时问我。

+   多模态性能测试

```
# 导入图片
image = Image.load_from_file(&quot;image.jpg&quot;)
```

```
image
```

![](img/5fbb9e20f316b9a2e2d8e33755d42f09.png)

```
# 创建模型
vision_model = GenerativeModel(&quot;gemini-pro-vision&quot;)
```

```
vision_model.generate_content([&quot;请帮我介绍下图片上展示的内容&quot;, image]).text
```

```
&#x27; 图片上展示了一只可爱的小猫在草地上奔跑。小猫全身毛茸茸的，眼睛是蓝色的，非常可爱。它正在追逐一只蝴蝶。小猫跑得很开心，它的心情看起来很好。&#x27;
```

📍更**多大模型技术内容学习**

**扫码添加助理****英英**，回复“大模型”，了解更多大模型技术详情哦👇

![](img/5981802f7c5641fb13aeddf92980eed4.png)

此外，**扫码回复“入群”**，即可加入**大模型技术社群****：海量硬核独家技术****干货内容****+无门槛****技术交流****！**