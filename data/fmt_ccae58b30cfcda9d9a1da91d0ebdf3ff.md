
# Transformer 模型实现与配置

## Transformer 类定义

```
class Transformer(PreTrainedModel):
    config_class = LMConfig
    last_loss: Optional[torch.Tensor]

    def __init__(self, params: LMConfig = None):
        """
        初始化 Transformer 模型，该模型基于自回归语言模型架构构建。

        参数：
        - params: LMConfig 实例，定义了模型的各种超参数。如果未提供，将使用默认配置。

        模型主要组成部分：
        - Token 嵌入层：将输入的 token 转换为高维嵌入。
        - TransformerBlock 层：由多个 TransformerBlock 组成的堆叠层，用于提取深度特征。
        - RMSNorm：归一化层，用于稳定训练。
        - 输出层：线性层，将高维特征转换为词汇表大小的输出。
        - 预计算的位置信息：预先计算位置嵌入用于位置编码。
        """
        super().__init__(params)
        if not params:
            params = LMConfig()
        self.params = params
        self.vocab_size = params.vocab_size  # 词汇表大小
        self.n_layers = params.n_layers  # TransformerBlock 层的数量

        self.tok_embeddings = nn.Embedding(params.vocab_size, params.dim)  # 词嵌入层
        self.dropout = nn.Dropout(params.dropout)  # dropout 用于正则化
        self.layers = torch.nn.ModuleList()  # TransformerBlock 层列表
        for layer_id in range(self.n_layers):
            self.layers.append(TransformerBlock(layer_id, params))
        self.norm = RMSNorm(params.dim, eps=params.norm_eps)  # 最后的归一化层
        self.output = nn.Linear(params.dim, params.vocab_size, bias=False)  # 输出层
        self.tok_embeddings.weight = self.output.weight  # 共享词嵌入和输出层的权重
        pos_cis = precompute_pos_cis(self.params.dim // self.params.n_heads, self.params.max_seq_len)  # 预计算的位置信息
        self.register_buffer("pos_cis", pos_cis, persistent=False)

        # 初始化权重
        self.apply(self._init_weights)

        # 对特定层的权重进行特殊初始化
        for pn, p in self.named_parameters():
            if pn.endswith('w3.weight') or pn.endswith('wo.weight'):
                torch.nn.init.normal_(p, mean=0.0, std=0.02 / math.sqrt(2 * params.n_layers))

        self.last_loss = None  # 保存最后计算的损失值
        self.OUT = CausalLMOutputWithPast()  # 自回归输出格式
        self._no_split_modules = [name for name, _ in self.named_modules()]  # 模型分割相关配置

    def _init_weights(self, module):
        """
        初始化权重：对线性层和嵌入层使用正态分布初始化，对 bias 使用零初始化。
        """
        if isinstance(module, nn.Linear):
            torch.nn.init.normal_(module.weight, mean=0.0, std=0.02)
            if module.bias is not None:
                torch.nn.init.zeros_(module.bias)
        elif isinstance(module, nn.Embedding):
            torch.nn.init.normal_(module.weight, mean=0.0, std=0.02)

    def forward(self, tokens: Optional[torch.Tensor] = None, targets: Optional[torch.Tensor] = None,
                kv_cache=False, **keyargs):
        """
        Transformer 的前向传播函数。

        参数：
        - tokens: 输入的 token 序列。
        - targets: 用于计算交叉熵损失的目标序列（默认为 None）。
        - kv_cache: 是否启用键值缓存以加速推理。
        - keyargs: 其他可选参数，如 'input_ids' 和 'attention_mask'。

        返回：
        - 输出 logits 和损失。
        """
        current_idx = 0
        if 'input_ids' in keyargs:
            tokens = keyargs['input_ids']
        if 'attention_mask' in keyargs:
            targets = keyargs['attention_mask']
        if 'current_idx' in keyargs:
            current_idx = int(keyargs['current_idx'])

        _bsz, seqlen = tokens.shape
        h = self.tok_embeddings(tokens)  # 将 token 转换为嵌入
        h = self.dropout(h)  # dropout 正则化
        pos_cis = self.pos_cis[current_idx:current_idx + seqlen]  # 获取位置嵌入

        # 通过每个 TransformerBlock 层
        for idx, layer in enumerate(self.layers):
            h = layer(h, pos_cis, kv_cache)

        h = self.norm(h)  # 通过归一化层

        # 如果有 targets，则计算交叉熵损失
        if targets is not None:
            logits = self.output(h)
            self.last_loss = F.cross_entropy(logits.view(-1, logits.size(-1)), targets.view(-1), ignore_index=-1)
        else:
            logits = self.output(h[:, [-1], :])  # 只取最后一个 token 的输出
            self.last_loss = None

        # 输出 logits 和损失
        self.OUT.__setitem__('logits', logits)
        self.OUT.__setitem__('last_loss', self.last_loss)
        return self.OUT

    @torch.inference_mode()
    def generate(self, idx, eos, max_new_tokens, temperature=0.7, top_k=8, stream=True, rp=1., kv_cache=True):
        """
        自回归生成函数，用于生成文本。

        参数：
        - idx: 输入的初始 token 序列。
        - eos: 结束符 token，当生成结束符时停止生成。
        - max_new_tokens: 生成的最大 token 数量。
        - temperature: 温度系数，用于控制生成的多样性。
        - top_k: top-k 采样，用于从前 k 个概率最高的 token 中采样。
        - stream: 是否流式输出生成的 token。
        - rp: 重复惩罚，用于避免生成重复的 token。
        - kv_cache: 是否使用键值缓存以加速生成过程。

        返回：
        - 生成的 token 序列（或通过 yield 流式输出）。
        """
        index = idx.shape[1]
        init_inference = True
        while idx.shape[1] < max_new_tokens - 1:
            if init_inference or not kv_cache:
                inference_res, init_inference = self(idx, kv_cache=kv_cache), False
            else:
                inference_res = self(idx[:, -1:], kv_cache=kv_cache, current_idx=idx.shape[1] - 1)

            logits = inference_res.logits
            logits = logits[:, -1, :]

            # 应用重复惩罚
            for token in set(idx.tolist()[0]):
                logits[:, token] /= rp

            # 温度调节和 top-k 采样
            if temperature == 0.0:
                _, idx_next = torch.topk(logits, k=1, dim=-1)
            else:
                logits = logits / temperature
                if top_k is not None:
                    v, _ = torch.topk(logits, min(top_k, logits.size(-1)))
                    logits[logits < v[:, [-1]]] = -float('Inf')

                probs = F.softmax(logits, dim=-1)
                idx_next = torch.multinomial(probs, num_samples=1, generator=None)

            if idx_next == eos:  # 生成结束符时停止生成
                break

            idx = torch.cat((idx, idx_next), dim=1)
            if stream:
                yield idx[:, index:]  # 流式输出

        if not stream:
            yield idx[:, index:]

    @torch.inference_mode()
    def eval_answer(self, idx):
        """
        评估函数，用于计算模型在给定输入下的输出 logits。

        参数：
        - idx: 输入 token 序列。

        返回：
        - 最后一个 token 的 logits 值。
        """
        idx_cond = idx if idx.size(1) <= self.params.max_seq_len else idx[:, -self.params.max_seq_len:]
        inference_res = self(idx_cond)
        logits = inference_res.logits
        logits = logits[:, -1, :]
        return logits
```

## 7. 配置文件的搭建

```
from transformers import PretrainedConfig
from typing import List

class LMConfig(PretrainedConfig):
    model_type = "MateConv-Mini"

    def __init__(self, dim: int = 512, n_layers: int = 8,
                 n_heads: int = 16, n_kv_heads: int = 8,
                 vocab_size: int = 6400, hidden_dim: int = None,
                 multiple_of: int = 64, norm_eps: float = 1e-5,
                 max_seq_len: int = 512, dropout: float = 0.0,
                 flash_attn: bool = True,
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
        super().__init__(**kwargs)
```

```
# from .LMConfig import LMConfig
```

📍更多大模型技术内容学习

扫码添加助理英英，回复“大模型”，了解更多大模型技术详情哦👇

![](img/8d6d33eb5032aac21174aa4f41fe2576.png)

此外，扫码回复“入群”，即可加入大模型技术社群：海量硬核独家技术干货内容+无门槛技术交流！
