
# 万字长文深度解析LLM Agent规划框架：HuggingGPT

> 来源：[https://agijuejin.feishu.cn/docx/YZ1vdmQ6Ko1uxAxp8SectnDznAv](https://agijuejin.feishu.cn/docx/YZ1vdmQ6Ko1uxAxp8SectnDznAv)

> 作者：还就可 | 深入LLM Agent应用开发

> 原文：[https://mp.weixin.qq.com/s/8-jrG-USKUJxJjFh3GT0yA](https://mp.weixin.qq.com/s/8-jrG-USKUJxJjFh3GT0yA)

书接上回 详解大语言模型LLM Agent中规划工作流，我们说到 HuggingGPT，它是一个结合了ChatGPT和Hugging Face平台上的各种专家模型，以解决复杂的AI任务，可以认为它是一种结合任务规划和工具调用两种Agent工作流的框架。它的工作流程主要分为以下几个步骤：

- 任务规划：使用ChatGPT分析用户的请求，理解他们的意图，并将其分解为可能可解决的任务。
- 模型选择：为了完成规划的任务，ChatGPT根据模型的描述选择托管在Hugging Face上的专家模型。
- 任务执行：调用并执行每个选定的模型，然后将结果返回给ChatGPT。
- 响应生成：最后，使用ChatGPT整合所有模型的预测结果，并生成响应。

![](img/3c39a215ebda9dc78988bfd57f448034.png)

图 1

光说不练假把式，我们先尝试运行，然后逐步分析各个阶段的Prompt设计和代码设计。

## 1. 运行

下载Repo `git clone https://github.com/microsoft/JARVIS.git`

### 1.1 安装依赖

安装server依赖

```
cd JARVIS/hugginggpt/server
conda create -n jarvis python=3.8
conda activate jarvis
pip install -r requirements.txt
```

安装前端页面

```
cd ../web
npm install
```

注意，`requirment.txt`中的`werkzeug`要更新为 `Werkzeug==2.2.2`，否则`Flask`会报不兼容问题。这里没有安装`pytorch`之类的，因为我们不打算在本地下载模型，所需空间过于巨大，直接访问线上的模型。

### 1.2 修改配置

既然需要在线使用`HuggingGPT`的模型，那么我们需要到 `HuggingGPT` [1] 上申请Token。修改 `server/configs/config.lite.yaml`，更新`huggingface` token。另外我们要使用本地的LLM模型，需要修改 `openai->api_key`，必须添加`sk`开头的字符串，不然报错。必须添加 `local->endpoint`，就是你本地openai的地址。此外，你可能还要修改是否采用续写 `use_completion` 和模型。如果无法访问`HuggingGPT`你还需要添加`proxy`。

```
openai: 
  api_key: sk-xxxx # added
huggingface:
  token: hf_xxx # updated
dev: true
debug: false
log_file: logs/debug.log
model: gpt-3.5-turbo # updated
use_completion: false # updated
inference_mode: huggingface # local, huggingface or hybrid, prefer hybrid
local_deployment: minimal # minimal, standard or full, prefer full
num_candidate_models: 5
max_description_length: 100
proxy: http://127.0.0.1:7890 # optional: your proxy server &quot;http://ip:port&quot;
local:
  endpoint: http://localhost:11434 # updated
...
```

此外，还需要修改 `server/awsome_chat.py`，添加`API_KEY`否则也无法运行本地LLM。

```
if API_TYPE == &quot;local&quot;:
     API_ENDPOINT = f&quot;{config[&#x27;local&#x27;][&#x27;endpoint&#x27;]}/v1/{api_name}&quot;
+    API_KEY = config[&#x27;openai&#x27;][&#x27;api_key&#x27;]
```

### 1.3 运行

开始运行

```
python --config configs/config.lite.yaml --mode server
npm run dev
```

然后我们打开浏览器`http://localhost:9999/#/`，出现类似下图的窗口。

![](img/2a3cc95ce243412cd105618c5faf4f64.png)

`image-20240524101621504`

输入类似

> describe the image `/examples/c.jpg`.

其中`examples`位于是 `hugginggpt/server/public/examples/`，所以如果你要测试自己的图片，可以考虑将图片放在这儿。会输出类似如下图的结果。

![](img/ecfa7362c86389d4fa2f2a1111d793fa.png)

`image-20240524102326517`

## 2. 分析

在文章开头我们有说过，任务是分为规划任务、选择模型、执行任务和生成响应。那么我们先从任务规划看起。

### 2.1 任务规划

任务规划需要LLM进行推理分解任务，对于这样一个将HuggingFace当做调用工具的框架，我们要如何设计Prompt？几个原则

1. 说明任务
2. 说明任务的输入输出
3. Few-Shot示例
4. 上下文
5. 用户输入

此外，我们还需要将HuggingFace所包含的API给到LLM，进行推理用户问题所需任务步骤。我们当然不可能将所有HuggingFace上的Model都提供给LLM，所以我们提供任务类型，`huggingface`上大约有19个任务类型 [2]，其中15个NLP任务类型，2个Audio任务类型，3个CV的任务类型。

> 我觉得这里通过任务类型缩小LLM选择工具选择范围，之后再通过任务类型，然后再让LLM选择具体的模型，相当于一种摘要技术，从大类选择，再缩小到具体选择。你要知道`hugginggpt`在`p0_models.jsonl`中缓存了大约673个任务，你不可能将他们所有的描述都发送给LLM，它包含2765000个字符。

说明任务，在输出上对LLM有强烈的要求，除了要求是`JSON`，而且要求推理各个任务的依赖关系，并且填充类似的JSON输出。然而我要说的这种复杂的输出要求，当前只能用于Demo，否则你会遇到非常多的解析，无法获得想要的JSON格式，或者丢失特定的字段。

```
The AI assistant can parse user input to several tasks: [{&quot;task&quot;: task, &quot;id&quot;: task_id, &quot;dep&quot;: dependency_task_id, &quot;args&quot;: {&quot;text&quot;: text or &lt;GENERATED&gt;-dep_id, &quot;image&quot;: image_url or &lt;GENERATED&gt;-dep_id, &quot;audio&quot;: audio_url or &lt;GENERATED&gt;-dep_id}}].
```

说明任务的输入和输出，除了上文说的解释任务依赖关系和如何生成`tasks`之外，这里设定了`task`必须是`HuggingFace`支持的这些类别，`args`必须是`text`、`image`和`audio`。还是老话，要求越多失败越多，我就遇到`args`偶尔缺失，`task`偶尔不对的问题。

```
The special tag &quot;&lt;GENERATED&gt;-dep_id&quot; refer to the one generated text/image/audio in the dependency task (Please consider whether the dependency task generates resources of this type.) and &quot;dep_id&quot; must be in &quot;dep&quot; list. The &quot;dep&quot; field denotes the ids of the previous prerequisite tasks which generate a new resource that the current task relies on. The &quot;args&quot; field must in [&quot;text&quot;, &quot;image&quot;, &quot;audio&quot;], nothing else. The task MUST be selected from the following options: &quot;token-classification&quot;, &quot;text2text-generation&quot;, &quot;summarization&quot;, &quot;translation&quot;, &quot;question-answering&quot;, &quot;conversational&quot;, &quot;text-generation&quot;, &quot;sentence-similarity&quot;, &quot;tabular-classification&quot;, &quot;object-detection&quot;, &quot;image-classification&quot;, &quot;image-to-image&quot;, &quot;image-to-text&quot;, &quot;text-to-image&quot;, &quot;text-to-video&quot;, &quot;visual-question-answering&quot;, &quot;document-question-answering&quot;, &quot;image-segmentation&quot;, &quot;depth-estimation&quot;, &quot;text-to-speech&quot;, &quot;automatic-speech-recognition&quot;, &quot;audio-to-audio&quot;, &quot;audio-classification&quot;, &quot;canny-control&quot;, &quot;hed-control&quot;, &quot;mlsd-control&quot;, &quot;normal-control&quot;, &quot;openpose-control&quot;, &quot;canny-text-to-image&quot;, &quot;depth-text-to-image&quot;, &quot;hed-text-to-image&quot;, &quot;mlsd-text-to-image&quot;, &quot;normal-text-to-image&quot;, &quot;openpose-text-to-image&quot;, &quot;seg-text-to-image&quot;.
```

让LLM推理规划，这里有个魔法 `Think step by step` 。

```
There may be multiple tasks of the same type. Think step by step about all the tasks needed to resolve the user\&#x27;s request. Parse out as few tasks as possible while ensuring that the user request can be resolved. Pay attention to the dependencies and order among tasks. If the user input can\&#x27;t be parsed, you need to reply empty JSON [], otherwise you must return JSON directly.
```

设定`Few shot` examples，大约有6个，考虑阅读体验，这里只放一个，更多的`Few shot` examples位于 `hugginggpt/server/demos/demo_parse_task.json` 。
