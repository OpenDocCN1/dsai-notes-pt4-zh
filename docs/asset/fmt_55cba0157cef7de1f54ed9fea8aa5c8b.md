
1. 然后输入需求：

请将图像（`https://ml2022.oss-cn-hangzhou.aliyuncs.com/img/image-20250908183154697.png`）上的红色沙发改为灰色，并将猫咪改成乖巧可爱的一只阿比西尼亚猫。请重新尝试

![](img/b5b0d9d68ca7eca1046282684e38dc2c.png)

1. 输出结果如下

![](img/2d8374f52f7c74710f56187c10a8ea94.png)

## 3.3 Dify 多模态 Agent 创建流程

这个文生图 `Agent` 不仅仅是一个“文字生成图像”的工具，而是一个真正意义上的 多模态创意智能体，它具备以下核心功能与应用价值：

### 双引擎集成，智能路由选择

同时集成了 `Google Nano Banana` 与 `Qwen Text2Image` 两大前沿模型，既能处理 `写实/摄影级` 场景，又能胜任 插画/二次元/国风 需求。

支持自动路由，根据输入语言、提示词风格、是否上传图片等条件，智能选择最佳引擎。

### 文本到图像（Text-to-Image）

用户只需输入一段自然语言描述（`prompt`），即可生成高质量图像。

支持多种风格（`realistic`、`anime`、`illustration`、`watercolor` 等），可一键切换视觉效果。

### 图像到图像（Image-to-Image）编辑

借助 `Nano Banana` 的能力，支持用户上传一张已有图片，然后基于新描述进行修改或再创作。

可进行 `局部重绘、背景替换、风格迁移`，保持主体一致性，非常适合电商/广告/影视设计团队。

### 批量生成与参数可控

可一次生成多张图片（1–4 张），通过 `seed` 参数保证结果可复现；

支持自定义尺寸、迭代步数、采样方法等，满足企业级创作对细节的精细控制需求。

### 安全与合规保障

内置敏感内容过滤机制，拒绝违法、暴力、色情、仇恨类生成请求；

保障了多模态 `Agent` 在企业级场景中的可用性与合规性。

简言之，这个文生图 `Agent` 不仅可以把文字变成图像，还能把图像二次创作，为用户提供一个低门槛、功能完备的 AI 创意工作室。无论是个人创作，还是企业的电商设计、广告投放、品牌视觉管理，都能显著提升效率和创意产出。

### 3.3.1 Agent 提示词模板

请将以下内容完整粘贴到 `Agent` 的“指令”区域：

```
你是一个智能多模态创意助手，支持两种图像生成工具：Nano Banana 与 Qwen Text2Image，提供“文本生成图像”与“图像编辑”两种能力。请根据以下流程工作：

1. 工具选择
   - 如果 {{engine}} == &quot;auto&quot;：
     - 若 {{lang}} == &quot;zh&quot; 或 {{#user_input}} 为中文描述，且风格关键词包含“二次元 / 水墨 / 插画”等，优先选择 wwwzhouhui/text2image；
     - 若 {{image_url}} 存在（非空），表示用户想进行图像修改，优先选择 nano_banana/text2image（支持图→图编辑）；
     - 否则，如 {{#user_input}} 主要为英文，或包含“写实 / 光影 / 电影质感 / 景深 / 胶片 / 写真”等关键词，优先选择 Nano Banana。
   - 如果 {{engine}} 为 &quot;nano&quot; 或 &quot;qwen&quot;，则强制使用对应工具。
   - 若所选工具调用失败，自动切换至另一工具，并在最终结果中注明“已回退”。

2. 操作逻辑
   - 若 {{image_url}} 存在：进入“图像编辑模式”，在原图基础上按 {{#user_input}}、{{style}}、{{negative_prompt}} 等要求进行编辑；
   - 否则：进入“文本生图模式”，依据 {{#user_input}}、{{style}} 等生成新图。

3. 参数映射
   - 将以下变量映射到工具允许的参数：{{#user_input}}, {{negative_prompt}}, {{size}} → (width/height), {{seed}}, {{steps}}, {{sampler}}, {{count}}。
   - 当 {{size}} 形如 &quot;1024x1536&quot; 时，拆分为 width=1024、height=1536；若工具不支持该分辨率，按插件默认值或最接近值回退。
   - 工具不支持的字段自动忽略；缺省参数使用插件默认值。

4. 结果格式
   - 以 Markdown 列表返回，每项包含：图片URL / 使用工具 / 参数摘要（如 width×height、steps、seed、style、是否图像编辑）。
   - 多图输出时按 1., 2., 3.… 编号。
   - 若失败，请返回失败原因与可操作建议（如缩短 {{#user_input}}、降低 {{size}}、减少 {{count}}、调整 {{steps}} 等）。

5. 安全规范
   - 拒绝生成任何违法、仇恨、暴力、色情内容；遇到此类请求需礼貌拒绝并说明原因；
   - 不输出任何凭据或内部信息。

现在请基于用户输入（{{#user_input}}）和已配置变量处理请求：
- 先用一句话说明你选择的引擎及理由（例如：“因输入为中文插画风格，优先使用 Qwen Text2Image”；或“因提供了 image_url，进入图像编辑模式，使用 Nano Banana”）；
- 随后调用工具生成图像，并按“结果格式”输出。
```

这份指令模板在逻辑上覆盖了：自动路由（文本 vs 编辑），工具能力区别（`Nano` 支持图→图），参数映射与容错，输出规范，以及安全校验，满足完整 `Agent` 实战要求。

### 3.3.2 Agent 变量设置

接下来请在“变量 → 添加”中逐一创建以下变量，方便提示词调用：

| 变量名 | 类型 | 建议值 / 说明 |
| --- | --- | --- |
| `style` | 下拉 | `realistic` / `anime` / `illustration` / `watercolor` / `sketch` —— 指定风格 |
| `lang` | 下拉 | `auto` / `zh` / `en` —— 语言辅助路由，可自动判断用户输入语种，也可手动指定 |
| `size` | 下拉 | `512x512`, `768x768`, `1024x1024`, `1024x1536` —— 常见分辨率 |
| `negative_prompt` | 段落 | 可选，排除不希望生成的元素（如 `watermark`, `text`, `lowres`, `blurry` 等） |
| `image_url` | 文本 | 可选，图像编辑模式时传入已有图片链接（支持 `Nano Banana` 的图→图修改） |
| `seed` | 数字 | 可选，固定随机种子，便于结果复现 |
| `steps` | 数字 | 20–50，控制生成的迭代步数，数值越高越细致，但生成速度更慢 |
| `sampler` | 下拉 | 可留空或按插件支持项（不同模型可能支持不同采样算法） |
| `engine` | 下拉 | `auto` / `nano` / `qwen` —— 控制路由选择，`auto` 时系统自动分配 |
| `count` | 数字 | 1–4，生成张数（注意：`Nano Banana` 建议仅生成 1 张，以保证稳定性） |

> 说明补充：
>
> 当 `image_url` 不为空时，表示进入“图像编辑模式”，`Agent` 会首先按照之前提示词中的路由规则，将任务交给支持图→图的 `Nano Banana`。
>
> `lang` 和 `prompt` 的语种影响工具选择，确保生成结果更符合语言习惯。
>
> `size`, `seed`, `steps`, `sampler`, `count` 等变量提供高度可控性，为企业创作流程提供灵活配置空间。

> `realistic`：写实
> `anime`：动漫
> `illustration`：插画
> `watercolor`：水彩
> `sketch`：素描 / 速写

## 3.4 多模态 Agent 应用实战

### A. 中文插画/国风（优先 `Qwen Text2Image`）

#### A-1 国风水墨纵幅海报

```
{{engine}}: auto
{{lang}}: zh
{{style}}: watercolor
{{size}}: 1024x1536
{{negative_prompt}}: watermark, logo, text
{{count}}: 1
{{steps}}: 28
{{seed}}: 1024
```

对话时输入：

```
国风水墨画，高山云海、古松与飞鸟，留白布局，层次分明，画面宁静
```

![](img/6e40124d795b2d30030f16880a0d1d94.png)

![](img/761d798c84aadcd1c470940a55b5e00f.png)

#### A-2 二次元角色立绘

```
{{engine}}: auto
{{lang}}: zh
{{style}}: anime
{{size}}: 1024x1024
{{negative_prompt}}: blurry, low quality
{{count}}: 1
{{steps}}: 30
{{seed}}: 7788
```

![](img/546005dc09aa3ed459f4ed7e7bdd6fb6.png)

对话时输入：

```
二次元风格，国潮少女，黑金拼色，赛博朋克细节，霓虹边缘发光，酷炫姿势
```

![](img/490bf2a6177e26bb082bc42136507c76.png)

![](img/d1ef5e7871d9b0f3d61036d771f392af.png)

#### A-3 儿童教育插画

```
{{engine}}: auto
{{lang}}: zh
{{style}}: illustration
{{size}}: 1024x1024
{{count}}: 1
{{steps}}: 26
```

对话时输入：

```
儿童教育插画，小朋友观察太阳系行星，画面温馨明亮，边角留空可放文字
```

![](img/2b95c2e74be91bc9d6e12400ddc9cabd.png)

![](img/7b4e58587b5ea253ab69c5e644bb3a53.png)

### B. 写实/摄影级（优先 `Nano Banana`）

#### B-1 产品写实渲染

```
{{engine}}: auto
{{lang}}: en
{{style}}: realistic
{{size}}: 1024x1024
{{negative_prompt}}: watermark, text, lowres
{{count}}: 1
{{steps}}: 28
{{seed}}: 42
```

对话时输入：

```
a matte-black wireless earbud on a clean white background, studio lighting, soft shadow, product showcase
```

> 在纯白色背景上放置一个哑光黑的无线耳机，使用影棚灯光，营造柔和的阴影，以展示产品。

![](img/459e1887ec8cb773a16e9559ee438f07.png)

![](img/1019c76b8be94a2aa4153e5ee03adcb8.png)

#### B-2 电影海报风人像

```
{{engine}}: auto
{{lang}}: en
{{style}}: realistic
{{size}}: 1024x1536
{{negative_prompt}}: cartoon, anime
{{count}}: 1
{{steps}}: 32
{{seed}}: 2025
```

![](img/ea33bdfc06479b5c861e20705a9da663.png)

对话时输入：

```
cinematic portrait of an astronaut in a studio, dramatic lighting, 85mm lens, shallow depth of field, film grain
```

![](img/b35a56ce853e873b56a7a98b4b52d186.png)

![](img/216f4e6530750587fa84aa284937665f.png)

#### B-3 旅行风景摄影

```
{{engine}}: auto
{{lang}}: en
{{style}}: realistic
{{size}}: 1024x1024
{{count}}: 1
{{steps}}: 28
```

对话时输入：

```
golden hour landscape of a coastal town, long exposure water, crisp details, natural color, wide angle
```

![](img/2a72d108d1c8725cbc26a1465e24cb24.png)

![](img/3f938761ee20f1246b4bb59f4074ae05.png)

### C. 图像编辑（`Nano Banana`）

#### C-1 产品换背景

`{{image_url}}`: `https://ml2022.oss-cn-hangzhou.aliyuncs.com/img/e06042229cc02164cb7d7a5f35bbb891.jpg`

```
{{engine}}: nano
{{lang}}: en
{{style}}: realistic
{{size}}: 1024x1024
{{negative_prompt}}: harsh shadow, halo, cut-out edge
{{count}}: 1
{{steps}}: 30
{{seed}}: 66
```

对话时输入：

```
place the product on a minimal wood tabletop with soft daylight, add subtle reflection, keep realistic shadows
```

![](img/e3d0ecee38b140a6ff72c3c67f1f5e37.png)

![](img/4725bf3f0e15becaf16630e818843d38.png)

![](img/0cf672764dde7eb1e3c477a0f30a9357.png)

> 将产品放置在光线柔和的极简木质桌面上，添加微妙的倒影，保持真实的阴影效果。

#### C-2 风格迁移（真人→插画）

`{{image_url}}`: `https://ml2022.oss-cn-hangzhou.aliyuncs.com/img/225911b708b15df44250a16cf639904b.jpg`

```
{{engine}}: nano
{{style}}: illustration
{{size}}: 1024x1024
{{negative_prompt}}: distortion, extra fingers
{{count}}: 1
{{steps}}: 32
```

对话时输入：

```
convert to clean illustration style, smooth color blocks, gentle outlines, keep facial identity
```

![](img/9683bed96a23ed67f9b0789ed629fca8.png)

![](img/33ecdabdd0aa0fe9f11090c4c0dd0dee.png)

![](img/1138ce0e83e4527903480cd7e6d6e3c4.png)

## 4. 论文科研助手开发实战

### 4.1 论文科研助手的功能与价值

在学术研究过程中，快速获取相关论文、理解核心结论并生成清晰报告，是研究人员最常遇到的需求之一。`Dify Agent` 结合 `Tavily` 与 `ArXiv` 两大工具插件，可以打造一个强大的“论文科研助手”：

#### 多渠道内容检索

通过 `Tavily` 实现广域网络搜索，获取学术博客、技术动态、新闻评论、会议介绍等实时信息；

通过 `ArXiv` 查询正式发布的学术论文，确保来源权威。

#### 过滤与结构提炼

结合大模型，有选择地提炼检索结果中的要点，如研究目的、方法、实验结果、核心贡献等；

自动生成论文摘要对比、研究进展总结等结构化信息。

#### 高效“研究秘书”

研究人员可仅输入一个问题或研究方向，`Agent` 会同步调用两个工具，获取并整理信息，提供及时、有参考价值的学术答复；

如“请找五篇关于量子纠缠在通信中的应用，并分别总结实验方法”，就可以直接输出段落对比总结。

#### 增强科研能力

除浏览论文之外，还能通过 `Tavily` 检索技术博客、项目链接或解读文章，形成多维视角；

避免遗漏新兴细节或实例案例，提升研究深度。

总之，这个 `Agent` 并非一个单纯的检索工具，而是一个具备：学术论文可检索 + 网页信息实时整合 + 内容结构化输出的智能助手，很贴近真实科研中“查文献 + 比较分析 + 总结汇报”的流程。

### 4.2 工具插件核心功能介绍与下载流程

在论文科研助手中，`Tavily` 与 `ArXiv` 是两大核心支撑插件。两者功能互补，一个偏向 广域搜索与实时情报获取，一个聚焦于 权威学术文献检索。理解它们的能力边界与适用场景，是合理设计 `Agent` 的关键。
