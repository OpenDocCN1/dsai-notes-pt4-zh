# 13-07神经网络-Transformer

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/RKJ7dgoicojSCYxde4QcEsmEn1g](https://qqs7y1hozd1.feishu.cn/docx/RKJ7dgoicojSCYxde4QcEsmEn1g)

**Transformer** 是一种用于处理序列数据（比如自然语言、时间序列等）的神经网络结构，它彻底改变了 NLP（自然语言处理）领域。

下面，咱们从6个方面给大家进行介绍：

##### 1. Transformer 从哪来的？为什么要它？

传统的循环神经网络（RNN）和长短期记忆网络（LSTM）在处理长序列时，会出现“信息丢失”或“记忆困难”的问题。

Transformer 则完全抛开了“按时间一步步算”的方式，改用“注意力机制”（Attention），一次性把整个句子都看一遍。这样它既能捕捉到句子里远距离词语的关系，又能并行计算，速度更快。

##### 2. Transformer 的“大脑” - 注意力机制（Attention）

1.  **注意力（Attention）**：就像人读文章时，遇到不懂的词，会去上下文里“瞄一眼”相关地方。

1.  自注意力（Self-Attention）：在一句话里，每个单词都去看这句话的其他单词，判断“和我最相关的是什么？” 比如“猫在追老鼠”，当模型看“追”这个词时，会重点关注“猫”和“老鼠”这两个词。

自注意力的计算过程大致是：

+   **查询（Query）**、**键（Key）**、**值（Value）**：把每个词分别映射成三个向量，Query 用来“提问”，Key 用来“给出提示”，Value 是“内容”本身。

+   用 Query 和所有 Key 做点积（越相关分数越大），得到的分数经过 Softmax 就是注意力权重，最后加权求和 Value，得到“融合了重要信息”的新表示。

##### 3. 多头注意力（Multi-Head Attention）

单一的注意力头可能只关注一种“关系”或“一类特征”。多头注意力就是并行做多组不同的 Query/Key/Value，让模型能从不同角度“看”句子。

最后再把多组结果拼在一起，丰富表示能力。

##### 4. 位置编码（Positional Encoding）

由于 Transformer 一开始并不考虑单词的先后顺序（因为它不按时间算），所以需要给每个位置（第几个词）加上「位置编码」，让模型知道顺序信息。通常用正余弦函数生成一组固定的向量，和词向量相加，就把顺序也带进去了。

##### 5. 整体结构：编码器 - 解码器（Encoder - Decoder）

+   **编码器（Encoder）**：把输入序列（比如一句话）编码成一串含有上下文信息的向量表示。通常有若干层，每层包含多头自注意力 + 前馈网络 + 残差连接 + LayerNorm。

+   **解码器（Decoder）**：根据编码器的输出，再结合已经生成的目标序列（比如翻译后的句子），一步步预测下一个词。也有多头注意力，既关注自己已生成的部分，也关注编码器的输出。

```
输入 → [编码器层 × N] → 编码表示
编码表示 + 已生成目标序列 → [解码器层 × N] → 下一个词概率
```

##### 6. 为什么 Transformer 这么火？

**并行化**：不按时间一步步算，可以一次把整个序列送入计算，大幅加速训练。

**捕捉长依赖**：自注意力直接连接任意两个位置，不会“越走越丢失”信息。

**可扩展**：只要堆更多层、更大的头数，就能得到更强的模型（如 BERT、GPT、T5、ViT 等）。

## 原理详解

令输入序列长度为  ，隐层维度为  。

在第   层，编码器输入为  ，解码器输入为  。

多头注意力头数为  ，每个头的维度均为  。

### Scaled Dot-Product Attention

**缩放点积注意力**，给定查询集  、键集  、值集  ，注意力输出为

+   **除以**  ：避免点积值过大导致 Softmax 梯度消失。

+   **Softmax** 在第   维度归一化，得到注意力权重矩阵  ，再与   相乘得输出。

### 多头注意力（Multi-Head Attention）

1.  **线性映射**
对同一输入  ，并行做   组不同的投影：
 
其中  。

1.  **并行注意力**
每组头分别计算：

1.  **拼接与线性变换**
将   个头的输出拼接，再投影回   维：

### 位置编码（Positional Encoding）

Transformer 本身无序列位置感知，需要向词向量中注入“位置信息”。常用“正余弦”编码：

其中  ， 。

最终输入：

### 编码器层（Encoder Layer）

每一层编码器包括三部分：多头自注意力 → Add & Norm → 前馈网络 → Add & Norm。

1.  **多头自注意力**
输入  ，输出  。

1.  **残差连接与层归一化**

1.  前馈网络（Position-wise FFN）
对每个位置独立操作：
 
其中  。

1.  **第二次残差与归一化**

整层映射：

### 解码器层（Decoder Layer）

在编码器基础上，解码器多了“掩码自注意力”和“对编码器的注意力”两部分。

1.  掩码自注意力（Masked Self-Attention）
同 MHA，但在   前加上“未来位置掩码”  ：
 
保证解码时只能看到当前位置及之前位置。

1.  **残差 & Norm**

1.  编码—解码注意力（Encoder–Decoder Attention）
将   作为  ，编码器输出   作为  ：

1.  **前馈网络 & Norm**

### 整体训练与推理流程

#### 训练阶段（Encoder–Decoder 共训练）

1.  对源序列  ，做词嵌入并加位置编码，得到  ；目标序列   同理，生成  。

1.  编码器层叠   次，输出  。

1.  解码器层叠   次，每次用掩码自注意力、对编码器注意力和 FFN，输出  。

1.  在   最后做线性+Softmax，预测下一个词分布。

1.  用交叉熵损失与真实   对比，反向传播更新所有参数。

#### 推理阶段（自回归生成）

1.  给定源序列编码得到  。

1.  初始化  ，迭代：

+   将已生成   输入解码器，得到下一步分布

+   取   或采样

+   若   则停止，否则继续。

### 关键设计动机

1.  **并行化计算** ：摈弃 RNN 的时间依赖，序列可一次性输入。

1.  **长程依赖捕获** ：自注意力直接连接任意两点，得以高效学习长距离关联。

1.  **可扩展性** ：堆叠更多层、更粗的头数即能升级成更强模型（如 GPT、BERT、T5 等）。

## 完整案例

这里，咱们使用 PyTorch 从零构建一个小规模的 Transformer Encoder，用于合成正弦波时间序列的预测任务。示例包含：

1.  合成数据

1.  数据集与 DataLoader

1.  Transformer 模型定义

1.  训练与评估

1.  可视化结果

1.  算法优化：学习率调度、Dropout、超参数调整等

```
import numpy as np
import torch
import torch.nn as nn
import torch.optim as optim
from torch.utils.data import Dataset, DataLoader
import matplotlib.pyplot as plt

# 1. 合成时间序列：正弦波 + 高斯噪声
np.random.seed(42)
time = np.arange(0, 1000, 0.1)
series = np.sin(0.02 * time) + 0.3 * np.random.randn(len(time))

# 2. 构建 Dataset：使用前 seq_len 步预测下一步
class TimeSeriesDataset(Dataset):
    def __init__(self, data, seq_len=50):
        xs, ys = [], []
        for i in range(len(data) - seq_len):
            xs.append(data[i:i+seq_len])
            ys.append(data[i+seq_len])
        self.x = torch.tensor(xs, dtype=torch.float32).unsqueeze(-1)   # (N, seq_len, 1)
        self.y = torch.tensor(ys, dtype=torch.float32).unsqueeze(-1)   # (N, 1)
    def __len__(self):
        return len(self.x)
    def __getitem__(self, idx):
        return self.x[idx], self.y[idx]

seq_len = 50
dataset = TimeSeriesDataset(series, seq_len)
train_size = int(0.8 * len(dataset))
train_ds, val_ds = torch.utils.data.random_split(dataset, [train_size, len(dataset)-train_size])
train_loader = DataLoader(train_ds, batch_size=64, shuffle=True)
val_loader   = DataLoader(val_ds, batch_size=64)

# 3. 定义 TransformerTimeSeries 模型
class TransformerTimeSeries(nn.Module):
    def __init__(self, seq_len, d_model=64, nhead=4, num_layers=2, dropout=0.2):
        super().__init__()
        # 输入投影
        self.input_proj = nn.Linear(1, d_model)
        # Positional Encoding（简化版：可学习）
        self.pos_emb = nn.Parameter(torch.randn(1, seq_len, d_model))
        # Transformer Encoder
        encoder_layer = nn.TransformerEncoderLayer(
            d_model=d_model,
            nhead=nhead,
            dim_feedforward=128,
            dropout=dropout,
            activation=&#x27;relu&#x27;
        )
        self.transformer = nn.TransformerEncoder(encoder_layer, num_layers=num_layers)
        # 输出层
        self.fc = nn.Linear(d_model * seq_len, 1)
    def forward(self, x):
        &quot;&quot;&quot;
        x: (batch, seq_len, 1)
        &quot;&quot;&quot;
        # 1) 投影 &amp; 加位置编码
        x = self.input_proj(x) + self.pos_emb  # (B, S, D)
        # 2) 转换为 Transformer 要求的 (S, B, D)
        x = x.permute(1, 0, 2)
        # 3) 编码器
        out = self.transformer(x)              # (S, B, D)
        # 4) 恢复形状并展平
        out = out.permute(1, 0, 2).contiguous().view(x.size(1), -1)  # (B, S*D)
        # 5) 预测
        return self.fc(out)

device = torch.device(&quot;cuda&quot; if torch.cuda.is_available() else &quot;cpu&quot;)
model = TransformerTimeSeries(seq_len=seq_len, d_model=64, nhead=4, num_layers=2, dropout=0.2).to(device)

# 4. 损失、优化器、调度器
criterion = nn.MSELoss()
optimizer = optim.Adam(model.parameters(), lr=1e-3)
scheduler = optim.lr_scheduler.StepLR(optimizer, step_size=10, gamma=0.5)

# 5. 训练与验证
num_epochs = 30
train_losses, val_losses = [], []

for epoch in range(1, num_epochs+1):
    model.train()
    running_loss = 0.0
    for xb, yb in train_loader:
        xb, yb = xb.to(device), yb.to(device)
        optimizer.zero_grad()
        preds = model(xb)
        loss = criterion(preds, yb)
        loss.backward()
        optimizer.step()
        running_loss += loss.item() * xb.size(0)
    train_loss = running_loss / train_size

    model.eval()
    running_vloss = 0.0
    with torch.no_grad():
        for xb, yb in val_loader:
            xb, yb = xb.to(device), yb.to(device)
            preds = model(xb)
            running_vloss += criterion(preds, yb).item() * xb.size(0)
    val_loss = running_vloss / (len(dataset) - train_size)

    train_losses.append(train_loss)
    val_losses.append(val_loss)
    scheduler.step()

    print(f&quot;Epoch {epoch:02d} | Train Loss: {train_loss:.4f} | Val Loss: {val_loss:.4f}&quot;)

# 6. 损失曲线可视化
plt.figure(figsize=(8,4))
plt.plot(range(1,num_epochs+1), train_losses, label=&#x27;Train Loss&#x27;, color=&#x27;magenta&#x27;, linewidth=2)
plt.plot(range(1,num_epochs+1), val_losses,   label=&#x27;Val Loss&#x27;,   color=&#x27;cyan&#x27;,    linewidth=2)
plt.title(&#x27;Loss Curve&#x27;)
plt.xlabel(&#x27;Epoch&#x27;)
plt.ylabel(&#x27;MSE Loss&#x27;)
plt.legend()
plt.grid(True, linestyle=&#x27;--&#x27;, alpha=0.3)
plt.show()

# 7. 样本预测展示
model.eval()
sample_x, sample_y = next(iter(val_loader))
sample_x, sample_y = sample_x.to(device), sample_y.to(device)
with torch.no_grad():
    sample_pred = model(sample_x).cpu().numpy().flatten()
true = sample_y.cpu().numpy().flatten()

plt.figure(figsize=(10,4))
plt.plot(true[:100],    label=&#x27;True&#x27;,      linestyle=&#x27;-&#x27;, linewidth=2)
plt.plot(sample_pred[:100], label=&#x27;Predicted&#x27;, linestyle=&#x27;--&#x27;, linewidth=2)
plt.title(&#x27;Sample Predictions&#x27;)
plt.xlabel(&#x27;Time Step&#x27;)
plt.ylabel(&#x27;Value&#x27;)
plt.legend()
plt.grid(True, linestyle=&#x27;:&#x27;)
plt.show()

# 8. 最终测试误差
print(f&quot;Final Validation MSE: {val_losses[-1]:.4f}&quot;)
```

#### 1. 数据合成和预处理

+   **合成过程**：我们用正弦函数再叠加高斯噪声，得到近似真实世界测量数据的曲线。

+   **滑动窗口**：选取前 seq_len=50 个点来预测第 51 个点，相当于“有记忆”的自回归模型。

#### 2. TransformerTimeSeries 模型结构

+   Input Projection：将单维度输入投影到 d_model=64 空间，使其与 Transformer 的隐藏维度匹配。

+   Positional Encoding：使用可学习的位置向量 self.pos_emb，让模型区分不同时间步。

+   TransformerEncoder：

+   num_layers=2、nhead=4：每层包含 4 个注意力头，堆叠两层。

+   dim_feedforward=128：前馈网络内部维度。

+   dropout=0.2：防止过拟合。

+   **输出层**：将整个序列每一步的输出拼接后，一次性预测下一个值。

#### 3. 训练与优化策略

+   **损失函数**：均方误差（MSELoss），常用于回归任务。

+   **优化器**：Adam(lr=1e-3)，自适应学习率。

+   Learning Rate Scheduler：每 10 个 epoch 学习率缩小一半，帮助模型在后期更稳定收敛。

+   **Early Stopping**（可选）：若验证损失长时间未下降，可提前停止训练。

![](img/630913b05b967fa61edb2aa1d653bfb4.png)

+   训练曲线（magenta）和验证曲线（cyan）在大多数 epoch 均向下，说明模型在不断学习。

+   最终两者趋于平滑，验证损失未大幅上升，说明未严重过拟合。

![](img/70b383582c7cbae160d6e6d484aea03a.png)

+   对比真实值（实线）与预测值（虚线），前 100 个时刻模型能较好地捕捉正弦波形的趋势和噪声分布。

#### 5. 算法优化效果

+   **Dropout**（0.2）显著降低了过拟合，验证误差更平稳；

+   **学习率调度** 让模型在初期快速下降、后期细致优化；

+   **多头注意力**（4 头）能并行捕捉不同滞后关系，比单头效果更好；

+   **层数与宽度**：可在实际数据集上再试验更多层、不同 d_model，平衡效果与计算开销。

大家可以将此模板改造为 NLP、信号处理、金融预测等各种序列任务，并根据数据规模、时序复杂度进一步调整超参数和网络深度。

## 模型分析

### Transformer 模型的优缺点

**优点**

1.  **长程依赖建模**：自注意力机制使模型能够直接关注序列中任意两个时间步之间的关系，而不像 RNN 那样信息需要逐步传递，因而能更准确地捕捉复杂的周期性和趋势变化。

1.  **并行计算**：不同于按时间顺序逐步计算的 RNN，Transformer 可以一次性处理整个序列，训练时更易并行化，显著提升了速度。

1.  **可扩展性强**：通过调整层数、头数、隐藏维度等即可方便地构建更大规模或更小巧的网络，适应不同精度与性能需求。

1.  **通用性高**：同一个框架能轻松迁移到 NLP、时序预测、图像处理等多种任务，只需微调结构或超参数。

**缺点**

1.  **计算与内存开销大**：自注意力计算量随序列长度平方增长，对内存和算力要求较高，长序列或大 batch 时容易成为瓶颈。

1.  **数据需求高**：大规模 Transformer 往往需要大量训练样本才能发挥优势，在小数据集上容易过拟合。

1.  **缺乏局部偏置**：与卷积或循环结构相比，Transformer 默认“全局视野”但对局部短期模式不够敏感，有时需要额外设计（如局部窗口注意力）来补强。

1.  **可解释性弱**：注意力权重虽可视，但整体决策过程仍较难直观理解，不如传统统计模型透明。

### 与相似算法的对比

| 算法 | 优势 | 劣势 |
| --- | --- | --- |
| **Transformer** | • 捕捉全局依赖  • 并行训练加速  • 结构可扩展、迁移方便 | • 计算量随序列长度平方增长  • 对小数据不友好 |
| **LSTM** | • 局部序列建模能力强  • 参数量相对较小  • 对中短期依赖敏感 | • 难并行化、训练速度慢  • 长依赖仍会衰减 |
| **GRU** | • 结构比 LSTM 更简洁，收敛更快  • 参数更少、速度更快 | • 表达能力略逊于 LSTM  • 仍为逐步计算 |
| **TCN** | • 卷积并行计算  • 固定感受野易控  • 对局部模式抓取优秀 | • 长依赖需要深层网络  • 感受野扩展效率不及自注意力 |
| **ARIMA** | • 模型简单、可解释性高  • 对小样本和线性趋势预测稳健 | • 难以捕捉非线性或复杂周期  • 手工调参繁琐 |

### 算法选择建议

+   优选 Transformer 的场景

+   数据量充足（数万以上序列样本），需要捕捉长程或全局模式。

+   对训练速度要求高，且拥有 GPU/TPU 等硬件资源。

+   需统一框架处理多种任务（如同时做分类、回归、生成等）。

+   **可考虑其他算法的场景**

+   **数据稀缺或实时性弱**：样本量小、设备算力受限时，LSTM/GRU 或 TCN 更轻量、易收敛。

+   **侧重线性趋势与解释性**：金融、经济等领域线性成分显著，ARIMA、SARIMAX 等统计模型更直观、易于置信区间评估。

+   **局部周期或短期预测**：当目标主要为短期波动时，卷积（TCN）或浅层 RNN 已可满足需求，无需全局自注意力的高成本。

**总结而言，Tra**nsformer 在大规模**、复杂依赖的预测任务中最具优**势；而在小样本、轻量化或强调可解释性的场景中，传统 RNN、卷积或统计模型往往更实用。