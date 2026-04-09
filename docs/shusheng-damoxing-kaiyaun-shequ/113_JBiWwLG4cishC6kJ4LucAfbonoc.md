# InternVL3.5 公式识别微调练习赛复现(swift版本)

> 来源：[https://aicarrier.feishu.cn/docx/AQX6d2vGpo6gOtxb9lFcTp6ZnOF](https://aicarrier.feishu.cn/docx/AQX6d2vGpo6gOtxb9lFcTp6ZnOF)

欢迎参加“Intern-S1 公式识别微微调练习赛”！本次比赛旨在鼓励开发者利用 InternVL 模型，通过微调提升其在数学公式识别任务上的性能。

# 赛事简介

数学**公式识别是**计**算机视觉领**域一个富有挑战性的任务。本次练习赛提供了一个完整的框架，帮助您快速上手、验证和评估您的微调模型。您将使用提供的数据集对 InternVL 模型进行微调，并通过一个结合了哈希值比对和图像相似度的综合评估系统来检验您的模型性能。

![](img/58f07c4f3290c46751d31cf7ee912236.png)

## 项目结构

Github 代码：https://github.com/JimmyMa99/VLM-formula-recognition-dataset?tab=readme-ov-file

```
├── data/                     # 示例数据
│   ├── output_eval/          # 用于评估的参考图片 (Ground Truth)
│   └── samples_test/         # 参考图片的真实latex 公式
├── eval_core/                # 评估核心逻辑
│   ├── cal_score.py          # 图像相似度计算
│   └── cal_score_hash.py     # 哈希值比较
├── infer_core/               # 推理核心逻辑
│   ├── infervl.py            # VL模型推理脚本
│   └── ...
├── swift_config/                       # 模型微调配置文件
│   ├── internvl3.5_1b_train.sh         # internvl3.5微调脚本
│   └── ...
├── eval.py                   # 一键评估入口脚本
├── env.sh                    # 环境配置脚本
└── uoload.py                 # 上传模型到魔搭
└── README.md                 # 项目说明
```

## 参赛流程

xx

# 微调环节

## 环境配置

安装必要的包和ms-swift

```
conda create -n ms-swift python=3.10 -y
conda activate ms-swift

git clone https://gh.llkk.cc/https://github.com/fak111/VLM-formula-recognition-dataset.git
cd VLM-formula-recognition-dataset
pip install -r requirements.txt
pip install transformers -U

git clone https://xget.xi-xu.me/gh/modelscope/ms-swift.git
cd ms-swift
pip install -e .
```

## 数据集

其中数据集已下载到共享目录下/root/share/datasets/VLM-formula-recognition-dataset_intern_camp/train/train_mini_abs.jsonl不需要重复下载。

![](img/d7cc33edcc9dff446bff8266b6f6fc9b.png)

数据集网站：https://modelscope.cn/datasets/JimmyMa99/VLM-formula-recognition-dataset_intern_camp

## 模型微调vl

我们推荐使用 Swift 框架进行模型微调。在 swift_config/ 目录下，我们提供了一个训练脚本 internvl3.5_1b_train.sh 作为参考。

您可以根据自己的需求，调整脚本中的超参数，例如：

+   dataset 数据集位置

+   learning_rate 自调学习率

+   num_train_epochs 自调训练轮数

配置完成后，执行训练脚本：

```
#!/bin/bash

# 创建日志目录
LOG_DIR=&quot;logs&quot;
mkdir -p $LOG_DIR

# 获取当前时间戳
TIMESTAMP=$(date +&quot;%Y%m%d_%H%M%S&quot;)
LOG_FILE=&quot;$LOG_DIR/[SFT]internvl3.5_1b_${TIMESTAMP}.log&quot;

# 设置环境变量
# export ENABLE_AUDIO_OUTPUT=False
export OMP_NUM_THREADS=1
export CUDA_VISIBLE_DEVICES=0

# 设置随机端口号，避免端口冲突
export MASTER_PORT=$((10000 + RANDOM % 50000))

# 先打印启动信息
echo &quot;Starting training...&quot;
echo &quot;Log file: $LOG_FILE&quot;
echo &quot;Using port: $MASTER_PORT&quot;

# 没有指定 model_type
# 启动训练并获取PID
nohup swift sft \
    --model &#x27;/root/share/new_models/InternVL3.5/InternVL3_5-1B&#x27;\
    --dataset &#x27;/root/share/datasets/VLM-formula-recognition-dataset_intern_camp/train/train_mini_abs.jsonl&#x27; \
    --eval_steps 1000 \
    --train_type lora \
    --lora_rank 4 \
    --lora_dropout 0.01 \
    --lora_alpha 8 \
    --torch_dtype bfloat16 \
    --num_train_epochs 5 \
    --per_device_train_batch_size 1 \
    --per_device_eval_batch_size 1 \
    --learning_rate 1e-4 \
    --warmup_ratio 0.05 \
    --gradient_accumulation_steps 4 \
    --save_steps 2000 \
    --save_total_limit 10 \
    --gradient_checkpointing_kwargs &#x27;{&quot;use_reentrant&quot;: false}&#x27; \
    --logging_steps 1 \
    --max_length 6000 \
    --output_dir ./swift_output/SFT-InternVL3_5-1B\
    --dataset_num_proc 8 \
    --dataloader_num_workers 8 \
    --metric acc \
    --freeze_vit true \
    &gt; &quot;$LOG_FILE&quot; 2&gt;&amp;1 &amp;

# 获取PID并等待一下确保进程启动
TRAIN_PID=$!
sleep 2

# 检查进程是否还在运行
if kill -0 $TRAIN_PID 2&gt;/dev/null; then
    echo &quot;Training started successfully with PID $TRAIN_PID&quot;
    echo &quot;To view logs in real-time, use:&quot;
    echo &quot;tail -f $LOG_FILE&quot;
    echo &quot;&quot;
    echo &quot;To stop training, use:&quot;
    echo &quot;kill -9 $TRAIN_PID&quot;
else
    echo &quot;Failed to start training process&quot;
    echo &quot;Check log file for errors: $LOG_FILE&quot;
fi

```

### 启动

```
cd VLM-formula-recognition-dataset
bash swift_config/internvl3.5_1b_train.sh
```

输入下面命令可以查看 log

```
tail -f xxx.log
```

等微调运行完成后，将 lora 微调产生的部分权重和源权重进行合并。

### 合并

请将以下黄色的路径替换为实际存储在您服务器上的模型权重文件路径：

```
swift export --adapters  VLM-formula-recognition-dataset/swift_output/SFT-Internvl/v0-20250904-171152/checkpoint-375   --merge_lora true 
```

整体目录结构如下

![](img/252ee19cd83f272539ef7a90536fd305.png)

### 特别注意  如果微调的 是s1

如果微调的是intern-s1-mini ，需要对照原目录并补充缺失的这些文件：

![](img/02f0800c65f93e74acd3c0991abae4d2.png)

快捷补全文件命令，注意把DST 换成微调后权重的目录位置即可：

```
SRC=&quot;/root/share/new_models/Intern-S1-mini&quot; DST=&quot;./checkpoint&quot; rsync -ah --ignore-existing &quot;$SRC&quot;/ &quot;$DST&quot;/
```

## 提交

上传模型到 model

```
from modelscope.hub.api import HubApi
from modelscope.hub.constants import Licenses, ModelVisibility

# 配置基本信息
YOUR_ACCESS_TOKEN = &#x27;ms-9fxx7e&#x27;#填写自己的 api token
api = HubApi()
api.login(YOUR_ACCESS_TOKEN)

# 取名字
owner_name = &#x27;xx&#x27;    # ModelScope 的用户名，需根据自己情况修改
model_name = &#x27;xx&#x27;    # 为模型库取个响亮优雅又好听的名字，需根据自己情况修改
model_id = f&quot;{owner_name}/{model_name}&quot;

#创建模型仓库
api.create_model(
     model_id,
     visibility=ModelVisibility.PUBLIC,
     license=Licenses.APACHE_V2,
     chinese_name=f&quot;{owner_name}的 latex 分类&quot;
    )

#上传模型到仓库
api.upload_folder(
    repo_id=f&quot;{owner_name}/{model_name}&quot;,
    folder_path=&#x27;xx&#x27;,    # 微调后模型的文件夹名称
    commit_message=&#x27;upload model folder to repo&#x27;,    # 写上传信息
)
```

上传后，提交表单的 prompt 参考如下：

```
&quot;&quot;&quot;请根据图片中的公式生成对应的 latex 公式文本，不要任何解释。

  输出格式要求：
  1. 必须使用 ```latex 代码块包裹
  2. 仅包含 LaTeX 代码，无任何文字说明
  3. 确保语法正确，下标用 {} 括起来

  输出案例：
  案例 1：
  ```latex
  \sum_{i=1}^{n} x_i = \mu
  ```

  案例 2：
  ```latex
  \begin{bmatrix}
  a &amp; b \\
  c &amp; d
  \end{bmatrix}
  ```
&quot;&quot;&quot;

```

注意模型权重 id 写对如下图：

![](img/a020b1488c9deb7629d1eda7458ca6e3.png)

提交完成最终获得成绩🎉🎉

![](img/1a95fc5a9e6a231bed0ecc2c11ed68f8.png)

# 测评环节

如完成微调环节，直接提交飞书后台测试即可。本测评环节只做参考。

## 下载 latex 资源

```
cd /root/share/tex/
dpkg -i *.deb
```

## 模型评估

微调完成后，您需要使用 eval.py 脚本对测试图片进行公式识别。

请修改以下路径：

+   model-path: 输入待测试的模型路径

+   input_dir: 输入待测试的**图片**目录

+   output_dir:  输出用于保存模型生成的 **LaTeX 文本**的目录。

+   report_path: 输出**报告结果**的路径。

然后运行评估脚本：

测试原本的InternVL3-1B表现：

```
python eval.py \
  --model-path /root/share/new_models/InternVL3/InternVL3-1B\
  --input-dir ./data/output_eval \
  --output-dir ./data/results_7 \
  --report-path ./evaluation_report.txt \
  --model-type vl
```

![](img/99d41f5dc766ffcafbe940bdae4cfa4e.png)

评估微调后的表现：

```
python eval.py \
  --model-path /root/user/VLM-formula-recognition-dataset/swift_output/SFT-Interns1mini/v0-20250904-143006/checkpoint-375-merged\
  --input-dir ./data/output_eval \
  --output-dir ./data/results_s1 \
  --report-path ./evaluation_report.txt \
  --model-type s1
```

![](img/4d94aa1ec65fedf170899c15cd0f8650.png)

脚本将在控制台输出详细的评估结果，并生成一份评估报告。

## 评估指标概述

+   **哈希比较成功率**: 模型生成的图片与参考图片哈希值**完全相同**的样本比例。

+   **相似度比较成功率**: 模型生成的图片与参考图片的**图像相似度**高于阈值的样本比例。

+   **最终综合得分**: 上述两项成功率的**加权平均值**，全面反映模型的性能。

### 哈希比较原理

#### 基本概念

哈希比较是一种通过计算文件数字指纹来判断两个文件是否完全相同的方法。本脚本使用加密哈希函数对LaTeX生成的图片和参考图片进行比较。

#### 哈希函数原理

哈希函数是一种单向数学函数，可以将任意长度的输入数据映射为固定长度的输出值：

```
H(data) → hash_value
```

其中：

+   H 表示哈希函数

+   data 是输入的文件二进制数据

+   hash_value 是固定长度的哈希值（数字指纹）

#### 支持的哈希算法

脚本支持三种常用的加密哈希算法：

+   **MD5**: 生成128位（32个十六进制字符）的哈希值

+   **SHA1**: 生成160位（40个十六进制字符）的哈希值

+   **SHA256**: 生成256位（64个十六进制字符）的哈希值

#### 文件哈希计算过程

##### 分块读取策略

为了处理大文件并避免内存溢出，脚本采用分块读取的方式：

```
for chunk in iter(lambda: f.read(4096), b&quot;&quot;):
    hasher.update(chunk)
```

数学表示为：

```
hash = H(chunk₁ || chunk₂ || ... || chunkₙ)
```

其中 || 表示数据连接操作。

#### 计算流程

1.  **初始化**：根据指定算法创建哈希对象

1.  **分块读取**：按4096字节分块读取文件

1.  **逐块更新**：将每个数据块输入哈希函数

1.  **生成摘要**：获得最终的十六进制哈希值

#### 相似度判断机制

##### 完全匹配原则

脚本采用严格的二进制比较方式：

```
identical = (hash_generated == hash_reference)
```

判断逻辑：

+   **相同**：hash₁ == hash₂ → 文件在字节级别完全相同

+   **不同**：hash₁ ≠ hash₂ → 文件存在任何差异

#### 整体相似度计算

批量比较的相似度用相同文件的比例表示：

```
相似度(%) = (相同文件数量 / 总文件数量) × 100
```

### 图像相似度比较原理

#### 概述

图像相似度比较是通过多种数学方法量化两幅图像之间视觉相似程度的技术。本脚本实现了五种不同的相似度计算方法，并通过加权平均得到综合相似度分数。

#### 核心算法原理

1.  直方图相似度 (Histogram Similarity)

直方图描述了图像中像素强度的分布特征。

**计算步骤：**

1.  将图像转换为灰度图

1.  计算像素强度直方图：H(i) = count(pixels with intensity i)

1.  归一化直方图：H_norm(i) = H(i) / Σ H(i)

**相似度计算方法：**

+   相关系数法 (Correlation)：

```
similarity = Σ(H₁(i) × H₂(i)) / √(Σ H₁²(i) × Σ H₂²(i))
```

+   卡方距离法 (Chi-Square)：

```
χ² = Σ((H₁(i) - H₂(i))² / (H₁(i) + H₂(i)))
similarity = 1 / (1 + χ²)
```

+   巴氏距离法 (Bhattacharyya)：

```
similarity = 1 - √(1 - Σ√(H₁(i) × H₂(i)))
```

1.  结构相似性指数 (SSIM)

SSIM通过比较亮度、对比度和结构信息来评估图像质量。

**数学公式：**

```
SSIM(x,y) = [l(x,y)]^α × [c(x,y)]^β × [s(x,y)]^γ
```

其中：

+   **亮度比较**：l(x,y) = (2μₓμᵧ + c₁) / (μₓ² + μᵧ² + c₁)

+   **对比度比较**：c(x,y) = (2σₓσᵧ + c₂) / (σₓ² + σᵧ² + c₂)

+   **结构比较**：s(x,y) = (σₓᵧ + c₃) / (σₓσᵧ + c₃)

参数说明：

+   μₓ, μᵧ：图像均值

+   σₓ, σᵧ：图像标准差

+   σₓᵧ：协方差

+   c₁, c₂, c₃：稳定常数

**取值范围**：[-1, 1]，1表示完全相同

1.  均方误差相似度 (MSE Similarity)

基于像素级差异的相似度计算。

**MSE计算公式：**

```
MSE = (1/N) × Σᵢ(I₁(i) - I₂(i))²
```

其中：

+   N：像素总数

+   I₁(i), I₂(i)：对应位置的像素值

**相似度转换：**

```
similarity = 1 - (MSE / MSE_max)
```

其中 MSE_max = 255² 为最大可能的MSE值。

1.  特征点相似度 (Feature Similarity)

通过检测和匹配图像特征点来计算相似度。

**算法流程：**

1.  **特征检测**：使用ORB或SIFT算法检测关键点

1.  **描述符计算**：为每个关键点计算特征描述符

1.  **特征匹配**：使用暴力匹配器进行特征匹配

1.  **相似度计算**：

```
similarity = good_matches / max(keypoints₁, keypoints₂)
```

**ORB特征**：

+   使用FAST角点检测器

+   采用BRIEF描述符

+   汉明距离匹配

**SIFT特征**：

+   尺度不变特征变换

+   欧几里得距离匹配

1.  感知哈希相似度 (Perceptual Hash)

基于图像感知特征的哈希算法，对轻微变化具有鲁棒性。

**pHash算法步骤：**

1.  **缩放图像**：调整到 (hash_size+1) × hash_size

1.  **计算梯度**：diff(i,j) = image(i,j+1) > image(i,j)

1.  **生成哈希**：将布尔梯度数组转换为二进制哈希值

**相似度计算：**

```
hamming_distance = popcount(hash₁ ⊕ hash₂)
similarity = 1 - (hamming_distance / hash_bits)
```

其中 ⊕ 表示异或运算，popcount 计算1的个数。

#### 综合相似度计算

##### 加权平均方法

```
comprehensive_score = Σ(wᵢ × scoreᵢ)
```

**默认权重配置：**

+   直方图相似度：20%

+   SSIM：30%

+   MSE相似度：20%

+   特征点相似度：20%

+   感知哈希：10%

##### 阈值判断机制

最终评分采用二进制判断：

```
final_score = {
    1, if comprehensive_score ≥ threshold
    0, if comprehensive_score &lt; threshold
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

+   **SSIM**：符合人眼视觉感知，结构感知强

+   **MSE**：像素级精确比较，对细微差异敏感

+   **特征点**：对几何变换鲁棒，关注局部特征

+   **感知哈希**：对小幅变化容忍度高，抗噪声

#### 互补性设计

通过组合多种方法，可以：

+   平衡精确性和鲁棒性

+   综合全局和局部特征

+   适应不同类型的图像差异

+   提高评估的可靠性

这种多算法融合的方法特别适合LaTeX公式图片的精确比较，既能检测到细微的数学符号差异，又能容忍渲染过程中的微小变化。

祝您在比赛中取得优异成绩！

# qa

### Swift 安装有问题

```
git clone https://xget.xi-xu.me/gh/modelscope/ms-swift.git
cd ms-swift
pip install -e .
```

![](img/81c5cf3100d6dd7e848f38c51f8c3ec1.png)

```
python -m src.workflow.pipeline \
  --poster_width 54 --poster_height 36 \
  --paper_path ./data/Truly_Scale_Equivariant_Deep_Nets_with_Fourier_Layers/paper.pdf \
  --text_model deepseek-chat \
  --vision_model deepseek-chat \
  --logo ./data/Truly_Scale_Equivariant_Deep_Nets_with_Fourier_Layers/logo.png \
  --aff_logo ./data/Truly_Scale_Equivariant_Deep_Nets_with_Fourier_Layers/aff.png
  
 
```

### 原始数据集处理

下载

```
apt-get install git-lfs
git lfs install
git clone https://www.modelscope.cn/datasets/JimmyMa99/VLM-formula-recognition-dataset_intern_camp.git
```

处理

```
python - &lt;&lt;&#x27;PY&#x27;
import json, os, sys

in_path  = &quot;xx/VLM-formula-recognition-dataset/train/train_mini.jsonl&quot;       # 你的现用 jsonl
out_path = &quot;xx/VLM-formula-recognition-dataset/train/train_mini_abs.jsonl&quot;   # 输出到新文件jsonl
base = &quot;xx/VLM-formula-recognition-dataset_intern_camp/train&quot;  # train_swift.jsonl所在的文件夹

def to_abs(p):
    if not p: return p
    # 仅当是相对路径时拼接；已是绝对路径则保留
    return p if os.path.isabs(p) else os.path.join(base, p)

n=0;m=0
with open(in_path,&#x27;r&#x27;,encoding=&#x27;utf-8&#x27;) as fin, open(out_path,&#x27;w&#x27;,encoding=&#x27;utf-8&#x27;) as fout:
    for line in fin:
        if not line.strip(): continue
        obj = json.loads(line)
        changed = False
        # 兼容两种常见结构：images: [..] 或 messages[].content[].image
        if &#x27;images&#x27; in obj and isinstance(obj[&#x27;images&#x27;], list):
            obj[&#x27;images&#x27;] = [to_abs(x) for x in obj[&#x27;images&#x27;]]
            changed = True
        if &#x27;messages&#x27; in obj:
            for msg in obj[&#x27;messages&#x27;]:
                c = msg.get(&#x27;content&#x27;)
                if isinstance(c, list):
                    for part in c:
                        if part.get(&#x27;type&#x27;) == &#x27;image&#x27; and &#x27;image&#x27; in part:
                            part[&#x27;image&#x27;] = to_abs(part[&#x27;image&#x27;])
                            changed = True
        fout.write(json.dumps(obj,ensure_ascii=False)+&#x27;\n&#x27;)
        n+=1
        m+=changed
print(f&quot;processed lines={n}, patched={m}, saved -&gt; {out_path}&quot;)
PY

```