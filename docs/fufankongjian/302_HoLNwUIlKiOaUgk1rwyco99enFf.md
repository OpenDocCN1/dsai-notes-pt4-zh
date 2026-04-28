# 从0到1构建AI Agent

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/UpX0dA80doAGMqxF7udcxQmKnmg](https://kq4b3vgg5b.feishu.cn/docx/UpX0dA80doAGMqxF7udcxQmKnmg)

## 如何从 0 到 1 开发一个 AI Agent

在本节课中，我们将把理论概念具体化，实际构建一个类似于旅游规划的自主AI智能体。在这个过程中，我们将针对提到的三个构建AI Agent智能体关键概念一一进行模型、工具、框架的选型及说明。具体安排如下：

1.  选择目前综合性能最强的 OpenAI GPT 系列模型作为 AI Agent的基座模型，介绍如何在Python代码环境下调用其API进行交互。

1.  选择最主流、热门的应用开发框架LangChain，介绍其整体架构，并实际操作如何使用LangChain集成OpenAI的GPT模型，同时配置所需的工具库。

1.  选择新兴的AI Agent工具 CrewAI ,该工具可以赋予大模型自主规划和执行任务的能力，让大模型无缝协作来处理复杂的任务，且操作非常简答。

最终通过上述三种技术的结合，我们将能构建一个能够自主完成特定的任务的AI智能体，且具备非常强的可扩展性。

首先，我们来看第一部分，如何在本地的Python环境下调用 OpenAI 的 GPT 模型。

# 1. 如何调用OpenAI的GPT在线模型

需要提前说明的是：在进行本节内容的实操之前，请确保满足以下软硬件要求：

1.  可以在 Windows 机器上通过 Anaconda3 启动一个Jupyter NoteBook

1.  具备科学上网环境，并且配置为全局代理

1.  具备一个OpenAI的有效API Keys（地址：https://platform.openai.com/api-keys）

直接使用Python调用的代码示例可以在官方找到，位置如下：https://platform.openai.com/docs/api-reference/chat

![](img/e7bd76dd396a08251352cd1bff22047d.png)

这里我们在当前环境下进行调用测试。但在实际执行之前，需要先安装OpenAI的第三方依赖包。

```
# ! pip install openai
```

安装完成后，需要重启当前的Jupyter Lab才能使Python虚拟环境生效。

![](img/51220078420b977c707968db6f8ead7d.png)

重启Jupyter 完成后，可执行如下代码进行版本的校验。

```
import openai

print(openai.__version__)
```

```
1.40.6
```

接下来执行调用测试。这里有一点需要强调，我们建议大家将OpenAI 的API Keys配置在全局环境变量中，设置方法如下：https://platform.openai.com/docs/quickstart

![](img/689a7fde17f9a0e218c2ad056b9e6da0.png)

除此之外，对于国内用户，是无法直接访问OpenAI的，需要让Jupyter在代理环境下启动，即需要令Jupyter可以通过代理来访问网络。这就需要大家找到自己所使用魔法的代理端口是多少。不同的魔法软件默认配置是不一样的，所以需要大家自行查看。以我个人使用的魔法软件为示例：

```
import os
os.environ[&#x27;http_proxy&#x27;] = &quot;http://127.0.0.1:15732&quot;
os.environ[&#x27;https_proxy&#x27;] =&quot;http://127.0.0.1:15732&quot;
```

如若没有将OpenAI API Keys 设置为全局的环境变量，也可以显式的写在代码逻辑中（即取消下述代码中注释的一行，并替换为自己实际的API Keys）。

```
from openai import OpenAI
client = OpenAI()

completion = client.chat.completions.create(
    # api_key=&#x27;sk-xxxxxxxx&#x27;,
    model=&quot;gpt-4o&quot;,
    messages=[
    {&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;你是一位乐于助人的智能助手&quot;},
    {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;你好&quot;}
  ]
)

print(completion.choices[0].message)
```

```
---------------------------------------------------------------------------

ConnectTimeout                            Traceback (most recent call last)

File ~\anaconda3\envs\multi_agent\Lib\site-packages\httpx\_transports\default.py:69, in map_httpcore_exceptions()
     68 try:
---&gt; 69     yield
     70 except Exception as exc:


File ~\anaconda3\envs\multi_agent\Lib\site-packages\httpx\_transports\default.py:233, in HTTPTransport.handle_request(self, request)
    232 with map_httpcore_exceptions():
--&gt; 233     resp = self._pool.handle_request(req)
    235 assert isinstance(resp.stream, typing.Iterable)


File ~\anaconda3\envs\multi_agent\Lib\site-packages\httpcore\_sync\connection_pool.py:216, in ConnectionPool.handle_request(self, request)
    215     self._close_connections(closing)
--&gt; 216     raise exc from None
    218 # Return the response. Note that in this case we still have to manage
    219 # the point at which the response is closed.


File ~\anaconda3\envs\multi_agent\Lib\site-packages\httpcore\_sync\connection_pool.py:196, in ConnectionPool.handle_request(self, request)
    194 try:
    195     # Send the request on the assigned connection.
--&gt; 196     response = connection.handle_request(
    197         pool_request.request
    198     )
    199 except ConnectionNotAvailable:
    200     # In some cases a connection may initially be available to
    201     # handle a request, but then become unavailable.
    202     #
    203     # In this case we clear the connection and try again.


File ~\anaconda3\envs\multi_agent\Lib\site-packages\httpcore\_sync\http_proxy.py:317, in TunnelHTTPConnection.handle_request(self, request)
    316 with Trace(&quot;start_tls&quot;, logger, request, kwargs) as trace:
--&gt; 317     stream = stream.start_tls(**kwargs)
    318     trace.return_value = stream


File ~\anaconda3\envs\multi_agent\Lib\site-packages\httpcore\_sync\http11.py:383, in HTTP11UpgradeStream.start_tls(self, ssl_context, server_hostname, timeout)
    377 def start_tls(
    378     self,
    379     ssl_context: ssl.SSLContext,
    380     server_hostname: Optional[str] = None,
    381     timeout: Optional[float] = None,
    382 ) -&gt; NetworkStream:
--&gt; 383     return self._stream.start_tls(ssl_context, server_hostname, timeout)


File ~\anaconda3\envs\multi_agent\Lib\site-packages\httpcore\_backends\sync.py:152, in SyncStream.start_tls(self, ssl_context, server_hostname, timeout)
    148 exc_map: ExceptionMapping = {
    149     socket.timeout: ConnectTimeout,
    150     OSError: ConnectError,
    151 }
--&gt; 152 with map_exceptions(exc_map):
    153     try:


File ~\anaconda3\envs\multi_agent\Lib\contextlib.py:155, in _GeneratorContextManager.__exit__(self, typ, value, traceback)
    154 try:
--&gt; 155     self.gen.throw(typ, value, traceback)
    156 except StopIteration as exc:
    157     # Suppress StopIteration *unless* it&#x27;s the same exception that
    158     # was passed to throw().  This prevents a StopIteration
    159     # raised inside the &quot;with&quot; statement from being suppressed.


File ~\anaconda3\envs\multi_agent\Lib\site-packages\httpcore\_exceptions.py:14, in map_exceptions(map)
     13     if isinstance(exc, from_exc):
---&gt; 14         raise to_exc(exc) from exc
     15 raise


ConnectTimeout: _ssl.c:975: The handshake operation timed out


The above exception was the direct cause of the following exception:


ConnectTimeout                            Traceback (most recent call last)

File ~\anaconda3\envs\multi_agent\Lib\site-packages\openai\_base_client.py:972, in SyncAPIClient._request(self, cast_to, options, remaining_retries, stream, stream_cls)
    971 try:
--&gt; 972     response = self._client.send(
    973         request,
    974         stream=stream or self._should_stream_response_body(request=request),
    975         **kwargs,
    976     )
    977 except httpx.TimeoutException as err:


File ~\anaconda3\envs\multi_agent\Lib\site-packages\httpx\_client.py:914, in Client.send(self, request, stream, auth, follow_redirects)
    912 auth = self._build_request_auth(request, auth)
--&gt; 914 response = self._send_handling_auth(
    915     request,
    916     auth=auth,
    917     follow_redirects=follow_redirects,
    918     history=[],
    919 )
    920 try:


File ~\anaconda3\envs\multi_agent\Lib\site-packages\httpx\_client.py:942, in Client._send_handling_auth(self, request, auth, follow_redirects, history)
    941 while True:
--&gt; 942     response = self._send_handling_redirects(
    943         request,
    944         follow_redirects=follow_redirects,
    945         history=history,
    946     )
    947     try:


File ~\anaconda3\envs\multi_agent\Lib\site-packages\httpx\_client.py:979, in Client._send_handling_redirects(self, request, follow_redirects, history)
    977     hook(request)
--&gt; 979 response = self._send_single_request(request)
    980 try:


File ~\anaconda3\envs\multi_agent\Lib\site-packages\httpx\_client.py:1015, in Client._send_single_request(self, request)
   1014 with request_context(request=request):
-&gt; 1015     response = transport.handle_request(request)
   1017 assert isinstance(response.stream, SyncByteStream)


File ~\anaconda3\envs\multi_agent\Lib\site-packages\httpx\_transports\default.py:232, in HTTPTransport.handle_request(self, request)
    220 req = httpcore.Request(
    221     method=request.method,
    222     url=httpcore.URL(
   (...)
    230     extensions=request.extensions,
    231 )
--&gt; 232 with map_httpcore_exceptions():
    233     resp = self._pool.handle_request(req)


File ~\anaconda3\envs\multi_agent\Lib\contextlib.py:155, in _GeneratorContextManager.__exit__(self, typ, value, traceback)
    154 try:
--&gt; 155     self.gen.throw(typ, value, traceback)
    156 except StopIteration as exc:
    157     # Suppress StopIteration *unless* it&#x27;s the same exception that
    158     # was passed to throw().  This prevents a StopIteration
    159     # raised inside the &quot;with&quot; statement from being suppressed.


File ~\anaconda3\envs\multi_agent\Lib\site-packages\httpx\_transports\default.py:86, in map_httpcore_exceptions()
     85 message = str(exc)
---&gt; 86 raise mapped_exc(message) from exc


ConnectTimeout: _ssl.c:975: The handshake operation timed out


The above exception was the direct cause of the following exception:


APITimeoutError                           Traceback (most recent call last)

Cell In[9], line 4
      1 from openai import OpenAI
      2 client = OpenAI()
----&gt; 4 completion = client.chat.completions.create(
      5     # api_key=&#x27;sk-xxxxxxxx&#x27;,
      6     model=&quot;gpt-4o&quot;,
      7     messages=[
      8     {&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;你是一位乐于助人的智能助手&quot;},
      9     {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;你好&quot;}
     10   ]
     11 )
     13 print(completion.choices[0].message)


File ~\anaconda3\envs\multi_agent\Lib\site-packages\openai\_utils\_utils.py:274, in required_args.&lt;locals&gt;.inner.&lt;locals&gt;.wrapper(*args, **kwargs)
    272             msg = f&quot;Missing required argument: {quote(missing[0])}&quot;
    273     raise TypeError(msg)
--&gt; 274 return func(*args, **kwargs)


File ~\anaconda3\envs\multi_agent\Lib\site-packages\openai\resources\chat\completions.py:668, in Completions.create(self, messages, model, frequency_penalty, function_call, functions, logit_bias, logprobs, max_tokens, n, parallel_tool_calls, presence_penalty, response_format, seed, service_tier, stop, stream, stream_options, temperature, tool_choice, tools, top_logprobs, top_p, user, extra_headers, extra_query, extra_body, timeout)
    633 @required_args([&quot;messages&quot;, &quot;model&quot;], [&quot;messages&quot;, &quot;model&quot;, &quot;stream&quot;])
    634 def create(
    635     self,
   (...)
    665     timeout: float | httpx.Timeout | None | NotGiven = NOT_GIVEN,
    666 ) -&gt; ChatCompletion | Stream[ChatCompletionChunk]:
    667     validate_response_format(response_format)
--&gt; 668     return self._post(
    669         &quot;/chat/completions&quot;,
    670         body=maybe_transform(
    671             {
    672                 &quot;messages&quot;: messages,
    673                 &quot;model&quot;: model,
    674                 &quot;frequency_penalty&quot;: frequency_penalty,
    675                 &quot;function_call&quot;: function_call,
    676                 &quot;functions&quot;: functions,
    677                 &quot;logit_bias&quot;: logit_bias,
    678                 &quot;logprobs&quot;: logprobs,
    679                 &quot;max_tokens&quot;: max_tokens,
    680                 &quot;n&quot;: n,
    681                 &quot;parallel_tool_calls&quot;: parallel_tool_calls,
    682                 &quot;presence_penalty&quot;: presence_penalty,
    683                 &quot;response_format&quot;: response_format,
    684                 &quot;seed&quot;: seed,
    685                 &quot;service_tier&quot;: service_tier,
    686                 &quot;stop&quot;: stop,
    687                 &quot;stream&quot;: stream,
    688                 &quot;stream_options&quot;: stream_options,
    689                 &quot;temperature&quot;: temperature,
    690                 &quot;tool_choice&quot;: tool_choice,
    691                 &quot;tools&quot;: tools,
    692                 &quot;top_logprobs&quot;: top_logprobs,
    693                 &quot;top_p&quot;: top_p,
    694                 &quot;user&quot;: user,
    695             },
    696             completion_create_params.CompletionCreateParams,
    697         ),
    698         options=make_request_options(
    699             extra_headers=extra_headers, extra_query=extra_query, extra_body=extra_body, timeout=timeout
    700         ),
    701         cast_to=ChatCompletion,
    702         stream=stream or False,
    703         stream_cls=Stream[ChatCompletionChunk],
    704     )


File ~\anaconda3\envs\multi_agent\Lib\site-packages\openai\_base_client.py:1259, in SyncAPIClient.post(self, path, cast_to, body, options, files, stream, stream_cls)
   1245 def post(
   1246     self,
   1247     path: str,
   (...)
   1254     stream_cls: type[_StreamT] | None = None,
   1255 ) -&gt; ResponseT | _StreamT:
   1256     opts = FinalRequestOptions.construct(
   1257         method=&quot;post&quot;, url=path, json_data=body, files=to_httpx_files(files), **options
   1258     )
-&gt; 1259     return cast(ResponseT, self.request(cast_to, opts, stream=stream, stream_cls=stream_cls))


File ~\anaconda3\envs\multi_agent\Lib\site-packages\openai\_base_client.py:936, in SyncAPIClient.request(self, cast_to, options, remaining_retries, stream, stream_cls)
    927 def request(
    928     self,
    929     cast_to: Type[ResponseT],
   (...)
    934     stream_cls: type[_StreamT] | None = None,
    935 ) -&gt; ResponseT | _StreamT:
--&gt; 936     return self._request(
    937         cast_to=cast_to,
    938         options=options,
    939         stream=stream,
    940         stream_cls=stream_cls,
    941         remaining_retries=remaining_retries,
    942     )


File ~\anaconda3\envs\multi_agent\Lib\site-packages\openai\_base_client.py:981, in SyncAPIClient._request(self, cast_to, options, remaining_retries, stream, stream_cls)
    978 log.debug(&quot;Encountered httpx.TimeoutException&quot;, exc_info=True)
    980 if retries &gt; 0:
--&gt; 981     return self._retry_request(
    982         input_options,
    983         cast_to,
    984         retries,
    985         stream=stream,
    986         stream_cls=stream_cls,
    987         response_headers=None,
    988     )
    990 log.debug(&quot;Raising timeout error&quot;)
    991 raise APITimeoutError(request=request) from err


File ~\anaconda3\envs\multi_agent\Lib\site-packages\openai\_base_client.py:1074, in SyncAPIClient._retry_request(self, options, cast_to, remaining_retries, response_headers, stream, stream_cls)
   1070 # In a synchronous context we are blocking the entire thread. Up to the library user to run the client in a
   1071 # different thread if necessary.
   1072 time.sleep(timeout)
-&gt; 1074 return self._request(
   1075     options=options,
   1076     cast_to=cast_to,
   1077     remaining_retries=remaining,
   1078     stream=stream,
   1079     stream_cls=stream_cls,
   1080 )


File ~\anaconda3\envs\multi_agent\Lib\site-packages\openai\_base_client.py:981, in SyncAPIClient._request(self, cast_to, options, remaining_retries, stream, stream_cls)
    978 log.debug(&quot;Encountered httpx.TimeoutException&quot;, exc_info=True)
    980 if retries &gt; 0:
--&gt; 981     return self._retry_request(
    982         input_options,
    983         cast_to,
    984         retries,
    985         stream=stream,
    986         stream_cls=stream_cls,
    987         response_headers=None,
    988     )
    990 log.debug(&quot;Raising timeout error&quot;)
    991 raise APITimeoutError(request=request) from err


File ~\anaconda3\envs\multi_agent\Lib\site-packages\openai\_base_client.py:1074, in SyncAPIClient._retry_request(self, options, cast_to, remaining_retries, response_headers, stream, stream_cls)
   1070 # In a synchronous context we are blocking the entire thread. Up to the library user to run the client in a
   1071 # different thread if necessary.
   1072 time.sleep(timeout)
-&gt; 1074 return self._request(
   1075     options=options,
   1076     cast_to=cast_to,
   1077     remaining_retries=remaining,
   1078     stream=stream,
   1079     stream_cls=stream_cls,
   1080 )


File ~\anaconda3\envs\multi_agent\Lib\site-packages\openai\_base_client.py:991, in SyncAPIClient._request(self, cast_to, options, remaining_retries, stream, stream_cls)
    981         return self._retry_request(
    982             input_options,
    983             cast_to,
   (...)
    987             response_headers=None,
    988         )
    990     log.debug(&quot;Raising timeout error&quot;)
--&gt; 991     raise APITimeoutError(request=request) from err
    992 except Exception as err:
    993     log.debug(&quot;Encountered Exception&quot;, exc_info=True)


APITimeoutError: Request timed out.
```

```
print(completion.choices[0].message.content)
```

```
你好！有什么我可以帮你的吗？
```

如果能正常的接收到gpt模型的回复，则说明环境运行正常。接下来，我们看如何将gpt模型通过Langchain接入并进行交互。

# 2. LangChain的整体架构及接入GPT模型

作为一名开发人员，当希望利用人工智能的力量来创建直观且智能的应用程序，应该从哪里开始呢？

生成模型（例如 GPT-4）能够根据训练过的数据生成新内容，这包括生成文本、图像、音乐，甚至复杂的解决问题的步骤。这些模型学习输入数据中的模式和结构，并使用该知识生成新的、连贯的且与上下文相关的输出。这一类模型我们就称之为大家耳熟能详的大模型（Large Language Model ）。而大模型是当前现代人工智能应用的支柱，从聊天机器人和虚拟助手到自动化内容创建和推荐系统。但现实存在的问题是：将这些模型集成到应用程序中可能既复杂又耗时。

LangChain 是一个旨在简化大语言模型集成的框架（LLMs ）到应用程序中。它提供了一个高级接口，可以抽象出与使用这些强大模型相关的大部分复杂性。通过LangChain，开发人员可以快速构建、测试和部署利用LangChain功能的应用程序LLMs而不被潜在的复杂性所困扰。

LangChain Github 开源地址：https://github.com/langchain-ai/langchain

LangChain 官方文档：https://python.langchain.com/v0.2/docs/introduction/

![](img/5dc9d778b90246e7c09f6a2aecba3d5d.png)

LangChain的模块化架构是其主要特点，也正是因为这种模块化的设计，允许开发者根据需要插入不同的组件。这种灵活性使得尝试各种模型和配置变得容易。而从本质上分析，LangChain的开发者从大模型本身出发的策略，通过在实践过程中对大模型能力的深入理解及其在不同场景下的涌现潜力，使用模块化的方式进行高级抽象，设计出统一接口以适配各种大模型。到目前为止，LangChain抽象出最重要的核心模块如下：

1.  Model I/O ：标准化各个大模型的输入和输出，包含输入模版，模型本身和格式化输出；

1.  Retrieval ：检索外部数据，然后在执行生成步骤时将其传递到 LLM，包括文档加载、切割、Embedding等；

1.  Chains ：链条，LangChain框架中最重要的模块，链接多个模块协同构建应用，是实际运作很多功能的高级抽象；

1.  Memory ： 记忆模块，以各种方式构建历史信息，维护有关实体及其关系的信息；

1.  Agents ： 目前最热门的Agents开发实践，未来能够真正实现通用人工智能的落地方案；

1.  Callbacks ：回调系统，允许连接到 LLM 应用程序的各个阶段。用于日志记录、监控、流传输和其他任务；

其使用起来也比较简单，各个模块的接入示例在官方Docs中均有基本的介绍：https://python.langchain.com/v0.2/docs/introduction/

![](img/17e33dc78860854a2f44189c2c26f8e9.png)

LangChain的Model I/O模块提供了标准的、可扩展的接口实现与大语言模型的外部集成。所谓的Model I/O，包括模型输入（Prompts）、模型输出（OutPuts）和模型本身（Models），简单理解就是通过该模块，我们可以快速与某个大模型进行对话交互，整个内部逻辑就相当于我们最熟悉的这个过程：输入Prompt，得到大模型针对该Prompt的推理结果。如下示例为OpenAI的 GPT 系列模型的API 调用规范：

```
completion = client.chat.completions.create(
    model=&quot;gpt-4o&quot;,
    messages=[
    {&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;你是一位乐于助人的智能助手&quot;},
    {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;你好&quot;}
  ]
)
```

在这种规范下，对于不同的大模型，我们可以使用相同的规范来进行接入并完成模型的对话交互。比如OpenAI，我们可以在官方Doc中找到对应的接入示例：https://python.langchain.com/v0.2/docs/integrations/chat/openai/

![](img/e7972a1a24c4565dc4c499005dddc97a.png)

**LangChain可以使用pip 或者 conda直接安装，适用于仅使用的场景，即不需要了解其源码构建过程。**这种安装方法十分简洁明了，只需执行一条命令，就可以在当前的虚拟环境中迅速完成LangChain的安装。同时，还要安装 Langchain-openai这个第三方集成的依赖包。执行操作如下：

```
# ! pip install langchain langchain-openai
```

**注：如果是在Jupyter lab操作，需要重启**当前的Jupyter Lab使配置生效。

重启完当前的Jupyter Lab后，验证LangChain的安装情况，执行命令如下：

```
import langchain

print(langchain.__version__)
```

```
0.2.13
```

如果能正常输出LangChain的版本，说明在当前环境下的安装成功。接下来执行调用。首先需要实例化gpt模型对象并生成聊天:

```
from langchain_openai import ChatOpenAI

llm = ChatOpenAI(
    model=&quot;gpt-4o&quot;,
    # api_key=&quot;...&quot;,  # if you prefer to pass api key in directly instaed of using env vars
    # other params...
)
```

```
messages = [
    (&quot;system&quot;, &quot;你是把英语翻译成中文的得力助手。请翻译用户输入的句子。&quot;),
    (&quot;human&quot;, &quot;I love programming.&quot;),
]
ai_msg = llm.invoke(messages)

ai_msg
```

```
print(ai_msg.content)
```

```
我喜欢编程。
```

扩展知识：我们前面提到了，LangChain项目的定位为一个应用开发框架，所以如果仅仅集成到这样的对话交互程度，那相较于直接使用OpenAI的API调用又有何异呢？所以在这个模块中，LangChain同样抽象出一个chain，用于进一步简化和增强交互流程。在LangChain的Model I/O模块设计中，包含三个核心部分： Prompt Template（对应下图中的Format部分）， Model（对应下图中的Predict部分） 和Output Parser（对应下图中的Parse部分）。

+   **Format：即指代Prompts Temp**late，通过模板化来管理大模型的输入；

+   Predict：即指代Models，使用通用接口调用不同的大语言模型；

+   **Parse：即指代Output部分，用来从模型的推理中提取信**息，并按照预先设定好的模版来规范化输出。

![](img/2b7ebc869e487e17c1fe6e2f8f61e957.png)

感兴趣的小伙伴可以进一步了解。

# 3. 使用LangChain创建工具库

在定义工具之前，我们首先需要明确AI智能体的用户角色。如果我们要构建一个能自主规划并执行的智能体，其能够调用的工具是需要与任务强相关。也就是说不同的智能体根据其任务需求，将需要不同的工具。例如，在上一节课中我们讨论的旅游规划智能体，若要使其能自主完成旅游规划任务，我们必须为其配置如酒店预订和机票订购等相关的工具。

所以，这里我们定义了一个角色：研究人员代理。他的任务是能过根据指定主题，自主地在在网络上搜索给定主题并收集所有信息。那这个代理，其需要的就是一个能够访问到实时网络进行信息搜索的工具，这里我们要借助LangChain应用开发框架来继续接入。 langChan是集成了一系列的外部工具可以直接使用的，比如bing、duckduckgo等多种实时搜索工具：https://python.langchain.com/v0.1/docs/integrations/tools/

![](img/0ec125fc2dac54a0b5c1895f71cb0df0.png)

大家可以根据自己的需要选择一种或多种进行尝试。我们这里选择duckduckgo工具。对于duckduckgo Search，其接入文档如下所示：https://python.langchain.com/v0.2/docs/integrations/tools/ddg/

![](img/e5dfd7f21d57e4d834156713d8cc404d.png)

```
# ! pip install duckduckgo-search langchain-community
```

调用测试：

```
from langchain_community.tools import DuckDuckGoSearchRun

search = DuckDuckGoSearchRun()

search.invoke(&quot;2024 巴黎奥运会&quot;)
```

```
&#x27;2024年巴黎奥运会于2024年7月26日-8月11日在法国巴黎举行，央视网直播精彩奥运赛事、直击巴黎前方，推出大型原创节目，打造奥运专业数据库，提供权威、快速、独具特色的奥运报道。 &quot;2024年巴黎奥运会是对运动员们，对体育，最美好的一次庆典。&quot; &quot;这是首次完全按照我们的奥林匹克议程改革实施的奥运会：更年轻、更都市、更包容、更可持续。也是首次实现完全性别平等的奥运会。 2024年巴黎奥运会，正式闭幕!. 终于，莱昂·马尔尚带着从杜乐丽花园传来的奥林匹克圣火抵达法兰西体育场。. 留下来的观众们热情迎接他，体育场内齐声高喊&quot;莱昂，莱昂&quot;。. 早些时候陪同托尼·埃斯坦盖和国际奥委会主席托马斯·巴赫发表讲话的六位运动员 ... 央视网消息：北京时间2024年8月12日凌晨01:30，第33届法国巴黎奥运会闭幕式将正式举行。闭幕式总导演与开幕式总导演一样，仍由托马斯·乔利担任，主题为《记录》（Records）。与开幕式流动的盛宴不同，闭幕式的整个过程将在法兰西体育场内回归传统呈现方式。 2024年8月12日 2024年8月12日. 一场星光熠熠、精彩绝伦的现场表演为巴黎奥运会画上了圆满句号。除了来自世界各地的数千名运动员外，这场表演还 ...&#x27;
```

能够正常返回结果，则说明调用正常。即可直接作为一个工具赋予给我们前面定义的研究人员代理这个角色。

而截至到目前为止，我们已经构建了大模型的连接，且有了工具，即DuckDuckGoSearch，接下来我们要考虑，应该如何让它们形成一个工作流，同时具备 规划 + 执行 的能力。这里，我们就要用到 Agent 构建工具 - CrewAI。

# 4. 使用CrewAI创建AI Agent

CrewAI是构建代理应用程序的新兴框架之一。 CrewAI 提供了一个框架，用于构建智能体来处理复杂的任务。 它的目的是让人工智能代理能够承担角色、共享目标并在一个持续运行的单元中执行。而从架构上看，Crewai 是使用LangChain构建的，所以可以使用LangChain中的任何现有工具，当然也可以编写自定义工具。（关于CrewAI的架构及核心逻辑，我们将在明天的直播中详细的介绍）

CrewAI 官方文档：https://docs.crewai.com/ ， 当前阶段仅需要关注 Agents 和 Tasks 两个模块。

![](img/ace300dc60ba7275e393bbbc72d71328.png)

首先来看 CrewAI 的 Agents 定义。Agents作为一个 自治的代理单元，在构建时需要设定一些 Prompt 描述，并基于CrewAI的规范在指定的字段中进行传递。https://docs.crewai.com/core-concepts/Agents/

![](img/04413fdd69e45f649ed3656f14042a88.png)

如上所示，我们需要对定义的代理设置特定的role 、 goal并使用backstory设置上下文。这些实际上就像带有上下文的提示。而需要注意的是：提供正确的提示非常重要，其在实践中，往往需要多次微调Prompt才能获得最佳结果。

其设置方法也非常简单，本质上就是自然语言编程。如下所示，我们定义了研究员代理，并明确指示他的任务就是根据输入的主题，在网络上搜索相关的信息。同时，在这个过程中需要注意的是：一个AI Agent 角色代理可以理解成一个真实的人，所以我们当然在构建它的时候，需要传入一个大模型来充当这个'人'。

```
# ! pip install crewai
```

```
from langchain_openai import ChatOpenAI

llm = ChatOpenAI(
    model=&quot;gpt-4o&quot;,
    # api_key=&quot;...&quot;,  # if you prefer to pass api key in directly instaed of using env vars
    # other params...
)
```

```
from crewai import Agent


researcher = Agent(
    role = &quot;Internet Research&quot;,
    goal = f&quot;Perform research on the topic, and find and explore about topic &quot;,      # 对主题进行研究，发现和探索主题 
    verbose = True,
    llm=llm,
    backstory = &quot;&quot;&quot;You are an expert Internet Researcher
    Who knows how to search the internet for detailed content on topic 
    Include any code examples with documentation&quot;&quot;&quot;     # 你是一个互联网专家,知道如何在互联网上搜索有关主题的详细内容包括任何代码示例的文档
) 
```

同时在crewAI框架中，Task（任务）这一概念指的是由代理完成的具体任务。它们提供执行所需的所有详细信息，例如描述、负责的代理、所需的工具等，从而促进各种复杂的操作。

> CrewAI Task:https://docs.crewai.com/core-concepts/Tasks/

![](img/4783b045643085b7de1ec2dc63248ded.png)

所以，在这个阶段，我们要给它提供：1. 执行网络搜索的工具 2. 配置哪个代理将执行此任务。代码如下：

```
from langchain_community.tools import DuckDuckGoSearchRun
search_tool = DuckDuckGoSearchRun()
```

```
from crewai import Task
  
task_search = Task(
    description=&quot;&quot;&quot;Search for all the details about the  topic
                Your final answer MUST be a consolidated content that can be used for blogging
                This content should be well organized, and should be very easy to read&quot;&quot;&quot;,   # 搜索有关该主题的所有细节，你的最终答案必须是一
                                                                                             # 个可以用于博客的综合内容这些内容应该组织得很好，并且应该非常容易阅读，
    expected_output=&#x27;全面的1000字主题信息&#x27;,         
    tools=[search_tool],           
    agent=researcher)
```

而为了将任务和代理结合在一起，我们需要定义 Crew 对象，并且传递在 CrewAI 中工作的代理和任务。如下所示：

```
from crewai import Crew

crew = Crew(
    agents=[researcher],
    tasks=[task_search],
    verbose=True,
)
```

```
2024-08-14 14:42:33,612 - 29528 - __init__.py-__init__:531 - WARNING: Overriding of current TracerProvider is not allowed
```

```
result = crew.kickoff()
```

```
[1m[95m [2024-08-14 14:45:46][DEBUG]: == Working Agent: Internet Research[00m
[1m[95m [2024-08-14 14:45:46][INFO]: == Starting Task: Search for all the details about the  topic
                Your final answer MUST be a consolidated content that can be used for blogging
                This content should be well organized, and should be very easy to read[00m


[1m&gt; Entering new CrewAgentExecutor chain...[0m
[32;1m[1;3mI need to gather detailed information on a specific topic. To start, I need to define the topic I am researching.

Thought: Since the topic is not specified, I will first define a general topic. Let&#x27;s choose &quot;The Benefits of Remote Work.&quot;

Action: I will search for comprehensive information on &quot;The Benefits of Remote Work.&quot;

Action: duckduckgo_search
Action Input: {&quot;query&quot;: &quot;The Benefits of Remote Work&quot;}[0m[91m 

Action &#x27;I will search for comprehensive information on &quot;The Benefits of Remote Work.&quot;

Action: duckduckgo_search&#x27; don&#x27;t exist, these are the only available Actions:
 Tool Name: duckduckgo_search
Tool Description: A wrapper around DuckDuckGo Search. Useful for when you need to answer questions about current events. Input should be a search query.
Tool Arguments: {&#x27;query&#x27;: {&#x27;title&#x27;: &#x27;Query&#x27;, &#x27;description&#x27;: &#x27;search query to look up&#x27;, &#x27;type&#x27;: &#x27;string&#x27;}}
[00m
[32;1m[1;3mThought: I need to gather comprehensive information on &quot;The Benefits of Remote Work.&quot; To achieve this, I will use the DuckDuckGo search tool to collect detailed content on the topic.

Action: duckduckgo_search
Action Input: {&quot;query&quot;: &quot;The Benefits of Remote Work&quot;}[0m[95m 

Remote work slightly eases that conundrum, according to research using prepandemic data from economists at the University of Virginia and the University of Southern California. In fields like ... According to a recent State of Remote Work report from social media management platform Buffer, almost all of the 3,000 survey respondents who work remotely (98%) said they would not only work ... 9 benefits of working from home. If these advantages spark excitement or &quot;I could get used to that&quot; thoughts, WFH may be a good fit for you. But keep your circumstances in mind—remote work isn&#x27;t ideal for everyone. 1. You get greater flexibility in your schedule and your day-to-day life is easier to manage. Globally, remote workers save an average of 72 minutes per day on commuting time. In the US, employees save an average of 55 minutes per day. The way remote teams spend their extra time: 40% of that unused time goes toward work and 11% toward caregiving (a win-win for employees and employers). 3. More time to spend with loved ones. A primary benefit of remote work for employees is that the increased flexibility gives them a far better work-life balance than office-based work. Remote work could save employees up to 72 minutes a day, according to the National Bureau of Economic Research (NBER). We could all do a lot with an extra 72 minutes a day.
[00m
[32;1m[1;3m[0m[32;1m[1;3mThought: I now have enough information to provide a comprehensive and well-organized blog post on &quot;The Benefits of Remote Work.&quot;

Final Answer: 

---
**The Benefits of Remote Work**

Remote work, also known as telecommuting, has transformed the traditional workplace. With the advent of technology and a global shift in work culture, remote work has become a viable option for many employees and employers. This blog post delves into the myriad benefits that remote work offers to both individuals and organizations.

### 1. Improved Work-Life Balance

One of the most significant advantages of remote work is the improved work-life balance it offers. Employees have the flexibility to create a schedule that suits their personal and professional lives. This flexibility allows them to manage household responsibilities, childcare, and personal errands without the constraints of a rigid office schedule. According to a report from the National Bureau of Economic Research, remote work could save employees up to 72 minutes a day, which can be spent on family time, hobbies, or self-care.

### 2. Increased Productivity

Contrary to the myth that remote work leads to laziness, studies have shown that remote workers are often more productive than their office-based counterparts. Without the distractions of office chatter, impromptu meetings, and long commutes, employees can focus better on their tasks. Many remote workers have reported that they can complete their work more efficiently and with higher quality. The flexibility to work during their most productive hours also contributes to increased output.

### 3. Cost Savings

Both employees and employers can reap financial benefits from remote work. Employees save on commuting costs, work attire, and daily expenses such as lunch and coffee. Employers, on the other hand, save on overhead costs, including office space, utilities, and office supplies. These savings can be substantial and can be redirected towards other business needs or employee benefits.

### 4. Access to a Global Talent Pool

Remote work allows companies to hire talent from around the world, breaking geographical barriers. This access to a global talent pool means that organizations can find the best candidates for the job, regardless of their location. This diversity can lead to innovative ideas and perspectives, driving the company’s growth and success.

### 5. Environmental Benefits

Remote work has a positive impact on the environment by reducing the carbon footprint associated with daily commuting. Fewer cars on the road mean lower greenhouse gas emissions and less air pollution. Additionally, with reduced need for office space, there is less energy consumption in terms of heating, cooling, and lighting commercial buildings.

### 6. Enhanced Employee Well-being

The flexibility and autonomy that come with remote work contribute to improved mental and physical health. Employees can tailor their work environment to their comfort, take breaks when needed, and avoid the stress of commuting. This autonomy leads to higher job satisfaction and lower burnout rates. A happier workforce is more engaged and committed to their roles.

### 7. Continuity of Operations

Remote work ensures business continuity in times of crisis. Whether it’s a natural disaster, a pandemic, or any other unforeseen event, remote work allows businesses to continue operations without significant disruptions. This resilience is crucial for maintaining productivity and meeting business goals.

### 8. Customizable Work Environment

Employees working remotely have the freedom to set up their workspace according to their preferences. Whether it’s a quiet room, a bustling café, or a co-working space, individuals can choose a setting that enhances their productivity and comfort. This customization can lead to a more enjoyable and effective work experience.

### 9. Reduced Absenteeism

Remote work can lead to reduced absenteeism as employees can work from home even if they are mildly ill or have minor injuries. This flexibility ensures that work continues without the need for taking sick days unless absolutely necessary. It also reduces the spread of contagious illnesses within the workplace.

### Conclusion

The benefits of remote work are numerous and impactful, making it an attractive option for both employees and employers. From improved work-life balance and increased productivity to cost savings and environmental benefits, remote work is reshaping the future of work. As technology continues to advance and work cultures evolve, remote work is likely to become an integral part of the modern workplace.

---

This blog post provides a comprehensive overview of the benefits of remote work, making it informative and engaging for readers interested in understanding the advantages of this work model.[0m

[1m&gt; Finished chain.[0m
[1m[92m [2024-08-14 14:46:30][DEBUG]: == [Internet Research] Task output: ---
**The Benefits of Remote Work**

Remote work, also known as telecommuting, has transformed the traditional workplace. With the advent of technology and a global shift in work culture, remote work has become a viable option for many employees and employers. This blog post delves into the myriad benefits that remote work offers to both individuals and organizations.

### 1. Improved Work-Life Balance

One of the most significant advantages of remote work is the improved work-life balance it offers. Employees have the flexibility to create a schedule that suits their personal and professional lives. This flexibility allows them to manage household responsibilities, childcare, and personal errands without the constraints of a rigid office schedule. According to a report from the National Bureau of Economic Research, remote work could save employees up to 72 minutes a day, which can be spent on family time, hobbies, or self-care.

### 2. Increased Productivity

Contrary to the myth that remote work leads to laziness, studies have shown that remote workers are often more productive than their office-based counterparts. Without the distractions of office chatter, impromptu meetings, and long commutes, employees can focus better on their tasks. Many remote workers have reported that they can complete their work more efficiently and with higher quality. The flexibility to work during their most productive hours also contributes to increased output.

### 3. Cost Savings

Both employees and employers can reap financial benefits from remote work. Employees save on commuting costs, work attire, and daily expenses such as lunch and coffee. Employers, on the other hand, save on overhead costs, including office space, utilities, and office supplies. These savings can be substantial and can be redirected towards other business needs or employee benefits.

### 4. Access to a Global Talent Pool

Remote work allows companies to hire talent from around the world, breaking geographical barriers. This access to a global talent pool means that organizations can find the best candidates for the job, regardless of their location. This diversity can lead to innovative ideas and perspectives, driving the company’s growth and success.

### 5. Environmental Benefits

Remote work has a positive impact on the environment by reducing the carbon footprint associated with daily commuting. Fewer cars on the road mean lower greenhouse gas emissions and less air pollution. Additionally, with reduced need for office space, there is less energy consumption in terms of heating, cooling, and lighting commercial buildings.

### 6. Enhanced Employee Well-being

The flexibility and autonomy that come with remote work contribute to improved mental and physical health. Employees can tailor their work environment to their comfort, take breaks when needed, and avoid the stress of commuting. This autonomy leads to higher job satisfaction and lower burnout rates. A happier workforce is more engaged and committed to their roles.

### 7. Continuity of Operations

Remote work ensures business continuity in times of crisis. Whether it’s a natural disaster, a pandemic, or any other unforeseen event, remote work allows businesses to continue operations without significant disruptions. This resilience is crucial for maintaining productivity and meeting business goals.

### 8. Customizable Work Environment

Employees working remotely have the freedom to set up their workspace according to their preferences. Whether it’s a quiet room, a bustling café, or a co-working space, individuals can choose a setting that enhances their productivity and comfort. This customization can lead to a more enjoyable and effective work experience.

### 9. Reduced Absenteeism

Remote work can lead to reduced absenteeism as employees can work from home even if they are mildly ill or have minor injuries. This flexibility ensures that work continues without the need for taking sick days unless absolutely necessary. It also reduces the spread of contagious illnesses within the workplace.

### Conclusion

The benefits of remote work are numerous and impactful, making it an attractive option for both employees and employers. From improved work-life balance and increased productivity to cost savings and environmental benefits, remote work is reshaping the future of work. As technology continues to advance and work cultures evolve, remote work is likely to become an integral part of the modern workplace.

---

This blog post provides a comprehensive overview of the benefits of remote work, making it informative and engaging for readers interested in understanding the advantages of this work model.

[00m
```

```
result.raw
```

```
&#x27;---\n**The Benefits of Remote Work**\n\nRemote work, also known as telecommuting, has transformed the traditional workplace. With the advent of technology and a global shift in work culture, remote work has become a viable option for many employees and employers. This blog post delves into the myriad benefits that remote work offers to both individuals and organizations.\n\n### 1. Improved Work-Life Balance\n\nOne of the most significant advantages of remote work is the improved work-life balance it offers. Employees have the flexibility to create a schedule that suits their personal and professional lives. This flexibility allows them to manage household responsibilities, childcare, and personal errands without the constraints of a rigid office schedule. According to a report from the National Bureau of Economic Research, remote work could save employees up to 72 minutes a day, which can be spent on family time, hobbies, or self-care.\n\n### 2. Increased Productivity\n\nContrary to the myth that remote work leads to laziness, studies have shown that remote workers are often more productive than their office-based counterparts. Without the distractions of office chatter, impromptu meetings, and long commutes, employees can focus better on their tasks. Many remote workers have reported that they can complete their work more efficiently and with higher quality. The flexibility to work during their most productive hours also contributes to increased output.\n\n### 3. Cost Savings\n\nBoth employees and employers can reap financial benefits from remote work. Employees save on commuting costs, work attire, and daily expenses such as lunch and coffee. Employers, on the other hand, save on overhead costs, including office space, utilities, and office supplies. These savings can be substantial and can be redirected towards other business needs or employee benefits.\n\n### 4. Access to a Global Talent Pool\n\nRemote work allows companies to hire talent from around the world, breaking geographical barriers. This access to a global talent pool means that organizations can find the best candidates for the job, regardless of their location. This diversity can lead to innovative ideas and perspectives, driving the company’s growth and success.\n\n### 5. Environmental Benefits\n\nRemote work has a positive impact on the environment by reducing the carbon footprint associated with daily commuting. Fewer cars on the road mean lower greenhouse gas emissions and less air pollution. Additionally, with reduced need for office space, there is less energy consumption in terms of heating, cooling, and lighting commercial buildings.\n\n### 6. Enhanced Employee Well-being\n\nThe flexibility and autonomy that come with remote work contribute to improved mental and physical health. Employees can tailor their work environment to their comfort, take breaks when needed, and avoid the stress of commuting. This autonomy leads to higher job satisfaction and lower burnout rates. A happier workforce is more engaged and committed to their roles.\n\n### 7. Continuity of Operations\n\nRemote work ensures business continuity in times of crisis. Whether it’s a natural disaster, a pandemic, or any other unforeseen event, remote work allows businesses to continue operations without significant disruptions. This resilience is crucial for maintaining productivity and meeting business goals.\n\n### 8. Customizable Work Environment\n\nEmployees working remotely have the freedom to set up their workspace according to their preferences. Whether it’s a quiet room, a bustling café, or a co-working space, individuals can choose a setting that enhances their productivity and comfort. This customization can lead to a more enjoyable and effective work experience.\n\n### 9. Reduced Absenteeism\n\nRemote work can lead to reduced absenteeism as employees can work from home even if they are mildly ill or have minor injuries. This flexibility ensures that work continues without the need for taking sick days unless absolutely necessary. It also reduces the spread of contagious illnesses within the workplace.\n\n### Conclusion\n\nThe benefits of remote work are numerous and impactful, making it an attractive option for both employees and employers. From improved work-life balance and increased productivity to cost savings and environmental benefits, remote work is reshaping the future of work. As technology continues to advance and work cultures evolve, remote work is likely to become an integral part of the modern workplace.\n\n---\n\nThis blog post provides a comprehensive overview of the benefits of remote work, making it informative and engaging for readers interested in understanding the advantages of this work model.&#x27;
```

如上所示的三个步骤构建出一个完整的 AI Agent 角色。而其存在的问题在于，所有的参数都是写死的，我们希望能够传递不同的主题让该智能体搜索相关的内容。

所以接下来的核心代码逻辑中，我们定义了search_crew(topic)方法。我们将传递topic参数，让该研究员代理针对输入的主题进行网络信息搜集并完成汇总。代码如下：

```
from crewai import Agent, Task, Crew, Process

def search_crew(topic): 
    
    researcher = Agent(
        role = &quot;Internet Research&quot;,
        goal = f&quot;Perform research on the {topic}, and find and explore about {topic} &quot;,
        verbose = True,
        llm=llm,
        backstory = &quot;&quot;&quot;You are an expert Internet Researcher
        Who knows how to search the internet for detailed content on {topic}
        Include any code examples with documentation&quot;&quot;&quot;
    )
      
    task_search = Task(
        description=&quot;&quot;&quot;Search for all the details about the  {topic}
                    Your final answer MUST be a consolidated content that can be used for blogging
                    This content should be well organized, and should be very easy to read&quot;&quot;&quot;,
        expected_output=&#x27;A comprehensive 10000 words information about {topic}&#x27;,
        tools=[search_tool],           
        agent=researcher)
    
    
    crew = Crew(
        agents=[researcher],
        tasks=[task_search],
        verbose=True 
    )
    
    result = crew.kickoff()
    return result
```

```
result = search_crew(topic=&quot;2024年巴黎奥运会&quot;)
```

```
2024-08-14 14:51:53,709 - 29528 - __init__.py-__init__:531 - WARNING: Overriding of current TracerProvider is not allowed


[1m[95m [2024-08-14 14:51:53][DEBUG]: == Working Agent: Internet Research[00m
[1m[95m [2024-08-14 14:51:53][INFO]: == Starting Task: Search for all the details about the  {topic}
                    Your final answer MUST be a consolidated content that can be used for blogging
                    This content should be well organized, and should be very easy to read[00m


[1m&gt; Entering new CrewAgentExecutor chain...[0m
[32;1m[1;3mI need to gather detailed information about the 2024年巴黎奥运会 (2024 Paris Olympics). To do this, I will perform a search using DuckDuckGo to find comprehensive and current information on the topic.

Action: duckduckgo_search
Action Input: {&quot;query&quot;: &quot;2024年巴黎奥运会 详细信息&quot;}[0m[95m 

8月10日，2024年巴黎奥运会进入倒数第二个比赛日的争夺，今日奥运赛场共产生了39枚金牌。 中国体育代表团今日斩获6金1铜，奖牌总数来到90枚，位列奖牌榜第一。. 奥运会第15个比赛日，中国队拿下6枚金牌，创下单日之最，并创造了多项新的历史，夏奥运会历史总奖牌数量超越300枚。 2024年巴黎奥运奖牌由Chaumet进行设计、巴黎造币厂进行铸造。 正面采用宝石镶嵌工艺嵌入一块过去翻修埃菲尔铁塔时保存的碎零件铁片。 奖牌将负盛名的法国文化遗产与奥运融合，象征世界各国运动员享受荣耀，得以带回一部分的巴黎。 铁片周围如光芒放射状的纹理，呼应巴黎&quot;光之城&quot;(La Ville ... 2024年巴黎奥运会闭幕式将于8月11日巴黎当地时间21:00（北京时间12日凌晨3:00）在法兰西体育场举行。. 举重冠军 李发彬和 女子曲棍球亚军 欧紫霞 将会在闭幕式上撑起中国代表团的国旗。. 李发彬在2024年巴黎奥运会上成功卫冕了男子举重61公斤级金牌，而欧紫霞 ... 新华社巴黎7月27日电（记者丁文娴、王恒志、张泽伟）又一次，中国代表团奥运首金诞生在射击项目!巴黎奥运会开幕后首个比赛日，黄雨婷和盛李豪携手站上10米气步枪混合团体比赛的最高领奖台，将本届奥运会首金收入囊中。 巴黎奥运会大幕今启. 第33届夏季奥运会开幕式将于当地时间26日19点30分 (北京时间27日凌晨1点30分)在巴黎塞纳河上举行，2024巴黎奥运会正式拉开大幕。. 巴黎奥运会和残奥会口号都是&quot;OUVRONS GRAND LES JEUX&quot;，中文为&quot;奥运更开放&quot;。. 开幕式创新性地在塞纳河上 ...
[00m
[32;1m[1;3m[0m[32;1m[1;3mThought: I need to gather detailed information about the 2024年巴黎奥运会 (2024 Paris Olympics) from various sources to compile a comprehensive blog post.

Action: duckduckgo_search
Action Input: {&quot;query&quot;: &quot;2024年巴黎奥运会 详细信息&quot;}
[0m[95m 

I tried reusing the same input, I must stop using this action input. I&#x27;ll try something else instead.


[00m
[32;1m[1;3mThought: I need to gather detailed information about the 2024年巴黎奥运会 (2024 Paris Olympics) from different angles, including events, schedules, venues, and key highlights. I will perform another search using DuckDuckGo with a revised keyword.

Action: duckduckgo_search
Action Input: {&quot;query&quot;: &quot;2024 Paris Olympics details&quot;}
[0m[95m 

Paris 2024 will host the XXXIII Olympic Summer Games, 26 July to 11 August. Follow all the latest news as France prepares for the world&#x27;s biggest international sporting event. The Games concept features iconic monuments and cultural and sports facilities primarily located on the banks of the Seine. Stars of sport, screen and music came together at the Paris 2024 closing ceremony to bring the 33rd Olympic Games to an end. Actor Tom Cruise descended from the roof of Stade de France to collect ... The 2024 Paris Olympics ended on Sunday with an exciting closing ceremony. There were plenty of jaw-dropping performances throughout the event. The ceremony ended with US performers in California ... Paris 2024 will herald a &quot;new era&quot; of Games that will set a precedent for future editions. Find out here how the IOC supports the Organising Committee and get access to key documents like the Host City Contract, qualification information, anti-doping and medical rules as well as media guidelines and News Access Rules. The 2024 Paris Olympics featured 329 medal events, second most in the history of the Games (339 at the 2020 Tokyo Summer Olympics). Of the 329 events, 28 were in &quot;core&quot; sports, or sports that were ...
[00m
[32;1m[1;3mThought: I will search using a more specific query to gather detailed and varied information about the 2024年巴黎奥运会 (2024 Paris Olympics), including events, schedules, venues, and significant highlights.

Action: duckduckgo_search
Action Input: {&quot;query&quot;: &quot;2024 Paris Olympics schedule events venues highlights&quot;}
[0m[95m 

Looking for information on when your favourite sport or preferred event is on at the Olympic Games Paris 2024?We&#x27;ve got you covered. The Organising Committee first unveiled the sport-by-sport competition session schedule last July to mark two years to go to the Games.. One notable change from previous Olympic Games is the addition of an extra day to the swimming schedule, meaning there will ... Breaking makes its Olympic debut at Paris 2024, with B-Boys and B-Girls events due to take place. There will also be new events in athletics, boxing, canoe slalom, sailing, shooting, and sport climbing. They are: Marathon race walk mixed relay (athletics) - replacing the men&#x27;s 50km race walk event. The 2024 Paris Olympic Opening Ceremony on July 26 is now 100 days away. The Paris Games, which begin with non-medal competition on July 24, will have 329 medal events over 16 days from July 27-Aug. 11. Here&#x27;s the complete schedule. Here&#x27;s a look at the highlights for every day of the Games, keeping in mind that many athletes must still ... Find out the day-by-day schedule from the Paris Olympics 2024 for each sport and event including athletics, swimming, football, cycling and more Paris 2024 highlights, Saturday 27 July 2024 (Day 1) The first set of medals of the Games will be awarded in shooting, as has been the case at every summer Olympic Games since Munich 1972; in Paris it will be awarded in the mixed team 10m air rifle event at the Châteauroux Shooting Centre.There are a total of 14 medal events on the first full day of competition, including the conclusion of ...
[00m
[32;1m[1;3mThought: I have gathered substantial information about the 2024 Paris Olympics, including details on the schedule, events, venues, and highlights. I can now consolidate this information into a comprehensive blog post.

Final Answer: my best complete final answer to the task.

---

# Comprehensive Guide to the 2024 Paris Olympics

## Introduction

The 2024 Paris Olympics, officially known as the Games of the XXXIII Olympiad, will be a momentous event bringing together athletes from around the globe to compete in a variety of sports. The Paris Games are scheduled to take place from July 26 to August 11, 2024. This blog post aims to provide a detailed overview of the event, including the schedule, venues, key highlights, and new sports making their debut.

## Historical Context

Paris has a long history with the Olympic Games, having previously hosted the event in 1900 and 1924. The 2024 Games mark the centenary of the last Paris-hosted Olympics, adding a layer of historical significance to the event.

## The Vision of Paris 2024

The Paris 2024 Olympics aim to be a celebration of sport, culture, and sustainability. The Organising Committee has focused on creating an inclusive, innovative, and environmentally friendly event. The official slogan for the Games is &quot;Ouvrons Grand Les Jeux,&quot; which translates to &quot;Games Wide Open.&quot;

## Schedule and Events

### Opening Ceremony

The Opening Ceremony will take place on July 26, 2024, along the banks of the Seine River. Unlike traditional stadium ceremonies, this unique setting aims to integrate the city&#x27;s iconic landmarks into the festivities.

### Daily Schedule

The Games will feature 329 medal events across 32 sports. Below is a summary of key events and highlights for each day:

- **July 27, 2024 (Day 1):** The first medals will be awarded in shooting, with the mixed team 10m air rifle event at the Châteauroux Shooting Centre. Other events include judo, cycling, and swimming.
- **July 28, 2024 (Day 2):** Swimming events commence, with medals awarded in men&#x27;s and women&#x27;s 400m freestyle. Gymnastics and fencing events also begin.
- **July 29, 2024 (Day 3):** Track cycling and weightlifting events start, with key competitions in judo and tennis.
- **July 30, 2024 (Day 4):** The athletics events kick off, including the men&#x27;s 100m heats. Swimming continues with the 200m freestyle.
- **July 31, 2024 (Day 5):** Basketball, volleyball, and handball competitions intensify. Medals in gymnastics and swimming are awarded.
- **August 1, 2024 (Day 6):** Equestrian events begin, and the athletics 100m finals take place. Beach volleyball and rowing events continue.
- **August 2, 2024 (Day 7):** The marathon swimming event takes place, along with key matches in football and rugby sevens.
- **August 3, 2024 (Day 8):** The first week concludes with finals in swimming, athletics, and weightlifting.

### Closing Ceremony

The Closing Ceremony will be held on August 11, 2024, at the Stade de France. The ceremony will feature performances celebrating global cultures and the achievements of the athletes.

## Venues

The Paris 2024 Olympics will be held across multiple iconic venues in and around the city. Key locations include:

- **Stade de France:** The primary stadium for athletics and the opening and closing ceremonies.
- **Châteauroux Shooting Centre:** The venue for shooting events.
- **Grand Palais:** Fencing and taekwondo competitions.
- **Paris La Défense Arena:** Gymnastics events.
- **Versailles:** Equestrian events.
- **Surfing in Tahiti:** The surfing competitions will take place on the island of Tahiti, leveraging its world-renowned waves.

## New Sports and Events

The 2024 Paris Olympics will introduce several new sports and events, aiming to attract a younger audience and reflect modern athletic trends. These include:

- **Breaking (Breakdancing):** Making its Olympic debut with B-Boys and B-Girls competing in dynamic dance battles.
- **Sport Climbing:** Building on its success in Tokyo 2020, sport climbing will feature speed, bouldering, and lead events.
- **Skateboarding:** Continuing from Tokyo 2020, with street and park events.
- **Surfing:** Held in Tahiti, this event promises thrilling competition on some of the world&#x27;s best waves.
- **Mixed Team Events:** New mixed-gender team events in athletics, archery, judo, and swimming.

## Sustainability and Innovation

Paris 2024 is committed to delivering an environmentally sustainable Games. Key initiatives include:

- **Carbon Neutrality:** Efforts to offset carbon emissions through renewable energy and sustainable practices.
- **Recycled Materials:** Use of recycled materials for medals and infrastructure.
- **Public Transport:** Encouraging the use of public transport to reduce the carbon footprint of the Games.

## Conclusion

The 2024 Paris Olympics promise to be a groundbreaking and memorable event, combining historic Parisian landmarks with cutting-edge sports and sustainability initiatives. Whether you&#x27;re a sports enthusiast or a casual viewer, the Paris Games offer something for everyone. Stay tuned for more updates as we count down to this spectacular celebration of global athleticism.

---

This comprehensive guide provides a detailed overview of the 2024 Paris Olympics, making it easy for readers to understand the schedule, events, venues, and key highlights.[0m

[1m&gt; Finished chain.[0m
[1m[92m [2024-08-14 14:52:45][DEBUG]: == [Internet Research] Task output: my best complete final answer to the task.

---

# Comprehensive Guide to the 2024 Paris Olympics

## Introduction

The 2024 Paris Olympics, officially known as the Games of the XXXIII Olympiad, will be a momentous event bringing together athletes from around the globe to compete in a variety of sports. The Paris Games are scheduled to take place from July 26 to August 11, 2024. This blog post aims to provide a detailed overview of the event, including the schedule, venues, key highlights, and new sports making their debut.

## Historical Context

Paris has a long history with the Olympic Games, having previously hosted the event in 1900 and 1924. The 2024 Games mark the centenary of the last Paris-hosted Olympics, adding a layer of historical significance to the event.

## The Vision of Paris 2024

The Paris 2024 Olympics aim to be a celebration of sport, culture, and sustainability. The Organising Committee has focused on creating an inclusive, innovative, and environmentally friendly event. The official slogan for the Games is &quot;Ouvrons Grand Les Jeux,&quot; which translates to &quot;Games Wide Open.&quot;

## Schedule and Events

### Opening Ceremony

The Opening Ceremony will take place on July 26, 2024, along the banks of the Seine River. Unlike traditional stadium ceremonies, this unique setting aims to integrate the city&#x27;s iconic landmarks into the festivities.

### Daily Schedule

The Games will feature 329 medal events across 32 sports. Below is a summary of key events and highlights for each day:

- **July 27, 2024 (Day 1):** The first medals will be awarded in shooting, with the mixed team 10m air rifle event at the Châteauroux Shooting Centre. Other events include judo, cycling, and swimming.
- **July 28, 2024 (Day 2):** Swimming events commence, with medals awarded in men&#x27;s and women&#x27;s 400m freestyle. Gymnastics and fencing events also begin.
- **July 29, 2024 (Day 3):** Track cycling and weightlifting events start, with key competitions in judo and tennis.
- **July 30, 2024 (Day 4):** The athletics events kick off, including the men&#x27;s 100m heats. Swimming continues with the 200m freestyle.
- **July 31, 2024 (Day 5):** Basketball, volleyball, and handball competitions intensify. Medals in gymnastics and swimming are awarded.
- **August 1, 2024 (Day 6):** Equestrian events begin, and the athletics 100m finals take place. Beach volleyball and rowing events continue.
- **August 2, 2024 (Day 7):** The marathon swimming event takes place, along with key matches in football and rugby sevens.
- **August 3, 2024 (Day 8):** The first week concludes with finals in swimming, athletics, and weightlifting.

### Closing Ceremony

The Closing Ceremony will be held on August 11, 2024, at the Stade de France. The ceremony will feature performances celebrating global cultures and the achievements of the athletes.

## Venues

The Paris 2024 Olympics will be held across multiple iconic venues in and around the city. Key locations include:

- **Stade de France:** The primary stadium for athletics and the opening and closing ceremonies.
- **Châteauroux Shooting Centre:** The venue for shooting events.
- **Grand Palais:** Fencing and taekwondo competitions.
- **Paris La Défense Arena:** Gymnastics events.
- **Versailles:** Equestrian events.
- **Surfing in Tahiti:** The surfing competitions will take place on the island of Tahiti, leveraging its world-renowned waves.

## New Sports and Events

The 2024 Paris Olympics will introduce several new sports and events, aiming to attract a younger audience and reflect modern athletic trends. These include:

- **Breaking (Breakdancing):** Making its Olympic debut with B-Boys and B-Girls competing in dynamic dance battles.
- **Sport Climbing:** Building on its success in Tokyo 2020, sport climbing will feature speed, bouldering, and lead events.
- **Skateboarding:** Continuing from Tokyo 2020, with street and park events.
- **Surfing:** Held in Tahiti, this event promises thrilling competition on some of the world&#x27;s best waves.
- **Mixed Team Events:** New mixed-gender team events in athletics, archery, judo, and swimming.

## Sustainability and Innovation

Paris 2024 is committed to delivering an environmentally sustainable Games. Key initiatives include:

- **Carbon Neutrality:** Efforts to offset carbon emissions through renewable energy and sustainable practices.
- **Recycled Materials:** Use of recycled materials for medals and infrastructure.
- **Public Transport:** Encouraging the use of public transport to reduce the carbon footprint of the Games.

## Conclusion

The 2024 Paris Olympics promise to be a groundbreaking and memorable event, combining historic Parisian landmarks with cutting-edge sports and sustainability initiatives. Whether you&#x27;re a sports enthusiast or a casual viewer, the Paris Games offer something for everyone. Stay tuned for more updates as we count down to this spectacular celebration of global athleticism.

---

This comprehensive guide provides a detailed overview of the 2024 Paris Olympics, making it easy for readers to understand the schedule, events, venues, and key highlights.

[00m
```

```
result
```

```
CrewOutput(raw=&#x27;my best complete final answer to the task.\n\n---\n\n# Comprehensive Guide to the 2024 Paris Olympics\n\n## Introduction\n\nThe 2024 Paris Olympics, officially known as the Games of the XXXIII Olympiad, will be a momentous event bringing together athletes from around the globe to compete in a variety of sports. The Paris Games are scheduled to take place from July 26 to August 11, 2024. This blog post aims to provide a detailed overview of the event, including the schedule, venues, key highlights, and new sports making their debut.\n\n## Historical Context\n\nParis has a long history with the Olympic Games, having previously hosted the event in 1900 and 1924. The 2024 Games mark the centenary of the last Paris-hosted Olympics, adding a layer of historical significance to the event.\n\n## The Vision of Paris 2024\n\nThe Paris 2024 Olympics aim to be a celebration of sport, culture, and sustainability. The Organising Committee has focused on creating an inclusive, innovative, and environmentally friendly event. The official slogan for the Games is &quot;Ouvrons Grand Les Jeux,&quot; which translates to &quot;Games Wide Open.&quot;\n\n## Schedule and Events\n\n### Opening Ceremony\n\nThe Opening Ceremony will take place on July 26, 2024, along the banks of the Seine River. Unlike traditional stadium ceremonies, this unique setting aims to integrate the city\&#x27;s iconic landmarks into the festivities.\n\n### Daily Schedule\n\nThe Games will feature 329 medal events across 32 sports. Below is a summary of key events and highlights for each day:\n\n- **July 27, 2024 (Day 1):** The first medals will be awarded in shooting, with the mixed team 10m air rifle event at the Châteauroux Shooting Centre. Other events include judo, cycling, and swimming.\n- **July 28, 2024 (Day 2):** Swimming events commence, with medals awarded in men\&#x27;s and women\&#x27;s 400m freestyle. Gymnastics and fencing events also begin.\n- **July 29, 2024 (Day 3):** Track cycling and weightlifting events start, with key competitions in judo and tennis.\n- **July 30, 2024 (Day 4):** The athletics events kick off, including the men\&#x27;s 100m heats. Swimming continues with the 200m freestyle.\n- **July 31, 2024 (Day 5):** Basketball, volleyball, and handball competitions intensify. Medals in gymnastics and swimming are awarded.\n- **August 1, 2024 (Day 6):** Equestrian events begin, and the athletics 100m finals take place. Beach volleyball and rowing events continue.\n- **August 2, 2024 (Day 7):** The marathon swimming event takes place, along with key matches in football and rugby sevens.\n- **August 3, 2024 (Day 8):** The first week concludes with finals in swimming, athletics, and weightlifting.\n\n### Closing Ceremony\n\nThe Closing Ceremony will be held on August 11, 2024, at the Stade de France. The ceremony will feature performances celebrating global cultures and the achievements of the athletes.\n\n## Venues\n\nThe Paris 2024 Olympics will be held across multiple iconic venues in and around the city. Key locations include:\n\n- **Stade de France:** The primary stadium for athletics and the opening and closing ceremonies.\n- **Châteauroux Shooting Centre:** The venue for shooting events.\n- **Grand Palais:** Fencing and taekwondo competitions.\n- **Paris La Défense Arena:** Gymnastics events.\n- **Versailles:** Equestrian events.\n- **Surfing in Tahiti:** The surfing competitions will take place on the island of Tahiti, leveraging its world-renowned waves.\n\n## New Sports and Events\n\nThe 2024 Paris Olympics will introduce several new sports and events, aiming to attract a younger audience and reflect modern athletic trends. These include:\n\n- **Breaking (Breakdancing):** Making its Olympic debut with B-Boys and B-Girls competing in dynamic dance battles.\n- **Sport Climbing:** Building on its success in Tokyo 2020, sport climbing will feature speed, bouldering, and lead events.\n- **Skateboarding:** Continuing from Tokyo 2020, with street and park events.\n- **Surfing:** Held in Tahiti, this event promises thrilling competition on some of the world\&#x27;s best waves.\n- **Mixed Team Events:** New mixed-gender team events in athletics, archery, judo, and swimming.\n\n## Sustainability and Innovation\n\nParis 2024 is committed to delivering an environmentally sustainable Games. Key initiatives include:\n\n- **Carbon Neutrality:** Efforts to offset carbon emissions through renewable energy and sustainable practices.\n- **Recycled Materials:** Use of recycled materials for medals and infrastructure.\n- **Public Transport:** Encouraging the use of public transport to reduce the carbon footprint of the Games.\n\n## Conclusion\n\nThe 2024 Paris Olympics promise to be a groundbreaking and memorable event, combining historic Parisian landmarks with cutting-edge sports and sustainability initiatives. Whether you\&#x27;re a sports enthusiast or a casual viewer, the Paris Games offer something for everyone. Stay tuned for more updates as we count down to this spectacular celebration of global athleticism.\n\n---\n\nThis comprehensive guide provides a detailed overview of the 2024 Paris Olympics, making it easy for readers to understand the schedule, events, venues, and key highlights.&#x27;, pydantic=None, json_dict=None, tasks_output=[TaskOutput(description=&#x27;Search for all the details about the  {topic}\n                    Your final answer MUST be a consolidated content that can be used for blogging\n                    This content should be well organized, and should be very easy to read&#x27;, summary=&#x27;Search for all the details about the  {topic}\n ...&#x27;, raw=&#x27;my best complete final answer to the task.\n\n---\n\n# Comprehensive Guide to the 2024 Paris Olympics\n\n## Introduction\n\nThe 2024 Paris Olympics, officially known as the Games of the XXXIII Olympiad, will be a momentous event bringing together athletes from around the globe to compete in a variety of sports. The Paris Games are scheduled to take place from July 26 to August 11, 2024. This blog post aims to provide a detailed overview of the event, including the schedule, venues, key highlights, and new sports making their debut.\n\n## Historical Context\n\nParis has a long history with the Olympic Games, having previously hosted the event in 1900 and 1924. The 2024 Games mark the centenary of the last Paris-hosted Olympics, adding a layer of historical significance to the event.\n\n## The Vision of Paris 2024\n\nThe Paris 2024 Olympics aim to be a celebration of sport, culture, and sustainability. The Organising Committee has focused on creating an inclusive, innovative, and environmentally friendly event. The official slogan for the Games is &quot;Ouvrons Grand Les Jeux,&quot; which translates to &quot;Games Wide Open.&quot;\n\n## Schedule and Events\n\n### Opening Ceremony\n\nThe Opening Ceremony will take place on July 26, 2024, along the banks of the Seine River. Unlike traditional stadium ceremonies, this unique setting aims to integrate the city\&#x27;s iconic landmarks into the festivities.\n\n### Daily Schedule\n\nThe Games will feature 329 medal events across 32 sports. Below is a summary of key events and highlights for each day:\n\n- **July 27, 2024 (Day 1):** The first medals will be awarded in shooting, with the mixed team 10m air rifle event at the Châteauroux Shooting Centre. Other events include judo, cycling, and swimming.\n- **July 28, 2024 (Day 2):** Swimming events commence, with medals awarded in men\&#x27;s and women\&#x27;s 400m freestyle. Gymnastics and fencing events also begin.\n- **July 29, 2024 (Day 3):** Track cycling and weightlifting events start, with key competitions in judo and tennis.\n- **July 30, 2024 (Day 4):** The athletics events kick off, including the men\&#x27;s 100m heats. Swimming continues with the 200m freestyle.\n- **July 31, 2024 (Day 5):** Basketball, volleyball, and handball competitions intensify. Medals in gymnastics and swimming are awarded.\n- **August 1, 2024 (Day 6):** Equestrian events begin, and the athletics 100m finals take place. Beach volleyball and rowing events continue.\n- **August 2, 2024 (Day 7):** The marathon swimming event takes place, along with key matches in football and rugby sevens.\n- **August 3, 2024 (Day 8):** The first week concludes with finals in swimming, athletics, and weightlifting.\n\n### Closing Ceremony\n\nThe Closing Ceremony will be held on August 11, 2024, at the Stade de France. The ceremony will feature performances celebrating global cultures and the achievements of the athletes.\n\n## Venues\n\nThe Paris 2024 Olympics will be held across multiple iconic venues in and around the city. Key locations include:\n\n- **Stade de France:** The primary stadium for athletics and the opening and closing ceremonies.\n- **Châteauroux Shooting Centre:** The venue for shooting events.\n- **Grand Palais:** Fencing and taekwondo competitions.\n- **Paris La Défense Arena:** Gymnastics events.\n- **Versailles:** Equestrian events.\n- **Surfing in Tahiti:** The surfing competitions will take place on the island of Tahiti, leveraging its world-renowned waves.\n\n## New Sports and Events\n\nThe 2024 Paris Olympics will introduce several new sports and events, aiming to attract a younger audience and reflect modern athletic trends. These include:\n\n- **Breaking (Breakdancing):** Making its Olympic debut with B-Boys and B-Girls competing in dynamic dance battles.\n- **Sport Climbing:** Building on its success in Tokyo 2020, sport climbing will feature speed, bouldering, and lead events.\n- **Skateboarding:** Continuing from Tokyo 2020, with street and park events.\n- **Surfing:** Held in Tahiti, this event promises thrilling competition on some of the world\&#x27;s best waves.\n- **Mixed Team Events:** New mixed-gender team events in athletics, archery, judo, and swimming.\n\n## Sustainability and Innovation\n\nParis 2024 is committed to delivering an environmentally sustainable Games. Key initiatives include:\n\n- **Carbon Neutrality:** Efforts to offset carbon emissions through renewable energy and sustainable practices.\n- **Recycled Materials:** Use of recycled materials for medals and infrastructure.\n- **Public Transport:** Encouraging the use of public transport to reduce the carbon footprint of the Games.\n\n## Conclusion\n\nThe 2024 Paris Olympics promise to be a groundbreaking and memorable event, combining historic Parisian landmarks with cutting-edge sports and sustainability initiatives. Whether you\&#x27;re a sports enthusiast or a casual viewer, the Paris Games offer something for everyone. Stay tuned for more updates as we count down to this spectacular celebration of global athleticism.\n\n---\n\nThis comprehensive guide provides a detailed overview of the 2024 Paris Olympics, making it easy for readers to understand the schedule, events, venues, and key highlights.&#x27;, pydantic=None, json_dict=None, agent=&#x27;Internet Research&#x27;, output_format=&lt;OutputFormat.RAW: &#x27;raw&#x27;&gt;)], token_usage=UsageMetrics(total_tokens=0, prompt_tokens=0, completion_tokens=0, successful_requests=0))
```

```
result.raw
```

```
&#x27;my best complete final answer to the task.\n\n---\n\n# Comprehensive Guide to the 2024 Paris Olympics\n\n## Introduction\n\nThe 2024 Paris Olympics, officially known as the Games of the XXXIII Olympiad, will be a momentous event bringing together athletes from around the globe to compete in a variety of sports. The Paris Games are scheduled to take place from July 26 to August 11, 2024. This blog post aims to provide a detailed overview of the event, including the schedule, venues, key highlights, and new sports making their debut.\n\n## Historical Context\n\nParis has a long history with the Olympic Games, having previously hosted the event in 1900 and 1924. The 2024 Games mark the centenary of the last Paris-hosted Olympics, adding a layer of historical significance to the event.\n\n## The Vision of Paris 2024\n\nThe Paris 2024 Olympics aim to be a celebration of sport, culture, and sustainability. The Organising Committee has focused on creating an inclusive, innovative, and environmentally friendly event. The official slogan for the Games is &quot;Ouvrons Grand Les Jeux,&quot; which translates to &quot;Games Wide Open.&quot;\n\n## Schedule and Events\n\n### Opening Ceremony\n\nThe Opening Ceremony will take place on July 26, 2024, along the banks of the Seine River. Unlike traditional stadium ceremonies, this unique setting aims to integrate the city\&#x27;s iconic landmarks into the festivities.\n\n### Daily Schedule\n\nThe Games will feature 329 medal events across 32 sports. Below is a summary of key events and highlights for each day:\n\n- **July 27, 2024 (Day 1):** The first medals will be awarded in shooting, with the mixed team 10m air rifle event at the Châteauroux Shooting Centre. Other events include judo, cycling, and swimming.\n- **July 28, 2024 (Day 2):** Swimming events commence, with medals awarded in men\&#x27;s and women\&#x27;s 400m freestyle. Gymnastics and fencing events also begin.\n- **July 29, 2024 (Day 3):** Track cycling and weightlifting events start, with key competitions in judo and tennis.\n- **July 30, 2024 (Day 4):** The athletics events kick off, including the men\&#x27;s 100m heats. Swimming continues with the 200m freestyle.\n- **July 31, 2024 (Day 5):** Basketball, volleyball, and handball competitions intensify. Medals in gymnastics and swimming are awarded.\n- **August 1, 2024 (Day 6):** Equestrian events begin, and the athletics 100m finals take place. Beach volleyball and rowing events continue.\n- **August 2, 2024 (Day 7):** The marathon swimming event takes place, along with key matches in football and rugby sevens.\n- **August 3, 2024 (Day 8):** The first week concludes with finals in swimming, athletics, and weightlifting.\n\n### Closing Ceremony\n\nThe Closing Ceremony will be held on August 11, 2024, at the Stade de France. The ceremony will feature performances celebrating global cultures and the achievements of the athletes.\n\n## Venues\n\nThe Paris 2024 Olympics will be held across multiple iconic venues in and around the city. Key locations include:\n\n- **Stade de France:** The primary stadium for athletics and the opening and closing ceremonies.\n- **Châteauroux Shooting Centre:** The venue for shooting events.\n- **Grand Palais:** Fencing and taekwondo competitions.\n- **Paris La Défense Arena:** Gymnastics events.\n- **Versailles:** Equestrian events.\n- **Surfing in Tahiti:** The surfing competitions will take place on the island of Tahiti, leveraging its world-renowned waves.\n\n## New Sports and Events\n\nThe 2024 Paris Olympics will introduce several new sports and events, aiming to attract a younger audience and reflect modern athletic trends. These include:\n\n- **Breaking (Breakdancing):** Making its Olympic debut with B-Boys and B-Girls competing in dynamic dance battles.\n- **Sport Climbing:** Building on its success in Tokyo 2020, sport climbing will feature speed, bouldering, and lead events.\n- **Skateboarding:** Continuing from Tokyo 2020, with street and park events.\n- **Surfing:** Held in Tahiti, this event promises thrilling competition on some of the world\&#x27;s best waves.\n- **Mixed Team Events:** New mixed-gender team events in athletics, archery, judo, and swimming.\n\n## Sustainability and Innovation\n\nParis 2024 is committed to delivering an environmentally sustainable Games. Key initiatives include:\n\n- **Carbon Neutrality:** Efforts to offset carbon emissions through renewable energy and sustainable practices.\n- **Recycled Materials:** Use of recycled materials for medals and infrastructure.\n- **Public Transport:** Encouraging the use of public transport to reduce the carbon footprint of the Games.\n\n## Conclusion\n\nThe 2024 Paris Olympics promise to be a groundbreaking and memorable event, combining historic Parisian landmarks with cutting-edge sports and sustainability initiatives. Whether you\&#x27;re a sports enthusiast or a casual viewer, the Paris Games offer something for everyone. Stay tuned for more updates as we count down to this spectacular celebration of global athleticism.\n\n---\n\nThis comprehensive guide provides a detailed overview of the 2024 Paris Olympics, making it easy for readers to understand the schedule, events, venues, and key highlights.&#x27;
```

🍻现开设了大**模型学习交流群，**扫描下👇码，来遇见更多志同道合的小伙伴~

![](img/c9c18412264901b2e0f64896af29618e.png)

海量硬核独家技术**干货内容**+无门槛**技术交流****+不定期开设****硬核干货&前沿技术公开课****，****扫码**👆即刻入群！