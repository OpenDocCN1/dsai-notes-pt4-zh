
```python
def clones(module, N):
    """克隆模型结构"""
    #参数module指要克隆的模型，N指要克隆的份数
    #clones函数将某个结构深拷贝若干份，并将以列表的形式存入ModuleList中，方便后续调用    
    return nn.ModuleList([copy.deepcopy(module) for _ in range(N)])  

class Encoder(nn.Module):
    def __init__(self, layer, N):
        super(Encoder, self).__init__()
        #我们将N个克隆层叠加在一起组成完整的Encoder
        self.layers = clones(layer, N)
        #在编码器中使用LayerNorm进行正则化
        self.norm = LayerNorm(layer.size)


    def forward(self, x, mask):
        """依次将输入和掩码传递到每个层"""
        for layer in self.layers:
            x = layer(x, mask)
        return self.norm(x)

class EncoderLayer(nn.Module):
    """编码器由self-attention层和feed_forward层组成"""
    def __init__(self, size, self_attn, feed_forward, dropout):
        super(EncoderLayer, self).__init__()
        self.self_attn = self_attn
        self.feed_forward = feed_forward
        self.sublayer = clones(SublayerConnection(size, dropout), 2) #之所以克隆两个层是因为self-attention层和feed_forward层都需要残差连接
        self.size = size

    def forward(self, x, mask):
        """Follow Figure 1 (left) for connections."""
        x = self.sublayer[0](x, lambda x: self.self_attn(x, x, x, mask)) #多头self-attention
        return self.sublayer[1](x, self.feed_forward) #前向传播

def Scaled_Dot_product_Attention(query, key, value, mask=None, dropout=None):
    #首先取query的最后一维的大小，对应词嵌入维度
    d_k = query.size(-1)
    #按照注意力公式，将query与key的转置相乘，这里面key是将最后两个维度进行转置，再除以缩放系数得到注意力得分张量scores
    scores = torch.matmul(query, key.transpose(-2, -1)) / math.sqrt(d_k)
    #判断是否使用掩码张量
    if mask is not None:
        #使用torch的masked_fill方法，主要是用来mask掉当前时刻后面时刻的序列信息
        #将掩码张量和scores张量每个位置一一比较，如果掩码张量不为空，则其对应的scores张量用-1e9这个置来替换
        scores = scores.masked_fill(mask == 0, -1e9)
    #对scores的最后一维进行softmax操作，使用F.softmax方法，这样获得最终的注意力张量
    p_attn = F.softmax(scores, dim = -1)
    #之后判断是否使用dropout对注意力进行随机置0
    if dropout is not None:
        p_attn = dropout(p_attn)
    #最后，根据公式将p_attn与value张量相乘获得最终的query注意力表示，同时返回注意力张量
    return torch.matmul(p_attn, value), p_attn

class LayerNorm(nn.Module):
    """对特征做正则化"""
    def __init__(self, features, eps=1e-6):
        #参数features表示词嵌入的维度,参数eps是一个足够小的数，在规范化公式的分母中出现,防止分母为0，默认是1e-6。
        super(LayerNorm, self).__init__()
        self.a_2 = nn.Parameter(torch.ones(features))
        self.b_2 = nn.Parameter(torch.zeros(features))
        self.eps = eps

    def forward(self, x):
        #对输入变量x求其最后一个维度的均值和标准差，并用keepdim=True将输出维度与输入维度保持一致
        #将标准化后的结果乘以我们的缩放参数a_2后再加上位移参数b_2。*号代表同型点乘，即对应位置进行乘法操作
        mean = x.mean(-1, keepdim=True)
        std = x.std(-1, keepdim=True)
        return self.a_2 * (x - mean) / (std + self.eps) + self.b_2

class Multi_Head_Attention(nn.Module):
    def __init__(self, dim_model, num_head, dropout):
        #参数h代表头数，d_model代表词嵌入的维度，dropout代表进行数据操作时置0比率，默认是0.1
        super(Multi_Head_Attention, self).__init__()
        #assert语句判断d_model是否能被h整除，这是因为我们之后要给每个头分配等量的词特征，也就是embedding_dim/head个
        assert dim_model % num_head == 0
        # We assume d_v always equals d_k
        #得到每个头获得的词向量维度d_k
        self.d_k = dim_model // num_head
        self.h = num_head
        #通过nn的Linear实例化创建linear层，它的内部变换矩阵大小是embedding_dim*embedding_dim
        #因为在多头注意力中，Q,K,V各需要一个线性层，最后拼接的矩阵还需要一个线性层，因此一共需要克隆四个
        self.linears = clones(nn.Linear(dim_model, dim_model), 4)
        #self.attn代表最后得到的注意力张量，现在还没有结果所以为None
        self.attn = None
        self.dropout = nn.Dropout(p=dropout)

    def forward(self, query, key, value, mask=None):
        """Implements Figure 2"""
        #前三个参数就是注意力机制需要的Q,K,V，最后一个参数是注意力机制中可能需要的mask掩码张量，默认是None 
        if mask is not None:
            # Same mask applied to all h heads.
            #使用unsqueeze扩展维度，代表多头中的第n头
            mask = mask.unsqueeze(1)
        #接着，我们获得一个batch_size的变量，他是query尺寸的第1个数字，代表有多少条样本
        nbatches = query.size(0)        
        # 1) Do all the linear projections in batch from d_model => h x d_k
        #首先利用zip函数和for循环将QKV分别传到线性层中，并分别为每个头做输入
        #view方法对线性变换的结构进行维度重塑，h代表头，d_k代表词向量维度，这样就意味着每个头可以获得一部分词特征组成的句子，
        #view中的-1代表自适应维度，计算机会根据这种变换自动计算这里的值，
        #然后对第二维和第三维进行转置操作，使得代表句子长度维度和词向量维度能够相邻，这样注意力机制才能找到词义与句子位置的关系，得到每个头的输入 
        query, key, value = [l(x).view(nbatches, -1, self.h, self.d_k).transpose(1, 2) for l, x in zip(self.linears, (query, key, value))]      
        # 2) Apply attention on all the projected vectors in batch. 
        #得到每个头的输入后，接下来就是将他们传入到attention中，这里直接调用我们之前实现的attention函数，同时也将mask和dropout传入其中                
        x, self.attn = Scaled_Dot_product_Attention(query, key, value, dropout=self.dropout)        
        # 3) "Concat" using a view and apply a final linear.      
        #为了方便后续的计算，我们要将每个头计算结果组成的4维张量转换为输入的形状
        #因此这里开始进行第一步处理环节的逆操作，先对第二和第三维进行转置，
        #然后使用contiguous方法将tensor变成在内存中连续分布的形式，从而让转置后的张量应用view方法，否则将无法直接使用
        #最后就是使用view重塑形状，变成和输入形状相同。   
        x = x.transpose(1, 2).contiguous().view(nbatches, -1, self.h * self.d_k)
        #使用线性层列表中的最后一个线性变换得到最终的多头注意力结构的输出
        return self.linears[-1](x)

def subsequent_mask(size):
    """Mask out subsequent positions."""
    #生成向后遮掩的掩码张量，参数size是掩码张量最后两个维度的大小，它最后两维形成一个方阵
    attn_shape = (1, size, size)
    #形成元素为1的上三角阵
    subsequent_mask = np.triu(np.ones(attn_shape), k=1).astype('uint8')
    #最后将numpy类型转化为torch中的tensor，内部做一个1- 的操作。这个其实是做了一个三角阵的反转，subsequent_mask中的每个元素都会被1减。
    #如果是0，subsequent_mask中的该位置由0变成1
    #如果是1，subsequect_mask中的该位置由1变成0
    return torch.from_numpy(subsequent_mask) == 0

class SublayerConnection(nn.Module):
    def __init__(self, size, dropout):
        super(SublayerConnection, self).__init__()
        self.norm = LayerNorm(size) #规范化
        self.dropout = nn.Dropout(dropout)

    def forward(self, x, sublayer):
        """“将剩余连接应用于具有相同大小的任何子层。”"""
        x = torch.tensor(x,dtype=torch.float)
        return x + self.dropout(sublayer(self.norm(x)))

class Position_wise_Feed_Forward(nn.Module):
    """Implements FFN equation."""
    def __init__(self, dim_model, hidden, dropout=0.1):
        #参数dim_model第一个是线性层的输入维度也是第二个线性层的输出维度，即通过前馈全连接层后输入和输出的维度不变，
        #第二个参数hidden就是第二个线性层的输入维度和第一个线性层的输出维度
        super(Position_wise_Feed_Forward, self).__init__()
        self.w_1 = nn.Linear(dim_model, hidden)
        self.w_2 = nn.Linear(hidden, dim_model)
        self.dropout = nn.Dropout(dropout)

    def forward(self, x):
        #输入参数为x，代表来自上一层的输出，首先经过第一个线性层，然后使用F中的relu函数进行激活，之后再使用dropout进行随机置0，最后通过第二个线性层w2，返回最终结果
        return self.w_2(self.dropout(F.relu(self.w_1(x))))
```

### Encode调用

```python
import copy

c = copy.deepcopy #定义深拷贝方法
num_head = 2
max_len = 9
dropout = 0.1
hidden = 2048
dim_model = 4

attn = Multi_Head_Attention(dim_model, num_head, dropout) #多头注意力层
ff = Position_wise_Feed_Forward(dim_model, hidden, dropout) #前向传播层
encode_mask = torch.ones(1, 1, 9) #encode端掩码
encode = Encoder(EncoderLayer(dim_model, c(attn), c(ff), dropout), N=2)  #完成了两次的堆叠
encode_output = encode(embeddings, src_mask)

print(encode_output)
print(f"encode_output.shape：{encode_output.shape}")
```

```
tensor([[[ 0.0230, 0.1577, -1.3065, 1.1259], [ 1.0587, 0.6489, -0.8586, -0.8490], [-0.7636, 0.1162, 1.3710, -0.7236], [-0.4283, -0.9035, 1.4120, -0.0802], [-0.1035, -0.3720, 1.4077, -0.9323], [-0.9910, -0.4046, 0.0345, 1.3612], [-1.1648, 0.7927, -0.5015, 0.8736], [-0.9640, 1.0525, -0.7367, 0.6483], [-0.7196, -0.8554, 0.2824, 1.2926]]], grad_fn=) encode_output.shape：torch.Size([1, 9, 4])
```

### 2.3 Decoder层

![](img/d9cccfea95286f55b71498e7e824d35f.png)

`Decoder`层位于transformer示意图的右边部分，一共包含3个部分的网络结构，分别是 Masked Multi-head attention 、 Multi-head attention 、 Feed-Forward 层，层与层之间使用了残差连接。使用的组件与`Encoder`并无太大区别

需要我们注意的是`Decoder`层里面有许多精巧的细节设计，图中这个与`Encoder`输出（`Memory`）进行交互的部分叫做 Encoder-Decoder attention 。 对于这部分的输入，`Q` 来自于`Decoder`下面多头注意力机制的输出，`K`和`V` 均是`Encoder` 部分的输出（`Memory`）经过 线性变换 后得到。以下是`Decoder`端的具体代码:
