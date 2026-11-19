
# Agent智能客服对话系统|工作流搭建

> 来源：[https://v11enp9ok1h.feishu.cn/docx/KUtXdy58io4wuixnYWScyRRmnBR](https://v11enp9ok1h.feishu.cn/docx/KUtXdy58io4wuixnYWScyRRmnBR)

## 背景信息

### 问题背景

1. 在企业服务用户的过程中，每天都会出现大量相关领域内的问题。
1. 传统的人工客服作息服务解答效率低，且成本高。
1. 智能客服对话系统可以帮助企业实现在线服务的智能化人机协作系统，提高坐席服务的效率、降低人力开销及新人业务学习成本。

### 解决方案

+ 机器学习平台PAI在智能客服领域，提供了端到端的纯白盒解决方案。客户只需要准备好自己相关领域的FAQ（常见问题）和知识图谱数据，就可以利用PAI自定义的搭建从算法构建到模型部署的人工智能流程，形成完整的端到端解决方案，实现对应领域智能客服的业务系统。

+ 本文以“汽车售前咨询”这一业务领域为例，展示如何快速搭建智能客服机器人，从而实现机器人自动回答用户关于汽车售前业务的相关问题。这样一方面节省客户的咨询等待和信息检索时间，另一方面节省企业运营人力成本，使精力可以集中在筛选出的高价值客户上。

### 方案优势

+ **纯白盒**：

可根据您自己具体的业务场景，自定义构建智能客服业务系统。

+ **端到端**：

从最初的数据准备到最后的模型部署推理，提供全链路的系统构建流程。

+ **有据可依**：

对话系统内所有的回答都是有依据的，避免纯深度学习方案的不可解释性。

+ **鲁棒可控**：

系统任何地方出了问题，都有相应的异常处理与bad case分析应对机制。

### 工程部署架构

> 用户将知识图谱数据，模型和特征文件以及FAQ数据存储在OSS上，然后将智能客服对话系统服务部署在PAI-EAS上就可以使用FAQ检索和知识图谱查询提供的功能来做智能客服问答。

> 详见第五章：智能客服在线服务配置。

---

> 系统初始化后进入闲聊状态，对用户输入“您好”等话术进行响应。响应完成后系统进入问题接收状态，此刻等待用户提问。接入通过实体归一化/属性归一化来对用户提问中的实体和属性进行明确，接着进入问答状态进行FAQ检索或者知识图谱查询，当出现异常状况时，进入异常处理模式，在异常处理中引导用户进入正确的查询。

---

## 基于FAQ常见问题检索的问答

基于FAQ（常见问题）的问答系统总体结构如下图所示，系统的关键模块包括：

+ 频繁问答对数据集，即FAQ问答库。
+ 预处理模块：负责对`query`进行分词等操作。
+ 检索模块：负责从FAQ中检索可能与用户查询相似的若干标准问句。
+ 相似问句选择模块：负责从候选相似问句中，选择与用户查询最相似的标准问句。

具体而言，检索模块会在对用户提出的`query`进行预处理的基础上，从FAQ数据集中检索`K`个与`query`最相似的问句。然后相似问句选择模块会从`K`个候选相似问句中，选出与`query`相似度最高的，并判断是否具有足够的置信度。

如果相似度最高的候选相似问句的置信度达标，“相似问句选择模块”就会判定该问句是用户`query`的同义问句，从而返回提前预置好的答案。

### FAQ频繁问答对数据集准备

FAQ，即Frequently Asked Questions，是业务场景中用户最常问的问题，又称“标准问题”。

+ 对这类问题，我们可以提前编制答案，构成问答对。问答对通常简称“QA对”（Question Answer Pair）。问答系统的知识库中存储的FAQ数据集，实际上是标准问题其答案构成的QA对数据集。如果用户提出的问题和FAQ数据集中的某个标准问题是相似问题，那么用户问题的答案就是该标准问题的答案。

本文以“汽车售前”的智能客服场景为例，展示FAQ所需的数据，文件列表如下所示：

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

首先对FAQ频繁问答库中的标准问题进行`ID`化编号，比如针对上面FAQ中的标准问题，`ID`化后，问题和数字之间用`tab`分隔。

下面为示例文件`query_label_idx_map.txt`的部分数据样例。

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

#### 步骤二：配置同义问题

准备好上面的标注问题ID编号文件后，现在需要为每个标准问题配置若干个同义问题。

以“保养灯的使用说明”为例，文件的每一行内容对应一个和“保养灯的使用说明”同义的问题。下面为示例文件`保养灯的使用说明`的部分数据样例。

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

然后将所有的标准问题同义配置文件放到文件夹`questions`中。

#### 步骤三：配置标准答案

准备好上面的标准问题列表后，下面介绍汽车售前FAQ数据问答数据集的准备。

以“特斯拉_model3_2021款_标准续航升级版”这款车为例，其中[]中表示问题类型，后面展示问题的答案，其中答案和问题类型之间用tab分隔。下面为示例文件`特斯拉_model3_2021款_标准续航升级版`的部分数据样例。

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

然后将所有的标准答案配置文件放到文件夹`answers`中。

### 问答特征向量化

利用PAI提供的可视化建模Studio平台，基于海量大数据语料预训练获得的NLP迁移学习模型，进行用户`query`向量表征模型的构建。其中示例实验用到的训练集、评估集、测试集。

1. 进入Designer控制台
1. 登录[PAI控制台](https://pai.console.aliyun.com/?spm=a2c0j.14094439.J_5834642020.3.32fb426aLl60J6&projectId=&regionId=#/overview)。
1. 在左侧导航栏中，选择模型开发和训练->可视化建模（Designer）。
1. 构建实验
1. 在可视化建模（Designer）控制台中，选择实验模板，在实验模板列表中找到智能客服问答特征向量化，点击并创建实验。
1. 在实验列表中找到刚才创建好的模板实验，并进入实验。
1. 系统根据预置的模板，自动构建实验，如下图所示。运行过程中，点击组件右键，可查看运行日志。

## 基于KBQA的知识图谱问答

KBQA的全称是基于知识库问答（Knowledge Base Question Answering），即给定自然语言问题，通过对问题进行语义理解和解析，进而基于知识图谱构建的知识库进行查询、推理得出答案。

基于知识图谱的精准问答子系统的关键模块包括：

+ 实体链接：将用户问句中提到的实体关联到唯一的实体表达。
+ 语义角色标注：对属性的类型进行自动打标。
+ 意图分类：判断用户问的是什么类型的问题。
+ 查询链路生成：生成查询知识库的查询语句
+ 答案生成：负责按照问句意图的类型，在答案模板里生成答案。

基于KBQA的知识图谱问答的架构图如下所示：

### 知识图谱数据准备

针对实体、属性名称和属性值，智能客户系统采用可读性强的JSON来构建数据文件。

属性名可以包括子属性，以“特斯拉model3_2021款_标准续航升级版”为例，车轮制动属性包括：前制动器类型，后制动器类型，驻车制动类型这三种子属性。用户需要将业务中的数据转换成JSON格式。

下面为示例文件`特斯拉model3_2021款_标准续航升级版.json`的部分数据样例。

```
{        
  "实体": 
  {           "名称":
  "特斯拉model3_2021款_标准续航升级版",          
  "关键词": "特斯拉|model3|2021款|标准续航升级版" 
    },          
    "属性":
    {                  
    "厂商":"特斯拉中国",                  
    "品牌":"特斯拉",                  
    "经销商报价":"27.6万",                 
     "级别":"中型车",          
     "上市时间":"2021.07",          
     "纯电续航里程":"525km",         
     "充电时间":"快充一小时，慢充10小时",          
     "百公里加速时间":"5.6s",          
     "整车保修期限":"4年或8万公里"       
     },      
     "变速箱":{                      
     "变速箱档位个数":"6个",                  
     "变速箱类型":"电动车单速变速箱"         },      
     "配置": {                
     "内部配置": {                      
     "空调": {                          
     "空调控制方式": "自动",                    
     "温度分区控制": "有",                    
     "后座出风口": "无"                  },                
     "行车电脑显示屏": "有",                
     "倒车视频影像": "有"     } 
            } 
    }
```

## 二、实现过程

现在，让我们挽起袖子，开始动手构建吧！

![](img/80cb8ddba95e2fe151d507f35d2cea72.png)

### 2.1 准备工作

本教程的完整代码可在AI智能体的GitHub仓库中获取。你可以在“Build an Agent from Scratch”这里找到具体实现。在运行代码之前，请确保你的系统满足以下先决条件：

#### Python环境设置

运行AI智能体需要安装Python。按照以下步骤来设置你的环境：

+ 安装Python（如果尚未安装）：从http://python.org下载并安装Python（推荐3.8及以上版本）。
+ **验证安装**：在命令行中输入`python --version`，查看是否正确安装。
+ **创建虚拟环境（推荐）**：使用虚拟环境来管理依赖项是个不错的选择。在命令行中输入`python -m venv ai_agents_env`创建虚拟环境，然后通过`source ai_agents_env/bin/activate`激活它。
+ **安装所需依赖项**：导航到代码仓库目录，然后在命令行中输入`pip install -r requirements.txt`来安装所需的依赖库。

#### 本地设置Ollama

Ollama用于高效地运行和管理本地语言模型。按照以下步骤安装和配置它：

+ **下载并安装Ollama**：访问Ollama的官方网站，下载适合你操作系统的安装程序，并按照平台对应的说明进行安装。
+ **验证Ollama安装**：在命令行中运行`ollama --version`，检查Ollama是否正确安装。
+ **拉取模型（如有需要）**：有些智能体实现可能需要特定的模型。你可以使用`ollama pull mistral`命令拉取模型。

#### AI Agent 选型

第二步，我们需要根据场景选择 AI Agent 开发平台，选择合适的大模型，并选用不同的工具来执行不同系统的操作。

#### 选择哪个 AI Agent 平台

先说说 AI Agent 开发平台，Dify、Coze、FastGPT 等这么多无代码 Agent 开发平台，我们该选择哪个比较合适呢？

+ **Coze** 只能使用云端，不能在本地部署。
+ **Dify** 是完全开源的，没有限制，但在知识问答方面的能力较弱。
+ **FastGPT** 的使用有一定限制，但在知识回答方面的能力相对较强。

> 更高级的开发平台，如 LangGraph、CrewAI 等，可以让 AI 自我规划并执行任务，但是需要编写代码。

> 是否选择这些平台，取决于我们的具体需求，当然也可以混合使用。

> 这就要求我们深入了解每个开发平台的特点，它擅长什么、不擅长什么，存在哪些明显的缺陷。只有掌握了这些信息，才能根据我们的场景做出合适的选型。

#### 选择哪个 LLM

再说说大模型的选择，现在有海外的 OpenAI、Claude、Gemini，也有国内的 Kim、通义千问和近期爆火的 DeepSeek，还有一些开源模型如 LLaMA、Grok，也有像 Mistral 这样的小模型。

![](img/8a9057e88a6c34cd1396b1e4add1b611.png)

### 2.2 实现步骤

#### 步骤1：设置环境

除了Python，我们还需要安装一些必要的库。在本教程中，我们将使用`requests`、`json`和`termcolor`库。另外，我们会使用`dotenv`来管理环境变量。在命令行中输入`pip install requests termcolor python-dotenv`进行安装。

#### 步骤2：定义模型类

我们首先需要一个能够处理用户输入的模型。我们将创建一个`OllamaModel`类，它通过与本地API进行交互来生成响应。以下是基本实现代码：



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
        """
        用给定的参数初始化OllamaModel。
        参数:
        model (str): 要使用的模型名称。
        system_prompt (str): 要使用的系统提示。
        temperature (float): 模型的温度设置。
        stop (str): 模型的停止标记。
        """
        self.model_endpoint = "http://localhost:11434/api/generate"
        self.temperature = temperature
        self.model = model
        self.system_prompt = system_prompt
        self.headers = {"Content-Type": "application/json"}
        self.stop = stop

    def generate_text(self, prompt):
        """
        根据提供的提示从Ollama模型生成响应。
        参数:
        prompt (str): 用于生成响应的用户查询。
        返回:
        dict: 模型的响应，以字典形式返回。
        """
        payload = {
            "model": self.model,
            "format": "json",
            "prompt": prompt,
            "system": self.system_prompt,
            "stream": False,
            "temperature": self.temperature,
            "stop": self.stop
        }
        try:
            request_response = requests.post(
                self.model_endpoint,
                headers=self.headers,
                data=json.dumps(payload)
            )
            print("REQUEST RESPONSE", request_response)
            request_response_json = request_response.json()
            response = request_response_json['response']
            response_dict = json.loads(response)
            print(f"\n\nResponse from Ollama model: {response_dict}")
            return response_dict
        except requests.RequestException as e:
            response = {"error": f"Error in invoking model! {str(e)}"}
            return response
```

这个类使用模型名称、系统提示、温度和停止标记进行初始化。`generate_text`方法向模型API发送请求并返回响应。

### 步骤3：为智能体创建工具

接下来是为我们的智能体创建可用的工具。这些工具是执行特定任务的简单Python函数。以下是一个基本计算器和字符串反转器的示例：

```
def basic_calculator(input_str):
    """
    根据输入字符串或字典对两个数字执行数值运算。
    参数:
    input_str (str或dict): 要么是表示包含'num1'、'num2'和'operation'键的字典的JSON字符串，
    要么是直接的字典。例如: '{"num1": 5, "num2": 3, "operation": "add"}'
    或{"num1": 67869, "num2": 9030393, "operation": "divide"}
    返回:
    str: 运算的格式化结果。
    抛出:
    Exception: 如果在运算过程中发生错误（例如，除以零）。
    ValueError: 如果请求了不支持的运算或输入无效。
    """
    try:
        if isinstance(input_str, dict):
            input_dict = input_str
        else:
            input_str_clean = input_str.replace("'", "\"")
            input_str_clean = input_str_clean.strip().strip('"')
            input_dict = json.loads(input_str_clean)
        if not all(key in input_dict for key in ['num1', 'num2', 'operation']):
            return "Error: Input must contain 'num1', 'num2', and 'operation'"
        num1 = float(input_dict['num1'])
        num2 = float(input_dict['num2'])
        operation = input_dict['operation'].lower()
    except (json.JSONDecodeError, KeyError) as e:
        return "Invalid input format. Please provide valid numbers and operation."
    except ValueError as e:
        return "Error: Please provide valid numerical values."
    operations = {
        'add': operator.add,
        'plus': operator.add,
        'subtract': operator.sub,
        'minus': operator.sub,
        'multiply': operator.mul,
        'times': operator.mul,
        'divide': operator.truediv,
        'floor_divide': operator.floordiv,
        'modulus': operator.mod,
        'power': operator.pow,
        'lt': operator.lt,
        'le': operator.le,
        'eq': operator.eq,
        'ne': operator.ne,
        'ge': operator.ge,
        'gt': operator.gt
    }
    if operation not in operations:
        return f"Unsupported operation: '{operation}'. Supported operations are: {', '.join(operations.keys())}"
    try:
        if (operation in ['divide', 'floor_divide', 'modulus']) and num2 == 0:
            return "Error: Division by zero is not allowed"
        result = operations[operation](num1, num2)
        if isinstance(result, bool):
            result_str = "True" if result else "False"
        elif isinstance(result, float):
            result_str = f"{result:.6f}".rstrip('0').rstrip('.')
        else:
            result_str = str(result)
        return f"The answer is: {result_str}"
    except Exception as e:
        return f"Error during calculation: {str(e)}"


def reverse_string(input_string):
    """
    反转给定的字符串。
    参数:
    input_string (str): 要反转的字符串。
    返回:
    str: 反转后的字符串。
    """
    if not isinstance(input_string, str):
        return "Error: Input must be a string"
    reversed_string = input_string[::-1]
    result = f"The reversed string is: {reversed_string}"
    return result
```

这些函数根据提供的输入执行特定任务。`basic_calculator`处理算术运算，而`reverse_string`则反转给定的字符串。

---

### 步骤4：构建工具箱

[`ToolBox`](https://zhida.zhihu.com/search?content_id=254820407&content_type=Article&match_order=1&q=ToolBox&zhida_source=entity)类用于存储智能体可以使用的所有工具，并为每个工具提供描述：

```
class ToolBox:
    def __init__(self):
        self.tools_dict = {}

    def store(self, functions_list):
        """
        存储列表中每个函数的名称和文档字符串。
        参数:
        functions_list (list): 要存储的函数对象列表。
        返回:
        dict: 以函数名称为键，其文档字符串为值的字典。
        """
        for func in functions_list:
            self.tools_dict[func.__name__] = func.__doc__
        return self.tools_dict

    def tools(self):
        """
        将store方法中创建的字典转换为文本字符串返回。
        返回:
        str: 存储的函数及其文档字符串的字典，以文本字符串形式返回。
        """
        tools_str = ""
        for name, doc in self.tools_dict.items():
            tools_str += f"{name}: \"{doc}\"\n"
        return tools_str.strip()
```

这个类将帮助智能体了解哪些工具可用以及每个工具的用途。

### 步骤5：创建智能体类

智能体需要进行思考、决定使用哪个工具并执行它。以下是`Agent`类的代码：



```
agent_system_prompt_template = """
你是一个智能AI助手，可以使用特定的工具。你的回复必须始终采用以下JSON格式：
{
    "tool_choice": "name_of_the_tool",
    "tool_input": "inputs_to_the_tool"
}
工具及使用场景：
1. basic_calculator：用于任何数学计算
    - 输入格式: {"num1": number, "num2": number, "operation": "add/subtract/multiply/divide"}
    - 支持的运算: add/plus, subtract/minus, multiply/times, divide
    - 示例输入和输出:
        - 输入: "Calculate 15 plus 7"
        - 输出: {"tool_choice": "basic_calculator", "tool_input": {"num1": 15, "num2": 7, "operation": "add"}}
        - 输入: "What is 100 divided by 5?"
        - 输出: {"tool_choice": "basic_calculator", "tool_input": {"num1": 100, "num2": 5, "operation": "divide"}}
2. reverse_string：用于任何涉及反转文本的请求
    - 输入格式: 仅需反转的文本字符串
    - 当用户提到“reverse”、“backwards”或要求反转文本时，始终使用此工具
    - 示例输入和输出:
        - 输入: "Reverse of 'Howwwww'?"
        - 输出: {"tool_choice": "reverse_string", "tool_input": "Howwwww"}
        - 输入: "What is the reverse of Python?"
        - 输出: {"tool_choice": "reverse_string", "tool_input": "Python"}
3. no tool：用于一般对话和问题
    - 示例输入和输出:
        - 输入: "Who are you?"
        - 输出: {"tool_choice": "no tool", "tool_input": "I am an AI assistant that can help you with calculations, reverse text, and answer questions. I can perform mathematical operations and reverse strings. How can I help you today?"}
        - 输入: "How are you?"
        - 输出: {"tool_choice": "no tool", "tool_input": "I'm functioning well, thank you for asking! I'm here to help you with calculations, text reversal, or answer any questions you might have."}
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
"""
```

```
class Agent:
    def __init__(self, tools, model_service, model_name, stop=None):
        """
        用工具列表和模型初始化智能体。
        参数:
        tools (list): 工具函数列表。
        model_service (class): 具有generate_text方法的模型服务类。
        model_name (str): 要使用的模型名称。
        """
        self.tools = tools
        self.model_service = model_service
        self.model_name = model_name
        self.stop = stop

    def prepare_tools(self):
        """
        将工具存储在工具箱中并返回其描述。
        返回:
        str: 存储在工具箱中的工具的描述。
        """
        toolbox = ToolBox()
        toolbox.store(self.tools)
        tool_descriptions = toolbox.tools()
        return tool_descriptions

    def think(self, prompt):
        """
        使用系统提示模板和工具描述在模型上运行generate_text方法。
        参数:
        prompt (str): 用于生成响应的用户查询。
        返回:
        dict: 模型的响应，以字典形式返回。
        """
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
        """
        解析think方法返回的字典并执行相应的工具。
        参数:
        prompt (str): 用于生成响应的用户查询。
        返回:
        执行相应工具的响应，如果未找到匹配的工具，则返回tool_input。
        """
        agent_response_dict = self.think(prompt)
        tool_choice = agent_response_dict.get("tool_choice")
        tool_input = agent_response_dict.get("tool_input")
        for tool in self.tools:
            if tool.__name__ == tool_choice:
                response = tool(tool_input)
                print(colored(response, 'cyan'))
                return
        print(colored(tool_input, 'cyan'))
        return
```

这个类有三个主要方法：

+   `prepare_tools`：存储并返回工具的描述。

+   `think`：根据用户提示决定使用哪个工具。

+   `work`：执行选择的工具并返回结果。

## 步骤6：运行智能体

最后，让我们把所有内容整合起来，运行我们的智能体。在脚本的主程序部分，初始化智能体并开始接受用户输入：

```
if __name__ == "__main__":
    """
    使用此智能体的说明：
    你可以尝试的示例查询：
    1. 计算器运算:
        - "Calculate 15 plus 7";
        - "What is 100 divided by 5?";
        - "Multiply 23 and 4";
    2. 字符串反转:
        - "Reverse the word 'hello world'";
        - "Can you reverse 'Python Programming'?";
    3. 一般问题（将得到直接回复）:
        - "Who are you?";
        - "What can you help me with?";
    Ollama命令（在终端中运行这些命令）:
        - 查看可用模型:    'ollama list';
        - 查看正在运行的模型:      'ps aux | grep ollama';
        - 列出模型标签:          'curl http://localhost:11434/api/tags';
        - 拉取新模型:         'ollama pull mistral';
        - 运行模型服务器:         'ollama serve';
    """
    tools = [basic_calculator, reverse_string]
    model_service = OllamaModel
    model_name = "llama2"
    stop = "<|eot_id|>"
    agent = Agent(tools=tools, model_service=model_service, model_name=model_name, stop=stop)
    print("\nWelcome to the AI Agent! Type 'exit' to quit.")
    print("You can ask me to:")
    print("1. Perform calculations (e.g., 'Calculate 15 plus 7')")
    print("2. Reverse strings (e.g., 'Reverse hello world')")
    print("3. Answer general questions\n")
    while True:
        prompt = input("Ask me anything: ")
        if prompt.lower() == "exit":
            break
        agent.work(prompt)
```

## 结论

在这篇博文中，我们从理解智能体是什么开始，逐步实现了一个智能体。我们设置了环境，定义了模型，创建了必要的工具，并构建了一个结构化的工具箱来支持智能体的功能。最后，我们通过运行智能体，将所有内容整合到了一起。
