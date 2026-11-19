
# Qwen-blog模型学习笔记

> 来源：[https://datawhaler.feishu.cn/docx/C7r5dxBWKoZlmtxqLixcPLeAnje](https://datawhaler.feishu.cn/docx/C7r5dxBWKoZlmtxqLixcPLeAnje)

## 0. 课程介绍

《大模型白盒子构建指南》是一个从原理出发、以“白盒”为导向、围绕大模型全链路的“手搓”大模型指南，旨在帮助有传统深度学习基础的读者从底层原理出发，“纯手搓”搭建一个清晰、可用的大模型系统，包括大模型本身、RAG 框架、Agent 系统及大模型评估体系。教程将从基础原理出发，深入剖析每一个技术点并附以完整的代码实现，以细致讲解和代码注释帮助读者独立复现大模型核心部分，并在复现中实现对大模型的深入理解与掌握。

本学习笔记是在datawhale出品的《大模型白盒子构建指南》的基础上进行的，融入笔者的思考、大白话、比喻、实操等过程。原始的课程内容需要具备一定的基础，对初学者来说还是有一定的门槛，本笔记将其通俗化。如有余力，想要更细致的了解其内在过程，可自行学习原始课程（`https://github.com/datawhalechina/tiny-universe`）

## 1. Qwen模型基础介绍

Qwen是一个大型语言模型，其架构与Llama2类似。作为一个初学者，我们需要先了解它的基本组成部分。

### 1.1 基本架构

#### A. 整体流程（宏观视角）

模型的整体处理流程如下：

##### 总览

![](img/7eb2eb134487921c74cf63190755edaf.png)

##### 输入层

![](img/24fafbc85c1096694b020a12c17cd52d.png)

> 说明：

- `Tokenizer`的分词规则是基于频率和统计的，不是简单的字符分割
- 每个`token`对应词表中的一个唯一数字ID
- 具体的分词方式取决于模型训练时使用的`tokenizer`

##### 核心处理层

![](img/4aa43fe50755b1be1a880d8774af3216.png)

**大白话解释：**

1. **数字变向量**：
   - 就像每个字都有多个含义一样
   - 模型用一串数字（向量）来表达每个字的各种特征
   - 比如"爱"可能包含"情感值：0.9，动作值：0.3"等信息

2. **上下文理解**：
   - 模型会像人一样"看"前面的词
   - "爱"这个字的理解会考虑到前面的"我"
   - "AI"的理解会考虑到"我爱"的语境

3. **输出处理**：
   - 模型根据理解的内容生成回应
   - 就像人思考后给出回答一样
   - 每个可能的回答都会有一个概率分数

#### B. Qwen2内部结构（细节视角）

##### Embedding层

![](img/31bc5aaa2a19a89c2f0123439b96d018.png)

**功能：**将`Input_ids`转换为向量表示

**特点：**保留了词的语义信息

**输出：**生成初始的`Hidden_states`

> 类比一下：

- 比如描述一个人：[身高=180cm, 体重=70kg, 年龄=25岁, ...]
- `Embedding`也是用一系列数字来描述每个词的各种特征，包含了词的含义、语法特点、使用场景等信息

> 为什么需要这一步？

- 计算机不理解纯数字(2768, 4925, 8956)
- 需要把每个数字变成一组有意义的特征值，帮助模型理解每个词的各种属性

##### Hidden_states处理

**作用：**存储词向量的状态信息

**特点：**包含了上下文的语义表示

**流向：**作为`Layers`的输入

大白话解释：

```
把Hidden_states想象成"记忆本"：

还是用"我爱AI"的例子：

1. 初始记忆（来自Embedding）：
   "我" → [0.1, -0.2, 0.3, ...]（只知道是"我"这个词的基本信息）
   "爱" → [0.8, 0.9, -0.1, ...]（只知道是"爱"这个词的基本信息）
   "AI" → [0.5, 0.6, 0.7, ...]（只知道是"AI"这个词的基本信息）

2. 上下文理解后的记忆：
   "我" → [0.15, -0.25, 0.35, ...]（知道这个"我"是句子开头）
   "爱" → [0.85, 0.95, -0.15, ...]（知道是"我爱..."的"爱"）
   "AI" → [0.55, 0.65, 0.75, ...]（知道是"我爱AI"中的"AI"）
```

> 为什么需要`Hidden_states`？

- 它像是模型的"工作记忆"
- 不仅记住每个词的意思，还要记住这个词在整句话中的作用

##### Layers层（核心处理单元）

![](img/596fce9d88f544b16ead0011ccd708c5.png)

**结构：**包含多个相同的`Decoder`层

**组成：**

- `Attention`层（处理注意力机制）
- `MLP`层（多层感知器）
- 残差连接（Residual Connection）

**特点：**层层堆叠，逐步提取特征

大白话解释：

```
把Layers层想象成"多轮思考"的过程：

还是拿"我爱AI"举例：

第1层思考（Layer 1）：
- Attention（注意力）：像人一样"看"句子
  * "我"：就是我自己
  * "爱"：哦，原来是"我"在爱
  * "AI"：明白了，是"我爱"的对象

- MLP（深入处理）：加工这个理解
  * 理解"我"是主语
  * 理解"爱"是感情表达
  * 理解"AI"是被爱的对象

第2层思考（Layer 2）：
- 在第1层的基础上更深入思考
  * "我爱"：表达主动的感情
  * "爱AI"：表达对技术的喜爱
  * 整体：这是一个人对AI的感情表达

第3层思考（Layer 3）：
- 更高层次的理解
  * 可能理解这是个正面的情感表达
  * 可能理解说话人对AI技术的态度
  * 可能理解这句话的语气和风格
```

> 类比一下：

- 就像人思考问题时的过程：
- 第一遍：获取基本信息
- 第二遍：理解深层含义
- 第三遍：形成完整认识
- 每思考一遍，理解就更深一层

**为什么需要多层？**

- 就像解决复杂问题需要多次思考
- 每一层关注不同的特征和关系
- 层层递进，理解越来越深入，最终形成全面的理解

> 残差连接的作用：

- 就像做数学题时保留草稿
- 即使想到新的理解，也不会忘记原始信息
- 帮助模型记住前面层的重要信息
- 防止信息在传递中丢失

##### RMSNorm层

![](img/6a54a0a33416447817f0ee28de98b76e.png)

**功能：**对`Hidden_states`进行标准化，使某个`token`的向量中的数值差异不要太大

**作用：**

- 稳定训练过程
- 加速收敛
- 防止梯度消失/爆炸

> 与普通`Norm`的主要区别：

1. **计算方式更简单**

   - 普通`Norm`：需要计算平均值和方差
   - `RMSNorm`：只计算均方根，更快更高效

2. **保留特征更好**

   - 普通`Norm`：可能过度平滑，丢失特征
   - `RMSNorm`：保留相对差异，不会太过平均化

3. **训练更稳定**

   - 普通`Norm`：有时会使训练不稳定
   - `RMSNorm`：更容易收敛，训练更平稳

#### C. 数据流动过程

##### 文本预处理阶段

原始文本 → `Tokenizer`分词 → 生成`Input_ids`

`Input_ids`进入`Embedding`层获得初始向量表示

##### 特征提取阶段

`Embedding`向量 → `Hidden_states`

`Hidden_states`在多个`Layers`中循环处理

每个`Layer`包含：

- 自注意力机制处理
- `MLP`特征转换
- 残差连接保留信息

##### 输出处理阶段

最终的`Hidden_states` → `Linear`层

计算`Loss`

生成最终输出

### 1.2 主要特点

1. **模型配置灵活**

   - 可以自定义词表大小(`vocab_size`)
   - 可以设置隐藏层大小(`hidden_size`)
   - 可以调整注意力头数量(`num_attention_heads`)

2. **标准化处理**

   - 使用`RMSNorm`进行标准化
   - 有助于模型训练的稳定性

3. **位置编码**

   - 使用旋转位置编码(`Rotary Position Embedding`)
   - 帮助模型理解文本中词语的位置关系

## 2. 代码详解：配置文件（Qwen2Config）

`Qwen2Config`是模型的配置类，就像是模型的"说明书"，定义了模型的基本参数：

1. **主要参数**：

   - `vocab_size`：词表大小（类似字典里有多少个词）
   - `hidden_size`：隐藏层的大小（决定了模型的理解能力）
   - `num_hidden_layers`：隐藏层的数量（决定了模型思考的深度）
   - `num_attention_heads`：注意力头的数量（决定了模型同时关注的角度）

2. **使用方式**：

```
# 类似字典的使用方式
config.pad_token_id  # 获取填充标记的ID
```

### 2.1 模型（Qwen2Model）

#### 2.1.1 模型初始化

大白话解释模型的初始化过程：

```
class Qwen2Model(Qwen2PreTrainedModel):
    def __init__(self, config: Qwen2Config):
        super().__init__(config)
        # 基本属性设置
        self.padding_idx = config.pad_token_id
        self.vocab_size = config.vocab_size

        # 核心组件初始化
        self.embed_tokens = nn.Embedding(config.vocab_size, config.hidden_size, self.padding_idx)
        self.layers = nn.ModuleList(
            [Qwen2DecoderLayer(config, layer_idx) for layer_idx in range(config.num_hidden_layers)]
        )
        self.norm = Qwen2RMSNorm(config.hidden_size, eps=config.rms_norm_eps)

        # 其他设置
        self.gradient_checkpointing = False
        # Initialize weights and apply final processing
        self.post_init() # 完成最后的初始化，做最后的检查和准备工作
        
    def post_init(self):
        """
        A method executed at the end of each Transformer model initialization, to execute code that needs the model's
        modules properly initialized (such as weight initialization).
        """
        self.init_weights()
        self._backward_compatibility_gradient_checkpointing()
```

整个初始化过程就像是准备开一家新餐厅：

- 配置文件(`Config`)就像是餐厅的规划书
- 基本属性就像是确定餐厅的规模和位置
- 核心组件就像是准备厨房设备和餐具
- 其他设置就像是做最后的卫生检查和人员培训

#### 2.1.2 Forward工作流

前向传播是模型处理输入数据的具体过程，就像是餐厅接待客人时的完整流程：

```
inputs_embeds = self.embed_tokens(input_ids)
# 就像接待员把客人的点单（input_ids）翻译成厨房能理解的语言（inputs_embeds）
hidden_states = inputs_embeds

# 多轮思考
for idx, decoder_layer in enumerate(self.layers):
    # 将所有的hidden_states保存成tuple
    if output_hidden_states:
        all_hidden_states += (hidden_states,)
    # 将hs送入每一层decoder_layer
    layer_outputs = decoder_layer(
        hidden_states,
        attention_mask=attention_mask,
        position_ids=position_ids,
        past_key_value=past_key_value,
        output_attentions=output_attentions,
        use_cache=use_cache,
    )
    # 取出上一层decoder_输出的hs,再传入下一个layer
    # 只要第一个,第二个是cache的一个类，然后进入下一个layer
    hidden_states = layer_outputs[0]
    
# 最终处理
# 将最后layers输出后的hidden_states进行标准化  
hidden_states = self.norm(hidden_states)
# 加上最后一层的hidden_states
if output_hidden_states:
    all_hidden_states += (hidden_states,)
```

类比：

1. **像流水线工作**：

   - 每个工序都有专门的处理环节
   - 前一个环节的输出是后一个环节的输入
   - 保证信息顺畅流动

2. **像多轮会议讨论**：

   - 每一轮都基于前面的讨论继续深入
   - 不同的人（层）关注不同的角度
   - 最后形成完整的理解

### 2.2 Qwen2DecoderLayer

![](img/76f026c189b41400aad74265c798f711.png)

#### 2.2.1 初始化

`Qwen2DecoderLayer`是模型中最重要的处理单元，每个`DecoderLayer`都包含了三个主要部分：注意力机制（`Attention`）、多层感知器（`MLP`）和标准化层（`RMSNorm`）。

```
QWEN2_ATTENTION_CLASSES = {
    "eager": Qwen2Attention,  # 一般情况下是这个
    "flash_attention_2": Qwen2FlashAttention2,
    "sdpa": Qwen2SdpaAttention,
}

class Qwen2DecoderLayer(nn.Module):
    def __init__(self, config: Qwen2Config):
        super().__init__()
        self.hidden_size = config.hidden_size
        # 三件套
        self.self_attn = QWEN2_ATTENTION_CLASSES[config._attn_implementation](config, layer_idx)
        self.mlp = Qwen2MLP(config)
        self.input_layernorm = Qwen2RMSNorm(config.hidden_size, eps=config.rms_norm_eps)
        self.post_attention_layernorm = Qwen2RMSNorm(config.hidden_size, eps=config.rms_norm_eps)
```

#### 2.2.2 Forward



## Qwen2DecoderLayer 前向传播

```python
# **第一阶段：注意力处理**
residual = hidden_states
#  标准化后送入attn
hidden_states = self.input_layernorm(hidden_states)  # RMSNorm标准化
# Self Attention
hidden_states, self_attn_weights, present_key_value = self.self_attn(  
    hidden_states=hidden_states,
    attention_mask=attention_mask,
    position_ids=position_ids,
    past_key_value=past_key_value,
    output_attentions=output_attentions,
    use_cache=use_cache,
    **kwargs,
)

# 残差与新的hidden_states相加
hidden_states = residual + hidden_states

# **第二阶段：MLP处理**
# 再次保存当前状态
residual = hidden_states
# 同样的RMSNorm标准化
hidden_states = self.post_attention_layernorm(hidden_states)
hidden_states = self.mlp(hidden_states)

hidden_states = residual + hidden_states

outputs = (hidden_states,)

return outputs
```

### 大白话解释

就像厨房处理食材：

- 先保留接待处理的结果（`residual`）
- 整理后交给厨师深度加工（`norm` + `mlp`）
- 最后把厨师的创新和原始风味结合（残差连接）

### 类比

像是写论文的过程：

- 保留文献阅读的理解（`residual`）
- 深入思考得出自己的见解（`mlp`）
- 把文献理解和自己的想法结合（残差连接）

## 2.3 Qwen2Attention

### 2.3.1 初始化

```python
class Qwen2Attention(nn.Module):
    """Multi-headed attention from 'Attention Is All You Need' paper"""

    def __init__(self, config: Qwen2Config):
        super().__init__()
        self.config = config
        self.layer_idx = layer_idx
        self.hidden_size = config.hidden_size
        self.num_heads = config.num_attention_heads
        self.head_dim = self.hidden_size // self.num_heads
        self.num_key_value_heads = config.num_key_value_heads
        self.num_key_value_groups = self.num_heads // self.num_key_value_heads
        self.max_position_embeddings = config.max_position_embeddings
        self.rope_theta = config.rope_theta
        self.is_causal = True
        self.attention_dropout = config.attention_dropout

        if (self.head_dim * self.num_heads) != self.hidden_size:
            raise ValueError(
                f"hidden_size must be divisible by num_heads (got `hidden_size`: {self.hidden_size}"
                f" and `num_heads`: {self.num_heads})."
            )
        self.q_proj = nn.Linear(self.hidden_size, self.num_heads * self.head_dim, bias=config.attention_bias)
        self.k_proj = nn.Linear(self.hidden_size, self.num_key_value_heads * self.head_dim, bias=config.attention_bias)
        self.v_proj = nn.Linear(self.hidden_size, self.num_key_value_heads * self.head_dim, bias=config.attention_bias)
        self.o_proj = nn.Linear(self.num_heads * self.head_dim, self.hidden_size, bias=config.attention_bias)
        
        self.rotary_emb = Qwen2RotaryEmbedding(
            self.head_dim,
            max_position_embeddings=self.max_position_embeddings,
            base=self.rope_theta,
        )
```

### 2.3.2 Forward

```python
# 获取形状信息,hidden_states输入的为(bs,T,hd)
bsz, q_len, _ = hidden_states.size()

# 对hidden_states进行Linear生成query、key、value
query_states = self.q_proj(hidden_states)
key_states = self.k_proj(hidden_states)
value_states = self.v_proj(hidden_states)

 # reshape多头处理--分块--(bs,T,heads,hd_d)
query_states = query_states.view(bsz, q_len, self.num_heads, self.head_dim).transpose(1, 2)
key_states = key_states.view(bsz, q_len, self.num_key_value_heads, self.head_dim).transpose(1, 2)
value_states = value_states.view(bsz, q_len, self.num_key_value_heads, self.head_dim).transpose(1, 2)

# 将旋转位置嵌入应用于查询和键张量。使用了旋转位置嵌入的余弦和正弦部分，将它们与查询和键张量相乘，并将结果相加，从而实现旋转位置嵌入的效果
cos, sin = self.rotary_emb(value_states, seq_len=kv_seq_len)
query_states, key_states = apply_rotary_pos_emb(query_states, key_states, cos, sin, position_ids)

# 先将key_states和value_states重复了num_key_value_groups次
key_states = repeat_kv(key_states, self.num_key_value_groups)
value_states = repeat_kv(value_states, self.num_key_value_groups)

# 使用dot attn实现q*kT/hd_d^0.5
attn_weights = torch.matmul(query_states, key_states.transpose(2, 3)) / math.sqrt(self.head_dim)

# 然后 attn_weights 加上 attention_mask，实现读取顺序
attn_weights = attn_weights + attention_mask

# softmax + dropout + values_states相乘
attn_weights = nn.functional.softmax(attn_weights, dim=-1, dtype=torch.float32).to(query_states.dtype)
attn_weights = nn.functional.dropout(attn_weights, p=self.attention_dropout, training=self.training)
attn_output = torch.matmul(attn_weights, value_states)

# 转置，修改形状等reshape操作
attn_output = attn_output.transpose(1, 2).contiguous()
attn_output = attn_output.reshape(bsz, q_len, self.hidden_size)

# 最后在进行一次o_proj
attn_output = self.o_proj(attn_output)

# 返回结果
return attn_output, attn_weights, past_key_value
```

### 假设处理"我爱AI"这句话

1. 参数设置：

- `hidden_size` = 1024（总特征维度）
- `num_heads` = 8（8个注意力头）
- `head_dim` = 128（每个头128维特征）
- `num_key_value_heads` = 4（4个KV组）

2. 工作方式：

- 8个注意力头分成4组
- 每组负责不同角度的理解：
  - 组1可能关注语法关系
  - 组2可能关注情感色彩
  - 组3可能关注语义联系
  - 组4可能关注上下文关系

## 2.4 Qwen2 MLP

![](img/0aa1e4b7217b69de78ae7af88dac4170.png)

`MLP`（多层感知器）是模型中的"深度思考"部分，就像是一个信息加工厂。它的工作原理：

```python
class Qwen2MLP(nn.Module):
    def __init__(self, config):
        super().__init__()
        # 这俩不必多说
        self.config = config
        self.hidden_size = config.hidden_size
        self.intermediate_size = config.intermediate_size

        # 三个全连接层
        self.gate_proj = nn.Linear(self.hidden_size, self.intermediate_size, bias=False)
        self.up_proj = nn.Linear(self.hidden_size, self.intermediate_size, bias=False)
        self.down_proj = nn.Linear(self.intermediate_size, self.hidden_size, bias=False)
        self.act_fn = ACT2FN[config.hidden_act]

    def forward(self, x):
        down_proj = self.down_proj(self.act_fn(self.gate_proj(x)) * self.up_proj(x))
        return down_proj
```

### 信息处理过程

```
输入信息 → 同时进入两条处理线
↓
路线1：gate_proj → 激活函数 → 产生"重要性权重"
路线2：up_proj → 产生"特征信息"
↓
两条线路的结果相乘（突出重要信息）
↓
down_proj整理打包，输出最终结果
```

### 生活中的类比

- 就像写读书笔记：
  - 第一遍（`gate_proj`）：标记重要段落
  - 第二遍（`up_proj`）：详细理解内容
  - 相乘：重点关注标记的重要内容
  - 最后（`down_proj`）：整理成简洁的笔记

## 2.5 Qwen2RMSNorm

`RMSNorm`是模型中的"数据调节器"，负责保持数据的稳定性。

公式：

![](img/4a039f7f94427549ea393c46502b57ca.png)

其中：

- `x` 是层的输入的 `hidden_state`
- `w_i` 表示的是 `hidden_state` 的最后一个维度的值
- `n` 表示上面输入的最后一个维度的数量
- `ε` 是一个很小的数，防止除0

```python
class Qwen2RMSNorm(nn.Module):  # 标准化层
    def __init__(self, hidden_size, eps=1e-6):
        """
        Qwen2RMSNorm is equivalent to T5LayerNorm
        """
        super().__init__()
        self.weight = nn.Parameter(torch.ones(hidden_size))
        self.variance_epsilon = eps

    def forward(self, hidden_states):
        input_dtype = hidden_states.dtype
        hidden_states = hidden_states.to(torch.float32)
        variance = hidden_states.pow(2).mean(-1, keepdim=True)
        hidden_states = hidden_states * torch.rsqrt(variance + self.variance_epsilon)
        return self.weight * hidden_states.to(input_dtype)
```

## 学习总结

说实话，一开始看到Qwen模型的代码时，我也是一头雾水，感觉像是在看天书，感谢datawhale的参考教程。

慢慢地，我发现这个模型其实就像是一个特别勤奋的学生，它学习新知识的方式和我们人类惊人地相似！它先是像小学生一样，一个字一个字地认识文字（`Tokenizer`），然后像中学生一样，开始理解每个词的含义（`Embedding`），接着像大学生一样，学会从多个角度思考问题（多头注意力），最后还像个研究生，能够举一反三、融会贯通（`MLP`层）。每一步都会做好笔记（残差连接），还会适时调整自己的学习状态（`RMSNorm`）。这么一想，原来AI也是像我们学习的过程一样，通过一步步的积累来学习的，只是它比我们更有耐心，更加严谨。

之后继续学习AI领域的一些原理、知识！

参考学习链接：`https://github.com/datawhalechina/tiny-universe/tree/main/content/Qwen-blog`。
