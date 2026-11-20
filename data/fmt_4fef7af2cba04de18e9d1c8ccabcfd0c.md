
我们需要在`/root/autodl-tmp/miniDeepSeek`文件夹内创建一个名为`model.py`的文件，并写入上述代码：

```
cd /root/autodl-tmp/MateConv/model
touch model.py
```

![](img/45f8b83e061a4bbf86093852d2bf40fc.png)

![](img/dbd43b4cc9e8397dc780d4f3cb8ee9e9.png)

写入完成后保存并退出。

## MiniDeepSeek 模型架构基本解释

### 1. `RMSNorm` 类

`RMSNorm`（Root Mean Square Normalization）是一个归一化层，它与常见的层归一化（`LayerNorm`）类似，但是使用了均方根（`RMS`）来计算标准差。该类的实现如下：

+   `__init__`：初始化时接受输入的维度 `dim` 和一个小常数 `eps` 用于防止除零错误。

+   `_norm`：对输入进行 RMS 归一化。

+   `forward`：对输入数据 `x` 进行归一化操作，并通过可训练的 `weight` 参数缩放输出。

### 2. `precompute_pos_cis` 函数

这个函数用于预计算位置编码的复数形式（`pos_cis`），在模型中用于增强位置感知能力。

+   `dim`：表示嵌入空间的维度。

+   `end`：序列的最大长度。

+   `theta`：频率的缩放因子。

+   这个函数生成了一个与输入序列长度和模型维度相对应的复数位置编码。

### 3. `apply_rotary_emb` 函数

该函数应用旋转位置编码（rotary embedding）到查询 `xq` 和键 `xk` 上，以便增强模型的位置信息。

+   `xq`、`xk`：查询和键，分别来自注意力机制的输入。

+   `pos_cis`：预计算的复数位置编码。

+   `apply_rotary_emb` 会对查询和键进行旋转编码，使得相对位置关系能够被更好地建模。

### 4. `Attention` 类

`Attention` 类实现了一个多头自注意力机制（Multi-head Attention）。其中包含：

+   `n_kv_heads`：表示键值对的头数，可以与查询头数不同。

+   `wq`、`wk`、`wv`、`wo`：用于映射输入的线性层，分别用于查询、键、值和输出。

+   `mask`：用于实现因果遮蔽（`causal masking`），防止模型看到未来的词。

+   `forward`：计算注意力分数并应用于值（`value`）。其中使用了旋转位置编码（`apply_rotary_emb`）来增强位置信息。

### 5. `FeedForward` 类

这是标准的前馈神经网络（`FeedForward Network`），包含了两层线性变换和一个激活函数（`silu`，类似于`gelu`）：

+   `forward`：执行输入数据的前向传播，经过线性变换和激活函数。

### 6. `MoEGate` 类

`MoEGate` 实现了一个门控机制，用于选择一个或多个专家（专家模型通常用于大规模任务，能够提高模型的计算效率）。具体步骤如下：

+   `scoring_func`：定义了如何计算门控函数的得分（比如使用 `Softmax`）。

+   `top_k`：表示每个 `token` 会选择哪些 `top_k` 个专家进行处理。

+   `forward`：根据计算的得分，选择专家并返回加权结果。

### 7. `MOEFeedForward` 类

该类结合了 `MoE`（混合专家网络）和前馈神经网络，在输入上应用 `MoE` 门控机制选择不同的专家进行计算：

+   `forward`：在训练时，根据门控机制对输入数据进行专家选择，并加权专家的输出；在推理时，选择最优的专家进行处理。

### 8. `TransformerBlock` 类

`TransformerBlock` 是标准的 `Transformer` 层，包含了多头自注意力和前馈神经网络。具体来说：

+   `attention`：多头自注意力层。

+   `feed_forward`：前馈神经网络，支持普通的前馈网络或 `MoE`（通过 `use_moe` 参数选择）。

+   `forward`：执行该层的前向传播，即先进行注意力计算，然后进行前馈神经网络计算。

### 9. `Transformer` 类

这是模型的主要类，继承自 `PreTrainedModel`：

+   `__init__`：初始化模型，包括词嵌入层、若干个 `TransformerBlock` 层、位置编码、归一化层等。

+   `forward`：执行模型的前向传播，处理输入的 `tokens` 和 `targets`，返回模型的输出。训练时计算交叉熵损失，推理时返回生成的 `logits`。

+   `generate`：生成文本的函数，支持温度采样、Top-k 采样等生成策略。

+   `eval_answer`：在推理模式下，根据输入 `idx` 计算下一个 `token` 的 `logits`。

`MiniDeepSeek`通过集成多种先进的技术，如多头注意力（`Attention`）、旋转位置编码（`Rotary Embedding`）、混合专家网络（`MoE`）等，旨在提升大规模语言模型的计算效率和表现。每个模块的设计都为处理长序列和多任务提供了灵活的功能，特别是 `MoE` 和自定义的门控机制使得模型可以选择性地激活部分计算资源，从而减少计算量。

## 2. MiniDeepSeek 模型配置文件编写

同时，`model.py`中预留了很多模型参数接口，可以统一通过继承一个`transformers`中的`PretrainedConfig`类来设置这些参数。这里我们在`model`文件夹内再创建一个名为`LMConfig.py`的文件，来设置`MateConv`的模型参数。

```
cd /root/autodl-tmp/MateConv/model
touch LMConfig.py
```

并在`LMConfig.py`文件中输入如下内容：

```
from transformers import PretrainedConfig
from typing import List


class LMConfig(PretrainedConfig):
    model_type = "MateConv"

    def __init__(
            self,
            dim: int = 512,
            n_layers: int = 8,
            n_heads: int = 16,
            n_kv_heads: int = 8,
            vocab_size: int = 6400,
            hidden_dim: int = None,
            multiple_of: int = 64,
            norm_eps: float = 1e-5,
            max_seq_len: int = 512,
            dropout: float = 0.0,
            flash_attn: bool = True,
            use_moe: bool = True,
            num_experts_per_tok=2,
            n_routed_experts=4,
            n_shared_experts: bool = True,
            scoring_func='softmax',
            aux_loss_alpha=0.01,
            seq_aux=True,
            norm_topk_prob=True,
            **kwargs,
    ):
        self.dim = dim
        self.n_layers = n_layers
        self.n_heads = n_heads
        self.n_kv_heads = n_kv_heads
        self.vocab_size = vocab_size
        self.hidden_dim = hidden_dim
        self.multiple_of = multiple_of
        self.norm_eps = norm_eps
        self.max_seq_len = max_seq_len
        self.dropout = dropout
        self.flash_attn = flash_attn
        self.use_moe = use_moe
        self.num_experts_per_tok = num_experts_per_tok  # 每个token选择的专家数量
        self.n_routed_experts = n_routed_experts  # 总的专家数量
        self.n_shared_experts = n_shared_experts  # 共享专家
        self.scoring_func = scoring_func  # 评分函数，默认为'softmax'
        self.aux_loss_alpha = aux_loss_alpha  # 辅助损失的alpha参数
        self.seq_aux = seq_aux  # 是否在序列级别上计算辅助损失
        self.norm_topk_prob = norm_topk_prob  # 是否标准化top-k概率
        super().__init__(**kwargs)
```

然后保存并退出：

![](img/b480cd1a9466c6a35cd97484c7538cbf.png)

`LMConfig` 类用于定义和控制模型架构中的一系列关键参数。它继承自 `PretrainedConfig`，允许用户通过配置参数来灵活地调整模型的结构和行为。以下是对 `LMConfig` 中相关参数的解释，以及这些参数如何影响原始模型架构和运行流程。

### `class LMConfig(PretrainedConfig)`

继承自 `PretrainedConfig`，意味着这个配置类是用来定义一个预训练模型的配置，能够被 Hugging Face 的模型管理工具加载和使用。

### `model_type = "miniDeepSeek"`

这是模型的名称，告诉 Hugging Face 框架这个配置是针对 `miniDeepSeek` 模型的。

### `def __init__(...)`

这是类的初始化方法，定义了多个参数，它们控制着 `miniDeepSeek` 模型的不同方面。所有参数都具有默认值，可以在实例化时修改。

#### 参数解释

+   `dim`: `int` = 512: 模型的隐藏层维度，即每层的神经元数量。

+   `n_layers`: `int` = 8: 模型的层数，指定模型包含多少个 `transformer` 层。

+   `n_heads`: `int` = 16: 自注意力机制中的头数，决定了每个 `transformer` 层的并行计算量。

+   `n_kv_heads`: `int` = 8: KV（`Key-Value`）头数，用于处理注意力机制中的键和值。

+   `vocab_size`: `int` = 6400: 词汇表的大小，定义了词嵌入层的输入大小。

+   `hidden_dim`: `int` = `None`: 隐藏层的维度，如果没有指定，则默认是 4 倍的 `dim`。

+   `multiple_of`: `int` = 64: 一个对隐藏层维度的约束，保证该维度是某个数字的倍数。

+   `norm_eps`: `float` = 1e-5: 用于归一化的 `epsilon` 值，防止数值不稳定。

+   `max_seq_len`: `int` = 512: 最大输入序列长度，决定模型能处理的最长文本的长度。

+   `dropout`: `float` = 0.0: 用于防止过拟合的 `dropout` 比例。

+   `flash_attn`: `bool` = `True`: 是否启用 `Flash Attention` 技术，提高注意力机制的效率。

+   `use_moe`: `bool` = `True`: 是否使用 `Mixture of Experts`（`MoE`）机制，允许模型选择不同的专家来处理不同的输入。

+   `num_experts_per_tok`: `int` = 2: 每个 `token` 所选择的专家数量，和 `MoE` 机制相关。

+   `n_routed_experts`: `int` = 4: 总的专家数量，`MoE` 机制下使用的专家数目。

+   `n_shared_experts`: `bool` = `True`: 是否共享专家。

+   `scoring_func`: `str` = 'softmax': 用于 `MoE` 的评分函数，默认为 `softmax`。

+   `aux_loss_alpha`: `float` = 0.01: 辅助损失的权重因子，用于 `MoE` 的训练。

+   `seq_aux`: `bool` = `True`: 是否计算序列级别的辅助损失。

+   `norm_topk_prob`: `bool` = `True`: 是否标准化 `top-k` 概率，通常与 `MoE` 机制一起使用。

### `super().__init__(**kwargs)`

这行代码调用父类 `PretrainedConfig` 的构造函数，并将额外的参数传递给它，确保配置类与 Hugging Face 框架兼容。

`LMConfig` 类提供了对 `miniDeepSeek` 模型各项超参数的控制，涵盖了模型结构（如维度、层数、头数等）、优化和训练设置（如 `dropout`、`MoE` 配置、辅助损失等）。这些参数允许在创建模型时进行灵活配置，使得该模型可以根据不同的任务需求进行调整。

### 数据读取与脚本

同时，我们需要在`model`文件夹内创建一个名为`dataset.py`的脚本，用于在训练时读取数据：
