# 02🈷️25日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/DRgtdSc1bodKUfx0ljQcZUP9nvf](https://v11enp9ok1h.feishu.cn/docx/DRgtdSc1bodKUfx0ljQcZUP9nvf)

> 🔔AI情报助手午报「02月25日」

> ✨✨✨✨✨✨✨✨

> 1️⃣

> 🚀

> [超大规模操作手册：在 GPU 集群上训练](https://mp.weixin.qq.com/s/RF1msgv0gRLcOJ4OCRslUQ) Part3(寻找最佳的GPU优化配置)

>

+   文章讨论了在GPU集群上训练大型模型时选择最佳技术组合的决策过程，涉及模型大小和Batch Size的适配问题。

+   分析了GPU资源丰富和资源匮乏情况下的不同并行技术选择，如张量并行、流水线并行和数据并行。

+   强调了实验的重要性，以找到最适合特定计算集群的配置，并讨论了如何优化训练吞吐量。

+   基准测试显示，性能严重依赖于实现质量，张量并行和流水线并行的性能比较。

+   文章还提到了在实际操作中遇到的挑战，如进程清理、作业管理器问题和内存使用模式等。

> 关键词：GPU并行技术，分布式训练，模型优化，张量并行，流水线并行

>

> 🔗 文中引用的链接

+   [nanotron 仓库](https://github.com/huggingface/nanotron)

+   [picotron 仓库](https://github.com/huggingface/picotron)

> 2️⃣

> 🤖

> [MLLM何时解决幻觉问题？北航等推出SimpleVQA](https://mp.weixin.qq.com/s/k5llT8OKAM7PAOC8lzuyOw)，首个全面多模态大语言模型事实性评估基准

>

+   北航等推出SimpleVQA，首个全面多模态大语言模型(MLLMs)事实性评估基准，涵盖中英文多任务和多场景。🌏

+   SimpleVQA包含9个任务和9个领域，通过严格质量控制流程确保答案高质量、简洁和清晰。📌

+   对18个MLLMs和8个LLMs进行全面评估，通过识别结果和分析错误案例，深入探讨模型的图像理解和知识内化生成能力。🔍

+   SimpleVQA发现MLLMs在VQA领域中的事实准确性不足，训练数据可能包含知识错误，图像内容理解是主要挑战。🚧

+   提高模型的视觉理解能力并增强模型的知识内化可以提高整体准确性，如通过监督的微调(SFT)培训。🌱

> 关键词：MLLM，SimpleVQA，多模态，视觉问答，知识内化

>

> 🔗 文中引用的链接

+   [论文链接](https://arxiv.org/abs/2502.13059)

+   [代码链接](https://github.com/SimpleVQA/SimpleVQA)

+   [榜单](https://github.com/SimpleVQA/SimpleVQA.github.io)

> 3️⃣

> 🚀

> [YOLO已经悄悄来到v12，首个以A](https://mp.weixin.qq.com/s/0VHSYYfsyG3HIjD-D2J2Rg)ttention为核心的YOLO框架问世

>

+   YOLOv12是首个以注意力机制为核心的实时目标检测框架，通过区域注意力模块和残差高效层聚合网络（R-ELAN）等创新，提升了检测性能。

+   区域注意力模块（A2）降低了attention的计算复杂度，提升了计算速度，同时保持了大感受野。

+   引入R-ELAN解决了大规模模型中的优化难题，提升了模型的优化稳定性和计算效率。

+   YOLOv12在保持高推理速度的同时，显著优于前代YOLO模型和其他实时检测器。

+   实验结果显示，YOLOv12在不同规模模型上均取得了性能提升，同时在推理速度、计算开销和参数量方面优于竞品。

> 关键词：YOLOv12，注意力机制，实时目标检测，性能提升，R-ELAN

>

> 🔗 文中引用的链接

+   [论文地址](https://arxiv.org/pdf/2502.12524)

+   [代码地址](https://github.com/sunsmarterjie/yolov12)

> 4️⃣

> 🧠

> [大模型轻量化系列解读（八）：降低LLM中因Acti](https://mp.weixin.qq.com/s/pyZ_gX_WQvsdf5pv2zGh2A)vation Spikes导致的量化误差

>

+   本文研究了大型语言模型（LLM）在后训练量化时面临的激活量化挑战，特别是GLU激活中的“激活尖峰”导致的量化误差。

+   提出了两种方法：量化自由模块（QFeM）和量化自由前缀（QFeP），通过隔离激活尖峰提升量化模型性能。

+   验证了这些方法在多个最新LLM中有效，包括LLAMA-2/3、Mistral等，显著改善了模型的推理效率和性能。

+   实验表明，量化weight和activation会降低性能，但QFeM和QFeP能显著恢复性能，接近FP16精度。

+   分析了不同激活量化方案的精度和推理时延，发现解决量化瓶颈对粗粒度量化的推理加速和降低时延很重要。

> 关键词：大模型轻量化，量化误差，LLM，GLU激活，后训练量化

>

> 🔗 文中引用的链接

+   [论文地址](http://arxiv.org/pdf/2405.14428)

+   [代码链接](http://github.com/onnoo/activation-spikes)

> 5️⃣

> 🔥

> [ICLR'25｜高效重建几何精准的大规模复杂三维场景](https://mp.weixin.qq.com/s/zKhNKWRYSNfT_s8rP2tjPg)，中科院提出CityGaussianV2

>

+   中科院自动化所张兆翔教授团队提出了CityGaussianV2算法，用于大规模复杂三维场景的高效重建，实现了快速训练和压缩，同时保证了几何结构的精准和实时渲染的逼真体验。

+   CityGaussianV2继承了CityGaussian的子模型划分与数据分配方案，并引入了基于延展率过滤和梯度解耦的稠密化技术，以及深度回归监督，合并了训练和压缩过程，有效加速算法收敛。

+   该算法在几何精度、召回率和F1-Score方面达到了最佳性能，且在渲染质量和几何精度上都取得了良好的结果，同时有效规避了显存爆炸问题，实现了显存优化。

+   CityGaussianV2填补了大规模场景下几何评测协议的空白，基于点云的目击频次统计设计了针对欠观测区域的边界估计方案，有效规避了重建效果不稳定带来的指标波动。

+   实验结果证明了CityGaussianV2在多个具有挑战性的数据集上的效率、有效性和鲁棒性，为大规模复杂场景的高效精准重建建立了几何精度评估基准。

> 关键词：三维场景重建，CityGaussianV2，几何精度，大规模场景，高效重建

>

> 🔗 文中引用的链接

+   [项目主页](https://dekuliutesla.github.io/CityGaussianV2)

+   [论文链接](https://arxiv.org/pdf/2411.00771)

+   [代码链接](https://github.com/DekuLiuTesla/CityGaussian)

> 6️⃣

> 🔮

> 当百度App喊回“百度一下，你就知道”，搜索市场风云如何变幻？

>

+   百度App的Slogan回归“百度一下，你就知道”，标志着其重新聚焦于搜索核心功能。

+   百度App通过DeepSeek和文心大模型的整合，强化了AI搜索能力，提供更精准和个性化的搜索结果。

+   生成式AI技术的发展为搜索引擎带来了新的机遇和挑战，百度App正适应这一变化。

+   百度App的升级不仅体现在Slogan上，更在于如何将AI技术与搜索业务深度融合。

+   百度App的战略布局重点在于整合资源，重塑搜索功能，以适应新时代的搜索需求。

> 关键词：AI搜索，DeepSeek，文心大模型，搜索市场，生成式AI

>

> 🔗 文中引用的链接

+   [百度App Slogan变迁](https://mp.weixin.qq.com/s/hTjDBw32RwwkllSBrJ1oIw)