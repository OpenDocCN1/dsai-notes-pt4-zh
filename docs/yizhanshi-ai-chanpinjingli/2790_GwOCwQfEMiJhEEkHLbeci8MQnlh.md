# 3🈷️10日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/T8KxdWwlGoxfC7xeyWQcaR7ynWc](https://v11enp9ok1h.feishu.cn/docx/T8KxdWwlGoxfC7xeyWQcaR7ynWc)

> 🔔AI情报助手晚报「03月10日」

> ✨✨✨✨✨✨✨✨

> 1️⃣

> 🍊

> The 2026 season Miyi...

>

+   2026年3月10日，四川攀枝花米易枇杷正式上市发货，为中国国家地理标志产品。

+   米易枇杷果大厚实、色泽鲜艳，果肉细腻甜浓汁多，富含多种维生素及矿物质。

+   购买可联系微信id（bbuf23333），国内包邮，2026年3月10日价格为98元/箱（5斤大果）。

+   购买后可加入卖家维护的售后群《米易县枇杷园 售后及评论群》。

+   该枇杷年产量有限，每年仅150-200箱左右，还展示了2026年真实用户好评。

> 关键词：米易枇杷，攀枝花特产，地理标志产品，农产品销售，2026上市

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/suRRAuQifAUGhqH5jB1cfg)

> 2️⃣

> 🤖

> CUTLASS 笔记 (3)：Tiled MMA

>

+   解析GEMM三级Tiling（Global→Block→Tile→MMA Atom），每级与GPU硬件（SM、SMEM、Tensor Core）特性绑定，解决并行、访存、算力利用问题。

+   介绍单指令MMA扩展到Tile的思路：扩展warp数量（线程）和单warp内mma指令数，示例Tile规模从16x8x8扩至32x32x16。

+   解析CUTLASS CuTe的make_tiled_mma API，新增MMAThrLayout（线程扩展映射）和MMATileLayout（Tile排列Layout）参数的作用。

+   讲解Tensor Metadata的存储介质、数据长度、Layout等信息，对应mma指令扩展的MMA_M/N/K维度。

+   分析Tile级MMA的SASS代码：K维度扩展mma指令后，LDG指令增加，累加器复用，STG条数不变，访存行为变化。

> 关键词：GEMM，Tiled MMA，CUTLASS，三级Tiling，Tensor Core

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/WBu3Bh0FCFQKf0pyNK-KSg)

+   [知乎专栏链接](https://www.zhihu.com/column/c_1938664963049763058)

> 3️⃣

> 🤖

> 我搭了一套国产的小龙虾方案，成本可控，还能 24小时自动干活

>

+   用nanobot（轻量框架）+Seed 2.0（字节国产大模型）+Claude Code搭建国产AI自动化编程系统，解决OpenClaw重、成本高问题。

+   系统分三层：数据层（todo.md任务队列）、调度层（无状态定时检查）、执行层（tmux后台运行），实现24小时自动任务分配。

+   实战测试：VLM视觉任务（图片转HTML还原度95%）、长程数据分析（17分钟稳定）、全栈开发（元宵应用并行25分钟完成）。

+   成本低：火山引擎Lite套餐40元/月，三任务仅用5%额度，适合个人开发者。

+   踩坑经验：PRD需明确、验收量化、大任务拆解；国产AI（Seed 2.0）已达生产可用水平。

> 关键词：国产大模型，AI自动化编程，nanobot，Seed 2.0，成本可控

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/3UPQ-hu2FlmqJtl9EaaqQA)

+   [火山引擎方舟平台](http://szacq.cn/YEubD/)

> 4️⃣

> 🤖

> [I](https://mp.weixin.qq.com/s/Gx1vw-RrLvJCR-mhWHm73A)CLR 2026 | 视频虚化新突破！vivo &浙大提出Any-to-Bokeh：基于视频扩散模型的一键式可控视频虚化框架

>

+   针对传统视频虚化帧间闪烁、控制难等痛点，vivo蓝图像实验室联合浙大提出一键式可控框架Any-to-Bokeh。

+   核心引入焦平面自适应MPI先验，结合单步视频扩散模型，实现精准虚化强度与焦平面控制，解决时序不一致。

+   三阶段渐进训练+加权交叠推理策略，提升深度噪声鲁棒性与长视频边界平滑度，推理效率显著高于同类。

+   合成成对数据集验证，定量多指标最优，定性保留复杂纹理，主观51人次评分获压倒性青睐。

+   框架已开源GitHub，未来可优化算力与长视频效率，推动计算摄影等领域落地。

> 关键词：CV，视频虚化，Any-to-Bokeh，视频扩散模型，MPI

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/jatkZhASbMYCV2DhYirOgg)

+   [项目GitHub](https://github.com/vivoCameraResearch/any-to-bokeh)

> 5️⃣

> 🤖

> [PR 2026 | 山东大学提出 G](https://mp.weixin.qq.com/s/yc63PvfPCAuY-tal_8_6kg)aitADIB：攻克未见视角步态识别难题

>

+   针对步态识别实际部署中视角缺失导致的性能衰退问题，山东大学贲晛烨团队提出GaitADIB对抗解耦信息瓶颈网络，将视角变化建模为连续风格扰动并提纯视角无关身份特征。

+   方法通过双信息变分编码器（DIVE）初步分离身份与风格特征，结合对抗互信息解耦（AMID）打破残余耦合，再经选择性自适应特征融合（SAFF）滤除噪声。

+   在CASIA-B数据集极端缺失协议（如IVC-R80）下，模型Rank-1准确率达66.0%，显著优于现有方法。

+   在OUMVLP大规模数据集上同样展现稳定优势，验证了跨数据集泛化能力。

+   论文发表于《Pattern Recognition》2026年第177卷，DOI为10.1016/j.patcog.2026.113243。

> 关键词：步态识别、视角缺失、对抗解耦、信息瓶颈、计算机视觉

>

> 🔗 文中引用的链接

+   原文链接：

+   [https://mp.weixin.qq.com/s/yc](https://mp.weixin.qq.com/s/yc63PvfPCAuY-tal_8_6kg)63PvfPCAuY-tal_8_6kg

+   论文地址：

+   [h](https://www.sciencedirect.com/science/article/pii/S0031320326002086)ttps://www.sciencedirect.com/science/article/pii/S0031320326002086