
These trends are new, only a few years old. But there is little reason to expect this process to slow down in the next few years. Even as we perhaps use up low-hanging fruit in algorithms and datasets, like scaling past the “Chinchilla ratio” of ~20 tokens per parameter, deeper innovations and investments inside the data center and at the silicon layer promise to pick up slack.

这些趋势是新的，只有几年的历史。但没有理由预计这一进程在未来几年会放缓。即使我们可能会在算法和数据集中使用容易实现的成果，例如扩展到每个参数约 20 个代币的“Chinchilla 比率”，但数据中心和硅层内部更深入的创新和投资有望弥补这一不足。

And this is perhaps the most important strategic fact: what is a completely infeasible floor demo or research paper today will become a premium feature in a few years and then a commodity shortly after. We should build our systems, and our organizations, with this in mind.

这也许是最重要的战略事实：今天完全不可行的现场演示或研究论文将在几年内成为一项高级功能，然后不久之后就会成为一种商品。我们应该牢记这一点来构建我们的系统和我们的组织。

## Enough 0 to 1 Demos, It’s Time for 1 to N Products 0 到 1 的演示已经够多了，是时候展示 1 到 N 个产品了

We get it; building LLM demos is a ton of fun. With just a few lines of code, a vector database, and a carefully crafted prompt, we create ✨magic ✨. And in the past year, this magic has been compared to the internet, the smartphone, and even the printing press.

我们懂了;构建 LLM 演示非常有趣。只需几行代码、一个矢量数据库和精心设计的提示，我们就创造了✨神奇✨。在过去的一年里，人们将这种魔力与互联网、智能手机甚至印刷机进行比较。

Unfortunately, as anyone who has worked on shipping real-world software knows, there’s a world of difference between a demo that works in a controlled setting and a product that operates reliably at scale.

不幸的是，任何从事过实际软件开发工作的人都知道，在受控环境中运行的演示与大规模可靠运行的产品之间存在天壤之别。

Take, for example, self-driving cars. The firs[t ca](https://proceedings.neurips.cc/paper/1988/file/812b4ba287f5ee0bc9d43bbf5bbe87fb-Paper.pdf)r was driven by a neural network in 1988. Twenty-five years later, Andrej Karpathy took his first demo ride in a Waymo. A decade after that, the company received its driverless permit. That’s thirty-five years of rigorous engineering, testing, refinement, and regulatory navigation to go from prototype to commercial product.

以自动驾驶汽车为例。 1988 年，第一辆汽车由神经网络驱动。二十五年后，Andrej Karpathy 第一次试驾了 Waymo。十年后，该公司获得了无人驾驶许可证。从原型到商业产品，经历了三十五年严格的工程、测试、改进和监管导航。

Across different parts of industry and academia, we have keenly observed the ups and downs for the past year: year 1 of N for LLM applications. We hope that the lessons we have learned—from tactics like rigorous operational techniques for building teams to strategic perspectives like which capabilities to build internally—help you in year 2 and beyond, as we all build on this exciting new technology together.

在工业界和学术界的不同领域，我们敏锐地观察到了过去一年的起起落落：LLM应用程序的 N 年 1。我们希望我们学到的经验教训——从建立团队的严格运营技术等策略到内部构建哪些能力等战略视角——在第二年及以后对您有所帮助，因为我们都在这项令人兴奋的新技术的基础上共同构建。

## About the authors 关于作者

**Eugene Yan** designs, builds, and operates machine learning systems that serve customers at scale. He’s currently a Senior Applied Scientist at Amazon where he builds RecSys for m[illions world](https://eugeneyan.com/)wide [and applies LL](https://applyingml.com/)Ms to serve customers better. Previously, he led machine learning at Lazada (acquired by Alibaba) and a Healthtech Series A. He writes & speaks about ML, RecSys, LLMs, and engineering at eugeneyan.com and ApplyingML.com.

Eugene Yan 设计、构建和运营为大规模客户服务的机器学习系统。他目前是 Amazon 的高级应用科学家，为全球数百万人构建 RecSys，并应用 LLMs 更好地为客户服务。此前，他领导过 Lazada（已被阿里巴巴收购）的机器学习和 Healthtech A 轮融资。他在 eugeneyan.com 和 ApplyingML.com 撰写并谈论 ML、RecSys、LLMs 以及工程。

**Bryan Bischof** is the Head of AI at Hex, where he leads the team of engineers building Magic – the data science and analytics copilot. Bryan has worked all over the data stack leading teams in analytics, machine learning engineering, data platform engineering, and AI engineering. He started the data team at Blue Bottle Coffee, led several projects at Stitch Fix, and built the data teams at Weights and Biases. Bryan previously co-authored the book Building Production Recommendation Systems with O’Reilly, and teaches Data Science and Analytics in the graduate school at Rutgers. His Ph.D. is in pure mathematics.

Bryan Bischof 是 Hex 的人工智能主管，他领导着构建 Magic 的工程师团队——数据科学和分析副驾驶。 Bryan 曾在分析、机器学习工程、数据平台工程和人工智能工程的数据堆栈领导团队中工作过。他在 Blue Bottle Coffee 组建了数据团队，在 Stitch Fix 领导了多个项目，并在 Weights and Biases 建立了数据团队。 Bryan 此前与 O’Reilly 合着了《构建生产推荐系统》一书，并在罗格斯大学研究生院教授数据科学和分析。他的博士学位。属于纯数学。

**Charles Frye** teach[es people to](https://journals.physiology.org/doi/full/10.1152/jn.00172.2016) build AI applications. After publishing research in psychopharmacology and neurobiology, he got his Ph.D. at the University of California, Berkeley, for dissertation work on neural network optimization. He has taught thousands the entire stack of AI application development, from linear algebra fundamentals to GPU arcana and building defensible businesses, through educational and consulting work at Weights and Biases, Full Stack Deep Learning, and Modal.

Charles Frye 教人们构建人工智能应用程序。在发表精神药理学和神经生物学研究后，他获得了博士学位。在加州大学伯克利分校攻读神经网络优化论文。他通过权重和偏差、全栈深度学习和模态方面的教育和咨询工作，教授了数千人人工智能应用程序开发的整个堆栈，从线性代数基础知识到 GPU 奥秘，再到构建可防御的业务。

**Hamel Husain** [is a machi](https://www.linkedin.com/in/hamelhusain/)ne learning engineer with over 25 years of experience. He has worked with innovative companies such as Airbnb and GitHub, which included early LLM research used by OpenAI for code understanding. He has also led and contributed to numerous popular open-source machine-learning tools. Hamel is currently an independent consultant helping companies operationalize Large Language Models (LLMs) to accelerate their AI product journey.

Hamel Husain 是一位拥有超过 25 年经验的机器学习工程师。他曾与 Airbnb 和 GitHub 等创新公司合作，其中包括 OpenAI 用于代码理解的早期 LLM 研究。他还领导并贡献了许多流行的开源机器学习工具。 Hamel 目前是一名独立顾问，帮助公司实施大型语言模型 (LLMs)，以加速他们的 AI 产品之旅。

**Jason Liu** is a distinguish[ed machine](https://jxnl.co/services/) learning consultant known for leading teams to successfully ship AI products. Jason’s technical expertise covers personalization algorithms, search optimization, synthetic data generation, and MLOps systems.

Jason Liu 是一位杰出的机器学习顾问，以领导团队成功交付人工智能产品而闻名。 Jason 的技术专长涵盖个性化算法、搜索优化、合成数据生成和 MLOps 系统。

His experience includes companies like Stitch Fix, where he created a recommendation framework and observability tools that handled 350 million daily requests. Additional roles have included Meta, NYU, and startups such as Limitless AI and Trunk Tools.

他曾在 Stitch Fix 等公司工作过，在那里他创建了一个推荐框架和可观察性工具，每天处理 3.5 亿个请求。其他角色包括 Meta、纽约大学以及 Limitless AI 和 Trunk Tools 等初创公司。

**Shreya Shankar** is an ML engineer and PhD student in computer science at UC Berkeley. She was the first ML engineer at 2 startups, building AI-powered products from scratch that serve thousands of users daily. As a researcher, her work focuses on addressing data challenges in production ML systems through a human-centered approach. Her work has appeared in top data management and human-computer interaction venues like VLDB, SIGMOD, CIDR, and CSCW.

Shreya Shankar 是加州大学伯克利分校的机器学习工程师和计算机科学博士生。她是两家初创公司的第一位机器学习工程师，从头开始构建人工智能驱动的产品，每天为数千名用户提供服务。作为一名研究人员，她的工作重点是通过以人为本的方法解决生产机器学习系统中的数据挑战。她的作品曾出现在VLDB、SIGMOD、CIDR、CSCW等顶级数据管理和人机交互场所。

<https://aigcreative.feishu.cn/sync/K4Gvd9vUQsy9Oubwqaeclu0Fnbg>
