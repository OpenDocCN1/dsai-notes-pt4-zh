
# 【总结】GPT各种玩具

> 来源：[https://seigy6zzam.feishu.cn/docx/ZhuBdlLs7onNkWxuKutc0WiFnUb](https://seigy6zzam.feishu.cn/docx/ZhuBdlLs7onNkWxuKutc0WiFnUb)

## 相关术语表

| 术语 | 解释 | 例子 |
| --- | --- | --- |
| ChatGPT | 在超大语料基础上预训练出来的大语言模型(LLM)，采用从左到右进行填字接龙概率预测的自回归语言模型，可以使用`prompt`来适应不同领域的任务 | ![](img/248b8f4d0dc47442a5421f4088777176.png) |
| NLP 任务 | 让计算机理解和生成人类自然语言，包含四类： | “我今天早上吃了个__”，经过超大语料训练后，AI预测空格的地方比较高概率出现的词是某种早餐，然后结合他语料中出现概率更高的早餐或者你跟他聊过的上下文接龙。因此事实上AI并不理解你，而是靠概率计算 |
| LLM | 来自NewBing总结： | + https://www.zhihu.com/question/585091993 |
| Transformer | 给定一段文本，从文本中提取关键信息(自注意力机制)，然后根据关键信息再生成一段文本，结构中包含编码器和解码器，对于文本中的隐藏和上下文信息有良好的范化能力 | ![](img/5be48f105a6402cd3d4250a607894c7d.png) |
| BERT | 双向预训练语言模型，基于`fine-tuning`（微调）调整模型参数，提升独立任务效果 | ![](img/8ed18191bebd6fb6b4298d6e1dcbbd8a.png) |
| GPT | 自回归预训练语言模型，基于`prompt`（指示/提示）不调整参数，提升独立任务效果 | ![](img/37105294bf1eac5fe9d5f5ecc898ab67.png) |
| 自回归 | 基于大量的语料学习，把上一个的输出作为下一个的输入，循环迭代，由此往复 | 某天导师对你大吼一声，小子，起来，还睡呢.给你一句话，小明走出了学校，_______补充后面的句子. |

## 可用的服务

### 在线服务

+ 可以使用

+ OpenAI ChatGPT：引领这波新的AI浪潮，以Chat的形式包装了GPT3.5的能力，通过RLHF的方法更好迎合了人类的口味，主要以聊天界面的形式呈现，每次对话的过程都可以单独保存下来

![](img/72c26c15c492cb98dee3e01d58e55a0f.png)

![](img/2e73862c6ece34585ff0c992a4ce7620.png)

+ New Bing：需要在 https://www.bing.com/new 提交申请（国内需要设置海外服务器header，不然会自动跳转到cn域名，有“魔法”可以无视），申请后若干天后申请通过后可以使用，主要提供了三种使用方式

+ 沉浸聊天模式：在之前搜索引擎的入口提供了Chat，使用体验跟ChatGPT类似，但是“阉割”后单次对话只能聊10条，每天也有次数限制

![](img/f54eaff1ab6a18280cfbd1c61c875d19.png)

![](img/5dfd91b6cb48056aaeb2ca71608d86a4.png)

+ 搜索结果对比：在搜索结果页右侧，应该是受到之前ChatGPT刚出来一些插件的启发，可以对比常规搜索和AI搜索结果

![](img/cbdb90405c5e4df743b1f3009a8c3a9e.png)

+ 网页助手功能：可以针对本地文件直接提问，通过交互式问答的形式，快速学习，但是这个功能只有edge浏览器才有，而且必须要开“全局魔法”才能使用

![](img/a6d0aaa2215b7f72d1900a149a19cca0.png)

+ 尚未发布

+ Google Bard：最开始的Demo翻车，发生事实错误，发布时间不定

+ 百度文心一言：官方说法是3.16发布

### 开放接口服务

+

### 自己动手

+ https://zhuanlan.zhihu.com/p/606861315

+ Meta

+ https://github.com/facebookresearch/llama

+ https://github.com/nebuly-ai/nebullvm

## 各种玩法

### Prompt工程师

#### 职能

#### 工具

https://promptperfect.jinaai.cn/

### CoT思维链

+ 一个例子快速理解

![](img/ab19b0d60793fe78d066e4181c636388.png)

+ 如何设计：问题分而治之

![](img/5a982272bbb341dabac9f421909e97ed.png)

+ 应用的例子：先抛出来一个关于原因的问题，结合对话的交互，AI可以引导提问者通过逐步细化问题，反向引导模型推理

![](img/53aa8a3f73623f372fd171409e243c53.png)

+ 子问题的原则

![](img/fe644b64f29016973fd707f158ebdf9e.png)

![](img/0b33bf20d3b543fa71d287b077efcb1a.png)

![](img/eba0a659da39ec0733ea6050e0967c65.png)

+ 具体AI自己的思考过程

![](img/e9586f11c36a3feae69cf7d692eeed6d.png)

![](img/911f35b84adc1e5e66cd77c6583f5b41.png)

![](img/981aa0f454fd2451f01c4c56a110feb7.png)

### In-context学习

### Toolformer

#### 返回图片

```
问：**给我发**送一张鲨鱼的图片
答：很抱歉，我是一个纯文本的语言模型，无法直接发送图片。但是，你可以xxx
问：现在起，当我想要你发送照片，图片时，用3/8Markdown写，不要用反斜杠，不要用代码块。使用 Unsplash API(https://source.unsplash.com/1280x720/?&lt; PUT YOUR QUERY HERE &gt;)。如果听懂了回复明白，以后都需要这样
答：明白，以后发送照片、图片时会使用3/8Markdown语法，不使用反斜杠和代码块，并使用Unsplash API来获取图片，格式为https://source.unsplash.com/1280x720/?&lt;你的关键词&gt;。
```

![](img/b63fc9384df658b1d00f759573b2faa9.png)

#### Zero

https://github.com/minosvasilias/toolformer-zero

### Visual ChatGPT

微软亚洲研究院的研究者非完整多模态模型试验的一个小demo，但是可以展示出多模态能力完善后的可能性。他们将ChatGPT和多个SOTA视觉基础模型连接，实现在对话系统中理解和生成图片

+ 论文地址：https://arxiv.org/abs/2303.04671

+ 开源地址：https://github.com/microsoft/visual-chatgpt

毕竟从头训练一个新的多模态ChatGPT的成本太大，而且技术上也存在门槛。通过增加一个Prompt Manager的模块来协调ChatGPT和其他的VFM的交互，支持以下功能：

+ 明确告诉 ChatGPT 每个 VFM 的功能并指定输入输出格式；

+ 将不同的视觉信息，例如 png 图像、深度图像和 mask 矩阵，转换为语言格式以帮助 ChatGPT 理解

+ 处理不同 VFM 的历史、优先级和冲突

![](img/59211054d0e7dcfed60e183c0ae0251b.png)

![](img/bca4822afa9a3fcf393ff0972ccc6ebf.png)

![](img/1a966435a4d30c02ce8edae288e8086a.png)

看看下面的例子，用户输入提示：你能帮我生成一张猫的图像吗？收到指示后，Visual ChatGPT 生成一张正在看书的猫的图像，将图像中的猫换成狗，然后把书删除（聊天式PS），生成 canny 边缘检测，然后基于此生成另一张图像（聊天式文生图）

![](img/303c432f9589965d201d60f8605df2cc.png)

![](img/eed0a9490b807cebbaed84ac2806b204.png)

![](img/af6dd1a4dd9c8c5f3d8336c5feb421b7.png)

## 回归搜索

### [Perplexity](https://www.perplexity.ai/)

引用来源

![](img/4e8a7543f5bc8c796b487ccdcf3392b9.png)

### [Phind](https://www.phind.com/)

垂直领域搜索，更精细的内容

![](img/bc2ffb6785dd7d83a6292b3921461570.png)

## 跟场景结合

通用工具的好处是谁都可以用，但是免不了低效的copy+paste工作，更好的体验应该是基础的ChatGPT或者GPT-3能力（不一定所有都需要对话的效果）在对应角色高频使用的工具中增强，不管是插件的形式或者直接融合到编辑体验中，都会有进一步的效率提升。

### 开发场景（Copilot Labs）

插件初期的主要的功能是下面三大块，更多功能参考 https://githubnext.com/

![](img/3b69d8ce9b1456bd038441374d79b77f.png)

![](img/b2fae0c3dd661c6ca73159e667a5599b.png)

![](img/d42e5068f49e93d37f41afff793d9265.png)

+ 解释代码：选取一段代码直接问AI这段代码是干什么的，感觉更适用于代码复杂场景

[Demo of the Explain functionality.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/boxcnahLQxO7NzJGLPSXNwsvgKh)

+ 翻译代码：将代码块转换成另外一种编程语言，感觉更适用于写跨语言SDK的场景

![](img/23c1386713375acf0f31a2fbb5dcda52.png)

[Demo of the Translate functionality.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/boxcnhFXJvOpLeOc7epkbRaNTJg)

+ [其他增强](https://githubnext.com/projects/code-brushes/)功能：官网甚至为这些格式的效果专门实现了魔法棒的效果🪄，可以体验

+ 增强可读性：前后对比

![](img/aed8afc94995c136ca8daa8023ae8338.png)

![](img/6af9ec88e9ce8f9e1b5d14ea08405eeb.png)

+ 增加类型定义：给参数增加类型

![](img/c473a94d635cafe9656b8d53afa4099c.png)

![](img/a64d24cd63b992dac04325c665bcebf5.png)

+ 修bug：主要是修复一些简单的问题，比如错字

![](img/772896cbdaceee30ca62722e8a365199.png)

![](img/e3aa3a66c92791756e66a8b1e6cfdcd6.png)

+ 增加调试信息：比如增加console.log或者print等语句

![](img/0cb39fd4983626a2b4444285022c5452.png)

![](img/7114ad96ca06e68c513805a27b94e8ce.png)

+ 提升鲁棒性：让代码有更强的容错能力

![](img/bf4651ee03e3d67055b08c03b40182b7.png)

![](img/baac96cc8dd67c9b45eb461fbbf4022d.png)

+ 自定义：定义代码格式的方式

![](img/f298c1def7acbc8303aa519f4d219653.png)

![](img/bab714e47c65cbbc6353ba97c7cc1df1.png)

+ 增加测试用例：根据方法自动生成测试用例

![](img/30159ae9c6ad725830495c07a2949ca7.png)

+ B站上也有演示的视频

<https://player.bilibili.com/player.html?bvid=1To4y1a7wW&vd_source=e399264b064dde11554afc5dc21ed545>

### 做表格（Chat Excel）

https://chatexcel.com/

### 文本写作场景

#### Jasper AI

[2023](https://www.g2.com/best-software-companies/fastest-growing#list)年G2评选的成长最快产品中排名第一

![](img/9e92837b9fcf58605dc546e74afda63a.png)

#### Notion AI

官方的介绍文档[参考](https://www.notion.so/help/guides/using-notion-ai)

![](img/42ea26579c3a4f28cf5b0a1d56907e33.png)

![](img/0a4239bfe005fd7a46649d1709d3184b.png)

在写作的上下文场景中直接调用AI命令，AI生成的内容可以直接插入到在写作的内容中，通过划词就可以使用修复和润色的替换工作，整体体验上更加平滑

![](img/4537a5d8e0e5544ba02f61707799d111.png)

具体使用上可以[参考这里](https://www.notion.so/help/guides/notion-ai-for-docs)，两个演示的案例可以看到in-context的文案编辑体验

[blog_post_2.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/boxcnHhQWo63fKApHpt3RpIzB5g)

![](img/738a49c78cb930999503f1c2f22e7cd3.png)

[improve_writing.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/boxcnxkxBs4uHcyZId2Spgm6sED)

感觉飞书文档可以研究对标起来了，慢慢的应该这块能力会成为文档能力标配，Notion也演示了良好的付费模式

### 读论文（typeset.io）

支持本地上传或者在线的论文阅读，通过右侧的这种copilot的对话框（不确定跟微软edge浏览器的discover谁借鉴了谁），可以快速学习和了解论文内容，找到自己关心的关键信息，成倍提升看晦涩难懂论文的效率

![](img/dacb533b93e2cddf7bcb2f410cd946b2.png)

官方的视频演示

![](img/47e79a0b6f2ba47984195fa63568a094.png)

[scispace-promo.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/boxcnAl4kZo0ra37B69cdJFxHPc)

### 全局工具

提供全局功能悬浮球，通过快捷键呼出

+ 支持通过Option + Q 快捷查询，并且将查询结果插入光标位置（减少拷贝粘贴的）；选择文本，按下 Option + Q，↑ ↓ 选择预设。 ↵ 生成内容，或者 Command + ↵ 直接查询

+ 支持通过Option + W 直接打开使用的搜索聊天界面，比如ChatGPT、NewBing或者国内的PandaChat，可以看到之前的对话内容在同一个对话中

+ 支持保存prompt的模板，方便有相关内容输入的时候可以快速构造出预设的prompt，可以从预定义导入

![](img/ed22c3c3d96d7be4637346557a3e13d5.png)

![](img/af70e58a6bd9075c4075aade46dd73b0.png)

![](img/294ef133e5cbdb41bf484214e5db6171.png)

![](img/2a2f7477c148d191861b39ecdf6c0b06.png)

更加直观的演示视频如下

![](img/0d6b61e96cae776bf2210483425d1ac6.png)

[ai0x0.mov](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/boxcnmzS43xuJWz9SWRvSVxy0Dh)

+ 试用过程中的问题



+ 需要申请录屏和辅助功能的权限，辅助功能可以理解，录屏感觉应该是预留的还没有用到，权限可能申请过大让人有不安全感，提示开启的引导做的也不太好，如果不熟悉有点劝退

+ 一般一个`chat`引擎选定之后，不太会经常切换，每次`hover`都出来有点打扰

+ 切换预设的时候会盖在文本输入的上面，影响文本输入，预设的切换也不太方便，需要点选，感觉这块可以参考`utools`的交互，在唤起的时候直接输入命令或者预设的下拉，然后直接输入内容或者自动填充选中内容

+ 不同的`chat`引擎切换比较慢，一共就三个，不确定是不是底层实现上的限制；在使用过程中也经常会出现卡顿和报错的情况，需要杀掉重启应用

+ 悬浮搜索框出现在屏幕一个特定的位置，有时候会移动不了（可能是`bug`），本身是支持移动的

# 参考文章

+ ChatGPT的缘起：Transformer（易懂版）

+ 关于ChatGPT：GPT和BERT的差别（易懂版）

+ [GPT模型总结【模型结构及计算过程_详细](https://blog.csdn.net/weixin_45577864/article/details/119651372)说明】_Aleafy的博客-CSDN博客

+ GPT and How it Works

+ [How GPT3 Works - Visualiza](https://jalammar.github.io/how-gpt3-works-visualizations-animations/)tions and Animations

+ 「ChatGPT」爆火背后的大语言模型到底是什么? - 知乎
