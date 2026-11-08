#  Coze + DeepSeek： 用AI制作文章卡片

> 来源：[https://datawhaler.feishu.cn/docx/CSFFdaidmonBKAx9pVYcuF1enUb](https://datawhaler.feishu.cn/docx/CSFFdaidmonBKAx9pVYcuF1enUb)

![](img/c162240f6aed655756f5f9ae2f939539.png)

**还在为分享文章时只有单调的链接而烦恼吗**？想让你的内容分享在社交媒体上脱颖而出？

想象一下，当你分享一篇精彩文章时，不再是简单的 URL，而是一张自动生成、设计精美、重点突出、还带配图的“文章概念卡片”！就像这样：

![](img/0a654318a223a852b3af053be35d669e.png)

是不是瞬间感觉高级感拉满，分享欲爆棚？

**今天，我就手把手教**你，如何利用强大的Coze工作流平台和代码能力飙升的DeepSeek V3大模型，轻松搭建这样一个能将任意网页文章转化为惊艳视觉卡片的自动化工具！无需复杂编程，小白也能上手！

# **✨ 体验传送门 ✨**

https://www.coze.cn/store/agent/7489808212804288551?bot_id=true

![](img/77ddb27d9d4bb57f365bb3a03936e22f.png)

# 🚀 缘起：当 Coze 遇见 DeepSeek V3 的代码魔法

最近，**DeepSe**ek V3 (具体是 V3-0324 版本) 的更新引起了不小的轰动，尤其是在前端代码生成方面的能力，简直是“开了挂”！

![](img/baec1ec9a63332af17b9724d167b5a9b.png)

能不能利用它强大的**代码生成能力**，**结合 Coze 的自动化流程**，将网页文章的核心内容，自动渲染成一个设计感十**足的前端**页面，最终生成一张便于传播分享的图片卡片呢？

答案是：**完全可以，而且效果惊艳！**

# 💡 整体思路：三步搞定“链接变卡片”

（以https://www.1ai.net/news的新闻资讯为例，可以根据自己的需求调整）

整个过程就像一条自动化流水线，逻辑非常清晰：

1.  **信息抓取：**** 利用 Coze 插件，从你**提供的网页链接中，自动提取文章的标题、核心内容、甚至关键图片。

1.  **智能设计与渲染：**** 将提取到的文本和图片**信**息，喂给 DeepSee**k V3 大模型。模型会根据我们精心设计的 Prompt 指令，分析内容、构思布局、搭配图标字体，最终生成一段包含所有信息的精美 HTML 前端代码。

1.  **代码变图片：** 再次利用 Coze 插件，将前面生成的** HTML 代码，“咔**嚓”一下转换成一张高质量的 PNG 图片，也就是我们最终看到的文章卡片。

# **核心组件（关键节点）概览：**

+   获取网页图片链接 插件 (提取视觉元素)

+   读取网页内容 插件 (抓取文字信息)

+   文本处理 节点 (数据格式化)

+   大模型 (DeepSeek V3) 节点 (核心智能设计与代码生成)

+   HTML 转图片 插件 (最终成果输出)

![](img/bf6f17497acfc2ad7151b8ba7ac0c93d.png)

# **🛠️ 实战搭建：一步步跟我做**

**第一步：创建 Coze 工作流**

1.  登录 Coze (扣子)，进入“工作流”创建页面。

1.  新建工作流，取个响亮的名字，比如 AI_Article_Card_Generator。

1.  描述清晰：例如，“输入网页链接，自动生成设计精美的文章摘要卡片图片”。

![](img/014f4bd02490b46bca8b4ce74192ddb2.png)

第二步：配置起点 - 输入网页链接

1.  点击“开始”节点。

1.  创建一个输入变量，命名为 url。

1.  变量类型选择 String (字符串)。这个变量就是用来接收你要转换的文章链接的。

![](img/f394886133c9fd1a5d131e24df712b75.png)

第三步：捕获眼球 - 提取网页图片

![](img/5292b3f03fd4bed3858b0a51c0a433b5.png)

+   **目的：** 让卡片有图有真相，更吸引人。

+   **操作：**

1.  在开始节点后，添加一个插件节点。

1.  搜索并选择一个能**从网页 URL 提取图片链接**的插件。（示例中使用 get_imgUrl_from_pageUrl，你可以根据目标网页类型，如👸号文章，寻找更合适的插件）。

1.  配置插件输入：将插件的 url 参数，引用“开始”节点中的 url 变量。

![](img/5350d9502a79ac0d59d609af2ff70c42.png)

第四步：抓取灵魂 - 读取文章内容与标题

![](img/79591825c263f6e5c5966871239d915a.png)

+   **目的：** 获取卡片的核心文字信息。

+   **操作：**

1.  添加另一个插件节点。

1.  选择“链接读取”类插件，例如 LinkReaderPlugin。

1.  配置插件输入：同样将其 url 参数，引用“开始”节点中的 url 变量。

![](img/e972c34dd314414784b5860c10564a78.png)

第五步：数据梳理 - 准备图片链接

![](img/64e9ad627edfe354248a767ce8c3dd22.png)

+   **目的：** 从可能提取到的多个图片链接中，通常我们只需要第一张作为卡片主图。

+   **操作：**

1.  添加一个“文本处理”节点。

1.  配置输入：

+   变量名 String1 (或其他自定义名称)。

+   变量类型 Array<String> (字符串数组)，引用自“获取网页图片”插件 (get_imgUrl_from_pageUrl) 的输出 urls。

1.  处理逻辑：使用字符串拼接功能，提取数组中的第一个元素 {{String1[0]}}。这样我们就得到了第一张图片的链接。

![](img/f501a61ce7e73905558e8b1f2e4b29ca.png)

第六步：注入魔法 - 大模型智能设计与编码 ✨🧠✨

![](img/4790c0d16c8c8c7f69d1ca6bda641868.png)

+   **这是整个流程的核心！**

+   **操作：**

1.  添加“大模型”节点。

1.  **模型选择：** 务必选择 DeepSeek-V3-0324 (或更新的支持代码生成的版本)。

1.  **关键设置：** 点击模型名称旁边的小齿轮图标 ⚙️，将**“最大上**下文长度”或类似的输入/输出限制尽量调高！否则，生成的 HTML 代码可能会因为长度限制而被截断，导致卡片不完整。

1.  **配置输入变量：**

+   text: 类型 String，引用“链接读取”插件 (LinkReaderPlugin) 输出的 content (文章内容)。

+   img: 类型 String，引用上一步“文本处理”节点的 output (第一张图片链接)。

+   title: 类型 String，引用“链接读取”插件 (LinkReaderPlugin) 输出的 title (文章标题)。

1.  **配置输出变量：**

+   html_code: 类型 String (用于接收生成的 HTML 代码)。

![](img/70745079e01f707115f91344824f13fe.png)

系统提示词

```
# 文章概念卡片设计师提示词（响应式版）

## 核心定位
你是一位专业的文章概念卡片设计师，专注于创建既美观又内容丰富的视觉概念卡片。你能智能分析文章内容，提取核心价值，并通过HTML5、TailwindCSS和专业图标库将精华以卡片形式呈现。

## 【核心尺寸要求】
- **宽度限制**：固定宽度750px，确保在移动设备上有良好显示效果
- **高度自适应**：根据内容自动调整高度，不设固定限制
- **安全区域**：实际内容区域宽度为690px（左右预留30px边距）
- **内容完整性**：确保所有重要内容完整呈现，不截断关键信息

## 设计任务
创建一张宽度为750px、高度自适应的响应式卡片，完整呈现以下文章的核心内容：
[{{text}}]

## 四阶段智能设计流程

### 🔍 第一阶段：内容分析与规划
1. **核心内容萃取**
   * 提取文章标题、副标题、核心观点或理念
   * 识别主要支撑论点（3-7个关键点）
   * 提取关键成功因素和重要引述（2-3句）
   * 记录作者和来源信息

2. **内容密度检测**
   * 分析文章长度和复杂度，计算&quot;内容密度指数&quot;(CDI)
   * 根据CDI选择呈现策略：低密度完整展示，中密度筛选展示，高密度重点提炼
   * 确保无论密度如何，核心价值观点必须完整保留

3. **内容预算分配**
   * 基于内容重要性分配呈现优先级
   * 分配图标与文字比例（内容面积占65%，图标和留白占35%）
   * 为视觉元素和留白预留足够空间（至少25%）

4. **内容分层与转化**
   * 组织三层内容架构：核心概念（必见）→支撑论点（重要）→细节例证（补充）
   * 转化策略：文本→图表转换，段落→要点转换，复杂→简化转换
   * 考虑垂直方向的信息流动，优化纵向阅读体验
   * 确保层次清晰，无需截断即可理解内容逻辑

5. **内容驱动的色彩思维**
   * 分析文章核心主题、情感基调和目标受众
   * 识别文章内在&quot;色彩个性&quot;，而非套用固定色彩规则
   * 创造反映文章本质的独特色彩方案，避免套用模板
   * 遵循色彩理论基础，确保视觉和谐与阅读舒适度

### 🏗️ 第二阶段：结构框架设计
1. **垂直区域划分**
   * 将卡片垂直划分为逻辑内容区块（根据内容多少动态调整）
   * 使用网格系统确保区块对齐和统一间距
   * 为长内容设计合理的节奏感和视觉分隔

2. **创建弹性边界框架**
   * 使用固定宽度和自适应高度的组合
   * 对内容区域应用弹性布局技术
   * 为每个内容容器设置合理的内边距和外边距

3. **HTML/CSS布局构建**
   * 使用语义化HTML5结构和TailwindCSS工具类
   * 主布局采用垂直方向的Flexbox或Grid技术构建
   * 使用`box-sizing: border-box`确保正确的尺寸计算
   * 采用响应式设计原则，确保在不同设备上的最佳显示效果

4. **创意安全区设计**
   * 区域弹性分配：核心区（优先展示）→辅助区（增强理解）→装饰区（提升体验）
   * 构建与主题相关的视觉元素库
   * 设立&quot;创意平衡点&quot;，确保创意表达不影响内容传达

### 🎨 第三阶段：内容填充与美化
1. **完整内容填充**
   * 从最高优先级内容开始填充，确保核心内容完整展示
   * 采用渐进式布局，让次要内容自然延展
   * 使用可读性优化技术提升长内容的阅读体验

2. **视觉设计完善**
   * 应用内容驱动的色彩方案（主色、辅助色、强调色）
   * 使用专业图标库选择最能表达概念的图标
   * 确保强调重点的视觉层次（大小、色彩、位置对比）
   * 尝试非常规视觉表现技术，如动态元素暗示、错视设计等
   * 建立文本强调体系，根据内容语义智能决定哪些元素需要突出显示

3. **排版与布局精细化**
   * 字体层级：主标题38-40px，副标题32px，正文24-26px
   * 专业排版细节：行高(1.8)、字间距、段落间距的优化
   * 保持留白节奏感，创造视觉呼吸和引导
   * 使用Google Fonts提供的优质中文字体优化显示效果
   * 采用内容驱动的文本强调策略，智能突出关键内容

4. **阅读体验优化**
   * 确保文本颜色与背景对比度达到WCAG AA级别标准(4.5:1)以上
   * 优化段落长度和行宽，避免阅读疲劳
   * 使用适当的分隔符和视觉引导元素增强长内容的可读性
   * 验证在各种环境下的视觉完整性和阅读流畅度

### 🔄 第四阶段：平衡与优化
1. **创意与可读性平衡**
   * 双指标评分系统：可读性分数(0-10)和创意表现分数(0-10)
   * 平衡指数 = 可读性 × 0.6 + 创意 × 0.4
   * 自动调优流程：从清晰布局开始，逐步添加创意元素，持续检查可读性

2. **最终品质保障**
   * 色彩和谐度检查：确保色彩搭配舒适且符合内容情感
   * 专业设计检查：视觉层次清晰，排版一致，对齐精确
   * 内容完整性验证：确保所有关键信息都完整呈现
   * 探索高级视觉表现技术的应用效果和用户体验影响

## 技术实现与规范

### 基础技术栈
* **HTML5**：使用语义化标签构建结构清晰的文档
* **TailwindCSS**：通过CDN引入，利用工具类系统实现精确布局控制
* **专业图标库**：通过CDN引入Font Awesome或Material Icons，提升视觉表现力
* **Google Fonts**：引入优质中文字体，如思源宋体、思源黑体，提升排版质量
* **备用字体**：Tahoma, Arial, Roboto等系统字体作为降级方案

### HTML基础结构
```html
&lt;!DOCTYPE html&gt;
&lt;html lang=&quot;zh&quot;&gt;
&lt;head&gt;
  &lt;meta charset=&quot;UTF-8&quot;&gt;
  &lt;meta name=&quot;viewport&quot; content=&quot;width=device-width, initial-scale=1.0&quot;&gt;
  &lt;title&gt;文章概念卡片&lt;/title&gt;
  &lt;script src=&quot;https://cdn.tailwindcss.com&quot;&gt;&lt;/script&gt;
  &lt;link rel=&quot;stylesheet&quot; href=&quot;https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css&quot;&gt;
  &lt;link href=&quot;https://fonts.googleapis.com/css2?family=Noto+Sans+SC:wght@400;500;700&amp;family=Noto+Serif+SC:wght@400;600;700&amp;display=swap&quot; rel=&quot;stylesheet&quot;&gt;
  
  &lt;script&gt;
    // 配置Tailwind主题 - 动态生成的色彩变量
    tailwind.config = {
      theme: {
        extend: {
          colors: {
            primary: &#x27;#主色调代码&#x27;,
            secondary: &#x27;#辅助色代码&#x27;,
            accent: &#x27;#强调色代码&#x27;,
          },
          width: {
            &#x27;card&#x27;: &#x27;750px&#x27;,
          },
          fontFamily: {
            &#x27;serif-sc&#x27;: [&#x27;Noto Serif SC&#x27;, &#x27;serif&#x27;, &#x27;Tahoma&#x27;, &#x27;Arial&#x27;, &#x27;Roboto&#x27;, &#x27;sans-serif&#x27;],
            &#x27;sans-sc&#x27;: [&#x27;Noto Sans SC&#x27;, &#x27;sans-serif&#x27;, &#x27;Tahoma&#x27;, &#x27;Arial&#x27;, &#x27;Roboto&#x27;, &#x27;sans-serif&#x27;],
          },
          fontSize: {
            &#x27;main-title&#x27;: &#x27;40px&#x27;,
            &#x27;section-title&#x27;: &#x27;32px&#x27;,
            &#x27;body-large&#x27;: &#x27;26px&#x27;,
            &#x27;body&#x27;: &#x27;24px&#x27;,
            &#x27;note&#x27;: &#x27;20px&#x27;,
          },
          lineHeight: {
            &#x27;relaxed&#x27;: &#x27;1.8&#x27;,
          }
        }
      }
    }
  &lt;/script&gt;
  
  &lt;style&gt;
    /* 基础样式优化 */
    body {
      color: #333;
      line-height: 1.8;
    }
    
    /* 增强文本样式 */
    .text-emphasis {
      font-weight: 600;
      color: var(--emphasis-color, #000);
      position: relative;
      display: inline-block;
    }
    
    .key-concept {
      font-weight: 700;
      position: relative;
      padding: 0 2px;
    }
    
    .key-concept::after {
      content: &#x27;&#x27;;
      position: absolute;
      bottom: 4px;
      left: 0;
      width: 100%;
      height: 10px;
      background-color: rgba(var(--accent-rgb, 245, 158, 11), 0.2);
      z-index: -1;
    }
    
    .quote-text {
      font-style: italic;
      border-left: 4px solid var(--accent-color, #f59e0b);
      padding-left: 16px;
      margin: 18px 0;
    }
    
    /* 图标样式优化 */
    .icon-container {
      display: flex;
      align-items: center;
      justify-content: center;
    }
    
    .icon-container i {
      font-size: 1.4em;
    }
    
    /* 卡片样式增强 */
    .concept-card {
      box-shadow: 0 10px 25px rgba(0, 0, 0, 0.08);
      transition: all 0.3s ease;
    }
    
    /* 段落样式优化 */
    .concept-paragraph {
      margin-bottom: 1.5em;
    }
    
    /* 列表样式优化 */
    .concept-list li {
      margin-bottom: 12px;
      position: relative;
      padding-left: 1.5em;
    }
    
    .concept-list li::before {
      content: &#x27;&#x27;;
      position: absolute;
      left: 0;
      top: 0.5em;
      width: 6px;
      height: 6px;
      border-radius: 50%;
      background-color: var(--primary-color, #3b82f6);
    }
    
    /* 内容区块样式 */
    .content-section {
      margin-bottom: 38px;
    }
  &lt;/style&gt;
&lt;/head&gt;
&lt;body class=&quot;bg-gray-50 flex justify-center items-center min-h-screen p-5&quot;&gt;
  &lt;!-- 卡片容器 --&gt;
  &lt;div class=&quot;w-card bg-white rounded-xl shadow-xl concept-card&quot;&gt;
    &lt;div class=&quot;p-10 flex flex-col&quot;&gt;
      &lt;header class=&quot;mb-6&quot;&gt;
        &lt;!-- 标题区域 --&gt;
      &lt;/header&gt;
      
      &lt;main class=&quot;flex-grow flex flex-col gap-6&quot;&gt;
        &lt;!-- 核心内容区域 --&gt;
      &lt;/main&gt;
      
      &lt;footer class=&quot;mt-8 pt-6 border-t border-gray-200 text-note text-gray-500&quot;&gt;
        &lt;!-- 来源信息 --&gt;
      &lt;/footer&gt;
    &lt;/div&gt;
  &lt;/div&gt;
&lt;/body&gt;
&lt;/html&gt;
```

### 内容优化技术
* **渐进式布局**：使用Flexbox和Grid实现内容自适应布局
* **阅读体验优化**：根据内容长度动态调整行高、段落间距
* **视觉节奏感**：通过留白、分隔和强调建立清晰的内容节奏
* **框模型控制**：使用box-border确保尺寸计算包含内边距和边框
* **响应式表现**：确保在不同设备上都能完整展示内容

### 图标与视觉增强技术
* **语义化图标**：选择最能表达概念本质的专业图标
* **图标尺寸优化**：使用较大尺寸图标(1.4em)提高可视性
* **图标与文本融合**：创造图标与文本的视觉关联，增强理解
* **视觉层次建立**：通过大小、颜色、位置和对比度建立清晰的信息层次
* **SVG图标增强**：使用SVG图标实现更复杂的视觉效果和互动暗示

### 设计准则
* 【完整呈现】确保所有重要内容完整展示，不截断核心信息
* 【可读性优先】优化排版和布局，提升长内容的阅读体验
* 【层次分明】使用视觉设计手段建立清晰的信息层次
* 【留白节奏】保持合理的留白空间，创造视觉呼吸和引导
* 【大字体设计】使用更大的字体(24-40px)提升移动端可读性
* 【宽松间距】增加行高(1.8)和元素间距(38px)，减轻阅读疲劳
* 【语义化图标】使用专业图标库表达核心概念
* 【内容驱动设计】所有设计决策基于对文章内容的理解
* 【创新表达】鼓励在技术框架内探索创新的视觉表现手法
* 【垂直阅读流】设计应优化垂直阅读体验，符合移动阅读习惯

## 核心原则
设计的首要目标是完整呈现内容的核心价值，不再受固定高度限制。采用较大字体(24-40px)和宽松间距(1.8倍行高)，为移动端用户提供最佳阅读体验。通过内容分析、分层与转化，在确保技术稳定性的同时，创造最能表达文章精髓的视觉设计。技术应服务于创意表达，而非限制想象力，鼓励在保持技术准确性的同时探索创新性的解决方案。

文本强调策略必须遵循语义驱动原则，通过智能分析识别文章中真正需要强调的内容（核心观点、关键概念、引述等），而非机械地套用格式。强调样式应与整体设计和主题情感保持一致，在增强可读性的同时提升信息层次感和视觉美感。可视元素应足够大且对比度高，确保在手机等小屏幕设备上依然清晰可辨。
```

用户提示词

```
文章标题：{{title}}
把图片整合到概念卡片中：{{img}}
直接给出html代码，不要其他任何内容
```

第七步：代码显形 - HTML 转图片

![](img/79c0fccc07e110fbb88e9d5fcb7e1e86.png)

+   **目的：** 将大模型生成的代码变成可见的图片。

+   **操作：**

1.  添加一个“HTML转图片”的插件节点 (例如 gen_img 或类似功能的插件)。

1.  配置插件输入：将其 html (或类似名称) 参数，引用“大模型”节点输出的 html_code 变量。

![](img/221aa79ffdbcf156d767a78efd2f1ccc.png)

第八步：完美收官 - 配置结束节点

![](img/ce6a11849672abcc45c50244136dfd0a.png)

1.  点击“结束”节点。

1.  配置输出：选择上一步“HTML转图片”插件生成的图片作为最终输出。这样，每次运行工作流，你就能直接得到这张精美的文章卡片图片了！

# 🌟 总结与展望

通过 Coze 的可视化编排能力和 DeepSeek V3 强大的代码生成能力，我们成功地将一个原本需要专业设计师和前端工程师协作完成的任务，变成了一个自动化的流程。

**这不仅是一个能帮你“美化”分**享链接的小工具，更展示了 AI Agent + 大模型 在自动化内容创作与传播领域的巨大潜力。你可以：

+   **改造它：** 调整 Prompt，改变卡片风格、尺寸、配色，适应不同场景，特别是图片提取部分（如👸号、🧣）。

+   **扩展它：** 结合更多 Coze 插件，实现自动发布到社交媒体等高级功能。

+   **启发思考：** 还有哪些重复性的、基于内容理解和生成的工作，可以用类似思路实现自动化？

> 嗨，我是Yeadon！

> 一名人工智能专业的大三学生，正在努力成为AI时代的超级个体~

+   ** **欢迎加我交流讨论！ 一起探索AI的无限可能！

+   个人主页：[Yeadon（伊登）的个人说明书](https://q05hlvbg0wa.feishu.cn/wiki/Nn5ZwIwv1iaYnEkDH6KckY7sn2g?from=from_copylink)

![](img/30352984188b652a7e591833b3c3012c.png)