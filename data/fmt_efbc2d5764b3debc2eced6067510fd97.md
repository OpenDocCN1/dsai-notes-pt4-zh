
# LLM用于多模态任务系列二：ChatVideo

> 来源：`https://agijuejin.feishu.cn/docx/SbZ4dA2kUomCd8xfUrZcagwan6b`
>
> 作者：Meta
>
> 原文：`https://zhuanlan.zhihu.com/p/629355868`
>
> 参考：`https://www.yuque.com/meta95/hmc3l4/gt2i8nfckaoc8zbc?singleDoc#`
>
> 论文：《ChatVideo: A Tracklet-centric Multimodal and Versatile Video Understanding System》，`https://arxiv.org/pdf/2304.14407.pdf`
>
> 项目：`https://www.wangjunke.info/ChatVideo/`

现有的深度视频模型受限于特定任务、固定的输入输出空间和较差的泛化能力，难以在真实场景中部署。在本文中，我们提出了对多模态和多功能视频理解的展望，并提出了一个名为ChatVideo的原型系统。我们的系统建立在以轨迹为中心的范式之上，将轨迹视为基本视频单元，并使用各种视频基础模型（ViFM）对它们的属性（例如外观、运动等）进行注释。所有检测到的轨迹都存储在一个数据库中，并通过数据库管理器与用户进行交互。我们在各种类型的野外视频上进行了广泛的案例研究，证明了我们的方法在回答与视频相关的各种问题时的有效性。

## 整体架构

大致流程如下：对于一个上传的视频，ChatVideo首先检测其中所有的`tracklets`，并识别出它们的类别、外观、动作等，这些都存储在数据库中。在用户输入诸如“该视频中有多少人”之类的问题后，基于LLM的数据库管理器将其转换为查询语句，并从上述数据库中检索有用的信息。最后，ChatGPT 对查询结果进行总结并将其润色为自然语言描述。

![](img/b2f6ec0040954d3c9932a575b96965e6.png)

## 基础模型

| 基础模型 | 功能 | 备注 |
| --- | --- | --- |
| OmniTracker | 获取轨迹段和运动信息 | |
| OmniVL | 针对跟踪结果，获取其中每个类别的静态外观描述信息及动态行为描述 | 静态信息prompt：“What does the`{cati}` look like?The`{cati}`”动态信息prompt：“What is the`{cati}`doing? The`{cati}`” |
| Whisper | 识别音频内容 | |
| Wave2Vec 2.0 | 判定音频情感 | |

## 数据库管理

### 存储格式

| ID | Category | Appearance | Motion | Trajectory | Audio |
| --- | --- | --- | --- | --- | --- |

![](img/608913aa07972f11a42cff7f527c94a2.png)

### 如何与用户交互

使用的Prompt：

```
Given an input question, first create a syntactically correct `{dialect}` query to run, then look at the results of the query and return the answer. Use the following format.

Question: "Question here"

SQL Query: "SQL Query to run"

SQL Result: "Result of the SQL Query"

Answer: "Final answer here"

Only use the following tables:

`{table_info}`

The records in the tables are in the following format:

ID: the primary key of the record.

Category: the category of the tracklet.

Appearance: the appearance of the tracklet.

Motion: the motion of the tracklet, described as "from `t1` to `t2` seconds, movements of the object".

Trajectory: the trajectory of the tracklet, described as "at `t` seconds, (`x1`, `y1`, `x2`, `y2`)". The velocity of the object could be obtained by calculating the distance between two positions.

Audio: the audio in this video

The records in the tables are randomly ordered. If the results of the SQL Query include multiple records, you should list them separately in your answers instead of mixing them together.

Question: `{input}`
```

## 案例

### 外观相关的问题

![](img/1b2555126157e828137efede8ca194dc.png)

### 运动相关的问题

![](img/d0957a617c0ee2157d64801a8b78a483.png)

### 语音相关的问题

![](img/fca9034ba4677737cde87ff9da68ced2.png)

## 当前局限性及未来方向

### 局限性

1. 难以跟踪快速移动的物体：跟踪快速移动的物体对于当前的跟踪算法来说是一个巨大的挑战。由于现有的跟踪模型容易受到这种情况的影响，我们的系统可能会对计数问题或严重依赖时间的问题给出不准确的答案。

2. 难以进行细粒度动作识别：虽然我们的系统可以有效识别大类动作，例如行走或跳跃，但它可能难以识别细粒度动作，例如区分不同类型的跳跃或识别更细微的动作。

3. 音频分类挑战大：受限于音频分类模型的泛化能力，我们的系统可能无法预测视频中声音的类别。因此，在该系统的实现中，无论音频的类别是什么，我们都将使用 ASR 模型来识别内容。

### 未来方向

1. 使用更强大的视频基础模型

2. 增强针对对抗性攻击的鲁棒性

3. 提升推理效率

4. 使用RLHF来对齐意图
