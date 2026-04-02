# 提示词拼接

> 来源：[https://ai.feishu.cn/docx/UormdC2gWo8nFxxFSIMcf3MMnSR](https://ai.feishu.cn/docx/UormdC2gWo8nFxxFSIMcf3MMnSR)

本章节将展示如何拼接提示词以适配 CogAgent-9b-20241220 模型。本模型不是连续对话模型，因此，本章节非常重要，请关注提示词是如何拼接的。

## 用户输入部分

用户输入部分分为四个字段，必须严格按照字段进行拼接，否则模型无法正常回答

### task 字段

用户输入的任务描述，类似文本格式的prompt，该输入可以指导 CogAgent 模型完成用户任务指令。请保证简洁明了。

该字段前面必须增加 Task:关键词。例如，用户的任务是打开微信。则该部分提示词是:

Task: 打开微信

### platform 字段

CogAgent 支持在多个平台上执行可操作Agent功能, 我们支持的带有图形界面的操作系统有三个系统，

+    Windows 10，11，请使用 WIN 字段。

+   Mac 14，15，请使用 MAC 字段。

+   Android 13，14，15 以及其他GUI和UI操作方式几乎相同的安卓UI发行版，请使用 Mobile 字段。

如果您使用的是其他系统，效果可能不佳，但可以尝试使用 Mobile 字段用于手机设备，WIN 字段用于Windows设备，MAC字段用于Mac设备。

### format 字段

### 用户希 CogAgent 返回何种格式的数据, 这里有以下几种选项:

+    Answer in Action-Operation-Sensitive format.:  返回模型的行为，对应的操作，以及对应的敏感程度。

+   Answer in Status-Plan-Action-Operation format.: 返回模型的装题，行为，以及相应的操作。

+   Answer in Status-Action-Operation-Sensitive format.: 返回模型的状态，行为，对应的操作，以及对应的敏感程度。

+   Answer in Status-Action-Operation format.: 返回模型的状态，行为。

+   Answer in Action-Operation format. 返回模型的行为，对应的操作。

### History 字段

对于模型的每个回复，仅仅保留模型输出的Action以及Grounded Operation字段信息。

只要该字段仅仅保留模型输出如下:

```
Status: 当前处于邮箱界面[[0, 2, 998, 905]]，左侧是邮箱分类[[1, 216, 144, 570]]，中间是收件箱[[144, 216, 998, 903]]，已经点击“全部标为已读”按钮[[223, 178, 311, 210]]。
Action: 点击页面顶部工具栏中的“全部标为已读”按钮，将所有邮件标记为已读。
Grounded Operation: CLICK(box=[[219,186,311,207]], element_type=&#x27;可点击文本&#x27;, element_info=&#x27;全部标为已读&#x27;)
&lt;&lt;一般操作&gt;&gt;
```

拼接的历史字段为:

```
History Steps: \n0. CLICK(box=[[219,186,311,207]], element_type=&#x27;可点击文本&#x27;, element_info=&#x27;全部标为已读&#x27;)\t点击页面顶部工具栏中的“全部标为已读”按钮，将所有邮件标记为已读。\n1. xxx
```

### 拼接顺序和结果

```
query = f&#x27;{task}{history}{platform}{format}&#x27;
```

每个部分的提示词拼接完成都需要\n进行换行。

## 继续执行

有时候，模型会返回 end()。但我们希望模型能继续运行，此时应该在提示词{task}之后拼接上固定特殊token

[Continue]。这能让模型强行继续回答。

```
query = f&#x27;{task}[Continue]\n{history}{platform}{format}&#x27;
```

## 模型回复部分

1.  敏感操作: 包括 <<敏感操作>> <<一般操作>> 几种类型，只有要求返回Sensitive的时候返回。

1.  Plan, Agent, Status, Action 字段: 用于描述模型的行为和操作。只有要求返回对应字段的时候返回，例如带有Action则返回

Action字段内容。

1.  常规回答部分，这部分回答会在格式化回答之前，表示综述。

1.  Grounded Operation 字段:

用于描述模型的具体操作，包括操作的位置，类型，以及具体的操作内容。其中 box 代表执行区域的坐标，element_type 代表执行的元素类型，

element_info 代表执行的元素描述。这些信息被一个 操作指令 操作所包裹。这些指令包括：

+   CLICK: 点击操作，模拟鼠标点击或者手指触摸。

+   LONGPRESS: 长案操作。仅在 Mobile 模式下支持。

## 完整示例

模型回复的格式和信息取决于用户提示词中format字段的信息。这里展现一组例子：

用户的任务是希望帮忙将所有邮件标记为已读，用户使用的是 Mac系统，希望返回的是Action-Operation-Sensitive格式。

正确拼接后的提示词应该为：

```
Task: 帮我将所有的邮件标注为已读
(Platform: Mac)
(Answer in Action-Operation-Sensitive format.)
```

这里展现了不同格式要求下的返回结果:

+   Answer in Action-Operation-Sensitive format

```
全部标为已读&quot;按钮，左键单击，将所有邮件标记为已读。
Action: 点击页面顶部工具栏中的“全部标为已读”按钮，将所有邮件标记为已读。
Grounded Operation: CLICK(box=[[219,186,311,207]], element_type=&#x27;可点击文本&#x27;, element_info=&#x27;全部标为已读&#x27;)
&lt;&lt;一般操作&gt;&gt;
```

+   Answer in Status-Plan-Action-Operation format

```
在收件箱中找到“全部标记为已读”按钮，左键单击该按钮，将所有邮件标记为已读。
Status: None
Plan: None.
Action: 点击收件箱页面顶部中间的“全部标记为已读”按钮，将所有邮件标记为已读。
Grounded Operation: CLICK(box=[[219,186,311,207]], element_type=&#x27;可点击文本&#x27;, element_info=&#x27;全部标为已读&#x27;)
```

+   Answer in Status-Action-Operation-Sensitive format

```
“全部标为已读”按钮，左键单击，将所有邮件标记为已读。
Status: 当前处于邮箱界面[[0, 2, 998, 905]]，左侧是邮箱分类[[1, 216, 144, 570]]，中间是收件箱[[144, 216, 998, 903]]，已经点击“全部标为已读”按钮[[223, 178, 311, 210]]。
Action: 点击页面顶部工具栏中的“全部标为已读”按钮，将所有邮件标记为已读。
Grounded Operation: CLICK(box=[[219,186,311,207]], element_type=&#x27;可点击文本&#x27;, element_info=&#x27;全部标为已读&#x27;)
&lt;&lt;一般操作&gt;&gt;
```

+   Answer in Status-Action-Operation format

```
在收件箱中找到“全部标记为已读”按钮，左键单击，将所有邮件标记为已读。
Status: None
Action: 在收件箱页面顶部，点击“全部标记为已读”按钮，将所有邮件标记为已读。
Grounded Operation: CLICK(box=[[219,186,311,207]], element_type=&#x27;可点击文本&#x27;, element_info=&#x27;全部标为已读&#x27;)
```

+   Answer in Action-Operation format

```
在收件箱中，右键单击第一封邮件，以打开操作菜单。
Action: 在左侧邮件列表中，右键单击第一封邮件，以打开操作菜单。
Grounded Operation: RIGHT_CLICK(box=[[154,275,343,341]], element_info=&#x27;[AXCell]&#x27;)
```