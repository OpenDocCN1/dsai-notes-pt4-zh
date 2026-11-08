# 8🈷️18日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/Jz2Wd6IO4oOy9tx2W6cc005LnEc](https://v11enp9ok1h.feishu.cn/docx/Jz2Wd6IO4oOy9tx2W6cc005LnEc)

> 🔔AI情报助手午报「08月18日」

> ✨✨✨✨✨✨✨✨

> 1️⃣

> 🤖

> 万字长文实录：RL界与CV界的“世...

>

+   2025年8月15日，雷峰网等举办“世界模型——通向通用智能的关键拼图”线上圆桌沙龙，探讨世界模型相关话题。

+   世界模型对自动驾驶、具身智能发展至关重要，其定义模糊，不同领域理解有差异，表征学习是实现通用智能的关键。

+   RL界与CV界的世界模型在服务对象、模型类型等方面存在不同，CV界模型缺乏奖励机制。

+   通用视频生成模型需具备动作、因果性、编辑能力等，才能成为真正的世界模型。

+   自动驾驶和具身智能的世界模型发展有各自特点和挑战，都需解决精度、物理规律建模等问题。

> 关键词：世界模型，RL，CV，自动驾驶，具身智能

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/9pPdicSQCNEqVuwk1Z2uNQ)

> 2️⃣

> 🤖

> Docker Model Runner,...

>

+   Docker推出Model Runner工具，让AI模型成Docker API“一等公民”，有新的<代码开始>/models<代码结束>端点及双API设计。

+   解决硬件加速集成、智能模型调度、多后端架构等问题，与Docker Compose深度整合。

+   对比Ollama，Docker在生态完整性有优势，对开发者更友好。

+   路线图亮点包括containerd集成、Kubernetes支持等。

+   它重新定义AI开发流程，为模型容器化和标准化部署开辟道路。

> 关键词：Docker Model Runner，AI模型，Ollama，容器化，标准化部署

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/nFnP63nKhfVhGn0xHJGytg)

> 3️⃣

> 🎯

> 细数CUTLASS 3.x中的内核设计抽...

>

+   CUTLASS 3.x旨在通过可组合、正交构建块的分层系统，最大化GEMM实现空间覆盖范围，提高代码可读性。

+   其开发了独立于特定硬件特征的概念GEMM层次结构，分为Atom、Tiled MMA/Copy等五层。

+   Collective层负责组织工作时间顺序，有mainloop和epilogue，可高度自定义。

+   Kernel层将collective mainloop和epilogue组装成设备内核，负责tile调度。

+   Device层将Kernel层暴露给主机代码，处理参数验证等。

> 关键词：CUTLASS 3.x，GEMM，内核设计，层次结构

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/nMWQ9CLF4PIj26hBiIf3tA)

+   CUTLASS 3.x概念GEMM层次结构

+   Hopper的CUTLASS示例49

+   Blackwell的示例71

+   Epilogue Visitor Tree教程

+   Blackwell的TMA warp特化内核

> 4️⃣

> 🤖

> 大模型如何推理？最火斯坦福CS25的重要...

>

+   Google DeepMind首席科学家Denny Zhou在斯坦福CS25课程分享大语言模型推理见解，总结四大关键点。

+   预训练模型有推理能力，通过生成中间token、多种提示技巧及强化学习微调可提升推理。

+   生成多个响应聚合、检索与推理结合能提高LLM推理能力，聚合多个答案比选单一答案好。

+   Denny Zhou认为推理比无推理好，Alpha微调比监督微调好，应聚焦构建真正应用程序。

+   研究可保持简洁，避免学术论文过于复杂。

> 关键词：大模型，推理，强化学习微调，检索，应用程序

>

> 🔗 文中引用的链接

+   [h](https://dennyzhou.github.io/LLM-Reasoning-Stanford-CS-25.pdf)ttps://dennyzhou.github.io/LLM-Reasoning-Stanford-CS-25.pdf

+   [ht](https://www.youtube.com/watch?v=ebnX5Ur1hBk&list=PLoROMvodv4rNiJRchCzutFw5ItR_Z27CM&index=38)tps://www.youtube.com/watch?v=ebnX5Ur1hBk&list=PLoROMvodv4rNiJRchCzutFw5ItR_Z27CM&index=38

+   [https://x.com/denny_zhou/status](https://x.com/denny_zhou/status/1948499173986201915)/1948499173986201915

> 5️⃣

> 🤖

> [Hugging Face 免费开了门 ](https://mp.weixin.qq.com/s/i2dM_uy4i4EF2PFUbohX🐧)AI Agents 课程：从入门到精通！

>

+   Hugging Face推出免费AI Agents课程，从理论到实践，定位务实，能让零基础者独立构建Agents。

+   课程含5个核心单元，从环境配置到实际用例，再到构建自己的Agent并竞技，还设有3个加分单元。

+   课程直接教授工具使用，每个单元配有预配置环境，学习有官方认证、作业点评等。

+   官方建议每周投入3 - 4小时，约5周完成课程。

+   该课程对想快速了解AI Agents领域的人是不错选择，兼具理论与实战价值。

> 关键词：Hugging Face，AI Agents课程，免费学习，实战，理论

>

> 🔗 文中引用的链接

+   [课程地址](https://huggingface.co/learn/agents-course/unit0/introduction)