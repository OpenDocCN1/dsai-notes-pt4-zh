# Part 4.GPT-OSS Agent性能微调

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/Yaejd8P1XojwSyxJMjVcqYHFn9d](https://kq4b3vgg5b.feishu.cn/docx/Yaejd8P1XojwSyxJMjVcqYHFn9d)

## 课程说明：

+   体验课内容节选自《2025大模型Agent智能体开发实战》(秋招冲刺班)完整版付费课程

体验课时间有限，若想深度学习大模型技术，欢迎大家报名由我主讲的《2025大模型Agent智能体开发实战》(秋招冲刺班)

---

# GPT-OSS高效微调实战

# Part 4.GPT-OSS Agent性能微调

在熟悉了Unsloth核心功能以及基础的微调数据集准备方法之后，接下来我们尝试进行一项更加复杂的微调工作——围绕GPT-OSS模型进行Agent性能微调。

## 九、Agent性能微调数据集准备

### 1. Hermes tool use数据集介绍

+   数据集地址：https://huggingface.co/datasets/interstellarninja/hermes_reasoning_tool_use

![](img/23803b1f58dda94329de0351095c7d54.png)

**interstellarninja / herme**s_reasoning_tool_use 是一个围绕“会思考、会用**工具”的开源**数据集：采用 Apache-2.0 许可、单一 train 切分约 51,004 条样本（约 392 MB），**以 ShareG**PT 会话格式存**储（Parquet），字段含 con****versations **/** tools / t**a**sk / categ**ory** / source** / scenario_category，便于直接用 Datas**ets/**axolotl 加载微调。其对话模板在 system 中明确要求将内在推理写入 <think>…</think>，并以 <tool_call>…</tool_call> 输出 OpenAI 风格的函数调用 JSON（含 pydantic schema），非常契合“思考链 + 函数调用”的监督信号。

样本覆盖 BFCL v3 的四类场景：single_turn、multi_turn、multi_step 与 relevance（无合适工具时应拒绝），有助于训练模型“何时/如何/是否”调用工具。构建流程基于 Nous Research 的 Atropos RL 栈，在自定义 MultiTurnToolCallingEnv 中进行 GRPO roll-outs，并通过奖励塑形与严格校验（如 _validate_think_plus_calls、_check_sequential_tools）仅保留结构与语义均合格的轨迹。

并且，数据集包含四种调用格式，single_turn（单轮单调用）、multi_turn（多轮多调用）、mul**ti_step（**单轮内**≥2 顺序调用**）、relevance（无合适工具应拒绝）。这天然覆盖了“何时/如何/是否”调用工具的决策学习。

总体而言，这是一个**兼具长链推理标注与工具调用轨迹**的实用数据集，既可用于 **SFT**，也可作为 **GRPO 预热**或评测基座，快速提升模型在真实代理场景下的工具使用稳健性。

+   数据集读取

```
from datasets import load_dataset, Dataset, Features, Value
import random, json, os
```

```
# 原始数据
ds_full = load_dataset(&quot;interstellarninja/hermes_reasoning_tool_use&quot;, split=&quot;train&quot;)
```

```
Using the latest cached version of the dataset since interstellarninja/hermes_reasoning_tool_use couldn&#x27;t be found on the Hugging Face Hub
WARNING:datasets.load:Using the latest cached version of the dataset since interstellarninja/hermes_reasoning_tool_use couldn&#x27;t be found on the Hugging Face Hub
Found the latest cached dataset configuration &#x27;default&#x27; at /root/.cache/huggingface/datasets/interstellarninja___hermes_reasoning_tool_use/default/0.0.0/55d824b623303055d5a76eb6ab12861b80a4ee20 (last modified on Mon Aug 25 20:12:37 2025).
WARNING:datasets.packaged_modules.cache.cache:Found the latest cached dataset configuration &#x27;default&#x27; at /root/.cache/huggingface/datasets/interstellarninja___hermes_reasoning_tool_use/default/0.0.0/55d824b623303055d5a76eb6ab12861b80a4ee20 (last modified on Mon Aug 25 20:12:37 2025).
```

```
ds_full[0]
```

```
{&#x27;conversations&#x27;: [{&#x27;from&#x27;: &#x27;system&#x27;,
   &#x27;value&#x27;: &#x27;You are a deep thinking AI, you may use extremely long chains of thought to deeply consider the problem and deliberate with yourself via systematic reasoning processes to help come to a correct solution prior to answering. You should enclose your thoughts and internal monologue inside &lt;think&gt; &lt;/think&gt; tags, and then provide your solution or response to the problem.\n\nYou are a function calling AI model. You are provided with function signatures within &lt;tools&gt; &lt;/tools&gt; XML tags. You may call one or more functions to assist with the user query. Don\&#x27;t make assumptions about what values to plug into functions. Here are the available tools: &lt;tools&gt; [{&quot;name&quot;: &quot;calculate_age&quot;, &quot;description&quot;: &quot;Calculates the age based on the birthdate.&quot;, &quot;parameters&quot;: {&quot;birthdate&quot;: {&quot;description&quot;: &quot;The birthdate in the format \&#x27;YYYY-MM-DD\&#x27;.&quot;, &quot;type&quot;: &quot;str&quot;}}}, {&quot;name&quot;: &quot;find_duplicate_number&quot;, &quot;description&quot;: &quot;Finds the duplicate number in a list of integers.&quot;, &quot;parameters&quot;: {&quot;nums&quot;: {&quot;description&quot;: &quot;The list of integers.&quot;, &quot;type&quot;: &quot;List[int]&quot;}}}, {&quot;name&quot;: &quot;word_frequency&quot;, &quot;description&quot;: &quot;Calculates the frequency of each word in a text.&quot;, &quot;parameters&quot;: {&quot;text&quot;: {&quot;description&quot;: &quot;The input text.&quot;, &quot;type&quot;: &quot;str&quot;}}}, {&quot;name&quot;: &quot;reverse_words&quot;, &quot;description&quot;: &quot;Reverses the order of words in a sentence.&quot;, &quot;parameters&quot;: {&quot;sentence&quot;: {&quot;description&quot;: &quot;The input sentence.&quot;, &quot;type&quot;: &quot;str&quot;}}}] &lt;/tools&gt; Use the following pydantic model JSON schema for each tool call you will make: {\&#x27;properties\&#x27;: {\&#x27;arguments\&#x27;: {\&#x27;title\&#x27;: \&#x27;Arguments\&#x27;, \&#x27;type\&#x27;: \&#x27;object\&#x27;}, \&#x27;name\&#x27;: {\&#x27;title\&#x27;: \&#x27;Name\&#x27;, \&#x27;type\&#x27;:\&#x27;string\&#x27;}},\&#x27;required\&#x27;: [\&#x27;arguments\&#x27;, \&#x27;name\&#x27;], \&#x27;title\&#x27;: \&#x27;FunctionCall\&#x27;, \&#x27;type\&#x27;: \&#x27;object\&#x27;} For each function call return a json object with function name and arguments within &lt;tool_call&gt; &lt;/tool_call&gt; XML tags as follows:\n&lt;tool_call&gt;\n{tool_call}\n&lt;/tool_call&gt;\n\n&#x27;},
  {&#x27;from&#x27;: &#x27;human&#x27;,
   &#x27;value&#x27;: &quot;What is the word frequency of &#x27;The quick brown fox jumps over the lazy dog&#x27;? And can you reverse the words in the sentence &#x27;Hello world this is a test&#x27;?&quot;},
  {&#x27;from&#x27;: &#x27;gpt&#x27;,
   &#x27;value&#x27;: &#x27;&lt;think&gt;\nOkay, the user has two separate questions here. The first is to find the word frequency of the sentence &quot;The quick brown fox jumps over the lazy dog&quot;. The second is to reverse the words in the sentence &quot;Hello world this is a test&quot;. Let me break down each part step by step.\n\nStarting with the word frequency: I need to use the word_frequency tool. The input text is given as a string, so I\&#x27;ll pass that into the tool. That should return a dictionary where each key is a word and the value is its frequency.\n\nNext, the reverse the sentence part. The user wants the words reversed. That\&#x27;s a straightforward reverse of words in the sentence. The reverse_words tool should handle this. The sentence is &quot;Hello world this is a test&quot;, so I\&#x27;ll pass that into the reverse_words function, which will return the sentence with words reversed.\n\nSo I need to make two separate function calls: one for word_frequency and another for reverse_words. I\&#x27;ll use the word_frequency tool with the sentence as the text parameter, and reverse_words with the same sentence. The outputs from each tool will be returned as JSON objects.\n&lt;/think&gt;\n&lt;tool_call&gt;\n{&quot;name&quot;:&quot;word_frequency&quot;,&quot;arguments&quot;:{&quot;text&quot;:&quot;The quick brown fox jumps over the lazy dog&quot;}}\n&lt;/tool_call&gt;\n&lt;tool_call&gt;\n{&quot;name&quot;:&quot;reverse_words&quot;,&quot;arguments&quot;:{&quot;sentence&quot;:&quot;Hello world this is a test&quot;}}\n&lt;/tool_call&gt;&#x27;}],
 &#x27;tools&#x27;: &#x27;[{&quot;name&quot;: &quot;calculate_age&quot;, &quot;description&quot;: &quot;Calculates the age based on the birthdate.&quot;, &quot;parameters&quot;: {&quot;birthdate&quot;: {&quot;description&quot;: &quot;The birthdate in the format \&#x27;YYYY-MM-DD\&#x27;.&quot;, &quot;type&quot;: &quot;str&quot;}}}, {&quot;name&quot;: &quot;find_duplicate_number&quot;, &quot;description&quot;: &quot;Finds the duplicate number in a list of integers.&quot;, &quot;parameters&quot;: {&quot;nums&quot;: {&quot;description&quot;: &quot;The list of integers.&quot;, &quot;type&quot;: &quot;List[int]&quot;}}}, {&quot;name&quot;: &quot;word_frequency&quot;, &quot;description&quot;: &quot;Calculates the frequency of each word in a text.&quot;, &quot;parameters&quot;: {&quot;text&quot;: {&quot;description&quot;: &quot;The input text.&quot;, &quot;type&quot;: &quot;str&quot;}}}, {&quot;name&quot;: &quot;reverse_words&quot;, &quot;description&quot;: &quot;Reverses the order of words in a sentence.&quot;, &quot;parameters&quot;: {&quot;sentence&quot;: {&quot;description&quot;: &quot;The input sentence.&quot;, &quot;type&quot;: &quot;str&quot;}}}]&#x27;,
 &#x27;task&#x27;: &quot;What is the word frequency of &#x27;The quick brown fox jumps over the lazy dog&#x27;? And can you reverse the words in the sentence &#x27;Hello world this is a test&#x27;?&quot;,
 &#x27;category&#x27;: &#x27;Word Frequency&#x27;,
 &#x27;source&#x27;: &#x27;Salesforce-Xlam&#x27;,
 &#x27;scenario_category&#x27;: &#x27;single&#x27;}
```

Hermes Reasoning Tool Use 是一个 “推理 + 工具调用（function calling）” 的教学/微调数据集。它不仅包含自然语言对话，还显式标注了：

+   **系统提示**（system）里提供的工具签名（函数名、参数等）；

+   模型在对话中**调用工具**的动作（函数名 + 参数）；

+   工具的**返回结果**（带 <tool_response>）；

+   模型的**思维过程**（使用 <think>…</think> 包裹的内在推理）。

**因此**它特别适合训练**/评**估：

何时**调用**哪个工具、如何构造参数、如何**把工具结果融**入最终答复、以及多步工具调用的策划与执行。

```
ds_full
```

```
Dataset({
    features: [&#x27;conversations&#x27;, &#x27;tools&#x27;, &#x27;task&#x27;, &#x27;category&#x27;, &#x27;source&#x27;, &#x27;scenario_category&#x27;],
    num_rows: 51004
})
```

```
ds_full[&#x27;task&#x27;][:10]
```

```
[&quot;What is the word frequency of &#x27;The quick brown fox jumps over the lazy dog&#x27;? And can you reverse the words in the sentence &#x27;Hello world this is a test&#x27;?&quot;,
 &quot;Search for &#x27;laptops&#x27; on Tmall and retrieve the second page of results.&quot;,
 &#x27;I would like to extract details from a LinkedIn company page. Could you assist me in fetching the information from this URL: https://www.linkedin.com/company/abc-corporation?&#x27;,
 &#x27;I need to compare the weights of two groups of athletes. Group X weighs [70, 72, 74, 76, 78] kg and Group Y weighs [71, 73, 75, 77, 79] kg. Perform the t-test with a significance level of 0.10.&#x27;,
 &#x27;I\&#x27;d like to learn Spanish and need to understand my current proficiency level. Also, once I know my level, I need a translation of the following text: &quot;I want to learn Spanish quickly&quot; into Spanish.&#x27;,
 &#x27;Retrieve the metadata information for Ethereum (ETH) on the Binance exchange, including metrics related to the asset.&#x27;,
 &#x27;Can you perform a static analysis on floors 3, 7, and 11?&#x27;,
 &#x27;How can I view the analysis for Tesla, Inc. from a week ago?&#x27;,
 &#x27;I need to find out about battles between June 12th and July 15th involving Germany and France. What types of military events took place?&#x27;,
 &quot;Generate a static map image of size 1600x1600 centered at the coordinates (55.7558, 37.6176) with a zoom level of 11, using the &#x27;toursprung-terrain&#x27; maptype, and include a delta_zoom of 2. Output the URL.&quot;]
```

```
ds_full[&#x27;source&#x27;][:10]
```

```
[&#x27;Salesforce-Xlam&#x27;,
 &#x27;Nvidia-When2Call&#x27;,
 &#x27;ToolAce&#x27;,
 &#x27;Nvidia-When2Call&#x27;,
 &#x27;ToolAce&#x27;,
 &#x27;Nvidia-When2Call&#x27;,
 &#x27;Nvidia-When2Call&#x27;,
 &#x27;Nvidia-When2Call&#x27;,
 &#x27;ToolAce&#x27;,
 &#x27;Nvidia-When2Call&#x27;]
```

```
ds_full[&#x27;scenario_category&#x27;][:10]
```

```
[&#x27;single&#x27;,
 &#x27;single&#x27;,
 &#x27;multiturn&#x27;,
 &#x27;single&#x27;,
 &#x27;multistep&#x27;,
 &#x27;relevance&#x27;,
 &#x27;relevance&#x27;,
 &#x27;relevance&#x27;,
 &#x27;relevance&#x27;,
 &#x27;single&#x27;]
```

数据集每条样本通常包含以下顶层字段：

+   conversations：**对话轮次**（list）。每轮有：

+   from：system / human / gpt / tool

+   value：该轮文本。

+   system 轮里常见 <tools> … </tools>（提供函数签名）

+   gpt 轮里可能含 <think>…</think> 与一个或多个 <tool_call> … </tool_call>

+   tool 轮里含 <tool_response> … </tool_response>

+   tools：工具清单（有时是 **字符串化 JSON**，有时是 list[dict]）。与 system 里的 <tools> 一致或互补。

+   task：该样本的人类任务简述。

+   category：任务类别（如 Is Hotel Available、Tool Use、Get Stock Price 等）。

+   source：来源（如 Nvidia-When2Call、ToolAce、Glaive、Nous-Hermes、Salesforce-Xlam）。

+   scenario_category：场景类型：

+   single：单轮对话 + 单次或多次调用（在一轮内发起多个工具调用）

+   multiturn：多轮往返

+   multistep：需要按顺序完成多个工具步骤

+   relevance：强调选择**相关**工具（干扰项较多）

> ⚠️ 真实数据里 **类型并不总统一**：

+   tools 可能是字符串，也可能是对象数组；

+   parameters.type 可能写成 "dict"（理想应为 "object"）；

+   required 可能放在工具**外层**而不是 parameters.required；

+   有的属性缺失 description。
这就是我们清洗规范化要解决的。

+   单工具调用示例

```
ds_full[0][&#x27;conversations&#x27;]
```

```
[{&#x27;from&#x27;: &#x27;system&#x27;,
  &#x27;value&#x27;: &#x27;You are a deep thinking AI, you may use extremely long chains of thought to deeply consider the problem and deliberate with yourself via systematic reasoning processes to help come to a correct solution prior to answering. You should enclose your thoughts and internal monologue inside &lt;think&gt; &lt;/think&gt; tags, and then provide your solution or response to the problem.\n\nYou are a function calling AI model. You are provided with function signatures within &lt;tools&gt; &lt;/tools&gt; XML tags. You may call one or more functions to assist with the user query. Don\&#x27;t make assumptions about what values to plug into functions. Here are the available tools: &lt;tools&gt; [{&quot;name&quot;: &quot;calculate_age&quot;, &quot;description&quot;: &quot;Calculates the age based on the birthdate.&quot;, &quot;parameters&quot;: {&quot;birthdate&quot;: {&quot;description&quot;: &quot;The birthdate in the format \&#x27;YYYY-MM-DD\&#x27;.&quot;, &quot;type&quot;: &quot;str&quot;}}}, {&quot;name&quot;: &quot;find_duplicate_number&quot;, &quot;description&quot;: &quot;Finds the duplicate number in a list of integers.&quot;, &quot;parameters&quot;: {&quot;nums&quot;: {&quot;description&quot;: &quot;The list of integers.&quot;, &quot;type&quot;: &quot;List[int]&quot;}}}, {&quot;name&quot;: &quot;word_frequency&quot;, &quot;description&quot;: &quot;Calculates the frequency of each word in a text.&quot;, &quot;parameters&quot;: {&quot;text&quot;: {&quot;description&quot;: &quot;The input text.&quot;, &quot;type&quot;: &quot;str&quot;}}}, {&quot;name&quot;: &quot;reverse_words&quot;, &quot;description&quot;: &quot;Reverses the order of words in a sentence.&quot;, &quot;parameters&quot;: {&quot;sentence&quot;: {&quot;description&quot;: &quot;The input sentence.&quot;, &quot;type&quot;: &quot;str&quot;}}}] &lt;/tools&gt; Use the following pydantic model JSON schema for each tool call you will make: {\&#x27;properties\&#x27;: {\&#x27;arguments\&#x27;: {\&#x27;title\&#x27;: \&#x27;Arguments\&#x27;, \&#x27;type\&#x27;: \&#x27;object\&#x27;}, \&#x27;name\&#x27;: {\&#x27;title\&#x27;: \&#x27;Name\&#x27;, \&#x27;type\&#x27;:\&#x27;string\&#x27;}},\&#x27;required\&#x27;: [\&#x27;arguments\&#x27;, \&#x27;name\&#x27;], \&#x27;title\&#x27;: \&#x27;FunctionCall\&#x27;, \&#x27;type\&#x27;: \&#x27;object\&#x27;} For each function call return a json object with function name and arguments within &lt;tool_call&gt; &lt;/tool_call&gt; XML tags as follows:\n&lt;tool_call&gt;\n{tool_call}\n&lt;/tool_call&gt;\n\n&#x27;},
 {&#x27;from&#x27;: &#x27;human&#x27;,
  &#x27;value&#x27;: &quot;What is the word frequency of &#x27;The quick brown fox jumps over the lazy dog&#x27;? And can you reverse the words in the sentence &#x27;Hello world this is a test&#x27;?&quot;},
 {&#x27;from&#x27;: &#x27;gpt&#x27;,
  &#x27;value&#x27;: &#x27;&lt;think&gt;\nOkay, the user has two separate questions here. The first is to find the word frequency of the sentence &quot;The quick brown fox jumps over the lazy dog&quot;. The second is to reverse the words in the sentence &quot;Hello world this is a test&quot;. Let me break down each part step by step.\n\nStarting with the word frequency: I need to use the word_frequency tool. The input text is given as a string, so I\&#x27;ll pass that into the tool. That should return a dictionary where each key is a word and the value is its frequency.\n\nNext, the reverse the sentence part. The user wants the words reversed. That\&#x27;s a straightforward reverse of words in the sentence. The reverse_words tool should handle this. The sentence is &quot;Hello world this is a test&quot;, so I\&#x27;ll pass that into the reverse_words function, which will return the sentence with words reversed.\n\nSo I need to make two separate function calls: one for word_frequency and another for reverse_words. I\&#x27;ll use the word_frequency tool with the sentence as the text parameter, and reverse_words with the same sentence. The outputs from each tool will be returned as JSON objects.\n&lt;/think&gt;\n&lt;tool_call&gt;\n{&quot;name&quot;:&quot;word_frequency&quot;,&quot;arguments&quot;:{&quot;text&quot;:&quot;The quick brown fox jumps over the lazy dog&quot;}}\n&lt;/tool_call&gt;\n&lt;tool_call&gt;\n{&quot;name&quot;:&quot;reverse_words&quot;,&quot;arguments&quot;:{&quot;sentence&quot;:&quot;Hello world this is a test&quot;}}\n&lt;/tool_call&gt;&#x27;}]
```

+   多工具并行调用示例

```
ds_full[1][&#x27;conversations&#x27;]
```

  &#x27;value&#x27;: &#x27;&lt;tool_response&gt;\n{&quot;name&quot;:&quot;tmall_search_by_keyword&quot;,&quot;content&quot;:{&quot;data&quot;:[{&quot;id&quot;:&quot;10000000&quot;,&quot;title&quot;:&quot;Acer Aspire 5 Slim Laptop&quot;,&quot;price&quot;:&quot;699.99&quot;,&quot;picture&quot;:&quot;https://example.com/acer-aspire-5.jpg&quot;,&quot;description&quot;:&quot;15.6 inches FHD IPS Display, 8GB RAM, 256GB SSD&quot;,&quot;brand&quot;:&quot;Acer&quot;,&quot;category&quot;:&quot;Laptops&quot;,&quot;rating&quot;:4.5},{&quot;id&quot;:&quot;10000001&quot;,&quot;title&quot;:&quot;HP Pavilion x360 14&quot;,&quot;price&quot;:&quot;599.99&quot;,&quot;picture&quot;:&quot;https://example.com/hp-pavilion-x360.jpg&quot;,&quot;description&quot;:&quot;14 inches Touchscreen Convertible, 8GB RAM, 256GB SSD&quot;,&quot;brand&quot;:&quot;HP&quot;,&quot;category&quot;:&quot;Laptops&quot;,&quot;rating&quot;:4.3},{&quot;id&quot;:&quot;10000002&quot;,&quot;title&quot;:&quot;Dell Inspiron 15 3501 Laptop&quot;,&quot;price&quot;:&quot;549.99&quot;,&quot;picture&quot;:&quot;https://example.com/dell-inspiron.jpg&quot;,&quot;description&quot;:&quot;15.6 inches HD Display, 8GB RAM, 256GB SSD&quot;,&quot;brand&quot;:&quot;Dell&quot;,&quot;category&quot;:&quot;Laptops&quot;,&quot;rating&quot;:4.1},{&quot;id&quot;:&quot;10000003&quot;,&quot;title&quot;:&quot;Lenovo IdeaPad 3 Laptop&quot;,&quot;price&quot;:&quot;529.99&quot;,&quot;picture&quot;:&quot;https://example.com/lenovo-ideapad.jpg&quot;,&quot;description&quot;:&quot;15.6 inches HD Display, 8GB RAM, 256GB SSD&quot;,&quot;brand&quot;:&quot;Lenovo&quot;,&quot;category&quot;:&quot;Laptops&quot;,&quot;rating&quot;:4.2},{&quot;id&quot;:&quot;10000004&quot;,&quot;title&quot;:&quot;ASUS VivoBook 15 Laptop&quot;,&quot;price&quot;:&quot;649.99&quot;,&quot;picture&quot;:&quot;https://example.com/asus-vivobook.jpg&quot;,&quot;description&quot;:&quot;15.6 inches FHD Display, 8GB RAM, 256GB SSD&quot;,&quot;brand&quot;:&quot;ASUS&quot;,&quot;category&quot;:&quot;Laptops&quot;,&quot;rating&quot;:4.4},{&quot;id&quot;:&quot;10000005&quot;,&quot;title&quot;:&quot;Samsung Galaxy Book Pro Laptop&quot;,&quot;price&quot;:&quot;849.99&quot;,&quot;picture&quot;:&quot;https://example.com/samsung-galaxy-book.jpg&quot;,&quot;description&quot;:&quot;13.3 inches FHD OLED Display, 8GB RAM, 256GB SSD&quot;,&quot;brand&quot;:&quot;Samsung&quot;,&quot;category&quot;:&quot;Laptops&quot;,&quot;rating&quot;:4.6}],&quot;page&quot;:2,&quot;total_results&quot;:50,&quot;total_pages&quot;:25}}\n&lt;/tool_response&gt;&#x27;}]

```
[{&#x27;from&#x27;: &#x27;system&#x27;,
  &#x27;value&#x27;: &#x27;You are a deep thinking AI, you may use extremely long chains of thought to deeply consider the problem and deliberate with yourself via systematic reasoning processes to help come to a correct solution prior to answering. You should enclose your thoughts and internal monologue inside &lt;think&gt; &lt;/think&gt; tags, and then provide your solution or response to the problem.\n\nYou are a function calling AI model. You are provided with function signatures within &lt;tools&gt; &lt;/tools&gt; XML tags. You may call one or more functions to assist with the user query. If available tools are not relevant in assisting with user query, just respond in natural conversational language. Don\&#x27;t make assumptions about what values to plug into functions. After calling &amp; executing the functions, you will be provided with function results within &lt;tool_response&gt; &lt;/tool_response&gt; XML tags. Here are the available tools:\n&lt;tools&gt;\n[\n{&quot;name&quot;: &quot;products_v2_list&quot;, &quot;description&quot;: &quot;Fetches a list of products from the ASOS store with various filtering and sorting options.&quot;, &quot;parameters&quot;: {&quot;type&quot;: &quot;dict&quot;, &quot;properties&quot;: {&quot;store&quot;: {&quot;description&quot;: &quot;The store identifier obtained from the countries/list API.&quot;, &quot;type&quot;: &quot;str&quot;, &quot;default&quot;: &quot;US&quot;}, &quot;offset&quot;: {&quot;description&quot;: &quot;The offset to skip already viewed products.&quot;, &quot;type&quot;: &quot;int&quot;, &quot;default&quot;: &quot;0&quot;}, &quot;categoryid&quot;: {&quot;description&quot;: &quot;The category identifier from the categories/list API.&quot;, &quot;type&quot;: &quot;int&quot;, &quot;default&quot;: &quot;4209&quot;}, &quot;limit&quot;: {&quot;description&quot;: &quot;The number of items per page.&quot;, &quot;type&quot;: &quot;int&quot;, &quot;default&quot;: &quot;48&quot;}, &quot;attribute_1046&quot;: {&quot;description&quot;: &quot;Filter by style, multiple values separated by comma.&quot;, &quot;type&quot;: &quot;str, optional&quot;, &quot;default&quot;: &quot;&quot;}, &quot;pricemin&quot;: {&quot;description&quot;: &quot;Minimum price filter.&quot;, &quot;type&quot;: &quot;int, optional&quot;, &quot;default&quot;: &quot;&quot;}, &quot;country&quot;: {&quot;description&quot;: &quot;Country code; default is \&#x27;US\&#x27;.&quot;, &quot;type&quot;: &quot;str, optional&quot;, &quot;default&quot;: &quot;US&quot;}, &quot;attribute_10147&quot;: {&quot;description&quot;: &quot;Filter by leather/non-leather, multiple values separated by comma.&quot;, &quot;type&quot;: &quot;str, optional&quot;, &quot;default&quot;: &quot;&quot;}, &quot;sort&quot;: {&quot;description&quot;: &quot;Sorting option, one of \&#x27;pricedesc\&#x27;, \&#x27;priceasc\&#x27;, or \&#x27;freshness\&#x27;; default is \&#x27;freshness\&#x27;.&quot;, &quot;type&quot;: &quot;str, optional&quot;, &quot;default&quot;: &quot;freshness&quot;}, &quot;q&quot;: {&quot;description&quot;: &quot;Search query for products by name (do not use with categoryId).&quot;, &quot;type&quot;: &quot;str, optional&quot;, &quot;default&quot;: &quot;&quot;}, &quot;base_colour&quot;: {&quot;description&quot;: &quot;Filter by color, multiple values separated by comma.&quot;, &quot;type&quot;: &quot;str, optional&quot;, &quot;default&quot;: &quot;&quot;}, &quot;range&quot;: {&quot;description&quot;: &quot;Filter by sale/new season, multiple values separated by comma.&quot;, &quot;type&quot;: &quot;str, optional&quot;, &quot;default&quot;: &quot;&quot;}, &quot;attribute_1047&quot;: {&quot;description&quot;: &quot;Filter by product type, multiple values separated by comma.&quot;, &quot;type&quot;: &quot;str, optional&quot;, &quot;default&quot;: &quot;&quot;}, &quot;currency&quot;: {&quot;description&quot;: &quot;Currency code obtained from countries/list API; default is \&#x27;USD\&#x27;.&quot;, &quot;type&quot;: &quot;str, optional&quot;, &quot;default&quot;: &quot;USD&quot;}, &quot;attribute_10155&quot;: {&quot;description&quot;: &quot;Filter by range, multiple values separated by comma.&quot;, &quot;type&quot;: &quot;str, optional&quot;, &quot;default&quot;: &quot;&quot;}, &quot;pricemax&quot;: {&quot;description&quot;: &quot;Maximum price filter.&quot;, &quot;type&quot;: &quot;int, optional&quot;, &quot;default&quot;: &quot;&quot;}, &quot;sizeschema&quot;: {&quot;description&quot;: &quot;Size schema identifier obtained from countries/list API; default is \&#x27;US\&#x27;.&quot;, &quot;type&quot;: &quot;str, optional&quot;, &quot;default&quot;: &quot;US&quot;}, &quot;brand&quot;: {&quot;description&quot;: &quot;Filter by brand, multiple values separated by comma.&quot;, &quot;type&quot;: &quot;str, optional&quot;, &quot;default&quot;: &quot;&quot;}, &quot;size&quot;: {&quot;description&quot;: &quot;Filter by size, multiple values separated by comma.&quot;, &quot;type&quot;: &quot;str, optional&quot;, &quot;default&quot;: &quot;&quot;}, &quot;lang&quot;: {&quot;description&quot;: &quot;Language code; default is \&#x27;en-US\&#x27;.&quot;, &quot;type&quot;: &quot;str, optional&quot;, &quot;default&quot;: &quot;en-US&quot;}}}, &quot;required&quot;: [&quot;store&quot;, &quot;offset&quot;, &quot;categoryid&quot;, &quot;limit&quot;]},\n{&quot;name&quot;: &quot;tmall_search_by_keyword&quot;, &quot;description&quot;: &quot;Searches for products on Tmall by keyword.&quot;, &quot;parameters&quot;: {&quot;type&quot;: &quot;dict&quot;, &quot;properties&quot;: {&quot;page&quot;: {&quot;description&quot;: &quot;The page number of the search results to retrieve.&quot;, &quot;type&quot;: &quot;int&quot;, &quot;default&quot;: &quot;1&quot;}, &quot;keyword&quot;: {&quot;description&quot;: &quot;The keyword to search for products.&quot;, &quot;type&quot;: &quot;str&quot;, &quot;default&quot;: &quot;\\u6d77\\u4e45\\u6c34\\u74f6&quot;}}}, &quot;required&quot;: [&quot;page&quot;, &quot;keyword&quot;]},\n{&quot;name&quot;: &quot;navigations_get_node_content&quot;, &quot;description&quot;: &quot;Fetches the content of a node in a navigation hierarchy.&quot;, &quot;parameters&quot;: {&quot;type&quot;: &quot;dict&quot;, &quot;properties&quot;: {&quot;is_id&quot;: {&quot;description&quot;: &quot;The \&#x27;id\&#x27; field value returned from the /navigations/get-root endpoint.&quot;, &quot;type&quot;: &quot;int&quot;, &quot;default&quot;: &quot;26066300130&quot;}, &quot;cat_id&quot;: {&quot;description&quot;: &quot;The \&#x27;cat_id\&#x27; field value returned from the /navigations/get-tabs endpoint.&quot;, &quot;type&quot;: &quot;int&quot;, &quot;default&quot;: &quot;2026&quot;}, &quot;language&quot;: {&quot;description&quot;: &quot;The 2-letter language code (default is \&#x27;en\&#x27;).&quot;, &quot;type&quot;: &quot;str, optional&quot;, &quot;default&quot;: &quot;en&quot;}, &quot;currency&quot;: {&quot;description&quot;: &quot;The 3-letter currency code (default is \&#x27;USD\&#x27;).&quot;, &quot;type&quot;: &quot;str, optional&quot;, &quot;default&quot;: &quot;USD&quot;}, &quot;country&quot;: {&quot;description&quot;: &quot;The 2-letter country code (default is \&#x27;US\&#x27;).&quot;, &quot;type&quot;: &quot;str, optional&quot;, &quot;default&quot;: &quot;US&quot;}}}, &quot;required&quot;: [&quot;is_id&quot;, &quot;cat_id&quot;]}\n]\n&lt;/tools&gt;\nFor each function call return a JSON object, with the following pydantic model json schema for each:\n{\&#x27;title\&#x27;: \&#x27;FunctionCall\&#x27;, \&#x27;type\&#x27;: \&#x27;object\&#x27;, \&#x27;properties\&#x27;: {\&#x27;name\&#x27;: {\&#x27;title\&#x27;: \&#x27;Name\&#x27;, \&#x27;type\&#x27;:\&#x27;string\&#x27;}, \&#x27;arguments\&#x27;: {\&#x27;title\&#x27;: \&#x27;Arguments\&#x27;, \&#x27;type\&#x27;: \&#x27;object\&#x27;}},\&#x27;required\&#x27;: [\&#x27;name\&#x27;, \&#x27;arguments\&#x27;]}\nEach function call should be enclosed within &lt;tool_call&gt; &lt;/tool_call&gt; XML tags.\nExample:\n&lt;tool_call&gt;\n{\&#x27;name\&#x27;: &lt;function-name&gt;,\&#x27;arguments\&#x27;: &lt;args-dict&gt;}\n&lt;/tool_call&gt;\n\n&#x27;},
 {&#x27;from&#x27;: &#x27;human&#x27;,
  &#x27;value&#x27;: &quot;Search for &#x27;laptops&#x27; on Tmall and retrieve the second page of results.&quot;},
 {&#x27;from&#x27;: &#x27;gpt&#x27;,
  &#x27;value&#x27;: &#x27;&lt;think&gt;\nOkay, the user wants to search for \&#x27;laptops\&#x27; on Tmall and retrieve the second page of results. Let\&#x27;s think about how to do this.\n\nFirst, the available functions are listed. The tmall_search_by_keyword function seems suitable here. Looking at its parameters, I need to provide the page number and the keyword. The keyword here is \&#x27;laptops\&#x27;, and the page is the second one, so page=2.\n\nSo the function call should use tmall_search_by_keyword with page=2 and keyword=\&#x27;laptops\&#x27;. That should give the results for the second page.\n\nWait, does the function return the correct data structure? The user might want the product details, like names, prices, maybe images. The documentation says tmall_search_by_keyword returns a list of products, each with title, price, picture, and other details. So yes, this should work.\n&lt;/think&gt;\n&lt;tool_call&gt;\n{&quot;name&quot;:&quot;tmall_search_by_keyword&quot;,&quot;arguments&quot;:{&quot;page&quot;:2,&quot;keyword&quot;:&quot;laptops&quot;}}\n&lt;/tool_call&gt;&#x27;},
 {&#x27;from&#x27;: &#x27;tool&#x27;,
```

+   多轮对话+工具调用示例

```
ds_full[2][&#x27;conversations&#x27;]
```

```
[{&#x27;from&#x27;: &#x27;system&#x27;,
  &#x27;value&#x27;: &#x27;You are a deep thinking AI, you may use extremely long chains of thought to deeply consider the problem and deliberate with yourself via systematic reasoning processes to help come to a correct solution prior to answering. You should enclose your thoughts and internal monologue inside &lt;think&gt; &lt;/think&gt; tags, and then provide your solution or response to the problem.\n\nYou are a function calling AI model. You are provided with function signatures within &lt;tools&gt; &lt;/tools&gt; XML tags. You may call one or more functions to assist with the user query. If available tools are not relevant in assisting with user query, just respond in natural conversational language. Don\&#x27;t make assumptions about what values to plug into functions. After calling &amp; executing the functions, you will be provided with function results within &lt;tool_response&gt; &lt;/tool_response&gt; XML tags. Here are the available tools:\n&lt;tools&gt;\n[{&quot;name&quot;:&quot;extract&quot;,&quot;description&quot;:&quot;Extracts data from a LinkedIn URL (prostring or company page).&quot;,&quot;parameters&quot;:{&quot;type&quot;:&quot;dict&quot;,&quot;properties&quot;:{&quot;url&quot;:{&quot;description&quot;:&quot;The LinkedIn URL to extract data from&quot;,&quot;type&quot;:&quot;string&quot;},&quot;html&quot;:{&quot;description&quot;:&quot;Whether to output raw HTML in.raw property. Disabled by default.&quot;,&quot;type&quot;:&quot;boolean&quot;,&quot;default&quot;:true}},&quot;required&quot;:[&quot;url&quot;]},&quot;required&quot;:null},{&quot;name&quot;:&quot;get_all_motivational_quotes&quot;,&quot;description&quot;:&quot;This endpoint fetches all motivational quotes. You can do pagination as well using the\&#x27;skip\&#x27; and \&#x27;limit\&#x27; parameters.&quot;,&quot;parameters&quot;:{&quot;type&quot;:&quot;dict&quot;,&quot;properties&quot;:{&quot;skip&quot;:{&quot;description&quot;:&quot;The number of quotes to skip from the beginning of the result set.&quot;,&quot;type&quot;:&quot;float&quot;},&quot;limit&quot;:{&quot;description&quot;:&quot;The maximum number of quotes to return.&quot;,&quot;type&quot;:&quot;float&quot;}},&quot;required&quot;:[]},&quot;required&quot;:null},{&quot;name&quot;:&quot;get_sub_regions&quot;,&quot;description&quot;:&quot;Get a list of sub-regions in the world, filtered by the specified region.&quot;,&quot;parameters&quot;:{&quot;type&quot;:&quot;dict&quot;,&quot;properties&quot;:{&quot;region&quot;:{&quot;description&quot;:&quot;The name of the region to get sub-regions for.&quot;,&quot;type&quot;:&quot;string&quot;},&quot;language&quot;:{&quot;description&quot;:&quot;The language to use for the response.&quot;,&quot;type&quot;:&quot;string&quot;}},&quot;required&quot;:[&quot;region&quot;]},&quot;required&quot;:null},{&quot;name&quot;:&quot;consulta_estabelecimento&quot;,&quot;description&quot;:&quot;Retrieve information about a company (estabelecimento) based on its CNPJ (Corporate Number) from various sources such as Receita Federal, Simples Nacional, and Cadastro de Contribuintes.&quot;,&quot;parameters&quot;:{&quot;type&quot;:&quot;dict&quot;,&quot;properties&quot;:{&quot;CNPJ&quot;:{&quot;description&quot;:&quot;The CNPJ of the company to retrieve information about&quot;,&quot;type&quot;:&quot;string&quot;},&quot;maxAge&quot;:{&quot;description&quot;:&quot;The maximum age of the data in days&quot;,&quot;type&quot;:&quot;float&quot;,&quot;default&quot;:30},&quot;simplesHistory&quot;:{&quot;description&quot;:&quot;Whether to include Simples Nacional history&quot;,&quot;type&quot;:&quot;boolean&quot;,&quot;default&quot;:false},&quot;registrations&quot;:{&quot;description&quot;:&quot;UFs (states) to include in the Cadastro de Contribuintes information, separated by commas. Use \&#x27;BR\&#x27; to include all states.&quot;,&quot;type&quot;:&quot;string&quot;,&quot;default&quot;:&quot;BR&quot;},&quot;simples&quot;:{&quot;description&quot;:&quot;Whether to include Simples Nacional information&quot;,&quot;type&quot;:&quot;boolean&quot;,&quot;default&quot;:true}},&quot;required&quot;:[&quot;CNPJ&quot;]},&quot;required&quot;:null},{&quot;name&quot;:&quot;realtor_school_list&quot;,&quot;description&quot;:&quot;This API returns a list of Realtor schools in a given city, state, and school level. The response includes school details such as name, rating, and reviews.&quot;,&quot;parameters&quot;:{&quot;type&quot;:&quot;dict&quot;,&quot;properties&quot;:{&quot;city&quot;:{&quot;description&quot;:&quot;The city to filter schools by&quot;,&quot;type&quot;:&quot;string&quot;},&quot;state_code&quot;:{&quot;description&quot;:&quot;The state code to filter schools by&quot;,&quot;type&quot;:&quot;string&quot;},&quot;school_level&quot;:{&quot;description&quot;:&quot;The school level to filter schools by (elementary, middle, high, private, charter)&quot;,&quot;type&quot;:&quot;string&quot;},&quot;page&quot;:{&quot;description&quot;:&quot;The page number to retrieve (default: 1)&quot;,&quot;type&quot;:&quot;int&quot;}},&quot;required&quot;:[&quot;city&quot;,&quot;state_code&quot;,&quot;school_level&quot;,&quot;page&quot;]},&quot;required&quot;:null}]\n&lt;/tools&gt;\nFor each function call return a JSON object, with the following pydantic model json schema for each:\n{\&#x27;title\&#x27;: \&#x27;FunctionCall\&#x27;, \&#x27;type\&#x27;: \&#x27;object\&#x27;, \&#x27;properties\&#x27;: {\&#x27;name\&#x27;: {\&#x27;title\&#x27;: \&#x27;Name\&#x27;, \&#x27;type\&#x27;:\&#x27;string\&#x27;}, \&#x27;arguments\&#x27;: {\&#x27;title\&#x27;: \&#x27;Arguments\&#x27;, \&#x27;type\&#x27;: \&#x27;object\&#x27;}},\&#x27;required\&#x27;: [\&#x27;name\&#x27;, \&#x27;arguments\&#x27;]}\nEach function call should be enclosed within &lt;tool_call&gt; &lt;/tool_call&gt; XML tags.\nExample:\n&lt;tool_call&gt;\n{\&#x27;name\&#x27;: &lt;function-name&gt;,\&#x27;arguments\&#x27;: &lt;args-dict&gt;}\n&lt;/tool_call&gt;\n\n&#x27;},
 {&#x27;from&#x27;: &#x27;human&#x27;,
  &#x27;value&#x27;: &#x27;I would like to extract details from a LinkedIn company page. Could you assist me in fetching the information from this URL: https://www.linkedin.com/company/abc-corporation?&#x27;},
 {&#x27;from&#x27;: &#x27;gpt&#x27;,
  &#x27;value&#x27;: &#x27;&lt;think&gt;\nOkay, the user wants to extract details from a LinkedIn company page. The URL provided is https://www.linkedin.com/company/company-name. I need to use the extract tool to get this done. The extract tool has a parameter for the URL and an optional HTML flag. Since the user didn\&#x27;t specify any other parameters, I\&#x27;ll just provide the URL.\n\nWait, the URL passed is a company page, so the extract function should handle that. The result will include things like the company\&#x27;s name, industry, employees, headquarters, etc. The user didn\&#x27;t mention any specific details to extract, so I should just go with the default extraction. The HTML flag is set to true by default, but since they might want the raw data, I should include it in the parameters. But the documentation says it\&#x27;s optional. The extract tool should handle the data extraction from the company page.\n&lt;/think&gt;\n&lt;tool_call&gt;\n{&quot;name&quot;:&quot;extract&quot;,&quot;arguments&quot;:{&quot;url&quot;:&quot;https://www.linkedin.com/company/abc-corporation&quot;}}\n&lt;/tool_call&gt;&#x27;},
 {&#x27;from&#x27;: &#x27;tool&#x27;,
  &#x27;value&#x27;: &#x27;&lt;tool_response&gt;\n{&quot;tool_call_id&quot;: &quot;call_45cde0fe39464c18b23c900c4a2375be&quot;, &quot;name&quot;: &quot;extract&quot;, &quot;content&quot;: {&quot;data&quot;: {&quot;name&quot;: &quot;ABC Corporation&quot;, &quot;industry&quot;: &quot;Information Technology and Services&quot;, &quot;website&quot;: &quot;http://www.abccorporation.com&quot;, &quot;headquarters&quot;: &quot;San Francisco, CA&quot;, &quot;yearFounded&quot;: 1998, &quot;companySize&quot;: &quot;11-50 employees&quot;, &quot;specialties&quot;: [&quot;Cloud computing&quot;, &quot;Software as a Service (SaaS)&quot;, &quot;IT consulting&quot;]}, &quot;raw&quot;: &quot;&lt;!DOCTYPE html&gt;&lt;html&gt;&lt;head&gt;&lt;title&gt;ABC Corporation&lt;/title&gt;&lt;/head&gt;&lt;body&gt;&lt;h1&gt;ABC Corporation&lt;/h1&gt;&lt;p&gt;Industry: Information Technology and Services&lt;/p&gt;&lt;p&gt;Website: http://www.abccorporation.com&lt;/p&gt;&lt;p&gt;Headquarters: San Francisco, CA&lt;/p&gt;&lt;p&gt;Year Founded: 1998&lt;/p&gt;&lt;p&gt;Company Size: 11-50 employees&lt;/p&gt;&lt;p&gt;Specialties: Cloud computing, Software as a Service (SaaS), IT consulting&lt;/p&gt;&lt;/body&gt;&lt;/html&gt;&quot;}}\n&lt;/tool_response&gt;&#x27;},
 {&#x27;from&#x27;: &#x27;gpt&#x27;,
  &#x27;value&#x27;: &quot;&lt;think&gt;\nOkay, the user provided a LinkedIn URL for the ABC Corporation. The extract tool successfully processed it and returned the company&#x27;s details such as name, industry, website, headquarters, year founded, company size, and specialties. Since the user didn&#x27;t ask for raw HTML, I can summarize the key data points without including the HTML.\n&lt;/think&gt;The LinkedIn page for ABC Corporation shows they are an IT services company operating in San Francisco, CA, founded in 1998 with 11-50 employees. Their specialties include cloud computing, Software as a Service (SaaS), and IT consulting. You can find more details on their website at http://www.abccorporation.com.&quot;},
 {&#x27;from&#x27;: &#x27;human&#x27;,
  &#x27;value&#x27;: &#x27;Could you share some motivational quotes that might inspire my team during this business project?&#x27;},
 {&#x27;from&#x27;: &#x27;gpt&#x27;,
  &#x27;value&#x27;: &#x27;&lt;think&gt;\nThe user wants motivational quotes to inspire their team during a business project. I should use the get_all_motivational_quotes tool to fetch some. Since they didn\&#x27;t specify a language, I\&#x27;ll use the default, maybe English. Also, no parameters for skip or limit were given, so using the default pagination. The quotes might be from famous people or general motivational messages that are suitable for a business context. The tool might return a list, so need to select the top few that are inspiring and relevant.\n&lt;/think&gt;\n&lt;tool_call&gt;\n{&quot;name&quot;:&quot;get_all_motivational_quotes&quot;,&quot;arguments&quot;:{}}\n&lt;/tool_call&gt;&#x27;},
 {&#x27;from&#x27;: &#x27;tool&#x27;,
  &#x27;value&#x27;: &#x27;&lt;tool_response&gt;\n{&quot;tool_call_id&quot;: &quot;call_85d0d52000614fb0aa94a4557275c8e8&quot;, &quot;name&quot;: &quot;get_all_motivational_quotes&quot;, &quot;content&quot;: {&quot;quotes&quot;: [{&quot;text&quot;: &quot;The only way to do great work is to love what you do.&quot;, &quot;author&quot;: &quot;Steve Jobs&quot;, &quot;id&quot;: 1}, {&quot;text&quot;: &quot;Believe you can and you\&#x27;re halfway there.&quot;, &quot;author&quot;: &quot;Theodore Roosevelt&quot;, &quot;id&quot;: 2}, {&quot;text&quot;: &quot;It always seems impossible until it\&#x27;s done.&quot;, &quot;author&quot;: &quot;Nelson Mandela&quot;, &quot;id&quot;: 3}, {&quot;text&quot;: &quot;You are never too old to set another goal or to dream a new dream.&quot;, &quot;author&quot;: &quot;C.S. Lewis&quot;, &quot;id&quot;: 4}]}}\n&lt;/tool_response&gt;&#x27;},
 {&#x27;from&#x27;: &#x27;gpt&#x27;,
  &#x27;value&#x27;: &#x27;Here are some motivational quotes to inspire your team:\n1. **Steve Jobs:** &quot;The only way to do great work is to love what you do.&quot;  \n2. **Theodore Roosevelt:** &quot;Believe you can and you\&#x27;re halfway there.&quot;  \n3. **Nelson Mandela:** &quot;It always seems impossible until it\&#x27;s done.&quot;  \n4. **C.S. Lewis:** &quot;You are never too old to set another goal or to dream a new dream.&quot;&#x27;}]
```

+   无干预下多轮工具调用示例

```
ds_full[4][&#x27;conversations&#x27;]
```

```
[{&#x27;from&#x27;: &#x27;system&#x27;,
  &#x27;value&#x27;: &#x27;You are a deep thinking AI, you may use extremely long chains of thought to deeply consider the problem and deliberate with yourself via systematic reasoning processes to help come to a correct solution prior to answering. You should enclose your thoughts and internal monologue inside &lt;think&gt; &lt;/think&gt; tags, and then provide your solution or response to the problem.\n\nYou are a function calling AI model. You are provided with function signatures within &lt;tools&gt; &lt;/tools&gt; XML tags. You may call one or more functions to assist with the user query. If available tools are not relevant in assisting with user query, just respond in natural conversational language. Don\&#x27;t make assumptions about what values to plug into functions. After calling &amp; executing the functions, you will be provided with function results within &lt;tool_response&gt; &lt;/tool_response&gt; XML tags. Here are the available tools:\n&lt;tools&gt;\n[{&quot;name&quot;:&quot;get_language_proficiency&quot;,&quot;description&quot;:&quot;Retrieve language proficiency information&quot;,&quot;parameters&quot;:{&quot;type&quot;:&quot;dict&quot;,&quot;properties&quot;:{&quot;language&quot;:{&quot;type&quot;:&quot;string&quot;,&quot;description&quot;:&quot;The language for which you want to get the proficiency (e.g., English, Spanish, French)&quot;},&quot;level&quot;:{&quot;type&quot;:&quot;string&quot;,&quot;description&quot;:&quot;The desired level of proficiency (e.g., Beginner, Intermediate, Advanced)&quot;}},&quot;required&quot;:[&quot;language&quot;]},&quot;required&quot;:null},{&quot;name&quot;:&quot;get_language_translation&quot;,&quot;description&quot;:&quot;Translate a given text to another language&quot;,&quot;parameters&quot;:{&quot;type&quot;:&quot;dict&quot;,&quot;properties&quot;:{&quot;text&quot;:{&quot;type&quot;:&quot;string&quot;,&quot;description&quot;:&quot;The text to be translated&quot;},&quot;source_language&quot;:{&quot;type&quot;:&quot;string&quot;,&quot;description&quot;:&quot;The source language of the text (e.g., English, Spanish, French)&quot;},&quot;target_language&quot;:{&quot;type&quot;:&quot;string&quot;,&quot;description&quot;:&quot;The target language for translation (e.g., English, Spanish, French)&quot;}},&quot;required&quot;:[&quot;text&quot;,&quot;source_language&quot;,&quot;target_language&quot;]},&quot;required&quot;:null}]\n&lt;/tools&gt;\nFor each function call return a JSON object, with the following pydantic model json schema for each:\n{\&#x27;title\&#x27;: \&#x27;FunctionCall\&#x27;, \&#x27;type\&#x27;: \&#x27;object\&#x27;, \&#x27;properties\&#x27;: {\&#x27;name\&#x27;: {\&#x27;title\&#x27;: \&#x27;Name\&#x27;, \&#x27;type\&#x27;:\&#x27;string\&#x27;}, \&#x27;arguments\&#x27;: {\&#x27;title\&#x27;: \&#x27;Arguments\&#x27;, \&#x27;type\&#x27;: \&#x27;object\&#x27;}},\&#x27;required\&#x27;: [\&#x27;name\&#x27;, \&#x27;arguments\&#x27;]}\nEach function call should be enclosed within &lt;tool_call&gt; &lt;/tool_call&gt; XML tags.\nExample:\n&lt;tool_call&gt;\n{\&#x27;name\&#x27;: &lt;function-name&gt;,\&#x27;arguments\&#x27;: &lt;args-dict&gt;}\n&lt;/tool_call&gt;\n\n&#x27;},
 {&#x27;from&#x27;: &#x27;human&#x27;,
  &#x27;value&#x27;: &#x27;I\&#x27;d like to learn Spanish and need to understand my current proficiency level. Also, once I know my level, I need a translation of the following text: &quot;I want to learn Spanish quickly&quot; into Spanish.\n\n&#x27;},
 {&#x27;from&#x27;: &#x27;gpt&#x27;,
  &#x27;value&#x27;: &#x27;&lt;think&gt;\nOkay, the user wants to know their Spanish language proficiency and also needs a translation of a text into Spanish. Let\&#x27;s start with the first part: getting their proficiency level. The get_language_proficiency tool requires specifying the language and level. The user specified Spanish, but they didn\&#x27;t mention their current level. Hmm, maybe I should assume the default for unknown users which is &quot;Beginner&quot;. But wait, the user said they want to learn Spanish, so maybe they are a beginner. Alternatively, maybe the tool allows for the user to input their current level. Let me check the tool parameters. The function requires both &quot;language&quot; and &quot;level&quot; as required parameters. Since the user hasn\&#x27;t specified a level, perhaps I should default it to &quot;Beginner&quot; as a safe assumption.\n\nOnce we have the proficiency level, the user wants the translation of &quot;I want to learn Spanish quickly&quot; into Spanish. The translation tool requires source and target languages. The source is English, target is Spanish. So I\&#x27;ll call get_language_translation with those parameters.\n\nSo first step: get_language_proficiency with language=&quot;Spanish&quot; and level=&quot;Beginner&quot;. Then, get_language_translation with text=&quot;I want to learn Spanish quickly&quot;, source_language=&quot;English&quot;, target_language=&quot;Spanish&quot;.\n\nAfter getting the responses, I\&#x27;ll output the proficiency level and the translated text.\n&lt;/think&gt;\n&lt;tool_call&gt;\n{&quot;name&quot;:&quot;get_language_proficiency&quot;,&quot;arguments&quot;:{&quot;language&quot;:&quot;Spanish&quot;,&quot;level&quot;:&quot;Beginner&quot;}}\n&lt;/tool_call&gt;&#x27;},
 {&#x27;from&#x27;: &#x27;tool&#x27;,
  &#x27;value&#x27;: &#x27;&lt;tool_response&gt;\n{&quot;tool_call_id&quot;: &quot;call_27ce8bdc10d34f5cbcdda87903bed07a&quot;, &quot;name&quot;: &quot;get_language_proficiency&quot;, &quot;content&quot;: {&quot;proficiency&quot;: &quot;Beginner&quot;}}\n&lt;/tool_response&gt;&#x27;},
 {&#x27;from&#x27;: &#x27;gpt&#x27;,
  &#x27;value&#x27;: &#x27;&lt;think&gt;\nOkay, the first tool response gave the Spanish proficiency level as Beginner. Now, the user wants the text &quot;I want to learn Spanish quickly&quot; translated into Spanish. Using the get_language_translation tool, the source language is English and target is Spanish. The text will be translated accordingly.\n\nI\&#x27;ll call the translation tool with the given parameters.\n&lt;/think&gt;\n&lt;tool_call&gt;\n{&quot;name&quot;:&quot;get_language_translation&quot;,&quot;arguments&quot;:{&quot;text&quot;:&quot;I want to learn Spanish quickly&quot;,&quot;source_language&quot;:&quot;English&quot;,&quot;target_language&quot;:&quot;Spanish&quot;}}\n&lt;/tool_call&gt;&#x27;},
 {&#x27;from&#x27;: &#x27;tool&#x27;,
  &#x27;value&#x27;: &#x27;&lt;tool_response&gt;\n{&quot;tool_call_id&quot;: &quot;call_fb5f7d2466414a0184b5207edfb63e2c&quot;, &quot;name&quot;: &quot;get_language_translation&quot;, &quot;content&quot;: {&quot;translation&quot;: &quot;Quiero aprender espa\\u00f1ol r\\u00e1pidamente&quot;}}\n&lt;/tool_response&gt;&#x27;},
 {&#x27;from&#x27;: &#x27;gpt&#x27;,
  &#x27;value&#x27;: &#x27;&lt;think&gt;\nOkay, the user wants their Spanish proficiency and the translation of &quot;I want to learn Spanish quickly&quot; into Spanish. The first tool response gave their proficiency as Beginner, and the second tool translated the text to &quot;Quiero aprender español rápidamente&quot;. That should cover both parts of their request.\n&lt;/think&gt;Your current Spanish proficiency level is **Beginner**. The translated text is: **&quot;Quiero aprender español rápidamente&quot;** (I want to learn Spanish quickly).&#x27;},
 {&#x27;from&#x27;: &#x27;gpt&#x27;,
  &#x27;value&#x27;: &#x27;&lt;think&gt;\nThe user has received the answers to their two questions: their Spanish proficiency level (Beginner) and the translation of their text. The task is complete.\n&lt;/think&gt;Your Spanish proficiency level is assessed as Beginner, and the translated text is &quot;Quiero aprender español rápidamente&quot;. If you have any further questions or need additional assistance, please let me know!&#x27;}]
```

### 2. Hermes tool use数据集清洗

```
import re, json, ast
from copy import deepcopy
from datasets import load_dataset
```

```
# ========= 1) 类型映射：把简写类型转为 JSON Schema =========
def _map_type(t: str):
    t = (t or &quot;&quot;).strip()
    if t.lower() in (&quot;str&quot;, &quot;string&quot;):
        return {&quot;type&quot;: &quot;string&quot;}
    if t.lower() in (&quot;int&quot;, &quot;integer&quot;):
        return {&quot;type&quot;: &quot;integer&quot;}
    if t.lower() in (&quot;float&quot;, &quot;number&quot;):
        return {&quot;type&quot;: &quot;number&quot;}
    if t.lower() in (&quot;bool&quot;, &quot;boolean&quot;):
        return {&quot;type&quot;: &quot;boolean&quot;}
    if t.startswith(&quot;List[&quot;) and t.endswith(&quot;]&quot;):
        inner = t[5:-1].strip()
        inner_schema = _map_type(inner)
        return {&quot;type&quot;: &quot;array&quot;, &quot;items&quot;: {&quot;type&quot;: inner_schema.get(&quot;type&quot;, &quot;string&quot;)}}
    # 兜底
    return {&quot;type&quot;: &quot;string&quot;}
```

```
import json, ast, re

# 1) 兜底的类型映射
def _map_base_type(t: str) -&gt; str:
    t = (t or &quot;string&quot;)
    if isinstance(t, str):
        t = t.strip().lower()
    else:
        t = str(t).strip().lower()
    # 去掉 like &quot;bool, optional&quot;
    t = t.split(&quot;,&quot;)[0].strip()
    MAP = {
        &quot;str&quot;: &quot;string&quot;, &quot;string&quot;: &quot;string&quot;,
        &quot;int&quot;: &quot;integer&quot;, &quot;integer&quot;: &quot;integer&quot;,
        &quot;float&quot;: &quot;number&quot;, &quot;number&quot;: &quot;number&quot;,
        &quot;bool&quot;: &quot;boolean&quot;, &quot;boolean&quot;: &quot;boolean&quot;,
        &quot;dict&quot;: &quot;object&quot;, &quot;object&quot;: &quot;object&quot;,
        &quot;array&quot;: &quot;array&quot;, &quot;list&quot;: &quot;array&quot;,
    }
    return MAP.get(t, &quot;string&quot;)

# 2) 将各种 parameters 归一化为 JSON Schema
def _normalize_parameters_to_jsonschema(params) -&gt; dict:
    if not isinstance(params, dict):
        return {&quot;type&quot;: &quot;object&quot;, &quot;properties&quot;: {}}

    if params.get(&quot;type&quot;) in (&quot;object&quot;, &quot;dict&quot;) and &quot;properties&quot; in params:
        schema = dict(params)
        if schema[&quot;type&quot;] == &quot;dict&quot;:
            schema[&quot;type&quot;] = &quot;object&quot;
        props = {}
        for p, spec in (schema.get(&quot;properties&quot;) or {}).items():
            if not isinstance(spec, dict):
                spec = {}
            base = _map_base_type(spec.get(&quot;type&quot;, &quot;string&quot;))
            props[p] = {
                &quot;type&quot;: base,
                &quot;description&quot;: spec.get(&quot;description&quot;, &quot;&quot;),  # ← 必有描述
            }
        req = schema.get(&quot;required&quot;)
        if not isinstance(req, list):
            req = list(props.keys())
        return {&quot;type&quot;: &quot;object&quot;, &quot;properties&quot;: props, &quot;required&quot;: req}

    # 简写 -&gt; 正规化
    props, required = {}, []
    for p, spec in (params.items() if isinstance(params, dict) else []):
        if not isinstance(spec, dict):
            spec = {}
        base = _map_base_type(spec.get(&quot;type&quot;, &quot;string&quot;))
        props[p] = {
            &quot;type&quot;: base,
            &quot;description&quot;: spec.get(&quot;description&quot;, &quot;&quot;),  # ← 必有描述
        }
        required.append(p)
    return {&quot;type&quot;: &quot;object&quot;, &quot;properties&quot;: props, &quot;required&quot;: required}

# 3) Hermes tools → gpt-oss tools（超稳健版）
def hermes_tools_to_gptoss_tools(hermes_tools):
    out = []
    if not hermes_tools:
        return out

    for t in hermes_tools:
        if isinstance(t, str):
            try:
                t = json.loads(t)
            except Exception:
                try:
                    import ast
                    t = ast.literal_eval(t)
                except Exception:
                    continue
        if not isinstance(t, dict):
            continue

        fn = t[&quot;function&quot;] if (t.get(&quot;type&quot;) == &quot;function&quot; and isinstance(t.get(&quot;function&quot;), dict)) else t
        name = (fn.get(&quot;name&quot;) or &quot;&quot;).strip()
        if not name:
            continue

        desc = fn.get(&quot;description&quot;) or &quot;&quot;  # ← 必有 description
        params = fn.get(&quot;parameters&quot;) or {}
        schema = _normalize_parameters_to_jsonschema(params)

        # ---- 补齐 properties 里的 type/description ----
        if schema.get(&quot;type&quot;) == &quot;object&quot; and isinstance(schema.get(&quot;properties&quot;), dict):
            for p, spec in list(schema[&quot;properties&quot;].items()):
                if not isinstance(spec, dict):
                    spec = {}
                spec.setdefault(&quot;type&quot;, &quot;string&quot;)
                spec.setdefault(&quot;description&quot;, &quot;&quot;)
                schema[&quot;properties&quot;][p] = spec
        # ---------------------------------------------

        out.append({
            &quot;type&quot;: &quot;function&quot;,
            &quot;function&quot;: {
                &quot;name&quot;: name,
                &quot;description&quot;: desc,
                &quot;parameters&quot;: schema,
            },
        })
    return out
```

```
# ========= 3) 从 system 文本里提取/去掉 &lt;tools&gt;... 块 =========
_TOOLS_BLOCK_RE = re.compile(r&quot;&lt;tools&gt;(.*?)&lt;/tools&gt;&quot;, re.DOTALL)

def strip_tools_block(text: str) -&gt; str:
    if not text:
        return text
    return _TOOLS_BLOCK_RE.sub(&quot;&quot;, text).strip()

def extract_tools_from_system(text: str):
    &quot;&quot;&quot;尽力从 &lt;tools&gt;...&lt;/tools&gt; 中解析出 Python 列表或 JSON 列表；失败则返回 [].&quot;&quot;&quot;
    if not text:
        return []
    m = _TOOLS_BLOCK_RE.search(text)
    if not m:
        return []
    raw = m.group(1).strip()
    # 尝试 JSON
    try:
        return json.loads(raw)
    except Exception:
        pass
    # 尝试 Python 字面量（Hermes 常用单引号）
    try:
        val = ast.literal_eval(raw)
        return val if isinstance(val, list) else []
    except Exception:
        return []
```

```
# ========= 4) 解析 assistant 的 &lt;think&gt; 与 &lt;tool_call&gt; =========
_THINK_RE = re.compile(r&quot;&lt;think&gt;(.*?)&lt;/think&gt;&quot;, re.DOTALL)
_TOOLCALL_RE = re.compile(r&quot;&lt;tool_call&gt;\s*(\{.*?\})\s*&lt;/tool_call&gt;&quot;, re.DOTALL)

def parse_assistant_content(value: str):
    &quot;&quot;&quot;返回 (thinking:str|None, tool_calls:list[{&#x27;function&#x27;:{&#x27;name&#x27;, &#x27;arguments&#x27;}}])&quot;&quot;&quot;
    if not value:
        return None, []
    thinking = None
    m = _THINK_RE.search(value)
    if m:
        thinking = m.group(1).strip()

    calls = []
    for m in _TOOLCALL_RE.finditer(value):
        blob = m.group(1)
        try:
            obj = json.loads(blob)
        except Exception:
            # 单引号风格
            try:
                obj = ast.literal_eval(blob)
            except Exception:
                continue
        name = obj.get(&quot;name&quot;)
        args = obj.get(&quot;arguments&quot;, {})
        if not isinstance(args, dict):
            # 再试一次解析
            try:
                args = json.loads(args) if isinstance(args, str) else {}
            except Exception:
                args = {}
        if name:
            calls.append({&quot;function&quot;: {&quot;name&quot;: name, &quot;arguments&quot;: args}})
    return thinking, calls
```

```
# ========= 5) 解析 tool 的 &lt;tool_response&gt; JSON 内容为字符串 =========
_TOOLRESP_RE = re.compile(r&quot;&lt;tool_response&gt;\s*(\{.*?\})\s*&lt;/tool_response&gt;&quot;, re.DOTALL)

def parse_tool_response_value(value: str) -&gt; str:
    &quot;&quot;&quot;
    输入 Hermes 的 tool.value，输出一个 JSON 字符串（尽量规范化）。
    &quot;&quot;&quot;
    if not value:
        return &quot;&quot;
    m = _TOOLRESP_RE.search(value)
    raw = (m.group(1).strip() if m else value.strip())
    # 尝试 JSON
    try:
        obj = json.loads(raw)
        return json.dumps(obj, ensure_ascii=False)
    except Exception:
        pass
    # 尝试 Python 字面量
    try:
        obj = ast.literal_eval(raw)
        return json.dumps(obj, ensure_ascii=False)
    except Exception:
        # 留原文（让模板做 tojson），至少是字符串
        return raw
```

```
DEV_NOISE_PATTERNS = [
    r&quot;Here are the available tools:.*&quot;,
    r&quot;Use the following pydantic model.*&quot;,
    r&quot;Each function call should be enclosed.*&quot;,
    r&quot;You are a function calling AI model\..*&quot;,
    r&quot;within\s*&lt;tools&gt;\s*&lt;/tools&gt;\s*XML tags.*&quot;,
]
DEV_NOISE_RE = re.compile(&quot;|&quot;.join(DEV_NOISE_PATTERNS), re.DOTALL)

def clean_developer_text(s: str) -&gt; str:
    s = _TOOLS_BLOCK_RE.sub(&quot;&quot;, s or &quot;&quot;)  # 先去掉 &lt;tools&gt;…&lt;/tools&gt;
    s = DEV_NOISE_RE.sub(&quot;&quot;, s).strip()
    s = re.sub(r&quot;\n{3,}&quot;, &quot;\n\n&quot;, s)
    return s
```

```
import re

# 原有噪音模式
DEV_NOISE_PATTERNS = [
    r&quot;Here are the available tools:.*&quot;,
    r&quot;Use the following pydantic model.*&quot;,
    r&quot;Each function call should be enclosed.*&quot;,
    r&quot;You are a function calling AI model\..*&quot;,
    r&quot;within\s*&lt;tools&gt;\s*&lt;/tools&gt;\s*XML tags.*&quot;,
    # 新增：清理 &lt;think&gt; 相关说明
    r&quot;You should enclose.*?&lt;think&gt;.*?&lt;/think&gt;.*?(solution|response).*&quot;,
    r&quot;enclose your thoughts.*?&lt;think&gt;.*?&lt;/think&gt;.*&quot;,
    r&quot;internal monologue.*?&lt;think&gt;.*?&lt;/think&gt;.*&quot;,
]
DEV_NOISE_RE = re.compile(&quot;|&quot;.join(DEV_NOISE_PATTERNS), re.DOTALL | re.IGNORECASE)

def final_clean_developer_text(s: str) -&gt; str:
    &quot;&quot;&quot;
    清理 Hermes system 提示，去掉工具声明、pydantic schema、&lt;think&gt; 标签说明，
    只保留干净的通用 developer 提示。
    &quot;&quot;&quot;
    if not s:
        return &quot;&quot;
    # 先去掉 &lt;tools&gt;...&lt;/tools&gt; 块
    s = _TOOLS_BLOCK_RE.sub(&quot;&quot;, s)
    # 去掉定义的噪音段落
    s = DEV_NOISE_RE.sub(&quot;&quot;, s).strip()
    # 压缩多余空行
    s = re.sub(r&quot;\n{3,}&quot;, &quot;\n\n&quot;, s)
    return s
```

```
import re, json

_UNPAIRED_THINK_OPEN  = re.compile(r&quot;&lt;think&gt;\s*&quot;, re.IGNORECASE)
_UNPAIRED_THINK_CLOSE = re.compile(r&quot;\s*&lt;/think&gt;&quot;, re.IGNORECASE)

def _strip_unpaired_think_tags(text: str) -&gt; str:
    if not isinstance(text, str):
        return &quot;&quot; if text is None else str(text)
    # 去掉单独出现（或未配对）的 &lt;think&gt; / &lt;/think&gt; 标签
    text = _UNPAIRED_THINK_OPEN.sub(&quot;&quot;, text)
    text = _UNPAIRED_THINK_CLOSE.sub(&quot;&quot;, text)
    return text

def _drop_none_fields(msg: dict) -&gt; dict:
    # 删掉值为 None 的键
    return {k: v for k, v in msg.items() if v is not None}

def sanitize_messages_for_gptoss(messages: list) -&gt; list:
    cleaned = []
    for m in messages:
        m = dict(m)  # 复制一份
        role = m.get(&quot;role&quot;)

        # 1) 去掉 thinking=None
        if &quot;thinking&quot; in m and (m[&quot;thinking&quot;] is None or m[&quot;thinking&quot;] == &quot;&quot;):
            m.pop(&quot;thinking&quot;, None)

        # 2) 对 content 做兜底清洗（移除残留 think 标签）
        if &quot;content&quot; in m:
            m[&quot;content&quot;] = _strip_unpaired_think_tags(m[&quot;content&quot;])

        # 3) tool 消息的 content 必须是字符串
        if role == &quot;tool&quot;:
            c = m.get(&quot;content&quot;, &quot;&quot;)
            if not isinstance(c, str):
                try:
                    m[&quot;content&quot;] = json.dumps(c, ensure_ascii=False)
                except Exception:
                    m[&quot;content&quot;] = str(c)

        # 4) assistant+tool_calls 情况：不能带 content
        if role == &quot;assistant&quot; and &quot;tool_calls&quot; in m and m.get(&quot;tool_calls&quot;):
            m.pop(&quot;content&quot;, None)  # 保守起见清掉

        # 5) 删除 None 字段
        m = _drop_none_fields(m)

        # 6) 丢弃完全空的消息（极少数脏数据）
        if role and (m.get(&quot;content&quot;) or m.get(&quot;tool_calls&quot;) or m.get(&quot;thinking&quot;) or role == &quot;tool&quot;):
            cleaned.append(m)

    return cleaned
```

```
# ========= 6) 单条样本转换：Hermes → gpt-oss (messages + tools) =========
def hermes_to_gptoss_example(ex: dict):
    conv = ex.get(&quot;conversations&quot;) or []
    top_tools_raw = ex.get(&quot;tools&quot;)  # 可能是 str / list[str] / list[dict] / dict / None

    messages = []
    idx = 0
    sys_text = &quot;&quot;

    if len(conv) and conv[0].get(&quot;from&quot;) == &quot;system&quot;:
        sys_text = conv[0].get(&quot;value&quot;, &quot;&quot;) or &quot;&quot;
        # 若顶层无 tools，从 system 的 &lt;tools&gt;... 抽
        if not top_tools_raw:
            extracted = extract_tools_from_system(sys_text)
            if extracted:
                top_tools_raw = extracted

        dev_text = final_clean_developer_text(sys_text)
        if dev_text.strip():
            messages.append({&quot;role&quot;: &quot;developer&quot;, &quot;content&quot;: dev_text.strip()})
        idx = 1

    # 1) 工具归一化（你已有）
    top_tools_norm = parse_tools_any(top_tools_raw)
    gptoss_tools = hermes_tools_to_gptoss_tools(top_tools_norm or [])

    # 2) 主循环（加一点点健壮性）
    for turn in conv[idx:]:
        who = (turn.get(&quot;from&quot;) or &quot;&quot;).lower()
        val = turn.get(&quot;value&quot;, &quot;&quot;)
        if not isinstance(val, str):  # 防御：确保是字符串
            val = str(val)

        if who in (&quot;human&quot;, &quot;user&quot;):
            if val.strip():
                messages.append({&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: val})

        elif who in (&quot;gpt&quot;, &quot;assistant&quot;):
            thinking, calls = parse_assistant_content(val)
            if calls:
                for j, call in enumerate(calls):
                    msg = {&quot;role&quot;: &quot;assistant&quot;, &quot;tool_calls&quot;: [call]}
                    if j == 0 and thinking:   # 只有第一条带 thinking
                        msg[&quot;thinking&quot;] = thinking
                    messages.append(msg)
            else:
                # 非工具调用回合
                content = val
                if thinking is not None:
                    content = _THINK_RE.sub(&quot;&quot;, val).strip()
                messages.append(
                    {&quot;role&quot;: &quot;assistant&quot;, **({&quot;thinking&quot;: thinking} if thinking else {}), &quot;content&quot;: content}
                )

        elif who == &quot;tool&quot;:
            tool_payload = parse_tool_response_value(val)  # -&gt; JSON 字符串
            messages.append({&quot;role&quot;: &quot;tool&quot;, &quot;content&quot;: tool_payload})

    # 3) 收尾清理（你已有）
    messages = sanitize_messages_for_gptoss(messages)
    return {&quot;messages&quot;: messages, &quot;tools&quot;: gptoss_tools}
```

```
# ========= 7) 批量映射 + 渲染成 text（用于 SFT） =========
def convert_hermes_dataset_to_gptoss_text(ds, tokenizer, reasoning_effort=&quot;low&quot;):
    &quot;&quot;&quot;
    输入：datasets.Dataset（Hermes），以及已加载好的 gpt-oss tokenizer
    输出：新的 Dataset，包含：
      - messages（供调试）
      - tools_obj（供调试）
      - text（喂给 SFTTrainer 的列）
    &quot;&quot;&quot;
    def map_one(ex):
        r = hermes_to_gptoss_example(ex)
        return {&quot;messages&quot;: r[&quot;messages&quot;], &quot;tools_obj&quot;: r[&quot;tools&quot;]}

    ds2 = ds.map(map_one)

    def render_batch(batch):
        out = []
        for msgs, tools in zip(batch[&quot;messages&quot;], batch[&quot;tools_obj&quot;]):
            txt = tokenizer.apply_chat_template(
                msgs,
                tools=tools,                   # ← 关键：工具通过参数传入
                add_generation_prompt=False,   # 训练集通常 False
                reasoning_effort=reasoning_effort,  # &quot;low&quot;/&quot;medium&quot;/&quot;high&quot;
                tokenize=False,
            )
            out.append(txt)
        return {&quot;text&quot;: out}

    ds3 = ds2.map(render_batch, batched=True)
    return ds3
```

```
import json, ast

def parse_tools_any(obj):
    &quot;&quot;&quot;
    把 Hermes 的 tools 字段归一化为 [dict, ...]：
    - 支持 str（JSON 或 Python 字面量，含单引号）
    - 支持 dict（单个工具，转成 [dict]）
    - 支持 list（其中元素可为 str 或 dict）
    - 其他类型返回 []
    &quot;&quot;&quot;
    if obj is None:
        return []

    # 纯字符串：尝试 json -&gt; ast
    if isinstance(obj, str):
        s = obj.strip()
        if not s:
            return []
        for loader in (json.loads, ast.literal_eval):
            try:
                val = loader(s)
                return parse_tools_any(val)  # 递归归一化
            except Exception:
                pass
        return []  # 实在解析不了就空

    # 字典：包成单元素列表
    if isinstance(obj, dict):
        return [obj]

    # 列表：逐个元素解析
    if isinstance(obj, list):
        out = []
        for it in obj:
            if isinstance(it, dict):
                out.append(it)
            elif isinstance(it, str):
                it_s = it.strip()
                ok = None
                for loader in (json.loads, ast.literal_eval):
                    try:
                        val = loader(it_s)
                        ok = val
                        break
                    except Exception:
                        pass
                if isinstance(ok, dict):
                    out.append(ok)
                elif isinstance(ok, list):
                    out.extend(parse_tools_any(ok))
                # 其他情况丢弃这个元素
        return out

    # 其他类型丢弃
    return []
```

定义好了数据清洗函数之后，接下来测试能否将原始数据转化为gpt-oss模型能够用于训练的文本格式：

```
ds_full
```

```
Dataset({
    features: [&#x27;conversations&#x27;, &#x27;tools&#x27;, &#x27;task&#x27;, &#x27;category&#x27;, &#x27;source&#x27;, &#x27;scenario_category&#x27;],
    num_rows: 51004
})
```

```
# 2) 随便取一条（比如第 0 条）
sample = ds_full[0]
print(&quot;原始 Hermes 数据：&quot;)
print(sample)
```

```
原始 Hermes 数据：
{&#x27;conversations&#x27;: [{&#x27;from&#x27;: &#x27;system&#x27;, &#x27;value&#x27;: &#x27;You are a deep thinking AI, you may use extremely long chains of thought to deeply consider the problem and deliberate with yourself via systematic reasoning processes to help come to a correct solution prior to answering. You should enclose your thoughts and internal monologue inside &lt;think&gt; &lt;/think&gt; tags, and then provide your solution or response to the problem.\n\nYou are a function calling AI model. You are provided with function signatures within &lt;tools&gt; &lt;/tools&gt; XML tags. You may call one or more functions to assist with the user query. Don\&#x27;t make assumptions about what values to plug into functions. Here are the available tools: &lt;tools&gt; [{&quot;name&quot;: &quot;calculate_age&quot;, &quot;description&quot;: &quot;Calculates the age based on the birthdate.&quot;, &quot;parameters&quot;: {&quot;birthdate&quot;: {&quot;description&quot;: &quot;The birthdate in the format \&#x27;YYYY-MM-DD\&#x27;.&quot;, &quot;type&quot;: &quot;str&quot;}}}, {&quot;name&quot;: &quot;find_duplicate_number&quot;, &quot;description&quot;: &quot;Finds the duplicate number in a list of integers.&quot;, &quot;parameters&quot;: {&quot;nums&quot;: {&quot;description&quot;: &quot;The list of integers.&quot;, &quot;type&quot;: &quot;List[int]&quot;}}}, {&quot;name&quot;: &quot;word_frequency&quot;, &quot;description&quot;: &quot;Calculates the frequency of each word in a text.&quot;, &quot;parameters&quot;: {&quot;text&quot;: {&quot;description&quot;: &quot;The input text.&quot;, &quot;type&quot;: &quot;str&quot;}}}, {&quot;name&quot;: &quot;reverse_words&quot;, &quot;description&quot;: &quot;Reverses the order of words in a sentence.&quot;, &quot;parameters&quot;: {&quot;sentence&quot;: {&quot;description&quot;: &quot;The input sentence.&quot;, &quot;type&quot;: &quot;str&quot;}}}] &lt;/tools&gt; Use the following pydantic model JSON schema for each tool call you will make: {\&#x27;properties\&#x27;: {\&#x27;arguments\&#x27;: {\&#x27;title\&#x27;: \&#x27;Arguments\&#x27;, \&#x27;type\&#x27;: \&#x27;object\&#x27;}, \&#x27;name\&#x27;: {\&#x27;title\&#x27;: \&#x27;Name\&#x27;, \&#x27;type\&#x27;:\&#x27;string\&#x27;}},\&#x27;required\&#x27;: [\&#x27;arguments\&#x27;, \&#x27;name\&#x27;], \&#x27;title\&#x27;: \&#x27;FunctionCall\&#x27;, \&#x27;type\&#x27;: \&#x27;object\&#x27;} For each function call return a json object with function name and arguments within &lt;tool_call&gt; &lt;/tool_call&gt; XML tags as follows:\n&lt;tool_call&gt;\n{tool_call}\n&lt;/tool_call&gt;\n\n&#x27;}, {&#x27;from&#x27;: &#x27;human&#x27;, &#x27;value&#x27;: &quot;What is the word frequency of &#x27;The quick brown fox jumps over the lazy dog&#x27;? And can you reverse the words in the sentence &#x27;Hello world this is a test&#x27;?&quot;}, {&#x27;from&#x27;: &#x27;gpt&#x27;, &#x27;value&#x27;: &#x27;&lt;think&gt;\nOkay, the user has two separate questions here. The first is to find the word frequency of the sentence &quot;The quick brown fox jumps over the lazy dog&quot;. The second is to reverse the words in the sentence &quot;Hello world this is a test&quot;. Let me break down each part step by step.\n\nStarting with the word frequency: I need to use the word_frequency tool. The input text is given as a string, so I\&#x27;ll pass that into the tool. That should return a dictionary where each key is a word and the value is its frequency.\n\nNext, the reverse the sentence part. The user wants the words reversed. That\&#x27;s a straightforward reverse of words in the sentence. The reverse_words tool should handle this. The sentence is &quot;Hello world this is a test&quot;, so I\&#x27;ll pass that into the reverse_words function, which will return the sentence with words reversed.\n\nSo I need to make two separate function calls: one for word_frequency and another for reverse_words. I\&#x27;ll use the word_frequency tool with the sentence as the text parameter, and reverse_words with the same sentence. The outputs from each tool will be returned as JSON objects.\n&lt;/think&gt;\n&lt;tool_call&gt;\n{&quot;name&quot;:&quot;word_frequency&quot;,&quot;arguments&quot;:{&quot;text&quot;:&quot;The quick brown fox jumps over the lazy dog&quot;}}\n&lt;/tool_call&gt;\n&lt;tool_call&gt;\n{&quot;name&quot;:&quot;reverse_words&quot;,&quot;arguments&quot;:{&quot;sentence&quot;:&quot;Hello world this is a test&quot;}}\n&lt;/tool_call&gt;&#x27;}], &#x27;tools&#x27;: &#x27;[{&quot;name&quot;: &quot;calculate_age&quot;, &quot;description&quot;: &quot;Calculates the age based on the birthdate.&quot;, &quot;parameters&quot;: {&quot;birthdate&quot;: {&quot;description&quot;: &quot;The birthdate in the format \&#x27;YYYY-MM-DD\&#x27;.&quot;, &quot;type&quot;: &quot;str&quot;}}}, {&quot;name&quot;: &quot;find_duplicate_number&quot;, &quot;description&quot;: &quot;Finds the duplicate number in a list of integers.&quot;, &quot;parameters&quot;: {&quot;nums&quot;: {&quot;description&quot;: &quot;The list of integers.&quot;, &quot;type&quot;: &quot;List[int]&quot;}}}, {&quot;name&quot;: &quot;word_frequency&quot;, &quot;description&quot;: &quot;Calculates the frequency of each word in a text.&quot;, &quot;parameters&quot;: {&quot;text&quot;: {&quot;description&quot;: &quot;The input text.&quot;, &quot;type&quot;: &quot;str&quot;}}}, {&quot;name&quot;: &quot;reverse_words&quot;, &quot;description&quot;: &quot;Reverses the order of words in a sentence.&quot;, &quot;parameters&quot;: {&quot;sentence&quot;: {&quot;description&quot;: &quot;The input sentence.&quot;, &quot;type&quot;: &quot;str&quot;}}}]&#x27;, &#x27;task&#x27;: &quot;What is the word frequency of &#x27;The quick brown fox jumps over the lazy dog&#x27;? And can you reverse the words in the sentence &#x27;Hello world this is a test&#x27;?&quot;, &#x27;category&#x27;: &#x27;Word Frequency&#x27;, &#x27;source&#x27;: &#x27;Salesforce-Xlam&#x27;, &#x27;scenario_category&#x27;: &#x27;single&#x27;}
```

```
# 3) 单条转换（不用整个 map）
converted = hermes_to_gptoss_example(sample)
print(&quot;\n转换后的 gpt-oss messages：&quot;)
for m in converted[&quot;messages&quot;]:
    print(m)
```

```
转换后的 gpt-oss messages：
{&#x27;role&#x27;: &#x27;developer&#x27;, &#x27;content&#x27;: &#x27;You are a deep thinking AI, you may use extremely long chains of thought to deeply consider the problem and deliberate with yourself via systematic reasoning processes to help come to a correct solution prior to answering.&#x27;}
{&#x27;role&#x27;: &#x27;user&#x27;, &#x27;content&#x27;: &quot;What is the word frequency of &#x27;The quick brown fox jumps over the lazy dog&#x27;? And can you reverse the words in the sentence &#x27;Hello world this is a test&#x27;?&quot;}
{&#x27;role&#x27;: &#x27;assistant&#x27;, &#x27;tool_calls&#x27;: [{&#x27;function&#x27;: {&#x27;name&#x27;: &#x27;word_frequency&#x27;, &#x27;arguments&#x27;: {&#x27;text&#x27;: &#x27;The quick brown fox jumps over the lazy dog&#x27;}}}], &#x27;thinking&#x27;: &#x27;Okay, the user has two separate questions here. The first is to find the word frequency of the sentence &quot;The quick brown fox jumps over the lazy dog&quot;. The second is to reverse the words in the sentence &quot;Hello world this is a test&quot;. Let me break down each part step by step.\n\nStarting with the word frequency: I need to use the word_frequency tool. The input text is given as a string, so I\&#x27;ll pass that into the tool. That should return a dictionary where each key is a word and the value is its frequency.\n\nNext, the reverse the sentence part. The user wants the words reversed. That\&#x27;s a straightforward reverse of words in the sentence. The reverse_words tool should handle this. The sentence is &quot;Hello world this is a test&quot;, so I\&#x27;ll pass that into the reverse_words function, which will return the sentence with words reversed.\n\nSo I need to make two separate function calls: one for word_frequency and another for reverse_words. I\&#x27;ll use the word_frequency tool with the sentence as the text parameter, and reverse_words with the same sentence. The outputs from each tool will be returned as JSON objects.&#x27;}
{&#x27;role&#x27;: &#x27;assistant&#x27;, &#x27;tool_calls&#x27;: [{&#x27;function&#x27;: {&#x27;name&#x27;: &#x27;reverse_words&#x27;, &#x27;arguments&#x27;: {&#x27;sentence&#x27;: &#x27;Hello world this is a test&#x27;}}}]}
```

```
from unsloth import FastLanguageModel
import torch
```

```
🦥 Unsloth: Will patch your computer to enable 2x faster free finetuning.


/root/miniconda3/envs/unsloth/lib/python3.12/site-packages/tqdm/auto.py:21: TqdmWarning: IProgress not found. Please update jupyter and ipywidgets. See https://ipywidgets.readthedocs.io/en/stable/user_install.html
  from .autonotebook import tqdm as notebook_tqdm


🦥 Unsloth Zoo will now patch everything to make training faster!
```

```
max_seq_length = 8192
dtype = None
```

```
model, tokenizer = FastLanguageModel.from_pretrained(
    model_name = &quot;./gpt-oss-20b-bf16&quot;,
    dtype = dtype, 
    max_seq_length = max_seq_length, 
    load_in_4bit = True,  
    full_finetuning = False,
)
```

```
==((====))==  Unsloth 2025.8.7: Fast Gpt_Oss patching. Transformers: 4.56.0.dev0.
   \\   /|    NVIDIA H800 PCIe. Num GPUs = 1. Max memory: 79.19 GB. Platform: Linux.
O^O/ \_/ \    Torch: 2.8.0+cu128. CUDA: 9.0. CUDA Toolkit: 12.8. Triton: 3.4.0
\        /    Bfloat16 = TRUE. FA [Xformers = None. FA2 = False]
 &quot;-____-&quot;     Free license: http://github.com/unslothai/unsloth
Unsloth: Fast downloading is enabled - ignore downloading bars which are red colored!
Unsloth: Gpt_Oss does not support SDPA - switching to eager!


Loading checkpoint shards: 100%|██████████| 9/9 [00:06&lt;00:00,  1.30it/s]
```

```
# 4) 渲染成 gpt-oss 文本
text = tokenizer.apply_chat_template(
    converted[&quot;messages&quot;],
    tools=converted[&quot;tools&quot;],
    add_generation_prompt=False,
    reasoning_effort=&quot;low&quot;,
    tokenize=False,
)
print(&quot;\n渲染后的训练文本：&quot;)
```

```
渲染后的训练文本：
```

```
text
```

```
&#x27;&lt;|start|&gt;system&lt;|message|&gt;You are ChatGPT, a large language model trained by OpenAI.\nKnowledge cutoff: 2024-06\nCurrent date: 2025-08-25\n\nReasoning: low\n\n# Valid channels: analysis, commentary, final. Channel must be included for every message.\nCalls to these tools must go to the commentary channel: \&#x27;functions\&#x27;.&lt;|end|&gt;&lt;|start|&gt;developer&lt;|message|&gt;# Instructions\n\nYou are a deep thinking AI, you may use extremely long chains of thought to deeply consider the problem and deliberate with yourself via systematic reasoning processes to help come to a correct solution prior to answering.\n\n# Tools\n\n## functions\n\nnamespace functions {\n\n// Calculates the age based on the birthdate.\ntype calculate_age = (_: {\n// The birthdate in the format \&#x27;YYYY-MM-DD\&#x27;.\nbirthdate: string\n}) =&gt; any;\n\n// Finds the duplicate number in a list of integers.\ntype find_duplicate_number = (_: {\n// The list of integers.\nnums: number[]\n}) =&gt; any;\n\n// Calculates the frequency of each word in a text.\ntype word_frequency = (_: {\n// The input text.\ntext: string\n}) =&gt; any;\n\n// Reverses the order of words in a sentence.\ntype reverse_words = (_: {\n// The input sentence.\nsentence: string\n}) =&gt; any;\n\n} // namespace functions&lt;|end|&gt;&lt;|start|&gt;user&lt;|message|&gt;What is the word frequency of \&#x27;The quick brown fox jumps over the lazy dog\&#x27;? And can you reverse the words in the sentence \&#x27;Hello world this is a test\&#x27;?&lt;|end|&gt;&lt;|start|&gt;assistant to=functions.word_frequency&lt;|channel|&gt;commentary json&lt;|message|&gt;{&quot;text&quot;: &quot;The quick brown fox jumps over the lazy dog&quot;}&lt;|call|&gt;&lt;|start|&gt;assistant to=functions.reverse_words&lt;|channel|&gt;commentary json&lt;|message|&gt;{&quot;sentence&quot;: &quot;Hello world this is a test&quot;}&lt;|call|&gt;&#x27;
```

```
sample = ds[15]
print(&quot;原始 Hermes 数据：&quot;)
print(sample)
```

```
原始 Hermes 数据：
{&#x27;conversations&#x27;: [{&#x27;from&#x27;: &#x27;system&#x27;, &#x27;value&#x27;: &#x27;You are a deep thinking AI, you may use extremely long chains of thought to deeply consider the problem and deliberate with yourself via systematic reasoning processes to help come to a correct solution prior to answering. You should enclose your thoughts and internal monologue inside &lt;think&gt; &lt;/think&gt; tags, and then provide your solution or response to the problem.\n\nYou are a function calling AI model. You are provided with function signatures within &lt;tools&gt; &lt;/tools&gt; XML tags. You may call one or more functions to assist with the user query. If available tools are not relevant in assisting with user query, just respond in natural conversational language. Don\&#x27;t make assumptions about what values to plug into functions. After calling &amp; executing the functions, you will be provided with function results within &lt;tool_response&gt; &lt;/tool_response&gt; XML tags. Here are the available tools:\n&lt;tools&gt;\n[{&quot;name&quot;:&quot;get_biodegradation_rate&quot;,&quot;description&quot;:&quot;Retrieve the rate of biodegradation for a specific substance&quot;,&quot;parameters&quot;:{&quot;type&quot;:&quot;dict&quot;,&quot;properties&quot;:{&quot;substance&quot;:{&quot;type&quot;:&quot;string&quot;,&quot;description&quot;:&quot;The substance for which you want to retrieve the biodegradation rate (e.g., plastic, oil, pesticide)&quot;}},&quot;required&quot;:[&quot;substance&quot;]},&quot;required&quot;:null},{&quot;name&quot;:&quot;calculate_co2_emissions&quot;,&quot;description&quot;:&quot;Calculate the CO2 emissions produced by a specific process&quot;,&quot;parameters&quot;:{&quot;type&quot;:&quot;dict&quot;,&quot;properties&quot;:{&quot;process&quot;:{&quot;type&quot;:&quot;string&quot;,&quot;description&quot;:&quot;The specific process for which you want to calculate CO2 emissions (e.g., manufacturing, transportation)&quot;},&quot;amount&quot;:{&quot;type&quot;:&quot;float&quot;,&quot;description&quot;:&quot;The amount of the process (e.g., number of units, distance traveled)&quot;}},&quot;required&quot;:[&quot;process&quot;,&quot;amount&quot;]},&quot;required&quot;:null},{&quot;name&quot;:&quot;get_microbial_diversity&quot;,&quot;description&quot;:&quot;Retrieve the microbial diversity in a specific environment&quot;,&quot;parameters&quot;:{&quot;type&quot;:&quot;dict&quot;,&quot;properties&quot;:{&quot;environment&quot;:{&quot;type&quot;:&quot;string&quot;,&quot;description&quot;:&quot;The environment for which you want to retrieve the microbial diversity (e.g., soil, water, air)&quot;}},&quot;required&quot;:[&quot;environment&quot;]},&quot;required&quot;:null},{&quot;name&quot;:&quot;get_toxicity_level&quot;,&quot;description&quot;:&quot;Retrieve the toxicity level of a specific substance&quot;,&quot;parameters&quot;:{&quot;type&quot;:&quot;dict&quot;,&quot;properties&quot;:{&quot;substance&quot;:{&quot;type&quot;:&quot;string&quot;,&quot;description&quot;:&quot;The substance for which you want to retrieve the toxicity level (e.g., heavy metals, pollutants, chemicals)&quot;}},&quot;required&quot;:[&quot;substance&quot;]},&quot;required&quot;:null},{&quot;name&quot;:&quot;measure_air_quality&quot;,&quot;description&quot;:&quot;Measure the air quality at a specific location&quot;,&quot;parameters&quot;:{&quot;type&quot;:&quot;dict&quot;,&quot;properties&quot;:{&quot;location&quot;:{&quot;type&quot;:&quot;string&quot;,&quot;description&quot;:&quot;The location for which you want to measure the air quality (e.g., city, address)&quot;},&quot;duration&quot;:{&quot;type&quot;:&quot;int&quot;,&quot;description&quot;:&quot;The duration of the air quality measurement in minutes&quot;}},&quot;required&quot;:[&quot;location&quot;,&quot;duration&quot;]},&quot;required&quot;:null}]\n&lt;/tools&gt;\nFor each function call return a JSON object, with the following pydantic model json schema for each:\n{\&#x27;title\&#x27;: \&#x27;FunctionCall\&#x27;, \&#x27;type\&#x27;: \&#x27;object\&#x27;, \&#x27;properties\&#x27;: {\&#x27;name\&#x27;: {\&#x27;title\&#x27;: \&#x27;Name\&#x27;, \&#x27;type\&#x27;:\&#x27;string\&#x27;}, \&#x27;arguments\&#x27;: {\&#x27;title\&#x27;: \&#x27;Arguments\&#x27;, \&#x27;type\&#x27;: \&#x27;object\&#x27;}},\&#x27;required\&#x27;: [\&#x27;name\&#x27;, \&#x27;arguments\&#x27;]}\nEach function call should be enclosed within &lt;tool_call&gt; &lt;/tool_call&gt; XML tags.\nExample:\n&lt;tool_call&gt;\n{\&#x27;name\&#x27;: &lt;function-name&gt;,\&#x27;arguments\&#x27;: &lt;args-dict&gt;}\n&lt;/tool_call&gt;\n\n&#x27;}, {&#x27;from&#x27;: &#x27;human&#x27;, &#x27;value&#x27;: &#x27;I am conducting an environmental impact study involving several substances. Could you help me find out the toxicity level of lead and then measure the air quality in New York City for the next 60 minutes based on the presence of lead?\n\n&#x27;}, {&#x27;from&#x27;: &#x27;gpt&#x27;, &#x27;value&#x27;: &#x27;&lt;think&gt;\nOkay, the user wants to find the toxicity level of lead and measure air quality in NYC for 60 minutes. Let me start with the first task: getting the toxicity level of lead. I\&#x27;ll use the \&#x27;get_toxicity_level\&#x27; tool with the substance \&#x27;lead\&#x27;.\n\nOnce I get that result, they also want air quality measurements in NYC for 60 minutes. So after the first response, I should call the\&#x27;measure_air_quality\&#x27; tool with the location set to NYC and duration 60.\n&lt;/think&gt;\n&lt;tool_call&gt;\n{&quot;name&quot;:&quot;get_toxicity_level&quot;,&quot;arguments&quot;:{&quot;substance&quot;:&quot;lead&quot;}}\n&lt;/tool_call&gt;&#x27;}, {&#x27;from&#x27;: &#x27;tool&#x27;, &#x27;value&#x27;: &#x27;&lt;tool_response&gt;\n{&quot;tool_call_id&quot;: &quot;call_1142e3bd37684f34a097b1722b664989&quot;, &quot;name&quot;: &quot;get_toxicity_level&quot;, &quot;content&quot;: {&quot;level&quot;: &quot;high&quot;}}\n&lt;/tool_response&gt;&#x27;}, {&#x27;from&#x27;: &#x27;gpt&#x27;, &#x27;value&#x27;: &#x27;&lt;think&gt;\nAlright, the toxicity level of lead is high. Now the next step is to measure the air quality in NYC for 60 minutes. I\&#x27;ll use the\&#x27;measure_air_quality\&#x27; tool with location as \&#x27;New York City\&#x27; and duration 60.\n\nWait, the user didn\&#x27;t specify any particular location within NYC, so I\&#x27;ll assume they mean the city as a whole. Let me make sure I input that correctly.\n&lt;/think&gt;\n&lt;tool_call&gt;\n{&quot;name&quot;:&quot;measure_air_quality&quot;,&quot;arguments&quot;:{&quot;location&quot;:&quot;New York City&quot;,&quot;duration&quot;:60}}\n&lt;/tool_call&gt;&#x27;}, {&#x27;from&#x27;: &#x27;tool&#x27;, &#x27;value&#x27;: &#x27;&lt;tool_response&gt;\n{&quot;tool_call_id&quot;: &quot;call_007f402863ee4a6da707337447343a52&quot;, &quot;name&quot;: &quot;measure_air_quality&quot;, &quot;content&quot;: {&quot;air_quality_index&quot;: 50}}\n&lt;/tool_response&gt;&#x27;}, {&#x27;from&#x27;: &#x27;gpt&#x27;, &#x27;value&#x27;: &quot;&lt;think&gt;\nOkay, the user asked for the toxicity level of lead and the air quality in NYC. The first tool provided a high toxicity level for lead, which is important to note in environmental impact studies. For the air quality measurement, it came back as 50, which is moderate. The air quality index (AQI) of 50 falls under the &#x27;Good&#x27; category, so the air quality in NYC is good. Since the user didn&#x27;t specify any other parameters, these two results should cover their request. I&#x27;ll summarize the findings to them.\n&lt;/think&gt;The toxicity level of lead is **high**, and the air quality in New York City for the next 60 minutes is **good** (AQI of 50). This indicates a moderate level of air quality, suggesting that there are no immediate health concerns related to air pollution.&quot;}, {&#x27;from&#x27;: &#x27;gpt&#x27;, &#x27;value&#x27;: &#x27;&lt;think&gt;\nOkay, the user wants to know the biodegradation rate of plastic and the CO2 emissions from burning oil. Let me start by calling the biodegradation tool for plastic. After that, for CO2 emissions, I need to specify the process as \&#x27;burning\&#x27; and the amount, but the user didn\&#x27;t mention the amount. Hmm, maybe they mean burning a certain amount, but it\&#x27;s not specified. Maybe I should ask for the amount as part of the process. Wait, the user might have missed providing that. Let me check the original query again. Oh, the user just says &quot;calculate CO2 emissions produced by burning oil&quot;, so perhaps they expect a general estimate. Maybe I should clarify with a friendly prompt, but the instructions say to proceed as given. Alternatively, maybe use a default amount or ask if they can specify, but perhaps the tools require a numeric value. Wait, looking at the calculate_co2_emissions tool parameters, it requires an amount. So without that, I might need to prompt the user, but according to the instructions, I should call the tool even if the parameters aren\&#x27;t fully specified. Wait, but the user might have meant that the amount is variable. Hmm, perhaps the tool expects a specific value. Maybe I should proceed by asking the user for the amount, but the original query doesn\&#x27;t specify. Since it\&#x27;s part of the available tools, perhaps I should call it with a placeholder, but that might be an issue. Alternatively, maybe the user expects a general estimate without a specific amount. Maybe I should proceed with a general calculation, but the tool requires an amount. This is a problem. Wait, the user\&#x27;s query says &quot;calculate CO2 emissions produced by burning oil&quot;. So they might want a general calculation, but the tool needs a numerical amount. I need to figure out if there\&#x27;s a standard amount to use or if I should prompt the user. However, the instructions say to use the given parameters as is. Therefore, perhaps I should proceed by indicating that the CO2 calculation requires an amount, which was missing. Let me proceed with the first tool call for biodegradation rate of plastic, then proceed to the CO2 emissions tool with a placeholder or error message. Wait, perhaps the user intended to provide the amount but missed it. Since I can\&#x27;t assume, maybe I should inform them that I need the amount for the CO2 calculation. But according to the initial instructions, should I proceed or wait? The instructions say to call exactly one tool at a time and wait for its response. Since the CO2 tool requires an amount and it\&#x27;s missing, perhaps I should inform the user to provide that before proceeding. But the system might expect me to proceed as per the query. Hmm, this is a challenge. Let me think again.\n\nThe user\&#x27;s exact query is: &quot;Calculate the CO2 emissions produced by burning oil.&quot; Without specifying the amount, like how much oil is being burned. So the calculate_co2_emissions tool requires both process (which is \&#x27;burning\&#x27;) and amount (the quantity, like litres, tonnes, etc.). Since that\&#x27;s missing, I should inform the user that the amount is missing. But the problem is that the user might not realize that. Given that the instructions say to proceed sequentially, perhaps I should call the first tool, then for the second, indicate that the amount is required. But the initial instructions say to decide whether to call another tool based on the response. So, for the CO2 call, I should call it with a placeholder amount, like 1000 units, but that\&#x27;s assuming. Alternatively, maybe the user expects using a common scenario. Alternatively, perhaps the user wants a general estimate without specific numbers, but the tool requires a number. Since I can\&#x27;t proceed without that, perhaps I should inform the user to provide the amount. But according to the initial problem statement, the user is supposed to be in a scenario where they are asking via the AI, and the assistant should handle it. Maybe I\&#x27;ll proceed by calling the CO2 tool with a placeholder amount and explain that. But I\&#x27;m not sure. Alternatively, maybe the user expects the tool to handle it. But in the absence of specific data, I can\&#x27;t proceed, so I should request the user to provide the amount. However, given the instructions, perhaps I should proceed to call the tools as per the initial query and note the missing data. Hmm, this is a bit of a problem. Let\&#x27;s proceed step by step.\n\nFirst, biodegradation rate of plastic:\n\nCall: get_biodegradation_rate with substance=\&#x27;plastic\&#x27;\n\nThen, calculate_co2_emissions with process=\&#x27;burning oil\&#x27; and amount=... (but amount is missing). Therefore, first, I\&#x27;ll call the biodegradation tool, then the CO2 tool with a placeholder amount, but that might not be accurate. Alternatively, maybe the user intended to provide an example, but didn\&#x27;t. Since I can\&#x27;t assume, I\&#x27;ll inform the user that the&#x27;}], &#x27;tools&#x27;: &#x27;[{&quot;name&quot;: &quot;get_biodegradation_rate&quot;, &quot;description&quot;: &quot;Retrieve the rate of biodegradation for a specific substance&quot;, &quot;parameters&quot;: {&quot;type&quot;: &quot;dict&quot;, &quot;properties&quot;: {&quot;substance&quot;: {&quot;type&quot;: &quot;string&quot;, &quot;description&quot;: &quot;The substance for which you want to retrieve the biodegradation rate (e.g., plastic, oil, pesticide)&quot;}}, &quot;required&quot;: [&quot;substance&quot;]}, &quot;required&quot;: null}, {&quot;name&quot;: &quot;calculate_co2_emissions&quot;, &quot;description&quot;: &quot;Calculate the CO2 emissions produced by a specific process&quot;, &quot;parameters&quot;: {&quot;type&quot;: &quot;dict&quot;, &quot;properties&quot;: {&quot;process&quot;: {&quot;type&quot;: &quot;string&quot;, &quot;description&quot;: &quot;The specific process for which you want to calculate CO2 emissions (e.g., manufacturing, transportation)&quot;}, &quot;amount&quot;: {&quot;type&quot;: &quot;float&quot;, &quot;description&quot;: &quot;The amount of the process (e.g., number of units, distance traveled)&quot;}}, &quot;required&quot;: [&quot;process&quot;, &quot;amount&quot;]}, &quot;required&quot;: null}, {&quot;name&quot;: &quot;get_microbial_diversity&quot;, &quot;description&quot;: &quot;Retrieve the microbial diversity in a specific environment&quot;, &quot;parameters&quot;: {&quot;type&quot;: &quot;dict&quot;, &quot;properties&quot;: {&quot;environment&quot;: {&quot;type&quot;: &quot;string&quot;, &quot;description&quot;: &quot;The environment for which you want to retrieve the microbial diversity (e.g., soil, water, air)&quot;}}, &quot;required&quot;: [&quot;environment&quot;]}, &quot;required&quot;: null}, {&quot;name&quot;: &quot;get_toxicity_level&quot;, &quot;description&quot;: &quot;Retrieve the toxicity level of a specific substance&quot;, &quot;parameters&quot;: {&quot;type&quot;: &quot;dict&quot;, &quot;properties&quot;: {&quot;substance&quot;: {&quot;type&quot;: &quot;string&quot;, &quot;description&quot;: &quot;The substance for which you want to retrieve the toxicity level (e.g., heavy metals, pollutants, chemicals)&quot;}}, &quot;required&quot;: [&quot;substance&quot;]}, &quot;required&quot;: null}, {&quot;name&quot;: &quot;measure_air_quality&quot;, &quot;description&quot;: &quot;Measure the air quality at a specific location&quot;, &quot;parameters&quot;: {&quot;type&quot;: &quot;dict&quot;, &quot;properties&quot;: {&quot;location&quot;: {&quot;type&quot;: &quot;string&quot;, &quot;description&quot;: &quot;The location for which you want to measure the air quality (e.g., city, address)&quot;}, &quot;duration&quot;: {&quot;type&quot;: &quot;int&quot;, &quot;description&quot;: &quot;The duration of the air quality measurement in minutes&quot;}}, &quot;required&quot;: [&quot;location&quot;, &quot;duration&quot;]}, &quot;required&quot;: null}]&#x27;, &#x27;task&#x27;: &#x27;I am conducting an environmental impact study involving several substances. Could you help me find out the toxicity level of lead and then measure the air quality in New York City for the next 60 minutes based on the presence of lead?&#x27;, &#x27;category&#x27;: &#x27;Get Toxicity Level&#x27;, &#x27;source&#x27;: &#x27;ToolAce&#x27;, &#x27;scenario_category&#x27;: &#x27;multistep&#x27;}
```

```
converted = hermes_to_gptoss_example(sample)
print(&quot;\n转换后的 gpt-oss messages：&quot;)
for m in converted[&quot;messages&quot;]:
    print(m)
```

```
转换后的 gpt-oss messages：
{&#x27;role&#x27;: &#x27;developer&#x27;, &#x27;content&#x27;: &#x27;You are a deep thinking AI, you may use extremely long chains of thought to deeply consider the problem and deliberate with yourself via systematic reasoning processes to help come to a correct solution prior to answering.&#x27;}
{&#x27;role&#x27;: &#x27;user&#x27;, &#x27;content&#x27;: &#x27;I am conducting an environmental impact study involving several substances. Could you help me find out the toxicity level of lead and then measure the air quality in New York City for the next 60 minutes based on the presence of lead?\n\n&#x27;}
{&#x27;role&#x27;: &#x27;assistant&#x27;, &#x27;tool_calls&#x27;: [{&#x27;function&#x27;: {&#x27;name&#x27;: &#x27;get_toxicity_level&#x27;, &#x27;arguments&#x27;: {&#x27;substance&#x27;: &#x27;lead&#x27;}}}], &#x27;thinking&#x27;: &quot;Okay, the user wants to find the toxicity level of lead and measure air quality in NYC for 60 minutes. Let me start with the first task: getting the toxicity level of lead. I&#x27;ll use the &#x27;get_toxicity_level&#x27; tool with the substance &#x27;lead&#x27;.\n\nOnce I get that result, they also want air quality measurements in NYC for 60 minutes. So after the first response, I should call the&#x27;measure_air_quality&#x27; tool with the location set to NYC and duration 60.&quot;}
{&#x27;role&#x27;: &#x27;tool&#x27;, &#x27;content&#x27;: &#x27;{&quot;tool_call_id&quot;: &quot;call_1142e3bd37684f34a097b1722b664989&quot;, &quot;name&quot;: &quot;get_toxicity_level&quot;, &quot;content&quot;: {&quot;level&quot;: &quot;high&quot;}}&#x27;}
{&#x27;role&#x27;: &#x27;assistant&#x27;, &#x27;tool_calls&#x27;: [{&#x27;function&#x27;: {&#x27;name&#x27;: &#x27;measure_air_quality&#x27;, &#x27;arguments&#x27;: {&#x27;location&#x27;: &#x27;New York City&#x27;, &#x27;duration&#x27;: 60}}}], &#x27;thinking&#x27;: &quot;Alright, the toxicity level of lead is high. Now the next step is to measure the air quality in NYC for 60 minutes. I&#x27;ll use the&#x27;measure_air_quality&#x27; tool with location as &#x27;New York City&#x27; and duration 60.\n\nWait, the user didn&#x27;t specify any particular location within NYC, so I&#x27;ll assume they mean the city as a whole. Let me make sure I input that correctly.&quot;}
{&#x27;role&#x27;: &#x27;tool&#x27;, &#x27;content&#x27;: &#x27;{&quot;tool_call_id&quot;: &quot;call_007f402863ee4a6da707337447343a52&quot;, &quot;name&quot;: &quot;measure_air_quality&quot;, &quot;content&quot;: {&quot;air_quality_index&quot;: 50}}&#x27;}
{&#x27;role&#x27;: &#x27;assistant&#x27;, &#x27;thinking&#x27;: &quot;Okay, the user asked for the toxicity level of lead and the air quality in NYC. The first tool provided a high toxicity level for lead, which is important to note in environmental impact studies. For the air quality measurement, it came back as 50, which is moderate. The air quality index (AQI) of 50 falls under the &#x27;Good&#x27; category, so the air quality in NYC is good. Since the user didn&#x27;t specify any other parameters, these two results should cover their request. I&#x27;ll summarize the findings to them.&quot;, &#x27;content&#x27;: &#x27;The toxicity level of lead is **high**, and the air quality in New York City for the next 60 minutes is **good** (AQI of 50). This indicates a moderate level of air quality, suggesting that there are no immediate health concerns related to air pollution.&#x27;}
{&#x27;role&#x27;: &#x27;assistant&#x27;, &#x27;content&#x27;: &#x27;Okay, the user wants to know the biodegradation rate of plastic and the CO2 emissions from burning oil. Let me start by calling the biodegradation tool for plastic. After that, for CO2 emissions, I need to specify the process as \&#x27;burning\&#x27; and the amount, but the user didn\&#x27;t mention the amount. Hmm, maybe they mean burning a certain amount, but it\&#x27;s not specified. Maybe I should ask for the amount as part of the process. Wait, the user might have missed providing that. Let me check the original query again. Oh, the user just says &quot;calculate CO2 emissions produced by burning oil&quot;, so perhaps they expect a general estimate. Maybe I should clarify with a friendly prompt, but the instructions say to proceed as given. Alternatively, maybe use a default amount or ask if they can specify, but perhaps the tools require a numeric value. Wait, looking at the calculate_co2_emissions tool parameters, it requires an amount. So without that, I might need to prompt the user, but according to the instructions, I should call the tool even if the parameters aren\&#x27;t fully specified. Wait, but the user might have meant that the amount is variable. Hmm, perhaps the tool expects a specific value. Maybe I should proceed by asking the user for the amount, but the original query doesn\&#x27;t specify. Since it\&#x27;s part of the available tools, perhaps I should call it with a placeholder, but that might be an issue. Alternatively, maybe the user expects a general estimate without a specific amount. Maybe I should proceed with a general calculation, but the tool requires an amount. This is a problem. Wait, the user\&#x27;s query says &quot;calculate CO2 emissions produced by burning oil&quot;. So they might want a general calculation, but the tool needs a numerical amount. I need to figure out if there\&#x27;s a standard amount to use or if I should prompt the user. However, the instructions say to use the given parameters as is. Therefore, perhaps I should proceed by indicating that the CO2 calculation requires an amount, which was missing. Let me proceed with the first tool call for biodegradation rate of plastic, then proceed to the CO2 emissions tool with a placeholder or error message. Wait, perhaps the user intended to provide the amount but missed it. Since I can\&#x27;t assume, maybe I should inform them that I need the amount for the CO2 calculation. But according to the initial instructions, should I proceed or wait? The instructions say to call exactly one tool at a time and wait for its response. Since the CO2 tool requires an amount and it\&#x27;s missing, perhaps I should inform the user to provide that before proceeding. But the system might expect me to proceed as per the query. Hmm, this is a challenge. Let me think again.\n\nThe user\&#x27;s exact query is: &quot;Calculate the CO2 emissions produced by burning oil.&quot; Without specifying the amount, like how much oil is being burned. So the calculate_co2_emissions tool requires both process (which is \&#x27;burning\&#x27;) and amount (the quantity, like litres, tonnes, etc.). Since that\&#x27;s missing, I should inform the user that the amount is missing. But the problem is that the user might not realize that. Given that the instructions say to proceed sequentially, perhaps I should call the first tool, then for the second, indicate that the amount is required. But the initial instructions say to decide whether to call another tool based on the response. So, for the CO2 call, I should call it with a placeholder amount, like 1000 units, but that\&#x27;s assuming. Alternatively, maybe the user expects using a common scenario. Alternatively, perhaps the user wants a general estimate without specific numbers, but the tool requires a number. Since I can\&#x27;t proceed without that, perhaps I should inform the user to provide the amount. But according to the initial problem statement, the user is supposed to be in a scenario where they are asking via the AI, and the assistant should handle it. Maybe I\&#x27;ll proceed by calling the CO2 tool with a placeholder amount and explain that. But I\&#x27;m not sure. Alternatively, maybe the user expects the tool to handle it. But in the absence of specific data, I can\&#x27;t proceed, so I should request the user to provide the amount. However, given the instructions, perhaps I should proceed to call the tools as per the initial query and note the missing data. Hmm, this is a bit of a problem. Let\&#x27;s proceed step by step.\n\nFirst, biodegradation rate of plastic:\n\nCall: get_biodegradation_rate with substance=\&#x27;plastic\&#x27;\n\nThen, calculate_co2_emissions with process=\&#x27;burning oil\&#x27; and amount=... (but amount is missing). Therefore, first, I\&#x27;ll call the biodegradation tool, then the CO2 tool with a placeholder amount, but that might not be accurate. Alternatively, maybe the user intended to provide an example, but didn\&#x27;t. Since I can\&#x27;t assume, I\&#x27;ll inform the user that the&#x27;}
```

```
text = tokenizer.apply_chat_template(
    converted[&quot;messages&quot;],
    tools=converted[&quot;tools&quot;],
    add_generation_prompt=False,
    reasoning_effort=&quot;low&quot;,
    tokenize=False,
)
print(&quot;\n渲染后的训练文本：&quot;)
```

```
渲染后的训练文本：
```

```
text
```

```
&#x27;&lt;|start|&gt;system&lt;|message|&gt;You are ChatGPT, a large language model trained by OpenAI.\nKnowledge cutoff: 2024-06\nCurrent date: 2025-08-25\n\nReasoning: low\n\n# Valid channels: analysis, commentary, final. Channel must be included for every message.\nCalls to these tools must go to the commentary channel: \&#x27;functions\&#x27;.&lt;|end|&gt;&lt;|start|&gt;developer&lt;|message|&gt;# Instructions\n\nYou are a deep thinking AI, you may use extremely long chains of thought to deeply consider the problem and deliberate with yourself via systematic reasoning processes to help come to a correct solution prior to answering.\n\n# Tools\n\n## functions\n\nnamespace functions {\n\n// Retrieve the rate of biodegradation for a specific substance\ntype get_biodegradation_rate = (_: {\n// The substance for which you want to retrieve the biodegradation rate (e.g., plastic, oil, pesticide)\nsubstance: string\n}) =&gt; any;\n\n// Calculate the CO2 emissions produced by a specific process\ntype calculate_co2_emissions = (_: {\n// The specific process for which you want to calculate CO2 emissions (e.g., manufacturing, transportation)\nprocess: string,\n// The amount of the process (e.g., number of units, distance traveled)\namount: any\n}) =&gt; any;\n\n// Retrieve the microbial diversity in a specific environment\ntype get_microbial_diversity = (_: {\n// The environment for which you want to retrieve the microbial diversity (e.g., soil, water, air)\nenvironment: string\n}) =&gt; any;\n\n// Retrieve the toxicity level of a specific substance\ntype get_toxicity_level = (_: {\n// The substance for which you want to retrieve the toxicity level (e.g., heavy metals, pollutants, chemicals)\nsubstance: string\n}) =&gt; any;\n\n// Measure the air quality at a specific location\ntype measure_air_quality = (_: {\n// The location for which you want to measure the air quality (e.g., city, address)\nlocation: string,\n// The duration of the air quality measurement in minutes\nduration: any\n}) =&gt; any;\n\n} // namespace functions&lt;|end|&gt;&lt;|start|&gt;user&lt;|message|&gt;I am conducting an environmental impact study involving several substances. Could you help me find out the toxicity level of lead and then measure the air quality in New York City for the next 60 minutes based on the presence of lead?\n\n&lt;|end|&gt;&lt;|start|&gt;assistant to=functions.get_toxicity_level&lt;|channel|&gt;commentary json&lt;|message|&gt;{&quot;substance&quot;: &quot;lead&quot;}&lt;|call|&gt;&lt;|start|&gt;functions.get_toxicity_level to=assistant&lt;|channel|&gt;commentary&lt;|message|&gt;&quot;{\\&quot;tool_call_id\\&quot;: \\&quot;call_1142e3bd37684f34a097b1722b664989\\&quot;, \\&quot;name\\&quot;: \\&quot;get_toxicity_level\\&quot;, \\&quot;content\\&quot;: {\\&quot;level\\&quot;: \\&quot;high\\&quot;}}&quot;&lt;|end|&gt;&lt;|start|&gt;assistant to=functions.measure_air_quality&lt;|channel|&gt;commentary json&lt;|message|&gt;{&quot;location&quot;: &quot;New York City&quot;, &quot;duration&quot;: 60}&lt;|call|&gt;&lt;|start|&gt;functions.measure_air_quality to=assistant&lt;|channel|&gt;commentary&lt;|message|&gt;&quot;{\\&quot;tool_call_id\\&quot;: \\&quot;call_007f402863ee4a6da707337447343a52\\&quot;, \\&quot;name\\&quot;: \\&quot;measure_air_quality\\&quot;, \\&quot;content\\&quot;: {\\&quot;air_quality_index\\&quot;: 50}}&quot;&lt;|end|&gt;&lt;|start|&gt;assistant&lt;|channel|&gt;final&lt;|message|&gt;The toxicity level of lead is **high**, and the air quality in New York City for the next 60 minutes is **good** (AQI of 50). This indicates a moderate level of air quality, suggesting that there are no immediate health concerns related to air pollution.&lt;|end|&gt;&lt;|start|&gt;assistant&lt;|message|&gt;Okay, the user wants to know the biodegradation rate of plastic and the CO2 emissions from burning oil. Let me start by calling the biodegradation tool for plastic. After that, for CO2 emissions, I need to specify the process as \&#x27;burning\&#x27; and the amount, but the user didn\&#x27;t mention the amount. Hmm, maybe they mean burning a certain amount, but it\&#x27;s not specified. Maybe I should ask for the amount as part of the process. Wait, the user might have missed providing that. Let me check the original query again. Oh, the user just says &quot;calculate CO2 emissions produced by burning oil&quot;, so perhaps they expect a general estimate. Maybe I should clarify with a friendly prompt, but the instructions say to proceed as given. Alternatively, maybe use a default amount or ask if they can specify, but perhaps the tools require a numeric value. Wait, looking at the calculate_co2_emissions tool parameters, it requires an amount. So without that, I might need to prompt the user, but according to the instructions, I should call the tool even if the parameters aren\&#x27;t fully specified. Wait, but the user might have meant that the amount is variable. Hmm, perhaps the tool expects a specific value. Maybe I should proceed by asking the user for the amount, but the original query doesn\&#x27;t specify. Since it\&#x27;s part of the available tools, perhaps I should call it with a placeholder, but that might be an issue. Alternatively, maybe the user expects a general estimate without a specific amount. Maybe I should proceed with a general calculation, but the tool requires an amount. This is a problem. Wait, the user\&#x27;s query says &quot;calculate CO2 emissions produced by burning oil&quot;. So they might want a general calculation, but the tool needs a numerical amount. I need to figure out if there\&#x27;s a standard amount to use or if I should prompt the user. However, the instructions say to use the given parameters as is. Therefore, perhaps I should proceed by indicating that the CO2 calculation requires an amount, which was missing. Let me proceed with the first tool call for biodegradation rate of plastic, then proceed to the CO2 emissions tool with a placeholder or error message. Wait, perhaps the user intended to provide the amount but missed it. Since I can\&#x27;t assume, maybe I should inform them that I need the amount for the CO2 calculation. But according to the initial instructions, should I proceed or wait? The instructions say to call exactly one tool at a time and wait for its response. Since the CO2 tool requires an amount and it\&#x27;s missing, perhaps I should inform the user to provide that before proceeding. But the system might expect me to proceed as per the query. Hmm, this is a challenge. Let me think again.\n\nThe user\&#x27;s exact query is: &quot;Calculate the CO2 emissions produced by burning oil.&quot; Without specifying the amount, like how much oil is being burned. So the calculate_co2_emissions tool requires both process (which is \&#x27;burning\&#x27;) and amount (the quantity, like litres, tonnes, etc.). Since that\&#x27;s missing, I should inform the user that the amount is missing. But the problem is that the user might not realize that. Given that the instructions say to proceed sequentially, perhaps I should call the first tool, then for the second, indicate that the amount is required. But the initial instructions say to decide whether to call another tool based on the response. So, for the CO2 call, I should call it with a placeholder amount, like 1000 units, but that\&#x27;s assuming. Alternatively, maybe the user expects using a common scenario. Alternatively, perhaps the user wants a general estimate without specific numbers, but the tool requires a number. Since I can\&#x27;t proceed without that, perhaps I should inform the user to provide the amount. But according to the initial problem statement, the user is supposed to be in a scenario where they are asking via the AI, and the assistant should handle it. Maybe I\&#x27;ll proceed by calling the CO2 tool with a placeholder amount and explain that. But I\&#x27;m not sure. Alternatively, maybe the user expects the tool to handle it. But in the absence of specific data, I can\&#x27;t proceed, so I should request the user to provide the amount. However, given the instructions, perhaps I should proceed to call the tools as per the initial query and note the missing data. Hmm, this is a bit of a problem. Let\&#x27;s proceed step by step.\n\nFirst, biodegradation rate of plastic:\n\nCall: get_biodegradation_rate with substance=\&#x27;plastic\&#x27;\n\nThen, calculate_co2_emissions with process=\&#x27;burning oil\&#x27; and amount=... (but amount is missing). Therefore, first, I\&#x27;ll call the biodegradation tool, then the CO2 tool with a placeholder amount, but that might not be accurate. Alternatively, maybe the user intended to provide an example, but didn\&#x27;t. Since I can\&#x27;t assume, I\&#x27;ll inform the user that the&lt;|return|&gt;&#x27;
```

接下来，我们抽取200条数据进行批量转化与快速微调效果验证：

```
from datasets import load_dataset, Dataset, Features, Value
import random, json, os
```

```
# 随机采样 200 条（可换成你想要的索引集合）
seed = 42
random.seed(seed)
idxs = random.sample(range(len(ds_full)), 200)
ds_sample = ds_full.select(idxs)
```

```
print(ds_sample)  # sanity check
```

```
Dataset({
    features: [&#x27;conversations&#x27;, &#x27;tools&#x27;, &#x27;task&#x27;, &#x27;category&#x27;, &#x27;source&#x27;, &#x27;scenario_category&#x27;],
    num_rows: 200
})
```

```
ds_sample[0]
```

```
{&#x27;conversations&#x27;: [{&#x27;from&#x27;: &#x27;system&#x27;,
   &#x27;value&#x27;: &#x27;You are a deep thinking AI, you may use extremely long chains of thought to deeply consider the problem and deliberate with yourself via systematic reasoning processes to help come to a correct solution prior to answering. You should enclose your thoughts and internal monologue inside &lt;think&gt; &lt;/think&gt; tags, and then provide your solution or response to the problem.\n\nYou are a function calling AI model. You are provided with function signatures within &lt;tools&gt; &lt;/tools&gt; XML tags. You may call one or more functions to assist with the user query. If available tools are not relevant in assisting with user query, just respond in natural conversational language. Don\&#x27;t make assumptions about what values to plug into functions. After calling &amp; executing the functions, you will be provided with function results within &lt;tool_response&gt; &lt;/tool_response&gt; XML tags. Here are the available tools:\n&lt;tools&gt;\n[\n{&quot;name&quot;: &quot;is_hotel_available&quot;, &quot;description&quot;: &quot;Checks the availability of a hotel for a given date range.&quot;, &quot;parameters&quot;: {&quot;type&quot;: &quot;dict&quot;, &quot;properties&quot;: {&quot;hotel&quot;: {&quot;description&quot;: &quot;The name of the hotel.&quot;, &quot;type&quot;: &quot;str&quot;}, &quot;city&quot;: {&quot;description&quot;: &quot;The city where the hotel is located.&quot;, &quot;type&quot;: &quot;str&quot;}, &quot;checkin&quot;: {&quot;description&quot;: &quot;The check-in date in the format \\&quot;YYYY-MM-DD\\&quot;.&quot;, &quot;type&quot;: &quot;str&quot;}, &quot;checkout&quot;: {&quot;description&quot;: &quot;The check-out date in the format \\&quot;YYYY-MM-DD\\&quot;.&quot;, &quot;type&quot;: &quot;str&quot;}}}, &quot;required&quot;: [&quot;hotel&quot;, &quot;city&quot;, &quot;checkin&quot;, &quot;checkout&quot;]}\n]\n&lt;/tools&gt;\nFor each function call return a JSON object, with the following pydantic model json schema for each:\n{\&#x27;title\&#x27;: \&#x27;FunctionCall\&#x27;, \&#x27;type\&#x27;: \&#x27;object\&#x27;, \&#x27;properties\&#x27;: {\&#x27;name\&#x27;: {\&#x27;title\&#x27;: \&#x27;Name\&#x27;, \&#x27;type\&#x27;:\&#x27;string\&#x27;}, \&#x27;arguments\&#x27;: {\&#x27;title\&#x27;: \&#x27;Arguments\&#x27;, \&#x27;type\&#x27;: \&#x27;object\&#x27;}},\&#x27;required\&#x27;: [\&#x27;name\&#x27;, \&#x27;arguments\&#x27;]}\nEach function call should be enclosed within &lt;tool_call&gt; &lt;/tool_call&gt; XML tags.\nExample:\n&lt;tool_call&gt;\n{\&#x27;name\&#x27;: &lt;function-name&gt;,\&#x27;arguments\&#x27;: &lt;args-dict&gt;}\n&lt;/tool_call&gt;\n\n&#x27;},
  {&#x27;from&#x27;: &#x27;human&#x27;,
   &#x27;value&#x27;: &#x27;I want to book a hotel in Berlin from 3/15/2022 to 3/20/2022. Is the Queens Hotel available?&#x27;},
  {&#x27;from&#x27;: &#x27;gpt&#x27;,
   &#x27;value&#x27;: &#x27;&lt;think&gt;\nOkay, the user wants to check the availability of the Queens Hotel in Berlin from March 15th to March 20th, 2022. Let me start by recalling the available tools. The only tool provided here is the hotel availability checker, which is called &quot;is_hotel_available&quot;. \n\nSo, I need to construct the proper function call for that tool. The parameters required are hotel, city, checkin, and checkout. The user mentioned the hotel is Queens Hotel, city is Berlin, check-in is March 15, 2022 (which should be formatted as YYYY-MM-DD, so 2022-03-15), and check-out is March 20, 2022 (2022-03-20). \n\nI\&#x27;ll call the is_hotel_available function with these parameters and then present the response. The Python code will need to use the function name and pass the dictionary with the required details. Once the function is called, the result will be in JSON format, which I should parse to show the availability information.\n&lt;/think&gt;\n&lt;tool_call&gt;\n{&quot;name&quot;:&quot;is_hotel_available&quot;,&quot;arguments&quot;:{&quot;hotel&quot;:&quot;Queens Hotel&quot;,&quot;city&quot;:&quot;Berlin, Germany&quot;,&quot;checkin&quot;:&quot;2022-03-15&quot;,&quot;checkout&quot;:&quot;2022-03-20&quot;}}\n&lt;/tool_call&gt;&#x27;},
  {&#x27;from&#x27;: &#x27;tool&#x27;,
   &#x27;value&#x27;: &#x27;&lt;tool_response&gt;\n{&quot;name&quot;: &quot;is_hotel_available&quot;, &quot;content&quot;: {&quot;hotel&quot;: &quot;Queens Hotel&quot;, &quot;city&quot;: &quot;Berlin, Germany&quot;, &quot;checkin&quot;: &quot;2022-03-15&quot;, &quot;checkout&quot;: &quot;2022-03-20&quot;, &quot;availability&quot;: &quot;Available&quot;, &quot;rooms_left&quot;: 5, &quot;price_per_night&quot;: 189, &quot;total_price&quot;: 945, &quot;cancellation_policy&quot;: &quot;Flexible until 5 days before arrival&quot;, &quot;special_offers&quot;: []}}\n&lt;/tool_response&gt;&#x27;}],
 &#x27;tools&#x27;: &#x27;[{&quot;name&quot;: &quot;is_hotel_available&quot;, &quot;description&quot;: &quot;Checks the availability of a hotel for a given date range.&quot;, &quot;parameters&quot;: {&quot;type&quot;: &quot;dict&quot;, &quot;properties&quot;: {&quot;hotel&quot;: {&quot;description&quot;: &quot;The name of the hotel.&quot;, &quot;type&quot;: &quot;str&quot;}, &quot;city&quot;: {&quot;description&quot;: &quot;The city where the hotel is located.&quot;, &quot;type&quot;: &quot;str&quot;}, &quot;checkin&quot;: {&quot;description&quot;: &quot;The check-in date in the format \\&quot;YYYY-MM-DD\\&quot;.&quot;, &quot;type&quot;: &quot;str&quot;}, &quot;checkout&quot;: {&quot;description&quot;: &quot;The check-out date in the format \\&quot;YYYY-MM-DD\\&quot;.&quot;, &quot;type&quot;: &quot;str&quot;}}}, &quot;required&quot;: [&quot;hotel&quot;, &quot;city&quot;, &quot;checkin&quot;, &quot;checkout&quot;]}]&#x27;,
 &#x27;task&#x27;: &#x27;I want to book a hotel in Berlin from 3/15/2022 to 3/20/2022. Is the Queens Hotel available?&#x27;,
 &#x27;category&#x27;: &#x27;Is Hotel Available&#x27;,
 &#x27;source&#x27;: &#x27;Nvidia-When2Call&#x27;,
 &#x27;scenario_category&#x27;: &#x27;single&#x27;}
```

```
# 注意：不要返回列名 &#x27;tools&#x27;，用新名字避免与原始列冲突
def map_convert_to_strings(ex):
    out = hermes_to_gptoss_example(ex)  # -&gt; {&quot;messages&quot;: [...], &quot;tools&quot;: [...]}
    return {
        &quot;messages_json&quot;: json.dumps(out[&quot;messages&quot;], ensure_ascii=False),
        &quot;tools_json&quot;: json.dumps(out[&quot;tools&quot;], ensure_ascii=False),
    }

# 显式声明 features 为 string，杜绝 Arrow 猜类型
features = Features({
    &quot;messages_json&quot;: Value(&quot;string&quot;),
    &quot;tools_json&quot;: Value(&quot;string&quot;),
})

# 移除原始所有列，只保留我们需要的两列，避免同名冲突
ds_conv = ds_sample.map(
    map_convert_to_strings,
    remove_columns=ds_sample.column_names,
    features=features,
)
print(ds_conv[0])
```

```
Map: 100%|██████████| 200/200 [00:00&lt;00:00, 2510.78 examples/s]

{&#x27;messages_json&#x27;: &#x27;[{&quot;role&quot;: &quot;developer&quot;, &quot;content&quot;: &quot;You are a deep thinking AI, you may use extremely long chains of thought to deeply consider the problem and deliberate with yourself via systematic reasoning processes to help come to a correct solution prior to answering.&quot;}, {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;I want to book a hotel in Berlin from 3/15/2022 to 3/20/2022. Is the Queens Hotel available?&quot;}, {&quot;role&quot;: &quot;assistant&quot;, &quot;tool_calls&quot;: [{&quot;function&quot;: {&quot;name&quot;: &quot;is_hotel_available&quot;, &quot;arguments&quot;: {&quot;hotel&quot;: &quot;Queens Hotel&quot;, &quot;city&quot;: &quot;Berlin, Germany&quot;, &quot;checkin&quot;: &quot;2022-03-15&quot;, &quot;checkout&quot;: &quot;2022-03-20&quot;}}}], &quot;thinking&quot;: &quot;Okay, the user wants to check the availability of the Queens Hotel in Berlin from March 15th to March 20th, 2022. Let me start by recalling the available tools. The only tool provided here is the hotel availability checker, which is called \\&quot;is_hotel_available\\&quot;. \\n\\nSo, I need to construct the proper function call for that tool. The parameters required are hotel, city, checkin, and checkout. The user mentioned the hotel is Queens Hotel, city is Berlin, check-in is March 15, 2022 (which should be formatted as YYYY-MM-DD, so 2022-03-15), and check-out is March 20, 2022 (2022-03-20). \\n\\nI\&#x27;ll call the is_hotel_available function with these parameters and then present the response. The Python code will need to use the function name and pass the dictionary with the required details. Once the function is called, the result will be in JSON format, which I should parse to show the availability information.&quot;}, {&quot;role&quot;: &quot;tool&quot;, &quot;content&quot;: &quot;{\\&quot;name\\&quot;: \\&quot;is_hotel_available\\&quot;, \\&quot;content\\&quot;: {\\&quot;hotel\\&quot;: \\&quot;Queens Hotel\\&quot;, \\&quot;city\\&quot;: \\&quot;Berlin, Germany\\&quot;, \\&quot;checkin\\&quot;: \\&quot;2022-03-15\\&quot;, \\&quot;checkout\\&quot;: \\&quot;2022-03-20\\&quot;, \\&quot;availability\\&quot;: \\&quot;Available\\&quot;, \\&quot;rooms_left\\&quot;: 5, \\&quot;price_per_night\\&quot;: 189, \\&quot;total_price\\&quot;: 945, \\&quot;cancellation_policy\\&quot;: \\&quot;Flexible until 5 days before arrival\\&quot;, \\&quot;special_offers\\&quot;: []}}&quot;}]&#x27;, &#x27;tools_json&#x27;: &#x27;[{&quot;type&quot;: &quot;function&quot;, &quot;function&quot;: {&quot;name&quot;: &quot;is_hotel_available&quot;, &quot;description&quot;: &quot;Checks the availability of a hotel for a given date range.&quot;, &quot;parameters&quot;: {&quot;type&quot;: &quot;object&quot;, &quot;properties&quot;: {&quot;hotel&quot;: {&quot;type&quot;: &quot;string&quot;, &quot;description&quot;: &quot;The name of the hotel.&quot;}, &quot;city&quot;: {&quot;type&quot;: &quot;string&quot;, &quot;description&quot;: &quot;The city where the hotel is located.&quot;}, &quot;checkin&quot;: {&quot;type&quot;: &quot;string&quot;, &quot;description&quot;: &quot;The check-in date in the format \\&quot;YYYY-MM-DD\\&quot;.&quot;}, &quot;checkout&quot;: {&quot;type&quot;: &quot;string&quot;, &quot;description&quot;: &quot;The check-out date in the format \\&quot;YYYY-MM-DD\\&quot;.&quot;}}, &quot;required&quot;: [&quot;hotel&quot;, &quot;city&quot;, &quot;checkin&quot;, &quot;checkout&quot;]}}}]&#x27;}
```

```
ds_conv[1]
```

```
{&#x27;messages_json&#x27;: &#x27;[{&quot;role&quot;: &quot;developer&quot;, &quot;content&quot;: &quot;You are a deep thinking AI, you may use extremely long chains of thought to deeply consider the problem and deliberate with yourself via systematic reasoning processes to help come to a correct solution prior to answering.&quot;}, {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;I am in New York City and I am looking for a restaurant within 500 meters. Can you help me?&quot;}, {&quot;role&quot;: &quot;assistant&quot;, &quot;tool_calls&quot;: [{&quot;function&quot;: {&quot;name&quot;: &quot;find_nearby_restaurants&quot;, &quot;arguments&quot;: {&quot;location&quot;: &quot;New York City&quot;, &quot;distance&quot;: 500}}}], &quot;thinking&quot;: &quot;Okay, the user is asking for nearby restaurants in New York City within 500 meters. Let me think about how to approach this. The available tools include find_nearby_restaurants. So the function find_nearby_restaurants takes a location and a distance as parameters. Since the user is in NYC, I need to specify the location as \\&quot;New York City\\&quot; and set the distance to 500 meters.\\n\\nWait, the function parameters require the location to be a string and the distance to be an integer. So I\&#x27;ll construct the call with location as \&#x27;New York City\&#x27; and distance 500. That should give the required restaurants.&quot;}, {&quot;role&quot;: &quot;tool&quot;, &quot;content&quot;: &quot;{\\&quot;restaurants\\&quot;: [{\\&quot;name\\&quot;: \\&quot;The Modern\\&quot;, \\&quot;distance\\&quot;: 300}, {\\&quot;name\\&quot;: \\&quot;Le Bernardin\\&quot;, \\&quot;distance\\&quot;: 450}, {\\&quot;name\\&quot;: \\&quot;Eleven Madison Park\\&quot;, \\&quot;distance\\&quot;: 500}]}&quot;}, {&quot;role&quot;: &quot;assistant&quot;, &quot;thinking&quot;: &quot;Okay, the assistant\&#x27;s response provided the nearby restaurants in NYC with their distances. The result shows three restaurants: The Modern (300m), Le Bernardin (450m), and Eleven Madison Park (500m). The user might want more details, so perhaps they could ask for more info like reviews or cuisine types. But since it\&#x27;s just a simple request, the answer is sufficient. No further action is needed.&quot;, &quot;content&quot;: &quot;The system found these restaurants within 500 meters of your location in New York City:\\n1. **The Modern** - 300 meters away\\n2. **Le Bernardin** - 450 meters away\\n3. **Eleven Madison Park** - 500 meters away\\n\\nWould you like additional details about any of these restaurants?&quot;}]&#x27;,
 &#x27;tools_json&#x27;: &#x27;[{&quot;type&quot;: &quot;function&quot;, &quot;function&quot;: {&quot;name&quot;: &quot;find_nearby_restaurants&quot;, &quot;description&quot;: &quot;Find nearby restaurants based on location&quot;, &quot;parameters&quot;: {&quot;type&quot;: &quot;object&quot;, &quot;properties&quot;: {&quot;location&quot;: {&quot;type&quot;: &quot;string&quot;, &quot;description&quot;: &quot;The location to search for restaurants&quot;}, &quot;distance&quot;: {&quot;type&quot;: &quot;integer&quot;, &quot;description&quot;: &quot;The maximum distance in meters&quot;}}, &quot;required&quot;: [&quot;location&quot;, &quot;distance&quot;]}}}, {&quot;type&quot;: &quot;function&quot;, &quot;function&quot;: {&quot;name&quot;: &quot;calculate_tip&quot;, &quot;description&quot;: &quot;Calculate the tip amount for a bill&quot;, &quot;parameters&quot;: {&quot;type&quot;: &quot;object&quot;, &quot;properties&quot;: {&quot;bill_amount&quot;: {&quot;type&quot;: &quot;number&quot;, &quot;description&quot;: &quot;The amount of the bill&quot;}, &quot;tip_percentage&quot;: {&quot;type&quot;: &quot;number&quot;, &quot;description&quot;: &quot;The percentage of tip to give&quot;}}, &quot;required&quot;: [&quot;bill_amount&quot;, &quot;tip_percentage&quot;]}}}]&#x27;}
```

```
save_path = &quot;hermes_200_gptoss.jsonl&quot;
with open(save_path, &quot;w&quot;, encoding=&quot;utf-8&quot;) as f:
    for ex in ds_conv:
        f.write(json.dumps({
            &quot;messages_json&quot;: ex[&quot;messages_json&quot;],
            &quot;tools_json&quot;: ex[&quot;tools_json&quot;],
        }, ensure_ascii=False) + &quot;\n&quot;)
print(&quot;Saved to:&quot;, os.path.abspath(save_path))
```

```
Saved to: /root/autodl-tmp/hermes_200_gptoss.jsonl
```

```
tokenizer
```

```
PreTrainedTokenizerFast(name_or_path=&#x27;./gpt-oss-20b-bf16&#x27;, vocab_size=199998, model_max_length=1000000000000000019884624838656, is_fast=True, padding_side=&#x27;left&#x27;, truncation_side=&#x27;right&#x27;, special_tokens={&#x27;bos_token&#x27;: &#x27;&lt;|startoftext|&gt;&#x27;, &#x27;eos_token&#x27;: &#x27;&lt;|return|&gt;&#x27;, &#x27;pad_token&#x27;: &#x27;&lt;|endoftext|&gt;&#x27;}, clean_up_tokenization_spaces=False, added_tokens_decoder={
        199998: AddedToken(&quot;&lt;|startoftext|&gt;&quot;, rstrip=False, lstrip=False, single_word=False, normalized=False, special=True),
        199999: AddedToken(&quot;&lt;|endoftext|&gt;&quot;, rstrip=False, lstrip=False, single_word=False, normalized=False, special=True),
        200000: AddedToken(&quot;&lt;|reserved_200000|&gt;&quot;, rstrip=False, lstrip=False, single_word=False, normalized=False, special=True),
        200001: AddedToken(&quot;&lt;|reserved_200001|&gt;&quot;, rstrip=False, lstrip=False, single_word=False, normalized=False, special=True),
        200002: AddedToken(&quot;&lt;|return|&gt;&quot;, rstrip=False, lstrip=False, single_word=False, normalized=False, special=True),
        200003: AddedToken(&quot;&lt;|constrain|&gt;&quot;, rstrip=False, lstrip=False, single_word=False, normalized=False, special=True),
        200004: AddedToken(&quot;&lt;|reserved_200004|&gt;&quot;, rstrip=False, lstrip=False, single_word=False, normalized=False, special=True),
        200005: AddedToken(&quot;&lt;|channel|&gt;&quot;, rstrip=False, lstrip=False, single_word=False, normalized=False, special=True),
        200006: AddedToken(&quot;&lt;|start|&gt;&quot;, rstrip=False, lstrip=False, single_word=False, normalized=False, special=True),
        200007: AddedToken(&quot;&lt;|end|&gt;&quot;, rstrip=False, lstrip=False, single_word=False, normalized=False, special=True),
        200008: AddedToken(&quot;&lt;|message|&gt;&quot;, rstrip=False, lstrip=False, single_word=False, normalized=False, special=True),
        200009: AddedToken(&quot;&lt;|reserved_200009|&gt;&quot;, rstrip=False, lstrip=False, single_word=False, normalized=False, special=True),
        200010: AddedToken(&quot;&lt;|reserved_200010|&gt;&quot;, rstrip=False, lstrip=False, single_word=False, normalized=False, special=True),
        200011: AddedToken(&quot;&lt;|reserved_200011|&gt;&quot;, rstrip=False, lstrip=False, single_word=False, normalized=False, special=True),
        200012: AddedToken(&quot;&lt;|call|&gt;&quot;, rstrip=False, lstrip=False, single_word=False, normalized=False, special=True),
        200013: AddedToken(&quot;&lt;|reserved_200013|&gt;&quot;, rstrip=False, lstrip=False, single_word=False, normalized=False, special=True),
        200014: AddedToken(&quot;&lt;|reserved_200014|&gt;&quot;, rstrip=False, lstrip=False, single_word=False, normalized=False, special=True),
        200015: AddedToken(&quot;&lt;|reserved_200015|&gt;&quot;, rstrip=False, lstrip=False, single_word=False, normalized=False, special=True),
        200016: AddedToken(&quot;&lt;|reserved_200016|&gt;&quot;, rstrip=False, lstrip=False, single_word=False, normalized=False, special=True),
        200017: AddedToken(&quot;&lt;|reserved_200017|&gt;&quot;, rstrip=False, lstrip=False, single_word=False, normalized=False, special=True),
        200018: AddedToken(&quot;&lt;|endofprompt|&gt;&quot;, rstrip=False, lstrip=False, single_word=False, normalized=False, special=True),
}
)
```

```
def _fix_tools_for_template(tools):
    out = []
    for t in tools or []:
        if not isinstance(t, dict): 
            continue
        fn = t.get(&quot;function&quot;) if (t.get(&quot;type&quot;) == &quot;function&quot; and isinstance(t.get(&quot;function&quot;), dict)) else t
        fn.setdefault(&quot;description&quot;, &quot;&quot;)
        params = fn.get(&quot;parameters&quot;) or {}
        if isinstance(params, dict):
            if params.get(&quot;type&quot;) == &quot;dict&quot;:
                params[&quot;type&quot;] = &quot;object&quot;                     # ① dict -&gt; object
            if params.get(&quot;type&quot;) == &quot;object&quot;:
                props = params.setdefault(&quot;properties&quot;, {})
                if not isinstance(props, dict): 
                    params[&quot;properties&quot;] = {}
                # 把外层 required 并进去
                if &quot;required&quot; not in params and isinstance(fn.get(&quot;required&quot;), list):
                    params[&quot;required&quot;] = fn[&quot;required&quot;]       # ② 合并 required
                # 保证每个属性有 type/description
                for k, spec in list(props.items()):
                    if not isinstance(spec, dict): 
                        spec = {}
                    spec.setdefault(&quot;type&quot;, &quot;string&quot;)
                    spec.setdefault(&quot;description&quot;, &quot;&quot;)
                    props[k] = spec
        out.append({&quot;type&quot;: &quot;function&quot;, &quot;function&quot;: fn})
    return out

def _fix_tool_messages(messages):
    for m in messages:
        if m.get(&quot;role&quot;) == &quot;tool&quot; and isinstance(m.get(&quot;content&quot;), str):
            s = m[&quot;content&quot;]
            try:
                m[&quot;content&quot;] = json.loads(s)                  # ③ 字符串 -&gt; 对象
            except Exception:
                try:
                    import ast
                    m[&quot;content&quot;] = ast.literal_eval(s)
                except Exception:
                    pass
    return messages

def render_batch(batch):
    texts = []
    for m_str, t_str in zip(batch[&quot;messages_json&quot;], batch[&quot;tools_json&quot;]):
        messages = json.loads(m_str)
        tools = json.loads(t_str)

        messages = _fix_tool_messages(messages)
        tools = _fix_tools_for_template(tools)

        txt = tokenizer.apply_chat_template(
            messages,
            tools=tools,
            add_generation_prompt=False,
            reasoning_effort=&quot;low&quot;,
            tokenize=False,
        )
        texts.append(txt)
    return {&quot;text&quot;: texts}
```

```
ds_text = ds_conv.map(render_batch, batched=True)
```

```
Map: 100%|██████████| 200/200 [00:00&lt;00:00, 1927.02 examples/s]
```

```
ds_text
```

```
Dataset({
    features: [&#x27;messages_json&#x27;, &#x27;tools_json&#x27;, &#x27;text&#x27;],
    num_rows: 200
})
```

完整数据集地址：

![](img/f467febb2442a465444752ca05981e8f.png)

![](img/b18b9944209fd4ac75cb7965395b4878.png)

![](img/456ddb28ea4e0a133e2b74fc7159fb6a.png)

我们抽取其中几条数据进行查看：

```
ds_text[0][&#x27;text&#x27;]
```

```
&#x27;&lt;|start|&gt;system&lt;|message|&gt;You are ChatGPT, a large language model trained by OpenAI.\nKnowledge cutoff: 2024-06\nCurrent date: 2025-08-25\n\nReasoning: low\n\n# Valid channels: analysis, commentary, final. Channel must be included for every message.\nCalls to these tools must go to the commentary channel: \&#x27;functions\&#x27;.&lt;|end|&gt;&lt;|start|&gt;developer&lt;|message|&gt;# Instructions\n\nYou are a deep thinking AI, you may use extremely long chains of thought to deeply consider the problem and deliberate with yourself via systematic reasoning processes to help come to a correct solution prior to answering.\n\n# Tools\n\n## functions\n\nnamespace functions {\n\n// Checks the availability of a hotel for a given date range.\ntype is_hotel_available = (_: {\n// The name of the hotel.\nhotel: string,\n// The city where the hotel is located.\ncity: string,\n// The check-in date in the format &quot;YYYY-MM-DD&quot;.\ncheckin: string,\n// The check-out date in the format &quot;YYYY-MM-DD&quot;.\ncheckout: string\n}) =&gt; any;\n\n} // namespace functions&lt;|end|&gt;&lt;|start|&gt;user&lt;|message|&gt;I want to book a hotel in Berlin from 3/15/2022 to 3/20/2022. Is the Queens Hotel available?&lt;|end|&gt;&lt;|start|&gt;assistant to=functions.is_hotel_available&lt;|channel|&gt;commentary json&lt;|message|&gt;{&quot;hotel&quot;: &quot;Queens Hotel&quot;, &quot;city&quot;: &quot;Berlin, Germany&quot;, &quot;checkin&quot;: &quot;2022-03-15&quot;, &quot;checkout&quot;: &quot;2022-03-20&quot;}&lt;|call|&gt;&lt;|start|&gt;functions.is_hotel_available to=assistant&lt;|channel|&gt;commentary&lt;|message|&gt;{&quot;name&quot;: &quot;is_hotel_available&quot;, &quot;content&quot;: {&quot;hotel&quot;: &quot;Queens Hotel&quot;, &quot;city&quot;: &quot;Berlin, Germany&quot;, &quot;checkin&quot;: &quot;2022-03-15&quot;, &quot;checkout&quot;: &quot;2022-03-20&quot;, &quot;availability&quot;: &quot;Available&quot;, &quot;rooms_left&quot;: 5, &quot;price_per_night&quot;: 189, &quot;total_price&quot;: 945, &quot;cancellation_policy&quot;: &quot;Flexible until 5 days before arrival&quot;, &quot;special_offers&quot;: []}}&lt;|end|&gt;&#x27;
```

```
ds_text[1][&#x27;text&#x27;]
```

```
&#x27;&lt;|start|&gt;system&lt;|message|&gt;You are ChatGPT, a large language model trained by OpenAI.\nKnowledge cutoff: 2024-06\nCurrent date: 2025-08-25\n\nReasoning: low\n\n# Valid channels: analysis, commentary, final. Channel must be included for every message.\nCalls to these tools must go to the commentary channel: \&#x27;functions\&#x27;.&lt;|end|&gt;&lt;|start|&gt;developer&lt;|message|&gt;# Instructions\n\nYou are a deep thinking AI, you may use extremely long chains of thought to deeply consider the problem and deliberate with yourself via systematic reasoning processes to help come to a correct solution prior to answering.\n\n# Tools\n\n## functions\n\nnamespace functions {\n\n// Find nearby restaurants based on location\ntype find_nearby_restaurants = (_: {\n// The location to search for restaurants\nlocation: string,\n// The maximum distance in meters\ndistance: number\n}) =&gt; any;\n\n// Calculate the tip amount for a bill\ntype calculate_tip = (_: {\n// The amount of the bill\nbill_amount: number,\n// The percentage of tip to give\ntip_percentage: number\n}) =&gt; any;\n\n} // namespace functions&lt;|end|&gt;&lt;|start|&gt;user&lt;|message|&gt;I am in New York City and I am looking for a restaurant within 500 meters. Can you help me?&lt;|end|&gt;&lt;|start|&gt;assistant to=functions.find_nearby_restaurants&lt;|channel|&gt;commentary json&lt;|message|&gt;{&quot;location&quot;: &quot;New York City&quot;, &quot;distance&quot;: 500}&lt;|call|&gt;&lt;|start|&gt;functions.find_nearby_restaurants to=assistant&lt;|channel|&gt;commentary&lt;|message|&gt;{&quot;restaurants&quot;: [{&quot;name&quot;: &quot;The Modern&quot;, &quot;distance&quot;: 300}, {&quot;name&quot;: &quot;Le Bernardin&quot;, &quot;distance&quot;: 450}, {&quot;name&quot;: &quot;Eleven Madison Park&quot;, &quot;distance&quot;: 500}]}&lt;|end|&gt;&lt;|start|&gt;assistant&lt;|channel|&gt;analysis&lt;|message|&gt;Okay, the assistant\&#x27;s response provided the nearby restaurants in NYC with their distances. The result shows three restaurants: The Modern (300m), Le Bernardin (450m), and Eleven Madison Park (500m). The user might want more details, so perhaps they could ask for more info like reviews or cuisine types. But since it\&#x27;s just a simple request, the answer is sufficient. No further action is needed.&lt;|end|&gt;&lt;|start|&gt;assistant&lt;|channel|&gt;final&lt;|message|&gt;The system found these restaurants within 500 meters of your location in New York City:\n1. **The Modern** - 300 meters away\n2. **Le Bernardin** - 450 meters away\n3. **Eleven Madison Park** - 500 meters away\n\nWould you like additional details about any of these restaurants?&lt;|return|&gt;&#x27;
```

```
ds_text[2][&#x27;text&#x27;]
```

```
&#x27;&lt;|start|&gt;system&lt;|message|&gt;You are ChatGPT, a large language model trained by OpenAI.\nKnowledge cutoff: 2024-06\nCurrent date: 2025-08-25\n\nReasoning: low\n\n# Valid channels: analysis, commentary, final. Channel must be included for every message.\nCalls to these tools must go to the commentary channel: \&#x27;functions\&#x27;.&lt;|end|&gt;&lt;|start|&gt;developer&lt;|message|&gt;# Instructions\n\nYou are a deep thinking AI, you may use extremely long chains of thought to deeply consider the problem and deliberate with yourself via systematic reasoning processes to help come to a correct solution prior to answering.\n\n# Tools\n\n## functions\n\nnamespace functions {\n\n// Search for a given keyword in the English translation of the Quran and return the verses containing the keyword and total matches in JSON format.\ntype search_for_a_word_in_quran = (_: {\n// The keyword or phrase to search for in the Quran.\nsearchterm: string\n}) =&gt; any;\n\n// Calculates the CO2 emissions in kilograms from the use of public transportation based on the distance traveled and type of transit.\ntype carbonfootprintfrompublictransit = (_: {\n// The distance traveled in kilometers.\ndistance: string,\n// The type of transportation, one of: \&#x27;Taxi\&#x27;, \&#x27;ClassicBus\&#x27;, \&#x27;EcoBus\&#x27;, \&#x27;Coach\&#x27;, \&#x27;NationalTrain\&#x27;, \&#x27;LightRail\&#x27;, \&#x27;Subway\&#x27;, \&#x27;FerryOnFoot\&#x27;, \&#x27;FerryInCar\&#x27;.\ntype: string\n}) =&gt; any;\n\n// Calculate the number of trees required to create a given weight of paper.\ntype treeequivalent = (_: {\n// The weight of the paper.\nweight: string,\n// The unit of measurement for the weight (e.g., \&#x27;kg\&#x27; or \&#x27;lb\&#x27;).\nunit: string\n}) =&gt; any;\n\n// Fetches the entire Surah (chapter) of the Koran, including all verses and some additional information, based on the provided chapter ID.\ntype get_entire_surah_chapter = (_: {\n// The unique identifier for a Surah (chapter) in the Koran. Must be between 1 and 114, inclusive.\nchapterid: number\n}) =&gt; any;\n\n} // namespace functions&lt;|end|&gt;&lt;|start|&gt;user&lt;|message|&gt;Could you help me with two things? First, search the Quran for the term \&#x27;kindness\&#x27; and second, find out how many trees are needed to produce 600 lb of paper?&lt;|end|&gt;&lt;|start|&gt;assistant to=functions.search_for_a_word_in_quran&lt;|channel|&gt;commentary json&lt;|message|&gt;{&quot;searchterm&quot;: &quot;kindness&quot;}&lt;|call|&gt;&lt;|start|&gt;assistant to=functions.treeequivalent&lt;|channel|&gt;commentary json&lt;|message|&gt;{&quot;weight&quot;: &quot;600&quot;, &quot;unit&quot;: &quot;lb&quot;}&lt;|call|&gt;&#x27;
```

```
ds_text[3][&#x27;text&#x27;]
```

```
&#x27;&lt;|start|&gt;system&lt;|message|&gt;You are ChatGPT, a large language model trained by OpenAI.\nKnowledge cutoff: 2024-06\nCurrent date: 2025-08-25\n\nReasoning: low\n\n# Valid channels: analysis, commentary, final. Channel must be included for every message.\nCalls to these tools must go to the commentary channel: \&#x27;functions\&#x27;.&lt;|end|&gt;&lt;|start|&gt;developer&lt;|message|&gt;# Instructions\n\nYou are a deep thinking AI, you may use extremely long chains of thought to deeply consider the problem and deliberate with yourself via systematic reasoning processes to help come to a correct solution prior to answering.\n\n# Tools\n\n## functions\n\nnamespace functions {\n\n// Fetches and returns a list of all available stock symbols from the real-time-quotes API.\ntype stock_symbols = (_: {\n// If True, includes stock names in the returned data. Defaults to None.\nincludenames?: any, // default: &quot;false&quot;\n}) =&gt; any;\n\n// Fetch the Total Value Locked (TVL) for a specified DeFi protocol from the RapidAPI Toolbench.\ntype tvl_protocol = (_: {\n// The slug identifier for the DeFi protocol.\nprotocol?: any, // default: &quot;&quot;\n}) =&gt; any;\n\n} // namespace functions&lt;|end|&gt;&lt;|start|&gt;user&lt;|message|&gt;I need the total value locked for MakerDAO.&lt;|end|&gt;&lt;|start|&gt;assistant&lt;|channel|&gt;analysis&lt;|message|&gt;Okay, let me think about this. The user wants the total value locked (TVL) for MakerDAO. The tvl_protocol tool seems relevant here. The parameters for tvl_protocol require a specific protocol slug. I need to figure out the correct slug for MakerDAO. I remember that slugs are usually derived from the protocol\&#x27;s name or a shortened version. MakerDAO is a well-known DeFi protocol, so maybe the slug is maker or makerdao. Let me check... Hmm, sometimes they might use the full name as the slug. Let me see if I can recall or infer the exact slug. Wait, I think the correct slug for MakerDAO is &quot;maker&quot; or maybe &quot;makerdao&quot;. Let me check the available tools again. Since the parameters require the protocol as a string, I should use the most accurate value. I think the slug is &quot;maker&quot;. So the function call would be tvl_protocol with protocol set to &quot;maker&quot;.&lt;|end|&gt;&lt;|start|&gt;assistant&lt;|channel|&gt;final&lt;|message|&gt;Apologies, but I\&#x27;m unable to provide the Total Value Locked (TVL) for MakerDAO at this time. The `tvl_protocol` tool requires a specific protocol slug as input, which I don\&#x27;t have. Without the correct slug identifier, I can\&#x27;t fetch the requested data. Could you please provide the exact name or identifier for the MakerDAO protocol? Knowing the precise name or any other identifying details would help me retrieve the accurate TVL.&lt;|return|&gt;&#x27;
```

```
ds_text[5][&#x27;text&#x27;]
```

```
&#x27;&lt;|start|&gt;system&lt;|message|&gt;You are ChatGPT, a large language model trained by OpenAI.\nKnowledge cutoff: 2024-06\nCurrent date: 2025-08-25\n\nReasoning: low\n\n# Valid channels: analysis, commentary, final. Channel must be included for every message.\nCalls to these tools must go to the commentary channel: \&#x27;functions\&#x27;.&lt;|end|&gt;&lt;|start|&gt;developer&lt;|message|&gt;# Instructions\n\nYou are a deep thinking AI, you may use extremely long chains of thought to deeply consider the problem and deliberate with yourself via systematic reasoning processes to help come to a correct solution prior to answering.\n\n# Tools\n\n## functions\n\nnamespace functions {\n\n// Fetch a list of hosts that ran an EcoIndex analysis.\n\n    Args:\n        version (str): Engine version used for analysis (v0 or v1).\n        q (str, optional): Filter for partial host name.\n        date_from (str, optional): Start date for filtering (YYYY-MM-DD).\n        size (int, optional): Number of elements per page. Defaults to 50.\n        date_to (str, optional): End date for filtering (YYYY-MM-DD).\n        page (int, optional): Page number to retrieve. Defaults to 1.\n\n    Returns:\n        List of hosts ordered by number of requests made.\n    \ntype get_host_list_version_hosts_get = (_: {\n// Engine version used to run the analysis (v0 or v1).\nversion: string,\n// Filter for partial host name. Defaults to None.\nq?: string,\n// Start date for filtering elements (format: YYYY-MM-DD). Defaults to None.\ndate_from?: string,\n// Number of elements per page. Defaults to 50.\nsize?: number,\n// End date for filtering elements (format: YYYY-MM-DD). Defaults to None.\ndate_to?: string,\n// Page number to retrieve. Defaults to 1.\npage?: number\n}) =&gt; any;\n\n// Fetch horoscope information for a given astrological sign.\n\n    Args:\n        s (str): The astrological sign to fetch information for.\n\n    Returns:\n        Horoscope information for the specified sign.\n\n    Raises:\n        ValueError: If an invalid sign is provided.\n    \ntype sign = (_: {\n// The astrological sign to fetch information for. Valid options include \&#x27;aries\&#x27;, \&#x27;taurus\&#x27;, \&#x27;gemini\&#x27;, \&#x27;cancer\&#x27;, \&#x27;leo\&#x27;, \&#x27;virgo\&#x27;, \&#x27;libra\&#x27;,\&#x27;scorpio\&#x27;,\&#x27;sagittarius\&#x27;, \&#x27;capricorn\&#x27;, \&#x27;aquarius\&#x27;, and \&#x27;pisces\&#x27;.\ns: string\n}) =&gt; any;\n\n// Fetch values from a specified data subset within a dataset.\n\n    Args:\n        subset (str): Identifier for the data subset.\n        dataset (str): Identifier for the dataset.\n        limit (int, optional): Limits output table lines. Defaults to 10. 0 means no limit.\n        debug (int, optional): Debug status. Non-zero enables debug mode. Defaults to 0.\n        format (str, optional): Output format. Defaults to \&#x27;tsv\&#x27;.\n\n    Returns:\n        Values from the specified data subset.\n    \ntype tsv2 = (_: {\n// Identifier for the data subset.\nsubset: string,\n// Identifier for the dataset.\ndataset: string,\n// Limits the number of lines in the output table. Defaults to 10. A value of 0 means no limit.\nlimit?: number,\n// Debug status. When set to a non-zero value, debug mode is enabled. Defaults to 0.\ndebug?: number,\n// Output format. Defaults to \&#x27;tsv\&#x27;.\nformat?: string\n}) =&gt; any;\n\n// Fetch order details given an order ID.\n\n    Args:\n        orderid (int): The ID of the order to be fetched.\n\n    Returns:\n        Order details for the specified ID.\n\n    Raises:\n        Exception: For IDs &gt; 5 and &lt;= 10.\n    \ntype getorderbyid = (_: {\n// The ID of the order to be fetched. For a valid response, use IDs with values &lt;= 5 or &gt; 10. Other values may generate exceptions.\norderid: number\n}) =&gt; any;\n\n// Fetch a list of quotes for a given genre.\n\n    Args:\n        name (str): The name of the genre to fetch quotes for.\n\n    Returns:\n        List of quotes for the specified genre.\n    \ntype get_quotes_by_genre = (_: {\n// The name of the genre to fetch quotes for.\nname: string\n}) =&gt; any;\n\n// Fetch the list of recitations for a specific Ayah.\n\n    Args:\n        recitation_id (int): The ID of the recitation.\n        ayah_key (str): The key of the Ayah (e.g., \&#x27;1:1\&#x27; for first Ayah of first Surah).\n\n    Returns:\n        List of recitations for the specified Ayah.\n    \ntype ayah_recitations = (_: {\n// The ID of the recitation.\nrecitation_id: number,\n// The key of the Ayah, which is a combination of the Surah number and Ayah number (e.g., \&#x27;1:1\&#x27; for the first Ayah of the first Surah).\nayah_key: string\n}) =&gt; any;\n\n} // namespace functions&lt;|end|&gt;&lt;|start|&gt;user&lt;|message|&gt;What are today\&#x27;s horoscope predictions for a person born under the sign of Virgo?&lt;|end|&gt;&lt;|start|&gt;assistant to=functions.sign&lt;|channel|&gt;commentary json&lt;|message|&gt;{&quot;s&quot;: &quot;virgo&quot;}&lt;|call|&gt;&lt;|start|&gt;functions.sign to=assistant&lt;|channel|&gt;commentary&lt;|message|&gt;{&quot;tool_call_id&quot;: &quot;call_X2BFdg8e0abUqscZyrg1VM1z&quot;, &quot;name&quot;: &quot;sign&quot;, &quot;content&quot;: &quot;{\&#x27;sign\&#x27;: \&#x27;Virgo\&#x27;, \&#x27;date\&#x27;: \&#x27;2024-06-28\&#x27;, \&#x27;prediction\&#x27;: \\&quot;Today is a day of heightened intuition and analytical prowess for Virgo. Your ability to notice details others miss will be particularly strong, allowing you to solve complex problems with ease. In your professional life, your meticulous nature will be greatly appreciated - you may find yourself taking on a leadership role in an important project. Financially, it\&#x27;s a good time to review your budget and make adjustments where necessary. In relationships, open and honest communication will strengthen your bonds. Take some time for self-care, perhaps through organizing your living space or indulging in a healthy, home-cooked meal.\\&quot;, \&#x27;lucky_number\&#x27;: 6,\&#x27;mood\&#x27;: \&#x27;Analytical\&#x27;, \&#x27;compatibility\&#x27;: \&#x27;Capricorn\&#x27;}&quot;}&lt;|end|&gt;&lt;|start|&gt;assistant&lt;|channel|&gt;final&lt;|message|&gt;Today\&#x27;s Virgo horoscope (June 28, 2024):  \n**Key Themes:**  \n- Heightened intuition and attention to detail.  \n- Leadership opportunities in projects requiring precision.  \n- Financial review and budget adjustments.  \n- Strengthened relationships through honest communication.  \n\nLucky number: 6  \nMood: Analytical  \nBest compatibility: Capricorn.  \n\nTake time today to appreciate your ability to solve complex problems and nurture connections. 🌟&lt;|end|&gt;&lt;|start|&gt;user&lt;|message|&gt;Can you provide the horoscope predictions for tomorrow for a person born under the sign of Virgo?\n\nPlease retrieve tomorrow\&#x27;s prediction for Virgo using their zodiac sign name.&lt;|end|&gt;&lt;|start|&gt;assistant to=functions.sign&lt;|channel|&gt;commentary json&lt;|message|&gt;{&quot;s&quot;: &quot;virgo&quot;, &quot;date&quot;: &quot;2024-06-29&quot;}&lt;|call|&gt;&lt;|start|&gt;functions.sign to=assistant&lt;|channel|&gt;commentary&lt;|message|&gt;{&quot;tool_call_id&quot;: &quot;call_9dMxtk9NPuAXXi7zS2v4ismo&quot;, &quot;name&quot;: &quot;sign&quot;, &quot;content&quot;: &quot;{\&#x27;sign\&#x27;: \&#x27;Virgo\&#x27;, \&#x27;date\&#x27;: \&#x27;2024-06-29\&#x27;, \&#x27;prediction\&#x27;: \\&quot;Tomorrow brings a day of heightened intuition and analytical prowess for Virgo. Your ability to notice subtle details will be particularly strong, allowing you to make significant progress on complex projects. In your professional life, your meticulous nature will be greatly appreciated - you may find yourself taking on a mentoring role or leading an important initiative. Financially, it\&#x27;s an excellent time to review your long-term goals and make adjustments to your savings strategy. In relationships, clear and honest communication will be key to strengthening your bonds. Take some time for self-care, perhaps through organizing your living space or indulging in a healthy, home-cooked meal. Your practical approach combined with a touch of creativity will lead to innovative solutions.\\&quot;, \&#x27;lucky_number\&#x27;: 7,\&#x27;mood\&#x27;: \&#x27;Insightful\&#x27;, \&#x27;compatibility\&#x27;: \&#x27;Capricorn\&#x27;}&quot;}&lt;|end|&gt;&lt;|start|&gt;assistant&lt;|channel|&gt;analysis&lt;|message|&gt;The user wants tomorrow\&#x27;s horoscope for Virgo. The tool returned a detailed prediction that includes the sign, date, prediction, lucky number, mood, and compatibility. The prediction talks about heightened intuition, leading projects requiring precision, mentoring roles, financial goals review, communication in relationships, self-care through organization or cooking, and a blend of analytical and creative approaches. I\&#x27;ll summarize this in a concise, friendly manner to provide the user with the essential information they requested.&lt;|end|&gt;&lt;|start|&gt;assistant&lt;|channel|&gt;final&lt;|message|&gt;Tomorrow\&#x27;s Virgo horoscope (June 29, 2024):  \n**Key Themes:**  \n- Enhanced intuitive insight and attention to detail.  \n- Leadership in mentoring or leading initiatives.  \n- Review of long-term financial goals.  \n- Strengthened relationships through clear communication.  \n\nLucky number: 7  \nMood: Insightful  \nBest compatibility: Capricorn.  \n\nTake time today to leverage your analytical skills while allowing creativity to inform your decisions. 🧩🔮&lt;|return|&gt;&#x27;
```

```
ds_text[6][&#x27;text&#x27;]
```

```
&#x27;&lt;|start|&gt;system&lt;|message|&gt;You are ChatGPT, a large language model trained by OpenAI.\nKnowledge cutoff: 2024-06\nCurrent date: 2025-08-25\n\nReasoning: low\n\n# Valid channels: analysis, commentary, final. Channel must be included for every message.\nCalls to these tools must go to the commentary channel: \&#x27;functions\&#x27;.&lt;|end|&gt;&lt;|start|&gt;developer&lt;|message|&gt;# Instructions\n\nYou are a deep thinking AI, you may use extremely long chains of thought to deeply consider the problem and deliberate with yourself via systematic reasoning processes to help come to a correct solution prior to answering.\n\n# Tools\n\n## functions\n\nnamespace functions {\n\n// Fetches the follower list for a specified user from the TikTok API.\ntype get_user_follower_list = (_: {\n// The ID of the user whose followers are to be fetched.\nuser_id: string,\n// The number of followers to retrieve, with a maximum of 200. Defaults to \&#x27;50\&#x27;.\ncount: string,\n// A timestamp indicating the starting point for the followers list. If there are more followers to retrieve, this value should be updated. Defaults to \&#x27;0\&#x27;.\ntime: string\n}) =&gt; any;\n\n// Fetch the Instagram user ID associated with a given username.\ntype get_user_id_by_username = (_: {\n// The Instagram username for which the user ID needs to be retrieved.\nusername: string\n}) =&gt; any;\n\n// Fetch the challenge details from TikTok based on a provided hashtag.\ntype challenge = (_: {\n// The hashtag of the challenge to fetch details for.\nhashtag: string\n}) =&gt; any;\n\n} // namespace functions&lt;|end|&gt;&lt;|start|&gt;user&lt;|message|&gt;Please get the follower count for TikTok user \&#x27;987654321\&#x27; and find the Instagram user ID for \&#x27;anotheruser456\&#x27;.&lt;|end|&gt;&lt;|start|&gt;assistant to=functions.get_user_follower_list&lt;|channel|&gt;commentary json&lt;|message|&gt;{&quot;user_id&quot;: &quot;987654321&quot;, &quot;count&quot;: &quot;50&quot;, &quot;time&quot;: &quot;0&quot;}&lt;|call|&gt;&lt;|start|&gt;assistant to=functions.get_user_id_by_username&lt;|channel|&gt;commentary json&lt;|message|&gt;{&quot;username&quot;: &quot;anotheruser456&quot;}&lt;|call|&gt;&#x27;
```

## 【微调实战二】微调GPT-OSS模型Agent性能微调

当我们准备好了微调数据集之后，实际的微调过程并不会太复杂。

+   **Step 1.读取数据集**

```
from datasets import load_dataset

dataset = load_dataset(&quot;json&quot;, data_files=&quot;hermes_200_gptoss.jsonl&quot;, split=&quot;train&quot;)
print(dataset[0])   # 查看第一条
```

```
{&#x27;messages_json&#x27;: &#x27;[{&quot;role&quot;: &quot;developer&quot;, &quot;content&quot;: &quot;You are a deep thinking AI, you may use extremely long chains of thought to deeply consider the problem and deliberate with yourself via systematic reasoning processes to help come to a correct solution prior to answering.&quot;}, {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;I want to book a hotel in Berlin from 3/15/2022 to 3/20/2022. Is the Queens Hotel available?&quot;}, {&quot;role&quot;: &quot;assistant&quot;, &quot;tool_calls&quot;: [{&quot;function&quot;: {&quot;name&quot;: &quot;is_hotel_available&quot;, &quot;arguments&quot;: {&quot;hotel&quot;: &quot;Queens Hotel&quot;, &quot;city&quot;: &quot;Berlin, Germany&quot;, &quot;checkin&quot;: &quot;2022-03-15&quot;, &quot;checkout&quot;: &quot;2022-03-20&quot;}}}], &quot;thinking&quot;: &quot;Okay, the user wants to check the availability of the Queens Hotel in Berlin from March 15th to March 20th, 2022. Let me start by recalling the available tools. The only tool provided here is the hotel availability checker, which is called \\&quot;is_hotel_available\\&quot;. \\n\\nSo, I need to construct the proper function call for that tool. The parameters required are hotel, city, checkin, and checkout. The user mentioned the hotel is Queens Hotel, city is Berlin, check-in is March 15, 2022 (which should be formatted as YYYY-MM-DD, so 2022-03-15), and check-out is March 20, 2022 (2022-03-20). \\n\\nI\&#x27;ll call the is_hotel_available function with these parameters and then present the response. The Python code will need to use the function name and pass the dictionary with the required details. Once the function is called, the result will be in JSON format, which I should parse to show the availability information.&quot;}, {&quot;role&quot;: &quot;tool&quot;, &quot;content&quot;: &quot;{\\&quot;name\\&quot;: \\&quot;is_hotel_available\\&quot;, \\&quot;content\\&quot;: {\\&quot;hotel\\&quot;: \\&quot;Queens Hotel\\&quot;, \\&quot;city\\&quot;: \\&quot;Berlin, Germany\\&quot;, \\&quot;checkin\\&quot;: \\&quot;2022-03-15\\&quot;, \\&quot;checkout\\&quot;: \\&quot;2022-03-20\\&quot;, \\&quot;availability\\&quot;: \\&quot;Available\\&quot;, \\&quot;rooms_left\\&quot;: 5, \\&quot;price_per_night\\&quot;: 189, \\&quot;total_price\\&quot;: 945, \\&quot;cancellation_policy\\&quot;: \\&quot;Flexible until 5 days before arrival\\&quot;, \\&quot;special_offers\\&quot;: []}}&quot;}]&#x27;, &#x27;tools_json&#x27;: &#x27;[{&quot;type&quot;: &quot;function&quot;, &quot;function&quot;: {&quot;name&quot;: &quot;is_hotel_available&quot;, &quot;description&quot;: &quot;Checks the availability of a hotel for a given date range.&quot;, &quot;parameters&quot;: {&quot;type&quot;: &quot;object&quot;, &quot;properties&quot;: {&quot;hotel&quot;: {&quot;type&quot;: &quot;string&quot;, &quot;description&quot;: &quot;The name of the hotel.&quot;}, &quot;city&quot;: {&quot;type&quot;: &quot;string&quot;, &quot;description&quot;: &quot;The city where the hotel is located.&quot;}, &quot;checkin&quot;: {&quot;type&quot;: &quot;string&quot;, &quot;description&quot;: &quot;The check-in date in the format \\&quot;YYYY-MM-DD\\&quot;.&quot;}, &quot;checkout&quot;: {&quot;type&quot;: &quot;string&quot;, &quot;description&quot;: &quot;The check-out date in the format \\&quot;YYYY-MM-DD\\&quot;.&quot;}}, &quot;required&quot;: [&quot;hotel&quot;, &quot;city&quot;, &quot;checkin&quot;, &quot;checkout&quot;]}}}]&#x27;}
```

+   **Step 2.数据格式转化**

有了数据集之后，我们还需要将数据集转化为gpt-oss能够识别的数据格式，首先第一步是需要将其转化为gpt-oss的基本消息格式：

```
ds_text = dataset.map(render_batch, batched=True)
```

```
ds_text
```

```
Dataset({
    features: [&#x27;messages_json&#x27;, &#x27;tools_json&#x27;, &#x27;text&#x27;],
    num_rows: 200
})
```

其中'text'字段中就保存着可以用于微调的完整文本结构，其中assistant之前是输入，之后是输出：

```
print(ds_text[0][&#x27;text&#x27;])
```

```
&lt;|start|&gt;system&lt;|message|&gt;You are ChatGPT, a large language model trained by OpenAI.
Knowledge cutoff: 2024-06
Current date: 2025-08-25

Reasoning: low

# Valid channels: analysis, commentary, final. Channel must be included for every message.
Calls to these tools must go to the commentary channel: &#x27;functions&#x27;.&lt;|end|&gt;&lt;|start|&gt;developer&lt;|message|&gt;# Instructions

You are a deep thinking AI, you may use extremely long chains of thought to deeply consider the problem and deliberate with yourself via systematic reasoning processes to help come to a correct solution prior to answering.

# Tools

## functions

namespace functions {

// Checks the availability of a hotel for a given date range.
type is_hotel_available = (_: {
// The name of the hotel.
hotel: string,
// The city where the hotel is located.
city: string,
// The check-in date in the format &quot;YYYY-MM-DD&quot;.
checkin: string,
// The check-out date in the format &quot;YYYY-MM-DD&quot;.
checkout: string
}) =&gt; any;

} // namespace functions&lt;|end|&gt;&lt;|start|&gt;user&lt;|message|&gt;I want to book a hotel in Berlin from 3/15/2022 to 3/20/2022. Is the Queens Hotel available?&lt;|end|&gt;&lt;|start|&gt;assistant to=functions.is_hotel_available&lt;|channel|&gt;commentary json&lt;|message|&gt;{&quot;hotel&quot;: &quot;Queens Hotel&quot;, &quot;city&quot;: &quot;Berlin, Germany&quot;, &quot;checkin&quot;: &quot;2022-03-15&quot;, &quot;checkout&quot;: &quot;2022-03-20&quot;}&lt;|call|&gt;&lt;|start|&gt;functions.is_hotel_available to=assistant&lt;|channel|&gt;commentary&lt;|message|&gt;{&quot;name&quot;: &quot;is_hotel_available&quot;, &quot;content&quot;: {&quot;hotel&quot;: &quot;Queens Hotel&quot;, &quot;city&quot;: &quot;Berlin, Germany&quot;, &quot;checkin&quot;: &quot;2022-03-15&quot;, &quot;checkout&quot;: &quot;2022-03-20&quot;, &quot;availability&quot;: &quot;Available&quot;, &quot;rooms_left&quot;: 5, &quot;price_per_night&quot;: 189, &quot;total_price&quot;: 945, &quot;cancellation_policy&quot;: &quot;Flexible until 5 days before arrival&quot;, &quot;special_offers&quot;: []}}&lt;|end|&gt;
```

+   **Step 3.LoRA参数灌注**

```
model = FastLanguageModel.get_peft_model(
    model,
    r = 8, # Choose any number &gt; 0 ! Suggested 8, 16, 32, 64, 128
    target_modules = [&quot;q_proj&quot;, &quot;k_proj&quot;, &quot;v_proj&quot;, &quot;o_proj&quot;,
                      &quot;gate_proj&quot;, &quot;up_proj&quot;, &quot;down_proj&quot;,],
    lora_alpha = 16,
    lora_dropout = 0, # Supports any, but = 0 is optimized
    bias = &quot;none&quot;,    # Supports any, but = &quot;none&quot; is optimized
    # [NEW] &quot;unsloth&quot; uses 30% less VRAM, fits 2x larger batch sizes!
    use_gradient_checkpointing = &quot;unsloth&quot;, # True or &quot;unsloth&quot; for very long context
    random_state = 3407,
    use_rslora = False,  # We support rank stabilized LoRA
    loftq_config = None, # And LoftQ
)
```

```
Unsloth: Making `model.base_model.model.model` require gradients
```

基本参数说明如下：

| 参数 | 含义 | 推荐值/当前设置 | 作用与说明 |
| --- | --- | --- | --- |
| **r** | LoRA 秩（rank） | 8（可选 8/16/32/64/128） | 控制新增参数规模；值越大表达能力越强，但显存占用也随之上升。中文推理链推荐从 8/16 起步。 |
| **target_modules** | 注入的线性层 | q/k/v/o 投影 + MLP (gate/up/down) | 指定在哪些层挂 LoRA。覆盖注意力 + MLP → 更好适配中文句法与逻辑；若显存不足可只保留 q/k/v/o。 |
| **lora_alpha** | LoRA 缩放系数 | 16 | 放大 LoRA 分支输出，等效调整学习率。常用范围 16–32，r 小时可适当调高。 |
| **lora_dropout** | LoRA dropout 概率 | 0 | 控制 LoRA 分支的随机丢弃率。0 在 Unsloth 中优化过，更快更省；若过拟合可尝试 0.05–0.1。 |
| **bias** | Bias 参数训练策略 | &quot;none&quot; | 是否对注入层 bias 可训练。&quot;none&quot; 开销最小且优化最好。 |
| use_gradient_checkpointing | 梯度检查点 | &quot;unsloth&quot; | 节省 ~30% 显存，支持更长上下文和更大 batch；推荐开启。 |
| **random_state** | 随机种子 | 3407 | 保证实验可复现；任意固定值均可。 |
| **use_rslora** | Rank-Stabilized LoRA | False | 在大 r（≥64）时可提高稳定性；一般场景不必启用。 |
| **loftq_config** | LoftQ 量化配置 | None | 结合量化训练 LoRA，在 4/8bit 推理时更稳。默认关闭；若目标是低比特部署，可配置启用。 |

+   **Step 4.设置微调参数**

```
ds_text
```

```
Dataset({
    features: [&#x27;messages_json&#x27;, &#x27;tools_json&#x27;, &#x27;text&#x27;],
    num_rows: 200
})
```

```
from trl import SFTConfig, SFTTrainer
trainer = SFTTrainer(
    model = model,
    tokenizer = tokenizer,
    train_dataset = ds_text,
    args = SFTConfig(
        max_seq_length = 4096,
        dataset_text_field = &quot;text&quot;,   # 你上一步 map 生成了 text 列
        packing = False,               # 一般先关掉 packing，避免拼接样本
        per_device_train_batch_size = 1,
        gradient_accumulation_steps = 4,
        warmup_steps = 5,
        num_train_epochs = 1, # Set this for 1 full training run.
        learning_rate = 2e-4,
        optim = &quot;adamw_8bit&quot;,
        weight_decay = 0.01,
        lr_scheduler_type = &quot;linear&quot;,
        seed = 3407,
        output_dir = &quot;outputs&quot;,
        report_to=&quot;tensorboard&quot;,     # 📌 启用 TensorBoard
        logging_dir=&quot;./logs&quot;,        # 日志目录
        logging_strategy=&quot;steps&quot;,
        logging_steps=1,            # 记录频率
    ),
)
```

```
Unsloth: Tokenizing [&quot;text&quot;] (num_proc=2): 100%|██████████| 200/200 [00:01&lt;00:00, 128.94 examples/s]
```

+   **Step 5.开始微调**

```
trainer_stats = trainer.train()
```

```
The tokenizer has new PAD/BOS/EOS tokens that differ from the model config and generation config. The model config and generation config were aligned accordingly, being updated with the tokenizer&#x27;s values. Updated tokens: {&#x27;bos_token_id&#x27;: 199998}.
==((====))==  Unsloth - 2x faster free finetuning | Num GPUs used = 1
   \\   /|    Num examples = 200 | Num Epochs = 1 | Total steps = 50
O^O/ \_/ \    Batch size per device = 1 | Gradient accumulation steps = 4
\        /    Data Parallel GPUs = 1 | Total batch size (1 x 4 x 1) = 4
 &quot;-____-&quot;     Trainable parameters = 3,981,312 of 20,918,738,496 (0.02% trained)
/root/miniconda3/envs/unsloth/lib/python3.12/site-packages/torch/_inductor/runtime/triton_helpers.py:150: UserWarning: Logical operators &#x27;and&#x27; and &#x27;or&#x27; are deprecated for non-scalar tensors; please use &#x27;&amp;&#x27; or &#x27;|&#x27; instead
  mask |= a_isnan and not b_isnan
UserWarning: Enable tracemalloc to get the object allocation traceback
/root/miniconda3/envs/unsloth/lib/python3.12/site-packages/torch/_inductor/runtime/triton_helpers.py:150: UserWarning: Logical operators &#x27;and&#x27; and &#x27;or&#x27; are deprecated for non-scalar tensors; please use &#x27;&amp;&#x27; or &#x27;|&#x27; instead
  mask |= a_isnan and not b_isnan
UserWarning: Enable tracemalloc to get the object allocation traceback
/root/miniconda3/envs/unsloth/lib/python3.12/site-packages/torch/_inductor/runtime/triton_helpers.py:152: UserWarning: Logical operators &#x27;and&#x27; and &#x27;or&#x27; are deprecated for non-scalar tensors; please use &#x27;&amp;&#x27; or &#x27;|&#x27; instead
  equal |= a_isnan and b_isnan
UserWarning: Enable tracemalloc to get the object allocation traceback
/root/miniconda3/envs/unsloth/lib/python3.12/site-packages/torch/_inductor/runtime/triton_helpers.py:152: UserWarning: Logical operators &#x27;and&#x27; and &#x27;or&#x27; are deprecated for non-scalar tensors; please use &#x27;&amp;&#x27; or &#x27;|&#x27; instead
  equal |= a_isnan and b_isnan
UserWarning: Enable tracemalloc to get the object allocation traceback


Unsloth: Will smartly offload gradients to save VRAM!




&lt;div&gt;

  &lt;progress value=&#x27;50&#x27; max=&#x27;50&#x27; style=&#x27;width:300px; height:20px; vertical-align: middle;&#x27;&gt;&lt;/progress&gt;
  [50/50 03:47, Epoch 1/1]
&lt;/div&gt;
&lt;table border=&quot;1&quot; class=&quot;dataframe&quot;&gt;
```

Step Training Loss 1 3.278600 2 3.128200 3 3.592000 4 3.236400 5 3.003900 6 2.331300 7 2.417000 8 2.222700 9 1.841600 10 1.481800 11 1.726700 12 1.356100 13 1.603300 14 1.576300 15 1.206200 16 1.626200 17 1.286500 18 1.326200 19 1.268700 20 0.985900 21 1.155000 22 1.427600 23 1.031600 24 1.201000 25 1.224500 26 1.147100 27 0.949200 28 1.432600 29 0.806900 30 1.206900 31 0.932200 32 1.125200 33 1.090500 34 1.139500 35 1.159000 36 0.702700 37 1.065400 38 0.622600 39 0.870500 40 0.835400 41 0.839700 42 1.157100 43 0.938000 44 1.112100 45 1.110400 46 0.937100 47 0.942600 48 0.913700 49 1.061700 50 0.769000

+   **Step 7.微调后模型导出**

最后，我们还可以将模型合并导出，以便下次使用：

```
model
```

```
PeftModel(
  (base_model): LoraModel(
    (model): GptOssForCausalLM(
      (model): GptOssModel(
        (embed_tokens): Embedding(201088, 2880, padding_idx=199999)
        (layers): ModuleList(
          (0-23): 24 x GptOssDecoderLayer(
            (self_attn): GptOssAttention(
              (q_proj): lora.Linear(
                (base_layer): Linear(in_features=2880, out_features=4096, bias=True)
                (lora_dropout): ModuleDict(
                  (default): Identity()
                )
                (lora_A): ModuleDict(
                  (default): Linear(in_features=2880, out_features=8, bias=False)
                )
                (lora_B): ModuleDict(
                  (default): Linear(in_features=8, out_features=4096, bias=False)
                )
                (lora_embedding_A): ParameterDict()
                (lora_embedding_B): ParameterDict()
                (lora_magnitude_vector): ModuleDict()
              )
              (k_proj): lora.Linear(
                (base_layer): Linear(in_features=2880, out_features=512, bias=True)
                (lora_dropout): ModuleDict(
                  (default): Identity()
                )
                (lora_A): ModuleDict(
                  (default): Linear(in_features=2880, out_features=8, bias=False)
                )
                (lora_B): ModuleDict(
                  (default): Linear(in_features=8, out_features=512, bias=False)
                )
                (lora_embedding_A): ParameterDict()
                (lora_embedding_B): ParameterDict()
                (lora_magnitude_vector): ModuleDict()
              )
              (v_proj): lora.Linear(
                (base_layer): Linear(in_features=2880, out_features=512, bias=True)
                (lora_dropout): ModuleDict(
                  (default): Identity()
                )
                (lora_A): ModuleDict(
                  (default): Linear(in_features=2880, out_features=8, bias=False)
                )
                (lora_B): ModuleDict(
                  (default): Linear(in_features=8, out_features=512, bias=False)
                )
                (lora_embedding_A): ParameterDict()
                (lora_embedding_B): ParameterDict()
                (lora_magnitude_vector): ModuleDict()
              )
              (o_proj): lora.Linear(
                (base_layer): Linear(in_features=4096, out_features=2880, bias=True)
                (lora_dropout): ModuleDict(
                  (default): Identity()
                )
                (lora_A): ModuleDict(
                  (default): Linear(in_features=4096, out_features=8, bias=False)
                )
                (lora_B): ModuleDict(
                  (default): Linear(in_features=8, out_features=2880, bias=False)
                )
                (lora_embedding_A): ParameterDict()
                (lora_embedding_B): ParameterDict()
                (lora_magnitude_vector): ModuleDict()
              )
            )
            (mlp): GptOssMLP(
              (router): GptOssTopKRouter()
              (experts): GptOssExperts()
            )
            (input_layernorm): GptOssRMSNorm((2880,), eps=1e-05)
            (post_attention_layernorm): GptOssRMSNorm((2880,), eps=1e-05)
          )
        )
        (norm): GptOssRMSNorm((2880,), eps=1e-05)
        (rotary_emb): GptOssRotaryEmbedding()
      )
      (lm_head): Linear(in_features=2880, out_features=201088, bias=False)
    )
  )
)
```

```
model.save_pretrained_merged(save_directory = &quot;GPT-OSS-finetuned&quot;, 
                             tokenizer = tokenizer, 
                             save_method = &quot;merged_16bit&quot;)
```

```
Detected local model directory: ./gpt-oss-20b-bf16
Found HuggingFace hub cache directory: /root/.cache/huggingface/hub
Copying safetensors from local directory: ./gpt-oss-20b-bf16
Copied safetensors index file from local model


Unsloth: Merging weights into 16bit:   0%|          | 0/9 [00:00&lt;?, ?it/s]

Copied model-00008-of-00009.safetensors from local model directory


Unsloth: Merging weights into 16bit:  11%|█         | 1/9 [00:10&lt;01:20, 10.00s/it]

Copied model-00004-of-00009.safetensors from local model directory


Unsloth: Merging weights into 16bit:  22%|██▏       | 2/9 [00:19&lt;01:06,  9.51s/it]

Copied model-00005-of-00009.safetensors from local model directory


Unsloth: Merging weights into 16bit:  33%|███▎      | 3/9 [00:28&lt;00:56,  9.38s/it]

Copied model-00003-of-00009.safetensors from local model directory


Unsloth: Merging weights into 16bit:  44%|████▍     | 4/9 [00:37&lt;00:46,  9.36s/it]

Copied model-00001-of-00009.safetensors from local model directory


Unsloth: Merging weights into 16bit:  56%|█████▌    | 5/9 [00:46&lt;00:37,  9.27s/it]

Copied model-00007-of-00009.safetensors from local model directory


Unsloth: Merging weights into 16bit:  67%|██████▋   | 6/9 [00:56&lt;00:28,  9.38s/it]

Copied model-00002-of-00009.safetensors from local model directory


Unsloth: Merging weights into 16bit:  78%|███████▊  | 7/9 [01:06&lt;00:18,  9.46s/it]

Copied model-00006-of-00009.safetensors from local model directory


Unsloth: Merging weights into 16bit:  89%|████████▉ | 8/9 [01:15&lt;00:09,  9.41s/it]

Copied model-00009-of-00009.safetensors from local model directory


Unsloth: Merging weights into 16bit: 100%|██████████| 9/9 [01:19&lt;00:00,  8.88s/it]
```

![](img/86a457c5fd0c8b2747c17ab79b6dff86.png)

+   **Step 8.微调效果对比**

接下来即可使用vllm对其进行调用，并借助EvalScope进行测试。需要借助vLLM调用导出后的模型，然后借助evalscope进行测试。

```
from evalscope import TaskConfig, run_task
task_cfg = TaskConfig(
    model=&#x27;./GPT-OSS-finetuned&#x27;,
    api_url=&#x27;http://127.0.0.1:8000/v1/chat/completions&#x27;,
    eval_type=&#x27;service&#x27;,
    datasets=[
        &#x27;data_collection&#x27;,
    ],
    dataset_args={
        &#x27;data_collection&#x27;: {
            &#x27;dataset_id&#x27;: &#x27;modelscope/EvalScope-Qwen3-Test&#x27;,
            &#x27;filters&#x27;: {&#x27;remove_until&#x27;: &#x27;&lt;/think&gt;&#x27;}  # 过滤掉思考的内容
        }
    },
    eval_batch_size=128,
    generation_config={
        &#x27;max_tokens&#x27;: 30000,  # 最大生成token数，建议设置为较大值避免输出截断
        &#x27;temperature&#x27;: 0.6,  # 采样温度 (qwen 报告推荐值)
        &#x27;top_p&#x27;: 0.95,  # top-p采样 (qwen 报告推荐值)
        &#x27;top_k&#x27;: 20,  # top-k采样 (qwen 报告推荐值)
        &#x27;n&#x27;: 1,  # 每个请求产生的回复数量
    },
    timeout=60000,  # 超时时间
    stream=True,  # 是否使用流式输出
    limit=2000,  # 设置为2000条数据进行测试
)

run_task(task_cfg=task_cfg)
```

测试后即可在前端中观察测评报告：

```
evalscope app
```

原始模型评分：

![](img/08d8ffd3a5899d9a2b228bd2fec30dbb.png)

微调后模型评分：

![](img/a12fb3f2f656069c89de081688ba88c4.png)

两个模型对比：

![](img/6ada7d3222d37f02222bded8e55440dc.png)