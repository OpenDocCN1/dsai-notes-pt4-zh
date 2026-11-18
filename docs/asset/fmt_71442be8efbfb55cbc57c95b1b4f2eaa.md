
# 一键生成故事视频！从文字到YouTube自动发布的完整配置手册

> 来源：[https://xuqiwei1986.feishu.cn/docx/FrBSdh37xoXfFExD1Yqceq3hnx3](https://xuqiwei1986.feishu.cn/docx/FrBSdh37xoXfFExD1Yqceq3hnx3)

你有没有想过这样的场景，动动手指头就可以完成视频的创作。现在每次写完一个故事,只需要复制粘贴到表单里,上传一张参考图,等15-20分钟,一个完整的视频就自动生成并上传到YouTube了。

**这套工作流做了什么？**

简单说,就是把一段文字故事变成一个完整的动画视频:

+   AI自动改编成6幕剧本
+   AI生成6张关键帧图片
+   AI把图片变成6段视频
+   自动合成、自动上传

整个过程不需要我手动操作。

**成本多少？**

单个视频成本大概¥30-80,具体取决于视频时长和画面复杂度。

如果你做自媒体、做内容营销,或者单纯想体验AI自动化,这套工作流能帮你节省大量时间。

**需要什么基础？**

老实说,有一定门槛:

+   需要懂一点`n8n`(一个自动化工具)
+   需要会配置API和OAuth
+   需要有1-2小时的配置时间

如果你完全不懂技术,建议先学习`n8n`基础,或者找懂的朋友帮忙搭建一次。

**这篇教程写什么？**

我会把29个节点的配置、每个API的参数、所有提示词,全部写清楚。

照着做,基本能跑通。遇到问题,文章里也有常见故障排查。

---

## 工作流概览

**文件名**:`故事类通用提示词生成工作流.json`

**核心功能**:

从一段「故事文本 + 图片上传」开始,自动完成:

1. 故事 → 六幕剧本(AI重构)
1. 剧本 → 六个关键帧提示词(AI生成)
1. 每帧 → AI生图
1. 每图 → AI动作提示词
1. 每帧 → 图生视频(海螺 [`fal.ai`](http://fal.ai))
1. 六段视频 → Creatomate合成完整视频
1. 上传到YouTube
1. 删除临时文件(Google Drive清理)

![](img/4d9779a02368bb5b246c638cc7fb8995.png)

**核心功能**:

从一段「故事文本 + 图片上传」开始,自动完成:

1. 故事 → 六幕剧本(AI重构)
1. 剧本 → 六个关键帧提示词(AI生成)
1. 每帧 → AI生图
1. 每图 → AI动作提示词
1. 每帧 → 图生视频(海螺 [`fal.ai`](http://fal.ai))
1. 六段视频 → Creatomate合成完整视频
1. 上传到YouTube
1. 删除临时文件(Google Drive清理)

---

## 全流程节点架构

---

## 节点逐项配置详解

---

### 节点 1：`On form submission`

**类型**：`n8n-nodes-base.formTrigger`

**功能**：生成一个在线表单,让用户提交故事内容和图片。

![](img/e03fec702ad6c2270ee3c38a47408e07.png)

![](img/eba00bcbd2e13db0216388778148a877.png)

**参数配置：**

```
{
  &quot;formTitle&quot;: &quot;故事收集&quot;,
  &quot;formFields&quot;: {
    &quot;values&quot;: [
      { &quot;fieldLabel&quot;: &quot;剧本内容&quot;, &quot;requiredField&quot;: true },
      { &quot;fieldLabel&quot;: &quot;image&quot;, &quot;fieldType&quot;: &quot;file&quot;, &quot;acceptFileTypes&quot;: &quot;.jpg, .png&quot;, &quot;requiredField&quot;: true }
    ]
  }
}
```

**输出示例：**

```
{
  &quot;剧本内容&quot;: &quot;一个小女孩在森林里发现了发光的石头...&quot;,
  &quot;image&quot;: &quot;uploaded_file.png&quot;
}
```

**连接关系：**

+   输出 → `Upload file1`(上传文件)
+   输出 → `Merge2`(故事文本合并)

---

### 节点 2：`Upload file1`

**类型**：`googleDrive`

**操作**：`upload`

**用途**：上传用户图片到指定文件夹作为临时缓存。

![](img/9215c817c52b1a88df36778b2028242e.png)

![](img/a363e25cc0d008e6577aaf92b3c40206.png)

**参数配置：**

+   `driveId`: "My Drive"
+   `folderId`: "1mTKklZcBTTEsuxIP3QZa8Zw5okf40SrR"
+   输出字段: `id`, `name`, `webViewLink`

**凭据要求**：`Google Drive` account (`OAuth2`)

**连接关系：**

+   输出 → `Merge2` (用于合并故事输入)
+   输出 → `Merge3` (用于最后清理)

---

### 节点 3：`Merge2`

**类型**：`merge`

**模式**：`combineByPosition`

**功能**：将表单提交的内容与上传文件的结果合并为一条完整数据。

![](img/408d55f98bdd5a9103d3320091de8e99.png)

**输出示例：**

```
{
  &quot;剧本内容&quot;: &quot;森林奇遇...&quot;,
  &quot;image&quot;: &quot;uploaded.png&quot;,
  &quot;drive_file_id&quot;: &quot;1xYwc...&quot;
}
```

**连接关系：** → `AI Agent`

---

### 节点 4：`AI Agent`(剧本整理 `AI Agent`)

**类型**：`@n8n/n8n-nodes-langchain.agent`

**模型连接**：OpenRouter Chat Model1 (`Gemini 2.5 Flash`)

**功能**：将输入的故事整理成6幕剧本,每幕包含一个核心视觉镜头。

**参数配置：**

+   `promptType`: `define`
+   `text`: =请你按照要求给我整理内容： `$json['剧本内容']`
+   `options.systemMessage`: 完整系统提示词(见下方)

**系统提示词完整内容：**

```
剧本整理 Agent —— 系统提示(System Prompt)

1. 角色
你是一名专业剧本编剧。
任务：解析用户提供的故事文本,在不改动核心情节与角色设定的前提下,将其提炼为由六个关键视觉镜头 (`[VFX]`) 构成的分幕剧本,并保留原题材与叙事风格。

⸻

2. 剧本与镜头规则 (固定 6 幕 / 6 个镜头)
必须将故事拆解为六幕(`Act I` 至 `Act VI`),每幕代表一个核心情节节点。
每幕内部必须包含且仅包含一个 `[VFX]` 视觉锚点,作为该幕的核心镜头。
为每幕撰写一句精炼的概括性标题作为幕名。
`[VFX]` 标记格式：
- 行首加 `[VFX]`。
- 后接 ≤ 25 字的中文摘要。
- 紧随其后的是圆括号包裹的英文视觉关键词(描述外貌、场景、光效等,用逗号分隔,不写人物名)。

⸻

3. 角色设定规则
在【角色设定】部分,用中文逐一列出主要角色的外貌与服饰特征。
剧本正文中再次出现角色时,应直接引用这些已设定的外貌关键词。
避免使用&quot;男人/女人/小孩&quot;等泛称。若原文信息不足,可进行最小必要限度的补全。
✅ 示例：&quot;身穿灰色风衣、留山羊胡的侦探&quot;
❌ 示例：&quot;男人&quot;

⸻

4. 输出格式(严禁变动)

【角色设定】
角色称谓: 外貌+服饰描述
角色称谓: ...

【剧本正文】
`Act I`: 幕名
旁白/动作: (对该镜头前的简要情景或动作描述)
`[VFX]` 关键帧画面的中文摘要 (`English`, `visual`, `keywords`, `for`, `AI`)
...
(共六幕)

⸻

5. 其他硬性要求
- 保持原故事的题材、受众与情感基调。
- 旁白与事件描述应自然、口语化,且逻辑连贯。
- 输出内容仅包含【角色设定】和【剧本正文】两个区块,不得添加任何解释性文字。
```

**输出示例：**

```
【角色设定】
女孩：黑发、身穿黄色斗篷...

【剧本正文】
`Act I`: 森林的呼唤
`[VFX]` 阳光透过树叶洒在石头上 (`forest`, `sunlight`, `magical light`)
...
```

**连接关系：** → `AI Agent1`

---

### 节点 5：`AI Agent1`(关键帧生成)

**类型**：`@n8n/n8n-nodes-langchain.agent`

**功能**：将6幕剧本转换为6个关键帧的图片生成提示词。

**参数配置：**

+   `promptType`: `define`
+   `text`: 完整提示词(见下方)
+   `hasOutputParser`: `true`
+   `options.systemMessage`: =请你按照要求给我整理内容：  `$json.output`

**提示词完整内容：**

```
# 角色与目标
你的角色是「影视分镜关键帧提示词生成器」。
你的核心任务是：阅读用户提供的完整剧本,分析其叙事节奏和视觉重点,并严格按照规则生成固定数量的关键帧提示词。

# 核心规则：固定数量与分配
1. **固定总数**: 输出必须包含正好 6 个关键帧提示词。
2. **平均分配**: 将 6 帧分配到每幕。
3. **内容选择**: 每幕挑选最具代表性画面。

# 提示词结构
每个关键帧输出字段：
- `scene_tag`: 场景标签(如 `Act I-KF01`)
- `zh_desc`: 中文画面描述
- `img_prompt`: 英文生图提示词
- `aspect_ratio`: 画面比例(如 `9:16`)

# 输出格式
严格为一个包含6个`JSON`对象的`JSON`数组。
不得包含任何解释性文字。
```

`Output Parser Schema` 配置：

```
[
  {
    &quot;scene_tag&quot;: &quot;Act I&quot;,
    &quot;zh_desc&quot;: &quot;示例：阳光客厅俯视镜头...&quot;,
    &quot;img_prompt&quot;: &quot;sun-lit living room, overhead view...&quot;,
    &quot;aspect_ratio&quot;: &quot;9:16&quot;
  }
]
```

**连接关系：** → `Structured Output Parser` → `Split Out`

---

### 节点 6：`Structured Output Parser`

**类型**：`outputParserStructured`

**功能**：强制验证上一步输出为合法 `JSON` 数组格式,确保数据结构正确。

---

### 节点 7：`Split Out`

**类型**：`splitOut`

**配置参数**：

+   `fieldToSplitOut` = `output`

**功能**：将包含6个关键帧的数组拆分成6个独立的数据对象,方便后续逐个处理。

**连接关系：** → `Download file`

---

### 节点 8：`Download file`

**类型**：`googleDrive.download`

**功能**：从`Google Drive`下载之前上传的参考图片。

**参数配置：**

```
{
  &quot;fileId&quot;: &quot;1xYwcgVGsz7_KGQxwRD9KzSNTw5qRSIyi&quot;,
  &quot;cachedResultName&quot;: &quot;印度故事.png&quot;
}
```

**连接关系：** → `生图节点`

---

### 节点 9：`生图节点`([`Yunwu.AI`](http://Yunwu.AI))

**类型**：`httpRequest`

![](img/5ffd8eab87f351c9483f5309dabf0615.png)

**请求方法**：`POST`

**认证方式**：`HTTP Header Auth` (`yunwu ai`)

**API配置：**

+   `URL`: https://yunwu.ai/api/generate
+   认证：需要在凭据中配置`yunwu ai`的`API Key`

请求体(`multipart-form-data`)：

```
{
  &quot;image&quot;: &quot; 上一步下载的图片二进制数据 &quot;,
  &quot;model&quot;: &quot;flux-kontext-pro&quot;,
  &quot;n&quot;: &quot;1&quot;,
  &quot;aspect_ratio&quot;: &quot;16:9&quot;,
  &quot;prompt&quot;: &quot;Refer to the artistic style of the image and use the following prompts to draw： $json.img_prompt &quot;
}
```

**输出字段示例：**

```
&quot;url&quot;: &quot;&lt;https://yunwu.ai/generated/image123.png&gt;&quot;
```

```
{
  &quot;data&quot;: [{
  }]
}
```

**连接关系：** → `Edit Fields`

---

### 节点 10：`Edit Fields`

**类型**：`set`

**功能**：从生图`API`的返回结果中提取图片`URL`。

![](img/a8ae52548dd12f6429e0399a47ead634.png)

**赋值规则：**

```
{
  &quot;url&quot;: &quot;= {{$json[&quot;data&quot;][0].url}} &quot;
}
```

**连接关系：** → `Merge1`

---

### 节点 11：`Merge1`

**类型**：`merge`

**模式**：`combineByPosition`

**功能**：合并生成的图片`URL`与关键帧描述信息。

**输出结构示例：**

```
&quot;url&quot;: &quot;&lt;https://yunwu.ai/generated/image123.png&gt;&quot;
```

```
{
  &quot;scene_tag&quot;: &quot;Act I-KF01&quot;,
  &quot;img_prompt&quot;: &quot;forest scene with magical light...&quot;,
}
```

**连接关系：** → `视频提示词生成节点`

---

### 节点 12：`视频提示词生成节点`

**类型**：`LangChain ChainLLM`

**模型**：`google/gemini-2.5-flash-preview`

**Prompt 定义完整内容：**
