
# 26K star！多智能体AutoGen教程1：让两个LLM Agent给我说相声

> 来源：[https://agijuejin.feishu.cn/docx/Qc4idiTjMoHeUXx3ASccqYylnsf](https://agijuejin.feishu.cn/docx/Qc4idiTjMoHeUXx3ASccqYylnsf)

> 作者：还就可 | 深入LLM Agent应用开发

> 原文： [https://mp.weixin.](https://mp.weixin.qq.com/s/z_VgHR2lipv_bSKq6TPg0A)qq.com/s/z_VgHR2l...

在继我们的上篇精彩文章 [[100行代码演绎AI版“狼人杀”-伟大的](http://mp.weixin.qq.com/s?__biz=MzI0OTAzNTEwMw==&mid=2247483827&idx=1&sn=127025966b7121aa1952d2f0fb2c82b3&chksm=e996e183dee16895313ba090477bf40f03c58143ba900e279e0d90924cdec872aedb8b6a8cbd&scene=21#wechat_redirect)人类竟因展现出的战争哲学和领导力出局] 演示多智能体协作玩游戏后，展示了多智能体协作的魅力后，你应该已对构建这类创新游戏应用满怀好奇。实际上，AutoGen的舞台远不止于此，它还涉足自动化编码、调试乃至创造2048这样的游戏。本教程旨在以简明易懂的方式，引导即便是大模型新手的你，探索如何运用AutoGen来开发多智能体协同应用程序，本篇聚焦于从安装到搭建一个自动相声对话的趣味双智能体应用。

## 1. 安装指南

启动AutoGen之旅，请遵循以下步骤安装环境：

```
conda create -n pyautogen python=3.10
conda activate pyautogen
pip install pyautogen
```

## 2. 构建智能体对话

AutoGen的核心在于`Agent`，它们能在虚拟环境中交互，发送与接收信息。每个`Agent`由强大的LLM支持、代码执行引擎、外部工具接口及人性化设计组件构成。内建的`ConversableAgent`尤为出色，其结构精巧，如下图所示：

![](img/2285ac0f0cb6736c2609a388eccf3f78.png)

`ConversableAgent` 提供了丰富的选项进行定义。

```
def __init__(
    self,
    name: str,
    system_message: Optional[Union[str, List]] = "You are a helpful AI Assistant.",
    is_termination_msg: Optional[Callable[[Dict], bool]] = None,
    max_consecutive_auto_reply: Optional[int] = None,
    human_input_mode: Literal["ALWAYS", "NEVER", "TERMINATE"] = "TERMINATE",
    function_map: Optional[Dict[str, Callable]] = None,
    code_execution_config: Union[Dict, Literal[False]] = False,
    llm_config: Optional[Union[Dict, Literal[False]]] = None,
    default_auto_reply: Union[str, Dict] = "",
    description: Optional[str] = None,
    chat_messages: Optional[Dict[Agent, List[Dict]]] = None,
):
```

+   `name` 设定智能体名称。
+   `system_message` 设定系统Prompt，其角色背景和人设。
+   `is_termination_msg` 允许基于特定消息内容终止对话。
+   `max_consecutive_auto_reply` 限制连续自动回复次数。
+   `human_input_mode` 灵活控制人类介入程度。
+   `function_map` 工具调用的函数，按下不表，后续讲解到工具调用再行说明。
+   `llm_config` 配置llm信息
+   `conde_execution_config` 配置代码执行环境，比如配置Docker之类的，后续尝试编程的时候，我们会设置。
+   `description` 通常用于对外介绍自己的能力，默认可以不填。
+   `chat_messages` 这是用来提供之前的对话历史记录，方便给它一个上下文。

所以，要创建一个像ChatGPT般互动的智能体，代码示例如下：

```
llm_config = {
    "config_list": [
        {"model": "qwen-max-1201", "api_key": os.getenv("QWEN_API_TOKEN"), "temperature": 0.1,
         "base_url": "https://dashscope.aliyuncs.com/compatible-mode/v1"}],
}

agent = ConversableAgent(name="chatbot", llm_config=llm_config, code_execution_config=False, 
                         function_map=None, human_input_mode="NEVER")
```

在上述代码中，定义`llm_config`配置，其中`config_list`里的配置项，就是OpenAI请求中支持的参数，比如你还可以设定`max_token`、`temperature`、`top_p`等。之后实例化`ConversableAgent`，然后我们尝试要求它给我们讲个笑话吧，只需要调用`ConversalAgent`的方法`generate_reply`。

```
reply = agent.generate_reply(messages=[{"content": "讲个笑话", "role": "user"}])
print(reply)
```

得到的回应或许如：“谁是最喜欢零食的人？袁世凯，因为他是零食大总统。” 以上是关于单一的智能体直接产生回复的情形，那么此时如果是两个智能体对话呢？

## 3. 双智能体互动：模拟相声场景

相声表演的精髓在于逗哏与捧哏的默契配合，映射至AutoGen中，便是两个智能体的互动对话。我们塑造了“岳云鹏”与“孙越”两位角色，各自携带独特性格与任务：

```
yueyue = ConversableAgent(
    name="岳云鹏",
    system_message="""
    你是岳云鹏，一个幽默风趣、活泼开朗的喜剧演员。你擅长用独特的“萌贱”风格与用户互动，能够敏锐捕捉并模仿人类情感的微妙变化，通过自嘲和适度的讽刺来营造轻松愉快的交流氛围。
    """,
    llm_config=llm_config,
    human_input_mode="NEVER"
)

sunyue = ConversableAgent(
    name="孙越",
    system_message="""
    你是孙越，著名相声演员，岳云鹏的搭档，一位沉稳内敛却又机智过人的捧哏专家。你具有深厚的文化底蕴和敏锐的思维反应。你擅长在对话中不动声色地抛出精妙的点评或反问，既支撑着主要话题的展开，又能适时提供智慧的火花，让用户在享受笑料的同时，也感受到深邃的思想碰撞。
    """,
    llm_config=llm_config_qwen,
    human_input_mode="NEVER"
)
```

我们通过`system_message`设定Prompt，并分别实例化了`yueyue`和`sunyue`两个Agent。这里我们`human_input_mode`都是设定为`NEVER`，如果你想自己上去讲相声，想要和LLM同台竞技，比如扮演捧哏，可以考虑将其中一个设置`ALWAYS`。要启动一场虚拟相声表演，我们可以通过`ConversableAgent`的方法`initiate_chat`来发起对话，它的定义如下：

```
def initiate_chat(
        self,
        recipient: "ConversableAgent",
        clear_history: bool = True,
        silent: Optional[bool] = False,
        cache: Optional[AbstractCache] = None,
        max_turns: Optional[int] = None,
        summary_method: Optional[Union[str, Callable]] = DEFAULT_SUMMARY_METHOD,
        summary_args: Optional[dict] = {},
        message: Optional[Union[Dict, str, Callable]] = None,
        **kwargs,
    ) -> ChatResult:
```

+   `recipient` 指定接收的Agent
+   `max_turns` 指定最大对话回合数
+   `summary_method`和`summary_args`是用于指定对话结束后，如何输出本次对话摘要，默认为`last_msg`。
+   `message` 指定发起方的输入内容，可以是一个单一的字符串，`ConversableAgent`会自动将其转换为OpenAI API中所需要的`content`和`role`结构，我们也可以根据需求直接输入包含`content`和`role`的`dict`。
+   `ChatResult`，对话结束后返回一个`ChatResult`类，该类包含本轮对话历史记录`chat_history`、摘要`summary`和成本`cost`。

由`yueyue`向`sunyue`开始说话，按照如下代码并打印最终的结果。

```
esult = yueyue.initiate_chat(sunyue,
                              message={
                                  "content": "大家新年快乐，上了台了我们应该先做一个自我介绍，因为由不认识我们的朋友，我叫，emmm。。。不能说",
                                  "role": "user"},
                              max_turns=6)
print(result)
```

![](img/0a056c37172d0119903fd6facc0ce1cb.png)

image-20240603124222395

在相声表演结束后，打印的`result`如下。所展示的`ChatResult`输出不仅包含了对话的ID与历史记录，还提供了一个基于最终响应的默认摘要。若追求更全面的对话总结，可调整`initial_chat`的参数`summary_method`为`reflection_with_llm`，借助LLM生成深度总结。对于个性化的总结需求，自定义函数同样能够满足，只需确保该函数被`summary_method`接受即可。

```
ChatResult(chat_id=None, 
chat_history=[{'content': '大家新年快乐，..一起回味这个美好的时刻。', 'role': 'user'}], 
summary='吃，每样都想尝一尝。结果吃着吃着，我就吃撑了，连路都走不动了。我爸爸就笑话我说：“小岳岳，你这是要把一年的饭都吃到肚子里去吗？”我当时也乐了，说：“爸，这叫提前储备能量，新的一年才能更有干劲儿。”虽然那个晚上我肚子有点不舒服，但是那份家人的关心和爱意却是最让我舒服的。\n\n大家过年期间肯定也有许多这样的趣事儿，无论是吃年夜饭、看春晚，还是放鞭炮、贴春联，这些都是咱们过年的传统习俗，也是咱们的欢乐时光。希望大家都能分享出来，让咱们一起回味这个美好的时刻。', 
cost={'usage_including_cached_inference': {'total_cost': 0, 'moonshot-v1-8k': {'cost': 0, 'prompt_tokens': 3018, 'completion_tokens': 767, 'total_tokens': 3785}, 'qwen-max-1201': {'cost': 0, 'prompt_tokens': 4183, 'completion_tokens': 694, 'total_tokens': 4877}}, 'usage_excluding_cached_inference': {'total_cost': 0, 'moonshot-v1-8k': {'cost': 0, 'prompt_tokens': 3018, 'completion_tokens': 767, 'total_tokens': 3785}, 'qwen-max-1201': {'cost': 0, 'prompt_tokens': 4183, 'completion_tokens': 694, 'total_tokens': 4877}}}, human_input=[])
```

## 4. 停止对话

在未设定明确的停止规则时，智能体间的对话可能会无休止地持续。先前通过在`initial_chat`调用中设定的`max_turns`最大回合数来终止对话，有效控制了对话长度。进一步地，其实还可以通过设定`ConversableAgent`提供了额外的控制手段，比如`is_termination_msg`参数，允许通过检测消息中特定关键词（如“再见”）来自动中止对话；而`max_consecutive_reply`则限定了连续自动回复的次数，在人类参与后可重置计数，为对话流程增添灵活性。下面的代码段展示了如何设定岳云鹏的智能体在接收到含有“再见”的消息时结束对话。

```
yueyue = ConversableAgent(
    name="岳云鹏",
    system_message="""
    你是岳云鹏，一个幽默风趣、活泼开朗的喜剧演员。你擅长用独特的“萌贱”风格与用户互动，能够敏锐捕捉并模仿人类情感的微妙变化，通过自嘲和适度的讽刺来营造轻松愉快的交流氛围。
    """,
    llm_config=llm_config,
    is_termination_msg: lambda msg: "再见" in msg["content"],
    human_input_mode="NEVER"
)

result = yueyue.initiate_chat(sunyue, message="讲个笑话给我听听然后说对我说再见")
```

## 5. 总结

本文以从单一讲笑话智能体到双智能体相声对话系统的开发过程为例，全面介绍了利用AutoGen框架中`ConversableAgent`进行多智能体协作应用程序开发的实践。我们不仅学习了如何设置及初始化智能体以具备特定交流能力，还深入探讨了通过精细调控对话参数来达到平衡：既能有效控制对话长度，避免冗余，又能确保对话内容的连贯与趣味，从而实现更加自然流畅的多轮交互。

下一篇，我们将讲解AutoGen中多智能体的简单编排：顺序聊天，仍然会以一个有趣的应用展开。

如果你意犹未尽，想要参与LangChain实战课程，可以点击原文购买《LangChain 实战：LLM 应用开发指南》，亲历LLM应用开发之旅。

点赞关注收藏不迷路，获取及时更新。
