# 2023-12-01亚马逊两日推出多项AI功能

> 来源：[https://xuqiwei1986.feishu.cn/docx/NnHwdpR7xoTTWjxtyd0cavQ7nnb](https://xuqiwei1986.feishu.cn/docx/NnHwdpR7xoTTWjxtyd0cavQ7nnb)

> 🔗 原文链接： h[ttps://www.thepape](https://www.thepaper.cn/newsDetail_forward_25483311?commTag=true)r.cn/newsDetail_...

在新兴的生成式人工智能领域，云计算巨头亚马逊网络服务（AWS）一直被认为落后于其竞争对手微软 Azure 和谷歌云（Google Cloud）。

但在过去两天的 AWS Re:Invent 大会上，亚马逊奋起直追，昭告了它希望成为生成式 AI 领域领头羊的愿望。

继昨日 AWS 首席执行官 Adam Selipsky 宣布推出了名为 Amazon Q 的新型聊天机器人，升级款的 AI 系统处理器 Trainium2，并与英伟达拓展伙伴关系外。今日，AWS 数据与人工智能副总裁 Swami Sivasubramanian 在其主题演讲中又发布了一系列公告。

以下是今天 14 项功能要点：

# **1. 更多 ****LLM**** 选择： **

通过 Bedrock，亚马逊 AWS 已为企业客户提供了对模型的访问权限，如自己的预训练基础模型 Titan，以及来自第三方的基础模型，如 AI21 的 Jurassic、Anthropic 的 Claude、Meta 的 Llama 2 和 Stable Diffusion。

正如此前 9 月，亚马逊向 Anthropic 投资 40 亿美元的热忱一样，Sivasubramanian 宣布将为 Anthropic 提供更多模型支持。他大肆宣扬了 Bedrocks 对 Anthropic 的 Claude 模型的支持，AWS 成为首家支持 Claude 2.1 的云提供商。据悉，该模型拥有业界领先的 20 万 token 上下文窗口、且准确性更高，幻觉率更低。此外，Sivasubramanian 还宣布了 Bedrocks 对 Meta 的 Llama 2、70B（一种开源模型）的支持，这表明 AWS 将继续支持开源。

# **2. 支持多模式向量嵌入： **

矢量嵌入是一种将文本和其他文件转化为称为矢量的数字表示的技术。这些向量可以让模型更容易地理解相似词语之间的关系，例如“猫”与“小猫”的意思相近，继而使模型能够为用户提供更相关的回复。亚马逊此前曾宣布采用 Titan 文本嵌入技术，并在内部将其用于亚马逊上的产品推荐，但它只适用于文本。

今天，Sivasubramanian 宣布，泰坦多模型嵌入技术（Titan Multi-model Embeddings）将全面投入使用，这将使在 LLM 中提供多模态搜索和推荐选项变得更加容易。

# **3. 文本****生成模型**** Titan TextLite 和 Tita**n TextExpress 已全面上市：

** **TextLite 是一种轻量级模型，适用于聊天机器人内的文本摘要、文案撰写和微调，而 Titan TextExpress 则适用于开放式文本生成和对话聊天。

# **4. 泰坦图像生成器（Titan Image G****en****erator）可**在预览模式下生成隐形水印，以确保安全：

这种模式使客户能够使用简单的语言提示，生成用于增强现有图像的高质量逼真图像。用户可以使用自己的数据自定义图像，创建反映自己品牌的内容。Sivasubramanian 表示，该模型在不同的数据集上进行训练，以实现准确的输出，同时还能减少毒性和偏差。

他说，人类评估员的测试结果表明，该模型的得分高于其他竞争模型。此外，该模型生成的所有图像默认都带有隐形水印，“旨在帮助避免虚假信息的传播……并具有防篡改功能”。他说，Titan Image Generator 是市场上第一款带有这种水印的产品。

在主题演讲中，Sivasubramanian 以一张鬣蜥图片为例，展示了该模型的编辑功能。其中一项功能叫做“outpainting”，允许用户替换图片的背景，在演示案例中，Sivasubramanian 用雨林图片替换了普通背景。此外，他还展示了如何改变图片主体，以及如何使用自然语言来改变鬣蜥的朝向。

# 5.使检索增强生成（RAG）更容易：

亚马逊正在使用 RAG 让 LLMs 搜索自己的专有数据存储。通常情况下，RAG 非常复杂，工程师必须将数据转换为矢量嵌入，并将其存储到矢量数据库中，需要的时间在数周甚至数月。为此，亚马逊发布了亚马逊 Bedrock 知识库，允许企业用户只需将 LLM 指向他们的数据位置（如 S3 存储桶），AWS Bedrock 就能获取相关文本或文档，并自动完成所有矢量转换。此外，它还可与 Vector Engine、Redis Enterprise Cloud 和 Pinecone 等流行的矢量数据库配合使用。亚马逊还宣布将“很快”支持 Amazon Aurora,、MongoDB 和更多数据库。

# 6. 亚马逊 Bedrock 模型评估预览：

这是企业评估、比较和选择最适合其用例的基础模型的一种方法。

# 7. 自动“代理”应用 RAG DIY：

生成式 AI 代理（Agents）最近很热，因为它们可以在一定程度上自主行动。Agents 是通过动态调用各种应用程序接口来执行复杂任务的人工智能应用程序，亚马逊通过 Agents for Amazon 对此提供了支持，该代理于昨天全面上线。

今天早上，Sivasubramanian 展示了一个名为 RAG DIY 的假想代理，以展示代理的能力。由 LLM 驱动的 RAG DIY 助手基于 Bedrock 中的 Claude 2，允许人们使用自然语言提问来完成家居和其他项目。Swami 举了一个例子：例如一位女士想更换浴室的梳妆台。那么，她可以向该助手询问任何类型的产品，并收到一份详细的步骤、材料和工具清单，以及所需的许可证。该助手会利用用户的输入，并调用 Titan image generator 的图像，然后，RAP DIY 应用程序使用多模态模型嵌入来搜索其大量库存，并检索所需的所有产品。通过调用专门用于摘要任务的 Cohere Command 模型，该助手还可以为她提供任何产品的用户评论摘要。

# **8. Gen ****AI** 创新中心帮助企业建立定制模型：

AWS 今年早些时候宣布成立创新中心，为企业建立基础模型提供专家帮助，包括数据科学和战略专业知识。AWS 今日宣布，从明年开始，它将为围绕 Anthropic 的 Claude 模型构建提供定制支持，包括提供专家团队，帮助企业利用自身数据对模型进行微调。

# **9.**** 用**于模型训练的 Sagemaker Hyperpod 移至 GA：

越来越多的公司发现，基础模型的训练过程极具挑战性。它需要海量数据、创建和维护由数千个 AI 加速器组成的昂贵集群、编写代码以在集群间分配模型训练。近日，亚马逊宣布与 Nvidia 达成合作，而这确保了对最新 GPU 集群的访问，因此 Hyperpod 可以为客户完成所有这些工作。AWS 表示，Hyperpod 可以将模型训练时间最多缩短 40%。此外，AWS 还宣布了 Sagemaker 在推理、训练和 MLOps 方面的一系列其他功能。

# 10. 重要的数据库集成，包括矢量支持：

亚马逊为管理数据的企业公司提供了最广泛的云数据库，但这些数据库仍然是孤岛式的，使得企业公司更难通过 LLM 轻松访问这些数据。亚马逊已经意识到了这一点，并正在努力打破这些孤岛，而微软对其 Fabric 计划的吹捧也刺激了亚马逊。目前，亚马逊已开始投资于这一所谓的零 ETL 愿景，并从去年开始实施，整合了自己的一些数据库，如 Aurora 和 Redshift。

今日，AWS 宣布整合 Amazon OpenSeach 和 Amazon S3，让用户可以在一个地方分析和可视化所有日志数据，而无需创建任何 ETL 管道。昨日，亚马逊宣布在其 Redshift lakehouse 数据库与其流行的 Aurora Postgres、Dynamo DB 和 Redis MySQL 数据库之间，以及 DynamoDB 与 OpenSearch 之间实现零 ETL 集成。

但企业对集成的渴望还包括允许在多个数据库中存储和查询矢量数据。最近，亚马逊为其 Aurora MySQL（一种基于云的关系数据库）添加了矢量搜索支持。今年 7 月，亚马逊在预览模式下为其 OpenSearch Serverless 产品推出了矢量数据库功能 Vector Engine。今天，该功能已进入全面可用状态。

同样在今天，AWS 宣布其两个更受欢迎的数据库 --DocumentDB 和 DynamoDB 都支持矢量搜索，DocumentDB 客户可以在同一个数据库中同时存储源数据和矢量数据。

# **11. 矢****量搜索可用于 **Redis 的内存数据库（预览模式）：

** **Sivasubramanian 强调了银行等注重安全的大型公司在欺诈检测或实时聊天机器人等用例中使用“超快”矢量搜索的需求。他说，DB for Redis 现在可以存储数百万个向量，并为向量查询提供个位数毫秒的响应时间。

# **12****. **结合 Neptune Analytics，现已进入 GA：

Sivasubramanian 补充说，客户正在寻找利用图形分析来分析相互关联数据的方法，因此亚马逊正在将矢量搜索与图形分析能力结合起来，以发现数据间更多隐藏的关系 -- 这可以让 LLM 变得更加强大。Neptune Analytics 是亚马逊图形数据库 Neptune 的分析引擎，它允许数据科学家从亚马逊 Neptune 图形数据或 S3 上的数据湖中找到联系，速度比以前快“80 倍”。它将图形和矢量数据存储在一起。Sivasubramanian 以 Snap 公司为例，该公司使用 Neptune Analytics 在“短短几秒钟内”就找到了其 5000 万活跃用户中的数十亿个连接。

# **13. ****让第三方在净室数**据上执行机器学习（预览模式）：

亚马逊宣布，客户可以在所谓的“净室”中与第三方共享数据，然后让他们在数据上运行机器学习（ML）模型，以获得预测性见解。这项服务被称为 AWS Clean Rooms ML。Sivasubramanian 表示，虽然基本的 ML 模型现在就可以使用，但专业的医疗保健和其他模型将在“未来几个月”推出。

# **14.**** 用于亚马逊 Redshi**ft 中生成式 SQL 的 Amazon Q：

Amazon Q 是一款为特定企业量身定制的人工智能助手，是 AWS 昨天发布的重头戏。亚马逊今天宣布 Q 可以支持 SQL，即客户日常用于查询文件的主要编码语言。有时这些 SQL 查询可能很复杂，但现在用户可以使用 Q 将自然语言提示转化为定制的查询建议，用于分析亚马逊 Redshift lakehouse 中 PB 级的非结构化数据。目前该功能还处于预览阶段。不久，用户还可以使用 Q 创建使用自然语言的数据集成管道（AWS 称之为 Amazon Glue）。

从这两日的宣告来看，亚马逊寻求差异化的战略主要在两个方面。首先是为客户提供选择。昨日，Selipsky 在会上暗暗影射了微软，称微软依赖于一家 LLM 公司（OpenAI），而亚马逊则计划向许多供应商提供服务。其次，亚马逊的另一项战略是打破各种数据库之间的孤岛，让企业客户在使用 LLM 时可以更轻松地利用他们的专有数据。

生成式 AI 的热潮已经席卷到了云计算巨头间，亚马逊 AWS 会迎头赶超，还是微软 Azure、谷歌 Cloud 将继续领跑？

参考来源：

https://venturebeat.com/ai/amazon-awss-barrage-of-gen-ai-announcements-aim-to-outdo-microsoft/