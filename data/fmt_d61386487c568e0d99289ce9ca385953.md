
# 基于LLM的信息抽取快速上手实践

> 来源：[https://agijuejin.feishu.cn/docx/EckOdSIFno1PlYxtHMNcMbEAnDc](https://agijuejin.feishu.cn/docx/EckOdSIFno1PlYxtHMNcMbEAnDc)
> 作者：lucas大叔
> 原文：[https://zhuanlan.zhihu.com/p/686632550](https://zhuanlan.zhihu.com/p/686632550)

基于LLM实现信息抽取的核心思想是定义抽取信息的`schema`，让LLM基于`schema`从文本中抽取相关信息。对于有`tool/function calling`能力的LLM，可以利用`tool/function calling`从文本中抽取信息。`langchain`官方列出支持的模型有：OpenAI、Fireworks、Mistral、Together。当然，中文大模型也有一些是支持`function calling`的，例如`chatglm3-6b`。但还是有很多开源大模型是不支持`tool/function calling`能力的，因此本文探索基于`prompt`的方法做信息提取，不要求模型具有`tool/function calling`能力。

## 导入必要的包

首先，导入必要的`packages`：

```
import re
import json
import torch
from typing import Optional, List
from langchain_core.pydantic_v1 import BaseModel, Field
from transformers import AutoModelForCausalLM, AutoTokenizer, pipeline
from langchain import HuggingFacePipeline
from langchain.schema.runnable import RunnablePassthrough
from langchain import PromptTemplate
```

## 加载LLM

加载LLM：

```
model_path = "/data/models/Baichuan2-13B-Chat"
# ---------------------------- 加载LLM ----------------------------------------- #
tokenizer = AutoTokenizer.from_pretrained(model_path,
                                          device_map="auto",
                                          trust_remote_code=True,
                                          torch_dtype=torch.float16)

model = AutoModelForCausalLM.from_pretrained(
    model_path,
    torch_dtype=torch.float16,
    trust_remote_code=True,
    device_map="auto",
)

pipeline = pipeline(
    "text-generation",
    model=model,
    tokenizer=tokenizer,
    return_full_text=True,
)

llm = HuggingFacePipeline(pipeline=pipeline)
```

## 定义信息抽取的schema

定义需要提取信息的`schema`，这里以提取人物信息为例。创建 `Person` 类，描述想从文本中提取哪些信息。

1. `schema`本身和属性的描述至关重要，这些信息发送给LLM用于改善信息抽取质量
2. 对属性使用`Optional`，允许LLM在不知道答案的时候返回`None`，不胡编乱造

在大多数情况下，需要提取实体列表，而不是单个实体。用`pydantic`将模型嵌套在彼此内部就可以轻松实现，这里创建`People`类实现`Person`类的嵌套。

```
# ---------------------------- 定义需要提取信息的schema ---------------------------- #
class Person(BaseModel):
    """Information about a person."""
    name: Optional[str] = Field(..., description="人物的名字")
    birth_date: Optional[str] = Field(..., description="出生日期")
    birth_place: Optional[str] = Field(..., description="出生地点")
    spouse: Optional[str] = Field(..., description="配偶，妻子或丈夫")
    university: Optional[str] = Field(..., description="毕业的学校")


class People(BaseModel):
    """Identifying information about all people in a text."""
    people: List[Person]
```

## 自定义解析函数

LLM返回的是字符串，自定义解析函数将schema信息解析出来。由于模型返回内容格式的差异，实际使用时需要调整代码。

```
# ---------------------------- 自定义解析函数 ------------------------------------ #
def extract_json(message: str) -> List[dict]:
    pattern = r"{([^{}]+)}"
    matches = re.findall(pattern, message, re.DOTALL)

    try:
        return [json.loads("{" + match.strip() + "}") for match in matches]
    except Exception:
        raise ValueError(f"Failed to parse: {message}")
```

## 构造提示模板和chain

构造提示模板和chain：

```
# ---------------------------- 构造提示模板和chain ------------------------------ #
template = '''
你是信息抽取专家，从文本中只提取出schema中的相关信息。
结果按照schema的json格式输出。

schema：
{schema}

文本：
{text}
'''
prompt = PromptTemplate(template=template, input_variables=["text"], partial_variables={"schema": People.schema()})

chain = (
        {"text": RunnablePassthrough()}
        | prompt
        | llm
        | extract_json
)
```

## 测试与结果

在百度百科上分别找了刘德华、郭德纲和沈腾的一句话介绍作为测试文本。

```
# ----------------- 多实体文本测试 ----------------
text = "刘德华（Andy Lau），1961年9月27日出生于中国香港，华语影视男演员、流行乐歌手、电影制片人、作词人。" \
       "郭德纲，1973年1月18日出生于中国天津市，祖籍中国山西省，中国内地相声演员、导演、编剧、歌手、演员、主持人、北京德云社创始人。" \
       "沈腾，1979年10月23日出生于黑龙江省齐齐哈尔市，毕业于解放军艺术学院。"

result = chain.invoke(text)
```

从测试结果可以看出，对文本中没有提及的属性返回为空，已提及的内容基本上都可以正确抽取。

```
[{'name': '刘德华', 'birth_date': '1961-09-27', 'birth_place': '中国香港', 'spouse': '', 'university': ''}, {'name': '郭德纲', 'birth_date': '1973-01-18', 'birth_place': '中国天津市', 'spouse': '', 'university': ''}, {'name': '沈腾', 'birth_date': '1979-10-23', 'birth_place': '黑龙江省齐齐哈尔市', 'spouse': '', 'university': '解放军艺术学院'}]
```

## 参考资料

- Quickstart | ️ Langchain
- Parsing | ️ Langchain
