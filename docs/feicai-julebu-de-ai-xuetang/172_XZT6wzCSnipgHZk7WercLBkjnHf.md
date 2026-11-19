
# Insights into DeepSeek-V3: Scaling Challenges and Reflections on

> 来源：[https://feicaiclub.feishu.cn/docx/V2SBdslkUojKYtxE3MEcycrKnZb](https://feicaiclub.feishu.cn/docx/V2SBdslkUojKYtxE3MEcycrKnZb)

![](img/3e453e80003a1803b687ba93289758a0.png)

这篇论文《DeepSeek-V3: 扩展挑战与AI架构硬件反思》详细探讨了如何通过硬件感知的模型协同设计来解决大规模语言模型的训练和推理挑战。DeepSeek-V3在仅使用2,048台NVIDIA H800 GPU的情况下，通过四项关键创新实现了高效扩展：多头潜在注意力机制(MLA)显著减少了KV缓存大小(每令牌仅70KB)，解决了内存效率问题；专家混合(MoE)架构实现了671B总参数但每令牌仅激活37B参数，大幅降低了计算需求；FP8混合精度训练减少了50%的内存使用而准确率损失不到0.25%；多平面网络拓扑优化集群级网络通信，降低了基础设施成本。论文还基于实际开发经验，提出了未来AI硬件发展方向，包括精确低精度计算单元、规模扩展和规模扩展融合、以及低延迟通信架构创新，为硬件和模型协同设计提供了实用蓝图，对解决AI工作负载不断增长的需求具有重要参考价值。

## 论文原文

![](img/67f447a91960725b5c73af7a66c71d34.png)

[Insights into DeepSeek-V3.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/P4oAbeMGaoaw2MxbY5pcUzAUnyd)

## 论文解读可视化

![](img/1179cc50b63bb589e6ad84acaf08cd35.png)
