# 0-1搭建一个决策执行Agent

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/Di4OdCdOcoNSfVxC76jcIfeHn1f](https://qqs7y1hozd1.feishu.cn/docx/Di4OdCdOcoNSfVxC76jcIfeHn1f)

> 本文使用 Python 从零开始构建一个任务执行Agent。

> 该Agent能够根据用户输入做出决策、选择适当的工具并执行相应的任务。

![](img/63237a28113d38a1cd97f83e620bf9d2.png)

#####

##### **代码库：**[**AI Agents**](https://github.com/vsingh9076/AI-Agents/tree/main/build-agent-from-scratch)

+   **Python 环境设置**

> 安装所需的依赖项，导航至本仓库代码页面，按照requirements.txt安装所需依赖。

**指令如下：**

```
pip install -r requirements.txt
```

#####

+   **在本地设置 Ollama**

> 拉取模型，某些Agent的实现可能需要特定的模型。

**可以使用以下命令拉取模型：**

```
ollama pull mistral  # Replace &#x27;mistral&#x27; with the model needed 
```

## **定义Model类**

**实现的具体流程如下：**

![](img/ed866fa9fe389ea340ec62059c7ac588.png)

> 除了 Python，还需要安装一些必要的库。

> 在本教程中，我们将使用 requests、json 和 termcolor。此外，我们还将使用 dotenv 来管理环境变量。

```
pip install requests termcolor python-dotenv
```

首先需要一个处理用户输入的模型。

> 创建一个 OllamaModel 类，该类与本地 API 交互以生成响应。

**下面是代码实现：**

```
from termcolor import colored
import os
from dotenv import load_dotenv
load_dotenv()
### Models
import requests
import json
import operator
```

```
class OllamaModel:
    def __init__(self, model, system_prompt, temperature=0, stop=None):
        &quot;&quot;&quot;
        Initializes the OllamaModel with the given parameters.
        Parameters:
        model (str): The name of the model to use.
        system_prompt (str): The system prompt to use.
        temperature (float): The temperature setting for the model.
        stop (str): The stop token for the model.
        &quot;&quot;&quot;
        self.temperature = temperature
        self.model = model
        self.system_prompt = system_prompt
        self.headers = {&quot;Content-Type&quot;: &quot;application/json&quot;}
        self.stop = stop
    def generate_text(self, prompt):
        &quot;&quot;&quot;
        Generates a response from the Ollama model based on the provided prompt.
        Parameters:
        prompt (str): The user query to generate a response for.
        Returns:
        dict: The response from the model as a dictionary.
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
                headers=self.headers, 
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

generate_text 函数向模型 API 发送请求并返回响应。

> 该类使用参数model、system_prompt、temperature和stop token进行初始化。

## **2.定义 Agent 的角色**

**最初的的agent模样：**

> 提供工具和prompt，完成agent自行探索-分析-决策-执行

>

![](img/ac1557178b46412fc1c929dcf33c99af.png)

> 尽管从宏观角度上来说这样并没有错，但这却让我踩进了第一个坑

> 让LLM控制一切：给LLM一堆工具让它自由发挥企图它能够按照心里的目标实现

> 而让LLM控制一切的结果就是，不完善的prompt和tools带来了无限循环。

## **3.创建Agent所需工具**

下一步是创建智能体Agent可以使用的工具。

> 这些工具是执行特定任务的简单 Python 函数。

下面是一个基本计算器和一个字符串反转器：

```
def basic_calculator(input_str):
    &quot;&quot;&quot;
    Perform a numeric operation on two numbers based on the input string or dictionary.    
    Parameters:
    input_str (str or dict): Either a JSON string representing a dictionary with keys &#x27;num1&#x27;, &#x27;num2&#x27;, and &#x27;operation&#x27;,
                                or a dictionary directly. Example: &#x27;{&quot;num1&quot;: 5, &quot;num2&quot;: 3, &quot;operation&quot;: &quot;add&quot;}&#x27;                            
                                or {&quot;num1&quot;: 67869, &quot;num2&quot;: 9030393, &quot;operation&quot;: &quot;divide&quot;}
    Returns:
    str: The formatted result of the operation.
    Raises:
    Exception: If an error occurs during the operation (e.g., division by zero).
    ValueError: If an unsupported operation is requested or input is invalid.
    &quot;&quot;&quot;
    try:
        # Handle both dictionary and string inputs
        if isinstance(input_str, dict):
            input_dict = input_str
        else:
            # Clean and parse the input string
            input_str_clean = input_str.replace(&quot;&#x27;&quot;, &quot;\&quot;&quot;)
            input_str_clean = input_str_clean.strip().strip(&quot;\&quot;&quot;)
            input_dict = json.loads(input_str_clean)
            
        # Validate required fields
        if not all(key in input_dict for key in [&#x27;num1&#x27;, &#x27;num2&#x27;, &#x27;operation&#x27;]):
            return &quot;Error: Input must contain &#x27;num1&#x27;, &#x27;num2&#x27;, and &#x27;operation&#x27;&quot;
        num1 = float(input_dict[&#x27;num1&#x27;])  # Convert to float to handle decimal numbers
        num2 = float(input_dict[&#x27;num2&#x27;])
        operation = input_dict[&#x27;operation&#x27;].lower()  # Make case-insensitive
    except (json.JSONDecodeError, KeyError) as e:
        return &quot;Invalid input format. Please provide valid numbers and operation.&quot;
    except ValueError as e:
        return &quot;Error: Please provide valid numerical values.&quot;
    # Define the supported operations with error handling
    operations = {
        &#x27;add&#x27;: operator.add,
        &#x27;plus&#x27;: operator.add,  # Alternative word for add
        &#x27;subtract&#x27;: operator.sub,
        &#x27;minus&#x27;: operator.sub,  # Alternative word for subtract
        &#x27;multiply&#x27;: operator.mul,
        &#x27;times&#x27;: operator.mul,  # Alternative word for multiply
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
    # Check if the operation is supported
    if operation not in operations:
        return f&quot;Unsupported operation: &#x27;{operation}&#x27;. Supported operations are: {&#x27;, &#x27;.join(operations.keys())}&quot;
    try:
        # Special handling for division by zero
        if (operation in [&#x27;divide&#x27;, &#x27;floor_divide&#x27;, &#x27;modulus&#x27;]) and num2 == 0:
            return &quot;Error: Division by zero is not allowed&quot;
        # Perform the operation
        result = operations[operation](num1, num2)
        
        # Format result based on type
        if isinstance(result, bool):
            result_str = &quot;True&quot; if result else &quot;False&quot;
        elif isinstance(result, float):
            # Handle floating point precision
            result_str = f&quot;{result:.6f}&quot;.rstrip(&#x27;0&#x27;).rstrip(&#x27;.&#x27;)
        else:
            result_str = str(result)
        return f&quot;The answer is: {result_str}&quot;
    except Exception as e:
        return f&quot;Error during calculation: {str(e)}&quot;
def reverse_string(input_string):
    &quot;&quot;&quot;
    Reverse the given string.
    Parameters:
    input_string (str): The string to be reversed.
    Returns:
    str: The reversed string.
    &quot;&quot;&quot;
    # Check if input is a string
    if not isinstance(input_string, str):
        return &quot;Error: Input must be a string&quot;
        
    # Reverse the string using slicing
    reversed_string = input_string[::-1]
    
    # Format the output
    result = f&quot;The reversed string is: {reversed_string}&quot;
    
    return result
```

这些函数旨在根据所提供的输入执行特定任务。

> basic_calculator 处理算术运算，而 reverse_string 则反转给定的字符串。

## 4.Agent的行为范式

业界已经有很多关于 Agent 行为范式的探讨：

> **ReAct 的核心是 Thought -> Action** -> Observation 的循环

![](img/787df5e6a1832fa16d2570e6754ff093.png)

LLM 思考一下，决定调用一个工具（Action），然后观察工具返回的结果，再进行下一轮思考 。

该框架本质上创建了一个反馈循环。

> 每次完成此循环时（即每次代理采取行动并根据该行动的结果进行观察时），代理必须决定是否重复或结束循环。

> LangGraph 中的 ReAct 代理模块通过预定义的系统提示实现，与该模块一起使用的 LLM 不需要任何其他示例即可充当 ReAct 代理。

## ** 5.创建工具箱**

工具箱ToolBox内存储了智能体可以使用的所有工具，并提供了每种工具的说明：

```
class ToolBox:
    def __init__(self):
        self.tools_dict = {}
    def store(self, functions_list):
        &quot;&quot;&quot;
        Stores the literal name and docstring of each function in the list.
        Parameters:
        functions_list (list): List of function objects to store.
        Returns:
        dict: Dictionary with function names as keys and their docstrings as values.
        &quot;&quot;&quot;
        for func in functions_list:
            self.tools_dict[func.__name__] = func.__doc__
        return self.tools_dict
    def tools(self):
        &quot;&quot;&quot;
        Returns the dictionary created in store as a text string.
        Returns:
        str: Dictionary of stored functions and their docstrings as a text string.
        &quot;&quot;&quot;
        tools_str = &quot;&quot;
        for name, doc in self.tools_dict.items():
            tools_str += f&quot;{name}: \&quot;{doc}\&quot;\n&quot;
        return tools_str.strip()
```

这个类将帮助智能体了解哪些工具可用以及每种工具的具体用途。

## 6.plan-and-execute

plan-and-execute旨在克服ReAct类代理的局限性。

> 通过在执行任务前明确规划所有必要步骤。这种方法旨在提高效率、降低成本并提升整体性能。

![](img/4c848afdd68dd47b8bec1e886bbd8aea.png)

> **其核心工作流程包含三个阶段：**

+   **规划阶段 ：**接收用户输入，并生成一个用于完成大型任务的多步骤计划或任务清单；

+   **执行阶段 ：**接收计划中的步骤，并调用一个或多个工具来按顺序完成每个子任务；

+   **重规划阶段：**根据执行结果动态调整计划或返回；

## 7.ReWOO（ Reasoning WithOut Observation ）

ReWOO，

一种创新的增强语言模型 (Augmented Language Model, ALM) 框架。

![](img/dd8d599413ffd3a58f0ac6bbdb699aba.png)

> 它将复杂任务的解决过程分解为三个独立的模块：

+   **规划器 (Planner)：** 基于 LLM 推理，提前生成任务蓝图（步骤顺序与逻辑），无需工具实时反馈。

+   **执行器 (Worker)： **按蓝图并行调用外部工具（如搜索、计算器），收集证据。

+   **求解器 (Solver)： **综合分析蓝图和执行证据，生成最终答案（含纠错总结）。

## **8.创建Agent类**

Agent需要思考、决定使用哪种工具并执行它。

**系统提示词如下：**

```
agent_system_prompt_template = &quot;&quot;&quot;
You are an intelligent AI assistant with access to specific tools. Your responses must ALWAYS be in this JSON format:
{{
    &quot;tool_choice&quot;: &quot;name_of_the_tool&quot;,
    &quot;tool_input&quot;: &quot;inputs_to_the_tool&quot;
}}
TOOLS AND WHEN TO USE THEM:
1. basic_calculator: Use for ANY mathematical calculations
  - Input format: {{&quot;num1&quot;: number, &quot;num2&quot;: number, &quot;operation&quot;: &quot;add/subtract/multiply/divide&quot;}}
  - Supported operations: add/plus, subtract/minus, multiply/times,
  - Example inputs and outputs:
   Input: &quot;Calculate 15 plus 7&quot;
    Output: {{&quot;tool_choice&quot;: &quot;basic_calculator&quot;, &quot;tool_input&quot;: 
```

> **该类有三个主要方法：**

+   **prepare_tools****: **存储并返回工具说明。

+   **think****:** 根据用户提示决定使用哪种工具。

+   **work**: 执行所选工具并返回结果。

## 9.“结构化工作流”Agent

> 设计一套固定的、结构化的工作流。

> 在这个范式里，人类负责定义“骨架”（Workflow），AI 负责填充“血肉”（Analysis & Generation）。

>

> 并且为了快速获得测试结果，我将任务拆分，从一个项目的部署物生成到存在docker-compose项目的部署物生成。

>

**使用 LangGraph **来编排这个工作流。下面是 MVP 的时序图：

![](img/2ae217c9050e809bc9a053ba0bb45bbb.png)

## **10.运行Agent**

最后，让将所有内容整合在一起，运行Agent智能体。

**在脚本的****main**入口函数内，初始化Agent并开始接受用户输入：

```
# Example usage
if __name__ == &quot;__main__&quot;:
    &quot;&quot;&quot;
    Instructions for using this agent:
    
    Example queries you can try:
    1. Calculator operations:       
     - &quot;Calculate 15 plus 7&quot;       
     - &quot;What is 100 divided by 5?&quot;       
     - &quot;Multiply 23 and 4&quot;
     
     2. String reversal:      
      - &quot;Reverse the word &#x27;hello world&#x27;&quot;      
      - &quot;Can you reverse &#x27;Python Programming&#x27;?&quot;
      
     3. General questions (will get direct responses):     
      - &quot;Who are you?&quot;      
      - &quot;What can you help me with?&quot;
     
     Ollama Commands (run these in terminal):   
      - Check available models:    &#x27;ollama list&#x27;   
      - Check running models:      &#x27;ps aux | grep ollama&#x27;   
      - List model tags:          &#x27;curl http://localhost:11434/api/tags&#x27;    - Pull a new model:         &#x27;ollama pull mistral&#x27;   
      - Run model server:         &#x27;ollama serve&#x27;
     &quot;&quot;&quot;    
     tools = [basic_calculator, reverse_string]
      # Uncomment below to run with OpenAI   
      # model_service = OpenAIModel   
      # model_name = &#x27;gpt-3.5-turbo&#x27;   
      # stop = None   
      # Using Ollama with llama2 model
      
```

这个部分正好贴合了Antropic的 Barry **Zhang 提出 A**gent 的概念，即在循环（Loop）中使用工具的模型。

![](img/ee163f606b5535f9701e870bea14be8c.png)