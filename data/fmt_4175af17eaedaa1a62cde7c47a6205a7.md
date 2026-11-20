
## （一）MLPRaw

```
def forward(self, idx, targets=None):
    # 定义前向传播方法 forward，接受两个参数：
    ## idx（输入 token 的索引）和 targets（目标 token 的索引，可选）。
    # idx are the input tokens, (B, T) tensor of integers
    # targets are the target tokens, (B, ) tensor of integers
    B, T = idx.size() # 获取输入 idx 的形状，B 表示批次大小，T 表示上下文长度。
    # forward pass
    # 使用嵌入层 self.wte 将输入 idx 转换为嵌入向量。
    emb = self.wte[idx] # (B, T, embedding_size) 
    # 将嵌入向量展平。
    emb = emb.view(B, -1) # (B, T * embedding_size)
    # 通过第一个全连接层和 tanh 激活函数，计算隐藏层的输出 hidden。
    hidden = torch.tanh(emb @ self.fc1_weights + self.fc1_bias)
    # 通过第二个全连接层计算输出 logits。
    ## 结果 logits 的形状为 (B, vocab_size)，表示每个输入序列在词汇表中每个词的得分。
    logits = hidden @ self.fc2_weights + self.fc2_bias
    # 如果提供了目标 targets，计算交叉熵损失 F.cross_entropy(logits, targets)。
    loss = None
    if targets is not None:
        loss = F.cross_entropy(logits, targets)
    return logits, loss
def __call__(self, idx, targets=None):
    # 定义 __call__ 方法，使得模型实例可以像函数一样被调用。
    return self.forward(idx, targets)
```

注：`hidden @ self.fc2_weights` 是矩阵乘法，适用于符合矩阵乘法规则的张量；`hidden * self.fc2_weights` 是逐元素相乘，要求两个操作数的形状完全相同。在`forward`方法中，我们需要进行的是矩阵乘法而不是逐元素相乘。

## （二）MLP

是不是觉得上面👆定义 `MLPRaw` 模型类的方法有些许的繁琐，那么接下来让我们一起看一下作者 Andrej Karpathy 基于`torch.nn.Module`给出的第二种实现方式。

`torch.nn.Module`是 PyTorch 中用于定义和管理神经网络的基类，该类提供了灵活的结构来定义网络层和实现前向传播，我们可以通过继承`nn.Module`类来构建自定义的神经网络模型。使用`nn.Module`构建模型的一般步骤包括：定义继承自`nn.Module`的类、在`init`方法中初始化模型层、在`forward`方法中定义前向传播逻辑。

代码如下：

```
class MLP(nn.Module): # 继承自 nn.Module，这是所有 PyTorch 模型的基类。
    def __init__(self, vocab_size, context_length, embedding_size, hidden_size, rng):
        # 接受五个参数：vocab_size（词汇表大小）、context_length（上下文长度）、
        ## embedding_size（嵌入层大小）、hidden_size（隐藏层大小）和 rng（随机数生成器）。
        # 调用 super().__init__() 初始化父类 nn.Module。
        super().__init__()
        # 定义一个嵌入层 self.wte，使用 nn.Embedding 将输入的 token 索引转换为嵌入向量。
        ## vocab_size 是词汇表的大小，embedding_size 是嵌入向量的维度。
        self.wte = nn.Embedding(vocab_size, embedding_size) 
        # 使用 nn.Sequential 定义一个多层感知机（MLP）：
        #
        self.mlp = nn.Sequential(
            nn.Linear(context_length * embedding_size, hidden_size), # 第一层全连接层，将输入的上下文嵌入向量映射到隐藏层。
            nn.Tanh(), # # Tanh 激活函数。
            nn.Linear(hidden_size, vocab_size) # 第二层线性层，将隐藏层的输出映射到词汇表大小的输出。
        )
        self.reinit(rng) # 调用 reinit 函数，使用自定义的随机数生成器 rng 初始化权重。
        
    @torch.no_grad()
    def reinit(self, rng):
        # 定义 reinit 函数，并使用 @torch.no_grad() 装饰器，表示在这个函数中不需要计算梯度。
        def reinit_tensor_randn(w, mu, sigma):
            # 以正态分布 N(mu, sigma) 初始化张量 w 的权重。
            winit = torch.tensor(rng.randn(w.numel(), mu=mu, sigma=sigma))
            w.copy_(winit.view_as(w))

        def reinit_tensor_rand(w, a, b):
            # 以均匀分布 U(a, b) 初始化张量 w 的权重。
            winit = torch.tensor(rng.rand(w.numel(), a=a, b=b))
            w.copy_(winit.view_as(w))

        # Let&#x27;s match the PyTorch default initialization:
        # 以均值为0、标准差为1的正态分布初始化嵌入层 self.wte 的权重。
        reinit_tensor_randn(self.wte.weight, mu=0, sigma=1.0)
        scale = (self.mlp[0].in_features)**-0.5 # 算第一层全连接层的缩放系数 scale，其值为输入特征数量的负平方根。
        # 以均匀分布 U(-scale, scale) 初始化第一层全连接的权重和偏置。
        reinit_tensor_rand(self.mlp[0].weight, -scale, scale) 
        reinit_tensor_rand(self.mlp[0].bias, -scale, scale)
        # 对于第二层全连接层的处理同上
        scale = (self.mlp[2].in_features)**-0.5
        reinit_tensor_rand(self.mlp[2].weight, -scale, scale)
        reinit_tensor_rand(self.mlp[2].bias, -scale, scale)
        
    def forward(self, idx, targets=None):
        # 与 MLPRaw 类的 forward 函数基本相同，但更简洁。
        B, T = idx.size()
        emb = self.wte(idx) # (B, T, embedding_size)
        emb = emb.view(B, -1) # (B, T * embedding_size)
        logits = self.mlp(emb)
        loss = None
        if targets is not None:
            loss = F.cross_entropy(logits, targets)
        return logits, loss
```

> Q：为什么需要定义 `reinit()` 函数？

> A：定义 `reinit()` 函数的主要原因是为了使用自定义的随机数生成器来初始化模型的权重和偏置，确保初始化的可控性和一致性，从而提高实验的可重复性和结果的可靠性。但在我们平时一般的工作中可以不使用。

我们可以使用 `torchinfo` 和 `torchviz` 第三方库来打印模型的结构等相关信息以及可视化计算图，如下所示~

![](img/1d2819494fb1f135423d5644636e1b96.png)

![](img/05ce6597655cf0ac99587aa040795321.png)

## 模型训练

### （一）数据准备

读取训练、验证和测试数据，进行字符到 token 的映射，并预处理数据以便后续使用。

代码如下：

```
# &quot;train&quot; the Tokenizer, so we&#x27;re able to map between characters and tokens
train_text = open(&#x27;data/train.txt&#x27;, &#x27;r&#x27;).read() # 读取训练数据文件 train.txt 的内容。
assert all(c == &#x27;\n&#x27; or (&#x27;a&#x27; &lt;= c &lt;= &#x27;z&#x27;) for c in train_text) # 断言检查所有字符是否为小写字母或换行符，以确保数据符合预期格式。
uchars = sorted(list(set(train_text))) # 提取输入文本中的唯一字符，并按字母顺序排序。
vocab_size = len(uchars) # 计算词汇表大小 vocab_size。
# 创建字符到 token 的映射 char_to_token 和 token 到字符的映射 token_to_char。
char_to_token = {c: i for i, c in enumerate(uchars)} 
token_to_char = {i: c for i, c in enumerate(uchars)}
EOT_TOKEN = char_to_token[&#x27;\n&#x27;] # 指定换行符 \n 为结束符 EOT_TOKEN。
# 将预先划分好的测试数据、验证数据和训练数据分别预处理为 token 列表。
test_tokens = [char_to_token[c] for c in open(&#x27;data/test.txt&#x27;, &#x27;r&#x27;).read()]
val_tokens = [char_to_token[c] for c in open(&#x27;data/val.txt&#x27;, &#x27;r&#x27;).read()]
train_tokens = [char_to_token[c] for c in open(&#x27;data/train.txt&#x27;, &#x27;r&#x27;).read()]
```

### （二）模型和优化器的创建

根据指定的参数创建模型实例，并初始化优化器。

代码如下：

```
# 设置模型参数：上下文长度 context_length、嵌入层大小 embedding_size 和隐藏层大小 hidden_size。
context_length = 3 # if 3 tokens predict the 4th, this is a 4-gram model
embedding_size = 48
hidden_size = 512
# 创建随机数生成器 init_rng 并设置种子 1337。
init_rng = RNG(1337)

# 创建模型实例 MLPRaw 或 MLP。这里选择了 MLPRaw，即手动实现的模型版本。
model = MLPRaw(vocab_size, context_length, embedding_size, hidden_size, init_rng)
# model = MLP(vocab_size, context_length, embedding_size, hidden_size, init_rng)

learning_rate = 7e-4 # 设置学习率 learning_rate。
optimizer = torch.optim.AdamW(model.parameters(), lr=learning_rate, weight_decay=1e-4) # 创建优化器 AdamW，并指定模型参数、学习率和权重衰减率。
```

### （三）训练配置和数据加载器初始化

配置训练参数（如批次大小和训练步数），创建数据加载器。

```
timer = StepTimer() # 创建计时器。
batch_size = 128 # 批次大小。
num_steps = 50000 # 训练步数。
print(f&#x27;num_steps {num_steps}, num_epochs {num_steps * batch_size / len(train_tokens):.2f}&#x27;) # 打印训练步数和相应的训练周期数。
train_data_iter = dataloader(train_tokens, context_length, batch_size) # 创建数据加载器
```

### （四）训练循环

执行训练步骤，包括学习率调度、前向传播、计算损失、反向传播、更新模型参数和定期评估

```
for step in range(num_steps):
    # 使用余弦退火算法来调整学习率。
    lr = learning_rate * 0.5 * (1 + math.cos(math.pi * step / num_steps))
    # 遍历优化器中的所有参数组，更新学习率。
    for param_group in optimizer.param_groups:
        param_group[&#x27;lr&#x27;] = lr
    # 每隔 200 步或在最后一步评估一次验证损失。
    last_step = step == num_steps - 1
    if step % 200 == 0 or last_step:    
        # 调用 eval_split 函数评估训练数据和验证数据的损失。
        train_loss = eval_split(model, train_tokens, max_batches=20)
        val_loss = eval_split(model, val_tokens)
        print(f&#x27;step {step:6d} | train_loss {train_loss:.6f} | val_loss {val_loss:.6f} | lr {lr:e} | time/step {timer.get_dt()*1000:.4f}ms&#x27;)
    # 使用计时器 timer 记录所需时间。
    with timer:
        # 获取下一个训练数据批次 inputs 和 targets。
        inputs, targets = next(train_data_iter)
        # 前向传播，计算损失 
        logits, loss = model(inputs, targets)
        # 反向传播，计算梯度 
        loss.backward()
        # 更新模型参数
        optimizer.step()
        # 梯度清零
        optimizer.zero_grad()
```

上面👆代码中用到的 `eval_split` 函数定义如下：

```
@torch.inference_mode() # 使用推理模式禁用梯度计算，以提高推理速度和减少内存消耗。
def eval_split(model, tokens, max_batches=None):
    total_loss = 0
    num_batches = len(tokens) // batch_size
    if max_batches is not None:
        num_batches = min(num_batches, max_batches)
    data_iter = dataloader(tokens, context_length, batch_size)
    for _ in range(num_batches):
        inputs, targets = next(data_iter)
        logits, loss = model(inputs, targets)
        total_loss += loss.item() # loss.item() 将损失从张量转换为 Python 标量。
    mean_loss = total_loss / num_batches # 计算平均损失
    return mean_loss
```

## 模型推理

在测试集上使用模型进行预测和损失计算。
