# 万字长文深度解析LLM Agent规划框架：HuggingGPT

> 来源：[https://agijuejin.feishu.cn/docx/YZ1vdmQ6Ko1uxAxp8SectnDznAv](https://agijuejin.feishu.cn/docx/YZ1vdmQ6Ko1uxAxp8SectnDznAv)

> 作者：还就可 | 深入LLM Agent应用开发

> 原文： [https://mp.weixin.](https://mp.weixin.qq.com/s/8-jrG-USKUJxJjFh3GT0yA)qq.com/s/8-jrG-US...

书接上回 详解大语言模型LLM Agent中规划 工作流 ，我们说到 HuggingGPT，它是一个结合了ChatGPT和Hugging Face平台上的各种专家模型，以解决复杂的AI任务，可以认为它是一种结合任务规划和工具调用两种Agent工作流的框架。 它的工作流程主要分为以下几个步骤：

+   任务规划：使用ChatGPT分析用户的请求，理解他们的意图，并将其分解为可能可解决的任务。

+   模型选择：为了完成规划的任务，ChatGPT根据模型的描述选择托管在Hugging Face上的专家模型。

+   任务执行：调用并执行每个选定的模型，然后将结果返回给ChatGPT。

+   响应生成：最后，使用ChatGPT整合所有模型的预测结果，并生成响应。

![](img/3c39a215ebda9dc78988bfd57f448034.png)

图 1

光说不练假把式，我们先尝试运行，然后逐步分析各个阶段的Prompt设计和代码设计。

## ** 1. 运行**

下载Repo git clone https://github.com/microsoft/JARVIS.git

### ** 1.1 安装依赖 **

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

注意，requirment.txt中的的werkzeug要更新为 Werkzeug==2.2.2 ，否则Flask会报不兼容问题。这里没有安装pytorch之类的，因为我们不打算在本地下载模型，所需空间过于巨大，直接访问线上的模型。

### ** 1.2 修改配置 **

**既然需要在线使用Hugging**GPT的模型，那么我们需要到 HuggingGPT [1] 上申请Token。修改 server/configs/config.lite.yaml ，更新huggingface token。另外我们要使用本地的LLM模型，需要修改 openai->api_key ，必须添加sk开头的字符串，不然报错。必须添加 local->endpoint , 就是你本地openai的地址。此外，你可能还要修改是否采用续写 use_completion 和模型。如果无法访问HuggingGPT你还需要添加proxy。

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

此外，还需要修改 server/awsome_chat.py ，添加API_KEY否则也无法运行本地LLM。

```
if API_TYPE == &quot;local&quot;:
     API_ENDPOINT = f&quot;{config[&#x27;local&#x27;][&#x27;endpoint&#x27;]}/v1/{api_name}&quot;
+    API_KEY = config[&#x27;openai&#x27;][&#x27;api_key&#x27;]
```

### ** 1.2 运行 **

开始运行

```
python  --config configs/config.lite.yaml --mode server
npm run dev
```

然后我们打开浏览器http://localhost:9999/#/, 出现类似下图的窗口。

![](img/2a3cc95ce243412cd105618c5faf4f64.png)

image-20240524101621504

输入类似

> describe the image /examples/c.jpg.

其中examples位于是 hugginggpt/server/public/examples/ ，所以如果你要测试自己的图片，可以考虑将图片放在这儿。会输出类似如下图的结果。

![](img/ecfa7362c86389d4fa2f2a1111d793fa.png)

image-20240524102326517

## ** 2. 分析**

在文章开头我们有说过，任务是分为规划任务、选择模型、执行任务和生成响应。那么我们先从任务规划看起。

### ** 2.1 任务规划 **

任务规划需要LLM进行推理分解任务，对于这样一个将HuggingFace当做调用工具的框架，我们要如何设计Prompt？几个原则

1.  说明任务

1.  说明任务的输入输出

1.  Few-Shot示例

1.  上下文

1.  用户输入

此外，我们还需要将HuggingFace所包含的API给到LLM，进行推理用户问题所需任务步骤。我们当然不可能将所有HuggingFace上的Model都提供给LLM，所以我们提供任务类型， huggingface上大约有19个任务类型 [2] , 其中15个NLP任务类型，2个Audio任务类型，3个CV的任务类型。

> 我觉得这里通过任务类型缩小LLM选择工具选择范围，之后再通过任务类型，然后再让LLM选择具体的模型，相当于一种摘要技术，从大类选择， 再 缩小到具体选择。 你要知道hugginggpt在p0_models.jsonl中缓存了大约673个任务，你不可能将他们所有的描述都发送给LLM，它包含2765000个字符。

说**明**任务，在输出上对LLM有强烈的要求，除了要求是JSON，而且要求推理各个任务的依赖关系，并且填充类似的JSON输出。 然而我要说的这种复杂的输出要求，当前只能用于Demo，否则你会遇到非常多的解析，无法获得想要的JSON格式，或者丢失特定的字段。

```
The AI assistant can parse user input to several tasks: [{&quot;task&quot;: task, &quot;id&quot;: task_id, &quot;dep&quot;: dependency_task_id, &quot;args&quot;: {&quot;text&quot;: text or &lt;GENERATED&gt;-dep_id, &quot;image&quot;: image_url or &lt;GENERATED&gt;-dep_id, &quot;audio&quot;: audio_url or &lt;GENERATED&gt;-dep_id}}].
```

说明**任务的输入和输出，除了上文说的解释任务**依赖关系和如何生成tasks之外，这里设定了task必须是HuggingFace支持的这些类别，args必须是text、imag和audio。 还是老话，要求越多失败越多，我就遇到args偶尔缺失，task偶尔不对的问题。

```
The special tag &quot;&lt;GENERATED&gt;-dep_id&quot; refer to the one generated text/image/audio in the dependency task (Please consider whether the dependency task generates resources of this type.) and &quot;dep_id&quot; must be in &quot;dep&quot; list. The &quot;dep&quot; field denotes the ids of the previous prerequisite tasks which generate a new resource that the current task relies on. The &quot;args&quot; field must in [&quot;text&quot;, &quot;image&quot;, &quot;audio&quot;], nothing else. The task MUST be selected from the following options: &quot;token-classification&quot;, &quot;text2text-generation&quot;, &quot;summarization&quot;, &quot;translation&quot;, &quot;question-answering&quot;, &quot;conversational&quot;, &quot;text-generation&quot;, &quot;sentence-similarity&quot;, &quot;tabular-classification&quot;, &quot;object-detection&quot;, &quot;image-classification&quot;, &quot;image-to-image&quot;, &quot;image-to-text&quot;, &quot;text-to-image&quot;, &quot;text-to-video&quot;, &quot;visual-question-answering&quot;, &quot;document-question-answering&quot;, &quot;image-segmentation&quot;, &quot;depth-estimation&quot;, &quot;text-to-speech&quot;, &quot;automatic-speech-recognition&quot;, &quot;audio-to-audio&quot;, &quot;audio-classification&quot;, &quot;canny-control&quot;, &quot;hed-control&quot;, &quot;mlsd-control&quot;, &quot;normal-control&quot;, &quot;openpose-control&quot;, &quot;canny-text-to-image&quot;, &quot;depth-text-to-image&quot;, &quot;hed-text-to-image&quot;, &quot;mlsd-text-to-image&quot;, &quot;normal-text-to-image&quot;, &quot;openpose-text-to-image&quot;, &quot;seg-text-to-image&quot;.
```

让LLM推理规划，这里有个魔法 Think step by step 。

```
There may be multiple tasks of the same type. Think step by step about all the tasks needed to resolve the user\&#x27;s request. Parse out as few tasks as possible while ensuring that the user request can be resolved. Pay attention to the dependencies and order among tasks. If the user input can\&#x27;t be parsed, you need to reply empty JSON [], otherwise you must return JSON directly.
```

设定Few shot examples，大约有6个，考虑阅读体验，这里只放一个，更多的Few shot examples位于 hugginggpt/server/demos/demo_parse_task.json 。

```
[
    {
        &quot;role&quot;: &quot;user&quot;,
        &quot;content&quot;: &quot;Give you some pictures e1.jpg, e2.png, e3.jpg, help me count the number of sheep?&quot;
    },
    {
        &quot;role&quot;: &quot;assistant&quot;,
        &quot;content&quot;: &quot;[{\&quot;task\&quot;: \&quot;image-to-text\&quot;, \&quot;id\&quot;: 0, \&quot;dep\&quot;: [-1], \&quot;args\&quot;: {\&quot;image\&quot;: \&quot;e1.jpg\&quot; }}, {\&quot;task\&quot;: \&quot;object-detection\&quot;, \&quot;id\&quot;: 1, \&quot;dep\&quot;: [-1], \&quot;args\&quot;: {\&quot;image\&quot;: \&quot;e1.jpg\&quot; }}, {\&quot;task\&quot;: \&quot;visual-question-answering\&quot;, \&quot;id\&quot;: 2, \&quot;dep\&quot;: [1], \&quot;args\&quot;: {\&quot;image\&quot;: \&quot;&lt;GENERATED&gt;-1\&quot;, \&quot;text\&quot;: \&quot;How many sheep in the picture\&quot;}} }}, {\&quot;task\&quot;: \&quot;image-to-text\&quot;, \&quot;id\&quot;: 3, \&quot;dep\&quot;: [-1], \&quot;args\&quot;: {\&quot;image\&quot;: \&quot;e2.png\&quot; }}, {\&quot;task\&quot;: \&quot;object-detection\&quot;, \&quot;id\&quot;: 4, \&quot;dep\&quot;: [-1], \&quot;args\&quot;: {\&quot;image\&quot;: \&quot;e2.png\&quot; }}, {\&quot;task\&quot;: \&quot;visual-question-answering\&quot;, \&quot;id\&quot;: 5, \&quot;dep\&quot;: [4], \&quot;args\&quot;: {\&quot;image\&quot;: \&quot;&lt;GENERATED&gt;-4\&quot;, \&quot;text\&quot;: \&quot;How many sheep in the picture\&quot;}} }}, {\&quot;task\&quot;: \&quot;image-to-text\&quot;, \&quot;id\&quot;: 6, \&quot;dep\&quot;: [-1], \&quot;args\&quot;: {\&quot;image\&quot;: \&quot;e3.jpg\&quot; }},  {\&quot;task\&quot;: \&quot;object-detection\&quot;, \&quot;id\&quot;: 7, \&quot;dep\&quot;: [-1], \&quot;args\&quot;: {\&quot;image\&quot;: \&quot;e3.jpg\&quot; }}, {\&quot;task\&quot;: \&quot;visual-question-answering\&quot;, \&quot;id\&quot;: 8, \&quot;dep\&quot;: [7], \&quot;args\&quot;: {\&quot;image\&quot;: \&quot;&lt;GENERATED&gt;-7\&quot;, \&quot;text\&quot;: \&quot;How many sheep in the picture\&quot;}}]&quot;
    },
    ...
]
```

最后上下文和用户输入，没什么好说的

```
The chat log [ {{context}} ] may contain the resources I mentioned. Now I input { {{input}} }. Pay attention to the input and output types of tasks and the dependencies between tasks.
```

任务规划阶段的Promt就已经结束了，代码实现上这一段较为简单，核心流程是 chat->chat_huggingface->parse_task->send_request 。其中parse_task负责组装prompt和构造open ai API所需的data参数。这里有一个简易的对话上下文移动窗口，就是计算历史对话文本的tokens，如果超过最大token就尝试pop掉最后一个，这是移除最近的对话记录保留开始的策略，当然你也可以尝试其他策略。

```
# cut chat logs
start = 0
while start &lt;= len(context):
    history = context[start:]
    prompt = replace_slot(parse_task_prompt, {
        &quot;input&quot;: input,
        &quot;context&quot;: history 
    })
    messages.append({&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: prompt})
    history_text = &quot;&lt;im_end&gt;\nuser&lt;im_start&gt;&quot;.join([m[&quot;content&quot;] for m in messages])
    num = count_tokens(LLM_encoding, history_text)
    if get_max_context_length(LLM) - num &gt; 800:
        break
    messages.pop()
    start += 2
```

### ** 2.2 模型选择 **

在上文parse_task完成后，在chat_huggingface中就会对返回的结果进行任务解析，此时由于LLM回复的不确定性，有时候你会遇到失败无法解析或者丢失字段，一个成功的任务会返回类似如下的JSON。

```
[
  {&#x27;task&#x27;: &#x27;object-detection&#x27;, &#x27;id&#x27;: 0, &#x27;dep&#x27;: [-1], &#x27;args&#x27;: {&#x27;image&#x27;: &#x27;/examples/a.jpg&#x27;}}, 
  {&#x27;task&#x27;: &#x27;image-to-image&#x27;, &#x27;id&#x27;: 1, &#x27;dep&#x27;: [-1], &#x27;args&#x27;: {&#x27;image&#x27;: &#x27;/examples/a.jpg&#x27;}}
]
```

根据task信息，在 run_task 中根据task值去 hugginggpt/server/data/p0_models.jsonl 这个json中去搜索前10的具体模型，最后根据类型查找到可用模型如下。

```
{&#x27;local&#x27;: [], &#x27;huggingface&#x27;: [&#x27;hustvl/yolos-tiny&#x27;, &#x27;microsoft/table-transformer-structure-recognition&#x27;, &#x27;facebook/detr-resnet-50&#x27;, &#x27;TahaDouaji/detr-doc-table-detection&#x27;, &#x27;hustvl/yolos-small&#x27;]}
```

> 这里在选择模型时候，需要修改一下代码否则你几乎获取不到任何一个可用模型，文章留言获取。

进入函数 choose_model 再次构造Prompt，让LLM帮助决策哪个模型更好, 以下prompt只是示意，因为源代码将其转换为了role content组成的arraylist。

```
System: Given the user request and the parsed tasks, the AI assistant helps the user to select a suitable model from a list of models to process the user request. The assistant should focus more on the description of the model and find the model that has the most potential to solve requests and tasks. Also, prefer models with local inference endpoints for speed and stability. 
[
User: {{input}},
Assistant: {{task}}..
]
User: Please choose the most suitable model from {{metas}} for the task {{task}}. The output must be in a strict JSON format: {&quot;id&quot;: &quot;id&quot;, &quot;reason&quot;: &quot;your detail reasons for the choice&quot;}, the id in JSON must be the one provided in the model description.
```

输出如下，已经给出了最佳的模型和原因，接下来进入模型执行。

```
{&quot;id&quot;: &quot;facebook/detr-resnet-50&quot;, &quot;reason&quot;: &quot;The model has the highest number of likes, and the description sounds promising with end-to-end detection using transformers. Also, it has a local inference endpoint for faster access.&quot;}
```

### ** 2.3 模型执行 **

上面模型已经选择，输入也有，接下来就是执行模型，这一步比较简单，就是构造huggingface api的请求。

```
inference_result = model_inference(best_model_id, args, hosted_on, command[&#x27;task&#x27;])
```

HuggingFace API [3] 的请求较为简单，只要你拥有Token，你甚至可以通过curl直接运行。

```
curl --location &#x27;https://api-inference.huggingface.co/models/facebook/detr-resnet-50-panoptic&#x27; \
--header &#x27;Authorization: Bearer replacewithyourowntoken&#x27; \
--header &#x27;Content-Type: image/jpeg&#x27; \
--data &#x27;@/Users/xxxx/dc579d59_track_0.jpg&#x27;
```

由于可能包含多个任务，所以任务是通过thread并发执行的，最后通过queue进行收取结果如下所示。

```
{&#x27;generated image&#x27;: &#x27;/images/a59b.jpg&#x27;, &#x27;predicted&#x27;: [{&#x27;score&#x27;: 0.9699670672416687, &#x27;label&#x27;: &#x27;potted plant&#x27;, &#x27;box&#x27;: {&#x27;xmin&#x27;: 0, &#x27;ymin&#x27;: 240, &#x27;xmax&#x27;: 187, &#x27;ymax&#x27;: 484}}, {&#x27;score&#x27;: 0.9995023012161255, &#x27;label&#x27;: &#x27;cat&#x27;, &#x27;box&#x27;: {&#x27;xmin&#x27;: 165, &#x27;ymin&#x27;: 59, &#x27;xmax&#x27;: 645, &#x27;ymax&#x27;: 522}}]}
DEBUG:__main__:{1: {&#x27;task&#x27;: {&#x27;task&#x27;: &#x27;image-to-image&#x27;, &#x27;id&#x27;: 1, &#x27;dep&#x27;: [-1], &#x27;args&#x27;: {&#x27;image&#x27;: &#x27;public//examples/a.jpg&#x27;}}, &#x27;inference result&#x27;: {&#x27;error&#x27;: &#x27;Model lambdalabs/sd-image-variations-diffusers is currently loading&#x27;, &#x27;estimated_time&#x27;: 248.20472717285156}, &#x27;choose model result&#x27;: {&#x27;id&#x27;: &#x27;lambdalabs/sd-image-variations-diffusers&#x27;, &#x27;reason&#x27;: &quot;The model has the most likes and it&#x27;s also the only model with the tag &#x27;stable-diffusion&#x27;, which indicates it&#x27;s a robust and popular choice for various image tasks.&quot;}}, 0: {&#x27;task&#x27;: {&#x27;task&#x27;: &#x27;object-detection&#x27;, &#x27;id&#x27;: 0, &#x27;dep&#x27;: [-1], &#x27;args&#x27;: {&#x27;image&#x27;: &#x27;public//examples/a.jpg&#x27;}}, &#x27;inference result&#x27;: {&#x27;generated image&#x27;: &#x27;/images/a59b.jpg&#x27;, &#x27;predicted&#x27;: [{&#x27;score&#x27;: 0.9699670672416687, &#x27;label&#x27;: &#x27;potted plant&#x27;, &#x27;box&#x27;: {&#x27;xmin&#x27;: 0, &#x27;ymin&#x27;: 240, &#x27;xmax&#x27;: 187, &#x27;ymax&#x27;: 484}}, {&#x27;score&#x27;: 0.9995023012161255, &#x27;label&#x27;: &#x27;cat&#x27;, &#x27;box&#x27;: {&#x27;xmin&#x27;: 165, &#x27;ymin&#x27;: 59, &#x27;xmax&#x27;: 645, &#x27;ymax&#x27;: 522}}]}, &#x27;choose model result&#x27;: {&#x27;id&#x27;: &#x27;facebook/detr-resnet-50&#x27;, &#x27;reason&#x27;: &#x27;The model has the highest number of likes, and the description sounds promising with end-to-end detection using transformers. Also, it has a local inference endpoint for faster access.&#x27;}}}
```

### ** 2.4 响应生成 **

在chat_huggingface中调用response_results，给定input和收集的results来构造类似如下所示的Prompt，其中processes就是上文模型执行的结果。该prompt先给定输入的问题和模型执行的结果，然后要求LLM仔细甄别输出的推测结果和问题是否匹配，然后进行作答。最后要求输出的它的工作流，包括使用的模型和推测结果和生成的文件位置等信息。

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

## ** 3. 总结**

纵观整个流程其实主要还是Prompt的设计，如何设计更好的Prompt生成任务规划、模型选择和响**应生成。在这种中间就是各种结果的解析工作，以及调用各种工具完成**任务。值得**一**说的是，我在实验过程中，遇到各种各样的错误， 尤其是第一阶段的任务规划中的任务输出，它对LLM的要求非常高，如果你只是本地的小模型或许是难以胜任的 。第二， 当你有很多工具选择，不妨将他们先进行分类，让LLM先从分类中选择，然后筛选出缩小后具体的工具列表，再次给到LLM选择最优最匹配的工具 。

望君有所得。如果您意犹未尽，想要参与LangChain实战课程，可以点击原文链接查看购买，亲历LLM应用开发之旅。关注我，及时获得更新。

关注点赞在看不迷路，获取即时更新。

### **参考资料**

[1]HuggingGPT: https://huggingface.co/settings/tokens

[2]huggingface上大约有19个任务类型: https://huggingface.co/docs/api-inference/detailed_parameters

[3]HuggingFace API: https://huggingface.co/docs/api-inference/detailed_parameters