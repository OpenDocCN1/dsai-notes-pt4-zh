# 基于LLM的信息抽取快速上手实践

> 来源：[https://agijuejin.feishu.cn/docx/EckOdSIFno1PlYxtHMNcMbEAnDc](https://agijuejin.feishu.cn/docx/EckOdSIFno1PlYxtHMNcMbEAnDc)

> 作者：lucas大叔

> 原文： [https://zhuanlan.z](https://zhuanlan.zhihu.com/p/686632550)hihu.com/p/686632...

基于LLM实现[信息抽取的核心](https://www.zhihu.com/search?q=prompt&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A686632550%7D)思想是定义抽取信息的schema，让LLM基于schema从文本中抽取相关信息。对于有tool/function calling能力的LLM，可以利用tool/function calling从文本中抽取信息。langchain官方列出支持的模型有：OpenAI、Fireworks、Mistral、Together。当然，中文大模型也有一些是支持function calling的，例如chatglm3-6b。但还是有很多开源大模型是不支持tool/function calling能力的，因此本文探索基于 prompt 的方法做信息提取，不要求模型具有tool/function calling能力。

---

首先，导入必要的packages

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

加载LLM

```
model_path = &quot;/data/mo[dels/Baichu](https://www.zhihu.com/search?q=torch_dtype&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A686632550%7D)an2-13B-Chat&quot;
# ---------------------------- 加载LLM ----------------------------------------- #
tokenizer = AutoTokenizer.from_pretrained(model_path,
                                          device_map=&quot;auto&quot;,
                                          trust_remote_code=True,
                                          torch_dtype=torch.float16)

model = AutoModelForCausalLM.from_pretrained(
    model_path,
    torch_dtype=torch.float16,
    trust_remote_code=True,
    device_map=&quot;auto&quot;,
)

pipeline = pipeline(
    &quot;text-generation&quot;,
    model=model,
    tokenizer=tokenizer,
    return_full_text=True,
)

llm = HuggingFacePipeline(pipeline=pipeline)
```

[定义需要提取信息](https://www.zhihu.com/search?q=Person%E7%B1%BB&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A686632550%7D)的schema，这里以提取人物信息为例。创建 Person类 ，描述想从文本中提取哪些信息。

1.  schema本身和属性的描述至关重要，这些信息发送给LLM用于改善信息抽取质量

1.  对属性使用Optional，允许LLM在不知道答案的时候返回None，不胡编乱造

在大多数情况下，需要提取实体列表，而不是单个实体。用pydantic将模型嵌套在彼此内部就可以轻松实现，这里创建People类实现Person类的嵌套。

```
# ---------------------------- 定义需要提取信息的schema ---------------------------- #
class Person(BaseModel):
    &quot;&quot;&quot;Information about a person.&quot;&quot;&quot;
    name: Optional[str] = Field(..., description=&quot;人物的名字&quot;)
    birth_date: Optional[str] = Field(..., description=&quot;出生日期&quot;)
    birth_place: Optional[str] = Field(..., description=&quot;出生地点&quot;)
    spouse: Optional[str] = Field(..., description=&quot;配偶，妻子或丈夫&quot;)
    university: Optional[str] = Field(..., description=&quot;毕业的学校&quot;)


class People(BaseModel):
    &quot;&quot;&quot;Identifying information about all people in a text.&quot;&quot;&quot;
    people: List[Person]
```

LLM返回的是字符串， [自定义解析函数 ](https://www.zhihu.com/search?q=%E8%87%AA%E5%AE%9A%E4%B9%89%E8%A7%A3%E6%9E%90%E5%87%BD%E6%95%B0&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A686632550%7D)将schema信息解析出来。由于模型返回内容格式的差异，实际使用时需要调整代码。

```
# --[------------](https://www.zhihu.com/search?q=extract_json&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A686632550%7D)-------------- 自定义解析函数 ------------------------------------ #
def extract_json(message: str) -&gt; List[dict]:
    pattern = r&quot;{([^{}]+)}&quot;
    matches = re.findall(pattern, message, re.DOTALL)

    try:
        return [json.loads(&quot;{&quot; + match.strip() + &quot;}&quot;) for match in matches]
    except Exception:
        raise ValueError(f&quot;Failed to parse: {message}&quot;)
```

构造提示模板和chain

```
# ---------------------------- 构造提示模板和chain ------------------------------ #
template = &#x27;&#x27;&#x27;
你是信息抽取专家，从文本中只提取出schema中的相关信息。
结果按照schema的json格式输出。

schema：
{schema}

文本：
{text}
&#x27;&#x27;&#x27;
prompt = PromptTemplate(template=template, input_variables=[&quot;text&quot;], partial_variables={&quot;schema&quot;: People.schema()})

chain = (
        {&quot;text&quot;: RunnablePassthrough()}
        | prompt
        | llm
        | extract_json
)
```

在 [百度百科 ](https://www.zhihu.com/search?q=%E7%99%BE%E5%BA%A6%E7%99%BE%E7%A7%91&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A686632550%7D)上分别找了刘德华、郭德纲和沈腾的一句话介绍作为测试文本。

```
[# --](https://www.zhihu.com/search?q=%E5%AE%9E%E4%BD%93%E6%96%87%E6%9C%AC&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A686632550%7D)-[-----](https://www.zhihu.com/search?q=%E7%94%B5%E5%BD%B1%E5%88%B6%E7%89%87%E4%BA%BA&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A686632550%7D)-[---](https://www.zhihu.com/search?q=%E4%BD%9C%E8%AF%8D%E4%BA%BA&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A686632550%7D)---------------- 多实体文本测试 --------------------------------- #
text = &quot;刘德华（Andy Lau），1961年9月27日出生于中国香港，华语影视男演员、流行乐歌手、电影制片人、作词人。&quot; \
       &quot;郭德纲，1973年1月18日出生于中国天津市，祖籍中国山西省，中国内地相声演员、导演、编剧、歌手、演员、主持人、北京德云社创始人。&quot; \
       &quot;沈腾，1979年10月23日出生于黑龙江省齐齐哈尔市，毕业于解放军艺术学院。&quot;

result = chain.invoke(text)
```

从测试结果可以看出，对文本中没有提及的属性返回为空，已提及的内容基本上都可以正确抽取。

```
[{&#x27;name&#x27;: &#x27;刘德华&#x27;, &#x27;birth_date&#x27;: &#x27;1961-09-27&#x27;, &#x27;birth_place&#x27;: &#x27;中国香港&#x27;, &#x27;spouse&#x27;: &#x27;&#x27;, &#x27;university&#x27;: &#x27;&#x27;}, {&#x27;name&#x27;: &#x27;郭德纲&#x27;, &#x27;birth_date
&#x27;: &#x27;1973-01-18&#x27;, &#x27;birth_place&#x27;: &#x27;中国天津市&#x27;, &#x27;spouse&#x27;: &#x27;&#x27;, &#x27;university&#x27;: &#x27;&#x27;}, {&#x27;name&#x27;: &#x27;沈腾&#x27;, &#x27;birth_date&#x27;: &#x27;1979-10-23&#x27;, &#x27;birth_place&#x27;:
 &#x27;黑龙江省齐齐哈尔市&#x27;, &#x27;spouse&#x27;: &#x27;&#x27;, &#x27;university&#x27;: &#x27;解放军艺术学院&#x27;}]
```

参考资料

Quickstart | ️ Langchain

Parsing | ️ Langchain