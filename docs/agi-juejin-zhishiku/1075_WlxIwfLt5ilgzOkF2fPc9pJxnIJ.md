# Outlines：让LLM结构化输出可控，提升LLM应用的稳定性

> 来源：[https://agijuejin.feishu.cn/docx/OpYKdOXWHoRRsvxXNEpcnosjnBf](https://agijuejin.feishu.cn/docx/OpYKdOXWHoRRsvxXNEpcnosjnBf)

> 作者：ully | AI工程化

> 原文：https://mp.weixin.qq.com/s/goe7G21ENExIZPWZtPFUyg

在面向LLM进行应用开发时，LLM相较于传统接口服务来讲的一个大的优势，即能够生成更符合人习惯的自然语言输出，但是这对于系统集成来讲却是一个障碍，系统之间的交互通常是结构化的。这就要求我们需要让LLM以某种格式输出，比如json，以便后续处理。通常做法是， 是在 prompt 中提供格式要求（并最好提供示例）来进行约束，但这并非百分之百有效，进而影响应用的稳定性。

```
Provide 3 suggestions for specific places to go to in Seattle on a rainy day. Respond in the form of JSON. The JSON should have the following forma

[
    { &quot;venue&quot;: &quot;...&quot;, &quot;description&quot;: &quot;...&quot; },
    { &quot;venue&quot;: &quot;...&quot;, &quot;description&quot;: &quot;...&quot; }
]
```

面[对这样可能的边缘情况，笔者之前介绍过微软相关的技术，如 G](http://mp.weixin.qq.com/s?__biz=MzA5MTIxNTY4MQ==&mid=2461140985&idx=1&sn=633a17ff038b1de085905665332fd823&chksm=873965d7b04eecc1677613b3eb72075993a0cd40c4e384c4a724807c06b671b9042e2cc2bf9e&scene=21#wechat_redirect)uidance， typechat，详情阅读《 新一代的交互形式LUI（language user interface）到来，微软为它架了一座桥 》。

![](img/d4f33fd94d14208c2490a53669f036f1.png)

+   选项：

```
import outlines

model = outlines.models.transformers(&quot;mistralai/Mistral-7B-Instruct-v0.2&quot;)

prompt = &quot;&quot;&quot;You are a sentiment-labelling assistant.
Is the following review positive or negative?

Review: This restaurant is just awesome!
&quot;&quot;&quot;

generator = outlines.generate.choice(model, [&quot;Positive&quot;, &quot;Negative&quot;])
answer = generator(prompt)
```

+   类型限制

```
from outlines import models, generate

model = models.transformers(&quot;mistralai/Mistral-7B-v0.1&quot;)
generator = generate.format(model, int)
answer = generator(&quot;When I was 6 my sister was half my age. Now I’m 70 how old is my sister?&quot;)
print(answer)
# 67
```

+   正则

```
import outlines

model = outlines.models.transformers(&quot;mistralai/Mistral-7B-Instruct-v0.2&quot;)

prompt = &quot;What is the IP address of the Google DNS servers? &quot;

generator = outlines.generate.text(model)
unstructured = generator(prompt, max_tokens=30)

generator = outlines.generate.regex(
    model,
    r&quot;((25[0-5]|2[0-4]\d|[01]?\d\d?)\.){3}(25[0-5]|2[0-4]\d|[01]?\d\d?)&quot;,
)
structured = generator(prompt, max_tokens=30)

print(unstructured)
# What is the IP address of the Google DNS servers?
#
# Passive DNS servers are at DNS servers that are private.
# In other words, both IP servers are private. The database
# does not contain Chelsea Manning

print(structured)
# What is the IP address of the Google DNS servers?
# 2.2.6.1
```

+   json model

```
from pydantic import BaseModel

from outlines import models, generate


class User(BaseModel):
    name: str
    last_name: str
    id: int


model = models.transformers(&quot;mistralai/Mistral-7B-v0.1&quot;)
generator = generate.json(model, User)
result = generator(
    &quot;Create a user profile with the fields name, last_name and id&quot;
)
print(result)
# User(name=&quot;John&quot;, last_name=&quot;Doe&quot;, id=11)
```

outlines还提供了Prompt template的功能，支持对prompt进行管理和优化了使用方法，更加符合python编程习惯。

```
import outlines

examples = [
    (&quot;The food was disgusting&quot;, &quot;Negative&quot;),
    (&quot;We had a fantastic night&quot;, &quot;Positive&quot;),
    (&quot;Recommended&quot;, &quot;Positive&quot;),
    (&quot;The waiter was rude&quot;, &quot;Negative&quot;)
]

@outlines.prompt
def labelling(to_label, examples):
    &quot;&quot;&quot;You are a sentiment-labelling assistant.

    {% for example in examples %}
    {{ example[0] }} // {{ example[1] }}
    {% endfor %}
    {{ to_label }} //
    &quot;&quot;&quot;

model = outlines.models.transformers(&quot;mistralai/Mistral-7B-Instruct-v0.2&quot;)
prompt = labelling(&quot;Just awesome&quot;, examples)
answer = outlines.generate.text(model)(prompt, max_tokens=100)
```

通过上面的介绍可以看出，outlines在使用上比较简单，非常适合python开发者，能够很好的完成LLM输出格式化的需求。

值得[一提的是，outlines的结构化生成方法在推](https://blog.dottxt.co/coalescence.html)理过程中几乎不产生任何额外成本，也不比从模型生成非结构化文本花费更多时间，甚至其推理速度比普通生成快5倍。具体分析可以查看官方解读文章《Coalescence: making LLM inference 5x faster》。