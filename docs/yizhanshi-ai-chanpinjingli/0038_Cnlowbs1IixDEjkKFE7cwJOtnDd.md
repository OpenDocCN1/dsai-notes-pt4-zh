# 适配 Langchain 的 Function Call

> 来源：[https://v11enp9ok1h.feishu.cn/docx/EKDOdLjuioxHjJxdsmPc2GXYn2f](https://v11enp9ok1h.feishu.cn/docx/EKDOdLjuioxHjJxdsmPc2GXYn2f)

<https://v11enp9ok1h.feishu.cn/sync/VztedzIoUstVPDb6RNMctK8Unie>

本文档将带领开发者如何将要ChatGLM3-6B适配Langchain框架。

请配合以下代码库中微调部分代码进行上手练习

https://github.com/THUDM/ChatGLM3/tree/main/langchain_demo

## 为什么需要这些代码

Langchain 作为最主流的大模型中间件开源框架，备受广大开发者的认可。Langchain中具有一套完整的 Agent 思维，包括灵活，简单的Function Call开发框架。ChatGLM3-6B 模型在同量级模型中有出色的Function Call能力。但遗憾的是，其训练过程并没有与Langchain进行原生对齐。这导致如果直接使用Langchian框架，将会遇到以下问题：

+   无法载入ChatGLM3-6B模型，Langchain中的 LLM模型 目前仅支持在线的几个主流模型，例如ChatGPT，Bard，Claude等

+   无法正常使用 Agent 的 Function Call 功能，ChatGLM3-6B的截断点与 Langchain 支持的并不同。

+   提示词不同，使用 Langchain 封装的 Agent 提示词完全无法胜任ChatGLM3-6B 的 Function Call 任务。

## 将GLM模型接入Langchain

首先，要解决第一个痛点：ChatGLM3-6B 模型能够被 Langchain 读入并执行。

那么，我们就需要基于Langchain的LLM类完成ChatGLM3-6B的模型实现。

我们为开发者准备了一份代码来初步实现基础的ChatGLM3-6B。

首先，初始化并定义我们的模型名称，max_tokens等信息。

```
class ChatGLM3(LLM):
    max_token: int = 8192
    do_sample: bool = False
    temperature: float = 0.8
    top_p = 0.8
    tokenizer: object = None
    model: object = None
    history: List = []
    tool_names: List = []
    has_search: bool = False

    def __init__(self):
        super().__init__()

    @property
    def _llm_type(self) -&gt; str:
        return &quot;ChatGLM3&quot;
```

接着，我们要写入读入模型的方法，这与 Langchain 支持的在线模型不同，这里使用 Huggingface 的方式进行读入。

```
def load_model(self, model_name_or_path=None):
    model_config = AutoConfig.from_pretrained(
        model_name_or_path,
        trust_remote_code=True
    )
    self.tokenizer = AutoTokenizer.from_pretrained(
        model_name_or_path,
        trust_remote_code=True
    )
    self.model = AutoModel.from_pretrained(
        model_name_or_path, config=model_config, trust_remote_code=True
    ).half().cuda()
```

然后，就是非常重要的一环。由于我们的工具调用和观察抓取与 Langchain 并不相同，我们需要对 Langchain 的提示词进行修改，并配上我们的提示词。

+   我们先从AgentType.STRUCTURED_CHAT_ZERO_SHOT_REACT_DESCRIPTION模板中截取到You have access to the following tools:\n\n")的关键词，并在合理插入已经注册的工具类型。

```
tool_prompts = prompt.split(
        &quot;You have access to the following tools:\n\n&quot;)[1].split(&quot;\n\nUse a json blob&quot;)[0].split(&quot;\n&quot;)

    tool_names = [tool.split(&quot;:&quot;)[0] for tool in tool_prompts]
    self.tool_names = tool_name
```

+   [在加载工具的过程中](https://github.com/THUDM/ChatGLM3/blob/main/README.md)，由于ChatGLM3模型需要按照在README.md中的json结构提示词，因此，在这里，我们在Tool中实现工具之后，还需要为每个工具设置一个json文件，用来传递结构化的参数和工具信息。在我们的代码中，我们设计了一个与工具同名的yaml文件来描述工具信息，这个yaml的格式如下:

```
name: arxiv
description: A wrapper around Arxiv.org for searching and retrieving scientific articles in various fields.
parameters:
  type: object
  properties:
    query:
      type: string
      description: The search query title
required:
  - query
```

+   这是一个使用Langchain官方的工具arxiv制作的yaml文件，第一个description代表工具的作用。第二个description位于properties中，代表需要传入的参数的意义。required参数代表需要由模型传入到工具的参数，请注意，parameters可以比required的参数更多，这类似于Langchain中Function_call功能的args_schema。在我们代码中，yaml文件通过以下代码被读入。

```
def tool_config_from_file(tool_name, directory=&quot;Tool/&quot;):
    &quot;&quot;&quot;search tool yaml and return json format&quot;&quot;&quot;
    for filename in os.listdir(directory):
        if filename.endswith(&#x27;.yaml&#x27;) and tool_name in filename:
            file_path = os.path.join(directory, filename)
            with open(file_path, encoding=&#x27;utf-8&#x27;) as f:
                return yaml.safe_load(f)
    return None
```

+   接着，被读入的yaml文件将会按照以下的方式被注册和构建提示词。

```
tool_names = [tool.split(&quot;:&quot;)[0] for tool in tool_prompts]
self.tool_names = tool_names
tools_json = []
for i, tool in enumerate(tool_names):
    tool_config = tool_config_from_file(tool)
    if tool_config:
        tools_json.append(tool_config)
    else:
        ValueError(
            f&quot;Tool {tool} config not found! It&#x27;s description is {tool_prompts[i]}&quot;
        )

ans.append({
    &quot;role&quot;: &quot;system&quot;,
    &quot;content&quot;: &quot;Answer the following questions as best as you can. You have access to the following tools:&quot;,
    &quot;tools&quot;: tools_json
})
query = f&quot;&quot;&quot;{prompt.split(&quot;Human: &quot;)[-1].strip()}&quot;&quot;&quot;
```

完成了工具构建之后，我们要开始构建模型的执行思维链。

由于ChatGLM3-6B拥有Observation角色，这与Langchain中原本设定的Observation截断并不相同，因此，在这里，我们需要做提取,在这段代码中，我们需要对原本Langchain中的Observation进行截断，并补充上我们的工具观察的结果。

```
def _extract_observation(self, prompt: str):
    return_json = prompt.split(&quot;Observation: &quot;)[-1].split(&quot;\nThought:&quot;)[0]
    self.history.append({
        &quot;role&quot;: &quot;observation&quot;,
        &quot;content&quot;: return_json
    })
    return
```

在这，我们还需要对执行工具进行截断和填充，使得其能满足ChatGLM3-6B的思维模式

```
def _extract_tool(self):
        if len(self.history[-1][&quot;metadata&quot;]) &gt; 0:
            metadata = self.history[-1][&quot;metadata&quot;]
            content = self.history[-1][&quot;content&quot;]
            if &quot;tool_call&quot; in content:
                for tool in self.tool_names:
                    if tool in metadata:
                        input_para = content.split(&quot;=&#x27;&quot;)[-1].split(&quot;&#x27;&quot;)[0]
                        action_json = {
                            &quot;action&quot;: tool,
                            &quot;action_input&quot;: input_para
                        }
                        self.has_search = True
                        return f&quot;&quot;&quot;
Action: 
```
{json.dumps(action_json, ensure_ascii=False)}
```&quot;&quot;&quot;
        final_answer_json = {
            &quot;action&quot;: &quot;Final Answer&quot;,
            &quot;action_input&quot;: self.history[-1][&quot;content&quot;]
        }
        self.has_search = False
        return f&quot;&quot;&quot;
Action: 
```
{json.dumps(final_answer_json, ensure_ascii=False)}
```&quot;&quot;&quot;
```

由于ChatGLM3-6B的思维方式并没有Action: 字段，而这是langchain的截断点，因此，我们需要对其进行补充，使得Langchain能知道，此时模型进入调用工具阶段。

最后，我们要基于Langchain的构造，重写_call函数,包括历史记录，提示词等拼接内容。

```
def _call(self, prompt: str, history: List = [], stop: Optional[List[str]] = [&quot;&lt;|user|&gt;&quot;]):
    if not self.has_search:
        self.history, query = self._tool_history(prompt)
    else:
        self._extract_observation(prompt)
        query = &quot;&quot;
    _, self.history = self.model.chat(
        self.tokenizer,
        query,
        history=self.history,
        do_sample=self.do_sample,
        max_length=self.max_token,
        temperature=self.temperature,
    )
    response = self._extract_tool()
    history.append((prompt, response))
    return response
```

## 使用接入了Langchain的ChatGLM3-6B模型

在完成了上述工作之后，我们就已经拥有了支持Langchain的ChatGLM3-6B模型，我们在main.py中对其进行了简单调用

```
def run_tool(tools, llm, prompt_chain: List[str]):
    loaded_tolls = []
    for tool in tools:
        if isinstance(tool, str):
            loaded_tolls.append(load_tools([tool], llm=llm)[0])
        else:
            loaded_tolls.append(tool)
    agent = initialize_agent(
        loaded_tolls, llm,
        agent=AgentType.STRUCTURED_CHAT_ZERO_SHOT_REACT_DESCRIPTION,
        verbose=True,
        handle_parsing_errors=True
    )
    for prompt in prompt_chain:
        agent.run(prompt)
```

该代码使用了LangChain自带的Agent框架，使用ChatGLM3-6B的工具调用。

以下为实际执行效果。

```
python main.py
Loading checkpoint shards: 100%|█████████████████████████████████████████████████████████████████████████████████████| 7/7 [00:04&lt;00:00,  1.43it/s]


&gt; Entering new AgentExecutor chain...
======
System: Respond to the human as helpfully and accurately as possible. You have access to the following tools:

arxiv: A wrapper around Arxiv.org Useful for when you need to answer questions about Physics, Mathematics, Computer Science, Quantitative Biology, Quantitative Finance, Statistics, Electrical Engineering, and Economics from scientific articles on arxiv.org. Input should be a search query., args: {{&#x27;query&#x27;: {{&#x27;title&#x27;: &#x27;Query&#x27;, &#x27;type&#x27;: &#x27;string&#x27;}}}}

Use a json blob to specify a tool by providing an action key (tool name) and an action_input key (tool input).

Valid &quot;action&quot; values: &quot;Final Answer&quot; or arxiv

Provide only ONE action per $JSON_BLOB, as shown:

```
{
  &quot;action&quot;: $TOOL_NAME,
  &quot;action_input&quot;: $INPUT
}
```

Follow this format:

Question: input question to answer
Thought: consider previous and subsequent steps
Action:
```
$JSON_BLOB
```
Observation: action result
... (repeat Thought/Action/Observation N times)
Thought: I know what to respond
Action:
```
{
  &quot;action&quot;: &quot;Final Answer&quot;,
  &quot;action_input&quot;: &quot;Final response to human&quot;
}
```

Begin! Reminder to ALWAYS respond with a valid json blob of a single action. Use tools if necessary. Respond directly if appropriate. Format is Action:```$JSON_BLOB```then Observation:.
Thought:
Human: 帮我查询GLM-130B相关工作


======

Action: 
```
{&quot;action&quot;: &quot;arxiv&quot;, &quot;action_input&quot;: &quot;GLM-130B&quot;}
```
Observation: Published: 2023-10-25
Title: GLM-130B: An Open Bilingual Pre-trained Model
Authors: Aohan Zeng, Xiao Liu, Zhengxiao Du, Zihan Wang, Hanyu Lai, Ming Ding, Zhuoyi Yang, Yifan Xu, Wendi Zheng, Xiao Xia, Weng Lam Tam, Zixuan Ma, Yufei Xue, Jidong Zhai, Wenguang Chen, Peng Zhang, Yuxiao Dong, Jie Tang
Summary: We introduce GLM-130B, a bilingual (English and Chinese) pre-trained language
model with 130 billion parameters. It is an attempt to open-source a 100B-scale
model at least as good as GPT-3 (davinci) and unveil how models of such a scale
can be successfully pre-trained. Over the course of this effort, we face
numerous unexpected technical and engineering challenges, particularly on loss
spikes and divergence. In this paper, we introduce the training process of
GLM-130B including its design choices, training strategies for both efficiency
and stability, and engineering efforts. The resultant GLM-130B model offers
significant outperformance over GPT-3 175B (davinci) on a wide range of popular
English benchmarks while the performance advantage is not observed in OPT-175B
and BLOOM-176B. It also consistently and significantly outperforms ERNIE TITAN
3.0 260B -- the largest Chinese language model -- across related benchmarks.
Finally, we leverage a unique scaling property of GLM-130B to reach INT4
quantization without post training, with almost no performance loss, making it
the first among 100B-scale models and more importantly, allowing its effective
inference on 4$\times$RTX 3090 (24G) or 8$\times$RTX 2080 Ti (11G) GPUs, the
most affordable GPUs required for using 100B-scale models. The GLM-130B model
weights are publicly accessible and its code, training logs, related toolkit,
and lessons learned are open-sourced at
\url{https://github.com/THUDM/GLM-130B/}.
Thought:======
System: Respond to the human as helpfully and accurately as possible. You have access to the following tools:

arxiv: A wrapper around Arxiv.org Useful for when you need to answer questions about Physics, Mathematics, Computer Science, Quantitative Biology, Quantitative Finance, Statistics, Electrical Engineering, and Economics from scientific articles on arxiv.org. Input should be a search query., args: {{&#x27;query&#x27;: {{&#x27;title&#x27;: &#x27;Query&#x27;, &#x27;type&#x27;: &#x27;string&#x27;}}}}

Use a json blob to specify a tool by providing an action key (tool name) and an action_input key (tool input).

Valid &quot;action&quot; values: &quot;Final Answer&quot; or arxiv

Provide only ONE action per $JSON_BLOB, as shown:

```
{
  &quot;action&quot;: $TOOL_NAME,
  &quot;action_input&quot;: $INPUT
}
```

Follow this format:

Question: input question to answer
Thought: consider previous and subsequent steps
Action:
```
$JSON_BLOB
```
Observation: action result
... (repeat Thought/Action/Observation N times)
Thought: I know what to respond
Action:
```
{
  &quot;action&quot;: &quot;Final Answer&quot;,
  &quot;action_input&quot;: &quot;Final response to human&quot;
}
```

Begin! Reminder to ALWAYS respond with a valid json blob of a single action. Use tools if necessary. Respond directly if appropriate. Format is Action:```$JSON_BLOB```then Observation:.
Thought:
Human: 帮我查询GLM-130B相关工作

This was your previous work (but I haven&#x27;t seen any of it! I only see what you return as final answer):

Action: 
```
{&quot;action&quot;: &quot;arxiv&quot;, &quot;action_input&quot;: &quot;GLM-130B&quot;}
```
Observation: Published: 2023-10-25
Title: GLM-130B: An Open Bilingual Pre-trained Model
Authors: Aohan Zeng, Xiao Liu, Zhengxiao Du, Zihan Wang, Hanyu Lai, Ming Ding, Zhuoyi Yang, Yifan Xu, Wendi Zheng, Xiao Xia, Weng Lam Tam, Zixuan Ma, Yufei Xue, Jidong Zhai, Wenguang Chen, Peng Zhang, Yuxiao Dong, Jie Tang
Summary: We introduce GLM-130B, a bilingual (English and Chinese) pre-trained language
model with 130 billion parameters. It is an attempt to open-source a 100B-scale
model at least as good as GPT-3 (davinci) and unveil how models of such a scale
can be successfully pre-trained. Over the course of this effort, we face
numerous unexpected technical and engineering challenges, particularly on loss
spikes and divergence. In this paper, we introduce the training process of
GLM-130B including its design choices, training strategies for both efficiency
and stability, and engineering efforts. The resultant GLM-130B model offers
significant outperformance over GPT-3 175B (davinci) on a wide range of popular
English benchmarks while the performance advantage is not observed in OPT-175B
and BLOOM-176B. It also consistently and significantly outperforms ERNIE TITAN
3.0 260B -- the largest Chinese language model -- across related benchmarks.
Finally, we leverage a unique scaling property of GLM-130B to reach INT4
quantization without post training, with almost no performance loss, making it
the first among 100B-scale models and more importantly, allowing its effective
inference on 4$\times$RTX 3090 (24G) or 8$\times$RTX 2080 Ti (11G) GPUs, the
most affordable GPUs required for using 100B-scale models. The GLM-130B model
weights are publicly accessible and its code, training logs, related toolkit,
and lessons learned are open-sourced at
\url{https://github.com/THUDM/GLM-130B/}.
Thought:
======

Action: 
```
{&quot;action&quot;: &quot;Final Answer&quot;, &quot;action_input&quot;: &quot;根据您的查询，我找到了一篇关于GLM-130B的论文，这是一篇关于英汉双语预训练语言模型的论文。该模型是由Aohannyu Lai, Ming Ding, Zhuoyi Yang, Yifan Xu, Wendi Zheng, Xiao Xia, Weng Lam Tam, Zixuan Ma, Yufei Xue, Jidong Zhai, Wenguang Chen, Peng Zhang, Yuxiao Dong, Jie Tang等人于2023年10月25日发表的。该模型采用了1300亿参数，是迄今为止最大规模的预训练语言模型之一。在英语基准测试中，GLM-130B模型在许多方面都表现出了显著的优势，而在汉语基准测试中，其表现则相对较弱。&quot;}
```

&gt; Finished chain.
```

## 注意事项

到此为止，你已经简单实现了使用LangChain调用ChatGLM3-6B模型来实现工具调用和其他基本用法。但是，在更多探索之前，请一定要看这部分的内容。这将能为你之后的开发减少不必要的麻烦。

### 使用LLMChain的工具

在官方的实现方案中，暂时不能解决在工具中仍然需要调用正常的LLMChain的操作，这意味着你在工具的设计中不能再次调用大模型进行更多操作，例如参数解析等，典型的错误例子为

> LLMMathChain

> 如果使用官方Demo调用这个工具，则必然遇到以下错误:

> line 120, in _process_llm_result

> raise ValueError(f"unknown format from LLM: {llm_output}")

> ValueError: unknown format from LLM: Action:

> {"action": "Calculator", "action_input": "23*23"}

这是因为在模型构建的过程中，模型会解析到tools，而在tools中的常规调用模型并没有修改模型的系统提示词，模型还会尝试调用工具，这在常规的Chain中是错误的。

### 无效的参数和固定的参数

+   ChatGLM3-6B必须使用结构化的Agent，在Langchain中，我们只适配了AgentType.STRUCTURED_CHAT_ZERO_SHOT_REACT_DESCRIPTION

+   如果使用LLMSingleActionAgent来构建，stop参数无效。

+   使用Tool.from_function时，args_schema无效。

+   每次创建一个新的Tools，都必须有同名的yaml，或者自己实现传入格式化的工具说明。

### 兼容OpenAI API

如果你使用的是我们的OpenAI API格式的demo，则无法适配Langchain的工具。