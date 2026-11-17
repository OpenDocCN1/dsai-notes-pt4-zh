
# 输出数据示例

```
输出的数据： [{'flower_type': '玫瑰', 'price': 50, 'description': '这束玫瑰花色鲜艳，花朵饱满，香气浓郁，是表达爱意、祝福和感激的最佳选择。无论是情人节、生日、结婚纪念日还是母亲节，都能收到意想不到的惊喜。赶快为你心爱的人订购吧！', 'reason': '玫瑰是世界上最受欢迎的花之一，它象征着爱情、热情和浪漫。我们专业的鲜花店为您精心挑选和包装，让您的心意通过这束玫瑰表达得更加完美。'}, {'flower_type': '百合', 'price': 30, 'description': '百合，一种清新淡雅的花卉，花瓣洁白如雪，散发出淡淡的芳香，是表达真挚感情的最佳选择。它的花朵寓意纯洁、高贵和祝福，可以用来送给自己的爱人或是亲朋好友，让他们感受到你的关爱和祝福。价格实惠，是您的最佳选择。', 'reason': '百合作为一种代表纯洁和高贵的花卉，具有独特的美感和芬芳，能够让人感受到浪漫和温馨的氛围，是您最好的选择。'}, {'flower_type': '康乃馨', 'price': 20, 'description': '这束20元的康乃馨，色彩鲜艳，花朵饱满，花语为爱情和美好，是送给爱人、朋友或家人的完美礼物。它们能够表达你对对方的爱意和祝福，让收到的人感受到温暖和幸福。不论是生日、纪念日、情人节还是母亲节，都是送康乃馨的最佳时机。让这束康乃馨为你的重要日子增添浪漫和温馨。', 'reason': '康乃馨是一种典雅、浪漫的鲜花，能够表达爱意和美好祝愿，是送礼的最佳选择。'}]
```

## 使用 `bejson.com` 调整格式

使用 `bejson.com` 调整格式之后

```
[{
    'flower_type': '玫瑰',
    'price': 50,
    'description': '这束玫瑰花色鲜艳，花朵饱满，香气浓郁，是表达爱意、祝福和感激的最佳选择。无论是情人节、生日、结婚纪念日还是母亲节，都能收到意想不到的惊喜。赶快为你心爱的人订购吧！',
    'reason': '玫瑰是世界上最受欢迎的花之一，它象征着爱情、热情和浪漫。我们专业的鲜花店为您精心挑选和包装，让您的心意通过这束玫瑰表达得更加完美。'
}, {
    'flower_type': '百合',
    'price': 30,
    'description': '百合，一种清新淡雅的花卉，花瓣洁白如雪，散发出淡淡的芳香，是表达真挚感情的最佳选择。它的花朵寓意纯洁、高贵和祝福，可以用来送给自己的爱人或是亲朋好友，让他们感受到你的关爱和祝福。价格实惠，是您的最佳选择。',
    'reason': '百合作为一种代表纯洁和高贵的花卉，具有独特的美感和芬芳，能够让人感受到浪漫和温馨的氛围，是您最好的选择。'
}, {
    'flower_type': '康乃馨',
    'price': 20,
    'description': '这束20元的康乃馨，色彩鲜艳，花朵饱满，花语为爱情和美好，是送给爱人、朋友或家人的完美礼物。它们能够表达你对对方的爱意和祝福，让收到的人感受到温暖和幸福。不论是生日、纪念日、情人节还是母亲节，都是送康乃馨的最佳时机。让这束康乃馨为你的重要日子增添浪漫和温馨。',
    'reason': '康乃馨是一种典雅、浪漫的鲜花，能够表达爱意和美好祝愿，是送礼的最佳选择。'
}]
```

# 自动修复解析器（`OutputFixingParser`）

## 模拟报错

```
# 导入所需要的库和模块

from langchain.output_parsers import PydanticOutputParser

from pydantic import BaseModel, Field

from typing import List

# 使用Pydantic创建一个数据格式，表示花

class Flower(BaseModel):

    name: str = Field(description="name of a flower")

    colors: List[str] = Field(description="the colors of this flower")

# 定义一个用于获取某种花的颜色列表的查询

flower_query = "Generate the charaters for a random flower."

# 定义一个格式不正确的输出

misformatted = "{'name': '康乃馨', 'colors': ['粉红色','白色','红色','紫色','黄色']}"

# 创建一个用于解析输出的Pydantic解析器，此处希望解析为Flower格式

parser = PydanticOutputParser(pydantic_object=Flower)

# 使用Pydantic解析器解析不正确的输出

parser.parse(misformatted)
```

## 加特效

```
# 从langchain库导入所需的模块

from langchain.chat_models import ChatOpenAI

from langchain.output_parsers import OutputFixingParser

# 设置OpenAI API密钥

import os

# 使用OutputFixingParser创建一个新的解析器，该解析器能够纠正格式不正确的输出

new_parser = OutputFixingParser.from_llm(parser=parser, llm=ChatOpenAI())

# 使用新的解析器解析不正确的输出

result = new_parser.parse(misformatted) # 错误被自动修正

print(result) # 打印解析后的输出结果
```

> 这段代码主要是用来创建一个解析器，它能够处理和修正格式错误的输出。这个解析器的名字叫做 `OutputFixingParser`，它是从 `langchain` 库中导入的。

> 在 `OutputFixingParser` 的内部，它首先尝试使用一个叫做 `PydanticOutputParser` 的解析器来处理输出。如果 `PydanticOutputParser` 可以成功处理输出，那么 `OutputFixingParser` 就会直接返回这个结果。

> 然而，如果 `PydanticOutputParser` 无法处理输出，也就是说输出的格式有错误，那么 `OutputFixingParser` 就会启动备用方案。它会把这个格式错误的输出和一个格式化的指令传递给一个大模型，这个大模型在这段代码中是 `ChatOpenAI`。

> `ChatOpenAI` 是一个能够与OpenAI API进行交互的模块，它会接收到 `OutputFixingParser` 传递过来的格式错误的输出和格式化的指令，然后进行相应的修复操作。

> 所以，这段代码的主要目的是创建一个能够自动修正格式错误的解析器，并使用它来处理可能的格式错误。当它遇到不能处理的格式错误时，它会调用一个更大的模型来进行修复。这样，无论输出的格式是否正确，这个解析器都能够返回一个格式正确的输出。

# 重试解析器（`RetryWithErrorOutputParser`）

```
# 定义一个模板字符串，这个模板将用于生成提问

template = """Based on the user question, provide an Action and Action Input for what step should be taken.

{format_instructions}

Question: {query}

Response:"""

# 定义一个Pydantic数据格式，它描述了一个"行动"类及其属性

from pydantic import BaseModel, Field

class Action(BaseModel):

    action: str = Field(description="action to take")

    action_input: str = Field(description="input to the action")

# 使用Pydantic格式Action来初始化一个输出解析器

from langchain.output_parsers import PydanticOutputParser

parser = PydanticOutputParser(pydantic_object=Action)

# 定义一个提示模板，它将用于向模型提问

from langchain.prompts import PromptTemplate

prompt = PromptTemplate(

    template="Answer the user query.\n{format_instructions}\n{query}\n",

    input_variables=["query"],

    partial_variables={"format_instructions": parser.get_format_instructions()},

)

prompt_value = prompt.format_prompt(query="What are the colors of Orchid?")

# 定义一个错误格式的字符串

bad_response = '{"action": "search"}'

parser.parse(bad_response) # 如果直接解析，它会引发一个错误
```

## 分析错误

```
from langchain.output_parsers import OutputFixingParser

from langchain.chat_models import ChatOpenAI

fix_parser = OutputFixingParser.from_llm(parser=parser, llm=ChatOpenAI())

parse_result = fix_parser.parse(bad_response)

print('OutputFixingParser的parse结果:', parse_result)
```

## 加特效

```
# 初始化RetryWithErrorOutputParser，它会尝试再次提问来得到一个正确的输出

from langchain.output_parsers import RetryWithErrorOutputParser

from langchain.llms import OpenAI

retry_parser = RetryWithErrorOutputParser.from_llm(

    parser=parser, llm=OpenAI(temperature=0)

)

parse_result = retry_parser.parse_with_prompt(bad_response, prompt_value)

print('RetryWithErrorOutputParser的parse结果:', parse_result)
```

以下是一个使用 Mermaid 创建的流程图，详细描述了这段代码的执行流程

```
graph TB

A[创建RetryWithErrorOutputParser对象] --> B[调用parse_with_prompt方法]

B --> C{解析出错?}

C -->|Yes| D[根据原始提示重新提问]

D --> B

C -->|No| E[打印解析结果]
```

![](img/b3a7049aa52f67768b7c8b8302fbfc0d.png)

这个流程图的解释如下：

1. 创建 `RetryWithErrorOutputParser` 对象。
2. 调用 `parse_with_prompt()` 方法尝试解析输出。
3. 判断解析是否出错。
   - 如果解析出错，根据原始提示重新提问，然后回到步骤 2，再次尝试解析。
   - 如果解析没有出错，打印解析结果。

> 更新代码

```
from pydantic import BaseModel, Field

from langchain.output_parsers import PydanticOutputParser, RetryWithErrorOutputParser

from langchain.llms import OpenAI

from langchain.prompts import PromptTemplate

class Action(BaseModel):

    action: str = Field(description="action to take")

    action_input: str = Field(description="input to the action")

parser = PydanticOutputParser(pydantic_object=Action)

prompt = PromptTemplate(

    template="Answer the user query.\n{format_instructions}\n{query}\n",

    input_variables=["query"],

    partial_variables={"format_instructions": parser.get_format_instructions()},

)

prompt_value = prompt.format_prompt(query="What are the colors of Orchid?")

retry_parser = RetryWithErrorOutputParser.from_llm(

    parser=parser, llm=OpenAI(temperature=0)

)

good_response = '{"action": "search", "action_input": "Orchid colors"}'

parse_result = retry_parser.parse_with_prompt(good_response, prompt_value)

print('RetryWithErrorOutputParser的parse结果:', parse_result)
```
