# 手把手带你从0到1实现大模型agent - 知乎

> 来源：[https://agijuejin.feishu.cn/docx/Itw6dJE1Po1x2DxTRYGcmGFIn1f](https://agijuejin.feishu.cn/docx/Itw6dJE1Po1x2DxTRYGcmGFIn1f)

> 作者：zuaC大

> 原文：[https://zhuanlan.z](https://zhuanlan.zhihu.com/p/700687839)hihu.com/p/700687...

## 前沿

最近出了 《手把手带你从0到1实现大模型agent》 视频系[列课程，从](https://www.zhihu.com/search?q=%E5%BC%80%E6%BA%90%E6%A1%86%E6%9E%B6&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A700687839%7D)无到有的实现一个完整的大模型agent。其实市面上已经有很多agent的 开源框架 ，实际的应用在框架基础上开发即可。出这个系列的原因主要还是希望能让大家对agent的底层原理和逻辑有一个清晰的认识，所谓知其然，更知其所以然。

## 目录

该 [系列课程的视频 ](https://link.zhihu.com/?target=https%3A//www.bilibili.com/video/BV1Sz421m7Rr/%3Fspm_id_from%3D333.999.0.0%26vd_source%3Dcd62f6bf001b64bc3c0e062e4c37bc6b)内容包括的核心内容，下文介绍核心模块及代码:

+   agent核心逻辑实现及讲解

+   agent各模块功能分析和梳理

+   tools的定义及与action的映射

+   prompt模板定义

+   prompt模板定义及工具prompt集成

+   通义千问大模型 作为基座模型接入

+   agent完整链路调试

+   针对不足进行 agent优化 升级

## 核心逻辑实现

[核心逻辑模](https://www.zhihu.com/search?q=%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A700687839%7D)块也是整个项目的入口，集成了 1) 环境变量 的设置 ; 2) 工具(action)的引入; 3) prompt模板；4)模型的初始化；5) 整个逻辑的交互。代码如下

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


if __name__ == &quot;__main__&quot;:
    main()
```

## tools的定义及与action的映射

agent中交互式让模型给出下一[步需要执行](https://www.zhihu.com/search?q=%E5%88%A4%E5%88%AB%E6%A8%A1%E5%9E%8B&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A700687839%7D)的动作(action，有点像 判别模型 )， 每个动作(action)的执行内容是需要提前定义好的，每个动作执行的定义我们称为工具，每个工具和动作是一一对应的，这里通过函数定义的方式定义工具。下文包括了读文件、写文件、增加内容及调用搜索功能。tools_map定义了action到tools的映射。

```
imp[ort ](https://www.zhihu.com/search?q=%E7%BD%91%E7%BB%9C%E6%90%9C%E7%B4%A2&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A700687839%7D)os
import[ json
from l](https://www.zhihu.com/search?q=workdir_root&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A700687839%7D)[angchain_c](https://www.zhihu.com/search?q=os.environ&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A700687839%7D)ommunity[.tools.tavil](https://www.zhihu.com/search?q=content_list&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A700687839%7D)y_search impo[rt TavilySe](https://www.zhihu.com/search?q=description&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A700687839%7D)[archResults

](https://www.zhihu.com/search?q=write_to_file&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A700687839%7D)&quot;&quot;[&quot;
1. 写文件
2. 读](https://www.zhihu.com/search?q=search%20engine&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A700687839%7D)文件
3. 追加
4. 网络搜索 
&quot;&quot;&quot;


def _get_workdir_root():
    workdir_root = os.environ.get(&quot;WORKDIR_ROOT&quot;, &#x27;./data/llm_result&#x27;)
    return workdir_root


WORKDIR_ROOT = _get_workdir_root()


def read_file(filename):
    filename = os.path.join(WORKDIR_ROOT, filename)
    if not os.path.exists(filename):
        return f&quot;{filename} not exist, please check file exist before read&quot;
    with open(filename, &#x27;r&#x27;, encoding=&#x27;utf-8&#x27;) as f:
        return &quot;\n&quot;.join(f.readlines())


def append_to_file(filename, content):
    filename = os.path.join(WORKDIR_ROOT, filename)
    if not os.path.exists(filename):
        return f&quot;{filename} not exist, please check file exist before read&quot;

    with open(filename, &#x27;a&#x27;, encoding=&#x27;utf-8&#x27;) as f:
        f.write(content)
    return &#x27;append content to file success&#x27;


def write_to_file(filename, content):
    filename = os.path.join(WORKDIR_ROOT, filename)
    if not os.path.exists(WORKDIR_ROOT):
        os.makedirs(WORKDIR_ROOT)

    with open(filename, &#x27;w&#x27;, encoding=&#x27;utf-8&#x27;) as f:
        f.write(content)
    return &#x27;write content to file success&#x27;


def search(query):
    tavily = TavilySearchResults(max_results=5)

    try:
        ret = tavily.invoke(input=query)

        &quot;&quot;&quot;
        ret:
        [{
            &quot;content&quot;: &quot;&quot;,
            &quot;url&quot;:
        }]
        &quot;&quot;&quot;
        print(&quot;搜索结果:&quot;, ret)
        content_list = [obj[&#x27;content&#x27;] for obj in ret]
        return &quot;\n&quot;.join(content_list)
    except Exception as err:
        return &quot;search err: {}&quot;.format(err)


tools_info = [
    {
        &quot;name&quot;: &quot;read_file&quot;,
        &quot;description&quot;: &quot;read file from agent generate, should write file before read&quot;,
        &quot;args&quot;: [{
            &quot;name&quot;: &quot;filename&quot;,
            &quot;type&quot;: &quot;string&quot;,
            &quot;description&quot;: &quot;read file name&quot;
        }]
    },
    {
        &quot;name&quot;: &quot;append_to_file&quot;,
        &quot;description&quot;: &quot;append llm content to file, should write file before read&quot;,
        &quot;args&quot;: [{
            &quot;name&quot;: &quot;filename&quot;,
            &quot;type&quot;: &quot;string&quot;,
            &quot;description&quot;: &quot;file name&quot;
        }, {
            &quot;name&quot;: &quot;content&quot;,
            &quot;type&quot;: &quot;string&quot;,
            &quot;description&quot;: &quot;append to file content&quot;
        }]
    },
    {
        &quot;name&quot;: &quot;write_to_file&quot;,
        &quot;description&quot;: &quot;write llm content to file&quot;,
        &quot;args&quot;: [{
            &quot;name&quot;: &quot;filename&quot;,
            &quot;type&quot;: &quot;string&quot;,
            &quot;description&quot;: &quot;file name&quot;
        }, {
            &quot;name&quot;: &quot;content&quot;,
            &quot;type&quot;: &quot;string&quot;,
            &quot;description&quot;: &quot;write to file content&quot;
        }]
    },
    {
        &quot;name&quot;: &quot;search&quot;,
        &quot;description&quot;: &quot;this is a search engine, you can gain additional knowledge though this search engine &quot;
                       &quot;when you are unsure of what large model return &quot;,
        &quot;args&quot;: [{
            &quot;name&quot;: &quot;query&quot;,
            &quot;type&quot;: &quot;string&quot;,
            &quot;description&quot;: &quot;search query to look up&quot;
        }]
    },
    {
        &quot;name&quot;: &quot;finish&quot;,
        &quot;description&quot;: &quot;return finish when you get exactly the right answer&quot;,
        &quot;args&quot;: [{
            &quot;name&quot;: &quot;answer&quot;,
            &quot;type&quot;: &quot;string&quot;,
            &quot;description&quot;: &quot;the final answer&quot;
        }]
    }
]

tools_map = {
    &quot;read_file&quot;: read_file,
    &quot;append_to_file&quot;: append_to_file,
    &quot;write_to_file&quot;: write_to_file,
    &quot;search&quot;: search
}
```

## prompt模板定义

prompt模板的定义需要包括工具的描述，返回结果及如何引导模型如何优化效果的描述等，当然具体的内容可以根据自身情况修改，并不是一成不变的，我实现的模板如下：

其中“{}”中的内容为需要补充填写的，每个定义也不一样，需要拼接成字符串的形式加到prompt中。

```
[constraints = [](https://www.zhihu.com/search?q=prompt_template&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A700687839%7D)[
   ](https://www.zhihu.com/search?q=%E6%9C%80%E4%BD%B3%E5%AE%9E%E8%B7%B5&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A700687839%7D) &quot;仅使用下面列出的动作&quot;,
    &quot;你只能主动行动，在计划行动时需要考虑到这一点&quot;,
    &quot;你无法与物理对象交互，如果对于完成任务或目标是绝对必要的，则必须要求用户为你完成，如果用户拒绝，并且没有其他方法实现目标，则直接终止，避免浪费时间和精力。&quot;
]

resources = [
    &quot;提供搜索和信息收集的互联网接入&quot;,
    &quot;读取和写入文件的能力&quot;,
    &quot;你是一个大语言模型，接受了大量文本的训练，包括大量的事实知识，利用这些知识来避免不必要的信息收集&quot;
]

best_practices = [
    &quot;不断地回顾和分析你的行为，确保发挥出你最大的能力&quot;,
    &quot;不断地进行建设性的自我批评&quot;,
    &quot;反思过去的决策和策略，完善你的方案&quot;,
    &quot;每个动作执行都有代价，所以要聪明高效，目的是用最少的步骤完成任务&quot;,
    &quot;利用你的信息收集能力来寻找你不知道的信息&quot;
]

prompt_template = &quot;&quot;&quot;
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
&quot;&quot;&quot;

response_format_prompt = &quot;&quot;&quot;
{
    &quot;action&quot;: {
        &quot;name&quot;: &quot;action name&quot;,
        &quot;args&quot;: {
             &quot;answer&quot;: &quot;任务的最终结果&quot;
        }
    },
    &quot;thoughts&quot;:
    {
        &quot;plan&quot;: &quot;简短的描述短期和长期的计划列表&quot;,
        &quot;criticism&quot;: &quot;建设性的自我批评&quot;,
        &quot;speak&quot;: &quot;当前步骤，返回给用户的总结&quot;,
        &quot;reasoning&quot;: &quot;推理&quot;
    },
    &quot;observation&quot;: &quot;观察当前任务的整体进度&quot;
}
&quot;&quot;&quot;

# todo: query, agent_scratch, actions
action_prompt = gen_tools_desc()
constraints_prompt = &quot;\n&quot;.join([f&quot;{idx+1}. {con}&quot; for idx, con in enumerate(constraints)])
resources_prompt = &quot;\n&quot;.join([f&quot;{idx+1}. {con}&quot; for idx, con in enumerate(resources)])
best_practices_prompt = &quot;\n&quot;.join([f&quot;{idx+1}. {con}&quot; for idx, con in enumerate(best_practices)])


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


user_prompt = &quot;根据给定的目标和迄今为止取得的进展，确定下一个要执行的action，并使用前面指定的JSON模式进行响应：&quot;
```

## 通义千问大模型 作为基座模型接入

经过[调研，在](https://www.zhihu.com/search?q=%E5%AE%A2%E6%88%B7%E7%AB%AF&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A700687839%7D)开源的模型中通义千问的指令遵循效果较好，尤其是格式输出的遵循比较好。一个能按格式输出的需求很重要，这便于模型结果的解析并提升模型调用的成功率（结果解析失败需要重新调用模型），本项目采用 客户端 的方式通义千问模型。

```
import o[s
import json
](https://www.zhihu.com/search?q=cur_retry_time&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A700687839%7D)import dashscope
fr[om dashscope.](https://www.zhihu.com/search?q=finish_reason&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A700687839%7D)[api_entities](https://www.zhihu.com/search?q=total_tokens&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A700687839%7D).dashscope_response import Message
from prompt_cn import user_prompt


class ModelProvider(object):
    def __init__(self):
        self.api_key = os.environ.get(&quot;API_KEY&quot;)
        self.model_name = os.environ.get(&quot;MODEL_NAME&quot;)
        self._client = dashscope.Generation()
        print(&quot;model_name:&quot;, self.model_name)
        self.max_retry_time = 3

    def chat(self, prompt, chat_history):
        cur_retry_time = 0
        while cur_retry_time &lt; self.max_retry_time:
            cur_retry_time += 1
            try:
                messages = [Message(role=&#x27;system&#x27;, content=prompt)]
                for his in chat_history:
                    messages.append(Message(role=&#x27;user&#x27;, content=his[0]))
                    messages.append(Message(role=&#x27;assistant&#x27;, content=his[1]))
                messages.append(Message(role=&#x27;user&#x27;, content=user_prompt))
                response = self._client.call(
                    model=self.model_name,
                    api_key=self.api_key,
                    messages=messages
                )
                &quot;&quot;&quot;
                {
                    &quot;status_code&quot;: 200,
                     &quot;request_id&quot;: &quot;c965bd27-c89c-9b5c-924d-2f1688e8041e&quot;, 
                     &quot;code&quot;: &quot;&quot;, 
                     &quot;message&quot;: &quot;&quot;, 
                     &quot;output&quot;: {
                        &quot;text&quot;: null, &quot;finish_reason&quot;: null,
                         &quot;choices&quot;: [{
                            &quot;finish_reason&quot;: &quot;null&quot;, &quot;message&quot;: 
                            {&quot;role&quot;: &quot;assistant&quot;, &quot;content&quot;: &quot;当然可以，这里有一个简单又美味&quot;}
                        }]
                    }, 
                    &quot;usage&quot;: {
                        &quot;input_tokens&quot;: 31, 
                        &quot;output_tokens&quot;: 8, 
                        &quot;total_tokens&quot;: 39, 
                        &quot;plugins&quot;: {}
                    }
                }
                &quot;&quot;&quot;
                print(&quot;response:&quot;, response)

                content = json.loads(response[&#x27;output&#x27;][&#x27;text&#x27;])
                return content
            except Exception as err:
                print(&quot;调用大模型出错：{}&quot;.format(err))
            return {}
```

## 其他说明

本项目中，调用大模型的api_key，模型名及调用搜索的key均通过环境变量的方式获取，只需要配置.env文件即可。

```
# bailian
MODEL_NAME=
API_KEY=sk-

# search
TAVILY_API_KEY=tvly-
```

让环境变量生效，只需要在项目启动入口执行如下语句，注意.env需要放到当前执行的目录下，或者指定load_dotenv的参数dotenv_path：

```
from model_provider import ModelProvider

# load env
load_dotenv()
```

## 总结

[更多相关](https://link.zhihu.com/?target=https%3A//www.bilibili.com/video/BV1Sz421m7Rr/%3Fspm_id_from%3D333.999.0.0%26vd_source%3Dcd62f6bf001b64bc3c0e062e4c37bc6b)内容，可以同步关注B站( 视频) ：acedar-C大，知乎&公众号：aC大，也可加v进群交流：aa836304831