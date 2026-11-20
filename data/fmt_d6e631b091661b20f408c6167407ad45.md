
# Qwen3 Think 解密

> 来源：[https://v11enp9ok1h.feishu.cn/docx/BULvdBCUHo8ccSxTLZdcUyDtnQ8](https://v11enp9ok1h.feishu.cn/docx/BULvdBCUHo8ccSxTLZdcUyDtnQ8)

在本次 Qwen3 的更新中，支持多种思考模式可谓是核心亮点之一。Qwen3 既可以在思考模式下深思熟虑，也可以在非思考模式中提供快速的响应。更重要的是，这两种模式的结合增强了 Qwen3 实现稳定且高效的“思考预算”控制能力。如下图所示，随着思考预算分配的提升，模型在评测集上的得分也逐渐提升。这样一来，用户可以根据不同的任务难度分配不同的预算，从而在速度与性能之间实现更优的平衡。

那么，思考预算和思考模式切换都是如何实现的呢？

![](img/46f4b112613af55bcb8e931f309bde80.png)

## 思考预算原理

在 Qwen Chat 网页版 `chat.qwen.ai` 上，深度思考的思考预算是可调节的，粒度为 1024 tokens，最大可达 38912 tokens，也就是 `38 * 1024` tokens，如下图所示。

![](img/82c908cd5d0de63852e65447b72e538e.png)

那么，在解密思考预算调整的原理之前，我们先来看一个例子。下图展示了 Qwen3-235B-A22B 在 1024 tokens 的思考预算下回答问题的情况。（为了能让 Qwen3 可以思考到 1024 tokens 或者更多，这里使用了一个马尔科夫链的相关问题）

![](img/53a5d25156c33a17f23612629a6d2b31.png)

从上图中可以看到，Qwen3 在达到思考预算上限时，思考过程就戛然而止了。所以很难不让人相信，思考预算的实现原理就是，统计思考过程中的（`<think>` 后）tokens 数量，达到思考预算上限时，立刻停止思考（强行截断停止），并补上 `</think>`，以进入输出阶段。

## 思考模式切换

最后是 Qwen3 思考模式切换的原理。这部分原理相对透明，可以在 Qwen3 的 `tokenizer_config.json` 中的 `chat_template` 中看到。下面展示了 Jinja 格式的 Qwen3 对话模板。

<details><summary>Jinja 格式 Qwen3 对话模板完整版</summary>

```
{%- if tools %}
    {{- &#x27;&lt;|im_start|&gt;system\n&#x27; }}
    {%- if messages[0].role == &#x27;system&#x27; %}
        {{- messages[0].content + &#x27;\n\n&#x27; }}
    {%- endif %}
    {{- &quot;# Tools\n\nYou may call one or more functions to assist with the user query.\n\nYou are provided with function signatures within &lt;tools&gt;&lt;/tools&gt; XML tags:\n&lt;tools&gt;&quot; }}
    {%- for tool in tools %}
        {{- &#x27;\n&#x27; }}
        {{- tool | tojson }}
    {%- endfor %}
    {{- &quot;\n&lt;/tools&gt;\n\nFor each function call, return a json object with function name and arguments within &lt;tool_call&gt;&lt;/tool_call&gt; XML tags:\n&lt;tool_call&gt;\n{\&quot;name\&quot;: &lt;function-name&gt;, \&quot;arguments\&quot;: &lt;args-json-object&gt;}\n&lt;/tool_call&gt;&lt;|im_end|&gt;\n&quot; }}
{%- else %}
    {%- if messages[0].role == &#x27;system&#x27; %}
        {{- &#x27;&lt;|im_start|&gt;system\n&#x27; + messages[0].content + &#x27;&lt;|im_end|&gt;\n&#x27; }}
    {%- endif %}
{%- endif %}
{%- set ns = namespace(multi_step_tool=true, last_query_index=messages|length - 1) %}
{%- for message in messages[::-1] %}
    {%- set index = (messages|length - 1) - loop.index0 %}
    {%- if ns.multi_step_tool and message.role == &quot;user&quot; and not(message.content.startsWith(&#x27;&lt;tool_response&gt;&#x27;) and message.content.endsWith(&#x27;&lt;/tool_response&gt;&#x27;)) %}
        {%- set ns.multi_step_tool = false %}
        {%- set ns.last_query_index = index %}
    {%- endif %}
{%- endfor %}
{%- for message in messages %}
    {%- if (message.role == &quot;user&quot;) or (message.role == &quot;system&quot; and not loop.first) %}
        {{- &#x27;&lt;|im_start|&gt;&#x27; + message.role + &#x27;\n&#x27; + message.content + &#x27;&lt;|im_end|&gt;&#x27; + &#x27;\n&#x27; }}
    {%- elif message.role == &quot;assistant&quot; %}
        {%- set content = message.content %}
        {%- set reasoning_content = &#x27;&#x27; %}
        {%- if message.reasoning_content is defined and message.reasoning_content is not none %}
            {%- set reasoning_content = message.reasoning_content %}
        {%- else %}
            {%- if &#x27;&lt;/think&gt;&#x27; in message.content %}
                {%- set content = message.content.split(&#x27;&lt;/think&gt;&#x27;)[-1].lstrip(&#x27;\n&#x27;) %}
                {%- set reasoning_content = message.content.split(&#x27;&lt;/think&gt;&#x27;)[0].rstrip(&#x27;\n&#x27;).split(&#x27;&lt;think&gt;&#x27;)[-1].lstrip(&#x27;\n&#x27;) %}
            {%- endif %}
        {%- endif %}
        {%- if loop.index0 &gt; ns.last_query_index %}
            {%- if loop.last or (not loop.last and reasoning_content) %}
                {{- &#x27;&lt;|im_start|&gt;&#x27; + message.role + &#x27;\n&lt;think&gt;\n&#x27; + reasoning_content.strip(&#x27;\n&#x27;) + &#x27;\n&lt;/think&gt;\n\n&#x27; + content.lstrip(&#x27;\n&#x27;) }}
            {%- else %}
                {{- &#x27;&lt;|im_start|&gt;&#x27; + message.role + &#x27;\n&#x27; + content }}
            {%- endif %}
        {%- else %}
            {{- &#x27;&lt;|im_start|&gt;&#x27; + message.role + &#x27;\n&#x27; + content }}
        {%- endif %}
        {%- if message.tool_calls %}
            {%- for tool_call in message.tool_calls %}
                {%- if (loop.first and content) or (not loop.first) %}
                    {{- &#x27;\n&#x27; }}
                {%- endif %}
                {%- if tool_call.function %}
                    {%- set tool_call = tool_call.function %}
                {%- endif %}
                {{- &#x27;&lt;tool_call&gt;\n{&quot;name&quot;: &quot;&#x27; }}
                {{- tool_call.name }}
                {{- &#x27;&quot;, &quot;arguments&quot;: &#x27; }}
                {%- if tool_call.arguments is string %}
                    {{- tool_call.arguments }}
                {%- else %}
                    {{- tool_call.arguments | tojson }}
                {%- endif %}
                {{- &#x27;}\n&lt;/tool_call&gt;&#x27; }}
            {%- endfor %}
        {%- endif %}
        {{- &#x27;&lt;|im_end|&gt;\n&#x27; }}
    {%- elif message.role == &quot;tool&quot; %}
        {%- if loop.first or (messages[loop.index0 - 1].role != &quot;tool&quot;) %}
            {{- &#x27;&lt;|im_start|&gt;user&#x27; }}
        {%- endif %}
        {{- &#x27;\n&lt;tool_response&gt;\n&#x27; }}
        {{- message.content }}
        {{- &#x27;\n&lt;/tool_response&gt;&#x27; }}
        {%- if loop.last or (messages[loop.index0 + 1].role != &quot;tool&quot;) %}
            {{- &#x27;&lt;|im_end|&gt;\n&#x27; }}
        {%- endif %}
    {%- endif %}
{%- endfor %}
{%- if add_generation_prompt %}
    {{- &#x27;&lt;|im_start|&gt;assistant\n&#x27; }}
    {%- if enable_thinking is defined and enable_thinking is false %}
        {{- &#x27;&lt;think&gt;\n\n&lt;/think&gt;\n\n&#x27; }}
    {%- endif %}
{%- endif %}
```

</details>

我们直接来看最后几行，也就是 `add_generation_prompt` 部分。

```
{%- if add_generation_prompt %}
    {{- &#x27;&lt;|im_start|&gt;assistant\n&#x27; }}
    {%- if enable_thinking is defined and enable_thinking is false %}
        {{- &#x27;&lt;think&gt;\n\n&lt;/think&gt;\n\n&#x27; }}
    {%- endif %}
{%- endif %}
```

上面就是 `add_generation_prompt` 部分的代码。可以看到，如果传入了 `enable_thinking` 并且为 false 的情况下，模型就会在 `<|im_start|>assistant\n` 的后面再补上 `<think>\n\n</think>\n\n` 以让模型结束思考，直接进入输出阶段。

让我们再来看 QwQ 在 `add_generation_prompt` 时的行为，来更好地理解 Qwen3 的思考模式切换逻辑。

```
{%- if add_generation_prompt %}
    {{- &#x27;&lt;|im_start|&gt;assistant\n&lt;think&gt;\n\n&#x27; }}
{%- endif %}
```

可以看到 QwQ 通过补上额外的 `<think>` 来让模型强制进入思考阶段。而 Qwen3 通过补上 `<think>\n\n</think>\n\n` 的方式告诉模型，思考阶段什么都没有，但是思考阶段已经结束了，需要进入最终输出阶段了，从而实现了思考模式切换。
