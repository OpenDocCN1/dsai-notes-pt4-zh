# 更全面的数据集方案

> 来源：[https://ai.feishu.cn/docx/MYAJdxPapokB97xEOzJcgpAcnyb](https://ai.feishu.cn/docx/MYAJdxPapokB97xEOzJcgpAcnyb)

经过[CogVideoX Lora消融实验](https://zhipu-ai.feishu.cn/wiki/JqWnwca2LiPfJXkwTqlcE0RGnmf) 我们发现，一个可靠的数据集更全面的视频数据集抽取方法

yolo11的能力

clip

siglip

几秒钟完成分类微调

不断地迭代更新提示任务

一种对话范畴，通过抽象一些关键点，来限定数据集的抽取时刻

例如 通过下面几组小任务，最终由大模型推理出一个合理的句子

在检测到目标后，尝试的操作

多个时间帧对比后，尝试分析目标的运动轨迹

根据目标在多个时间帧中的场景，尝试分析进行了哪些交互