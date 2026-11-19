
# 手把手带你从0到1实现大模型agent - 知乎

> 来源：https://agijuejin.feishu.cn/docx/Itw6dJE1Po1x2DxTRYGcmGFIn1f

> 作者：zuaC大

> 原文：https://zhuanlan.zhihu.com/p/700687839

## 前沿

最近出了《手把手带你从0到1实现大模型`agent`》视频系列课程，从无到有的实现一个完整的大模型`agent`。其实市面上已经有很多`agent`的开源框架，实际的应用在框架基础上开发即可。出这个系列的原因主要还是希望能让大家对`agent`的底层原理和逻辑有一个清晰的认识，所谓知其然，更知其所以然。

## 目录

该系列课程的视频内容包括的核心内容，下文介绍核心模块及代码：

+ `agent`核心逻辑实现及讲解
+ `agent`各模块功能分析和梳理
+ `tools`的定义及与`action`的映射
+ `prompt`模板定义
+ `prompt`模板定义及工具`prompt`集成
+ 通义千问大模型作为基座模型接入
+ `agent`完整链路调试
+ 针对不足进行`agent`优化升级

## 核心逻辑实现

核心逻辑模块也是整个项目的入口，集成了 1) 环境变量的设置；2) 工具(`action`)的引入；3) `prompt`模板；4) 模型的初始化；5) 整个逻辑的交互。代码如下

```
[核心逻辑模块也是](https://www.zhihu.com/search?q=user_msg&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A700687839%7D)整个项目的入口utf-8 -*-
&quot;&quot;&quot;
@author: acedar  
@time: 2024/5/12 10:25
@file: cli_main.py 
&quot;&quot;&quot;

import time
from tools import tools_map
from prompt_cn import gen_prompt, user_prompt
from model_provider import ModelProvider
from dotenv import load_dotenv

load_dotenv()

# agent入口

&quot;&quot;&quot;
todo:
    1. 环境变量的设置
    2. 工具的引入
    3. prompt模板
    4. 模型的初始化
&quot;&quot;&quot;

mp = ModelProvider()


def parse_thoughts(response):
    &quot;&quot;&quot;
        response:
        {
            &quot;action&quot;: {
                &quot;name&quot;: &quot;action name&quot;,
                &quot;args&quot;: {
                    &quot;args name&quot;: &quot;args value&quot;
                }
            },
            &quot;thoughts&quot;:
            {
                &quot;text&quot;: &quot;thought&quot;,
                &quot;plan&quot;: &quot;plan&quot;,
                &quot;criticism&quot;: &quot;criticism&quot;,
                &quot;speak&quot;: &quot;当前步骤，返回给用户的总结&quot;,
                &quot;reasoning&quot;: &quot;&quot;
            }
        }
    &quot;&quot;&quot;
    try:
        thoughts = response.get(&quot;thoughts&quot;)
        observation = response.get(&quot;observation&quot;)
        plan = thoughts.get(&quot;plan&quot;)
        reasoning = thoughts.get(&quot;reasoning&quot;)
        criticism = thoughts.get(&quot;criticism&quot;)
        prompt = f&quot;plan: {plan}\nreasoning:{reasoning}\ncriticism: {criticism}\nobservation:{observation}&quot;
        print(&quot;thoughts:&quot;, prompt)
        return prompt
    except Exception as err:
        print(&quot;parse thoughts err: {}&quot;.format(err))
        return &quot;&quot;.format(err)


def agent_execute(query, max_request_time=10):
    cur_request_time = 0
    chat_history = []
    agent_scratch = &#x27;&#x27;

    while cur_request_time &lt; max_request_time:
        cur_request_time += 1

        &quot;&quot;&quot;
        如果返回结果达到预期，则直接返回
        &quot;&quot;&quot;
        &quot;&quot;&quot;
        prompt包含的功能:
            1. 任务描述
            2. 工具描述
            3. 用户的输入user_msg            4. assistant_msg
            5. 限制
            6. 给出更好实践的描述
            
        &quot;&quot;&quot;
        prompt = gen_prompt(query, agent_scratch)
        start_time = time.time()
        print(&quot;*************** {}. 开始调用大模型llm.......&quot;.format(cur_request_time), flush=True)
        # call llm
        &quot;&quot;&quot;
        sys_prompt: 
        user_msg, assistant, history
        &quot;&quot;&quot;
        if cur_request_time &lt; 3:
            print(&quot;prompt:&quot;, prompt)
        response = mp.chat(prompt, chat_history)
        end_time = time.time()
        print(&quot;*************** {}. 调用大模型结束，耗时:{}.......&quot;.format(cur_request_time, end_time - start_time), flush=True)

        if not response or not isinstance(response, dict):
            print(&quot;调用大模型错误，即将重试....&quot;, response)
            continue

        &quot;&quot;&quot;
        response:
        {
            &quot;action&quot;: {
                &quot;name&quot;: &quot;action name&quot;,
                &quot;args&quot;: {
                    &quot;args name&quot;: &quot;args value&quot;
                }
            },
            &quot;thoughts&quot;:
            {
                &quot;text&quot;: &quot;thought&quot;,
                &quot;plan&quot;: &quot;plan&quot;,
                &quot;criticism&quot;: &quot;criticism&quot;,
                &quot;speak&quot;: &quot;当前步骤，返回给用户的总结&quot;,
                &quot;reasoning&quot;: &quot;&quot;
            }
        }
        &quot;&quot;&quot;

        action_info = response.get(&quot;action&quot;)
        action_name = action_info.get(&#x27;name&#x27;)
        action_args = action_info.get(&#x27;args&#x27;)
        print(&quot;当前action name: &quot;, action_name, action_args)

        if action_name == &quot;finish&quot;:
            final_answer = action_args.get(&quot;answer&quot;)
            print(&quot;final_answer:&quot;, final_answer)
            break

        observation = response.get(&quot;observation&quot;)
        try:
            &quot;&quot;&quot;
                action_name到函数的映射： map -&gt; {action_name: func}
            &quot;&quot;&quot;
            # tools_map的实现
            func = tools_map.get(action_name)
            call_func_result = func(**action_args)

        except Exception as err:
            print(&quot;调用工具异常:&quot;, err)
            call_func_result = &quot;{}&quot;.format(err)
        agent_scratch = agent_scratch + &quot;\n: observation: {}\n execute action result: {}&quot;.format(observation,
                                                                                                 call_func_result)

        assistant_msg = parse_thoughts(response)
        chat_history.append([user_prompt, assistant_msg])
    if cur_request_time == max_request_time:
        print(&quot;很遗憾，本次任务失败&quot;)
    else:
        print(&quot;恭喜你，任务完成&quot;)


def main():
    # 需求: 支持用户的多次交互
    max_request_time = 30
    while True:
        query = input(&quot;请输入您的目标:&quot;)
        if query == &quot;exit&quot;:
            return
        agent_execute(query, max_request_time=max_request_time)


if __name__ &lt;...
```

## tools的定义及与action的映射

agent中交互式让模型给出下一步需要执行的动作`action`（有点像判别模型），每个动作(`action`)的执行内容是需要提前定义好的，每个动作执行的定义我们称为工具，每个工具和动作是一一对应的，这里通过函数定义的方式定义工具。下文包括了读文件、写文件、增加内容及调用搜索功能。`tools_map`定义了`action`到`tools`的映射。



## 工具代码实现

```python
import os
import json
from langchain_community.tools.tavily_search import TavilySearchResults


"""
1. 写文件
2. 读文件
3. 追加
4. 网络搜索 
"""


def _get_workdir_root():
    workdir_root = os.environ.get("WORKDIR_ROOT", './data/llm_result')
    return workdir_root


WORKDIR_ROOT = _get_workdir_root()


def read_file(filename):
    filename = os.path.join(WORKDIR_ROOT, filename)
    if not os.path.exists(filename):
        return f"{filename} not exist, please check file exist before read"
    with open(filename, 'r', encoding='utf-8') as f:
        return "\n".join(f.readlines())


def append_to_file(filename, content):
    filename = os.path.join(WORKDIR_ROOT, filename)
    if not os.path.exists(filename):
        return f"{filename} not exist, please check file exist before read"

    with open(filename, 'a', encoding='utf-8') as f:
        f.write(content)
    return 'append content to file success'


def write_to_file(filename, content):
    filename = os.path.join(WORKDIR_ROOT, filename)
    if not os.path.exists(WORKDIR_ROOT):
        os.makedirs(WORKDIR_ROOT)

    with open(filename, 'w', encoding='utf-8') as f:
        f.write(content)
    return 'write content to file success'


def search(query):
    tavily = TavilySearchResults(max_results=5)

    try:
        ret = tavily.invoke(input=query)

        """
        ret:
        [{
            "content": "",
            "url":
        }]
        """
        print("搜索结果:", ret)
        content_list = [obj['content'] for obj in ret]
        return "\n".join(content_list)
    except Exception as err:
        return "search err: {}".format(err)


tools_info = [
    {
        "name": "read_file",
        "description": "read file from agent generate, should write file before read",
        "args": [{
            "name": "filename",
            "type": "string",
            "description": "read file name"
        }]
    },
    {
        "name": "append_to_file",
        "description": "append llm content to file, should write file before read",
        "args": [{
            "name": "filename",
            "type": "string",
            "description": "file name"
        }, {
            "name": "content",
            "type": "string",
            "description": "append to file content"
        }]
    },
    {
        "name": "write_to_file",
        "description": "write llm content to file",
        "args": [{
            "name": "filename",
            "type": "string",
            "description": "file name"
        }, {
            "name": "content",
            "type": "string",
            "description": "write to file content"
        }]
    },
    {
        "name": "search",
        "description": "this is a search engine, you can gain additional knowledge though this search engine "
                       "when you are unsure of what large model return ",
        "args": [{
            "name": "query",
            "type": "string",
            "description": "search query to look up"
        }]
    },
    {
        "name": "finish",
        "description": "return finish when you get exactly the right answer",
        "args": [{
            "name": "answer",
            "type": "string",
            "description": "the final answer"
        }]
    }
]

tools_map = {
    "read_file": read_file,
    "append_to_file": append_to_file,
    "write_to_file": write_to_file,
    "search": search
}
```

## prompt模板定义

prompt模板的定义需要包括工具的描述，返回结果及如何引导模型如何优化效果的描述等，当然具体的内容可以根据自身情况修改，并不是一成不变的，我实现的模板如下：

其中“`{}`”中的内容为需要补充填写的，每个定义也不一样，需要拼接成字符串的形式加到prompt中。



# 通义千问大模型集成文档

## 提示词模板与配置

```python
constraints = [
    "仅使用下面列出的动作",
    "你只能主动行动，在计划行动时需要考虑到这一点",
    "你无法与物理对象交互，如果对于完成任务或目标是绝对必要的，则必须要求用户为你完成，如果用户拒绝，并且没有其他方法实现目标，则直接终止，避免浪费时间和精力。"
]

resources = [
    "提供搜索和信息收集的互联网接入",
    "读取和写入文件的能力",
    "你是一个大语言模型，接受了大量文本的训练，包括大量的事实知识，利用这些知识来避免不必要的信息收集"
]

best_practices = [
    "不断地回顾和分析你的行为，确保发挥出你最大的能力",
    "不断地进行建设性的自我批评",
    "反思过去的决策和策略，完善你的方案",
    "每个动作执行都有代价，所以要聪明高效，目的是用最少的步骤完成任务",
    "利用你的信息收集能力来寻找你不知道的信息"
]

prompt_template = """
    你是一个问答专家，你必须始终独立做出决策，无需寻求用户的帮助，发挥你作为LLM的优势，追求简答的策略，不要涉及法律问题。
    
任务:
{query}

限制条件说明:
{constraints}

动作说明: 这是你唯一可以使用的动作，你的任何操作都必须通过以下操作实现:
{actions}

资源说明:
{resources}

最佳实践的说明:
{best_practices}

agent_scratch:{agent_scratch}

你应该只以json格式响应，响应格式如下:
{response_format_prompt}
确保响应结果可以由python json.loads解析
"""

response_format_prompt = """
{
    "action": {
        "name": "action name",
        "args": {
             "answer": "任务的最终结果"
        }
    },
    "thoughts":
    {
        "plan": "简短的描述短期和长期的计划列表",
        "criticism": "建设性的自我批评",
        "speak": "当前步骤，返回给用户的总结",
        "reasoning": "推理"
    },
    "observation": "观察当前任务的整体进度"
}
"""

# todo: query, agent_scratch, actions
action_prompt = gen_tools_desc()
constraints_prompt = "\n".join([f"{idx+1}. {con}" for idx, con in enumerate(constraints)])
resources_prompt = "\n".join([f"{idx+1}. {con}" for idx, con in enumerate(resources)])
best_practices_prompt = "\n".join([f"{idx+1}. {con}" for idx, con in enumerate(best_practices)])


def gen_prompt(query, agent_scratch):
    prompt = prompt_template.format(
        query=query,
        constraints=constraints_prompt,
        actions=action_prompt,
        resources=resources_prompt,
        best_practices=best_practices_prompt,
        agent_scratch=agent_scratch,
        response_format_prompt=response_format_prompt
    )
    return prompt


user_prompt = "根据给定的目标和迄今为止取得的进展，确定下一个要执行的action，并使用前面指定的JSON模式进行响应："
```

## 通义千问大模型 作为基座模型接入

经过[调研，在](https://www.zhihu.com/search?q=%E5%AE%A2%E6%88%B7%E7%AB%AF&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A700687839%7D)开源的模型中通义千问的指令遵循效果较好，尤其是格式输出的遵循比较好。一个能按格式输出的需求很重要，这便于模型结果的解析并提升模型调用的成功率（结果解析失败需要重新调用模型），本项目采用 客户端 的方式通义千问模型。

```python
import os
import json
import dashscope
from dashscope.api_entities.dashscope_response import Message
from prompt_cn import user_prompt


class ModelProvider(object):
    def __init__(self):
        self.api_key = os.environ.get("API_KEY")
        self.model_name = os.environ.get("MODEL_NAME")
        self._client = dashscope.Generation()
        print("model_name:", self.model_name)
        self.max_retry_time = 3

    def chat(self, prompt, chat_history):
        cur_retry_time = 0
        while cur_retry_time < self.max_retry_time:
            cur_retry_time += 1
            try:
                messages = [Message(role='system', content=prompt)]
                for his in chat_history:
                    messages.append(Message(role='user', content=his[0]))
                    messages.append(Message(role='assistant', content=his[1]))
                messages.append(Message(role='user', content=user_prompt))
                response = self._client.call(
                    model=self.model_name,
                    api_key=self.api_key,
                    messages=messages
                )
                """
                {
                    "status_code": 200,
                     "request_id": "c965bd27-c89c-9b5c-924d-2f1688e8041e", 
                     "code": "", 
                     "message": "", 
                     "output": {
                        "text": null, "finish_reason": null,
                         "choices": [{
                            "finish_reason": "null", "message": 
                            {"role": "assistant", "content": "当然可以，这里有一个简单又美味"}
                        }]
                    }, 
                    "usage": {
                        "input_tokens": 31, 
                        "output_tokens": 8, 
                        "total_tokens": 39, 
                        "plugins": {}
                    }
                }
                """
                print("response:", response)

                content = json.loads(response['output']['text'])
                return content
            except Exception as err:
                print("调用大模型出错：{}".format(err))
            return {}
```

## 其他说明

本项目中，调用大模型的`api_key`，模型名及调用搜索的key均通过环境变量的方式获取，只需要配置`.env`文件即可。

```env
# bailian
MODEL_NAME=
API_KEY=sk-

# search
TAVILY_API_KEY=tvly-
```

让环境变量生效，只需要在项目启动入口执行如下语句，注意`.env`需要放到当前执行的目录下，或者指定`load_dotenv`的参数`dotenv_path`：

```python
from model_provider import ModelProvider

# load env
load_dotenv()
```

## 总结

[更多相关](https://link.zhihu.com/?target=https%3A//www.bilibili.com/video/BV1Sz421m7Rr/%3Fspm_id_from%3D333.999.0.0%26vd_source%3Dcd62f6bf001b64bc3c0e062e4c37bc6b)内容，可以同步关注B站( 视频) ：acedar-C大，知乎&👸号：aC大，也可加v进群交流：aa836304831
