
# Llama 3.1 原生 Function Calling 深度体验（LMDeploy 版）

> 来源：[https://aicarrier.feishu.cn/docx/BFT5d32ugolJ0Sx7TaAcqaXlnDc](https://aicarrier.feishu.cn/docx/BFT5d32ugolJ0Sx7TaAcqaXlnDc)

Meta 发布了 `Llama 3.1` 第一次支持了原生 `Function Calling` 功能，本文将带大家使用 `LMDeploy` 深度体验 `Llama 3.1` 的 `Function Calling` 功能。

## InternLM2.5 Function Calling

![](img/fe73dd91d7b4dc4775bd7003d470f0c0.png)

```python
from openai import OpenAI
import math

import addict
from func_timeout import func_timeout

def _safe_eval(expr):
    math_methods = {k: v for k, v in math.__dict__.items()
                    if not k.startswith('_')}
    allowed_methods = {
        'math': addict.Addict(math_methods),
        'max': max,
        'min': min,
        'round': round,
        'sum': sum,
        **math_methods,
    }
    allowed_methods['__builtins__'] = None
    return eval(expr, allowed_methods, allowed_methods)

def calculator(expression: str):
    res = func_timeout(timeout=5, func=_safe_eval, args=[expression])
    return f'The result of {expression} is {res}'

tools = [
    {
        "type": "function",
        "function": {
            "name": "calculator",
            "description": ('A calculator tool. The input must be a single Python '
                            'expression and you cannot import packages. You can use functions '
                            'in the `math` package without import.'),
            "parameters": {
                "type": "object",
                "properties": {
                    "expression": {
                        "type": "string",
                        "description": "The expression for calculator",
                    },
                },
                "required": ["expression"],
            },
        }
    }
]

client = OpenAI(api_key='YOUR_API_KEY', base_url='http://0.0.0.0:23333/v1')

model_name = client.models.list().data[0].id

messages = [{'role': 'user', 'content': '使用计算器，13.8 和 13.11 谁更大？'}]

response = client.chat.completions.create(
    model=model_name,
    messages=messages,
    temperature=0.8,
    top_p=0.8,
    stream=False,
    tools=tools)

print(response)
func1_name = response.choices[0].message.tool_calls[0].function.name
func1_args = response.choices[0].message.tool_calls[0].function.arguments
func1_out = eval(f'{func1_name}(**{func1_args})')
print(func1_out)

messages.append(
    {'role': 'assistant', 'content': response.choices[0].message.content})
messages.append(
    {'role': 'environment', 'content': func1_out, 'name': 'plugin'})

response = client.chat.completions.create(
    model=model_name,
    messages=messages,
    temperature=0.8,
    top_p=0.8,
    stream=False,
    tools=tools)
print(response)

messages.append(
    {'role': 'assistant', 'content': response.choices[0].message.content})

for message in messages:
    print(f'{message["role"]}: {message["content"]}\n')
```
