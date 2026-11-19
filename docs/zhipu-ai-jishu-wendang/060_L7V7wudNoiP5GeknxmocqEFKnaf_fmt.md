
# CogAgent 模板

> 来源：[https://ai.feishu.cn/docx/F2aJdtisxoqaLXxh654cO2QunXx](https://ai.feishu.cn/docx/F2aJdtisxoqaLXxh654cO2QunXx)

本章节将带领开发者阐述如何使用 `CogAgent` 的模板进行提问。

## 提示词模板

在模型的训练中，我们对以下模板进行了`Agent`增强训练，如果使用这些模板进行提问，将有效的激发模型的`Agent`能力（非 `Function Call`）

```
TASK_EN = [
    &quot;Can you advise me on how to &lt;TASK&gt;?&quot;,
    &quot;I&#x27;m looking for guidance on how to &lt;TASK&gt;.&quot;,
    &quot;What steps do I need to take to &lt;TASK&gt;?&quot;,
    &quot;Could you provide instructions for &lt;TASK&gt;?&quot;,
    &quot;I&#x27;m wondering what the process is for &lt;TASK&gt;.&quot;,
    &quot;How can I go about &lt;TASK&gt;?&quot;,
    &quot;I need assistance with planning to &lt;TASK&gt;.&quot;,
    &quot;Do you have any recommendations for &lt;TASK&gt;?&quot;,
    &quot;Please share some tips for &lt;TASK&gt;.&quot;,
    &quot;I&#x27;d like to know the best way to &lt;TASK&gt;.&quot;,
    &quot;What&#x27;s the most effective way to &lt;TASK&gt;?&quot;,
    &quot;I&#x27;m seeking advice on accomplishing &lt;TASK&gt;.&quot;,
    &quot;Could you guide me through the steps to &lt;TASK&gt;?&quot;,
    &quot;I&#x27;m unsure how to start with &lt;TASK&gt;.&quot;,
    &quot;Is there a strategy for successfully &lt;TASK&gt;?&quot;,
    &quot;What&#x27;s the proper procedure for &lt;TASK&gt;?&quot;,
    &quot;How should I prepare for &lt;TASK&gt;?&quot;,
    &quot;I&#x27;m not sure where to begin with &lt;TASK&gt;.&quot;,
    &quot;I need some insights on &lt;TASK&gt;.&quot;,
    &quot;Can you explain how to tackle &lt;TASK&gt;?&quot;,
    &quot;I&#x27;m interested in the process of &lt;TASK&gt;.&quot;,
    &quot;Could you enlighten me on &lt;TASK&gt;?&quot;,
    &quot;What are the recommended steps for &lt;TASK&gt;?&quot;,
    &quot;Is there a preferred method for &lt;TASK&gt;?&quot;,
    &quot;I&#x27;d appreciate your advice on &lt;TASK&gt;.&quot;,
    &quot;Can you shed light on &lt;TASK&gt;?&quot;,
    &quot;What would be the best approach to &lt;TASK&gt;?&quot;,
    &quot;How do I get started with &lt;TASK&gt;?&quot;,
    &quot;I&#x27;m inquiring about the procedure for &lt;TASK&gt;?&quot;,
    &quot;Could you share your expertise on &lt;TASK&gt;?&quot;,
    &quot;I&#x27;d like some guidance on &lt;TASK&gt;.&quot;,
    &quot;What&#x27;s your recommendation for &lt;TASK&gt;?&quot;,
    &quot;I&#x27;m seeking your input on how to &lt;TASK&gt;?&quot;,
    &quot;Can you provide some insights into &lt;TASK&gt;?&quot;,
    &quot;How can I successfully accomplish &lt;TASK&gt;?&quot;,
    &quot;What steps are involved in &lt;TASK&gt;?&quot;,
    &quot;I&#x27;m curious about the best way to &lt;TASK&gt;.&quot;,
    &quot;Could you show me the ropes for &lt;TASK&gt;?&quot;,
    &quot;I need to know how to go about &lt;TASK&gt;.&quot;,
    &quot;What are the essential steps for &lt;TASK&gt;?&quot;,
    &quot;Is there a specific method for &lt;TASK&gt;?&quot;,
    &quot;I&#x27;d like to get some advice on &lt;TASK&gt;.&quot;,
    &quot;Can you explain the process of &lt;TASK&gt;?&quot;,
    &quot;I&#x27;m looking for guidance on how to approach &lt;TASK&gt;.&quot;,
    &quot;What&#x27;s the proper way to handle &lt;TASK&gt;?&quot;,
    &quot;How should I proceed with &lt;TASK&gt;?&quot;,
    &quot;I&#x27;m interested in your expertise on &lt;TASK&gt;?&quot;,
    &quot;Could you walk me through the steps for &lt;TASK&gt;?&quot;,
    &quot;I&#x27;m not sure where to begin when it comes to &lt;TASK&gt;.&quot;,
    &quot;What should I prioritize when doing &lt;TASK&gt;?&quot;,
    &quot;How can I ensure success with &lt;TASK&gt;?&quot;,
    &quot;I&#x27;d appreciate some tips on &lt;TASK&gt;?&quot;,
    &quot;Can you provide a roadmap for &lt;TASK&gt;?&quot;,
    &quot;What&#x27;s the recommended course of action for &lt;TASK&gt;?&quot;,
    &quot;I&#x27;m seeking your guidance on &lt;TASK&gt;?&quot;,
    &quot;Could you offer some suggestions for &lt;TASK&gt;?&quot;,
    &quot;I&#x27;d like to know the steps to take for &lt;TASK&gt;?&quot;,
    &quot;What&#x27;s the most effective way to achieve &lt;TASK&gt;?&quot;,
    &quot;How can I make the most of &lt;TASK&gt;?&quot;,
    &quot;I&#x27;m wondering about the best approach to &lt;TASK&gt;?&quot;,
    &quot;Can you share your insights on &lt;TASK&gt;?&quot;,
    &quot;What steps should I follow to complete &lt;TASK&gt;?&quot;,
    &quot;I&#x27;m looking for advice on &lt;TASK&gt;?&quot;,
    &quot;What&#x27;s the strategy for successfully completing &lt;TASK&gt;?&quot;,
    &quot;How should I prepare myself for &lt;TASK&gt;?&quot;,
    &quot;I&#x27;m not sure where to start with &lt;TASK&gt;?&quot;,
    &quot;What&#x27;s the procedure for &lt;TASK&gt;?&quot;,
    &quot;Could you provide some guidance on &lt;TASK&gt;?&quot;,
    &quot;I&#x27;d like to get some tips on how to &lt;TASK&gt;?&quot;,
    &quot;Can you explain how to tackle &lt;TASK&gt; step by step?&quot;,
    &quot;I&#x27;m interested in understanding the process of &lt;TASK&gt;?&quot;,
    &quot;What are the key steps to &lt;TASK&gt;?&quot;,
    &quot;Is there a specific method that works for &lt;TASK&gt;?&quot;,
    &quot;I&#x27;d appreciate your advice on successfully completing &lt;TASK&gt;?&quot;,
    &quot;Can you shed light on the best way to &lt;TASK&gt;?&quot;,
    &quot;What would you recommend as the first step to &lt;TASK&gt;?&quot;,
    &quot;How do I initiate &lt;TASK&gt;?&quot;,
    &quot;I&#x27;m inquiring about the recommended steps for &lt;TASK&gt;?&quot;,
    &quot;Could you share some insights into &lt;TASK&gt;?&quot;,
    &quot;I&#x27;m seeking your expertise on &lt;TASK&gt;?&quot;,
    &quot;What&#x27;s your recommended approach for &lt;TASK&gt;?&quot;,
    &quot;I&#x27;d like some guidance on where to start with &lt;TASK&gt;?&quot;,
    &quot;Can you provide recommendations for &lt;TASK&gt;?&quot;,
    &quot;What&#x27;s your advice for someone looking to &lt;TASK&gt;?&quot;,
    &quot;I&#x27;m seeking your input on the process of &lt;TASK&gt;?&quot;,
    &quot;How can I achieve success with &lt;TASK&gt;?&quot;,
    &quot;What&#x27;s the best way to navigate &lt;TASK&gt;?&quot;,
    &quot;I&#x27;m curious about the steps required for &lt;TASK&gt;?&quot;,
    &quot;Could you show me the proper way to &lt;TASK&gt;?&quot;,
    &quot;I need to know the necessary steps for &lt;TASK&gt;?&quot;,
    &quot;What&#x27;s the most efficient method for &lt;TASK&gt;?&quot;,
    &quot;I&#x27;d appreciate your guidance on &lt;TASK&gt;?&quot;,
    &quot;Can you explain the steps involved in &lt;TASK&gt;?&quot;,
    &quot;I&#x27;m looking for recommendations on how to approach &lt;TASK&gt;?&quot;,
    &quot;What&#x27;s the right way to handle &lt;TASK&gt;?&quot;,
    &quot;How should I manage &lt;TASK&gt;?&quot;,
    &quot;I&#x27;m interested in your insights on &lt;TASK&gt;?&quot;,
    &quot;Could you provide a step-by-step guide for &lt;TASK&gt;?&quot;,
    &quot;I&#x27;m not sure how to start when it comes to &lt;TASK&gt;?&quot;,
    &quot;What are the key factors to consider for &lt;TASK&gt;?&quot;,
    &quot;How can I ensure a successful outcome with &lt;TASK&gt;?&quot;,
    &quot;I&#x27;d like some tips and tricks for &lt;TASK&gt;?&quot;,
    &quot;Can you offer a roadmap for accomplishing &lt;TASK&gt;?&quot;,
    &quot;What&#x27;s the preferred course of action for &lt;TASK&gt;?&quot;,
    &quot;I&#x27;m seeking your expert advice on &lt;TASK&gt;?&quot;,
    &quot;Could you suggest some best practices for &lt;TASK&gt;?&quot;,
    &quot;I&#x27;d like to understand the necessary steps to complete &lt;TASK&gt;?&quot;,
    &quot;What&#x27;s the most effective strategy for &lt;TASK&gt;?&quot;,
]
```

在这份模板中，`<TASK>` 是用户在问题中提到的任务。

## 使用模板提出问题

### 模板格式

用户应该按照以下公式设计提示词模板，以获最好的执行效果。

`模板 + <Task> + (with grouding)`

其中：

+   模板：上述107种模板中的其中一种

+   `<Task>`: 用户的任务

+   `(with grouding)`: 可选,如果希望模型输出视觉位置信息，则在提示词最后加上 `(with grouding)`字段。

如果用户使用我们模板中的格式，将会获得更好的回答效果，例如，使用以下两种方式提出同一个问题:

+   按照模板提问进行提问



```
Human: 
I'm looking for guidance on how to search the CogVLM arxiv paper

CogAgent:
Follow the Plan: 
1. Click on the search bar. 
2. Type 'CogVLM arxiv'. 
3. Press enter or click the search icon.
Next Action: 
Move the cursor to the search bar and click on it to activate the text entry mode.
```

在这里，用户的提示词使用了`I'm looking for guidance on how to <TASK>`这个模板，其中，`<Task>` 任务是 `search the CogVLM arxiv paper`。

我们可以观察到，`CogAgent` 给得出的回答非常具有步骤性，并且回答完全正确。

其中，关键词 `Next Action` 可用于更好的被中间件框架捕获，实现更多的 `Agent` 任务。

+   没有按照模板进行提问

```
Human: 
Find CogVLM arxiv paper?
CogAgent:
Click on the search bar. and type 'CogVLM arxiv paper'
Next Action: press enter to search. 
```

虽然模型也能进行正确的回答，但是明显缺乏有条理性，效果没有使用了模板的提问更强。

因此，如果您基于 `Langchain`，`AutoGen` 或者其他 `Agent` 任务框架的时，需要通过捕捉格式化输出的时，使用符合模板的提问更能获得格式化的输出。

## 特殊字段和格式化

在模型训练的时候，没有任何 `Special token` 用于特殊截断，这意味着无法使用这种检测方法来检测模型是否进入了 `Observation` 或者 `Action` 的状态。

事实上，模型也不具备完整思维链的能力，无法形成类似 `ChatGLM3` 的完整工具调用。

基于我们的训练方法，模型的输出是具有规律的，用户可以根据以下关键词设定自己的行动方案：

`Plan`:

`Next Action`: 这代表模型给出的下一步行动

`Grounded Operation`: 大模型给出的需要行动的检测目标位置

-> 用户需要执行的操作，分为电脑和手机两种方式

+   电脑：坐标 `[x1,y1,x2,y2]`，用户应该在这个范围内的任何一个坐标执行动作即可。

+   手机：触摸点 `[x,y]`

## 示例

在这个示例中，用户想在 `google` 中搜索 `CogVLM` 的论文。让我们来根据这个连续对话例子来解析输出。展现 `CogAgent` 是如何与用户进行交互的。

### 第一轮对话

图片：

![](img/ec80d1efd1ed7c0415baea56233f0f21.png)

```
Human: 
I'm looking for guidance on how to search the CogVLM arxiv paper (with grouding) 
```

```
Plan: 
1. Type the paper title 'CogVLM' into the search bar. 
2. Review the search results for the correct paper. 
3. Click on the paper link to access the full text. 
4. If needed, use the provided search tools to further refine the search or understand the paper's details.
Next Action: 
Move the cursor to the search bar coordinates given by {'x_min': 0.25, 'y_min': 0.524, 'width': 0.553, 'height': 0.078} and type 'CogVLM' into the Google search field.
Grounded Operation:
[combobox]  Google -> TYPE: CogVLM at the box [[250,524,703,602]]
```

其中：

`Plan`: 模型希望用户按照这些操作执行获得结果：

1. 在搜索栏输入 `CogVLM`

1. 查看搜索的向相关结果

1. 进入正确的链接并查看论文

1. 查看更多细节

`Next Action`: 要完成 `Plan` 中的所有计划，你需要在搜索框输入 `cogvlm` 然后在 `[[250,524,703,602]]` 上按下鼠标搜索

### 第二轮对话

图片：

![](img/b52cceaf7a2cffa36ea159ad61c907ce.png)

```
Human: 
I have entered CogVLM in the input box as required, What steps do I need to take to search the CogVLM arxiv paper (with grounding)
```

```
Plan: 
1. Once the paper is open, read through the abstract to confirm it is the desired paper. 
2. If the paper is relevant, save or take note of the details for further review or use. 
3. If the paper is not relevant, return to the search results and repeat the process with other papers until the correct one is found.
Next Action: 
Move the cursor to the box with coordinates {'x_min': 0.138, 'y_min': 0.29, 'width': 0.537, 'height': 0.053} which contains the title 'THUDM/CogVLM: a state-of-the-art-level open visual ...' and perform a click operation.
Grounded Operation:[heading]  CogVLM: Visual Expert for Pretrained Language Models ... -> CLICK at the box [[138,290,675,343]]
```

`Plan`: 模型希望用户按照这些操作执行

1. 选择可能正确的页面并打开

1. 获得下载链接

`Next Action`: 要完成 `Plan` 中的所有计划，你应该点击第二个选项 `CogVLM: Visual Expert for Pretrained Language Models`

### 第三轮对话

图片：

![](img/42c9562d6daf58a701a8f947f6e088e9.png)

```
Human: 
I followed your instructions, What steps do I need to take to search the CogVLM arxiv paper (with grounding)
```

```
Plan: 

1. You have enter the right page. After clicking on the 'Download PDF' link, wait for the download manager or browser to initiate the download. 
2. If a confirmation dialog appears, confirm the download. 
3. Wait for the PDF to download and save it to the designated location on the computer.
Next Action: 
Move the cursor to the 'Download PDF' link located at the box with coordinates {'x_min': 0.84, 'y_min': 0.152, 'width': 0.128, 'height': 0.019} on the screen and perform a click operation.
Grounded Operation:[link]  Download PDF -> CLICK at the box [[840,152,968,171]]
```

`Plan`: 用户已经成功访问论文的目录地址

1. 确认是否要下载

1. 如果是，则等待下载成功就行

`Next Action`: 点击 `Download PDF` 按键

## 注意事项

1. 本模型一次只能对一个图片进行处理，不能同时处理多张图片。

1. 由于每次处理一张新的图片，之前的聊天信息会被删除，相当于没有进行多轮对话，而是多个单轮对话，因此，需要传入的提示词包括完整的连续对话信息。

1. 本模型没有 执行功能，如果你希望执行点击，输入等动作，你需要自己适配对应的执行工具。
