# 简单训练一个 Transformer，简单用一用

> 来源：[https://agijuejin.feishu.cn/docx/ZaJ4d1g7Uo4jURx524ycu7JVn3f](https://agijuejin.feishu.cn/docx/ZaJ4d1g7Uo4jURx524ycu7JVn3f)

> 作者：王几行XING

> 原文：[https://zhuanlan.z](https://zhuanlan.zhihu.com/p/679596407)hihu.com/p/679596...

## 案例目标

+   通过训练一个 Transformer 模型，简单地实现输出和输入一模一样的序列。比如你输入[1, 2,3,4]，那么模型也输出 [1, 2, 3, 4]

+   可见，这是一个 seq2seq 序列模型

## 第一步，环境准备

```
!pip install x_transformers -q
import tqdm
import torch
import torch.optim as optim
from x_transformers import XTransformer

## 定义一些超参数
NUM_BATCHES = 1000     # 训练过程中的批次总数，这里是 1000
BATCH_SIZE = 32             # 每个批次中的样本数量，这里是 32
LEARNING_RATE = 3e-4        # 学习率，这里是 0.0003
GENERATE_EVERY = 100        # 指定每训练 100 批次后进行一次生成测试
NUM_TOKENS = 16 + 2         # 词汇表的大小，这里是 18（可能包括特殊 token）
ENC_SEQ_LEN = 32            # 编码器处理的序列长度，这里是 32
DEC_SEQ_LEN = 64 + 1        # 解码器处理的序列长度，这里是 65（包括一个起始 token 或其他特殊 token）
```

## 第二步，数据准备

下面利用 cycle 函数，无限地生成数据源。只要训练不停止，就永远地提供训练数据。

```
def cycle():
    # 定义一个无限循环生成器函数 cycle
    while True:
        # 创建一个全为1的张量作为序列的起始token，形状为 (BATCH_SIZE, 1)，并将其转移到 GPU
        prefix = torch.ones((BATCH_SIZE, 1)).long().cuda()

        # 随机生成源序列，值在 [2, NUM_TOKENS) 范围内，形状为 (BATCH_SIZE, ENC_SEQ_LEN)，并将其转移到 GPU
        src = torch.randint(2, NUM_TOKENS, (BATCH_SIZE, ENC_SEQ_LEN)).long().cuda()

        # 将起始token与源序列拼接两次作为目标序列，形成新的张量 tgt
        tgt = torch.cat((prefix, src, src), 1)

        # 创建一个形状与源序列相同的布尔型掩码张量，全为 True，表示所有元素都是有效的，转移到 GPU
        src_mask = torch.ones(BATCH_SIZE, src.shape[1]).bool().cuda()

        # 使用 yield 语句返回一个元组，包含源序列、目标序列和源序列掩码
        yield (src, tgt, src_mask)
```

## 第三步，查看数据

```
# 获取一批数据
src, tgt, src_mask = next(cycle())

import torch
import pandas as pd
# Set the options to display all columns
pd.set_option(&#x27;display.max_columns&#x27;, None)

# Set the options to display all rows
pd.set_option(&#x27;display.max_rows&#x27;, None)



# 将张量转移到 CPU 并转换为 NumPy 数组
numpy_array = src.cpu().numpy()

# 创建 DataFrame
dfsrc = pd.DataFrame(numpy_array)

# 显示 DataFrame
dfsrc
```

![](img/8d9b6ae974f9564cbf94a8caf2d1aed2.png)

```
# 将张量转移到 CPU 并转换为 NumPy 数组
numpy_array = tgt.cpu().numpy()

# 创建 DataFrame
dftgt = pd.DataFrame(numpy_array)

# 显示 DataFrame
dftgt


## target 数据是 source 数据的2次重复，并在最开头加上数字“1”
```

![](img/7b504497738f0a5318dc21d398bb0824.png)

举例：

+   source: [2,2,2]

+   target: [1, 2, 2, 2, 2, 2, 2]

source_mask：

![](img/06da3255bbb7de20252aa21117b97697.png)

## 第四步，建立模型并训练

```
# 实例化模型
model = XTransformer(
    dim = 512,  # 设置模型的维度为 512
    tie_token_emb = True,  # 将编码器和解码器的 token embeddings 绑定
    return_tgt_loss = True,  # 指定模型返回目标损失
    enc_num_tokens = NUM_TOKENS,  # 设置编码器的词汇表大小
    enc_depth = 3,  # 设置编码器的层数为 3
    enc_heads = 8,  # 设置编码器的头数为 8
    enc_max_seq_len = ENC_SEQ_LEN,  # 设置编码器的最大序列长度
    dec_num_tokens = NUM_TOKENS,  # 设置解码器的词汇表大小
    dec_depth = 3,  # 设置解码器的层数为 3
    dec_heads = 8,  # 设置解码器的头数为 8
    dec_max_seq_len = DEC_SEQ_LEN  # 设置解码器的最大序列长度
).cuda()  # 将模型移至 GPU

# 优化器
optim = torch.optim.Adam(model.parameters(), lr=LEARNING_RATE)  # 使用 Adam 优化器，设置学习率

# 训练
for i in tqdm.tqdm(range(NUM_BATCHES), mininterval=10., desc=&#x27;training&#x27;):  # 使用 tqdm 显示训练进度
    model.train()  # 将模型设置为训练模式

    src, tgt, src_mask = next(cycle())  # 从生成器中获取一批数据

    loss = model(src, tgt, mask=src_mask)  # 计算损失
    loss.backward()  # 反向传播
    print(f&#x27;{i}: {loss.item()}&#x27;)  # 打印损失值

    optim.step()  # 更新模型参数
    optim.zero_grad()  # 清空梯度

    if i != 0 and i % GENERATE_EVERY == 0:  # 在指定的间隔内进行性能评估
        model.eval()  # 将模型设置为评估模式
        src, _, src_mask = next(cycle())  # 获取一批数据
        src, src_mask = src[:1], src_mask[:1]  # 仅取一条数据进行测试
        start_tokens = (torch.ones((1, 1)) * 1).long().cuda()  # 创建起始 token

        sample = model.generate(src, start_tokens, ENC_SEQ_LEN, mask = src_mask)  # 生成输出序列
        incorrects = (src != sample).abs().sum()  # 计算不正确的 token 数量

        # 打印输入序列、预测输出序列和不正确的数量
        print(f&quot;input:  &quot;, src)
        print(f&quot;predicted output:  &quot;, sample)
        print(f&quot;incorrects: {incorrects}&quot;)
```

## 第五步，调用模型，完成预测

```
# 假设我们有一些源序列数据
src_sequence = torch.randint(2, NUM_TOKENS, (1, ENC_SEQ_LEN)).long().cuda()  # 随机生成一个源序列

# 创建源序列的掩码
src_mask = torch.ones(1, src_sequence.shape[1]).bool().cuda()

# 设置起始 token，通常是特定的一个 token，比如 1
start_tokens = torch.ones((1, 1)).long().cuda() * 1

# 使用模型生成输出
model.eval()  # 确保模型处于评估模式
generated_sequence = model.generate(src_sequence, start_tokens, ENC_SEQ_LEN, mask=src_mask)


# 打印数据源
print(&#x27;Source Sequence: &#x27;, src_sequence)

# 打印生成的序列
print(&quot;Generated Sequence: &quot;, generated_sequence)
```

输出的结果中，包含了模型的架构，以及数据源序列，和预测结果：

```
XTransformer(
  (encoder): TransformerWrapper(
    (token_emb): TokenEmbedding(
      (emb): Embedding(18, 512)
    )
    (pos_emb): AbsolutePositionalEmbedding(
      (emb): Embedding(32, 512)
    )
    (post_emb_norm): Identity()
    (emb_dropout): Dropout(p=0, inplace=False)
    (project_emb): Identity()
    (attn_layers): Encoder(
      (layers): ModuleList(
        (0): ModuleList(
          (0): ModuleList(
            (0): LayerNorm((512,), eps=1e-05, elementwise_affine=True)
            (1-2): 2 x None
          )
          (1): Attention(
            (to_q): Linear(in_features=512, out_features=512, bias=False)
            (to_k): Linear(in_features=512, out_features=512, bias=False)
            (to_v): Linear(in_features=512, out_features=512, bias=False)
            (attend): Attend(
              (attn_dropout): Dropout(p=0.0, inplace=False)
            )
            (to_out): Linear(in_features=512, out_features=512, bias=False)
          )
          (2): Residual()
        )
        (1): ModuleList(
          (0): ModuleList(
            (0): LayerNorm((512,), eps=1e-05, elementwise_affine=True)
            (1-2): 2 x None
          )
          (1): FeedForward(
            (ff): Sequential(
              (0): Sequential(
                (0): Linear(in_features=512, out_features=2048, bias=True)
                (1): GELU(approximate=&#x27;none&#x27;)
              )
              (1): Dropout(p=0.0, inplace=False)
              (2): Linear(in_features=2048, out_features=512, bias=True)
            )
          )
          (2): Residual()
        )
        (2): ModuleList(
          (0): ModuleList(
            (0): LayerNorm((512,), eps=1e-05, elementwise_affine=True)
            (1-2): 2 x None
          )
          (1): Attention(
            (to_q): Linear(in_features=512, out_features=512, bias=False)
            (to_k): Linear(in_features=512, out_features=512, bias=False)
            (to_v): Linear(in_features=512, out_features=512, bias=False)
            (attend): Attend(
              (attn_dropout): Dropout(p=0.0, inplace=False)
            )
            (to_out): Linear(in_features=512, out_features=512, bias=False)
          )
          (2): Residual()
        )
        (3): ModuleList(
          (0): ModuleList(
            (0): LayerNorm((512,), eps=1e-05, elementwise_affine=True)
            (1-2): 2 x None
          )
          (1): FeedForward(
            (ff): Sequential(
              (0): Sequential(
                (0): Linear(in_features=512, out_features=2048, bias=True)
                (1): GELU(approximate=&#x27;none&#x27;)
              )
              (1): Dropout(p=0.0, inplace=False)
              (2): Linear(in_features=2048, out_features=512, bias=True)
            )
          )
          (2): Residual()
        )
        (4): ModuleList(
          (0): ModuleList(
            (0): LayerNorm((512,), eps=1e-05, elementwise_affine=True)
            (1-2): 2 x None
          )
          (1): Attention(
            (to_q): Linear(in_features=512, out_features=512, bias=False)
            (to_k): Linear(in_features=512, out_features=512, bias=False)
            (to_v): Linear(in_features=512, out_features=512, bias=False)
            (attend): Attend(
              (attn_dropout): Dropout(p=0.0, inplace=False)
            )
            (to_out): Linear(in_features=512, out_features=512, bias=False)
          )
          (2): Residual()
        )
        (5): ModuleList(
          (0): ModuleList(
            (0): LayerNorm((512,), eps=1e-05, elementwise_affine=True)
            (1-2): 2 x None
          )
          (1): FeedForward(
            (ff): Sequential(
              (0): Sequential(
                (0): Linear(in_features=512, out_features=2048, bias=True)
                (1): GELU(approximate=&#x27;none&#x27;)
              )
              (1): Dropout(p=0.0, inplace=False)
              (2): Linear(in_features=2048, out_features=512, bias=True)
            )
          )
          (2): Residual()
        )
      )
      (final_norm): LayerNorm((512,), eps=1e-05, elementwise_affine=True)
    )
    (to_logits): Linear(in_features=512, out_features=18, bias=False)
  )
  (decoder): AutoregressiveWrapper(
    (net): TransformerWrapper(
      (token_emb): TokenEmbedding(
        (emb): Embedding(18, 512)
      )
      (pos_emb): AbsolutePositionalEmbedding(
        (emb): Embedding(65, 512)
      )
      (post_emb_norm): Identity()
      (emb_dropout): Dropout(p=0, inplace=False)
      (project_emb): Identity()
      (attn_layers): Decoder(
        (layers): ModuleList(
          (0-1): 2 x ModuleList(
            (0): ModuleList(
              (0): LayerNorm((512,), eps=1e-05, elementwise_affine=True)
              (1-2): 2 x None
            )
            (1): Attention(
              (to_q): Linear(in_features=512, out_features=512, bias=False)
              (to_k): Linear(in_features=512, out_features=512, bias=False)
              (to_v): Linear(in_features=512, out_features=512, bias=False)
              (attend): Attend(
                (attn_dropout): Dropout(p=0.0, inplace=False)
              )
              (to_out): Linear(in_features=512, out_features=512, bias=False)
            )
            (2): Residual()
          )
          (2): ModuleList(
            (0): ModuleList(
              (0): LayerNorm((512,), eps=1e-05, elementwise_affine=True)
              (1-2): 2 x None
            )
            (1): FeedForward(
              (ff): Sequential(
                (0): Sequential(
                  (0): Linear(in_features=512, out_features=2048, bias=True)
                  (1): GELU(approximate=&#x27;none&#x27;)
                )
                (1): Dropout(p=0.0, inplace=False)
                (2): Linear(in_features=2048, out_features=512, bias=True)
              )
            )
            (2): Residual()
          )
          (3-4): 2 x ModuleList(
            (0): ModuleList(
              (0): LayerNorm((512,), eps=1e-05, elementwise_affine=True)
              (1-2): 2 x None
            )
            (1): Attention(
              (to_q): Linear(in_features=512, out_features=512, bias=False)
              (to_k): Linear(in_features=512, out_features=512, bias=False)
              (to_v): Linear(in_features=512, out_features=512, bias=False)
              (attend): Attend(
                (attn_dropout): Dropout(p=0.0, inplace=False)
              )
              (to_out): Linear(in_features=512, out_features=512, bias=False)
            )
            (2): Residual()
          )
          (5): ModuleList(
            (0): ModuleList(
              (0): LayerNorm((512,), eps=1e-05, elementwise_affine=True)
              (1-2): 2 x None
            )
            (1): FeedForward(
              (ff): Sequential(
                (0): Sequential(
                  (0): Linear(in_features=512, out_features=2048, bias=True)
                  (1): GELU(approximate=&#x27;none&#x27;)
                )
                (1): Dropout(p=0.0, inplace=False)
                (2): Linear(in_features=2048, out_features=512, bias=True)
              )
            )
            (2): Residual()
          )
          (6-7): 2 x ModuleList(
            (0): ModuleList(
              (0): LayerNorm((512,), eps=1e-05, elementwise_affine=True)
              (1-2): 2 x None
            )
            (1): Attention(
              (to_q): Linear(in_features=512, out_features=512, bias=False)
              (to_k): Linear(in_features=512, out_features=512, bias=False)
              (to_v): Linear(in_features=512, out_features=512, bias=False)
              (attend): Attend(
                (attn_dropout): Dropout(p=0.0, inplace=False)
              )
              (to_out): Linear(in_features=512, out_features=512, bias=False)
            )
            (2): Residual()
          )
          (8): ModuleList(
            (0): ModuleList(
              (0): LayerNorm((512,), eps=1e-05, elementwise_affine=True)
              (1-2): 2 x None
            )
            (1): FeedForward(
              (ff): Sequential(
                (0): Sequential(
                  (0): Linear(in_features=512, out_features=2048, bias=True)
                  (1): GELU(approximate=&#x27;none&#x27;)
                )
                (1): Dropout(p=0.0, inplace=False)
                (2): Linear(in_features=2048, out_features=512, bias=True)
              )
            )
            (2): Residual()
          )
        )
        (final_norm): LayerNorm((512,), eps=1e-05, elementwise_affine=True)
      )
      (to_logits): Linear(in_features=512, out_features=18, bias=False)
    )
  )
)

## 数据源序列
Source Sequence:  tensor([[10,  8,  4, 15, 16,  5, 14, 14, 16, 13,  6,  7,  5,  2,  7,  8, 13, 16,
          2, 14,  8, 12, 12,  3, 10,  3,  8,  5,  8,  8, 10, 10]],
       device=&#x27;cuda:0&#x27;)

## 预测结果
Generated Sequence:  tensor([[10,  8,  4, 15, 16,  5, 14, 14, 16, 13,  6,  7,  5,  2,  7,  8, 13, 16,
          2, 14,  8, 12, 12,  3, 10,  3,  8,  5,  8,  8, 10, 10]],
       device=&#x27;cuda:0&#x27;)
```

下面是添加注释后的模型架构：

```
# XTransformer 模型的定义
XTransformer(
  # 编码器部分
  (encoder): TransformerWrapper(
    # 词嵌入层，用于将词汇编码为固定维度的向量
    (token_emb): TokenEmbedding(
      # Embedding 层，词汇表大小为 18，嵌入维度为 512
      (emb): Embedding(18, 512)
    ),
    # 绝对位置编码层，为每个位置提供唯一的编码
    (pos_emb): AbsolutePositionalEmbedding(
      # Embedding 层，位置个数为 32，嵌入维度为 512
      (emb): Embedding(32, 512)
    ),
    # 后嵌入层正规化，这里使用的是 Identity，即不进行变换
    (post_emb_norm): Identity(),
    # 嵌入后的 Dropout 层，防止过拟合，这里的 Dropout 概率为 0
    (emb_dropout): Dropout(p=0, inplace=False),
    # 嵌入后的投影层，这里使用的是 Identity，即不进行变换
    (project_emb): Identity(),
    # 编码器的注意力层，负责处理序列
    (attn_layers): Encoder(
      # 编码器的多个层，这里使用 ModuleList 管理
      (layers): ModuleList(
        # 下面是编码器中的多个子层，每个子层包含不同的组件
        # 第一个子层
        (0): ModuleList(
          # 层正规化组件
          (0): ModuleList(
            # LayerNorm 层，用于正规化输入
            (0): LayerNorm((512,), eps=1e-05, elementwise_affine=True),
            # 以下两个位置是预留给其他可能的组件，这里没有使用
            (1-2): 2 x None
          ),
          # 注意力机制组件
          (1): Attention(
            # 将输入转换为查询向量的线性层
            (to_q): Linear(in_features=512, out_features=512, bias=False),
            # 将输入转换为键向量的线性层
            (to_k): Linear(in_features=512, out_features=512, bias=False),
            # 将输入转换为值向量的线性层
            (to_v): Linear(in_features=512, out_features=512, bias=False),
            # 注意力计算组件
            (attend): Attend(
              # 注意力计算中的 Dropout 层
              (attn_dropout): Dropout(p=0.0, inplace=False)
            ),
            # 将注意力输出转换为最终输出的线性层
            (to_out): Linear(in_features=512, out_features=512, bias=False)
          ),
          # 残差连接组件
          (2): Residual()
        ),
        # 以下是编码器的其他子层，结构与第一个子层类似
        (1): ModuleList(
          (0): ModuleList(
            (0): LayerNorm((512,), eps=1e-05, elementwise_affine=True),
            (1-2): 2 x None
          ),
          (1): FeedForward(
            (ff): Sequential(
              (0): Sequential(
                (0): Linear(in_features=512, out_features=2048, bias=True),
                (1): GELU(approximate=&#x27;none&#x27;)
              ),
              (1): Dropout(p=0.0, inplace=False),
              (2): Linear(in_features=2048, out_features=512, bias=True)
            )
          ),
          (2): Residual()
        ),
        # ...（其他子层的结构与前述类似）
      ),
      # 编码器最终的层正规化
      (final_norm): LayerNorm((512,), eps=1e-05, elementwise_affine=True)
    ),
    # 将编码器的输出转换为最终预测的线性层
    (to_logits): Linear(in_features=512, out_features=18, bias=False)
  )


  # 解码器部分
  (decoder): AutoregressiveWrapper(
    # 解码器内的 Transformer 包装器
    (net): TransformerWrapper(
      # 词嵌入层，用于将词汇编码为固定维度的向量
      (token_emb): TokenEmbedding(
        # Embedding 层，词汇表大小为 18，嵌入维度为 512
        (emb): Embedding(18, 512)
      ),
      # 绝对位置编码层，为每个位置提供唯一的编码
      (pos_emb): AbsolutePositionalEmbedding(
        # Embedding 层，位置个数为 65，嵌入维度为 512
        (emb): Embedding(65, 512)
      ),
      # 后嵌入层正规化，这里使用的是 Identity，即不进行变换
      (post_emb_norm): Identity(),
      # 嵌入后的 Dropout 层，防止过拟合，这里的 Dropout 概率为 0
      (emb_dropout): Dropout(p=0, inplace=False),
      # 嵌入后的投影层，这里使用的是 Identity，即不进行变换
      (project_emb): Identity(),
      # 解码器的注意力层，负责处理序列
      (attn_layers): Decoder(
        # 解码器的多个层，这里使用 ModuleList 管理
        (layers): ModuleList(
          # 下面是解码器中的多个子层，每个子层包含不同的组件
          # 第一个子层
          (0-1): 2 x ModuleList(
            # 层正规化组件
            (0): ModuleList(
              # LayerNorm 层，用于正规化输入
              (0): LayerNorm((512,), eps=1e-05, elementwise_affine=True),
              # 以下两个位置是预留给其他可能的组件，这里没有使用
              (1-2): 2 x None
            ),
            # 注意力机制组件
            (1): Attention(
              # 将输入转换为查询向量的线性层
              (to_q): Linear(in_features=512, out_features=512, bias=False),
              # 将输入转换为键向量的线性层
              (to_k): Linear(in_features=512, out_features=512, bias=False),
              # 将输入转换为值向量的线性层
              (to_v): Linear(in_features=512, out_features=512, bias=False),
              # 注意力计算组件
              (attend): Attend(
                # 注意力计算中的 Dropout 层
                (attn_dropout): Dropout(p=0.0, inplace=False)
              ),
              # 将注意力输出转换为最终输出的线性层
              (to_out): Linear(in_features=512, out_features=512, bias=False)
            ),
            # 残差连接组件
            (2): Residual()
          ),
          # 第二个子层的结构与第一个子层类似
          (2): ModuleList(
            (0): ModuleList(
              (0): LayerNorm((512,), eps=1e-05, elementwise_affine=True),
              (1-2): 2 x None
            ),
            (1): FeedForward(
              (ff): Sequential(
                (0): Sequential(
                  (0): Linear(in_features=512, out_features=2048, bias=True),
                  (1): GELU(approximate=&#x27;none&#x27;)
                ),
                (1): Dropout(p=0.0, inplace=False),
                (2): Linear(in_features=2048, out_features=512, bias=True)
              )
            ),
            (2): Residual()
          ),
          # ...（其他子层的结构与前述类似）
          # 最终的层正规化
          (final_norm): LayerNorm((512,), eps=1e-05, elementwise_affine=True)
        ),
        # 将解码器的输出转换为最终预测的线性层
        (to_logits): Linear(in_features=512, out_features=18, bias=False)
      )
    )
  )
)
```

参考案例： [ht](https://link.zhihu.com/?target=https%3A//github.com/lucidrains/x-transformers/blob/main/examples/toy_tasks/enc_dec_copy.py)tps:// github.com/lucidrains/x -transformers/blob/main/examples/toy_tasks/enc_dec_copy.py