# 2023-02-22GPT系列的数据集之谜_夕小瑶的博客-CSDN博客

> 来源：[https://xuqiwei1986.feishu.cn/docx/HztOdAHgZoogAKxw18VcoM5rnT1](https://xuqiwei1986.feishu.cn/docx/HztOdAHgZoogAKxw18VcoM5rnT1)

> 🔗 原文链接： h[ttps://blog.csdn.n](https://blog.csdn.net/xixiaoyaoww/article/details/129173512?spm=1001.2101.3001.6650.11&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-11-129173512-blog-130176785.235%5Ev38%5Epc_relevant_sort_base2&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-11-129173512-blog-130176785.235%5Ev38%5Epc_relevant_sort_base2&utm_relevant_index=12)et/xixiaoyaoww/a...

# GPT系列的数据集之谜

> 文｜Alan D. Thompson

> 源｜OneFlow

> 译｜杨婷、徐佳渝、贾川

[半个月以](https://so.csdn.net/so/search?q=%E4%B8%AD%E6%96%87%E7%89%88&spm=1001.2101.3001.7020)来，ChatGPT这把火越烧越旺。国内很多大厂相继声称要做 中文版 ChatGPT，还公布了上线时间表，不少科技圈已功成名就的大佬也按捺不住，携巨资下场，要创建“中国版OpenAI“。

不过，看看过去半个月在群众眼里稍显窘迫的Meta的Galactica，以及Google紧急发布的Bard，就知道在短期内打造一个比肩甚至超越ChatGPT效果的模型没那么简单。

[让很多人不免感到诧异的是](https://so.csdn.net/so/search?q=Transformer&spm=1001.2101.3001.7020)，ChatGPT的核心算法 Transformer 最初是由Google提出的，并且在大模型技术上的积累可以说不弱于OpenAI，当然他们也不缺算力和数据，但为什么依然会被ChatGPT打的措手不及？

Meta首席AI科学家Yann LeCun最近抨击ChatGPT的名言实际上解释了背后的门道。他说，ChatGPT“只是巧妙的组合而已”，这句话恰恰道出了一种无形的技术壁垒。

[简单来说，即使](https://so.csdn.net/so/search?q=OpenAI&spm=1001.2101.3001.7020)其他团队的算法、数据、算力都准备的与 OpenAI 相差无几，但就是没想到以一种精巧的方式把这些元素组装起来，没有OpenAI，全行业不知道还需要去趟多少坑。

即使OpenAI给出了算法上的一条路径，后来者想复现ChatGPT，算力、工程、数据，每一个要素都需要非常深的积累。七龙珠之中，算力是自由流通的商品，花钱可以买到，工程上有OneFlow这样的开源项目和团队，因此，对互联网大厂之外的团队来说，剩下最大的挑战在于高质量训练数据集。

至今，OpenAI并没有公开训练ChatGPT的相关数据集来源和具体细节，一定程度上也暂时卡了追赶者的脖子，更何况，业界公认中文互联网数据质量堪忧。

好在，互联网上总有热心的牛人分析技术的细枝末节，从杂乱的资料中串联起蛛丝马迹，从而归纳出非常有价值的信息。

此前，OneFlow发布了《ChatGPT背后的经济账》，其作者从经济学视角推导了训练大型语言模型的成本。本文作者则整理分析了2018年到2022年初从GPT-1到Gopher的相关大型语言模型的所有数据集相关信息，希望帮助有志于开发“类ChatGPT”模型的团队少走一步弯路。

一些研究人员的报告称，通用人工智能（AGI）可能是从我们当前的语言模型技术进行演进[1]，预训练Transformer语言模型为AGI的发展铺平了道路。虽然模型训练数据集日渐增大，但缺乏基本指标文档，包括数据集大小、数据集token数量和具体的内容细节。

尽管业内提出了数据集组成和整理文档的标准[2]，但几乎所有重点研究实验室在揭示模型训练数据集细节这方面都做得不够。这里整合的研究涵盖了2018年到2022年初从GPT-1到Gopher的精选语言模型的所有数据集（包括主要数据集：Wikipedia和Common Crawl）的综合视图。

###  **1 概述**

![](img/e1d0584a426fb79472190c237d670d18.png)

▲图 1. 主要数据集大小的可视化汇总。未加权大小，以GB为单位。

2018年以来，大语言模型的开发和生产使用呈现出爆炸式增长。一些重点研究实验室报告称，公众对大语言模型的使用率达到了惊人高度。2021年3月，OpenAI宣布[3]其GPT-3语言模型被“超过300个应用程序使用，平均每天能够生成45亿个词”，也就是说仅单个模型每分钟就能生成310万词的新内容。

值得注意的是，这些语言模型甚至还没有被完全理解，斯坦福大学的研究人员[4]最近坦言，“目前我们对这些模型还缺乏认知，还不太了解这些模型的运转模式、不知道模型何时会失效，更不知道这些模型的突现性（emergent properties）能产生什么效果”。

随着新型AI技术的快速发展，模型训练数据集的相关文档质量有所下降。模型内部到底有什么秘密？它们又是如何组建的？本文综合整理并分析了现代大型语言模型的训练数据集。

因为这方面的原始文献并不对外公开，所以本文搜集整合了二、三级研究资料，在必要的时候本文会采用假设的方式来推算最终结果。

在本文中，我们会将原始论文中已经明确的特定细节（例如token数量或数据集大小）归类为“公开的（disclosed）”数据，并作加粗处理。

多数情况下，适当地参考二、三级文献，并采用假设的方式来确定最终结果是很有必要的。在这些情况下，token数量和数据集大小等细节是“确定的（determined）”，并以斜体标记。

模型数据集可分为六类，分别是：维基百科、书籍、期刊、Reddit链接、Common Crawl和其他数据集。

![](img/1d63463bb013dde7c4cd7cc79a439321.png)

▲表1. 主要数据集大小汇总。以GB为单位。公开的数据以粗体表示。确定的数据以斜体表示。仅原始训练数据集大小。

####  **1.1. 维基百科**

维基百科是一个免费的多语言协作在线百科全书，由超过300,000名志愿者组成的社区编写和维护。截至2022年4月，英文版维基百科中有超过640万篇文章，包含超40亿个词[5]。维基百科中的文本很有价值，因为它被严格引用，以说明性文字形式写成，并且跨越多种语言和领域。一般来说，重点研究实验室会首先选取它的纯英文过滤版作为数据集。

####  **1.2. 书籍**

故事型书籍由小说和非小说两大类组成，主要用于训练模型的故事讲述能力和反应能力，数据集包括Project Gutenberg和Smashwords (Toronto BookCorpus/BookCorpus)等。

####  **1.3. 杂志期刊**

预印本和已发表期刊中的论文为数据集提供了坚实而严谨的基础，因为学术写作通常来说更有条理、理性和细致。这类数据集包括ArXiv和美国国家卫生研究院等。

####  **1.4. Reddit链接**

WebText是一个大型数据集，它的数据是从社交媒体平台Reddit所有出站链接网络中爬取的，每个链接至少有三个赞，代表了流行内容的风向标，对输出优质链接和后续文本数据具有指导作用。

####  1.5. Common Crawl

Common Crawl是2008年至今的一个网站抓取的大型数据集，数据包含原始网页、元数据和文本提取，它的文本来自不同语言、不同领域。重点研究实验室一般会首先选取它的纯英文过滤版（C4）作为数据集。

####  **1.6. 其他数据集**

不同于上述类别，这类数据集由GitHub等代码数据集、StackExchange 等对话论坛和视频字幕数据集组成。

###  2 常用数据集

2019年以来，大多数基于Transformer的大型语言模型 (LLM) 都依赖于英文维基百科和Common Crawl的大型数据集。在本节中，我们参考了Jesse Dodge和AllenAI（AI2）[8]团队的综合分析，按类别对英文维基百科作了高级概述，并在Common Crawl数据集[7]的基础上，用谷歌C4[6] (Colossal Clean Crawled Corpus)在Common Crawl中提供了顶级域（domains）。

####  2.1. 维基百科（英文版）分析

下面按类别[9]列出了维基百科的详细信息，涵盖了2015年抽样的1001篇随机文章，研究人员注意到随时间推移文章传播的稳定性。假设一个11.4GB、经过清理和过滤的维基百科英文版有30亿token，我们就可以确定类别大小和token。

![](img/6e1d875a25b56fbac7533a87c67c2e44.png)

表2. 英文维基百科数据集类别。公开的数据以粗体表示。确定的数据以斜体表示。

####  2.2 Common Crawl分析

基于AllenAI (AI2)的C4论文，我们可以确定，过滤后的英文C4数据集的每个域的token数和总体百分比，该数据集为305GB，其中token数为1560亿。

![](img/a5242a5ac822b7940c902d607b959863.png)

▲表3. C4：前23个域（不包括维基百科）。公开的数据以粗体表示，确定的数据以斜体表示。

###  3 GPT-1数据集

2018年，OpenAI发布了1.17亿参数的GPT-1。在论文中，OpenAI并没有公布模型训练数据集的来源和内容[10]，另外，论文误将‘BookCorpus’拼写成了‘BooksCorpus’。BookCorpus以作家未出版的免费书籍为基础，这些书籍来自于Smashwords，这是一个自称为“世界上最大的独立电子书分销商” 的电子书网站。这个数据集也被称为Toronto BookCorpus。经过几次重构之后，BookCorpus数据集的最终大小确定为4.6GB[11]。

2021年，经过全面的回顾性分析，BookCorpus数据集对按流派分组的书籍数量和各类书籍百分比进行了更正[12]。数据集中有关书籍类型的更多详细信息如下：

![](img/95b5b6206db21c58a4db7371321b6dbd.png)

▲表4. BookCorpus书籍类型。公开的数据以粗体表示，确定的数据以斜体表示。

在随后的数据集重构中，BookCorpus数据集进一步过滤掉了书籍中的“吸血鬼”类别、降低了言情类书籍的百分比、增加了“历史”类书籍，增加了收集的书籍数量。

**3.1. GPT-1数据集总结**

GPT-1最终的数据集总结分析如下：

![](img/5e3a4b450ad98b6acac282522b1cb9b8.png)

▲表5.GPT-1数据集总结。以GB为单位。公开的数据以粗体表示，确定的数据以斜体表示。

###  4 GPT-2数据集

2019年，OpenAI发布了拥有15亿参数的语言模型GPT-2。GPT-2论文阐明了所用训练数据集的大小[13]，不过并未说明其内容。而GPT-2模型卡（model card）（在GPT-2 GitHub仓库中）说明了模型内容[14]。

我们可以从GPT-3论文中得到token数量，该论文使用了WebText扩展版本来表示190亿token。据推测，2020年推出的WebText扩展版本拥有12个月的额外数据（additional data），因此它可能比2019年推出的GPT-2版本大25%左右[15]。GPT-2最终的token数量确定为150亿左右。

如GPT-2论文所述，假设模型卡显示链接数时，每个链接都可以被4500万链接总数所除，那WebText的内容在数据集中所占的百分比的详细信息就可以确定。

然后可以使用确定的150亿token数量来查找每个域的token数量。请注意，在可用的前1,000个域中，此处仅显示前50个域。

![](img/8dadb34be115e513a277537b5c13e602.png)

▲表6. WebText: 前50个域。 公开的数据以粗体表示，确定的数据以斜体表示。

####  **4.1. GPT-2数据集总结**

GPT-2模型最终的数据集总结分析如下：

![](img/d05823d98f490e42cca63b287cecdea5.png)

▲表7. GPT-2数据集总结。 公开的数据以粗体表示，确定的数据以斜体表示.

###  5 GPT-3数据集

GPT-3模型由OpenAI于2020年发布。论文阐明了所用训练数据集的token数量[16]，但训练数据集的内容和大小尚不清楚（Common Crawl的数据集大小除外[17]）

![](img/ea52999a5cb811a7f3b37b9f5b7d4b4f.png)

▲表8. GPT-3数据集。 公开的数据以粗体表示，确定的数据以斜体表示。

####  5.1. GPT-3：关于Books1和Books2数据集的分析

特别值得关注的是，在OpenAI的GPT-3论文中，并未公开Books1数据集（120亿token）和Books2数据集（550亿token）的大小和来源。关于这两个数据集的来源人们提出了几个假设，包括来自LibGen18和Sci-Hub的类似数据集，不过这两个数据集常以TB为计，大到无法匹配。

####  5.2. GPT-3：Books1

GPT-3使用的Books1数据集不可能与GPT-1使用的BookCorpus数据集相同，原因在于Books1的数据集更大，达120亿token。在一篇引用的论文[19]中就提及GPT-1使用的BookCorpus数据集拥有9.848亿个词，但这可能只相当于13亿token（984.8字x 1.3字的token乘数）。

通过标准化项目古腾堡语料库（SPGC），Books1有可能与古腾堡项目保持一致性。SPGC是一种开放式科学方法，被用于古腾堡项目完整的PG数据的精选（curated）版本。SPGC包含120亿个token[20]，大约为21GB[21]。

####  5.3. GPT-3：Books2

Books2（550亿token）可能与Bibliotik保持一致，并由EleutherA收集该来源的数据，组成数据集，使其成为The Pile v1的一部分。Bibliotik版本为100.96GB[22]，其确定的token数仅为250亿，低于Books2公开的550亿。然而，使用SPGC的‘每字节token数’比率（大约为1:1.75），Bibliotik的token数和大小将更接近于Books2。

####  **5.4. GPT-3数据集总结**

附录A概述了使用Wikipedia + CommonCrawl + WebText数据集的顶级资源列表。GPT-3模型的最终数据集总结分析如下：

![](img/9a1df37cc277de872280ce2dedf299ef.png)

▲表9.GPT-3数据集总结。公开的数据以粗体表示，确定的数据以斜体表示。

###  6 The Pile v19（GPT-J、GPT-NeoX-20B）数据集

The Pile v1数据集由EleutherAI于2021年发布，该数据集已被用于训练包括GPT-J、GPT-NeoX-20B在内的多种模型，并作为包括MT-NLG在内的其他模型的部分数据集。The Pile v1论文阐明了所用训练数据集的来源和大小。随着token数量的增加，The Pile v1论文应被用作未来数据集文档的黄金标准。

有关token数量的更多详情，可以使用本文提供的信息来确定，参见表1（大小以GB为单位）和表7（token/每字节）[23]。

![](img/c4bd846eb3d8ebc03164f4587b5ad537.png)

▲表10. The Pile v1数据集。公开的数据以粗体表示，确定的数据以斜体表示。

####  **6.1. The Pile v1分组数**据集（Grouped Datasets）

为了确定如‘Books’、‘Journals’和‘CC’这类数据集的大小，笔者对数据集进行了分组，如下表所示。

![](img/2e43765b6553a382738e1c462d192dc9.png)

▲表11. The Pile v1分组数据集（不包括Wikipedia、CC 和 WebText）。公开的数据以粗体表示，确定的以斜体表示。

####  6.2. The Pile v1数据集总结

The Pile v1数据集与GPT-J和GPT-NeoX-20B模型的最终数据集总结分析如下：

![](img/291681c9252628e6484d2e1540a388c8.png)

▲表 12. Pile v1 数据集总结。 公开的数据以粗体表示，确定的数据以斜体表示。

###  7 Megatron-11B和RoBERTa数据集

2019年，Meta AI(当时称之为Facebook AI)和华盛顿大学联合发布了拥有1.25亿参数的RoBERTa模型。次年，Meta AI发布了拥有110亿参数的Megatron-11B模型。Megatron-11B使用的训练数据集与RoBERTa相同。RoBERTa[24]论文阐明了所用训练数据集的内容，不过必须参考引用的论文(BERT[25]和toryes[26])来确定最终的数据集大小。

**BookCorpus **： 确定的数据集为4.6GB，如上面的GPT-1部分所示。

维基百科：公开的数据集为“16GB（BookCorpus加上英文维基百科）”。在减去BookCorpus数据集（4.6GB，如上面的GPT-1部分所述）后，维基百科数据集确定为11.4GB。

**CC-News **：（经过滤后）公开的数据集为76GB。

**OpenWebText **: 公开的数据集为38GB。

**Stories **: 公开的数据集为31GB。请注意，此数据集是“基于常识推理任务问题”的Common Crawl内容，不属于本文的‘Books’类别。相反，将Stories与CC-News数据集（76GB）相结合，Common Crawl的总数据集则为107GB。

####  7.1. Megatron-11B和RoBERTa的数据集总结

Megatron-11B和RoBERTa最终的数据集总结分析如下：

![](img/a1b1d4acbdc365f3bb9484504e43167b.png)

▲表13. Megatron-11B和RoBERTa的数据集总结。 公示的数据以粗体表示，确定的数据以斜体表示

###  8 MT-NLG数据集

2021年，英伟达和微软发布了拥有5300亿参数的语言模型MT-NLG。MT-NLG是微软Turing NLG（拥有170亿参数）和英伟达Megatron-LM（拥有83亿参数）的“继任者”。MT-NLG论文阐明了所用训练数据集的来源和token数量，不过没有明确指出数据集的大小。

如前所述，有关数据集大小的更多详情，可以使用The Pile v1论文中提供的信息来确定。虽然使用的组件相同，但注意的是，MT-NLG和The Pile v1中报告的组件大小却各不相同，这是由于来自Eleuther AI (The Pile v1数据集)和Microsoft/NVIDIA (MT-NLG模型)的研究人员采用了不同的数据过滤和去重方法。

####  8.1. MT-NLG中的Common Crawl数据集

Pile-CC：公开的数据集为498亿token，确定的数据为227.12GB左右，参见上述Pile v1部分。

CC-2020-50: 公开的数据集为687亿token，假设token的每字节率（per byte rate）为0.25 TpB=274.8GB。

CC-2021-04：公开的数据集为826亿token，假设token的每字节率为0.25 TpB=330.4GB

RealNews（来自RoBERTa/Megatron-11B）：显示为219亿token。根据RealNews论文[27]，数据集确定为120GB。

CC-Stories(来自RoBERTa/Megatron-11B)：公开的数据集为53亿token，如上述RoBERTa部分所示，数据集确定为31GB。

根据以上来源，可确认Common Crawl的总数据量为983.32GB，共计2283亿token。

####  8.2. MT-NLG分组数据集（Grouped Datasets）

![](img/d0cc9a71e98d760ad4d1281dcd85eb0f.png)

▲表14. MT-NLG 分组数据集。公开的数据以粗体表示，确定的数据以斜体表示

####  8.3. MT-NLG数据集总结

MT-NLG模型最终的数据集总结分析如下：

![](img/95199b4afbffb70134b51322d87e3a25.png)

▲表15. MT-NLG数据集总结。 公示的数据以粗体表示，确定的数据以斜体表示。

###  9 MT-NLG 数据集Gopher数据集

Gopher模型由DeepMind于2021年发布，有2800亿参数。该论文清楚地说明了所使用训练数据集所包含的高级token数量和大小[28]，但没有说明详细内容。

![](img/1840e975d748ea2f06cfaa7e8da7f8a3.png)

▲表16. 公开的Gopher数据集 (MassiveText)。公开的数据以粗体表述，确定的数据以斜体表示。

有趣的是，据Gopher论文披露：其Books数据集中包含一些超过500年历史（1500-2008）的书籍。

####  9.1. MassiveWeb数据集分析

DeepMind于2014年被谷歌收购，并在创建MassiveText时获得了海量数据。虽然Gopher论文中没有进一步详细描述MassiveWeb，但第44页附录中的表A3b注明了MassiveWeb中出现的前20个域[29]。根据披露的每个域所占的百分比，我们可以使用MassiveWeb的总token数（5060亿token）和总原始大小（1900GB）来确定每个域的token数量和大小。

![](img/ce1572994a81d03913d0181370bc22d4.png)

▲表17. MassiveWeb：前20个域。公开的数据以粗体表示，确定的数据以斜体表示。

####  9.2. Gopher：关于维基百科数据集的分析

维基百科数据集的总规模很难确定。在Gopher论文中，研究人员指出维基百科没有进行数据去重[30]。然而，论文中列出的不同大小数据集（12.5GB MassiveWeb Wikipedia与1GB MassiveText Wikipedia）可能是由于失误而造成的，误将“10GB”写成了“1GB”。无论如何，本文仅使用MassiveWeb数据集版本 (12.5GB)。

####  9.3. Gopher:不包括WebText

Gopher数据集的组成部分不包括Reddit外链的WebText数据集。为了清楚起见，尽管Reddit是MassiveWeb中的顶级域，但该数据集仅抓取Reddit域内的Reddit链接。根据定义，WebText[31]由“所有Reddit的外链”组成（即指向Reddit域外的链接）。

####  9.4. Gopher分组数据集

MassiveWeb被认为是MassiveText的子组件，并被集成到Gopher的数据集汇总中，其分组基于以下列出的可用信息：

![](img/5a59cce0b0b68a5747a4febad5e85761.png)

▲表18. Gopher分组数据集。公开的数据以粗体表示，确定的数据以斜体表示

####  9.5. Gopher数据集总结

Gopher是本文中最大的数据集，大小为10.5TB。Gopher模型的最终数据集总结分析为：

![](img/da529fe449d999daabb08821e66b8778.png)

▲表19. Gopher数据集总结。公开的数据以粗体表示，确定的数据以斜体表示。

###  10 结论

对于训练当代Transformer大型语言模型的数据集而言，这可能是最全面的整合分析内容（截止2022年初）。在主要数据源不透明的情况下，本次研究主要从二级和三级来源收集数据，并经常需要假定来确定最终估计值。随着研究人员要处理千万亿个token（1,000万亿）和数千TB的数据（1,000TB），确保详细披露数据集组成的文档变得越来越重要。

特别值得关注的是，基于大型语言模型的强大AI系统产生的冗长而匿名的输出正在迅速发展，其中许多数据集的细节内容几乎没有文档说明。

强烈建议研究人员使用突出显示的“数据集的数据表（Datasheet for Datasets）”论文中提供的模板，并在记录数据集时使用最佳实践论文（即Pile v1论文，包括token数量）。数据集大小（GB）、token数量（B）、来源、分组和其他详细信息指标均应完整记录和发布。

随着语言模型不断发展并更广泛地渗透到人们的生活中，确保数据集的详细信息公开透明、所有人都可访问且易于理解是有用、紧迫和必要的。

**扩展阅读及脚注**

考虑到简洁和可读性，本文使用了脚注而非文本/括弧式引文。主要参考文献如下，或者参见http://lifearchitect.ai/papers/，获取大语言模型领域的主要基础论文。以下论文按本文顺序显示。

1.  Datasheets for Datasets Gebru, T., Morgenstern, J., Vecchione, B., Vaughan, J., Wallach, H., Daumé III, H., & Crawford, K. (2018). Datasheets for Datasets. https://arxiv.org/abs/1803.09010

1.  GPT-1 paper Radford, A., & Narasimhan, K. (2018). Improving Language Understanding by Generative Pre-Training. OpenAI. https://cdn.openai.com/research-covers/language-unsupervised/language_understan ding_paper.pdf

1.  GPT-2 paper Radford, A., Wu, J., Child, R., Luan, D., Amodei, D. & Sutskever, I. (2019). Language Models are Unsupervised Multitask Learners. OpenAI. https://cdn.openai.com/better-language-models/language_models_are_unsupervised _multitask_learners.pdf

1.  GPT-3 paper Brown, T., Mann, B., Ryder, N., Subbiah, M., Kaplan, J., & Dhariwal, P. et al. (2020). OpenAI. Language Models are Few-Shot Learners. https://arxiv.org/abs/2005.14165

1.  The Pile v1 paper Gao, L., Biderman, S., Black, S., Golding, L., Hoppe, T., & Foster, C. et al. (2021). The Pile: An 800GB Dataset of Diverse Text for Language Modeling.

1.  EleutherAI. https://arxiv.org/abs/2101.00027

1.  GPT-J announcement Komatsuzak, A., Wang, B. (2021). GPT-J-6B: 6B JAX-Based Transformer. https://arankomatsuzaki.wordpress.com/2021/06/04/gpt-j/

1.  GPT-NeoX-20B paper Black, S., Biderman, S., Hallahan, E. et al. (2022). EleutherAI. GPT-NeoX-20B: An Open-Source Autoregressive Language Model. http://eaidata.bmk.sh/data/GPT_NeoX_20B.pdf

1.  RoBERTa paper Liu, Y., Ott, M., Goyal, N., Du, J., Joshi, M., & Chen, D. et al. (2019). RoBERTa: A Robustly Optimized BERT Pretraining Approach. Meta AI. https://arxiv.org/abs/1907.11692

1.  MT-NLG paper Smith, S., Patwary, M., Norick, B., LeGresley, P., Rajbhandari, S., & Casper, J. et al. (2021). Using DeepSpeed and Megatron to Train Megatron-Turing NLG 530B, A Large-Scale Generative Language Model. Microsoft/NVIDIA. https://arxiv.org/abs/2201.11990

1.  Gopher paper Rae, J., Borgeaud, S., Cai, T., Millican, K., Hoffmann, J., & Song, F. et al. (2021). Scaling Language Models: Methods, Analysis & Insights from Training Gopher. DeepMind. https://arxiv.org/abs/2112.11446

1.  Appendix A: Top 50 Resources: Wikipedia + CC + WebText (i.e. GPT-3)

附录 A：前50个资源：Wikipedia + CC + WebText（即 GPT-3）

基于本文内容，尤其是每个数据集中每个资源的token数量，我们可以对将Wikipedia + Common Crawl + WebText数据集的组合，作为其整体训练数据集的一部分模型进行资源或域的排序。为清楚起见，这包括以下模型：OpenAI GPT-3、EleutherAI GPT-J、EleutherAI GPT-NeoX-20B、Meta AI Megatron-11B和RoBERTA，以及 Microsoft/NVIDIA MT-NLG等。

请注意，展示的排名基于数据集中可用的未加权总token，每个数据集的主观权重由研究人员在模型预训练之前计算得出。其中有一些重复（例如，《纽约时报》既出现在有1.11亿token的WebText中，也出现在过滤后有1亿token的Common Crawl中）。

![](img/7443c0e715c999032172ef18e23be392.png)

**关于作者**

Alan D. Thompson博士是人工智能专家、顾问。在2021年8月的世界人才大会（World Gifted Conference）上，Alan与Leta（由GPT-3提供支持的AI）共同举办了一场名为“The new irrelevance of intelligence”的研讨会。他的应用型人工智能研究和可视化成果受到了国际主要媒体的报道，同时还在2021年12月牛津大学有关AI伦理的辩论中被引用。他曾担任门萨国际（Mensa International）主席、通用电气（GE）和华纳兄弟（Warner Bros）顾问，也曾是电气与电子工程师协会（IEEE）和英国工程技术学会（IET）会员。

>

![](img/900095d2d1a352d0e91e1733841daccc.png)

> 后台回复关键词【 **入群 **】

>

> 加入卖萌屋NLP、CV、搜推广与求职讨论群

>

![](img/71fcdb1beb5cd00b00559959def87135.png)

>

##### [1] GPT-NeoX-20B paper: pp11, section 6 http://eaidata.bmk.sh/data/GPT_NeoX_20B.pdf

##### [2] Datasheet for Datasets paper: https://arxiv.org/abs/1803.09010

##### [3] OpenAI blog: https://openai.com/blog/gpt-3-apps/

##### [4] On the Opportunities and Risks of Foundation Models: https://arxiv.org/abs/2108.07258

##### [5] Size of Wikipedia: https://en.wikipedia.org/wiki/Wikipedia:Size_of_Wikipedia

##### [6] C4 dataset:

##### https://www.tensorflow.org/datasets/catalog/c4

##### [7] Common Crawl website: https://commoncrawl.org/

##### [8] C4 paper: https://arxiv.org/abs/2104.08758 pp2, Figure 1 right

##### [9] Wikipedia categories:

##### https://en.wikipedia.org/wiki/User:Smallbones/1000_random_results: “维基百科涵盖哪些主题？覆盖范围是否随时间变化？使用2015年12月抽取的1001篇随机文章对这些问题和类似问题进行了查验...随着时间推移，这些比例相当稳定...传记（27.8%），地理（17.7%），文化和艺术（15.8%），历史（9.9%），生物学、健康和医学（7.8%），体育（6.5%），商业（4.8%），其他社会（4.4%），科学与数学（3.5%），教育（1.8%）。”

##### [10] GPT-1 paper: pp4 “We use the BooksCorpus dataset for training the language model.”

##### [11]https://huggingface.co/datasets/bookcorpus: “Size of the generated dataset: 4629.00 MB”

##### [12]BookCorpus Retrospective Datasheet paper: pp9 https://arxiv.org/abs/2105.05241

##### [13] GPT-2 paper: pp3 “我们从社交媒体平台Reddit中抓取了至少有3个karma的所有出站链接。这可以被认为是一个启发式指标，用于判断其他用户是否觉得该链接有趣、有教育意义或只是有趣……WebText包含这4500万个链接的文本子集……其中不包括2017年12月之后创建的链接。经过去重和一些基于启发式的清理后，其中包含大约超过800万个文档，总共40GB文本。我们从WebText中移除了所有维基百科文档...”

##### [14] GPT-2 model card: https://github.com/openai/gpt-2/blob/master/model_card.md: “我们已经发布了WebText中出现的前1,000个域及其频率的列表。WebText中排名前15位的域是：Google、Archive、Blogspot、GitHub、纽约时报、Wordpress、华盛顿邮报、维基亚、BBC、卫报、eBay、Pastebin、CNN、雅虎和赫芬顿邮报。”

##### [15] GPT-3 paper: “WebText2：190亿token。[Alan：WebText2是从WebText稍微扩展而来，所以我们可以减去20%，得到150亿token]”

##### [16] GPT-2 paper: pp3 “GPT-3: pp9, Table 2.2 “CC: 4100亿token. WebText2: 190亿token. Books1: 120亿token. Books2: 550亿token. Wiki: 30亿token”

##### [17] GPT-3 paper: pp8

##### [18] BookCorpus repo: soskek/bookcorpus#27: “books3.tar.gz似乎类似于OpenAI在他们的论文中引用的神秘“books2”数据集。不幸的是，OpenAI不会提供细节，所以我们对其差异知之甚少。人们怀疑它是“libgen的全部”，但这纯粹是猜测。尽管如此，books3仍是“所有的bibliotik”......”

##### [19]BookCorpus paper: https://arxiv.org/abs/1506.06724: “# of words: 984,846,357 [Alan: BookCorpus有13亿token。我们想要有120-550亿token]”

##### [20] Gutenberg paper: https://arxiv.org/abs/1812.08092: “我们介绍了标准化项目古腾堡语料库（SPGC），这是一种开放的科学方法，用于处理完整PG数据的精选版本，其中包含超过50,000本书和3×109word-token[Alan：相当于大约120亿BPE token，见下文 ]”

##### [21] Gutenberg repo: https://zenodo.org/record/2422561 “未压缩大小：3GB（count）+ 18GB（token）[总计21GB]”

##### [22] The Pile v1 paper: “Books3（Bibliotik tracker）：100.96GB” [Alan：乘以每字节token数0.2477 = 250亿token]

##### [23] The Pile v1 paper: pp3, Table 1 for datasets. pp28, Table 7 for Tokens per byte.

##### [24] RoBERTa paper: https://arxiv.org/abs/1907.11692 “BOOKCORPUS加上英文WIKIPEDIA。这是用来训练 BERT的原始数据。（16GB）。”

##### [25] BERT paper: https://arxiv.org/abs/1810.04805 “BERT在BooksCorpus（8亿字）和维基百科（25亿字）上进行训练。”

##### [26] Stories paper: https://arxiv.org/abs/1806.02847 pp5-6

##### [27] RealNews paper: https://arxiv.org/abs/1905.12616v3 “去重后，RealNews在没有压缩的情况下为120GB。”

##### [28] Gopher paper: https://arxiv.org/abs/2112.11446 pp 7: list of sizes and tokens.

##### [29] Gopher paper: https://arxiv.org/abs/2112.11446 pp 44, Figure A3b.

##### [30] Gopher paper: pp41n14 “请注意，我们将文档去重应用于除Wikipedia和GitHub之外的所有MassiveText子集“

##### [31] GPT-2 paper, pp3.