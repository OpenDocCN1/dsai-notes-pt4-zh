
```python
class MixerModel(nn.Module):
    def __init__(
        self,
        d_model: int,
        n_layer: int,
        vocab_size: int,
        ssm_cfg=None,
        norm_epsilon: float = 1e-5,
        rms_norm: bool = False,
        initializer_cfg=None,
        fused_add_norm=False,
        residual_in_fp32=False,
        device=None,
        dtype=None,
    ) -> None:
        factory_kwargs = {"device": device, "dtype": dtype}
        super().__init__()
        self.residual_in_fp32 = residual_in_fp32

        self.embedding = nn.Embedding(vocab_size, d_model, **factory_kwargs)

        # We change the order of residual and layer norm:
        # Instead of LN -> Attn / MLP -> Add, we do:
        # Add -> LN -> Attn / MLP / Mixer, returning both the residual branch (output of Add) and
        # the main branch (output of MLP / Mixer). The model definition is unchanged.
        # This is for performance reason: we can fuse add + layer_norm.
        self.fused_add_norm = fused_add_norm
        if self.fused_add_norm:
            if layer_norm_fn is None or rms_norm_fn is None:
                raise ImportError("Failed to import Triton LayerNorm / RMSNorm kernels")

        self.layers = nn.ModuleList(
            [
                create_block(
                    d_model,
                    ssm_cfg=ssm_cfg,
                    norm_epsilon=norm_epsilon,
                    rms_norm=rms_norm,
                    residual_in_fp32=residual_in_fp32,
                    fused_add_norm=fused_add_norm,
                    layer_idx=i,
                    **factory_kwargs,
                )
                for i in range(n_layer)
            ]
        )

        self.norm_f = (nn.LayerNorm if not rms_norm else RMSNorm)(
            d_model, eps=norm_epsilon, **factory_kwargs
        )

        self.apply(
            partial(
                _init_weights,
                n_layer=n_layer,
                **(initializer_cfg if initializer_cfg is not None else {}),
            ))
```

官方也提供了多个版本的 checkpoint，下面以 `mamba-2.8b` 为例，实现其端到端的推理过程

```python
import time
import json
import torch
import torch.nn.functional as F

from einops import rearrange
from transformers import AutoTokenizer, AutoModelForCausalLM
from mamba_ssm.models.mixer_seq_simple import MambaLMHeadModel

model_name = "state-spaces/mamba-2.8b"
prompt = "I have a dream and I"
promptlen = 100
genlen = 100
temperature = 1.0
topk = 1
topp = 1.0
minp = 0.0
repetition_penalty = 1.1
batch =1

repeats = 3
device = "cuda:0"
dtype = torch.float16

print(f"Loading model {model_name}")
tokenizer = AutoTokenizer.from_pretrained("EleutherAI/gpt-neox-20b")
model = MambaLMHeadModel.from_pretrained(model_name, device=device, dtype=dtype)
model.eval()
print(f"Number of parameters: {sum(p.numel() for p in model.parameters() if p.requires_grad)}")

torch.random.manual_seed(0)
tokens = tokenizer(prompt, return_tensors="pt")
input_ids = tokens.input_ids.to(device=device)
attn_mask = tokens.attention_mask.to(device=device)
max_length = input_ids.shape[1] + genlen

fn = lambda: model.generate(
        input_ids=input_ids,
        max_length=max_length,
        cg=True,
        return_dict_in_generate=True,
        output_scores=True,
        enable_timing=False,
        temperature=temperature,
        top_k=topk,
        top_p=topp,
        min_p=minp,
        repetition_penalty=repetition_penalty,
    )
out = fn()
 
print(tokenizer.batch_decode(out.sequences.tolist()))
torch.cuda.synchronize()
start = time.time()
for _ in range(repeats):
    fn()
torch.cuda.synchronize()
print(f"Prompt length: {len(input_ids[0])}, generation length: {len(out.sequences[0]) - len(input_ids[0])}")
print(f"{model_name} prompt processing + decoding time: {(time.time() - start) / repeats * 1000:.0f}ms")
```

结果如下所示：

```
Loading model state-spaces/mamba-2.8b
Number of parameters: 2768345600
['I have a dream and I want to make it come true.\n\n"The first thing is that we need more people in the community who are willing to help us out, because there\'s not enough of them," he said. "We\'re going through this process now with our new building."\n\nHe added: "It would be great if they could get some volunteers from the local business owners or maybe even residents themselves so that when we do go into construction mode on the project... then at least you know somebody will\']
Prompt length: 6, generation length: 100
state-spaces/mamba-2.8b prompt processing + decoding time: 569ms
```

## 三、语言模型之外的 Mamba

除了在语言模型上的应用外，`Mamba` 还在图像和时间序列模型上有所应用，下面分别介绍相关工作。

### 3.1 图像中的 Mamba

处理图像本质上比处理文本要复杂得多。因为图像不仅仅是像素的序列；它们还包含复杂的模式，变化的空间关系，以及理解整体环境的需要。这种复杂性使得视觉数据的有效处理成为一项具有挑战性的任务，特别是在规模和高分辨率下。

`Mamba` 块是 `Vim` 的一个关键特性，通过使用位置嵌入标记图像序列，并使用双向状态空间模型压缩视觉表示，`Vision Mamba` 可以有效地捕获图像的全局上下文。这种方法解决了可视数据固有的位置敏感性，这是传统 `Transformer` 模型经常遇到的一个关键问题，特别是在更高分辨率下。

![](img/91dc4baae72eb0906420fffeb750d641.png)

`Vim` 模型首先将输入图像划分为小块，然后将小块投影到 token 中。这些 token 随后被输入到 `Vim` 编码器中。对于像 ImageNet 分类这样的任务，在 token 序列中添加了一个额外的可学习分类标记，与用于文本序列建模的 `Mamba` 模型不同，`Vim` 编码器在正向和反向两个方向上处理 token 序列。

`ViM` 的突出特点之一是其双向处理能力，类似于 `LSTM` 的工作原理。与许多以单向方式处理数据的模型不同，`ViM` 的编码器可以向前和向后处理 token。双向模型可以更丰富地理解图像上下文，这是准确图像分类和分割的关键因素。

一旦 token 被卷积并激活，算法就会执行额外的线性变换并应用 `softplus` 函数，以确保输出值保持正值。这些转换为 SSM 序列建模功能准备 token。

在 SSM 操作之后，该算法应用门控机制，通过 SSM 输出与 `SiLU` 激活的前向和后向序列的元素相乘来调制信息流。这种门控机制可能旨在控制每个方向处理的贡献。

![](img/e0ad96cb83c85f3268747b13686f6745.png)

最后一步是残差连接，将原始输入序列添加到门控输出中，这有助于保留前几层的信息并解决梯度消失问题。整个过程的输出是一个新的标记序列，它可能经过了复杂的转换，捕捉到了序列两个方向上的复杂依赖关系。最后，算法会返回这个经过转换的 token 序列。

在 ImageNet 分类、COCO 物体检测和 ADE20K 语义分割等基准测试中，`ViM` 不仅表现更好，而且效率更高。例如，在处理高分辨率图像（1248 × 1248）时，`ViM` 比 `DEIT` 快 2.8 倍，使用的 GPU 内存减少了 86%。考虑到处理高分辨率图像时通常面临的内存限制，这无疑是一个重大改进。

### 3.2 时间序列中的 Mamba

通过开始阶段的研究和讨论，可以看到，SSM 本来就是用来处理时间序列信号的，因此 `Mamba` 也可以直接用来处理时间序列的任务，以下是其简单的实现：

```python
class TSModel(nn.Module):
def __init__(self, d_model, d_state, d_conv, expand, forecast, lookback, dropout=0.5, device="cpu"):
    super(TSModel,self).__init__()
    self.device=device
    self.mamba = Mamba(d_model=d_model, d_state=d_state,d_conv=d_conv, expand=expand).to(device)
    self.d1_nn = nn.Dropout(p=dropout).to(device)
    self.fc1=nn.Linear(in_features=lookback*d_model, out_features=forecast).to(device)

def forward(self, input):
    bs=input.shape[0]
    h_out = self.mamba(input)
    h_out = rearrange(h_out, 'b l c -> b (l c)')
    h_out = self.d1_nn(h_out)

    out = self.fc1(h_out)
    return out

def predict(self,input):
    with torch.no_grad():
        predictions=self.forward(input)
    return predictions
```

最后，比较一下 `Transformer`, `RNN`, `Mamba` 的区别：

![](img/3f613f9c7733644b24274a52eaad6f03.png)

## 参考资料

[1] [Introduction to Stat](https://link.zhihu.com/?target=https%3A//huggingface.co/blog/lbourdois/get-on-the-ssm-train)e Space Models (SSM)

[2] [Modeling sequences with str](https://link.zhihu.com/?target=https%3A//searchworks.stanford.edu/view/14784021)uctured state spaces

[3] [Mamba Simplified - P](https://link.zhihu.com/?target=https%3A//blog.premai.io/s4-and-mamba/)art 2 - S4 and Mamba

[4] zero-order hold – studywolf

[5] [http:// arrow.utias.utoronto.](https://link.zhihu.com/?target=http%3A//arrow.utias.utoronto.ca/~damaren/s9.pdf)ca /~damaren/s9.pdf

[6] [h](https://link.zhihu.com/?target=https%3A//arxiv.org/ftp/arxiv/papers/2312/2312.00752.pdf)ttps:// arxiv.org/ftp/arxiv/pap ers/2312/2312.00752.pdf

[7] [https:// medium.com/@oxenai/mamb a-linear-time-sequence-mo](https://link.zhihu.com/?target=https%3A//medium.com/%40oxenai/mamba-linear-time-sequence-modeling-with-selective-state-spaces-arxiv-dives-cf96518d7ec4)deling-with-selective-state-spaces-arxiv-dives-cf96518d7ec4

[8] [https:// arxiv.org/](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2401.09417.pdf)pdf/2401.0941 7.pdf
