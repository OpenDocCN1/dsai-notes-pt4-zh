
# ChatGLM-6B模型结构组件源码阅读

> 来源：`https://geekbang.feishu.cn/docx/ZSomdCVDwoE4qRxgZ2Gc6txcnaf`

> 👨🏻 作者：余俊晖

> 📃 作者介绍：一名NLP炼丹师，NLP算法竞赛TOP选手

> 🛖 👸号：大模型自然语言处理

> ⏰ 收录时间：2024-05-28 17:09:42

> 🔗 原文链接：`https://mp.weixin.qq.com/s?__biz=Mzg4NjI0NDg0Ng==&mid=2247484096&idx=1&sn=44fb68e4147c3e775d9dc57f0700e07f&chksm=cf9dd641f8ea5f57f0d6b5743038dda08bec6dbc7b9c81c7635945e6ed78bef6c083ba079e5d#rd`

![](img/191a3508a299a253178cee3e9124f74c.png)

关注作者👸号，获取更多AI知识

## 一、前言

本文将介绍ChatGLM-6B的模型结构组件源码。

代练链接：`https://huggingface.co/THUDM/chatglm-6b/blob/main/modeling_chatglm.py`

## 二、激活函数

```
@torch.jit.script
def gelu_impl(x):
    """OpenAI's gelu implementation."""
    return 0.5 * x * (1.0 + torch.tanh(0.7978845608028654 * x *
                                       (1.0 + 0.044715 * x * x)))


def gelu(x):
    return gelu_impl(x)
```

## 三、位置编码

### 3.1、RoPE原理简介

ChatGLM-6B的位置编码采用的旋转位置编码（详细推导过程见原文：ROPE），简单来说其目的就是构建一个包含相对位置信息的Attention矩阵，其公式如下：

式中，  、  分别表示注意力机制中的`query`和`key`，  、  分别表示两个位置，  表示位置`i`处处理的矩阵，其中  的形式为：

原作者提到，由于  非常稀疏，直接用矩阵乘法来实现会很浪费算力，推荐通过下述方式来实现RoPE：

### 3.2、ChatGLM-6B中RoPE代码实现

这里直接上代码阅读

```
class RotaryEmbedding(torch.nn.Module):
    def __init__(self, dim, base=10000, precision=torch.half, learnable=False):
        super().__init__()
        inv_freq = 1. / (base ** (torch.arange(0, dim, 2).float() / dim))
        inv_freq = inv_freq.half()
        self.learnable = learnable
        if learnable:
            self.inv_freq = torch.nn.Parameter(inv_freq)
            self.max_seq_len_cached = None
        else:
            self.register_buffer('inv_freq', inv_freq)
            self.max_seq_len_cached = None
            self.cos_cached = None
            self.sin_cached = None
        self.precision = precision

    def _load_from_state_dict(self, state_dict, prefix, local_metadata, strict, missing_keys, unexpected_keys,
                              error_msgs):
        pass

    def forward(self, x, seq_dim=1, seq_len=None):
        if seq_len is None:
            seq_len = x.shape[seq_dim]
        if self.max_seq_len_cached is None or (seq_len > self.max_seq_len_cached):
            self.max_seq_len_cached = None if self.learnable else seq_len
            # 在计算旋转嵌入之前，根据当前的嵌入维度和基数计算频率因子 inv_freq。将其转换为半精度数据类型（如果指定的 precision 为 bfloat16，则转换为单精度）。
            t = torch.arange(seq_len, device=x.device, dtype=self.inv_freq.dtype)
            # 使用 爱因斯坦求和函数 einsum 将 t 和 inv_freq 相乘，得到频率矩阵 freqs。
            freqs = torch.einsum('i,j->ij', t, self.inv_freq)
            # 通过在频率矩阵 freqs 中进行重复和拼接操作，生成旋转嵌入矩阵 emb，其维度为 [seq_len, 2 * dim]。
            emb = torch.cat((freqs, freqs), dim=-1).to(x.device)
            if self.precision == torch.bfloat16:
                emb = emb.float()

            # 将旋转嵌入矩阵 emb 分别进行余弦和正弦运算。
            cos_cached = emb.cos()[:, None, :]
            sin_cached = emb.sin()[:, None, :]
            if self.precision == torch.bfloat16:
                cos_cached = cos_cached.bfloat16()
                sin_cached = sin_cached.bfloat16()
            if self.learnable:
                return cos_cached, sin_cached
            self.cos_cached, self.sin_cached = cos_cached, sin_cached
        # 按照序列长度截取
        return self.cos_cached[:seq_len, ...], self.sin_cached[:seq_len, ...]
```

## 四、注意力层

### 4.1、2D位置编码

ChatGLM-6B代码中这一层采用的位置编码是GLM的中提出的2D位置编码，详细原理见原文：GLM: General Language Model Pretraining with Autoregressive Blank Infilling，其原理图如下图：

![](img/fec995f18bfbe3c731dc2bb3addc8776.png)

输入的序列是  ，片段  和片段  被随机MASK，原始的输入序列则变为  ，如上图(a)和(b)所示。将三个片段拼接得到模型的输入 ，模型的输出则是被遮蔽掉的片段，如上图(c)所示。这里使用了2种位置编码：第一种编码为整个输入嵌入位置信息，能够表示MASK片段在原始输入中的位置;第二种位置编码则是为MASK片段内的tokens输入位置信息。

### 4.2、注意力机制

ChatGLM-6B相比标准的自注意力机制在`Q`和`K`中注入了RoPE位置信息。

+   标准自注意力机制 `attention_fn`：
