
# 公式识别微调练习打榜赛

> 来源：[https://aicarrier.feishu.cn/docx/LH9KdEmrOoLnKUxkAP7cSum3nOd](https://aicarrier.feishu.cn/docx/LH9KdEmrOoLnKUxkAP7cSum3nOd)

欢迎参加“`Intern-S1` 公式识别微微调练习赛”！本次比赛旨在鼓励开发者利用 `Intern-S1`、`InternVL` 模型，通过微调提升其在数学公式识别任务上的性能。

**🔥 悬赏：第一位使用「沐曦算力」+「XTuner」+「`InternVL 3.5-1B`](https://aicarrier.feishu.cn/wiki/LzvswzVNbisIK1kNBhGcBS9gnHh)」模型在练习赛打榜中分数超过 40 分的同学获得 6666 算力点 + 1 亿 `Intern-S1` API Tokens 激励（有效期六期内）**

## 赛事简介

数学公式识别是计算机视觉领域一个富有挑战性的任务。本次练习赛提供了一个完整的框架，帮助您快速上手、验证和评估您的微调模型。您将使用提供的数据集对 `Intern-S1-mini` 或 `InternVL` 模型进行微调，并通过一个结合了哈希值比对和图像相似度的综合评估系统来检验您的模型性能。

![](img/0b22852d9164a478e62ae9b1069663ea.png)

### 项目结构

Github 代码：https://github.com/JimmyMa99/VLM-formula-recognition-dataset?tab=readme-ov-file

```
├── data/                     # 示例数据
│   ├── output_eval/          # 用于评估的参考图片 (Ground Truth)
│   ├── samples/              # 训练样本的 LaTeX 文本
│   └── samples_test/         # 测试样本的 LaTeX 文本
├── eval_core/                # 评估核心逻辑
│   ├── cal_score.py          # 图像相似度计算
│   └── cal_score_hash.py     # 哈希值比较
├── infer_core/               # 推理核心逻辑
│   ├── inferVLM.py           # VLM模型推理脚本
│   └── ...
├── main_eval.py              # 主评估脚本
├── swift_config/             # Swift 微调配置
│   └── internvl3_1b_train.sh # 训练脚本示例
├── env.sh                    # 环境配置脚本
└── requirements.txt          # Python 依赖
```

### 参赛流程

#### 报名

点击以下链接进行报名

https://colearn.intern-ai.org.cn/set?s=gamef

#### 提交参赛权重

微调后的权重请从以下链接入口提交，请注意截图内容，不要填写链接，本次比赛你可以选择自己的提示词，这有可能提高你的模型识别精度

![](img/004ef1d5ccaa39e946a750fad8ba4560.png)

注意：需要选择好 `base model`，本次提供两个模型给大家进行选择，如不在此范围内的模型请勿提交，否则可能会出现测评异常情况。

本次测评会提取模型返回中的**最后一个 `latex` 块**

```
```latex

E = mc^2

```
```

#### 查看状态

点击以下链接

https://aicarrier.feishu.cn/share/base/query/shrcnM5VzC2jnErQig5Wk88kgoe

进行查看，输入你的 `uid`，比如闻星（`uid`：4）

在这里能够查看状态

![](img/cdd3f53ba6544ce8569812053e9bd808.png)

#### 排行榜

https://aicarrier.feishu.cn/share/base/dashboard/shrcnrA82hMMRd1FUuWs8EhOjag

<https://aicarrier.feishu.cn/share/base/dashboard/shrcnrA82hMMRd1FUuWs8EhOjag?iframeFrom=docx&ccm_open=iframe>

## 微调环节

### 环境配置

安装 `ms-swift`

```
pip install 'ms-swift'
```

### 数据集下载

```
#数据集下载
from modelscope.msdatasets import MsDataset
ds =  MsDataset.load('JimmyMa99/VLM-formula-recognition-dataset_intern_camp')
#您可按需配置 subset_name、split，参照“快速使用”示例代码
```

### 模型微调

我们推荐使用 Swift 框架进行模型微调。在 `swift_config/` 目录下，我们提供了一个训练脚本 `internvl3_1b_train.sh` 作为参考。

您可以根据自己的需求，调整脚本中的超参数，例如：

+   `dataset`

+   `learning_rate`

+   `num_train_epochs`

配置完成后，执行训练脚本：

```
bash swift_config/internvl3_1b_train.sh
```

## 测评环节

### 环境准备

首先，请确保您的开发环境已经就绪。

```
bash env.sh
pip install -r requirements.txt
```

### 数据准备

本次比赛的样例数据集存放在 `data/` 目录下。

+   `data/samples/`: 包含用于**训练**的 LaTeX 文本文件。

+   `data/output_eval/`: 包含与训练样本对应的**原始渲染图片**，作为 Ground Truth。

您需要利用这些数据来准备您的测试集。

### 模型推理

微调完成后，您需要使用 `infer_core/inferVLM.py` 脚本对测试图片进行公式识别。

在 `infer_core/inferVLM.py` 中，请修改以下路径：

+   `model_path`: 指向您**微调后**的模型权重目录。

+   `input_directory`: 指向包含**待识别图片**的目录。

+   `output_directory`: 用于保存模型生成的 **LaTeX 文本**的目录。

然后运行推理脚本：

```
python infer_core/inferVLM.py
```

### 性能评估

最后，使用 `main_eval.py` 脚本来评估您的模型性能。

在 `main_eval.py` 中，配置以下参数：

+   `txt_dir`: 包含模型生成的 **LaTeX 文本**的目录。

+   `ref_dir`: 包含**原始参考图片**的目录 (例如 `data/output_eval`)。

+   `report_path`: 评估报告的输出路径。

+   `hash_weight` 和 `similarity_weight`: 哈希与相似度在最终得分中的权重。

+   `similarity_threshold`: 相似度得分的通过阈值。

运行评估脚本：

```
python main_eval.py
```

脚本将在控制台输出详细的评估结果，并生成一份评估报告。

### 评估指标

+   **哈希比较成功率**: 模型生成的图片与参考图片哈希值**完全相同**的样本比例。

+   **相似度比较成功率**: 模型生成的图片与参考图片的**图像相似度**高于阈值的样本比例。

+   **最终综合得分**: 上述两项成功率的**加权平均值**，全面反映模型的性能。

#### 哈希比较原理

##### 基本概念

哈希比较是一种通过计算文件数字指纹来判断两个文件是否完全相同的方法。本脚本使用加密哈希函数对LaTeX生成的图片和参考图片进行比较。

##### 哈希函数原理

哈希函数是一种单向数学函数，可以将任意长度的输入数据映射为固定长度的输出值：

```
H(data) → hash_value
```

其中：

+   `H` 表示哈希函数

+   `data` 是输入的文件二进制数据

+   `hash_value` 是固定长度的哈希值（数字指纹）

##### 支持的哈希算法

脚本支持三种常用的加密哈希算法：

+   **MD5**: 生成128位（32个十六进制字符）的哈希值

+   **SHA1**: 生成160位（40个十六进制字符）的哈希值

+   **SHA256**: 生成256位（64个十六进制字符）的哈希值

##### 文件哈希计算过程

###### 分块读取策略

为了处理大文件并避免内存溢出，脚本采用分块读取的方式：

```
for chunk in iter(lambda: f.read(4096), b""):
    hasher.update(chunk)
```

数学表示为：

```
hash = H(chunk₁ || chunk₂ || ... || chunkₙ)
```

其中 || 表示数据连接操作。

##### 计算流程

1.  **初始化**：根据指定算法创建哈希对象

1.  **分块读取**：按4096字节分块读取文件

1.  **逐块更新**：将每个数据块输入哈希函数

1.  **生成摘要**：获得最终的十六进制哈希值

##### 相似度判断机制

###### 完全匹配原则

脚本采用严格的二进制比较方式：

```
identical = (hash_generated == hash_reference)
```

判断逻辑：

+   **相同**：`hash₁` == `hash₂` → 文件在字节级别完全相同

+   **不同**：`hash₁` ≠ `hash₂` → 文件存在任何差异

##### 整体相似度计算

批量比较的相似度用相同文件的比例表示：

```
相似度(%) = (相同文件数量 / 总文件数量) × 100
```

#### 图像相似度比较原理

##### 概述

图像相似度比较是通过多种数学方法量化两幅图像之间视觉相似程度的技术。本脚本实现了五种不同的相似度计算方法，并通过加权平均得到综合相似度分数。

##### 核心算法原理

1.  直方图相似度 (`Histogram Similarity`)

直方图描述了图像中像素强度的分布特征。

**计算步骤：**

1.  将图像转换为灰度图

1.  计算像素强度直方图：`H(i)` = count(pixels with intensity `i`)

1.  归一化直方图：`H_norm(i)` = `H(i)` / Σ `H(i)`

**相似度计算方法：**

+   相关系数法 (`Correlation`)：

```
similarity = Σ(H₁(i) × H₂(i)) / √(Σ H₁²(i) × Σ H₂²(i))
```

+   卡方距离法 (`Chi-Square`)：

```
χ² = Σ((H₁(i) - H₂(i))² / (H₁(i) + H₂(i)))
similarity = 1 / (1 + χ²)
```

+   巴氏距离法 (`Bhattacharyya`)：

```
similarity = 1 - √(1 - Σ√(H₁(i) × H₂(i)))
```

1.  结构相似性指数 (`SSIM`)

`SSIM`通过比较亮度、对比度和结构信息来评估图像质量。

**数学公式：**

```
SSIM(x,y) = [l(x,y)]^α × [c(x,y)]^β × [s(x,y)]^γ
```

其中：

+   **亮度比较**：`l(x,y)` = (2`μₓ``μᵧ` + `c₁`) / (`μₓ²` + `μᵧ²` + `c₁`)

+   **对比度比较**：`c(x,y)` = (2`σₓ``σᵧ` + `c₂`) / (`σₓ²` + `σᵧ²` + `c₂`)

+   **结构比较**：`s(x,y)` = (`σₓᵧ` + `c₃`) / (`σₓ``σᵧ` + `c₃`)

参数说明：

+   `μₓ`, `μᵧ`：图像均值

+   `σₓ`, `σᵧ`：图像标准差

+   `σₓᵧ`：协方差

+   `c₁`, `c₂`, `c₃`：稳定常数

**取值范围**：[-1, 1]，1表示完全相同

1.  均方误差相似度 (`MSE Similarity`)

基于像素级差异的相似度计算。

**MSE计算公式：**

```
MSE = (1/N) × Σᵢ(I₁(i) - I₂(i))²
```

其中：

+   `N`：像素总数

+   `I₁(i)`, `I₂(i)`：对应位置的像素值

**相似度转换：**

```
similarity = 1 - (MSE / MSE_max)
```

其中 `MSE_max` = 255² 为最大可能的MSE值。

1.  特征点相似度 (`Feature Similarity`)

通过检测和匹配图像特征点来计算相似度。

**算法流程：**

1.  **特征检测**：使用`ORB`或`SIFT`算法检测关键点

1.  **描述符计算**：为每个关键点计算特征描述符

1.  **特征匹配**：使用暴力匹配器进行特征匹配

1.  **相似度计算**：

```
similarity = good_matches / max(keypoints₁, keypoints₂)
```

**`ORB`特征**：

+   使用`FAST`角点检测器

+   采用`BRIEF`描述符

+   汉明距离匹配

**`SIFT`特征**：

+   尺度不变特征变换

+   欧几里得距离匹配

1.  感知哈希相似度 (`Perceptual Hash`)

基于图像感知特征的哈希算法，对轻微变化具有鲁棒性。

**`pHash`算法步骤：**

1.  **缩放图像**：调整到 (`hash_size`+1) × `hash_size`

1.  **计算梯度**：`diff(i,j)` = `image(i,j+1)` > `image(i,j)`

1.  **生成哈希**：将布尔梯度数组转换为二进制哈希值

**相似度计算：**

```
hamming_distance = popcount(hash₁ ⊕ hash₂)
similarity = 1 - (hamming_distance / hash_bits)
```

其中 `⊕` 表示异或运算，`popcount` 计算1的个数。

##### 综合相似度计算

###### 加权平均方法

```
comprehensive_score = Σ(wᵢ × scoreᵢ)
```

**默认权重配置：**

+   直方图相似度：20%

+   `SSIM`：30%

+   `MSE`相似度：20%

+   特征点相似度：20%

+   感知哈希：10%

###### 阈值判断机制

最终评分采用二进制判断：

```
final_score = {
    1, if comprehensive_score ≥ threshold
    0, if comprehensive_score < threshold
}
```

**评估流程：**

1.  **LaTeX编译**：将文本转换为图像

1.  **多方法计算**：并行计算五种相似度分数

1.  **加权融合**：计算综合相似度分数

1.  **阈值判断**：输出0或1的最终得分

1.  **批量统计**：计算通过率百分比

#### 算法特点分析

##### 各方法的优势

+   **直方图**：对颜色分布敏感，计算简单

+   `SSIM`：符合人眼视觉感知，结构感知强

+   `MSE`：像素级精确比较，对细微差异敏感

+   特征点：对几何变换鲁棒，关注局部特征

+   感知哈希：对小幅变化容忍度高，抗噪声

##### 互补性设计

通过组合多种方法，可以：

+   平衡精确性和鲁棒性

+   综合全局和局部特征

+   适应不同类型的图像差异

+   提高评估的可靠性

这种多算法融合的方法特别适合LaTeX公式图片的精确比较，既能检测到细微的数学符号差异，又能容忍渲染过程中的微小变化。

祝您在比赛中取得优异成绩！
