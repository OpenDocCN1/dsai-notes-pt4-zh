# 实现与大模型的交互

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/BxgRdROdqoUAxOxVcGacPcjonF4](https://kq4b3vgg5b.feishu.cn/docx/BxgRdROdqoUAxOxVcGacPcjonF4)

## 本地Jupyter调用OpneAI API实现与大模型的交互

进入本节课程之前，大家需要掌握：

1.  如何在Windows机器上通过Anaconda3 启动一个Jupyter笔记本

1.  具备🪜环境，并且配置为全局代理

1.  具备一个有效的OpenAI API Keys

# 1. 安装最新的OpenAI依赖包

```
! pip install --upgrade openai
```

```
Requirement already satisfied: openai in d:\software\anaconda3\lib\site-packages (1.26.0)
Collecting openai
  Obtaining dependency information for openai from https://files.pythonhosted.org/packages/16/35/10bb93c51c446370cf760ea0e722c78755bbe0572aeb4663bf8c19c9b082/openai-1.30.1-py3-none-any.whl.metadata
  Downloading openai-1.30.1-py3-none-any.whl.metadata (21 kB)
Requirement already satisfied: anyio&lt;5,&gt;=3.5.0 in d:\software\anaconda3\lib\site-packages (from openai) (3.5.0)
Requirement already satisfied: distro&lt;2,&gt;=1.7.0 in d:\software\anaconda3\lib\site-packages (from openai) (1.8.0)
Requirement already satisfied: httpx&lt;1,&gt;=0.23.0 in d:\software\anaconda3\lib\site-packages (from openai) (0.25.1)
Requirement already satisfied: pydantic&lt;3,&gt;=1.9.0 in d:\software\anaconda3\lib\site-packages (from openai) (2.6.0)
Requirement already satisfied: sniffio in d:\software\anaconda3\lib\site-packages (from openai) (1.2.0)
Requirement already satisfied: tqdm&gt;4 in d:\software\anaconda3\lib\site-packages (from openai) (4.66.4)
Requirement already satisfied: typing-extensions&lt;5,&gt;=4.7 in d:\software\anaconda3\lib\site-packages (from openai) (4.10.0)
Requirement already satisfied: idna&gt;=2.8 in d:\software\anaconda3\lib\site-packages (from anyio&lt;5,&gt;=3.5.0-&gt;openai) (3.4)
Requirement already satisfied: certifi in d:\software\anaconda3\lib\site-packages (from httpx&lt;1,&gt;=0.23.0-&gt;openai) (2024.2.2)
Requirement already satisfied: httpcore in d:\software\anaconda3\lib\site-packages (from httpx&lt;1,&gt;=0.23.0-&gt;openai) (1.0.1)
Requirement already satisfied: annotated-types&gt;=0.4.0 in d:\software\anaconda3\lib\site-packages (from pydantic&lt;3,&gt;=1.9.0-&gt;openai) (0.6.0)
Requirement already satisfied: pydantic-core==2.16.1 in d:\software\anaconda3\lib\site-packages (from pydantic&lt;3,&gt;=1.9.0-&gt;openai) (2.16.1)
Requirement already satisfied: colorama in d:\software\anaconda3\lib\site-packages (from tqdm&gt;4-&gt;openai) (0.4.6)
Requirement already satisfied: h11&lt;0.15,&gt;=0.13 in d:\software\anaconda3\lib\site-packages (from httpcore-&gt;httpx&lt;1,&gt;=0.23.0-&gt;openai) (0.14.0)
Downloading openai-1.30.1-py3-none-any.whl (320 kB)
   ---------------------------------------- 0.0/320.6 kB ? eta -:--:--
   ---------------------------------------- 0.0/320.6 kB ? eta -:--:--
   - -------------------------------------- 10.2/320.6 kB ? eta -:--:--
   - -------------------------------------- 10.2/320.6 kB ? eta -:--:--
   - -------------------------------------- 10.2/320.6 kB ? eta -:--:--
   --- ----------------------------------- 30.7/320.6 kB 131.3 kB/s eta 0:00:03
   --- ----------------------------------- 30.7/320.6 kB 131.3 kB/s eta 0:00:03
   --- ----------------------------------- 30.7/320.6 kB 131.3 kB/s eta 0:00:03
   ---- ---------------------------------- 41.0/320.6 kB 115.5 kB/s eta 0:00:03
   ------- ------------------------------- 61.4/320.6 kB 156.1 kB/s eta 0:00:02
   ------- ------------------------------- 61.4/320.6 kB 156.1 kB/s eta 0:00:02
   --------- ----------------------------- 81.9/320.6 kB 176.6 kB/s eta 0:00:02
   --------- ----------------------------- 81.9/320.6 kB 176.6 kB/s eta 0:00:02
   -------------- ----------------------- 122.9/320.6 kB 225.3 kB/s eta 0:00:01
   --------------- ---------------------- 133.1/320.6 kB 231.4 kB/s eta 0:00:01
   --------------- ---------------------- 133.1/320.6 kB 231.4 kB/s eta 0:00:01
   --------------------- ---------------- 184.3/320.6 kB 278.5 kB/s eta 0:00:01
   ------------------------ ------------- 204.8/320.6 kB 276.8 kB/s eta 0:00:01
   ------------------------------- ------ 266.2/320.6 kB 348.8 kB/s eta 0:00:01
   ------------------------------- ------ 266.2/320.6 kB 348.8 kB/s eta 0:00:01
   ------------------------------- ------ 266.2/320.6 kB 348.8 kB/s eta 0:00:01
   ------------------------------- ------ 266.2/320.6 kB 348.8 kB/s eta 0:00:01
   ------------------------------- ------ 266.2/320.6 kB 348.8 kB/s eta 0:00:01
   ------------------------------- ------ 266.2/320.6 kB 348.8 kB/s eta 0:00:01
   ------------------------------- ------ 266.2/320.6 kB 348.8 kB/s eta 0:00:01
   ------------------------------- ------ 266.2/320.6 kB 348.8 kB/s eta 0:00:01
   -------------------------------------- 320.6/320.6 kB 283.7 kB/s eta 0:00:00
Installing collected packages: openai
  Attempting uninstall: openai
    Found existing installation: openai 1.26.0
    Uninstalling openai-1.26.0:
      Successfully uninstalled openai-1.26.0
Successfully installed openai-1.30.1


ERROR: pip&#x27;s dependency resolver does not currently take into account all the packages that are installed. This behaviour is the source of the following dependency conflicts.
llama-index-core 0.10.37.post1 requires fsspec&gt;=2023.5.0, but you have fsspec 2023.4.0 which is incompatible.
llama-index-legacy 0.9.48 requires fsspec&gt;=2023.5.0, but you have fsspec 2023.4.0 which is incompatible.
langchain-openai 0.0.8 requires langchain-core&lt;0.2.0,&gt;=0.1.27, but you have langchain-core 0.0.13 which is incompatible.
```

```
import openai
print(openai.__version__)
```

```
1.30.1
```

# 2. 设置全局代理

对于国内用户，是无法直接访问OpenAI的，需要让Jupyter在代理环境下启动，即需要令Jupyter可以通过代理来访问网络。这就需要大家找到自己所使用魔法的代理端口是多少。不同的魔法软件默认配置是不一样的，所以需要大家自行查看。以我个人使用的魔法软件为示例：

```
import os
os.environ[&#x27;http_proxy&#x27;] = &quot;http://127.0.0.1:15732&quot;
os.environ[&#x27;https_proxy&#x27;] =&quot;http://127.0.0.1:15732&quot;
```

+   测试能否调用OpenAI API

```
from openai import OpenAI

#client = OpenAI(api_key=&#x27;sk-xxxxxxx&#x27;)

client = OpenAI()

completion = client.chat.completions.create(
  model=&quot;gpt-4&quot;,
  messages=[
    {&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;你是一位乐于助人的AI小助理&quot;},
    {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;你好，请你介绍一下你自己&quot;}
  ]
)

print(completion.choices[0].message)
```

```
ChatCompletionMessage(content=&#x27;你好，我是一款Artificial Intelligence小助手，由OpenAI研发，主要职责是为用户提供各种信息查询、任务管理和决策支持等方面的帮助。我能解答你的各类问题，从天气状况、日常知识问答，到复杂的问题解决方案，甚至在某些情况下，我还能进行简单的心理疏导哦。我的工作效率高，响应速度快，并且不知疲倦。你可以随时随地地向我提问或表示需求，我都会尽我所能为你提供帮助。&#x27;, role=&#x27;assistant&#x27;, function_call=None, tool_calls=None)
```

只要能够顺利得到大模型的回复，则说明已经能够正常地与GPT模型进行对话交互了。

---

🍻现开设了大**模型学习交流群，**扫描下👇码，来遇见更多志同道合的小伙伴~

![](img/2b23b2e4b354ffc1b9a644e2b21595ef.png)

海量硬核独家技术**干货内容**+无门槛**技术交流****+不定期开设****硬核干货&前沿技术公开课****，****扫码**👆即刻入群！