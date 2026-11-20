
# Lora_style_2

> 来源：[https://ai.feishu.cn/docx/Hk0Ndk0xCoLrlVxgnNRcKFtXnTh](https://ai.feishu.cn/docx/Hk0Ndk0xCoLrlVxgnNRcKFtXnTh)

## 实验信息

训练数据来源猫和老鼠

大概五秒左右的数据片段.最终生成47672个训练样本，

这次样本抽取训练集中的 `test_2*`、`test_3*` 序号相关的数据，大概6000左右的训练样本

![](img/593f972330e33b36bd867f2a3247a010.png)

[`cogvideox_2b_lora.yaml`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/PLXbbe7tgo4z8ZxEfYHcRnhsnde)

![](img/4f59ad17c03623edf8a1194f84a5e586.png)

[`sft.yaml`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/RpuqbGSosokETwxr9YucXzIgnsh)

## 结果评价

这次进行了详细的抽样，从700到4000的抽样信息如下

![](img/4ee1c8b802572ba6630e62a991e741be.png)

[`抽样.txt`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Qs5bbMvCuoSE2VxXLR4cDzLYnYg)

### 总体结论

+ 低抽样点（700、800）：模型无法正确渲染人物，场景变化过大，生成质量不佳。

+ 中间抽样点（1300、3000）：生成的内容无法识别，主体运动不连贯，场景无法识别。

+ 较高抽样点（3200、3600）：能识别主题信息，但存在 CFG 过高的问题，导致主体无法识别，场景异常。人物无法保持正常的肢体结构，面部细节有问题。

+ 更高抽样点（3700、3800、3900）：无法生成有用信息或出现奇怪的细节。出现 CFG 融合问题，物体边缘明显。场景显示正常，但人物变形。

### 建议

+ 避免过度融合导致的生成问题。

+ 优化模型训练：考虑增加训练数据的多样性，特别是针对人物渲染和肢体动作的样本。

+ 调整推理步骤：尝试不同的推理步数，以寻找最佳生成效果。

+ 进一步测试：在抽样点 4000 及以上继续测试，观察模型性能的变化。

## 新的任务

在5E-4上拟合一个只有tom和jerry的数据集，大概267条数据，增加分片视频首帧和尾帧的描述，总结之后作为训练提示词

## 关联任务

[Lora_style_3](https://zhipu-ai.feishu.cn/wiki/SMLJw9lXTi3PEYkWlAycLD9InOA)

## 抽样数据

多维表格（Pdv0bQFu7arQTAsobKQcVC0Xnvc_tbldmMu13FPygerQ）
