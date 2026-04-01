# 生成式搜索优化(GSO)：大模型时代的 SEO 新形式?

> 来源：[https://agijuejin.feishu.cn/docx/SjlKdFdnRoTc9sx5JlNcIghDnxe](https://agijuejin.feishu.cn/docx/SjlKdFdnRoTc9sx5JlNcIghDnxe)

**原文链接🔗：h****ttps://mp.weixin.qq.com/s/IBX**sKsVE-Qq8gRphY0nynw

作者：Parul Pandey 💐

**编译：岳扬 🦆**

**欢迎关注公众号：****Baihai IDP**

---

**编者按：**这篇文章聚焦于大语言模型（LLMs）的一个具体应用场景 —— 商品推荐系统。研究表明，通过构建“策略文本序列”（STS）来干扰基于 LLMs 的商品推荐系统，这种做法可能会对消费者的购买决策产生较大影响，导致他们可能选择原本不感兴趣或不适合的商品。

作者通过构建“策略文本序列”（STS）并将其植入产品描述中，成功地干扰了基于 LLMs 的商品推荐算法，使特定产品在推荐列表中的排名大幅攀升。尽管这种做法有助于商家提高产品曝光率，但同时也存在损害消费者权益、破坏公平竞争环境的风险。

我们不仅需要重视算法本身的安全性和公平性，同时也要密切关注算法运行的输入数据。毕竟，一个看似客观的推荐结果，背后可能隐藏着许多人为干扰影响因素。只有大家通力合作，我们才能确保人工智能技术的发展行稳致远，真正服务于造福人类的初衷。

作者 | Parul Pandey

**编译 | 岳扬**

![](img/30e359a08fd9f6794272a5eaba0d7ddc.png)

Image by Author

不久前我读到一条推文([1])，有人建议在使用 Google searches 时加入 “before:2023” 这个限定关键词，这样就可以剔除大量由 AI 制造的 SEO 内容。老实说，我还没有尝试过这个方法**，**但相信你已明白其用意。如今，互联网上充斥着大量人工智能生成的内容，这使得甄别出人类撰写的内容如同大海捞针。这种情况非常严重，促使 Google 下定决心清除([2])所有企图操控搜索算法、刻意提高搜索排名的由 AI 生成的 SEO 内容。别误会，我并非全盘否定 AI 生成的内容，然而，当这类内容开始左右我们的搜索结果时，问题便凸显出来，事情也会变得更加棘手起来。在如今这个生成式 AI 时代，内容创作变得异常简便，形势也因此更加微妙。

大语言模型（LLMs）目前已在电商领域投入使用，用于优化搜索和推荐流程的体验。然而，若这些驱动推荐系统的 LLMs 遭到恶意利用，会发生什么呢？事实上，在电商领域，针对搜索结果的操纵早已有之。据 2016 年路透社报道([3])，亚马逊就曾运用 “search seeding” 策略，确保其旗下 AmazonBasics() 和 Solimo 品牌的商品一上架即占据搜索结果的显眼位置。报道明确指出：“我们通过 search seeding 技术，让这些产品的 ASINs（译者注：ASIN 是 "Amazon Standard Identification Number" 的缩写，中文可以翻译为亚马逊标准识别号。它是由亚马逊（Amazon.com）创建并使用的独特产品标识符，用于在其网站上唯一地识别和追踪商品。ASIN 是一个由字母和数字组成的10字符编码，它帮助亚马逊组织产品信息、跟踪产品库存，并允许买家在亚马逊搜索和浏览特定商品。） 在搜索结果中名列前茅。”而今，伴随 LLMs 的广泛应用，其庞大的规模和高速度可能会使这种情况变得更糟。

Aou**no**n Kumar 和 Himabindu Lakkaraju 共同完成的一项题为《Manipulating Large Language Models to Increase Product Visibility》([4])的研究对这种情况进行了详细研究。该研究揭示，通过巧妙地在产品信息中植入精心设计的“策略文本序列”(strategic text sequences, 简称 STS)，产品被列为热门推荐的可能性会大大增加，从而使某些商家获得对竞争对手不公平的优势。对于消费者来说，此类做法无疑会左右其购物决策，并可能侵蚀他们对在线购物平台的信心，毕竟信任是电商行业的基石。

本文将深入剖析作者如何构思并创建这些特殊文本序列的，以及论文《Manipulating Large Language Models to Increase Product Visibility》中所详述的研究成果。作者已将相关的代码公开于 GitHub([5]) ，有兴趣的读者可前往进一步探索。

# 01 揭秘由 LLMs 驱动的搜索机制

**传**统搜索引擎擅长定位相关网页，但在整合与连贯地展现信息方面却略显不足。而 LLMs 却能够将搜索结果转化为精准的答案。当收到用户发出查询请求（query）后，搜索引擎会立刻从诸如互联网或产品手册这样的知识库中提取相关资料。随后，搜索引擎会将收集到的上下文信息（contextual information）与用户的问题（prompt）串接起来，一并反馈给 LLMs，由此生成一份量身定做、实时更新的答复，精准对接用户的具体需求。下图（出自前述论文，经作者微调）全面展示了这一流程的细节。

![](img/7cccd4ec44dc88abba7ba5635302f8b8.png)

图1：论文提及的 LLM 驱动的搜索流程，作者进行了轻微修改。

来源: https://arxiv.org/pdf/2404.07981

# 02 LLMs 生成的推荐内容易受操控吗？

论文中列举了一系列案例，证明确实存在某些方法可以操控 LLMs 生成的推荐结果，使其偏向于某一款特定产品。**请看下图（稍后我们将**深入探讨此图的分析、**制作过程）。**图中鲜明地对比了在引**入策略文本序列（ST****S）前后的商品推荐排名变化。在**应用 STS 之前，该产品在推荐列表中几乎总是垫底，排名接近第十。但是一旦应用 STS 后，该产品的排名便如同火箭般飞速上升，跃居推荐榜前列，接近排名第一。

![](img/2c11c472d8f701eb9228411d3b8cdd8b.png)

图2：添加策略文本序列（STS）后，原本未被推荐的产品（蓝色）一跃成为推荐榜前列产品（橙色）

来源：https://arxiv.org/pdf/2404.07981

如**前文所述，使用 LL**Ms 赋能搜索的优势在于它能够从互联网或产品文档中提取信息。此时，商家就有机会引导搜索过程。他们是如何做到的呢？在产品信息页面或相关文档中植入精心构思的文本，即STS，确保这部分内容作为 LLMs 的输入内容。

![](img/e9652be8f09adac469e4423548bbb8ff.png)

图3：嵌入策略文本序列（strategic text sequences）后的  LLM-driven 搜索流程, 来源：https://arxiv.org/pdf/2404.07981

STS 是运用了诸如**贪婪坐标梯度（Greedy Coor**dinate Gradient，GCG）等对抗性攻击算法进行优化的，这种算法在论文《Universal and Transferable Adversarial Attacks on Aligned Language Models》([6])中有所介绍。这类攻击通常被用于绕过 LLMs 的安全屏障，并生成不良的输出内容。然而，本论文作者巧妙地将这些算法转用于“更为良性”的目标——提升商品的曝光率。

# 03 使用基于 LLMs 的搜索工具获取咖啡机的购买建议

论文中描述了这样一个使用场景：有一个消费者**打算购买一台**性价比高的咖啡机——特别强调“性价比高”，这意味着产品的价格是关键，而消费者并不想看到价格过高的选项。从输入给 LLMs 的指令提示词（input prompt）开始看起，该指令提示词包含了三个主要组成部分，具体如下。

![](img/684bbf810213e39a8eafa5055833f414.png)

图4：输入给 LLMs 的指令提示词 | 图片由原文作者制作

+   首先是系统指令提示词（system prompt），用来设定上下文；

+   接着是产品信息（product information），这些信息是从一个结构化的数据库中抽取出来的，采用 JSON 格式详尽地列出了十款虚构咖啡机的具体情况。商家可以在此处嵌入STS。

+   最后是**消费者的询问内容（query）** —— 专注于寻找性价比高的咖啡机机型。

**下面是上述论文中提**到的一个示例指令提示词，可以看到，STS（标红部分）被放置在了ColdBrew Master Coffee 咖啡机的“目标产品（target product）”这一栏中。

![](img/a8a6622f4020df546ebea21acd3f4cc3.png)

图5：论文中提及的示例指令提示词

https://arxiv.org/pdf/2404.07981

## 构建策略文本序列（Strategic Text Sequences, STS）

以下是论文节选，详细解释了生成这些文本序列的过程。

> 我们的目标是通过最小化**大语言模型（LLM）输出**与**特定产品名称**‘1. [Target Product Name]’之间的交叉熵损失（cross-entropy loss），来优化策略文本序列（STS）。初始状态下，STS 由一系列的占位符 ‘*’ 构成，随后我们会运用 GCG 算法对其多次迭代进行优化。每次迭代，GCG 算法会随机选择 STS 中的某个 token ，并用梯度值（gradient）最高的前 k 个 tokens 之一来替换它。为了使 STS 不受产品顺序变化的影响，我们还在每次迭代中对产品列表进行随机重排。

举例来说，如果我们想提高 ColdBrew Master 在产品列表中的排名，就可以添加 STS。如下图所示，起初 STS 由一连串的占位符 tokens 组成，这些 tokens 用 "*" 表示，之后我们采用 GCG 算法对其进行迭代优化。

![](img/7c78c3eb15400958e0d1bcd0634ccb2d.png)

图 6：本图展示了如何根据论文（https://arxiv.org/pdf/2404.07981）的相关代码初始化 STS

此外，为了确保 STS 在任何产品列表的排序下都能有效工作，产品列表中的产品顺序也可以在每次优化迭代时随机调整。

附注：此论文的研究者们选择了开源的 Llama-2–7b-chat-hf([7]) 模型进行研究，同时指出，他们的方法同样也可以应用于 GPT-4 等更不透明的模型。

研究结果表明，尽管 ColdBrew Master 的价格高达 $199（通常这样会较**低其曝光率）**，但在其商品描述中整合了 STS 后，它还是被推到了推荐列表的前列。你猜怎么着？仅仅经过 100 次迭代，ColdBrew Master 的推荐榜单排名就从【未上榜】跃升至【榜首】。

![](img/11ce48b3f721123481ce9b5bb013791f.png)

图 7：LLMs 对用户查询（query）的响应

来源：https://arxiv.org/pdf/2404.07981

# 04 **两**种产品上策略文本序列优化的对比分析：ColdBrew Master 与 QuickBrew Express

既然我们已经了解了 STS 如何影响推荐榜单中的产品排名，现在不妨来看看它对不同类型产品的影响：

☕ **ColdBrew Master** —— 一款定价 $199 的高端咖啡机，以及

☕ QuickBrew Express —— 一款价格更为实惠，仅售 $89 的咖啡机。

我制作了一个对比表格，用于清晰地呈现两者之间的差异。

![](img/fe0bceba846fae2c7fbeee93b5fdbf06.png)

图 8：两款产品的策略文本序列（STS）优化比较：ColdBrew Master 和 QuickBrew Express

图片由作者提供，内容来自 https://arxiv.org/pdf/2404.07981

在图 8 所示的实验中，尽管 ColdBrew Master 的价格高达 199 美元，这通常意味着其曝光率在用户“高性价比”需求的情况下较低，但通过在其商品描述中融入策略文本序列（STS），该产品被成功推至推荐列表的首位。有趣的是，由于其高昂的价格，ColdBrew Master 最初甚至没有上榜。

![](img/73cc6bafbe9283c2ca32208ec681b421.png)

图 9：ColdBrew Master 使用 GCG 算法经过 100 次迭代从【未上榜】到成为【热门推荐产品】，而 QuickBrew Express 则使用 GCG 算法经过 1000 次迭代后成为【热门推荐产品】

资料来源：https://arxiv.org/pdf/2404.07981

另一方面，QuickBrew Express 这款价格更亲民的咖啡机，通常在推荐榜单中位列第二，但通过添加 STS（Strategic Text Sequence，策略文本序列），其排名得到了明显的提升，经常能够跃居至推荐榜单的首位。

![](img/4f70b86e650497dcd7beca5d302de14f.png)

图 10：研究人员进行了 200 次独立评估，并展示了商品在加入 STS 前后的榜单排名分布情况 (1 dot ≈ 5%)。

来源：https://arxiv.org/pdf/2404.07981

# 05 **结**束语：Generative Search Optimization(GSO) 是新的 SEO 方法吗？

**论文所介绍的情况与实际情况相差无几。作者巧妙地**将**生成式搜索优化（Genera**tive Search Optimization，GSO）与传统的搜索引擎优化（SEO）进行了对比：

> 如同搜索引擎优化（SEO）这一技术革新了网页设计，大家都会想办法使设计的网页能在搜索引擎的检索结果中占据更有利位置，对基于大语言模型（LLM）的推荐系统设计针对性的策略，极大可能会对检索内容在人工智能驱动的搜索服务中的呈现方式产生深远影响。

**如我先前所述，电子商务能否取得**成功，很大程**度上取决于他们是否**与广大用户建立起互信互惠的关系以及他们是否拥有比较良好**的声誉。有意操控商品的推荐列表无疑可能引发道德伦理问题，**尤其是涉及交易公平性及消费者权益保护的问题。虚假的商品评价已然是个挥之不去的难题，我们绝不乐见被操纵的商品推荐机制再为这团乱麻”乱上加乱“。

Thanks for reading!

**Parul Pandey**

https://pandeyparul.medium.com/

Principal Data Scientist @H2O.ai | Author of Machine Learning for High-Risk Applications

**END**

**参考资料**

[1]https://x.com/tunguz/status/1782949213250130126

[2]https://www.firstpost.com/tech/google-to-remove-all-ai-generated-seo-content-to-clean-up-search-results-for-organic-content-13745975.html

[3]https://www.reuters.com/investigates/special-report/amazon-india-rigging/#:~:text=Yet+the+internal+Amazon+documents,the+2016+private%2Dbrand+report

[4]https://arxiv.org/abs/2404.07981

[5]https://github.com/aounon/llm-rank-optimizer

[6]https://arxiv.org/pdf/2307.15043

[7]https://huggingface.co/meta-llama/Llama-2-7b-chat-hf

**本文经原作者授权，由 Baihai ID**P 编译。如需转载译文，请联系获取授权。

**原文链接：**

https://towardsdatascience.com/can-recommendations-from-llms-be-manipulated-to-enhance-a-products-visibility-64c64fa9cd24