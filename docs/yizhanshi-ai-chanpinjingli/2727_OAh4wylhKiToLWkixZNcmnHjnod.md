# 1🈷️04日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/VLL9dZhy2odbBJx1kc6c074lnxe](https://v11enp9ok1h.feishu.cn/docx/VLL9dZhy2odbBJx1kc6c074lnxe)

> 🔔AI情报助手午报「01月04日」

> ✨✨✨✨✨✨✨✨

> 1️⃣

> 🔮

> 优化即几何，几何即推理：用数学终结Transformer的黑盒时代

>

+   📚哥伦比亚大学和Dream Sports团队发布三部曲论文，试图用数学连接优化目标、几何结构与推理功能，终结Transformer黑盒时代。

+   📊构建“贝叶斯风洞”验证，Transformer预测熵精确贴合理论贝叶斯后验，展现逐Token实时推理能力。

+   🔄揭示三阶段几何演化机制：假设框架构建、渐进假设消除、熵有序流形形成。

+   ⚙️梯度动力学推导显示，交叉熵诱导优势路由和责任加权更新的正反馈，等价于隐式EM算法。

+   🧠解释Chain-of-Thought是几何延展器，通过中间步骤增加计算轮次，避免低置信度幻觉。

> 关键词：Transformer，贝叶斯推理，几何演化，梯度动力学，Chain-of-Thought

>

> 🔗 文中引用的链接

+   [原🛰️文章](https://mp.weixin.qq.com/s/uTEx3FrJjBNrBwvOunTBiQ)

+   [论文1：The Bayesian Geometry of T](https://arxiv.org/abs/2512.22471)ransformer Attention

+   [论文2：Gradient Dynamics of Attention: How ](https://arxiv.org/abs/2512.22473)Cross-Entropy Sculpts Bayesian Manifolds

+   [Medium博客：Attention I](https://medium.com/@vishalmisra/attention-is-bayesian-inference-578c25db4501)s Bayesian Inference

> 2️⃣

> 🔥

> 重构通用异常检测新范式：Dinomaly2实现跨模态、跨任务的无缝统一

>

+   Dinomaly2是统一异常检测框架，首次实现跨模态、任务、领域的全面统一，在MVTec-AD等数据集达SOTA性能。

+   核心含通用ViT表征、噪声瓶颈（Dropout）、非聚焦线性注意力等极简组件，解决过度泛化等问题。

+   终结多类别与单类别性能鸿沟，MVTec-AD I-AUROC 99.9%、VisA 99.3%，超越专门方法。

+   无需修改即可扩展到多视角、RGB-3D融合、少样本场景，8样本性能超全数据SOTA。

+   首次验证异常检测规模化定律：模型/分辨率/预训练越好性能越强，ViT-S版RTX4090达253 FPS。

> 关键词：Dinomaly2，异常检测，跨模态，Transformer，规模化定律

>

> 🔗 文中引用的链接

+   [原文章链接](https://mp.weixin.qq.com/s/y6jR_iglyuz6hSbATVcRNA)

+   [论文链接](https://arxiv.org/abs/2510.17611v2)

+   [代码仓库](https://github.com/guojiajeremy/Dinomaly)

> 3️⃣

> 📌

> [万](https://mp.weixin.qq.com/s/NbIb7kFthiKyerLoeJF-Cg)字长文！从 requests 到 Playwright，两个简单项目带你理解 Python 爬虫到底能干什么

>

+   文章阐述爬虫本质是理解网页结构、与网站通信、自动化操作三种能力结合，可实现自动收集信息、批量下载等自动化任务。

+   项目一用requests获取静态网页内容，结合BeautifulSoup解析HTML，提取文本、链接、图片并本地保存。

+   项目二针对动态网页，用Playwright启动真实浏览器执行JS，模拟点击Python官网按钮，提取真实下载链接完成下载。

+   对比静态爬虫（requests+bs4）与动态爬虫（Playwright）适用场景：静态适用于HTML直接含数据的页面，动态适用于依赖JS或交互的场景。

+   文章指出爬虫可扩展到分页爬取、登录模拟等复杂场景，后续将深入表单提交和验证码解决问题。

> 关键词：Python爬虫，requests，BeautifulSoup，Playwright，动态网页

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/NbIb7kFthiKyerLoeJF-Cg)

+   [httpbin测试页面](http://httpbin.org/html)

+   [Python官网](https://www.python.org)

+   [Python下载链接示例](https://www.python.org/ftp/python/3.13.7/Python-3.13.7.tar.xz)

> 4️⃣

> 🏆

> 马斯克10年梦成真！特斯拉全球首次自动驾驶横穿美国，人类0接管

>

+   2026年1月1日，车主David Moss驾驶搭载FSD v14.2的Model3完成全球首次第三方验证的零接管横穿美国，里程约4397公里，耗时2天20小时，零人工干预。

+   此次壮举是特斯拉端到端神经网络技术的里程碑，前AI总监Karpathy称是"软件2.0"在物理世界的胜利，马斯克转发点赞"酷！"。

+   FSD v14.2关键突破：将导航与路径规划整合进神经网络，不再依赖传统导航模块，能实时应对复杂路况（暴雨、夜间驾驶、自动充电等）。

+   特斯拉从FSD v12起抛弃C++规则代码，转向端到端神经网络，通过视频数据学习驾驶，V14.2补全导航拼图。

+   此次验证纯视觉方案（无激光雷达/高精地图）可实现L4级长途自动驾驶，突破行业对纯视觉路线的质疑。

> 关键词：特斯拉FSD，端到端神经网络，零接管横穿美国，自动驾驶里程碑，纯视觉方案

>

> 🔗 文中引用的链接

+   [David Moss推特](https://x.com/DavidMoss/status/2006255297212358686)

+   [Teslarati报道](https://www.teslarati.com/tesla-fsd-successfully-completes-full-coast-to-coast-drive-with-zero-interventions/)

+   [NYPost报道](https://nypost.com/2025/12/31/tech/tesla-owner-completes-first-fully-autonomous-drive-across-america-and-elon-musk-weighs-in-on-the-historic-road-trip/)

+   [YouTube视频](https://www.youtube.com/watch?v=dnLswbNB0SU&t=4s)

> 5️⃣

> 🔥

> 10个月打满400亿美元！孙正义重注OpenAI冲刺万亿估值

>

+   软银对OpenAI的410亿美元投资10个月内全部到位（原计划两年），持股约11%，资金用于重组、算力基建（Stargate项目、日本数据中心）及收购合作。

+   OpenAI重组为公益公司（PBC）铺路IPO，估值从2600亿升至7500-8300亿区间，冲击万亿估值。

+   孙正义清仓英伟达58亿股份，斥资40亿收购DigitalBridge，聚焦AI基础设施布局。

+   OpenAI算力缺口显著，资金用于算力底座扩容；软银通过投资锁定算力供应链核心位置。

+   微软（约130亿）、亚马逊（拟超100亿）也投资OpenAI，巨头争夺下一代算力入口。

> 关键词：OpenAI，软银孙正义，算力基建，万亿估值，IPO

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/oHD8dAKl5HxsDBxdpdrcZQ)

+   [参考资料：CNBC报道](https://www.cnbc.com/2025/12/30/softbank-openai-investment.html)

> 6️⃣

> 🔥

> F1暴涨20分，推理速度恒定！新架构VGent：多目标定位又快又准

>

+   来自UIC和Adobe的团队提出VGent模块化模型，解决多目标视觉定位难题，16B内参数量在ORES基准超Qwen3-VL-30B，平均F1提升+18.24。

+   VGent采用编码器-解码器架构，解耦语义推理与像素预测，推理速度恒定不随目标数量增加变慢。

+   模型引入QuadThinker强化多目标推理、Mask-aware Label解决检测分割歧义、Global Target Recognition增强全局感知。

+   在ORES基准上VGent比RAS13B提升+20.58% F1，单目标任务平均准确率90.1%，超越更大规模模型。

+   VGent在密集多目标和视觉参照场景鲁棒，精准定位目标排除干扰，展现优越细粒度推理能力。

> 关键词：VGent，视觉定位，多目标检测，模块化架构，MLLM

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/R_RVJPAqzgfvn65AJYlvnw)

+   [论文地址](https://arxiv.org/abs/2512.11099)