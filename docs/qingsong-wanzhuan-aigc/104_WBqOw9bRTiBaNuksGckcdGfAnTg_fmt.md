
# 《牛津图解词典》风格 AI 文生图 Prompt 通用模板

> 来源：[https://my.feishu.cn/docx/AVofdI4MzobHebxVXMwcrH7InHd](https://my.feishu.cn/docx/AVofdI4MzobHebxVXMwcrH7InHd)

创作初衷：希望创建一个模仿《牛津图解词典》风格的通用 AI 文生图 Prompt 模板。该模板应能指导 AI 生成高清晰、生动逼真的场景插图，并在图中清晰地标注出关键元素（词汇或句子），同时提供英文原版和简体中文翻译，以增强视觉学习效果。

![](img/265ee0e5e71a694ca80b942cc178e056.png)

[Oxford Picture Dictionary, 3rd Edition (Jayme Adelson-Goldstein, Norma Shapiro) (Z-Library).pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/NzWMbaIZFoJjb4xS00QcyyKQnBA)

**适用AI模型：Nano Banana Pro**（Gemini 3 Pro Image）

---

## 《牛津图解词典》风格插图Prompt 1️⃣

> 在每次提问前，补充最后 `${核心场景/主题}` 这个变量部分的具体内容即可使用：

```
# AI 文生图 Prompt 通用模板：牛津图解词典风格

## 0. 角色设定与任务综述 (Role & Objective)
你是一位精通双语教育的视觉内容设计师。你的核心任务是模拟经典的《牛津图解词典》(Oxford Picture Dictionary) 风格，为语言学习者制作一张“沉浸式情景词汇学习卡”。

请根据文章末尾提供的【核心场景/主题】，生成一张图文并茂的图片。该图片必须具备高度的实用性，通过“场景再现 + 核心物体编号 + 双语词汇侧栏/标注”的形式，帮助用户直观地建立单词与实物之间的联系。

## Prompt 指令结构

### I. 图像风格与质量要求 (Aesthetic & Quality)
**风格**: 生成一张高分辨率、细节丰富的教育性插图 (Educational Illustration Style) 图片。--ar 3:4
**视觉效果**: 采用鲜艳、清晰的色彩方案，构图应清晰地展示一个真实的日常或工作环境，旨在提供无可匹敌的词汇教学支持。
**情绪/氛围 (可选)**: [添加所需的特定氛围，例如：繁忙而高效，温馨的家庭环境，或注重安全的工作场所]

### II. 核心构图与场景元素 (Composition & Elements)
**主题聚焦**: 画面应以 `${核心场景/主题}` 为中心展开，所有元素均置于有意义的语境之中。
**元素选择**: 构图必须包含 5到10个 (或更多) 与主题紧密相关的、实用且核心的视觉元素或物体。
*(例如：若主题为“健康护理”，应包含：医生 (doctor)、病人 (patient)、听诊器 (stethoscope)、病床 (hospital bed) 等。)*
**人物动作 (可选)**: 如果场景中有人物，请描述他们正在进行与主题相关的动作或行为 (例如：打招呼、做饭、填写表格、或在课堂上提问)。

### III. 嵌入式文本标注要求 (Crucial Text Overlay)
**【重要说明：图像生成模型必须将以下标注文本以图文并茂的方式直接嵌入到最终图片中。】**

1. **标注方式**: 使用编号 (1., 2., 3., ...) 或箭头清晰地指向图片中的每个核心元素。
2. **双语并列**: 每个被标记的元素旁边，必须同时并列显示其英文单词/短语和对应的简体中文翻译。

   **格式范例**:
   > [英文单词/短语] (简体中文翻译)
   > server (服务员)
   > kitchen utensils (厨房用具)

3. **对话/句子 (如果适用)**: 在图片的空白区域或边框上，嵌入 1到2句 主题相关的实用英语对话或短语 (例如“日常会话”或“请求”)。这些句子也必须提供英汉对照。

   **格式范例**:
   > **English**: May I have the check, please?
   > **中文翻译**: 请问可以买单吗?
   >
   > **English**: Please take a seat.
   > **中文翻译**: 请坐。

---

## 🚀 开始执行：核心场景/主题输入

**请基于以下场景生成图片：**

### `${核心场景/主题}`: 在拥挤的机场办理登机手续 (Check-in at a busy airport)
*(提示：包含乘客、护照、行李传送带、登机牌、地勤人员等元素)*    
```

### 生成效果示例：

【核心场景/主题】在拥挤的机场办理登机手续

![](img/b8d6eef0849139b9f344e808c892d052.png)

**【核心场景/主题】一家繁忙的超市/杂货店，**顾客和店员正在互动，展示各种商品和区域。

![](img/0652d4b6dc880aa6199a7acaceb9f392.png)

---

## 《牛津图解词典》风格插图Prompt 2️⃣

> 在每次提问前，补充最后 `${核心场景/主题}` 这个变量部分的具体内容即可使用：

```
# AI 文生图 Prompt 通用模板：牛津图解词典风格

## 0. 角色设定与任务综述 (Role & Objective)
你是一位精通双语教育的视觉内容设计师。你的核心任务是模拟经典的《牛津图解词典》(Oxford Picture Dictionary) 风格，为语言学习者制作一张“沉浸式情景词汇学习卡”。

请根据文章末尾提供的【核心场景/主题】，生成一张图文并茂的图片。该图片必须具备高度的实用性，通过“场景再现 + 核心物体编号 + 双语词汇侧栏/标注”的形式，帮助用户直观地建立单词与实物之间的联系。

## Prompt 指令结构

### I. 图像风格与质量要求 (Aesthetic & Quality)
**风格**: 生成一张高分辨率、细节丰富的教育性插图 (Educational Illustration Style) 图片。--ar 3:4
**视觉效果**: 采用鲜艳、清晰的色彩方案，光线明亮均匀。构图应清晰地展示一个真实的日常或工作环境，旨在提供无可匹敌的词汇教学支持。
**情绪/氛围 (可选)**: 积极、专业且具有生活气息。

### II. 核心构图与场景元素 (Composition & Elements)
**主题聚焦**: 画面应以用户提供的 `${核心场景/主题}` 为中心展开，所有元素均置于有意义的语境之中。
**元素选择**: 构图必须包含 5到10个 (或更多) 与主题紧密相关的、实用且核心的视觉元素或物体。
*(例如：若主题为“健康护理”，应包含：医生、病人、听诊器、病床等。)*
**人物动作**: 场景中的人物应正在进行与主题相关的自然互动或操作，以增强情境感。

### III. 嵌入式文本标注要求 (Crucial Text Overlay)
**【重要说明：图像生成模型必须具备文字渲染能力，并将以下标注以图文并茂的方式直接嵌入图片中。】**

1. **标注方式**: 使用清晰的数字编号 (1, 2, 3...) 或引线，准确指向图片中的每个核心元素。
2. **词汇列表**: 在图片的侧边、底部空白处，或直接在元素旁，列出对应的双语词汇。
   * **格局**: `[编号]. [英文单词] - [中文翻译]`
   * **范例**: `1. Stethoscope - 听诊器`
3. **实用例句 (可选)**: 在图片的空白区域或边框上，嵌入 1-2 句与场景相关的实用英语对话或短语，并附带中文。
   * **格局**:
     > **English**: [英文句子]
     > **CN**: [中文翻译]

---

## 🚀 开始执行：核心场景/主题输入

**请基于以下场景生成图片：**

### `${核心场景/主题}`: 在拥挤的机场办理登机手续 (Check-in at a busy airport)
*(提示：包含乘客、护照、行李传送带、登机牌、地勤人员等元素)*
```

### 生成效果示例：

【核心场景/主题】在拥挤的机场办理登机手续

![](img/fd204a01272792616094d8bea720c635.png)

**【核心场景/主题】一家繁忙的超市/杂货店，**顾客和店员正在互动，展示各种商品和区域。

![](img/f3afd0444e76aa4778ba93cdcd2b49f2.png)

---

## 《牛津图解词典》风格插图Prompt 3️⃣

> 在每次提问前，补充第二段里 `核心场景/主题` 这个变量部分的具体内容即可使用：

```
# Role: Professional Lexicographer & Illustrator (Oxford Picture Dictionary Style)
You are tasked with creating a single, high-resolution "Visual Dictionary Page" that combines a detailed scene with a bilingual vocabulary list in one image.

# User Scenario
The topic is: "[在此处填入您的场景，例如：Subway Station / In the Kitchen]"

# Image Generation Instructions
Generate **ONE single image** containing two distinct sections (Vertical Layout):

## Section 1: The Illustration (Top 70% of the image)
* **Content**: A realistic, wide-angle educational illustration of the [User Scenario].
* **Style**: Oxford Picture Dictionary 3rd Edition style. Clean lines, realistic colors, isometric or clear perspective.
* **Labeling**: Identify 8-10 key objects in the scene and place small, clear **Circles with Numbers (①, ②, ③...)** next to them.
* **Clarity**: Ensure the numbers are distinct and do not obscure the objects.

## Section 2: The Vocabulary Legend (Bottom 30% of the image)
* **Format**: A clean white section at the bottom acting as a legend.
* **Content**: A structured list corresponding to the numbers in the illustration.
* **Text Layout**: Display the text in columns or a grid.
   * Format: **"Number. English Word - Chinese Word"**
   * Example: "1. Train - 火车  2. Ticket - 车票"
* **Typography**: Use a standard, legible sans-serif font. Ensure the Chinese characters (Simplified Chinese) are rendered as accurately as possible.

# Technical Specifications
* **Aspect Ratio**: 3:4 (Vertical Textbook Page).
* **Resolution**: High Definition.
* **Overall Vibe**: Professional educational material, clear, informative, and easy to read.

# Execution
Create this full dictionary page now.
```

### 生成效果示例：

【场景/主题】Boarding a plane and finding my seat

![](img/0b92ed8294103cc3e8f4771d926869a2.png)

【场景/主题】First day at a new offic

![](img/33087ceda85846b5fdafec679ec7beaf.png)
