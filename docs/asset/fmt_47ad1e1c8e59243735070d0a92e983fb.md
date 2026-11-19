
# 基于高清地图的自动驾驶项目

> 来源：https://v11enp9ok1h.feishu.cn/docx/EE7vdqxtJoGVvAx3mIPckHk7n3d

对于安全的自动驾驶，准确解释车道和交通标志至关重要，以确保车辆保持正确的定位并遵守驾驶规则。下图示了一个交叉路口场景，其中提取的交通标志规则被集成到高清地图上的相应车道中。

![](img/6bf9e750558d5363b34309038de2f9a5.png)

Step1 ~ Step4 展示了按规则驾驶的情况。步骤2和步骤3分别演示了两个子任务的具体作用。

![](img/7614cad739b3fc6d50c171a8ad3dab2b.png)

从交通标志中辨别规则并将其与特定车道相关联的能力对于自动导航至关重要。如上图所示，交通标志是车道级规则的主要指标。

我们提出的任务涉及两个核心子任务：

1. 从交通标志中提取车道级规则，以及
1. 在这些规则和中心线之间建立对应关系。一般来说，车辆沿着车道中心行驶。因此，我们使用中心线来表示车道。

这种方法反映了人类驾驶员观察交通标志的本能，然后将指示的规则与它们所管理的车道联系起来。

## 数据集：MapDR

> 如下图所示，本文将车道级交通规则定义为多个 `key：value` 构成的结构化表达，以便于整合至自动驾驶系统中服务于规控等下游步骤。现实场景中一个标志牌中可能包含多条车道级交通规则，同时每条交通规则也可能与多条车道中心线相关联，这也是解决此任务中的难点。

![](img/d56bdf1a002f0e7d2b3457938b04b6ba.png)

单个交通标志的多个车道级规则以 `{key：value}` 格式进行注释。有向线表示规则与特定中心线之间的对应关系。

![](img/efbbae875f840ff206a4952a91657be8.png)

MapDR 数据均高德地图的真实采集数据，关注于来自于北京、上海、广州三座城市的常见交通标志牌以及道路信息。数据集包含了超过 10,000 个行车场景，其中超过 18,000 条驾驶规则。

### 其中每个场景（一个 `Clip`）包含：

+ `Raw Data`

+ 30+ 帧连续前景图像

+ 关注交通标志的位姿（每个场景仅关注于一个标牌）

+ 以标牌为中心 `100m * 100m` 范围内的矢量化地图（包含矢量线型）

+ 相机内参以及每一帧对应的相机位姿

+ `Annotation`

+ 标牌中包含的若干条车道级交通规则以及对应的车道中心线

+ 每条交通规则对应的标牌内区域（以位姿表示）

## 数据表示

![](img/1bd82e87068bfdfdf5c2172fb549052e.png)

![](img/1ba888dd3be3f5f1f932c16cdc1199e8.png)

## Evaluation Metric & Benchmark

![](img/fa633ce4664c0a432870e70c2397fca0.png)

总体任务可以视为一个二分图匹配任务，如上图所示。

> 其中抽取车道级交通规则视作对于图中 `Rule Node` 的预测，`Rule` 和 `Centerline` 之间的关联关系可以视作图中 `Edge` 的预测。评测时两个子任务分别关注于 `Rule Node` 和 `Edge` 的 `Precision` 和 `Recall`，总体任务关注于最小子图（由一个 `Rule Node`、一个 `Centerline Node` 和 一条 `Edge` 组成）的 `Precision` 和 `Recall`

示例如下（R.E. 指规则抽取，C.R.指关联关系推理）：

![](img/29acb12222f462f669c0ce887d6f7f2f.png)

![](img/aeceb0406f7eeadd65d26a9ee71f1b25.png)

![](img/092996e2fb68531aa9a6498387b0b7b5.png)

![](img/32f21b73e439bf86c5e3f7c498216349.png)

## 模块化方法

![](img/b87304588e7c9efffaf767890c00dc88.png)

整个方法可以分为两个主要部分：

> 从交通标志中提取规则（上）和规则车道对应推理（下）。规则提取模型由两个结构相同的 `VLE` 但参数不共享的顺序阶段组成，训练过程是独立的。

### 模块化方法由三个级联模块构成：

+ `Grouping`：融合标牌图像信息和 OCR 信息完成标牌上元素的车道级区域分组

+ `Understanding`：依次融合单个分组的 OCR 和标牌图像信息，使用融合后的信息进行分类以预测交通规则中每个 `key` 对应的 `value`

+ `Association`：对矢量地图进行特征编码并且与交通规则特征进行信息融合，通过二分类完成判定每条车道中心线是否与交通规则关联

![](img/e2f98b82e33fec401ee30f774dcdc5dc.png)

+ 其中 `MEE` 模型专门用于对矢量地图进行特征编码，将每个矢量点进行 `tokenize`，使用 `learnable query` 配合 `Intra & Inter Instance Attention` 进行矢量特征的聚合，融合后每个 `query` 对应一条矢量的特征信息。

+ 通过 `CrossAttention` 进行矢量特征和交通规则特征的融合，使用融合后的 `feature token` 进行分类以判定中心线与规则的关联关系。

### 端到端方法

![](img/1cd36690cbfc3a9fb2fffaec04dc3736.png)

> `Qwen-VL`（`TextPrompt`）将光伏图像中的中心线坐标编码为文本，输入到 `QwenLM` 中；`Qwen-VL`（`VisualPrompt`）将 PV 图像中的中心线及其索引可视化，并将其作为 `Visual Prompt` 输入到 `QwenLM` 中，而 `RuleVLM` 则使用去除交叉注意力层的 `MEE` 对矢量化中心线结果进行编码，并通过适配器将其与 `LLM` 对齐。

>

> 端到端方法在多模态大模型基础上进行了探索，以 `Qwen-VL-Chat 7B` 作为基础模型，如图所示分别以不同方式进行了 `LoRA SFT`，令模型生成结构化文本形式的交通规则：

>

+ `TextPrompt`：输入完整前景图像 + 标牌图像，将矢量点坐标以文本形式进行输入。

+ `VisualPrompt`：输入包含矢量投影的完整前景图像 + 标牌图像

+ `RuleVLM`（best）：输入完整前景图像 + 标牌图像，使用 `MEE` 对矢量进行特征抽取并且对齐至 `LLM`

## 实验

![](img/61392501971b8693d65d319d5fe59795.png)

> 启发式方法和 `Qwen-VL`（`TextPrompt`）方法分别作为模块化和端到端方法的基线。“−”表示端到端方法不适合对 C.R. 进行独立评估，因为这些方法没有独立地利用规则的基本事实进行对应推理。

>

> 在 MapDR 数据集上进行的实验结果如上表所示，启发式方法（OCR 判定 + 最近车道线匹配）难以理解复杂的交通标志牌内容，同时也无法完成复杂场景下的车道-规则关联。`VLE-MEE` 和 `RuleVLM` 分别为模块化方法和端到端方法提供了有效的 `Baseline`。

>

> Conclusion

> 综上所述，本文的贡献如下：

> 1、首次提出了 `Integrating traffic regulations into online HD maps` 任务，以及用于基准测试的 MapDR 数据集和评测指标。

>

> 2、MapDR 包含 10,000+ `clip`，涵盖了不同的交通状况，并包括超过 18,000 条人工标注的车道级交通规则。所有数据都是真实采集，并且经过了细致的校验，助力于相关任务的研究。

>

> 3、提出了模块化（`VLE-MEE`）和端到端（`RuleVLM`）两种建模方法，为目标任务并和未来的研究提供有效的 `Baseline` 方法。

## BibTeX

```
@misc{chang2025drivingrulesbenchmarkintegrating,
      title={Driving by the Rules: A Benchmark for Integrating Traffic Sign Regulations into Vectorized HD Map}, 
      author={Xinyuan Chang and Maixuan Xue and Xinran Liu and Zheng Pan and Xing Wei},
      year={2025},
      eprint={2410.23780},
      archivePrefix={arXiv},
      primaryClass={cs.CV},
      url={https://arxiv.org/abs/2410.23780}
}
```
