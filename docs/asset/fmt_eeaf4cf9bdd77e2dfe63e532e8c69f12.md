
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
