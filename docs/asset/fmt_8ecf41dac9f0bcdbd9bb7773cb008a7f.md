
# 稀疏专家系统与 MoE 路由机制

## 结论与负载均衡策略

结论 `TC` 在语义完整性上占优但易遭遇专家能力两极化；`EC` 在负载均衡上占优但要承受潜在可能的语义丢失风险，两种模式代表了稀疏专家系统在信息完整性 vs 负载均衡上的典型权衡抉择，根据近期的研究也有解决这个权衡问题的思路：

| 策略名 | 核心思路 | 说明 |
|------|----------|------|
| 辅助负载均衡 | 在训练 `loss` 中加入一个正则项，鼓励专家之间接收的 `token` 量、路由概率趋于均匀分布，避免少数专家处理掉绝大多数 `token` | 这是最早也是最经典的方法，在 `Switch Transformer` 以及后续很多 `MoE` 实现中被采用。 |
| 容量控制 + `expert capacity` + `overflow` 机制 | 给每个专家设定一个容量上限，超过后不再接 `token`、转为 `fallback` 路径(或 `dropout`、备用 `expert`)；避免单个专家过载，也避免忽略“冷门” `expert` | 多 `MoE` 系统建议通过 `capacity factor` + `expert capacity` 控制单专家负载以及治理 `overflow` 情况。 |
| 动态、无辅助损失的负载均衡 | 避免引入额外训练梯度，通过对每个专家加 `bias`（基于过去负载统计）动态调整路由分数，从而平衡专家负载，无需 `aux-loss` 也可稳定路由分布 | 最近研究 `Loss-Free Balancing for MoE` 提出该方式，显示比传统 `aux-loss` 更稳定，不破坏原模型优化目标。 |
| 改善路由器、相似度保持路由 | 设计路由器，使相似语义的 `token` → 相似分配专家、在专家间分布均匀；减少重复路由和专家负载偏移 | 提升收敛速度和负载均衡效果。 |
| 改进专家结构、路由机制 | 通过改变专家参数化例如用正交基、`basis` 或用更稳定、可解释的路由评分而非简单 `linear logits`，提升路由稳定性以及专家利用率 | 最新工作不仅减轻传统路由不稳定和专家闲置，还自然实现更均匀专家负载。 |
| 混合共享 + 路由专家池 | 将部分专家设为共享专家，所有 `token` 都激活其余为路由专家，共享专家保证即使路由阶段极不均衡也能覆盖所有 `token`，减少 `token dropout` 与语义丢失 | 工程实践中如 `DeepSeekMoE` 使用该办法以折中保持语义覆盖 + 专家特长训练。 |

MoE 研究中有一个值得深思的事实。部分研究表明，在某些场景下复杂的智能路由器比如 `Top-K` 路由并非绝对必要。存在哈希路由等非学习式方法，这类方法通过固定哈希函数将输入映射到专家，天然具备较好或易于实现的负载均衡与低开销实现，尽管在语义灵活性和精细化专家分工上通常不如可学习 `Top-K`，但在若干基准和工程场景中，哈希路由仍能展现出相当竞争力，说明 `MoE` 的架构表现能力或许在很大程度上是源自稀疏激活 + 参数容量扩张。

## 哈希路由与 LSH

![](img/c5fc25a38bde161ed874b6da93f172b9.png)

图 5.3 哈希路由

`LSH` 为例，采用固定的、非训练的哈希函数。每个哈希函数通过将输入 `Token` 嵌入 `x` 投影到由随机向量 `ai` 和随机偏置 `bi` 定义的平面上，再通过桶宽度 `ϵ` （间接控制每个桶的 `token` 容量）进行量化，从而将 `x` 映射到一个索引值为 `i` 的 `hi(x)` 整数哈希桶。

```
hi(x) = ⌊aiᵀx + biϵ⌋
hi(x) = ⌊ϵaiᵀx + bi⌋
```

这里的 `DD` 是复合哈希函数即随机投影方向的数量。这种方法不通过梯度优化哈希参数，但路由结果会因随训练演化的 `xx`（`Token Embedding`）而动态改变，`LSH` 概率性地实现了负载均衡，并且由于其局部敏感性，能够保留弱局部性——即相似 `Token` 更可能落入同一哈希桶。因此，`LSH` 算是一种“弱语义”非学习路由。

桶宽度 `ϵϵ` 与上述量化步长对应，用于把连续投影值分桶；桶越宽，同一桶内聚集的向量越多。

> 说明： 上文带 `ϵϵ` 的公式刻画的是随机投影 + 标量量化一类 `LSH`；下文示例代码则采用随机超平面符号哈希（对投影结果取正负号再编码为比特串），二者同属 `LSH` 家族但实现细节不同，阅读时请勿把公式参数与代码逐行一一对应。

## 基于 LSH 路由机制的简易 MoE 实现

```
import torch
import torch.nn as nn

# 简单字符级 tokenizer
class CharTokenizer:
    def __init__(self):
        self.vocab = {}
        self.inv_vocab = {}

    def build_vocab(self, texts):
        chars = set("".join(texts))
        self.vocab = {c: i for i, c in enumerate(sorted(chars))}
        self.inv_vocab = {i: c for c, i in self.vocab.items()}

    def encode(self, text):
        return [self.vocab[c] for c in text]

    def decode(self, ids):
        return "".join([self.inv_vocab[i] for i in ids])


# Expert FFN
class Expert(nn.Module):
    def __init__(self, dim):
        super().__init__()
        self.ffn = nn.Sequential(
            nn.Linear(dim, dim * 4),
            nn.ReLU(),
            nn.Linear(dim * 4, dim)
        )

    def forward(self, x):
        return self.ffn(x)


# LSH Router
class LSHRouter(nn.Module):
    def __init__(self, d_model, num_experts, n_hashes=8):
        super().__init__()
        self.num_experts = num_experts
        self.n_hashes = n_hashes
        self.register_buffer("random_vectors", torch.randn(n_hashes, d_model))

    def forward(self, x):
        projections = x @ self.random_vectors.T
        signs = (projections > 0).long()
        hashes = signs @ (1 << torch.arange(self.n_hashes, device=x.device))
        expert_ids = hashes % self.num_experts
        return hashes, expert_ids


# LSH-MoE
class LSH_MoE_Text(nn.Module):
    def __init__(self, dim, num_experts, n_hashes=8, vocab_size=None):
        super().__init__()
        self.embedding = nn.Embedding(vocab_size, dim)  # embedding 层
        self.dim = dim
        self.num_experts = num_experts
        self.router = LSHRouter(dim, num_experts, n_hashes)
        self.experts = nn.ModuleList([Expert(dim) for _ in range(num_experts)])

    def forward(self, token_lists, verbose=True):
        """
        token_lists: list of LongTensor，每条 tensor 是一条文本 token ID
        """
        lengths = [t.size(0) for t in token_lists]
        total_tokens = sum(lengths)
        x_flat = torch.cat(token_lists, dim=0)  # [total_tokens]
        x_flat = self.embedding(x_flat)         # [total_tokens, D]

        hashes, expert_ids = self.router(x_flat)
        out_flat = torch.zeros_like(x_flat)

        expert_load = torch.zeros(self.num_experts, dtype=torch.long, device=x_flat.device)
        for e_id, expert in enumerate(self.experts):
            mask = (expert_ids == e_id).float().unsqueeze(1)
            n_tokens = int(mask.sum().item())
            expert_load[e_id] = n_tokens
            if n_tokens > 0:
                out_flat += expert(x_flat * mask) * mask

        # 拆回原句子
        outputs = []
        start = 0
        for l in lengths:
            outputs.append(out_flat[start:start + l])
            start += l
        if verbose:
            print("\n========== LSH-MoE Token 哈希映射 ==========")
            start = 0
            for idx, l in enumerate(lengths):
                for j in range(l):
                    token_idx = start + j
                    print(f"Sentence {idx}, Char {j}: Hash={hashes[token_idx].item()} -> Expert {expert_ids[token_idx].item()}")
                start += l
            print("\n========== LSH-MoE 专家负载统计 ==========")
            for e in range(self.num_experts):
                print(f"Expert {e}: {expert_load[e].item()} tokens")
            print("------------------------------------------------\n")
        return outputs


# 测试
if __name__ == "__main__":
    sentences = ["你好世界", "今天天气很好"]
    tokenizer = CharTokenizer()
    tokenizer.build_vocab(sentences)
    token_lists = [torch.tensor(tokenizer.encode(s), dtype=torch.long) for s in sentences]
    dim, num_experts = 16, 5    # 每个 token 的 embedding 维度，专家数量
    moe_text = LSH_MoE_Text(dim=dim, num_experts=num_experts, vocab_size=len(tokenizer.vocab))
    outputs = moe_text(token_lists)
    for i, out in enumerate(outputs):
        print(f"Sentence {i} 输出shape: {out.shape}")
```

### 输入

> `dim`, `num_experts` = 16, 5

### 输出

> 每个句子的 `token` 哈希映射以及 `LSH_MoE` 专家负载统计。

输出结果会随着 `embedding` 层动态变化。

## MoE 路由机制对比

### 5.1.2 MoE 变体

在混合专家模型 `MoE` 里，每个专家就像一位“老师”，负责处理输入的一部分（`token`）。然而在实际训练中，有两个常见问题会阻碍专家真正形成“专业领域”：

1. 知识混合：分配给某个专家的 `token` 可能多样化，即涵盖多种不同类型的知识。就像一位老师被要求同时教数学、历史和美术，他很难在自己的课堂上把每门课都讲得透彻。
2. 知识重复：不同专家处理的 `token` 可能存在重叠的知识需求。结果就像几位老师都在准备相同的教材，每个人都在重复劳动，无法凸显各自的“专业特长”，也导致专家之间缺乏明确的“专业分工”。

这两个问题叠加起来，可能会限制 `MoE` 模型发挥其理论上最大的能力，让专家难以真正做到“各司其职”，理解这些限制，可以启发我们设计更智能的路由策略，让每位专家专注于自己的“领域”，从而提升模型整体表现。在接下来的内容中，我们将介绍两种 `MoE` 变体，这些方法正是为了解决知识杂乱和重复问题而提出的。

#### DeepSpeed-MoE

`DeepSpeed-MoE` 在降低 `MoE` 训练成本方面的贡献涵盖从模型结构、训练系统到推理加速的全栈设计，使得稀疏专家模型比同质量的稠密模型在训练成本、部署效率和实时性上都更具优势，从而让超大规模 `LLM` 向更高效、更可落地的方向发展。

- 参数效率提升：`DeepSpeed-MoE` 在模型结构上提出了 `PR-MoE` 以及其蒸馏压缩版本 `MoS`，`PR-MoE` 用固定 `MLP` + “专家残差纠错”减少参数与通信，再用“金字塔式专家数量”把专家集中在深层，从而达到更高的参数效率；`MoS` 则通过分阶段蒸馏进一步压缩模型，让 `MoE` 在保持性能的同时显著加速推理。

- 蒸馏压缩加速推理：`MoS` 通过“分阶段知识蒸馏”将 `PR-MoE` 进一步压缩，用较浅的稀疏学生模型替代原模型以提升推理速度。由于直接减少层数会导致模型能力下降，而从头到尾都用教师信号训练也会导致学生欠拟合，`MoS` 采用“两阶段”方式：训练前期使用蒸馏稳定学习教师分布，后期关闭蒸馏、只优化语言模型损失，让学生模型具备自主泛化能力。实践中，`MoS` 可以将模型尺寸再缩小 3.7 倍，同时推理速度比质量相当的稠密模型还要更快。

- 系统优化升级：系统层重写 `MoE` 并行与通信方式，使 `MoE` 在真实大规模训练和推理中更快、更稳定：
  - 由于不同层的专家数量不一致，它采用专家并行、专家切片、数据并行和张量切片的灵活组合，保证每层都能获得最合适的并行方式。
  - 这种自适应并行让 `MoE` 可以在上百块 `GPU` 上稳定扩展，并避免负载不均与显存浪费。
  - 在通信方面，`DeepSpeed-MoE` 通过张量切片将 `All-to-All` 的复杂度从 `O(p)` 降到 `O(p/L)`，并使用分层 `All-to-All` 来降低跨节点延迟。最核心的优化是将 `MoE` 的稀疏重排改写为显式数据布局转换，使关键内核延迟下降 6 倍以上，从而显著提升推理速度。

> 分层 `All-to-All` 指的是在 `MoE` 中，把原本一次性、全局、所有 `GPU` 之间的 `token` 通信，按照硬件拓扑拆成逐级执行的多层通信：先在同一节点、同一机器内部完成高速的 `All-to-All`，再跨不同节点、不同机器之间进行必要的数据交换，从而显著减少跨机器通信量。

#### Switch Transformer

`Switch Transformer` 致力于在不显著增加每个样本 `FLOPs` 的前提下极大扩展模型参数量。其核心策略是把标准 `Transformer` 中的密集前馈网络替换为稀疏激活的专家集合，使不同输入动态激活不同专家，从而扩展参数容量而不显著增加计算成本，其中有辅助损失函数添加解决负载均衡的问题、以及中间传输过程使用低精度、关键路由决策等使用中精度计算。

- 路由器计算：路由器对 `token` 的表示 `x` 计算 `logits`，通常对 `logits` 做 `softmax` 可以得到每个专家的概率分布 `pi(x)`。

- 实际路由决策：采用 `Top-1` 策略，每个 `token` 被分配到得分最高的单个专家执行 `FFN`。`softmax` 得到的概率主要用于统计、辅助损失，而实际的前向计算只使用被选定的专家即稀疏激活，相较于 `Top-k`，`Top-1` 路由显著简化实现、减少跨设备通信并降低专家同时被调用的计算量，从而提高硬件、通信效率。
