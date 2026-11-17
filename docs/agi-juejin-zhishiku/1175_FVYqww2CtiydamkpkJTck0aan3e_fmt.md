
# LLM用于多模态任务系列一：HuggingGPT

> 来源：https://agijuejin.feishu.cn/docx/OszIdVrAuoZfGhxCygEcfxiCnze

> 作者：Meta

> 原文：https://zhuanlan.zhihu.com/p/621302688

[《LLM用于多模态任务系列一：HuggingGPT》](https://www.yuque.com/meta95/hmc3l4/hozcxokgd9f7d3n1?singleDoc#)

## 论文解读

[HuggingGPT.pdf](https://link.zhihu.com/?target=https%3A//www.yuque.com/attachments/yuque/0/2023/pdf/1570749/1681230412905-651788a3-001b-404b-be26-ea9619af72c4.pdf)

本文介绍了一种名为HuggingGPT的框架，该框架旨在通过将大型语言模型作为控制器来管理现有的人工智能模型以解决复杂的人工智能任务。

作者认为，由于大型语言模型在语言理解、生成、交互和推理方面表现出了卓越的能力，因此语言可以成为通用接口来实现这一目标。然而，由于不同领域和模态的人工智能任务需要使用不同的模型和算法，因此如何有效地选择和组合这些模型是一个具有挑战性的问题。

本文提出的HuggingGPT框架主要分为四个步骤：任务规划、模型选择、执行子任务和结果汇总。

![](img/b7db52521514a6f1541f566387f53c52.png)

在接收用户请求时，使用ChatGPT作为语言理解模型，将用户请求转换为机器可处理的格式，并将其传递给任务规划模块。任务规划模块使用ChatGPT分析用户请求，理解其意图，并将其分解成多个子任务。具体就是通过采用合适的`prompt`来实现，给出任务描述+预先设定的任务范围+用例+当前输入，让chatgpt完成任务分解。

![](img/fb51a6bbe66fca7d8e3705daebdc972d.png)

对于每个子任务，模型选择模块根据其功能描述从Hugging Face中选择最适合的人工智能模型。也是采用合适的`prompt`，给出任务描述+各个模型及其功能描述，让chatgpt自动选择模型。

选定所需的人工智能模型后，执行子任务模块使用所选的人工智能模型执行每个子任务，并将结果返回给ChatGPT。

最后，ChatGPT根据执行结果汇总所有子任务的结果并生成最终响应，然后将响应返回给用户。

HuggingGPT框架的优点在于它可以自动选择最合适的人工智能模型来完成不同领域和模态的人工智能任务。通过使用大型语言模型作为控制器，HuggingGPT框架可以有效地解决不同领域和模态的人工智能任务之间的差异性问题。此外，HuggingGPT框架还可以方便地集成不同领域和模态的人工智能模型，而无需修改任何结构或提示设置。这种开放和连续的方式使得HuggingGPT框架更加灵活和可扩展，从而为实现更先进的人工智能提供了一步之遥。

然而，该论文也存在一些局限性。

效率问题。效率的瓶颈在于大型语言模型的推理，对于每一轮用户请求，HuggingGPT在任务规划、模型选择和响应生成阶段都至少需要与大型语言模型进行一次交互。这些交互大大增加了响应延迟，导致用户体验下降。

最大上下文长度的限制。由于LLM可以接受的token数量的最大值有限，HuggingGPT也面临着最大上下文长度的限制。我们在任务规划阶段使用了会话窗口，只跟踪对话上下文以缓解这个问题。

系统稳定性问题，主要表现在两个方面：

一是在大型语言模型推理过程中出现的叛逆行为。大型语言模型偶尔无法遵守指令，推断出的输出格式可能与预期不符，导致程序工作流程中的异常。

二是Hugging Face推理端点上托管的专家模型的不可控状态。Hugging Face上的专家模型可能会受到网络延迟或服务状态的影响，导致任务执行阶段出现错误。

总的来说，HuggingGPT框架是一种有前途的方法来解决复杂的人工智能任务，并为实现更先进的人工智能提供了新思路。未来研究可以进一步探索如何优化该框架以提高其准确性、效率和可扩展性，并将其应用于更广泛的领域和场景中。

## 试用

代码：https://github.com/microsoft/JARVIS

### case1: tell me how many zebras in the picture of `/examples/c.jpg`

![](img/388f49de8d5629aa48bb9af28571efb1.png)

+ 任务编排

+ `prompt` `1-#1 Task Planning Stage.json`



# Task Planning Stage

The AI assistant can parse user input to several tasks: `[{"task": task, "id": task_id, "dep": dependency_task_id, "args": {"text": text or `<GENERATED>-dep_id`, "image": `image_url` or `<GENERATED>-dep_id`, "audio": `audio_url` or `<GENERATED>-dep_id}}]`.

The special tag `<GENERATED>-dep_id` refers to the one generated text/image/audio in the dependency task (please consider whether the dependency task generates resources of this type), and `dep_id` must be in the `dep` list. The `dep` field denotes the IDs of the previous prerequisite tasks which generate a new resource that the current task relies on. The `args` field must be in `["text", "image", "audio"]`, nothing else.

The task MUST be selected from the following options: `token-classification`, `text2text-generation`, `summarization`, `translation`, `question-answering`, `conversational`, `text-generation`, `sentence-similarity`, `tabular-classification`, `object-detection`, `image-classification`, `image-to-image`, `image-to-text`, `text-to-image`, `text-to-video`, `visual-question-answering`, `document-question-answering`, `image-segmentation`, `depth-estimation`, `text-to-speech`, `automatic-speech-recognition`, `audio-to-audio`, `audio-classification`, `canny-control`, `hed-control`, `mlsd-control`, `normal-control`, `openpose-control`, `canny-text-to-image`, `depth-text-to-image`, `hed-text-to-image`, `mlsd-text-to-image`, `normal-text-to-image`, `openpose-text-to-image`, `seg-text-to-image`. There may be multiple tasks of the same type.

Think step by step about all the tasks needed to resolve the user's request. Parse out as few tasks as possible while ensuring that the user request can be resolved. Pay attention to the dependencies and order among tasks. If the user input can't be parsed, you need to reply empty JSON `[]`.

## Examples

```
<im_start>user
Give you some pictures e1.jpg, e2.png, e3.jpg, help me count the number of sheep?<im_end>
<im_start>assistant
[{"task": "image-to-text", "id": 0, "dep": [-1], "args": {"image": "e1.jpg"}}, {"task": "object-detection", "id": 1, "dep": [-1], "args": {"image": "e1.jpg"}}, {"task": "visual-question-answering", "id": 2, "dep": [1], "args": {"image": "<GENERATED>-1", "text": "How many sheep in the picture"}}], {"task": "image-to-text", "id": 3, "dep": [-1], "args": {"image": "e2.png"}}, {"task": "object-detection", "id": 4, "dep": [-1], "args": {"image": "e2.png"}}, {"task": "visual-question-answering", "id": 5, "dep": [4], "args": {"image": "<GENERATED>-4", "text": "How many sheep in the picture"}}], {"task": "image-to-text", "id": 6, "dep": [-1], "args": {"image": "e3.jpg"}},  {"task": "object-detection", "id": 7, "dep": [-1], "args": {"image": "e3.jpg"}}, {"task": "visual-question-answering", "id": 8, "dep": [7], "args": {"image": "<GENERATED>-7", "text": "How many sheep in the picture"}}]<im_end>
```

```
<im_start>user
Look at /e.jpg, can you tell me how many objects in the picture? Give me a picture and video similar to this one.<im_end>
<im_start>assistant
[{"task": "image-to-text", "id": 0, "dep": [-1], "args": {"image": "/e.jpg"}}, {"task": "object-detection", "id": 1, "dep": [-1], "args": {"image": "/e.jpg"}}, {"task": "visual-question-answering", "id": 2, "dep": [1], "args": {"image": "<GENERATED>-1", "text": "how many objects in the picture?"}}, {"task": "text-to-image", "id": 3, "dep": [0], "args": {"text": "<GENERATED-0>"}}, {"task": "image-to-image", "id": 4, "dep": [-1], "args": {"image": "/e.jpg"}}, {"task": "text-to-video", "id": 5, "dep": [0], "args": {"text": "<GENERATED-0>"}}]<im_end>
```

```
<im_start>user
given a document /images/e.jpeg, answer me what is the student amount? And describe the image with your voice<im_end>
<im_start>assistant
{"task": "document-question-answering", "id": 0, "dep": [-1], "args": {"image": "/images/e.jpeg", "text": "what is the student amount?"}}, {"task": "visual-question-answering", "id": 1, "dep": [-1], "args": {"image": "/images/e.jpeg", "text": "what is the student amount?"}}, {"task": "image-to-text", "id": 2, "dep": [-1], "args": {"image": "/images/e.jpg"}}, {"task": "text-to-speech", "id": 3, "dep": [2], "args": {"text": "<GENERATED>-2"}}]<im_end>
```

```
<im_start>user
Given an image /example.jpg, first generate a hed image, then based on the hed image generate a new image where a girl is reading a book<im_end>
<im_start>assistant
[{"task": "openpose-control", "id": 0, "dep": [-1], "args": {"image": "/example.jpg"}},  {"task": "openpose-text-to-image", "id": 1, "dep": [0], "args": {"text": "a girl is reading a book", "image": "<GENERATED>-0"}}]<im_end>
```

```
<im_start>user
please show me a video and an image of (based on the text) 'a boy is running' and dub it<im_end>
<im_start>assistant
[{"task": "text-to-video", "id": 0, "dep": [-1], "args": {"text": "a boy is running"}}, {"task": "text-to-speech", "id": 1, "dep": [-1], "args": {"text": "a boy is running"}}, {"task": "text-to-image", "id": 2, "dep": [-1], "args": {"text": "a boy is running"}}]<im_end>
```

```
<im_start>user
please show me a joke and an image of cat<im_end>
<im_start>assistant
[{"task": "conversational", "id": 0, "dep": [-1], "args": {"text": "please show me a joke of cat"}}, {"task": "text-to-image", "id": 1, "dep": [-1], "args": {"text": "a photo of cat"}}]<im_end>
```

```
<im_start>user
The chat log [ [] ] may contain the resources I mentioned. Now I input { tell me how many zebras in the picture of /examples/c.jpg }. Pay attention to the input and output types of tasks and the dependencies between tasks.<im_end>
<im_start>assistant
```



# 输入的 prompt 包括三个部分

输入的`prompt`包括三个部分：任务描述+示例+当前任务

## 第一阶段：任务规划

`chatgpt` 结果 [`1-#1 Task Planning`](https://link.zhihu.com/?target=https%3A//www.yuque.com/attachments/yuque/0/2023/json/1570749/1681229034061-ae58b4c3-7e7b-4e36-aaba-ece0e057d62f.json) `Stage-response.json`

`\n[{"task": "image-to-text", "id": 0, "dep": [-1], "args": {"image": "/examples/c.jpg"}}, {"task": "object-detection", "id": 1, "dep": [-1], "args": {"image": "/examples/c.jpg"}}, {"task": "visual-question-answering", "id": 2, "dep": [1], "args": {"image": "<GENERATED>-1", "text": "how many zebras in the picture?"}}]` 根据任务描述拆分为 3 个 `task`

![](img/e3f580ea1c7e1aa0b935f0afafef1ca8.png)

## 第二阶段：模型选择

### 选择 `image-to-text` 模型

`prompt 2-#2 Model Selection Stage.json`

```
#2 Model Selection Stage: Given the user request and the parsed tasks, the AI assistant helps the user to select a suitable model from a list of models to process the user request. The assistant should focus more on the description of the model and find the model that has the most potential to solve requests and tasks. Also, prefer models with local inference endpoints for speed and stability.&lt;im_start&gt;user\ntell me how many zebras in the picture of /examples/c.jpg&lt;im_end&gt;\n&lt;im_start&gt;assistant\n{&#x27;task&#x27;: &#x27;image-to-text&#x27;, &#x27;id&#x27;: 0, &#x27;dep&#x27;: [-1], &#x27;args&#x27;: {&#x27;image&#x27;: &#x27;public//examples/c.jpg&#x27;}}&lt;im_end&gt;\n&lt;im_start&gt;user\nPlease choose the most suitable model from [{&#x27;id&#x27;: &#x27;nlpconnect/vit-gpt2-image-captioning&#x27;, &#x27;inference endpoint&#x27;: [&#x27;Salesforce/blip2-opt-2.7b&#x27;, &#x27;nlpconnect/vit-gpt2-image-captioning&#x27;, &#x27;naver-clova-ix/donut-base&#x27;, &#x27;Salesforce/blip-image-captioning-base&#x27;], &#x27;likes&#x27;: 219, &#x27;description&#x27;: &#x27;\\n\\n# nlpconnect/vit-gpt2-image-captioning\\n\\nThis is an image captioning model trained by @ydshieh in [&#x27;, &#x27;tags&#x27;: [&#x27;image-to-text&#x27;, &#x27;image-captioning&#x27;]}, {&#x27;id&#x27;: &#x27;Salesforce/blip-image-captioning-base&#x27;, &#x27;inference endpoint&#x27;: [&#x27;Salesforce/blip2-opt-2.7b&#x27;, &#x27;nlpconnect/vit-gpt2-image-captioning&#x27;, &#x27;naver-clova-ix/donut-base&#x27;, &#x27;Salesforce/blip-image-captioning-base&#x27;], &#x27;likes&#x27;: 44, &#x27;description&#x27;: &#x27;\\n\\n# BLIP: Bootstrapping Language-Image Pre-training for Unified Vision-Language Understanding and Ge&#x27;, &#x27;tags&#x27;: [&#x27;image-captioning&#x27;]}, {&#x27;id&#x27;: &#x27;naver-clova-ix/donut-base&#x27;, &#x27;inference endpoint&#x27;: [&#x27;Salesforce/blip2-opt-2.7b&#x27;, &#x27;nlpconnect/vit-gpt2-image-captioning&#x27;, &#x27;naver-clova-ix/donut-base&#x27;, &#x27;Salesforce/blip-image-captioning-base&#x27;], &#x27;likes&#x27;: 36, &#x27;description&#x27;: &#x27;\\n\\n# Donut (base-sized model, pre-trained only) \\n\\nDonut model pre-trained-only. It was introduced in &#x27;, &#x27;tags&#x27;: [&#x27;donut&#x27;, &#x27;image-to-text&#x27;, &#x27;vision&#x27;]}, {&#x27;id&#x27;: &#x27;Salesforce/blip2-opt-2.7b&#x27;, &#x27;inference endpoint&#x27;: [&#x27;Salesforce/blip2-opt-2.7b&#x27;, &#x27;nlpconnect/vit-gpt2-image-captioning&#x27;, &#x27;naver-clova-ix/donut-base&#x27;, &#x27;Salesforce/blip-image-captioning-base&#x27;], &#x27;likes&#x27;: 25, &#x27;description&#x27;: &#x27;\\n\\n# BLIP-2, OPT-2.7b, pre-trained only\\n\\nBLIP-2 model, leveraging [OPT-2.7b](https://huggingface.co/f&#x27;, &#x27;tags&#x27;: [&#x27;vision&#x27;, &#x27;image-to-text&#x27;, &#x27;image-captioning&#x27;, &#x27;visual-question-answering&#x27;]}] for the task {&#x27;task&#x27;: &#x27;image-to-text&#x27;, &#x27;id&#x27;: 0, &#x27;dep&#x27;: [-1], &#x27;args&#x27;: {&#x27;image&#x27;: &#x27;public//examples/c.jpg&#x27;}}. The output must be in a strict JSON format: {\&quot;id\&quot;: \&quot;id\&quot;, \&quot;reason\&quot;: \&quot;your detail reasons for the choice\&quot;}.&lt;im_end&gt;\n&lt;im_start&gt;assistant
```

`chatgpt` 结果 [`2-#2 Model Selection`](https://link.zhihu.com/?target=https%3A//www.yuque.com/attachments/yuque/0/2023/json/1570749/1681229509924-94b91aff-a4c5-4a6e-9066-8dcd4504ee20.json) `Stage-response.json`，`chatgpt` 选择了 `nlpconnect/vit-gpt2-image-captioning`

### 选择 `object-detection` 模型

`prompt 3-#2 Model Selection Stage.json`

```
#2 Model Selection Stage: Given the user request and the parsed tasks, the AI assistant helps the user to select a suitable model from a list of models to process the user request. The assistant should focus more on the description of the model and find the model that has the most potential to solve requests and tasks. Also, prefer models with local inference endpoints for speed and stability.&lt;im_start&gt;user\ntell me how many zebras in the picture of /examples/c.jpg&lt;im_end&gt;\n&lt;im_start&gt;assistant\n{&#x27;task&#x27;: &#x27;object-detection&#x27;, &#x27;id&#x27;: 1, &#x27;dep&#x27;: [-1], &#x27;args&#x27;: {&#x27;image&#x27;: &#x27;public//examples/c.jpg&#x27;}}&lt;im_end&gt;\n&lt;im_start&gt;user\nPlease choose the most suitable model from [{&#x27;id&#x27;: &#x27;facebook/detr-resnet-50&#x27;, &#x27;inference endpoint&#x27;: [&#x27;facebook/detr-resnet-50&#x27;, &#x27;microsoft/table-transformer-structure-recognition&#x27;, &#x27;facebook/detr-resnet-101&#x27;], &#x27;likes&#x27;: 129, &#x27;description&#x27;: &#x27;\\n\\n# DETR (End-to-End Object Detection) model with ResNet-50 backbone\\n\\nDEtection TRansformer (DETR) m&#x27;, &#x27;tags&#x27;: [&#x27;object-detection&#x27;, &#x27;vision&#x27;]}, {&#x27;id&#x27;: &#x27;microsoft/table-transformer-structure-recognition&#x27;, &#x27;inference endpoint&#x27;: [&#x27;facebook/detr-resnet-50&#x27;, &#x27;microsoft/table-transformer-structure-recognition&#x27;, &#x27;facebook/detr-resnet-101&#x27;], &#x27;likes&#x27;: 31, &#x27;description&#x27;: &#x27;\\n\\n# Table Transformer (fine-tuned for Table Structure Recognition) \\n\\nTable Transformer (DETR) model &#x27;, &#x27;tags&#x27;: None}, {&#x27;id&#x27;: &#x27;facebook/detr-resnet-101&#x27;, &#x27;inference endpoint&#x27;: [&#x27;facebook/detr-resnet-50&#x27;, &#x27;microsoft/table-transformer-structure-recognition&#x27;, &#x27;facebook/detr-resnet-101&#x27;], &#x27;likes&#x27;: 30, &#x27;description&#x27;: &#x27;\\n\\n# DETR (End-to-End Object Detection) model with ResNet-101 backbone\\n\\nDEtection TRansformer (DETR) &#x27;, &#x27;tags&#x27;: [&#x27;object-detection&#x27;, &#x27;vision&#x27;]}] for the task {&#x27;task&#x27;: &#x27;object-detection&#x27;, &#x27;id&#x27;: 1, &#x27;dep&#x27;: [-1], &#x27;args&#x27;: {&#x27;image&#x27;: &#x27;public//examples/c.jpg&#x27;}}. The output must be in a strict JSON format: {\&quot;id\&quot;: \&quot;id\&quot;, \&quot;reason\&quot;: \&quot;your detail reasons for the choice\&quot;}.&lt;im_end&gt;\n&lt;im_start&gt;assistant
```

`chatgpt` 结果 [`3-#2 Model Selection`](https://link.zhihu.com/?target=https%3A//www.yuque.com/attachments/yuque/0/2023/json/1570749/1681229700665-ff6cf9f1-1239-4a0c-8767-0a9053661046.json) `Stage-response.json`

### 选择 `visual-question-answering` 模型

由于针对这一任务只有一个备选模型，直接选择 `dandelin/vilt-b32-finetuned-vqa`

## 第三阶段：任务执行

分别调用子任务的模型获得结果

## 第四阶段：答案编排

`prompt` [`4-#4 Response Generation Stage.json`](https://link.zhihu.com/?target=https%3A//www.yuque.com/attachments/yuque/0/2023/json/1570749/1681230007320-a0b03200-57de-4bfc-b14a-c8be009b3c58.json) 将 `task` 编排结果、模型执行结果作为 `prompt`，要求 `chatgpt` 整合输出



# 4 Response Generation Stage

```
#4 Response Generation Stage: With the task execution logs, the AI assistant needs to describe the process and inference results.<im_start>user
tell me how many zebras in the picture of /examples/c.jpg<im_end>
<im_start>assistant
Before give you a response, I want to introduce my workflow for your request, which is shown in the following JSON data: [{'task': {'task': 'image-to-text', 'id': 0, 'dep': [-1], 'args': {'image': 'public//examples/c.jpg'}}, 'inference result': {'generated text': 'two zebras standing in a field of dry grass '}, 'choose model result': {'id': 'nlpconnect/vit-gpt2-image-captioning', 'reason': 'This model is best suited for the task of image-to-text as it has the highest number of likes and is specifically designed for this task. It also has a local inference endpoint which makes it faster and more stable'}}, {'task': {'task': 'object-detection', 'id': 1, 'dep': [-1], 'args': {'image': 'public//examples/c.jpg'}}, 'inference result': {'generated image with predicted box': '/images/8092.jpg', 'predicted': [{'score': 0.9968145489692688, 'label': 'zebra', 'box': {'xmin': 491, 'ymin': 140, 'xmax': 658, 'ymax': 482}}, {'score': 0.9991269707679749, 'label': 'zebra', 'box': {'xmin': 35, 'ymin': 116, 'xmax': 344, 'ymax': 483}}, {'score': 0.9980229139328003, 'label': 'zebra', 'box': {'xmin': 317, 'ymin': 73, 'xmax': 541, 'ymax': 482}}]}, 'choose model result': {'id': 'facebook/detr-resnet-50', 'reason': 'This model is best suited for this task as it has a ResNet-50 backbone and is specifically designed for this purpose. It also has the most likes and the most detailed description, making it the best choice'}}, {'task': {'task': 'visual-question-answering', 'id': 2, 'dep': [1], 'args': {'image': 'public//examples/c.jpg', 'text': 'how many zebras in the picture?'}}, 'inference result': [{'score': 0.6324987411499023, 'answer': '2'}, {'score': 0.4428333342075348, 'answer': '3'}, {'score': 0.02447054162621498, 'answer': '4'}, {'score': 0.0017950749024748802, 'answer': 'yes'}, {'score': 0.0013340209843590856, 'answer': '5'}], 'choose model result': {'id': 'dandelin/vilt-b32-finetuned-vqa', 'reason': 'Only one model available.'}}]. Do you have any demands regarding my response?<im_end>
<im_start>user
Yes. Please first think carefully and directly answer my request based on the inference results. Some of the inferences may not always turn out to be correct and require you to make careful consideration in making decisions. Then please detail your workflow including the used models and inference results for my request in your friendly tone. Please filter out information that is not relevant to my request. Tell me the complete path or urls of files in inference results. If there is nothing in the results, please tell me you can't make it. }<im_end>
<im_start>assistant
```

+ chatgpt回答 [4-#4 Response Generation](https://link.zhihu.com/?target=https%3A//www.yuque.com/attachments/yuque/0/2023/json/1570749/1681230200124-41129145-c73e-4e30-8394-3c466b877a84.json) `Stage-response.json`

## 答案

Based on inference results, there are two zebras in the picture of `/examples/c.jpg`. The workflow I used to answer your request includes three tasks: `image-to-text`, `object-detection`, and `visual-question-answering`. For the `image-to-text` task, I used the model `nlpconnect/vit-gpt2-image-captioning`, which is best suited for this task as it has the highest number of likes and is specifically designed for this task. It also has a local inference endpoint which makes it faster and more stable. For the `object-detection` task, I used the model `facebook/detr-resnet-50`, which is best suited for this task as it has a `ResNet-50` backbone and is specifically designed for this purpose. It also has the most likes and the most detailed description, making it the best choice. For the `visual-question-answering` task, I used the model `dandelin/vilt-b32-finetuned-vqa`, which is the only model available. The inference result for this task is that there are two zebras in the picture. The complete path or urls of files in inference results are `/images/8092.jpg` and `public//examples/c.jpg`. Does this answer your question？

![](img/e165ff85827c971e85d9b3398f0a80b8.png)
