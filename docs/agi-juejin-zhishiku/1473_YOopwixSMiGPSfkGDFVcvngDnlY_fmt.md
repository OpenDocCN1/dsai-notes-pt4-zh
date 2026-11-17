
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



```
[
    {
        "role": "user",
        "content": "Give you some pictures e1.jpg, e2.png, e3.jpg, help me count the number of sheep?"
    },
    ...
]
```

最后上下文和用户输入，没什么好说的

```
The chat log [ {{context}} ] may contain the resources I mentioned. Now I input { {{input}} }. Pay attention to the input and output types of tasks and the dependencies between tasks.
```

任务规划阶段的Promt就已经结束了，代码实现上这一段较为简单，核心流程是 `chat` -> `chat_huggingface` -> `parse_task` -> `send_request`。其中 `parse_task` 负责组装 `prompt` 和构造 `open ai API` 所需的 `data` 参数。这里有一个简易的对话上下文移动窗口，就是计算历史对话文本的 `tokens`，如果超过最大 `token` 就尝试 `pop()` 掉最后一个，这是移除最近的对话记录保留开始的策略，当然你也可以尝试其他策略。

```
# cut chat logs
start = 0
while start <= len(context):
    history = context[start:]
    prompt = replace_slot(parse_task_prompt, {
        "input": input,
        "context": history 
    })
    messages.append({"role": "user", "content": prompt})
    history_text = "<im_end>\nuser<im_start>".join([m["content"] for m in messages])
    num = count_tokens(LLM_encoding, history_text)
    if get_max_context_length(LLM) - num > 800:
        break
    messages.pop()
    start += 2
```

## 2.2 模型选择

在上文 `parse_task` 完成后，在 `chat_huggingface` 中就会对返回的结果进行任务解析，此时由于 `LLM` 回复的不确定性，有时候你会遇到失败无法解析或者丢失字段，一个成功的任务会返回类似如下的 JSON。

```
[
  {'task': 'object-detection', 'id': 0, 'dep': [-1], 'args': {'image': '/examples/a.jpg'}}, 
  {'task': 'image-to-image', 'id': 1, 'dep': [-1], 'args': {'image': '/examples/a.jpg'}}
]
```

根据 `task` 信息，在 `run_task` 中根据 `task` 值去 `hugginggpt/server/data/p0_models.jsonl` 这个 JSON 中去搜索前 10 的具体模型，最后根据类型查找到可用模型如下。

```
{'local': [], 'huggingface': ['hustvl/yolos-tiny', 'microsoft/table-transformer-structure-recognition', 'facebook/detr-resnet-50', 'TahaDouaji/detr-doc-table-detection', 'hustvl/yolos-small']}
```

> 这里在选择模型时候，需要修改一下代码否则你几乎获取不到任何一个可用模型，文章留言获取。

进入函数 `choose_model` 再次构造 `Prompt`，让 `LLM` 帮助决策哪个模型更好，以下 `Prompt` 只是示意，因为源代码将其转换为了 `role` `content` 组成的 `arraylist`。

```
System: Given the user request and the parsed tasks, the AI assistant helps the user to select a suitable model from a list of models to process the user request. The assistant should focus more on the description of the model and find the model that has the most potential to solve requests and tasks. Also, prefer models with local inference endpoints for speed and stability. 
[
User: {{input}},
Assistant: {{task}}..
]
User: Please choose the most suitable model from {{metas}} for the task {{task}}. The output must be in a strict JSON format: {"id": "id", "reason": "your detail reasons for the choice"}, the id in JSON must be the one provided in the model description.
```

输出如下，已经给出了最佳的模型和原因，接下来进入模型执行。

```
{"id": "facebook/detr-resnet-50", "reason": "The model has the highest number of likes, and the description sounds promising with end-to-end detection using transformers. Also, it has a local inference endpoint for faster access."}
```

## 2.3 模型执行

上面模型已经选择，输入也有，接下来就是执行模型，这一步比较简单，就是构造 `HuggingFace API` 的请求。

```
inference_result = model_inference(best_model_id, args, hosted_on, command['task'])
```

`HuggingFace API` [3] 的请求较为简单，只要你拥有 `Token`，你甚至可以通过 `curl` 直接运行。

```
curl --location 'https://api-inference.huggingface.co/models/facebook/detr-resnet-50-panoptic' \
--header 'Authorization: Bearer replacewithyourowntoken' \
--header 'Content-Type: image/jpeg' \
--data '@/Users/xxxx/dc579d59_track_0.jpg'
```

由于可能包含多个任务，所以任务是通过 `thread` 并发执行的，最后通过 `queue` 进行收取结果如下所示。

```
{'generated image': '/images/a59b.jpg', 'predicted': [{'score': 0.9699670672416687, 'label': 'potted plant', 'box': {'xmin': 0, 'ymin': 240, 'xmax': 187, 'ymax': 484}}, {'score': 0.9995023012161255, 'label': 'cat', 'box': {'xmin': 165, 'ymin': 59, 'xmax': 645, 'ymax': 522}}]
DEBUG:__main__:{1: {'task': {'task': 'image-to-image', 'id': 1, 'dep': [-1], 'args': {'image': 'public//examples/a.jpg'}}, 'inference result': {'error': 'Model lambdalabs/sd-image-variations-diffusers is currently loading', 'estimated_time': 248.20472717285156}, 'choose model result': {'id': 'lambdalabs/sd-image-variations-diffusers', 'reason': "The model has the most likes and it's also the only model with the tag 'stable-diffusion', which indicates it's a robust and popular choice for various image tasks."}}, 0: {'task': {'task': 'object-detection', 'id': 0, 'dep': [-1], 'args': {'image': 'public//examples/a.jpg'}}, 'inference result': {'generated image': '/images/a59b.jpg', 'predicted': [{'score': 0.9699670672416687, 'label': 'potted plant', 'box': {'xmin': 0, 'ymin': 240, 'xmax': 187, 'ymax': 484}}, {'score': 0.9995023012161255, 'label': 'cat', 'box': {'xmin': 165, 'ymin': 59, 'xmax': 645, 'ymax': 522}}], 'choose model result': {'id': 'facebook/detr-resnet-50', 'reason': 'The model has the highest number of likes, and the description sounds promising with end-to-end detection using transformers. Also, it has a local inference endpoint for faster access.'}}}
```

## 2.4 响应生成



在`chat_huggingface`中调用`response_results`，给定`input`和收集的`results`来构造类似如下所示的Prompt，其中`processes`就是上文模型执行的结果。该prompt先给定输入的问题和模型执行的结果，然后要求LLM仔细甄别输出的推测结果和问题是否匹配，然后进行作答。最后要求输出的它的工作流，包括使用的模型和推测结果和生成的文件位置等信息。

```
System: #4 Response Generation Stage: With the task execution logs, the AI assistant needs to describe the process and inference results.
User: Look at /examples/a.jpg picture, can you tell me how many objects in the picture? Give me a picture similar to this one.
Assitant: Before give you a response, I want to introduce my workflow for your request, which is shown in the following JSON data: {{processes}}. Do you have any demands regarding my response?
User: Yes. Please first think carefully and directly answer my request based on the inference results. Some of the inferences may not always turn out to be correct and require you to make careful consideration in making decisions. Then please detail your workflow including the used models and inference results for my request in your friendly tone. Please filter out information that is not relevant to my request. Tell me the complete path or urls of files in inference results. If there is nothing in the results, please tell me you can&#x27;t make it.
```

最终LLM输入如下响应

```
Sure! Based on the inference results, there are two objects in the picture &quot;/examples/a.jpg&quot;. They are a potted plant and a cat.
For your second request, after reviewing the available options, I&#x27;ve selected a similar image using a model with the ID &quot;facebook/detr-resnet-50&quot;. This model identified a similar image with the path: &quot;/images/a59b.jpg&quot;.
The workflow goes like this:
1. Object Detection: I used a model called &quot;facebook/detr-resnet-50&quot; to detect objects in your image. It&#x27;s an end-to-end model that uses transformers and has shown great performance. It successfully identified two objects for us.
2. Image Similarity: To find a similar image, I picked another pre-trained model called &quot;lambdalabs/sd-image-variations-diffusers&quot;. This model is known to create variations of input images while keeping the main content stable. It&#x27;s a diffusion-based model, and these types of models are good at generating new content with preserving the essence of the original image. 
However, I need to let you know that getting the similar image didn&#x27;t work this time. The inference took longer than expected, which is not uncommon for some complex models, and eventually timed out. So, unfortunately, I couldn&#x27;t generate a similar image within the given constraints.
I hope this helps! Let me know if you&#x27;d like more details on any specific steps or have additional questions.
```

## 3. 总结

纵观整个流程其实主要还是Prompt的设计，如何设计更好的Prompt生成任务规划、模型选择和响应生成。在这种中间就是各种结果的解析工作，以及调用各种工具完成任务。值得一说的是，我在实验过程中，遇到各种各样的错误，尤其是第一阶段的任务规划中的任务输出，它对LLM的要求非常高，如果你只是本地的小模型或许是难以胜任的。第二，当你有很多工具选择，不妨将他们先进行分类，让LLM先从分类中选择，然后筛选出缩小后具体的工具列表，再次给到LLM选择最优最匹配的工具。

望君有所得。如果您意犹未尽，想要参与LangChain实战课程，可以点击原文链接查看购买，亲历LLM应用开发之旅。关注我，及时获得更新。

关注点赞在看不迷路，获取即时更新。

### 参考资料

[1]HuggingGPT: https://huggingface.co/settings/tokens

[2]huggingface上大约有19个任务类型: https://huggingface.co/docs/api-inference/detailed_parameters

[3]HuggingFace API: https://huggingface.co/docs/api-inference/detailed_parameters
