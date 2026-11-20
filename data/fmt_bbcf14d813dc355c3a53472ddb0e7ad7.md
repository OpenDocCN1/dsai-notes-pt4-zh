
# 微调数据集结构与 Function calling

## 基础 Function calling 流程

更进一步的，如果对话过程中带入了Function calling，此时首先模型会读取提前准备好的tool schema（也可能是自动生成的，例如MCP即可自动创建tool schema）：

```
tool_schema = [{
    &quot;name&quot;: &quot;get_weather&quot;,
    &quot;description&quot;: &quot;查询指定城市的天气信息&quot;,
    &quot;parameters&quot;: {
        &quot;type&quot;: &quot;object&quot;,
        &quot;properties&quot;: {
            &quot;location&quot;: {
                &quot;type&quot;: &quot;string&quot;,
                &quot;description&quot;: &quot;要查询天气的城市名称&quot;
            }
        },
        &quot;required&quot;: [&quot;location&quot;]
    }
}]
```

而假设我们的对话内容如下：

+ 系统提示词（`instruction`）：你是一名助人为乐的助手。当用户查询天气的时候，请调用`get_weather`函数进行天气信息查询。

+ 用户输入（`input`）：你好，请帮我查询下北京天气。

+ 助手回复（`output`）：`{"name": "get_weather", "arguments": {"location": "北京"}}`

> 此时回复内容就是一条Function call message

而此时模型真实的输入和输出内容如下：

```
&lt;|im_start|&gt;system
你是一名助人为乐的助手。当用户查询天气的时候，请调用get_weather函数进行天气信息查询。

# Tools

You may call one or more functions to assist with the user query.

You are provided with function signatures within &lt;tools&gt;&lt;/tools&gt; XML tags:
&lt;tools&gt;
{&quot;name&quot;: &quot;get_weather&quot;, &quot;description&quot;: &quot;查询指定城市的天气信息&quot;, &quot;parameters&quot;: {&quot;type&quot;: &quot;object&quot;, &quot;properties&quot;: {&quot;location&quot;: {&quot;type&quot;: &quot;string&quot;, &quot;description&quot;: &quot;要查询天气的城市名称&quot;}}, &quot;required&quot;: [&quot;location&quot;]}}
&lt;/tools&gt;

For each function call, return a json object with function name and arguments within &lt;tool_call&gt;&lt;/tool_call&gt; XML tags:
&lt;tool_call&gt;
{&quot;name&quot;: &lt;function-name&gt;, &quot;arguments&quot;: &lt;args-json-object&gt;}
&lt;/tool_call&gt;
&lt;|im_end|&gt;
&lt;|im_start|&gt;user
你好，请帮我查询下北京天气。&lt;|im_end|&gt;
&lt;|im_start|&gt;assistant
&lt;tool_call&gt;
{&quot;name&quot;: &quot;get_weather&quot;, &quot;arguments&quot;: {&quot;location&quot;: &quot;北京&quot;}}
&lt;/tool_call&gt;&lt;|im_end|&gt;
```

接下来在进行训练时，模型同样根据`assistant`前的内容，学习`assistant`后面的输出内容。不过需要注意的是，由于高效微调调整的参数量较少，因此只能优化模型的Function calling能力，并不能从无到有让模型学会Function calling。

## 带有思考过程的微调数据集结构

而如果是带有思考链，则一个简单的问答数据如下：

![](img/f487fea0a2f8b678737861c4dbda183a.png)

+ 系统提示词（`instruction`）：你是一名助人为乐的助手。

+ 用户输入（`input`）：你好，好久不见。

+ 助手回复（`output`）：好的，用户发来“你好，好久不见！”，我需要回应。首先，用户可能希望得到亲切的回应，所以应该用友好的语气。/n是的呀，好久不见，最近有什么有趣的事情要和我分享么？

此时模型真实的内部输入和输出结果如下：

```
&lt;|im_start|&gt;system
你是一名助人为乐的助手。&lt;|im_end|&gt;
&lt;|im_start|&gt;user
你好，好久不见。&lt;|im_end|&gt;
&lt;|im_start|&gt;assistant
&lt;think&gt;
好的，用户发来“你好，好久不见！”，我需要回应。首先，用户可能希望得到亲切的回应，所以应该用友好的语气。
&lt;/think&gt;

是的呀，好久不见，最近有什么有趣的事情要和我分享么？&lt;|im_end|&gt;
```

模型同样根据`assistant`前的内容，学习`assistant`后面的输出内容。也就是说，所谓的思考过程，本质上其实是一种文本响应格式，通过模型训练而来。

## 带有思考过程的 Function calling 流程

最后难度升级，假设是带有思考过程、系统提示词的Function calling流程呢？此时一次对话的基本数据结构如下：

![](img/b78f0d608c56026d36f844e8edb7a35f.png)

内容如下：

+ 系统提示词（`instruction`）：你是一名助人为乐的助手。当用户查询天气的时候，请调用`get_weather`函数进行天气信息查询。

+ 用户输入（`input`）：你好，请帮我查询下北京天气。

+ 助手回复（`output`）：好的，用户问北京今天的天气，我应该尝试调用工具`get_weather`，并将参数设置为北京。/n`{"name": "get_weather", "arguments": {"location": "北京"}}`

而此时模型的真实输入和输出如下：

```
&lt;|im_start|&gt;system
你是一名助人为乐的助手。当用户查询天气的时候，请调用get_weather函数进行天气信息查询。

# Tools

You may call one or more functions to assist with the user query.

You are provided with function signatures within &lt;tools&gt;&lt;/tools&gt; XML tags:
&lt;tools&gt;
{&quot;name&quot;: &quot;get_weather&quot;, &quot;description&quot;: &quot;查询指定城市的天气信息&quot;, &quot;parameters&quot;: {&quot;type&quot;: &quot;object&quot;, &quot;properties&quot;: {&quot;location&quot;: {&quot;type&quot;: &quot;string&quot;, &quot;description&quot;: &quot;要查询天气的城市名称&quot;}}, &quot;required&quot;: [&quot;location&quot;]}}
&lt;/tools&gt;

For each function call, return a json object with function name and arguments within &lt;tool_call&gt;&lt;/tool_call&gt; XML tags:
&lt;tool_call&gt;
{&quot;name&quot;: &lt;function-name&gt;, &quot;arguments&quot;: &lt;args-json-object&gt;}
&lt;/tool_call&gt;
&lt;|im_end|&gt;
&lt;|im_start|&gt;user
你好，请帮我查询下北京天气。&lt;|im_end|&gt;
&lt;|im_start|&gt;assistant
&lt;think&gt;
好的，用户问北京今天的天气，我应该尝试调用工具 get_weather，并将参数设置为北京。
&lt;/think&gt;

&lt;tool_call&gt;
{&quot;name&quot;: &quot;get_weather&quot;, &quot;arguments&quot;: {&quot;location&quot;: &quot;北京&quot;}}
&lt;/tool_call&gt;&lt;|im_end|&gt;
```

模型同样根据`assistant`前的内容，学习`assistant`后面的输出内容。由此可见，模型拥有不同功能的背后，其实源于不同格式的训练数据集的训练。而对于gpt-oss这种模型来说，同时拥有Function calling、混合推理等功能，属于功能非常复杂的模型了。在实际微调过程中，稍有不慎就会令其丧失原有能力。

---
