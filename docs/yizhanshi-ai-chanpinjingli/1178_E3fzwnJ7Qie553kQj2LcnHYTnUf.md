# Agent智能客服对话系统|工作流搭建

> 来源：[https://v11enp9ok1h.feishu.cn/docx/KUtXdy58io4wuixnYWScyRRmnBR](https://v11enp9ok1h.feishu.cn/docx/KUtXdy58io4wuixnYWScyRRmnBR)

## 背景信息

### 问题背景

1.  在企业服务用户的过程中，每天都会出现大量相关领域内的问题。

1.  传统的人工客服作息服务解答效率低，且成本高。

1.  智能客服对话系统可以帮助企业实现在线服务的智能化人机协作系统，提高坐席服务的效率、降低人力开销及新人业务学习成本。

### 解决方案

+   机器学习平台PAI在智能客服领域，提供了端到端的纯白盒解决方案。客户只需要准备好自己相关领域的FAQ（常见问题）和知识图谱数据，就可以利用PAI自定义的搭建从算法构建到模型部署的人工智能流程，形成完整的端到端解决方案，实现对应领域智能客服的业务系统。

+   本文以“汽车售前咨询”这一业务领域为例，展示如何快速搭建智能客服机器人，从而实现机器人自动回答用户关于汽车售前业务的相关问题。这样一方面节省客户的咨询等待和信息检索时间，另一方面节省企业运营人力成本，使精力可以集中在筛选出的高价值客户上。

### 方案优势

+   **纯白盒**：

可根据您自己具体的业务场景，自定义构建智能客服业务系统。

+   **端到端：  **

从最初的数据准备到最后的模型部署推理，提供全链路的系统构建流程。

+   **有据可依：**

对话系统内所有的回答都是有依据的，避免纯深度学习方案的不可解释性。

+   **鲁棒可控：**

系统任何地方出了问题，都有相应的异常处理与bad case分析应对机制。

### 工程部署架构

> 用户将**知识图谱数据**，模型和特征文件以及**FAQ数据**存储在**OSS****上，然后将智能**客服对话系统**服务部署在**PAI-EAS上就可以使用FAQ检索和知识图谱查询提供的功能来做智能客服问答。

> **详见第五章**：智能客服在线服务配置。

---

### 对话系统内部状态

> 系**统初始化后**进**入闲聊状态****，对用户输**入“**您好”等话术**进行响应。响应完成后系统进入问题接收状态，此刻等待用户提问。接入通过实体归一化/属性归一化来对用户提问中的实体和属性进行明确，接着进入问答状态进行FAQ检索或者知识图谱查询，当出现异常状况时，进入异常处理模式，在异常处理中引导用户进入正确的查询。

---

## 基于FAQ常见问题检索的问答

**基于FAQ（常见问题**）的问答系统总体结构如下图所示，系统的关键模块包括：

+   频繁问答对数据集，即**FAQ问答库**。

+   **预处理模块**：负责对query进行分词等操作。

+   **检索模块**：负责从FAQ中检索可能与用户查询相似的若干标准问句。

+   **相似问句选择模块**：负责从候选相似问句中，选择与用户查询最相似的标准问句。

**具体而言，**检索模块会在对用户提出**的query**进行预**处**理的**基础上，从****F**AQ数据集中检索K个与q**uery最**相似的问句。然后相似问句选择模块会从K个候选相似问句中，选出与query相似度最高的，并判断是否具有足够的置信度。

**如果相似度**最高的候选相似问句的置信度达标，“相似问句选择模块”就会判定该问句是用户query的同义问句，从而返回提前预置好的答案。

---

###  FAQ频繁问答对数据集准备

**FAQ**，即Frequently Asked Quest**ions、是业务**场景中用户最常问的问题，又称“标准问题”。

+   **对这类问题，我****们可以提前****编制答案，构**成问答对。问答对通常简称“QA**对”(Que**s**tion Answer Pair)。问答系统的知识库中存储**的FAQ数据集，实际上是标准问题其答案构成的QA对数据集。如果用户提出的问题和FAQ数据集中的某个标准问题是相似问题，那么用户问题的答案就是该标准问题的答案。

**本文以****“汽车售前”的智能客服场景为例**，展示FAQ所需的数据，文件列表如下所示：

```
car_faqs ├── query_label_idx_map.txt        # 标注问题ID编号文件   
 ├── questions                      # 标准问题同义配置文件 
 │  ├── 保养灯的使用说明 
 │  ├── 保养一次多少钱          
 │   ├── 变速箱故障灯亮   
 │   ├── ...... 
 ├── answers                        # 标准答案配置文件 
 │   ├── 特斯拉_model3_2021款_标准续航升级版       
 │   ├── 特斯拉_modely_2021款_长续航版  
 │   ├── ......
 
```

#### 步骤一：给标注问题编号

为提升检索模块的召回率，一般会为每一个标准问句配置若干同义问句。

首先对**FAQ**频繁问答库中的标准问题进行**ID**化编号，比如针对上面**FAQ**中的标准问题，ID化后，问题和数字之间用tab分隔。

下面为示例文件【query_label_idx_map.txt】的部分数据样例。

```
有几个排气管  0 
是双排气管吗  1 
问某款车是什么悬挂 2 
有换挡拨片吗 3 
问发动机产地 4 
变速箱是哪里产的 5 
问上市时间 6 
问某款车上市了吗 7 
可以加装换挡拨片吗 8 
一保多少钱 9
```

---

#### 步骤二：配置同义问题

准备好上面的标注问题ID编号文件后，现在需要为每个标准问题配置若干个同义问题。

以“保养灯的使用说明”为例，文件的每一行内容对应一个和“保养灯的使用说明”同义的问题。下面为示例文件【保养灯的使用说明】的部分数据样例。

```
新款保养灯怎么设定
保养灯是什么样子的图标
保养灯什么消岀
保养灯归是什么标志?
保养灯长什么样子
保养灯如何设置。因为过保不想去4S店了,有谁知道的说一下
保养灯归零，是怎么，弄得，请问各位兄弟，谢谢！
保养灯和燃油指示灯同时闪烁是怎么回事？
有保养灯的没有?
汽车保养灯归零
```

然后将所有的标准问题同义配置文件放到文件夹questions中。

---

#### 步骤三：配置标准答案

准备好上面的标准问题列表后，下面介绍汽车售前FAQ数据问答数据集的准备。

以“特斯拉_model3_2021款_标准续航升级版”这款车为例，其中[]中表示问题类型，后面展示问题的答案，其中答案和问题类型之间用tab分隔。下面为示例文件【特斯拉_model3_2021款_标准续航升级版】的部分数据样例。

```
[有几个排气管]  0个
[是双排气管吗]  不是
[问某款车是什么悬挂] 前麦弗逊式独立悬架，后多连杆独立悬架
[有换挡拨片吗]  没有
[问发动机产地]  美国
[变速箱是哪里产的]  上海
[问上市时间] 2021年1月
[问某款车上市了吗]  2021年1月已上市
[可以加装换挡拨片吗] 可以加装，但需要加装模块
[一保多少钱] 5000公里做首保，首保免费
```

然后将所有的标准答案配置文件放到文件夹answers中。

---

###  问答特征向量化

利用**PAI**提供的可视化建模**Studio****平台，基于**海量大数据语料预训练获得的NLP迁移学习模型，进行用户query向量表征模型的构建。其中示例实验用到的训练集、评估集、测试集。

1.  **进入**Designer**控制台**

1.  登录[PAI控制台](https://pai.console.aliyun.com/?spm=a2c0j.14094439.J_5834642020.3.32fb426aLl60J6&projectId=&regionId=#/overview)。

1.  在左侧导航栏中，选择**模型开发和训练**->**可视化建模（Designer）****。**

1.  **构建实验**

1.  在可视化建模（Designer）控制**台中，选择实验模板，在****实**验模板列表中找到智能客服问答特征向量化，点击并创建实验。

1.  在实验列表中找到刚才创建好的模板实验，并进入实验。

1.  系统根据预置的模板，自动构建实验，如下图所示。运行过程中，点击组件右键，可查看运行日志。

##

## 基于KBQA的知识图谱问答

**KBQA**的全称是**基于知识库问答**（Knowledge Base Question Answering），即给定自然语言问题，通过对问题进行语义理解和解析，进而基于知识图谱构建的知识库进行查询、推理得出答案。

基于知识图谱的精准问答子系统的关键模块包括：

+   **实体链接：**将用户问句中提到的实体关联到唯一的实体表达。

+   **语义角色标注：**对属性的类型进行自动打标。

+   **意图分类：**判断用户问的是什么类型的问题。

+   **查询链路生成：**生成查询知识库的查询语句

+   **答案生成：**负责按照问句意图的类型，在答案模板里生成答案。

基于KBQA的知识图谱问答的架构图如下所示：

### 知识图谱数据准备

针对**实体**、**属性**名称和**属性值**，智能客户系统采用可读性强的**JSON**来构建数据文件。

属性名可以包括子属性，以“特斯拉model3_2021款_标准续航升级版”为例，车轮**制动属性包括**：前制动器类型，后制动器类型，驻车制动类型这三种子属性。用户需要将业务中的数据转换成JSON格式。

**下面为示例文件**【特斯拉model3_2021款_标准续航升级版.json】的部分数据样例。

```
{        
 &quot;实体&quot;: 
  {          &quot;名称&quot;:
  &quot;特斯拉model3_2021款_标准续航升级版&quot;,          
  &quot;关键词&quot;: &quot;特斯拉|model3|2021款|标准续航升级版&quot; 
    },          
    &quot;属性&quot;:
    {                  
    &quot;厂商&quot;:&quot;特斯拉中国&quot;,                  
    &quot;品牌&quot;:&quot;特斯拉&quot;,                  
    &quot;经销商报价&quot;:&quot;27.6万&quot;,                 
     &quot;级别&quot;:&quot;中型车&quot;,          
     &quot;上市时间&quot;:&quot;2021.07&quot;,          
     &quot;纯电续航里程&quot;:&quot;525km&quot;,         
     &quot;充电时间&quot;:&quot;快充一小时，慢充10小时&quot;,          
     &quot;百公里加速时间&quot;:&quot;5.6s&quot;,          
     &quot;整车保修期限&quot;:&quot;4年或8万公里&quot;       
     },      
     &quot;变速箱&quot;:{                      
     &quot;变速箱档位个数&quot;:&quot;6个&quot;,                  
     &quot;变速箱类型&quot;:&quot;电动车单速变速箱&quot;         },      
     &quot;配置&quot;: {                
     &quot;内部配置&quot;: {                      
     &quot;空调&quot;: {                          
     &quot;空调控制方式&quot;: &quot;自动&quot;,                    
     &quot;温度分区控制&quot;: &quot;有&quot;,                    
     &quot;后座出风口&quot;: &quot;无&quot;                  },                
     &quot;行车电脑显示屏&quot;: &quot;有&quot;,                
     &quot;倒车视频影像&quot;: &quot;有&quot;     } 
            } 
    }
```

# 二、实现过程

现在，让我们挽起袖子，开始动手构建吧！

![](img/80cb8ddba95e2fe151d507f35d2cea72.png)

## 2.1 准备工作

本教程的完整代码可在AI智能体的GitHub仓库中获取。你可以在 “Build an Agent from Scratch” 这里找到具体实现。在运行代码之前，请确保你的系统满足以下先决条件：

#### Python环境设置

运行AI智能体需要安装Python。按照以下步骤来设置你的环境：

+   安装Python（如果尚未安装）：从http://python.org下载并安装Python（推荐3.8及以上版本）。

+   **验证安装**：在命令行中输入python --version，查看是否正确安装。

+   **创建虚拟环境（推荐）**：使用虚拟环境来管理依赖项是个不错的选择。在命令行中输入python -m venv ai_agents_env创建虚拟环境，然后通过source ai_agents_env/bin/activate激活它。

+   **安装所需依赖项**：导航到代码仓库目录，然后在命令行中输入pip install -r requirements.txt来安装所需的依赖库。

#### 本地设置Ollama

Ollama用于高效地运行和管理本地语言模型。按照以下步骤安装和配置它：

+   **下载并安装Ollama**：访问Ollama的官方网站，下载适合你操作系统的安装程序，并按照平台对应的说明进行安装。

+   **验证Ollama安装**：在命令行中运行ollama --version，检查Ollama是否正确安装。

+   **拉取模型（如有需要）**：有些智能体实现可能需要特定的模型。你可以使用ollama pull mistral命令拉取模型。

### AI Agent 选型

第二步，我们需要根据场景选择 AI Agent 开发平台，选择合适的大模型，并选用不同的工具来执行不同系统的操作。

### 选择哪个 AI Agent 平台

先说说 AI Agent 开发平台，Dify、Coze、FastGPT 等这么多无代码 Agent 开发平台，我们该选择哪个比较合适呢？

+   **Coze **只能使用云端，不能在本地部署。

+   **Dify**是完全开源的，没有限制，但在知识问答方面的能力较弱。

+   **FastGPT** 的使用有一定限制，但在知识回答方面的能力相对较强。

> 更高级的开发平台，如 LangGraph、CrewAI 等，可以让 AI 自我规划并执行任务，但是需要编写代码。

> 是否选择这些平台，取决于我们的具体需求，当然也可以混合使用。

> 这就要求我们深入了解每个开发平台的特点，它擅长什么、不擅长什么，存在哪些明显的缺陷。只有掌握了这些信息，才能根据我们的场景做出合适的选型。

### 选择哪个 LLM

[再说说大模型](https://zhida.zhihu.com/search?content_id=253679336&content_type=Article&match_order=1&q=OpenAI&zhida_source=entity)的[选择，现在有](https://zhida.zhihu.com/search?content_id=253679336&content_type=Article&match_order=1&q=Claude&zhida_source=entity)[海外的 Open](https://zhida.zhihu.com/search?content_id=253679336&content_type=Article&match_order=1&q=DeepSeek&zhida_source=entity)AI、Claude、G[emini](https://zhida.zhihu.com/search?content_id=253679336&content_type=Article&match_order=1&q=LLaMA&zhida_source=entity)，[也有国内](https://zhida.zhihu.com/search?content_id=253679336&content_type=Article&match_order=1&q=Grok&zhida_source=entity)的 Kim[i、通义千问和](https://zhida.zhihu.com/search?content_id=253679336&content_type=Article&match_order=1&q=Mistral&zhida_source=entity)近期爆火的 DeepSeek，还有一些开源模型如 LLaMA、Grok，也有像 Mistral 这样的小模型。

![](img/8a9057e88a6c34cd1396b1e4add1b611.png)

### 2.2 实现步骤

#### 步骤1：设置环境

除了Python，我们还需要安装一些必要的库。在本教程中，我们将使用requests、json和termcolor库。另外，我们会使用dotenv来管理环境变量。在命令行中输入pip install requests termcolor python-dotenv进行安装。

#### 步骤2：定义模型类

我们首先需要一个能够处理用户输入的模型。我们将创建一个OllamaModel类，它通过与本地API进行交互来生成响应。以下是基本实现代码：

```
from termcolor import colored
import os
from dotenv import load_dotenv
load_dotenv()
import requests
import json
import operator

class OllamaModel:
    def __init__(self, model, system_prompt, temperature=0, stop=None):
        &quot;&quot;&quot;
        用给定的参数初始化OllamaModel。
        参数:
        model (str): 要使用的模型名称。
        system_prompt (str): 要使用的系统提示。
        temperature (float): 模型的温度设置。
        stop (str): 模型的停止标记。
        &quot;&quot;&quot;
        self.model_endpoint = &quot;http://localhost:11434/api/generate&quot;
        self.temperature = temperature
        self.model = model
        self.system_prompt = system_prompt
        self.headers = {&quot;Content-Type&quot;: &quot;application/json&quot;}
        self.stop = stop

    def generate_text(self, prompt):
        &quot;&quot;&quot;
        根据提供的提示从Ollama模型生成响应。
        参数:
        prompt (str): 用于生成响应的用户查询。
        返回:
        dict: 模型的响应，以字典形式返回。
        &quot;&quot;&quot;
        payload = {
            &quot;model&quot;: self.model,
            &quot;format&quot;: &quot;json&quot;,
            &quot;prompt&quot;: prompt,
            &quot;system&quot;: self.system_prompt,
            &quot;stream&quot;: False,
            &quot;temperature&quot;: self.temperature,
            &quot;stop&quot;: self.stop
        }
        try:
            request_response = requests.post(
                self.model_endpoint,
                headers=self.headers,
                data=json.dumps(payload)
            )
            print(&quot;REQUEST RESPONSE&quot;, request_response)
            request_response_json = request_response.json()
            response = request_response_json[&#x27;response&#x27;]
            response_dict = json.loads(response)
            print(f&quot;\n\nResponse from Ollama model: {response_dict}&quot;)
            return response_dict
        except requests.RequestException as e:
            response = {&quot;error&quot;: f&quot;Error in invoking model! {str(e)}&quot;}
            return response
```

这个类使用模型名称、系统提示、温度和停止标记进行初始化。generate_text方法向模型API发送请求并返回响应。

#### 步骤3：为智能体创建工具

接下来是为我们的智能体创建可用的工具。这些工具是执行特定任务的简单Python函数。以下是一个基本计算器和字符串反转器的示例：

```
def basic_calculator(input_str):
    &quot;&quot;&quot;
    根据输入字符串或字典对两个数字执行数值运算。
    参数:
    input_str (str或dict): 要么是表示包含&#x27;num1&#x27;、&#x27;num2&#x27;和&#x27;operation&#x27;键的字典的JSON字符串，
    要么是直接的字典。例如: &#x27;{&quot;num1&quot;: 5, &quot;num2&quot;: 3, &quot;operation&quot;: &quot;add&quot;}&#x27;
    或{&quot;num1&quot;: 67869, &quot;num2&quot;: 9030393, &quot;operation&quot;: &quot;divide&quot;}
    返回:
    str: 运算的格式化结果。
    抛出:
    Exception: 如果在运算过程中发生错误（例如，除以零）。
    ValueError: 如果请求了不支持的运算或输入无效。
    &quot;&quot;&quot;
    try:
        if isinstance(input_str, dict):
            input_dict = input_str
        else:
            input_str_clean = input_str.replace(&quot;&#x27;&quot;, &quot;\&quot;&quot;)
            input_str_clean = input_str_clean.strip().strip(&quot;\&quot;&quot;)
            input_dict = json.loads(input_str_clean)
        if not all(key in input_dict for key in [&#x27;num1&#x27;, &#x27;num2&#x27;, &#x27;operation&#x27;]):
            return &quot;Error: Input must contain &#x27;num1&#x27;, &#x27;num2&#x27;, and &#x27;operation&#x27;&quot;
        num1 = float(input_dict[&#x27;num1&#x27;])
        num2 = float(input_dict[&#x27;num2&#x27;])
        operation = input_dict[&#x27;operation&#x27;].lower()
    except (json.JSONDecodeError, KeyError) as e:
        return &quot;Invalid input format. Please provide valid numbers and operation.&quot;
    except ValueError as e:
        return &quot;Error: Please provide valid numerical values.&quot;
    operations = {
        &#x27;add&#x27;: operator.add,
        &#x27;plus&#x27;: operator.add,
      &#x27;subtract&#x27;: operator.sub,
      &#x27;minus&#x27;: operator.sub,
      &#x27;multiply&#x27;: operator.mul,
        &#x27;times&#x27;: operator.mul,
        &#x27;divide&#x27;: operator.truediv,
        &#x27;floor_divide&#x27;: operator.floordiv,
      &#x27;modulus&#x27;: operator.mod,
        &#x27;power&#x27;: operator.pow,
        &#x27;lt&#x27;: operator.lt,
        &#x27;le&#x27;: operator.le,
        &#x27;eq&#x27;: operator.eq,
        &#x27;ne&#x27;: operator.ne,
        &#x27;ge&#x27;: operator.ge,
        &#x27;gt&#x27;: operator.gt
    }
    if operation not in operations:
        return f&quot;Unsupported operation: &#x27;{operation}&#x27;. Supported operations are: {&#x27;, &#x27;.join(operations.keys())}&quot;
    try:
        if (operation in [&#x27;divide&#x27;, &#x27;floor_divide&#x27;,&#x27;modulus&#x27;]) and num2 == 0:
            return &quot;Error: Division by zero is not allowed&quot;
        result = operations[operation](num1, num2)
        if isinstance(result, bool):
            result_str = &quot;True&quot; if result else &quot;False&quot;
        elif isinstance(result, float):
            result_str = f&quot;{result:.6f}&quot;.rstrip(&#x27;0&#x27;).rstrip(&#x27;.&#x27;)
        else:
            result_str = str(result)
        return f&quot;The answer is: {result_str}&quot;
    except Exception as e:
        return f&quot;Error during calculation: {str(e)}&quot;


def reverse_string(input_string):
    &quot;&quot;&quot;
    反转给定的字符串。
    参数:
    input_string (str): 要反转的字符串。
    返回:
    str: 反转后的字符串。
    &quot;&quot;&quot;
    if not isinstance(input_string, str):
        return &quot;Error: Input must be a string&quot;
    reversed_string = input_string[::-1]
    result = f&quot;The reversed string is: {reversed_string}&quot;
    return result
```

这些函数根据提供的输入执行特定任务。basic_calculator处理算术运算，而reverse_string则反转给定的字符串。

---

#### 步骤4：构建工具箱

[ToolBox](https://zhida.zhihu.com/search?content_id=254820407&content_type=Article&match_order=1&q=ToolBox&zhida_source=entity)类用于存储智能体可以使用的所有工具，并为每个工具提供描述：

```
class ToolBox:
    def __init__(self):
        self.tools_dict = {}

    def store(self, functions_list):
        &quot;&quot;&quot;
        存储列表中每个函数的名称和文档字符串。
        参数:
        functions_list (list): 要存储的函数对象列表。
        返回:
        dict: 以函数名称为键，其文档字符串为值的字典。
        &quot;&quot;&quot;
        for func in functions_list:
            self.tools_dict[func.__name__] = func.__doc__
        return self.tools_dict

    def tools(self):
        &quot;&quot;&quot;
        将store方法中创建的字典转换为文本字符串返回。
        返回:
        str: 存储的函数及其文档字符串的字典，以文本字符串形式返回。
        &quot;&quot;&quot;
        tools_str = &quot;&quot;
        for name, doc in self.tools_dict.items():
            tools_str += f&quot;{name}: \&quot;{doc}\&quot;\n&quot;
        return tools_str.strip()
```

这个类将帮助智能体了解哪些工具可用以及每个工具的用途。

#### 步骤5：创建智能体类

智能体需要进行思考、决定使用哪个工具并执行它。以下是Agent类的代码：

```
agent_system_prompt_template = &quot;&quot;&quot;
你是一个智能AI助手，可以使用特定的工具。你的回复必须始终采用以下JSON格式：
{
    &quot;tool_choice&quot;: &quot;name_of_the_tool&quot;,
    &quot;tool_input&quot;: &quot;inputs_to_the_tool&quot;
}
工具及使用场景：
1. basic_calculator：用于任何数学计算
    - 输入格式: {{&quot;num1&quot;: number, &quot;num2&quot;: number, &quot;operation&quot;: &quot;add/subtract/multiply/divide&quot;}}
    - 支持的运算: add/plus, subtract/minus, multiply/times, divide
    - 示例输入和输出:
        - 输入: &quot;Calculate 15 plus 7&quot;
        - 输出: {{&quot;tool_choice&quot;: &quot;basic_calculator&quot;, &quot;tool_input&quot;: {{&quot;num1&quot;: 15, &quot;num2&quot;: 7, &quot;operation&quot;: &quot;add&quot;}}}}
        - 输入: &quot;What is 100 divided by 5?&quot;
        - 输出: {{&quot;tool_choice&quot;: &quot;basic_calculator&quot;, &quot;tool_input&quot;: {{&quot;num1&quot;: 100, &quot;num2&quot;: 5, &quot;operation&quot;: &quot;divide&quot;}}}}
2. reverse_string：用于任何涉及反转文本的请求
    - 输入格式: 仅需反转的文本字符串
    - 当用户提到“reverse”、“backwards”或要求反转文本时，始终使用此工具
    - 示例输入和输出:
        - 输入: &quot;Reverse of &#x27;Howwwww&#x27;?&quot;
        - 输出: {{&quot;tool_choice&quot;: &quot;reverse_string&quot;, &quot;tool_input&quot;: &quot;Howwwww&quot;}}
        - 输入: &quot;What is the reverse of Python?&quot;
        - 输出: {{&quot;tool_choice&quot;: &quot;reverse_string&quot;, &quot;tool_input&quot;: &quot;Python&quot;}}
3. no tool：用于一般对话和问题
    - 示例输入和输出:
        - 输入: &quot;Who are you?&quot;
        - 输出: {{&quot;tool_choice&quot;: &quot;no tool&quot;, &quot;tool_input&quot;: &quot;I am an AI assistant that can help you with calculations, reverse text, and answer questions. I can perform mathematical operations and reverse strings. How can I help you today?&quot;}}
        - 输入: &quot;How are you?&quot;
        - 输出: {{&quot;tool_choice&quot;: &quot;no tool&quot;, &quot;tool_input&quot;: &quot;I&#x27;m functioning well, thank you for asking! I&#x27;m here to help you with calculations, text reversal, or answer any questions you might have.&quot;}}
严格规则：
1. 对于关于身份、能力或感受的问题：
    - 始终使用“no tool”
    - 提供完整、友好的回复
    - 提及你的能力
2. 对于任何文本反转请求：
    - 始终使用“reverse_string”
    - 仅提取要反转的文本
    - 去除引号、“reverse of”和其他多余文本
3. 对于任何数学运算：
    - 始终使用“basic_calculator”
    - 提取数字和运算
    - 将文本形式的数字转换为数字
以下是你的工具列表及其描述：
{tool_descriptions}
记住：你的回复必须始终是包含“tool_choice”和“tool_input”字段的有效JSON。
&quot;&quot;&quot;


class Agent:
    def __init__(self, tools, model_service, model_name, stop=None):
        &quot;&quot;&quot;
        用工具列表和模型初始化智能体。
        参数:
        tools (list): 工具函数列表。
        model_service (class): 具有generate_text方法的模型服务类。
        model_name (str): 要使用的模型名称。
        &quot;&quot;&quot;
        self.tools = tools
        self.model_service = model_service
        self.model_name = model_name
        self.stop = stop

    def prepare_tools(self):
        &quot;&quot;&quot;
        将工具存储在工具箱中并返回其描述。
        返回:
        str: 存储在工具箱中的工具的描述。
        &quot;&quot;&quot;
        toolbox = ToolBox()
        toolbox.store(self.tools)
        tool_descriptions = toolbox.tools()
        return tool_descriptions

    def think(self, prompt):
        &quot;&quot;&quot;
        使用系统提示模板和工具描述在模型上运行generate_text方法。
        参数:
        prompt (str): 用于生成响应的用户查询。
        返回:
        dict: 模型的响应，以字典形式返回。
        &quot;&quot;&quot;
        tool_descriptions = self.prepare_tools()
        agent_system_prompt = agent_system_prompt_template.format(tool_descriptions=tool_descriptions)
        if self.model_service == OllamaModel:
            model_instance = self.model_service(
                model=self.model_name,
                system_prompt=agent_system_prompt,
                temperature=0,
                stop=self.stop
            )
        else:
            model_instance = self.model_service(
                model=self.model_name,
                system_prompt=agent_system_prompt,
                temperature=0
            )
        agent_response_dict = model_instance.generate_text(prompt)
        return agent_response_dict

    def work(self, prompt):
        &quot;&quot;&quot;
        解析think方法返回的字典并执行相应的工具。
        参数:
        prompt (str): 用于生成响应的用户查询。
        返回:
        执行相应工具的响应，如果未找到匹配的工具，则返回tool_input。
        &quot;&quot;&quot;
        agent_response_dict = self.think(prompt)
        tool_choice = agent_response_dict.get(&quot;tool_choice&quot;)
        tool_input = agent_response_dict.get(&quot;tool_input&quot;)
        for tool in self.tools:
            if tool.__name__ == tool_choice:
                response = tool(tool_input)
                print(colored(response, &#x27;cyan&#x27;))
                return
        print(colored(tool_input, &#x27;cyan&#x27;))
        return
```

这个类有三个主要方法：

+   prepare_tools：存储并返回工具的描述。

+   think：根据用户提示决定使用哪个工具。

+   work：执行选择的工具并返回结果。

#### 步骤6：运行智能体

最后，让我们把所有内容整合起来，运行我们的智能体。在脚本的主程序部分，初始化智能体并开始接受用户输入：

```
if __name__ == &quot;__main__&quot;:
    &quot;&quot;&quot;
    使用此智能体的说明：
    你可以尝试的示例查询：
    1. 计算器运算:
        - &quot;Calculate 15 plus 7&quot;
        - &quot;What is 100 divided by 5?&quot;
        - &quot;Multiply 23 and 4&quot;
    2. 字符串反转:
        - &quot;Reverse the word &#x27;hello world&#x27;&quot;
        - &quot;Can you reverse &#x27;Python Programming&#x27;?&quot;
    3. 一般问题（将得到直接回复）:
        - &quot;Who are you?&quot;
        - &quot;What can you help me with?&quot;
    Ollama命令（在终端中运行这些命令）:
        - 查看可用模型:    &#x27;ollama list&#x27;
        - 查看正在运行的模型:      &#x27;ps aux | grep ollama&#x27;
        - 列出模型标签:          &#x27;curl http://localhost:11434/api/tags&#x27;
        - 拉取新模型:         &#x27;ollama pull mistral&#x27;
        - 运行模型服务器:         &#x27;ollama serve&#x27;
    &quot;&quot;&quot;
    tools = [basic_calculator, reverse_string]
    model_service = OllamaModel
    model_name = &quot;llama2&quot;
    stop = &quot;&lt;|eot_id|&gt;&quot;
    agent = Agent(tools=tools, model_service=model_service, model_name=model_name, stop=stop)
    print(&quot;\nWelcome to the AI Agent! Type &#x27;exit&#x27; to quit.&quot;)
    print(&quot;You can ask me to:&quot;)
    print(&quot;1. Perform calculations (e.g., &#x27;Calculate 15 plus 7&#x27;)&quot;)
    print(&quot;2. Reverse strings (e.g., &#x27;Reverse hello world&#x27;)&quot;)
    print(&quot;3. Answer general questions\n&quot;)
    while True:
        prompt = input(&quot;Ask me anything: &quot;)
        if prompt.lower() == &quot;exit&quot;:
            break
        agent.work(prompt)
```

## 结论

在这篇博文中，我们从理解智能体是什么开始，逐步实现了一个智能体。我们设置了环境，定义了模型，创建了必要的工具，并构建了一个结构化的工具箱来支持智能体的功能。最后，我们通过运行智能体，将所有内容整合到了一起。