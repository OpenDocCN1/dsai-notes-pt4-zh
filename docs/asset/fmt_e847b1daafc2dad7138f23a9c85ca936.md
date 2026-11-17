
## FewShotPromptTemplate 基础用法

### 创建 FewShotPromptTemplate 对象
+   创建一个`FewShotPromptTemplate`对象：使用`FewShotPromptTemplate`类创建了一个对象`prompt`。这个对象使用了前面定义的`samples`和`prompt_sample`，并定义了一个新的占位符字符串`suffix`，以及输入变量`flower_type`和`occasion`。

### 将提示传递给大模型
+   将提示传递给大模型：首先，从环境变量中获取`OpenAI` API Key（这行代码被注释掉了，你需要用你自己的 API Key 替换掉`'你的OpenAI API Key'`）。然后，使用`OpenAI`类创建了一个模型对象`model`。最后，将`prompt`对象格式化后的字符串传递给`model`，并打印出模型的输出结果。

## 使用示例选择器优化成本

当示例数量过多时，一次性将所有示例发送给模型是不现实且低效的。同时，每次包含过多的 Token 也会浪费流量，因为 `OpenAI` 是按照 Token 数量来收取费用。

`LangChain` 提供了示例选择器，可以选择最合适的样本。这需要安装向量数据库，如本例中的`Qdrant`。

### 安装包

```
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple qdrant-client==1.7.0
```

### 启动 Qdrant 数据库

需要使用`Docker`启动一个`Qdrant`数据库

> `Qdrant`（读作：quadrant）是一个向量相似性搜索引擎。它提供了一个生产就绪的服务，带有一个方便的 API 来存储、搜索和管理点——带有额外负载的向量。`Qdrant`被定制为支持扩展过滤。它使得它对所有类型的神经网络或基于语义的匹配、分面搜索和其他应用程序都有用。

```
docker run -p 6333:6333 qdrant/qdrant
```

### 使用示例选择器

```python
# 5. 使用示例选择器
from langchain.prompts.example_selector import SemanticSimilarityExampleSelector
from langchain.vectorstores import Qdrant
from langchain.embeddings import OpenAIEmbeddings

# 初始化示例选择器
example_selector = SemanticSimilarityExampleSelector.from_examples(
    samples,
    OpenAIEmbeddings(),
    Qdrant,
    k=1
)

# 创建一个使用示例选择器的 FewShotPromptTemplate 对象
prompt = FewShotPromptTemplate(
    example_selector=example_selector, 
    example_prompt=prompt_sample, 
    suffix="鲜花类型: {flower_type}\n场合: {occasion}", 
    input_variables=["flower_type", "occasion"]
)
print(prompt.format(flower_type="红玫瑰", occasion="爱情"))
```

> 这段 Python 代码使用了`langchain`库来生成一个关于红玫瑰和爱情场合的广告文案。以下是每一部分的详细解释：

+   **导入所需的模块和类**：从`langchain.prompts.example_selector`导入`SemanticSimilarityExampleSelector`，从`langchain.vectorstores`导入`Qdrant`，从`langchain.embeddings`导入`OpenAIEmbeddings`。

+   **初始化示例选择器**：使用`SemanticSimilarityExampleSelector.from_examples()`方法创建一个示例选择器`example_selector`。这个方法需要四个参数：示例（`samples`）、嵌入（`OpenAIEmbeddings()`）、向量存储（`Qdrant`）和最近邻数量（`k=1`）。

+   创建一个使用示例选择器的`FewShotPromptTemplate`对象：使用`FewShotPromptTemplate`类创建一个对象`prompt`。这个对象使用了前面定义的示例选择器`example_selector`和示例提示`prompt_sample`，并定义了一个新的占位符字符串`suffix`，以及输入变量`flower_type`和`occasion`。

+   **格式化提示并打印**：使用`prompt`对象的`format()`方法，将输入变量（`flower_type`和`occasion`）填充到模板中，生成最终的提示，并打印出来。

## CoT

```python
# 设置环境变量和 API 密钥
import os
# API_SECRET_KEY = os.getenv("OPENAI_API_KEY")
# BASE_URL = os.getenv("OPENAI_API_BASE")

# 创建聊天模型
from langchain.chat_models import ChatOpenAI
llm = ChatOpenAI(temperature=0)

# 设定 AI 的角色和目标
role_template = "你是一个为花店电商公司工作的 AI 助手, 你的目标是帮助客户根据他们的喜好做出明智的决定"

# CoT 的关键部分，AI 解释推理过程，并加入一些先前的对话示例（Few-Shot Learning）
cot_template = """
作为一个为花店电商公司工作的 AI 助手，我的目标是帮助客户根据他们的喜好做出明智的决定。 

我会按部就班的思考，先理解客户的需求，然后考虑各种鲜花的涵义，最后根据这个需求，给出我的推荐。
同时，我也会向客户解释我这样推荐的原因。

示例 1:
  人类：我想找一种象征爱情的花。
  AI：首先，我理解你正在寻找一种可以象征爱情的花。在许多文化中，红玫瑰被视为爱情的象征，这是因为它们的红色通常与热情和浓烈的感情联系在一起。因此，考虑到这一点，我会推荐红玫瑰。红玫瑰不仅能够象征爱情，同时也可以传达出强烈的感情，这是你在寻找的。

示例 2:
  人类：我想要一些独特和奇特的花。
  AI：从你的需求中，我理解你想要的是独一无二和引人注目的花朵。兰花是一种非常独特并且颜色鲜艳的花，它们在世界上的许多地方都被视为奢侈品和美的象征。因此，我建议你考虑兰花。选择兰花可以满足你对独特和奇特的要求，而且，兰花的美丽和它们所代表的力量和奢侈也可能会吸引你。
"""
from langchain.prompts import ChatPromptTemplate, HumanMessagePromptTemplate, SystemMessagePromptTemplate
system_prompt_role = SystemMessagePromptTemplate.from_template(role_template)
system_prompt_cot = SystemMessagePromptTemplate.from_template(cot_template)

# 用户的询问
human_template = "{human_input}"
human_prompt = HumanMessagePromptTemplate.from_template(human_template)

# 将以上所有信息结合为一个聊天提示
chat_prompt = ChatPromptTemplate.from_messages([system_prompt_role, system_prompt_cot, human_prompt])

prompt = chat_prompt.format_prompt(human_input="我想为我的女朋友购买一些花。她喜欢粉色和紫色。你有什么建议吗?").to_messages()

# 接收用户的询问，返回回答结果
response = llm(prompt)
print(response)
```

> 这段代码的整个流程是：先设定 AI 的角色和目标，然后解释 AI 的推理过程，接着接收用户的询问，最后返回符合逻辑的答案。这是一个典型的 AI 对话模型的工作流程。

+   **设置环境变量和 API 密钥**：这里首先设置你的 `OpenAI` API 密钥，这个密钥是用来访问 `OpenAI` 的 API 的。这是使用 `OpenAI` 的前提条件。

+   **创建聊天模型**：使用 `ChatOpenAI` 类创建一个聊天模型，`temperature` 参数设置为 `0`，表示生成的文本将更加确定和一致。这有助于得到更准确的答案。

+   **设定 AI 的角色和目标**：在 `role_template` 中，设定了 AI 的角色和目标，即作为一个为花店电商公司工作的 AI 助手，目标是帮助客户根据他们的喜好做出明智的决定。这有助于指导 AI 的行为。

+   **CoT 的关键部分**：在 `cot_template` 中，AI 解释了它的推理过程，并加入了一些先前的对话示例。这是 Few-Shot Learning 的一个示例，可以帮助 AI 更好地理解任务。

+   **用户的询问**：`human_template` 是用户询问的模板，`human_input` 是用户将在运行时提供的实际询问。这是输入到模型的实际问题。

+   将以上所有信息结合为一个聊天提示：这里将所有的提示信息结合到一起，形成一个完整的聊天提示。这是输入到模型的完整信息。

+   **接收用户的询问，返回回答结果**：最后，将聊天提示输入到模型中，模型会返回一个答案，并打印出来。这是模型的输出结果。

# 输出解析

## Pydantic（JSON）解析器

### 安装包

```
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple pydantic==1.10.12
```

```python
# 设置 OpenAI API 密钥
import os
# os.environ["OPENAI_API_KEY"] = 'Your OpenAI API Key'

# 创建模型实例
from langchain import OpenAI
# from langchain.chat_models import ChatOpenAI
model = OpenAI(model_name='gpt-3.5-turbo-instruct')
# model = ChatOpenAI(model_name='gpt-4')

# ------Part 2
# 创建一个空的 DataFrame 用于存储结果
import pandas as pd
df = pd.DataFrame(columns=["flower_type", "price", "description", "reason"])

# 数据准备
flowers = ["玫瑰", "百合", "康乃馨"]
prices = ["50", "30", "20"]

# 定义我们想要接收的数据格式
from pydantic import BaseModel, Field
class FlowerDescription(BaseModel):
    flower_type: str = Field(description="鲜花的种类")
    price: int = Field(description="鲜花的价格")
    description: str = Field(description="鲜花的描述文案")
    reason: str = Field(description="为什么要这样写这个文案")

# ------Part 3
# 创建输出解析器
from langchain.output_parsers import PydanticOutputParser
output_parser = PydanticOutputParser(pydantic_object=FlowerDescription)

# 获取输出格式指示
format_instructions = output_parser.get_format_instructions()
# 打印提示
print("输出格式：", format_instructions)


# ------Part 4
# 创建提示模板
from langchain import PromptTemplate
prompt_template = """您是一位专业的鲜花店文案撰写员。
对于售价为 {price} 元的 {flower} ，您能提供一个吸引人的简短中文描述吗？
{format_instructions}"""

# 根据模板创建提示，同时在提示中加入输出解析器的说明
prompt = PromptTemplate.from_template(prompt_template, 
       partial_variables={"format_instructions": format_instructions}) 

# 打印提示
print("提示：", prompt)

# ------Part 5
for flower, price in zip(flowers, prices):
    # 根据提示准备模型的输入
    input = prompt.format(flower=flower, price=price)
    # 打印提示
    print("提示：", input)

    # 获取模型的输出
    output = model(input)

    # 解析模型的输出
    parsed_output = output_parser.parse(output)
    parsed_output_dict = parsed_output.dict()  # 将 Pydantic 格式转换为字典

    # 将解析后的输出添加到 DataFrame 中
    df.loc[len(df)] = parsed_output.dict()

# 打印字典
print("输出的数据：", df.to_dict(orient='records'))
```

> 这段代码的主要目标是使用 `OpenAI` 的模型生成一些描述鲜花的文案，并将结果存储在一个 `DataFrame` 中。以下是每个部分的详细解释：

+   **设置 `OpenAI` API 密钥**：这部分代码设置了 `OpenAI` API 的密钥，这是使用 `OpenAI` 服务的凭证。

+   **创建模型实例**：这部分代码创建了一个 `OpenAI` 模型的实例，模型名称为 `'gpt-3.5-turbo-instruct'`。

+   **数据准备**：这部分代码创建了一个空的 `DataFrame`，用于存储结果。然后，定义了一些鲜花的种类和价格。接着，定义了一个 `Pydantic` 模型，用于描述期望的输出格式。

+   **创建输出解析器**：这部分代码创建了一个输出解析器，用于将模型的输出解析为 `FlowerDescription` 对象。

+   **创建提示模板**：这部分代码创建了一个提示模板，用于生成模型的输入。提示模板中包含了输出格式的说明。

+   **生成文案并存储结果**：这部分代码遍历了所有的鲜花和价格，对于每一种鲜花，都生成了一个提示，然后将提示作为输入传给模型。模型的输出被解析为 `FlowerDescription` 对象，然后将解析后的结果添加到 `DataFrame` 中。

> 最后，打印了 `DataFrame` 中的所有数据。

> 注意：受限于模型的输出质量，这段代码执行成功的机率可能没有预期的高，可以考虑使用质量更好（更贵）的模型或者对提示词进行进一步优化。

正常结果如下：

```
输出格式： The output should be formatted as a JSON instance that conforms to the JSON schema below.

As an example, for the schema {"properties": {"foo": {"title": "Foo", "description": "a list of strings", "type": "array", "items": {"type": "string"}}}, "required": ["foo"]}
the object {"foo": ["bar", "baz"]} is a well-formatted instance of the schema. The object {"properties": {"foo": ["bar", "baz"]}} is not well-formatted.

Here is the output schema:
```
