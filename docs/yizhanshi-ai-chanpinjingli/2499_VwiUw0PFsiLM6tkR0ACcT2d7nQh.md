# 05🈷️12日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/TXoSdJStZowVebx4T0Acouv4nDg](https://v11enp9ok1h.feishu.cn/docx/TXoSdJStZowVebx4T0Acouv4nDg)

> 🔔AI情报助手早报「05月12日」

> ✨✨✨✨✨✨✨✨

> 1️⃣

> 🎸

> 一起聊聊Nvidia Blackwell 新特性之UMMA

>

+   NVIDIA Blackwell架构引入新特性改变GEMM内核形态，本系列旨在说明如何更新或编写适用于该架构的GEMM内核。

+   Blackwell以tcgen05.mma指令（UMMA）取代Hopper架构的wgmma.mma_async，在数据类型、块缩放等方面有差异。

+   张量内存（TMEM）是专供张量核心的片上存储器，可降低MMA操作寄存器压力，与CTA主执行流程解耦。

+   CUTLASS库为UMMA指令提供接口，文章通过示例展示其在GMEM切分、SMEM布局等方面的使用。

+   文章示例仅涉及单SM的UMMA指令，后续将讨论线程簇级协作的相关内容。

> 关键词：Nvidia Blackwell，UMMA，张量内存，CUTLASS，GEMM内核

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/k2mJxZmrw130kaMLig-hgg)

+   [研究链接](https://research.colfax-intl.com/cutlass-tutorial-writing-gemm-kernels-using-tensor-memory-for-nvidia-blackwell-gpus/)

> 2️⃣

> 🤖

> [Karpathy最新提出的“系统提示](https://mp.weixin.qq.com/s/SIYVcJ9TthjSWcJP0kTZAw)学习”是什么？ 未来context为王？

>

+   前OpenAI科学家Andrej Karpathy提出“系统提示学习”，认为LLM除预训练和微调外，需新学习范式。

+   该思考源于对Claude系统提示研究，其长文本含大量通用问题解决策略。

+   Karpathy提出实现“系统提示学习”的潜在方向，如LLM自主生成编辑提示文本等。

+   此概念触及LLM发展关键挑战，引发讨论。

+   这为实现更灵活智能体指明方向，但具体技术待解。

> 关键词：系统提示学习，LLM，Karpathy，策略学习

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/SIYVcJ9TthjSWcJP0kTZAw)

> 3️⃣

> 🎯

> [2](https://mp.weixin.qq.com/s/1PIF01INQM5eXS9COMKB_Q)个月涨粉10000+，多篇文章阅读过万！黄叔是如何在AI浪潮中找到清晰方向的？（内附“双杠杆”实践心得）

>

+   黄叔分享在AI浪潮中摸索方向的历程，曾历经迷雾与自问、试错与探索，在迷茫中寻找ikigai。

+   他抓住AI相关机会，如写AI编程教程、参与直播等，实现粉丝增长，迎来高光时刻。

+   做AI产品走过弯路，后回归写长文，赢得机会。

+   提出“内容”与“产品”双杠杆模式，发布AI编程蓝皮书巩固定位。

+   黄叔希望与同路人并肩，做出有价值产品，未来将继续探索，优先做2门课。

> 关键词：AI浪潮，双杠杆，AI编程，内容创作，产品实践

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/1PIF01INQM5eXS9COMKB_Q)

> 4️⃣

> 🤖

> 2025，我们又一次用AI，交了个朋友。

>

+   2025年5月10日在杭州举办了一场AI聚会，虽准备有不足但获大家包容。

+   开场由小旭音乐带乐队演奏，还有篮球宝贝伴舞等，十分精彩。

+   活动通过AI进行分组破冰，创造新连接。

+   四位嘉宾从创业者、媒体人等不同视角分享AI落地、创作等内容。

+   抽奖环节虽遇问题但氛围拉满，活动感谢各方支持，系列还会继续。

> 关键词：AI聚会，嘉宾分享，AI分组，抽奖，阿里云

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/HiOOXbkSxX6EjvkMGlHWpQ)

> 5️⃣

> 🤖

> AI逼疯七巨头，马斯克们深陷中年危机！

>

+   科技巨头面临AI带来的「中年危机」，谷歌搜索流量下滑，苹果AI进展缓慢，Meta Llama 4未达预期，特斯拉销量股价双降。

+   巨头们虽利润惊人，但处于十字路口，应对方式如同《创新者的窘境》案例，或被新兴小公司颠覆。

+   目前主流平台未找到AI制胜法则，年轻投资人看好下一代AI初创公司。

+   微软、英伟达在AI浪潮中表现出色，但新兴模型DeepSeek带来变数。

+   AI浪潮下，巨头若无法摆脱路径依赖，或成AI时代「诺基亚」。

> 关键词：AI，科技巨头，中年危机，创新者的窘境，新兴模型

>

> 🔗 文中引用的链接

+   [https://www.wsj.com/tech/ai/the-giants-of-silicon-valley-are](https://www.wsj.com/tech/ai/the-giants-of-silicon-valley-are-having-a-midlife-crisis-over-ai-74968a07?mod=tech_lead_pos2)-having-a-midlife-crisis-over-ai-74968a07?mod=tech_lead_pos2

+   [ht](https://www.wsj.com/tech/personal-tech/apple-ai-future-two-paths-75fcf12a?mod=article_inline)tps://www.wsj.com/tech/personal-tech/apple-ai-future-two-paths-75fcf12a?mod=article_inline

+   [h](https://www.wsj.com/tech/ai/mark-zuckerberg-ai-digital-future-0bb04de7)ttps://www.wsj.com/tech/ai/mark-zuckerberg-ai-digital-future-0bb04de7

+   [https://edition.cnn.com/2025/03/19](https://edition.cnn.com/2025/03/19/tech/apple-iphone-siri-delay/index.html)/tech/apple-iphone-siri-delay/index.html

+   [ht](https://techcrunch.com/2025/05/09/coreweave-reportedly-looks-to-raise-1-5b-in-debt-as-ipo-disappoints/)tps://techcrunch.com/2025/05/09/coreweave-reportedly-looks-to-raise-1-5b-in-debt-as-ipo-disappoints/