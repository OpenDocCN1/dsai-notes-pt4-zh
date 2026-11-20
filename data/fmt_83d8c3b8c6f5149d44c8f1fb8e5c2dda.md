
> https://manus.im/share/brWKUSp51ItvVMBpcXNCZ1?replay=1

**写一本自定义主题的小说** [点击直通车](https://manus.im/share/Aa0M0gvOybi9cRwDK0RNpx?replay=1&mark_id=999_reallog_mark_ad%3A999%7CWeiboADNatural)

**财务估值模型搭建** [点击直通车](https://manus.im/share/hRGARE7EBqtDhLHBGISLP7?replay=1&mark_id=999_reallog_mark_ad%3A999%7CWeiboADNatural)

**泽连斯基的白宫互动游戏** [点击直通车](https://manus.im/share/Sc5a78DH57lQ5aYJbVAvZp?replay=1&mark_id=999_reallog_mark_ad%3A999%7CWeiboADNatural)

**谷歌CEO模拟器** [点击直通车](https://manus.im/share/cmRIphYJybxNiLSkWn6PJn?replay=1&mark_id=999_reallog_mark_ad%3A999%7CWeiboADNatural)

**泽 连 斯 基 VC 万斯**

> 提示词

> 让 **Manus** 写了一个游戏，让你扮演在椭圆办公室的泽连斯基，面对拜登和万斯的攻击

> 看看你能不能走出椭圆办公室

![](img/03176d1776759a99666b57c7a80f3364.png)

**北京买房**

![](img/8dc5f6c2d04c6ff5c5f090b0e1d02a4e.png)

![](img/b5c468f49e60ac0e692d96d59e7110bc.png)

![](img/07b808e3a940ecfb9ca873a588a88c2c.png)

## 六、拆解Manus工作流

> 核心内容：

1. 通用Agent Manus的前沿特性与应用场景

1. Workflow：任务规划与`md`文件管理的独特结合

1. 从技术层面深入分析Manus的创新与应用实例

昨晚全球首个通用Agent Manus预览版正式发布，其官方展示的效果令人惊叹，展现了强大的任务规划与执行能力。目前还没拿到邀请码，以下是通过对官方网站案例的拆解，以下将从workflow，memory和前端交互三个方面进行拆解，总的来说就是整合了 computer use、deep research、coding agent等概念的产物。

“Manus 都能通过独立思考和系统规划，在自己的虚拟环境中灵活调用各类工具——编写并执行代码、智能浏览网页、操作各类网页应用——为你直接交付完整的任务成果，而非仅仅提供建议或答案。”

### 一、workflow

1. plan规划 ——创建`todo.md`

首先创建to do task list，并通过创建`md`文件来进行管理更新作为agent记忆，这与业界很多通过plan agent的记忆上下文来管理task的思路不同。

示例：7-Day Japan Itinerary with Proposal Ideas

![](img/5dc50aabedd4b606a8209926a656a8d3.png)

`todo.md`示例

![](img/bb3b2817bc142733f8484e33d35f7714.png)

![](img/3314435cd72e6460c75b41a181de6f78.png)

### 2、To do

computer use、deep research、coding agent

Manus将会根据`md`文件的to do list去逐一完成每个板块，其中每个板块的具体实现涉及大量的computer use、deep research以及coding agent。

![](img/c8b51691c52921e9c559e3a8a81a6912.png)

#### 2.1 deep Research

1. 搜索网页（一次检索23个结果）

![](img/ed58780d5e04b0b4e5a2566bdc512c5c.png)

2. Browser use

Manus 通过browser use 去实现网页的浏览和操作（如滚动，点击事件），每一步操作都会截图保存。

- 浏览

![](img/f829f36a88770d7097c784fde7e93eff.png)

- 向下滚动

- 点击

#### 2.2 computer use

虚拟机环境的交互，比如涉及虚拟机终端命令运行，浏览器的交互，文件的管理（增删改）等。

- 终端交互：

![](img/4976e2afa97206af9954056f3da5d81b.png)

- 浏览器的交互：如2.1 browser use

- 文件管理

![](img/272558e6101e5022e26b27e20511e1c5.png)

#### 2.3 coding agent

manus将涉及到代码的任务分发给coding agent进行完成，如`html`，`py`等文件的编写，coding agent 这块官方的实现效果有点像是用claude的模型实现的。

![](img/a05df619f1d008f5eeb949caa3f37ce5.png)

### 3.更新`todo.md`文件

综合以上，Manus会根据`todo.md`文件的事项清单通过computer use、deep research以及coding agent的结合逐步去完成任务。随着任务不断进行，每完成一定任务会编辑`todo.md`文件标识`[x]`已完成进行更新`md`文件（agent的上下文记忆）

![](img/ce8483684499d7bcb41821f147a39197.png)

示例：7-Day Japan Itinerary with Proposal Ideas

![](img/c99bb0aab578ba54a4ef94ea2c722985.png)

- 研究日本旅行基础知识创建项目目录结构研究最佳交通选择（JR Pass、当地交通）研究四月份的天气检查美国公民的签证要求研究货币兑换和预算分配查看从西雅图到日本的航班选择
- 研究城市和景点研究东京的景点和活动研究京都的历史遗迹和文化体验研究奈良鹿公园及周边景点研究大阪的美食和文化体验寻找隐藏的宝藏和非热门地点研究文化体验（剑道、茶道、禅修）
- 规划7天行程创建每日行程并安排时间规划城市间的交通平衡城市探索与文化体验留出放松和自发探索的时间确保行程符合预算限制（2500-5000美元）
- 研究求婚地点确定风景优美且浪漫的求婚地点研究所选地点的最佳求婚时间规划求婚时刻的细节
- 整理日语短语和旅行建议创建常用日语短语清单整理文化礼仪建议研究餐饮习俗和美食推荐整理安全和紧急信息
- 创建HTML旅行手册设计HTML手册结构创建地图部分，标出关键位置编写景点描述加入日语短语部分添加旅行建议部分确保设计适配移动设备
- 完成并交付文件审查并最终确定7天行程测试HTML手册功能整理所有文件和资源向用户交付最终成果

### 4.完成交付

从`todo.md`可以看到最后一项任务是完成并交付文件

示例：7-Day Japan Itinerary with Proposal Ideas

![](img/b7d241ba86cbe092bb7dd225c5730237.png)

这一套工作下来会产生多个交付文件

![](img/f2eaf0797a054a8cf4249ef93c2350de.png)

为了增强用户体验，官方也设计了一个管理会话文件的地方方便用户管理。

![](img/1d0e19df71ef97b9553753406fa129a5.png)

### 二、memory

Manus有自己的知识和记忆，知识让 Manus 能够学习您的偏好和任务特定的最佳实践。Manus 会在需要时自动回忆相关知识。这样就可以教Manus在下一次处理类似的任务会怎么做？

![](img/1175aacf2d33a32ed849e5965a05a2a4.png)

这个知识关键是描述清楚让Manus知道在什么情况下需要用到这个知识。

以下是官方视频demo中提到的示例，“在为招聘目的筛选简历时，直接将结果汇总到电子表格中”，这样下次Manus处理类似任务就会继续通过表格汇总实现

![](img/1e92e6b4bb0f102b0105d311870ad71f.png)

### 三、前端交互

Manus的交互效果做的非常惊艳，特别是会话回放的流式输出效果以及右侧的实时进度跟踪。那这样就可以实时知道Manus完成任务的进展到哪了。

![](img/af0fea7d785bd56b4293cfa84cef2397.png)

![](img/1dea99a012fadf87a5579aa34436c267.png)

总的来说，Manus就是一个整合了 computer use、deep research、coding agent等概念的会规划并执行的Multiagent system ，并在前端交互用户体验上做到了极致。正如官方说那样：

“我们坚信并践行 less structure more intelligence 的哲学：当你的数据足够优质、模型足够强大、架构足够灵活、工程足够扎实，那么 computer use、deep research、coding agent 等概念就从产品特性变为了自然涌现的能力。”

Manues 的创始团队，是很久的朋友了，各个顶呱呱。

上线前给留了个码，让我能来测测这一产品。

顺道着，摸了点独家消息。

![](img/5aad79f553f616e91369c936b23f91d2.png)

我先随便跑了个．．．非常离谱的任务：

帮我整理一份逐月编年史，纪录从 2022 年到现在，AI 行业每个月都发生了哪些事情。整理完之后，再从现在的视角回顾下这段历史

然后．．．迎来了我的一声声“卧槽“，首先．．他清晰的去规划任务

然后，开着浏览器导出去吧啦信息．．．

![](img/f06ded4094b6efb5ded45c17a749745c.png)

还会不断的回顾矫正。。。

![](img/3d5bf887667a91245043848772d30d81.png)

没错，是我想要的 Agent 了！

（不过这个 case 实在太大了，跑了几个小时还没跑完）

同时的，我去跑了另一个 Case：写个游戏

帮我写一个DOOM的网页版游戏，高保真，我可以用鼠标和键盘来玩

这里补充说一下：《Doom》（《毁灭战士》）是由 id Software 开发并于 1993年 发行的里程碑式第一人称射击游戏（FPS）。它不仅是电子游戏历史上的经典之作，更被认为是现代FPS类型的奠基者之一。

在我把需求给到 Manus 之后，就看着他一阵捣鼓。

![](img/513f67cd97a70fceb25447ae5dc17b7c.png)

而最终呢，不但完成了任务，甚至帮我部署好了

https://etuswgwm.manus.space/

顺道还给我留了个手册

当然了，这里还有茫茫多的 case，来自官方

Manus 通过资料研究、数据分析捕捉了过去四个季度市场对 Amazon 的情绪变化。

![](img/8de199cae4ab027e1034b3d372ad9c0a.png)

Manus 全网深入调研找到最符合需求的货源

![](img/98c343a609361a9ffa559aad95f4c5c9.png)

让 Manus 根据你的个人主页或简历制作一个有设计感的名片。

![](img/63bb4a142495d86c22b7f411482334bb.png)

Manus 将深入理解和分析你的家庭情况和需求，并去专业的房产销售网站为你筛选最符合需求的标的。

![](img/b7db9e3a9a363e8e7b0a294b9d9bf751.png)

来看看 Manus 是高效合理的安排 40 位候选人的时间

上传你的课堂录音，让 manus 为你做一个图文并茂的课堂笔记

Manus 为你全面个性化整理高质量的强化学习学习资源。

![](img/09daa28a02bad1351dd9cf7fd2fd6d08.png)

Manus 可以为你从各个渠道收集你关心的某个事件关键人物的公开观点

![](img/a8a21db51ce358c78d6fc06585f14d2e.png)

买保险的时候要对比太多 Policy 条款？交给 Manus 为你制作清晰的关键信息对比表，给出你最佳决策建议

![](img/60924be446e11b8d06de212613ad4264.png)

枯燥但需要打起十二分精神来完成的繁琐的合同审查工作，也是 Manus 非常擅长的。Manus 将会根据你的合同先去研究和学习相关的法律法规，再有理有据的告诉你的合同目前存在的纰漏。

![](img/ca85ff7f512aa30d81e4773cc3211695.png)

把你的 API excel 文档交给 Manus，让他为你转换成一个详尽的 doc 版本的 API 文档。

![](img/34ba1afdf8258e2db6ee61135dda49f4.png)

Manus 深入文献研究 Pitera 这种化妆品成分，并撰写了 Youtube 视频讲解脚本。

![](img/f5cec28f82bf1d1d76e1c5a419ab3232.png)

Manus 通过公开信息的深入搜集和分析，绘制一份公司组织关系图。

![](img/84e3e77a3ca55f755436e374fd776425.png)

我们让 Manus 看 Techcrunch 的官网设计风格，并根据这个风格设计了一套 Slide 可用的 icon 图表库

Manus 根据你的喜好为你定制一段任意长度的冥想引导音频

![](img/b291a47dee3e3bc8b17a5888f35f8769.png)

Manus 深入进行股票分析，并制作惊艳的 Dashboard 为你展示股票的详细信息

![](img/2248d9069cb053887fe8c00d80cc971e.png)

Manus 可以为你定制化你想要的各种奇怪的音效，来听听 Manus 为我们做的鸟鸣混合蒸汽的音效

![](img/c0243be74c705791c5e392e651339873.png)

Manus 可以深入分析地区 POI 数据，交叉验证官方统计数据，深入分析某地病患人口统计

![](img/2ed4ae7195c9bb27296c4f4bde19a546.png)

Manus 深入搜集了洛克菲勒家族并将家族关系整理成了清晰的关系图表

![](img/7a50c1b23f7a6a030a5dd45f93bd9810.png)

Manus 通过资料搜集、分析并发挥想象力为编剧创作者提供资料支持和灵感启发

![](img/523a22321a86c3c4cac6d0314cbffb36.png)

根据财务记录文件审核公司的财务报表

![](img/15abb548ce97fa0de8e937b1b7d2308f.png)

描述你公司的业务和你期待的潜在 B 端客户画像，Manus 将为你深入 Mapping

Manus 为一家得克萨斯州的 BBQ 门店深入进行门店周边的数据估算和分析并提供销售额提升策略报告

![](img/3add0910b8d6e05f1033df2a2e5e16df.png)

当你需要学习某个开源项目时，manus 会深入代码库理解代码，绘制系统结构图，并为你深入讲解项目细节

![](img/6766dec8ef60fb76cf1cf61b2bddb388.png)

上传室内平面图，Manus 学习室内灯光设计科学后为你设计室内灯光

![](img/dce37e48206529e6d452ebfb0cee8af0.png)

Manus 为你制作宇宙大小测量的互动课程网页

![](img/b2d3ac23c92c40097cf904e7a0464ec6.png)

为中学教师制作动量定理讲解的视频展示课件

![](img/75962952ba7099c997ece466c8b5061f.png)

Manus 为你清晰、惊艳的制作 Transformer 架构的互动讲解网页

Manus 可以代理你深入体验某个网站的功能并作为一个产品专家为你写详尽的产品体验报告

![](img/7c61f68dba3485c7515b30a96eca8a71.png)

Manus 可以为记者或投资人深入整理待访谈人的全面信息并准备访谈所需的所有材料

![](img/2db7f65078e9dcfa4687452c916efa40.png)

为你的复杂租赁合同制作一份关键信息表格

![](img/57a022ed66501b071eaf3bd3ac275f2a.png)

我们想要 YC w25 batch 中所有的 B2B 公司的名单，Manus 自己操作 YC 官网筛选出符合要求的公司，并高效完成任务

Manus 为我们讲解莱克星顿第一枪的战役过程时，配上了自己画的可视化地图

![](img/398ed161832d26743dffaae06bd3d15a.png)

你下一场的演讲需要个逐字稿提词器？Manus 将贴心地为你制作一个可控制速度、可控制字体大小、阅读清晰的 presentattion 文件

让 Manus 深入研究美国在感恩节期间的各州移动互联网流量消费情况并制作丰富的可视化图表

![](img/86e5e2020202db3f69a83aada819bb92.png)

按照要求深入分析英伟达的财报

![](img/a4f7e1fe01feed452a0abab186bac064.png)

上传你的 Amzon 网店销售数据表格，Manus 为你深入分析、可视化并提供销售提升建议

![](img/6648f2d39f67b4f5c1d45872ead0afeb.png)

给 Manus 一个 Kaggle 竞赛网址，Manus 自己完成了代码编写，预测结果输出。一次打到了前 10%的成绩

Manus 为你定制个性化的小游戏。来试试泽连斯基白宫论战模拟器
