
# telco_data 数据集介绍

## 数据集概述

| Churn | 用户是否流失 | No, Yes | Yes (是), No (否) |

```python
messages = [
    {"role": "system", "content": data_dictionary}, 
    {"role": "user", "content": "请帮我简单介绍telco_data数据集"}
]
```

```python
client = ZhipuAI(api_key=api_key) 
response = client.chat.completions.create(
    model="glm-4",  
    messages=messages
)
display(Markdown(response.choices[0].message.content))
```

telco_data 数据集是一个关于电信行业用户流失情况的数据集，它包含了描述用户基本情况和其服务使用状况的多项指标，以及一个关键的标签字段，指明用户是否已经流失。这个数据集对于电信公司来说非常有价值，可以帮助它们分析用户流失的原因，从而采取相应措施来改善客户留存率。

以下是 telco_data 数据集的简单介绍：

- **数据集目的**：分析用户特征与用户流失之间的关系，预测并减少用户流失。
- **数据集结构**：
  - **字段（Column Name）**：数据集包含多个字段，每个字段都描述了用户的不同属性，如性别、年龄、服务使用情况等。
  - **描述（Description）**：每个字段都有对应的描述，说明了该字段所代表的内容。
  - 取值范围（Value Range）：指明了每个字段可能的取值。
  - 取值解释（Value Explanation）：对每个字段可能的取值进行了详细解释。
- **关键字段**：
  - `customerID`：唯一标识每个用户的 ID。
  - `Churn`：这是一个关键字段，表明用户是否已经流失（`Yes` 表示流失，`No` 表示未流失）。
- **用户特征**：包括性别、年龄（是否为老人）、家庭状况（伴侣、经济独立）、服务使用情况（电话服务、互联网服务类型、增值服务）等。
- **服务使用相关字段**：包含用户的服务订阅情况，如网络安全服务、在线备份、设备保护、技术支持、网络电视等。
- **合同与财务信息**：包括用户的合同类型、账单方式、月费用和总费用等。
- **存储位置**：数据集以 CSV 格式存储在本地文件夹中，文件名为 `telco_data.csv`。

通过分析 telco_data 数据集，电信公司可以识别可能导致用户流失的风险因素，并据此调整他们的服务、定价策略或客户服务策略，以提高用户满意度和忠诚度。

```python
messages = [
    {"role": "system", "content": data_dictionary}, 
    {"role": "user", "content": "如果我想读取telco_data数据集，应该如何操作呢？"}
]
```

```python
response = client.chat.completions.create(
    model="glm-4",  
    messages=messages
)
display(Markdown(response.choices[0].message.content))
```

telco_data 数据集是一个关于电信公司用户信息的数据库，它专门用于分析和预测用户流失情况。该数据集包含了用户的各种属性信息，这些信息可以帮助分析人员理解用户的留存或流失行为。

以下是 telco_data 数据集的简单介绍：

- **数据集目的**：数据集主要用于分析电信用户的流失情况，即用户停止使用该公司服务的原因。
- **数据存储**：数据以 CSV 格式存储在本地文件夹中，文件名为 `telco_data.csv`。
- **字段说明**：数据集包含多个字段，每个字段代表用户的某一特征或服务使用情况，如下：

  - `customerID`：客户的唯一标识。
  - `gender`：用户的性别。
  - `SeniorCitizen`：用户是否为老年人。
  - `Partner`：用户是否有伴侣。
  - `Dependents`：用户是否有经济依赖。
  - `PhoneService`：用户是否订购了电话服务。
  - `MultipleLines`：用户是否订购了多条电话线。
  - `InternetService`：用户选择的互联网服务类型。
  - `OnlineSecurity`、`OnlineBackup`、`DeviceProtection`、`TechSupport`、`StreamingTV`：用户是否订购了相应的附加服务。
  - `tenure`：用户作为公司客户的时间长度。
  - `Contract`：用户选择的合同类型。
  - `PaperlessBilling`：用户是否选择了无纸化账单。
  - `PaymentMethod`：用户的支付方式。
  - `MonthlyCharges`：用户的月费用。
  - `TotalCharges`：用户累积的总费用。
  - `Churn`：用户是否已经流失。

通过分析这些字段，数据科学家和分析师可以建立模型来预测哪些用户可能流失，并制定相应的策略来改善客户留存率。这个数据集对于电信行业来说非常有价值，因为它可以帮助公司了解客户的需求，改进服务和产品，最终提升企业的竞争力。

## 外部函数 python_inter

```python
def python_inter(py_code, g='globals()'):
    """
    专门用于执行非绘图类python代码，并获取最终查询或处理结果。若是设计绘图操作的Python代码，则需要调用fig_inter函数来执行。
    :param py_code: 字符串形式的Python代码，用于执行对telco_db数据库中各张数据表进行操作
    :param g: g，字符串形式变量，表示环境变量，无需设置，保持默认参数即可
    :return：代码运行的最终结果
    """    
    
    global_vars_before = set(g.keys())
    try:
        exec(py_code, g)            
    except Exception as e:
        return f"代码执行时报错{e}"
    global_vars_after = set(g.keys())
    new_vars = global_vars_after - global_vars_before
    # 若存在新变量
    if new_vars:
        result = {var: g[var] for var in new_vars}
        return str(result)
    # 若不存在新变量，即有可能是代码是表达式，也有可能代码对相同变量重复赋值
    else:
        try:
            # 尝试如果是表达式，则返回表达式运行结果
            return str(eval(py_code, g))
        # 若报错，则先测试是否是对相同变量重复赋值
        except Exception as e:
            try:
                exec(py_code, g)
                return "已经顺利执行代码"
            except Exception as e:
                pass
            # 若不是重复赋值，则报错
            return f"代码执行时报错{e}"
```

```python
# 表达式语句
2+5
```

```
7
```

```python
# 表达式语句运行测试
code_str1 = '2 + 5'
python_inter(py_code = code_str1, g=globals())
```

```
'7'
```

```python
# 赋值语句运行测试
code_str1 = 'a = 1'
python_inter(py_code = code_str1, g=globals())
```

```
'已经顺利执行代码'
```

```python
a
```

```
1
```

```python
# 重复赋值语句运行测试
code_str1 = 'a = 10'
python_inter(py_code = code_str1, g=globals())
```

```
'已经顺利执行代码'
```

```python
a
```

```
10
```

## 编写函数说明

```python
python_inter_function_info = {
    'name': 'python_inter',
    'description': '专门用于python代码，并获取最终查询或处理结果。',
    'parameters': {
        'type': 'object',
        'properties': {
            'py_code': {
                'type': 'string',
                'description': '用于执行在本地环境运行的python代码'
            },
            'g': {
                'type': 'string',
                'description': '环境变量，可选参数，保持默认参数即可'
            }
        },
        'required': ['py_code']
    }
}
```

```python
available_functions = {
    "python_inter": python_inter,
}
```

```python
tools = [
    {
        "type": "function",
        "function": python_inter_function_info
    }
]
```

```python
messages = [
    {"role": "system", "content": data_dictionary}, 
    {"role": "user", "content": "请帮我将telco_data数据集读取到本地Python环境中"}
]
```

```python
response = client.chat.completions.create(
    model="glm-4",  
    messages=messages,
    tools=tools,
)
```

```python
response_message = response.choices[0].message
response_message
```

```
CompletionMessage(content=None, role='assistant', tool_calls=[CompletionMessageToolCall(id='call_8352717810239625769', function=Function(arguments='{"py_code":"import pandas as pd\\n\\ntelco_data = pd.read_csv(\"telco_data.csv\")\\n\\nprint(telco_data.head())"}', name='python_inter', type='function'))])
```

```python
response_message.tool_calls[0]
```

```
CompletionMessageToolCall(id='call_8352717810239625769', function=Function(arguments='{"py_code":"import pandas as pd\\n\\ntelco_data = pd.read_csv(\"telco_data.csv\")\\n\\nprint(telco_data.head())"}', name='python_inter', type='function')
```

```python
response_message.tool_calls[0].id
```

```
'call_8352717810239625769'
```

```python
response_message.tool_calls[0].function
```

```
Function(arguments='{"py_code":"import pandas as pd\\n\\ntelco_data = pd.read_csv(\"telco_data.csv\")\\n\\nprint(telco_data.head())"}', name='python_inter')
```

```python
# 完成对话需要调用的函数名称
function_name = response_message.tool_calls[0].function.name
function_name
```

```
'python_inter'
```

```python
# 具体的函数对象
fuction_to_call = available_functions[function_name]
fuction_to_call
```

```
<function __main__.python_inter(py_code, g='globals()')>
```

```python
response_message.tool_calls[0].function.arguments
```

```
'{"py_code":"import pandas as pd\\n\\ntelco_data = pd.read_csv(\"telco_data.csv\")\\n\\nprint(telco_data.head())"}'
```

```python
# 执行该函数所需要的参数
function_args = json.loads(response_message.tool_calls[0].function.arguments)
function_args
```

```
{'py_code': 'import pandas as pd\n\ntelco_data = pd.read_csv("telco_data.csv")\n\nprint(telco_data.head())'}
```

```python
# 将当前操作空间中的全局变量添加到外部函数中
function_args['g'] = globals() 
# 将函数参数输入到函数中，获取函数计算结果
function_response = fuction_to_call(**function_args)
```
