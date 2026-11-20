
# CogAgent 提示词模板使用指南

如果用户使用我们模板中的格式，将会获得更好的回答效果，例如，使用以下两种方式提出同一个问题：

## 按照模板提问进行提问

```
Human: 
I&#x27;m looking for guidance on how to search the CogVLM arxiv paper

CogAgent:
Follow the Plan: 
1. Click on the search bar. 
2. Type &#x27;CogVLM arxiv&#x27;. 
3. Press enter or click the search icon.
Next Action: 
Move the cursor to the search bar and click on it to activate the text entry mode.
```

在这里，用户的提示词使用了 `` `<TASK>` `` 这个模板，其中，`` `<Task>` `` 任务是 `search the CogVLM arxiv paper`。

我们可以观察到，`CogAgent` 给得出的回答非常具有步骤性，并且回答完全正确。

其中，关键词 `Next Action` 可用于更好的被中间件框架捕获，实现更多的 `Agent` 任务。

## 没有按照模板进行提问

```
Human: 
Find CogVLM arxiv paper?
CogAgent:
Click on the search bar. and type &#x27;CogVLM arxiv paper&#x27;
Next Action: press enter to search. 
```

虽然模型也能进行正确的回答，但是明显缺乏有条理性，效果没有使用了模板的提问更强。

因此，如果您基于 `Langchain`、`AutoGen` 或者其他 `Agent` 任务框架时，需要通过捕捉格式化输出时，使用符合模板的提问更能获得格式化的输出。

## 特殊字段和格式化

在模型训练的时候，没有任何 `Special token` 用于特殊截断，这意味着无法使用这种检测方法来检测模型是否进入了 `Observation` 或者 `Action` 的状态。

事实上，模型也不具备完整思维链的能力，无法形成类似 `ChatGLM3` 的完整工具调用。

基于我们的训练方法，模型的输出是具有规律的，用户可以根据以下关键词设定自己的行动方案：

`Plan`:
`Next Action`: 这代表模型给出的下一步行动
`Grounded Operation`: 大模型给出的需要行动的检测目标位置

-> 用户需要执行的操作，分为电脑和手机两种方式：

+ 电脑：坐标 `[x1,y1,x2,y2]`，用户应该在这个范围内的任何一个坐标执行动作即可。
+ 手机：触摸点 `[x,y]`

## 示例

在这个示例中，用户想在 google 中搜索 `CogVLM` 的论文。让我们来根据这个连续对话例子来解析输出。展现 `CogAgent` 是如何与用户进行交互的。

### 第一轮对话

图片：

![](img/be9e34bbfb72c3ea677a08fdcea4c148.png)

```
Human: 
I&#x27;m looking for guidance on how to search the CogVLM arxiv paper (with grouding) 
```

```
Plan: 
1. Type the paper title &#x27;CogVLM&#x27; into the search bar. 
2. Review the search results for the correct paper. 
3. Click on the paper link to access the full text. 
4. If needed, use the provided search tools to further refine the search or understand the paper&#x27;s details.
Next Action: 
Move the cursor to the search bar coordinates given by {&#x27;x_min&#x27;: 0.25, &#x27;y_min&#x27;: 0.524, &#x27;width&#x27;: 0.553, &#x27;height&#x27;: 0.078} and type &#x27;CogVLM&#x27; into the Google search field.
Grounded Operation:
[combobox]  Google -> TYPE: CogVLM at the box [[250,524,703,602]]
```

其中：

`Plan`: 模型希望用户按照这些操作执行获得结果：

1. 在搜索栏输入 CogVLM
1. 查看搜索的向相关结果
1. 进入正确的链接并查看论文
1. 查看更多细节

`Next Action`: 要完成 `Plan` 中的所有计划，你需要在搜索框输入 cogvlm 然后在 `[[250,524,703,602]]` 上按下鼠标搜索

### 第二轮对话

图片：

![](img/b5b170e885db6b031aa461c81d8822f3.png)

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
Move the cursor to the box with coordinates {&#x27;x_min&#x27;: 0.138, &#x27;y_min&#x27;: 0.29, &#x27;width&#x27;: 0.537, &#x27;height&#x27;: 0.053} which contains the title &#x27;THUDM/CogVLM: a state-of-the-art-level open visual ...&#x27; and perform a click operation.
Grounded Operation:[heading]  CogVLM: Visual Expert for Pretrained Language Models ... -> CLICK at the box [[138,290,675,343]]
```

`Plan`: 模型希望用户按照这些操作执行

1. 选择可能正确的页面并打开
1. 获得下载链接

`Next Action`: 要完成 `Plan` 中的所有计划，你应该点击第二个选项 `CogVLM: Visual Expert for Pretrained Language Models`

### 第三轮对话

图片：

![](img/bc4d567247d665644915fbaf2e04eefd.png)

```
Human: 
I followed your instructions, What steps do I need to take to search the CogVLM arxiv paper (with grounding)
```

```
Plan: 

1. You have enter the right page. After clicking on the &#x27;Download PDF&#x27; link, wait for the download manager or browser to initiate the download. 
2. If a confirmation dialog appears, confirm the download. 
3. Wait for the PDF to download and save it to the designated location on the computer.
Next Action: 
Move the cursor to the &#x27;Download PDF&#x27; link located at the box with coordinates {&#x27;x_min&#x27;: 0.84, &#x27;y_min&#x27;: 0.152, &#x27;width&#x27;: 0.128, &#x27;height&#x27;: 0.019} on the screen and perform a click operation.
Grounded Operation:[link]  Download PDF -> CLICK at the box [[840,152,968,171]]
```

`Plan`: 用户已经成功访问论文的目录地址

1. 确认是否要下载
1. 如果是，则等待下载成功就行

`Next Action`: 点击 `Download PDF` 按键

## 注意事项

1. 本模型一次只能对一个图片进行处理，不能同时处理多张图片。
1. 由于每次处理一张新的图片，之前的聊天信息会被删除，相当于没有进行多轮对话，而是多个单轮对话，因此，需要传入的提示词包括完整的连续对话信息。
1. 本模型没有 执行功能，如果你希望执行点击、输入等动作，你需要自己适配对应的执行工具。
