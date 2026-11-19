
# 推理加速

> 来源：[https://v11enp9ok1h.feishu.cn/docx/DmTXdgBf0oPryFxbcfCcnvkqnag](https://v11enp9ok1h.feishu.cn/docx/DmTXdgBf0oPryFxbcfCcnvkqnag)

<https://v11enp9ok1h.feishu.cn/sync/VztedzIoUstVPDb6RNMctK8Unie>

在[低资源适配中，采样fp8 ](https://github.com/thu-ml/SageAttention)j精度下，我们可以做到2B模型仅需4G显存运行推理，5B仅需9G显存，这些都基于`sageattention` 的支持(Linux only) ，通过此方法你可以减少~20-30% 的资源，

量化后的int8、nf4权重，可能让您在更短的时间加速推理
