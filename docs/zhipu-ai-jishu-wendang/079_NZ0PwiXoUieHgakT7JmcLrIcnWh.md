# 模型推理

> 来源：[https://ai.feishu.cn/docx/DqILdNSvaoMgH6xbuQtcprWGnLi](https://ai.feishu.cn/docx/DqILdNSvaoMgH6xbuQtcprWGnLi)

##

CogVideoX作为一个DIT架构的模型，原则上根据transformer的架构，我们的模型支持很多推理策略，这一小节我们介绍使用众多框架快速接入

目前开源社区中，CogViideoX在资源方向上diffusers框架支持了推理加速、低资源推理微调，

推理形式上，我们支持更多的玩法，

+   [Tora](https://github.com/alibaba/Tora)拓展了运动点跟随模块，

+   [Interpolation](https://github.com/feizc/CogvideX-Interpolation)支持了首尾帧拼接

+   [CogVideoX-Fun](https://github.com/aigc-apps/CogVideoX-Fun)支持了动作姿态演示。

[在Com](https://github.com/kijai/ComfyUI-CogVideoXWrapper)fyui生态中我们还有更加便捷的包装节点，社区中的一些视频到视频、姿态到视频都有完整的流程图演示

在[低资源适配中，采样fp8 ](https://github.com/thu-ml/SageAttention)j精度下，我们可以做到2B模型仅需4G显存运行推理，5B仅需9G显存，这些都基于sageattention 的支持(Linux only) ，通过此方法你可以减少~20-30% 的资源,

在多策略中陆续也有一些作者对我们的模型进行了LoRAs和ControlNet的微调开源