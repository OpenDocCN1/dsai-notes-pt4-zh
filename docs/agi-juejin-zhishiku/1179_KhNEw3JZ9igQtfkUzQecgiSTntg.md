# 一个视频让你对flash attention2下头（比较FA2和sdpa的效率）

> 来源：[https://agijuejin.feishu.cn/docx/T1d8dHldPoljZfxg6eDcTBPNn7e](https://agijuejin.feishu.cn/docx/T1d8dHldPoljZfxg6eDcTBPNn7e)

# 👀快看👀

> **作者：**[**良睦路程序员**](https://space.bilibili.com/45156039)

> **原文链接：**[**一个视频让你对flash attenti**](https://www.bilibili.com/video/BV18ceWeFEoP/?spm_id_from=333.999.0.0)on2下头（比较FA2和sdpa的效率）

---

# ⚡️摘要·作者有话说⚡️

> 在transformers的提交区里面，看到了有人对比flash attention2和sdpa的推理效率。竟然惊奇的发现FA2并没有快很多。

> 再加上FA2又很难安装，就想让大家下下头，其实pytorch的sdpa真的挺好的。

>

> 相关参考链接：

> https://github.com/huggingface/transformers/commit/1c37e8c1a6274e6e87b45c6319eb190757214c2a

> https://github.com/huggingface/transformers/pull/31940

> https://gist.github.com/qubvel/ac691a54e54f9fae8144275f866a7ff8

---

# 🤖分集🤖

### 一个视频让你对flash attention2下头（比较FA2和sdpa的效率）

主要介绍了 flash attention v2（FAv2）与 SDPA（Sparse Dot Product Attention）在计算 attention 时的效率比较，并对两者在实际应用中的优缺点进行了分析。

![](img/6075106ac1901906963ebea5649e3d88.png)

[一个视频让你对flash attention2下头（比较FA2和sdpa的效率）.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/GJaHbzZ3coSe7xxoFTYcMr2Sn4f)

---

# ✏️课代表✏️

+   FAv2 并非总是比传统的 attention 计算方法更快，尤其是在处理小数据集时。

>

+   SDPA 在性能上展现出与 FAv2 相当甚至更优的表现，尤其在内存占用和推理速度上。

>

+   SDPA 的实现相对简单，不需要复杂的安装过程，对于开发者来说更加友好。

>

+   开发者在选择 attention 机制时，应该根据实际需求和性能测试结果来决定，而不是盲目追求某一特定技术。

>

+   持续关注 transformer 模型的最新提交记录，有助于获取行业动态和最新的技术实现。