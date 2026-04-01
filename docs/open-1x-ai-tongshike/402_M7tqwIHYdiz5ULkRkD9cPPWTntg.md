# （选修）实践任务：HW2(DNN)深度神经网络-分类任务

> 来源：[https://datawhaler.feishu.cn/docx/MUYpdYCKHoswk7xHkB7cesjQnvh](https://datawhaler.feishu.cn/docx/MUYpdYCKHoswk7xHkB7cesjQnvh)

> **《深度学习详解》学习活动实践**** **是 Datawh**ale 2024 年 AI ****夏令**营第五期**的学习活动（李宏毅-深度学习详**解-入门班方向），基于李宏毅机器学习2023年春课程开展的实践学习，其中本文内容为Homework2 DNN的内容——

+   适合想 **入门并****动手****实践****深度学习****算法、****通过实践理解深度学习理论**** **的学习者参与

>

> **学习内容提要**：基于自己构建的数据处理函数和深度学习算法DNN解决HW2中音素分类任务并通过修改模型结构和参数深入理解深度学习算法中各模块的作用。

> **纸质版教材指路（享五折优惠~）****：**https://u.jd.com/muCV8tI

内容速览

# 15分钟完成HW2作业！

> **教程贡献者说：**

> 我们整个学习活动，将带你

+   **跑通****最简单****的Baseline**

+   **了解**Baseline各部分功能与作用

+   **精读Baseline与****了解进阶思路**

> 这些一个都不会少！

+   **千里之行**，始于足下，从这里，开启你的 深度学习 学习之旅吧！

+   记得在每次完成学习后，写笔记打卡哟~ 及时总结沉淀能让学习事半功倍~

## [Step1：准备算力实例](https://university.aliyun.com/mobile?userCode=1h9ofupt)

<https://datawhaler.feishu.cn/sync/EK3adf2v3sULohbTxe8c2McXnRe>

## Step2：下载数据集并解压

> 链接：git clone https://oauth2:3EQxRxxHC8AwoQfojKpK@www.modelscope.cn/datasets/Datawhale/HW2-DNN-libriphone.git

> **启动创建的实例，等待几分钟直到启动完成，然后点击进入JupyterLab。接着，点击“Terminal**”打开命令行窗口，输入以下代码并按下回车键。稍等片刻，数据集和代码文件（notebook）将会自动下载，大约需要一分钟。

> git clone https://oauth2:3EQxRxxHC8AwoQfojKpK@www.modelscope.cn/datasets/Datawhale/HW2-DNN-libriphone.git

> **解压数据集命令：**

> cd HW2-DNN-libriphone/

> ls

> unzip -q ml2023spring-hw2.zip

![](img/6748d5967b2338073159511429a416a9.png)

![](img/92d9bccb2a08f8640c0431e4d0bb91f8.png)

![](img/7386b686102b5156a4f4cded8491bae1.png)

![](img/fcf5691839a16d1a8213e958b8f0ee3c.png)

## [Step3：](https://www.modelscope.cn/my/mynotebook/preset)15分钟体验一站式baseline教程！

> **点击打开****HW2**-DNN-libriphone文件夹

> **点击打开****HW3-DNN.ipynb****文件**

![](img/e242dd7a0d76546db31726abc005132d.png)

![](img/8171d6599cf14322b28499f798b08e66.png)

![](img/8d4bdf8c08eb05bbaff4425ca3bc6217.png)

> 等待几分钟后即可完成训练与模型验证，可以通过单元格（cell）查看模型在测试集上的分类准确率。生成的prediction.csv即是模型在测试集上的分类结果。

![](img/cd01e9d089cbf61b0fcfb72e6f8f4727.png)

![](img/a93db7392ca1ae21fc75bed1d1c923ec.png)

训练完成后需要点击停止实例，关闭后下次需要用还可以打开，以防在后台一直扣费。

![](img/637e700d13bb13a95cabafdfa6023b7a.png)

![](img/e9e96090a3bab9f6fb9575c27b24ab95.png)

![](img/b53ad9d5fab2971cab09924adff121c3.png)

## Step4：精读baseline！

> **代码详解**

> 该作业是利用深度神经网络（DNNs）来解决音素分类问题的一个范例。

> [音素](https://baike.baidu.com/item/%E9%9F%B3%E7%B4%A0/5689615?fromModule=lemma_inlink)：人说话的声音是由若干单个的音组成的，即使是一个很短的字、词也是由一定的读音组成的。英语把组成一个读音的最小单位叫音素。

> 音素分类：我们输入到计算机中的语音数据由多个帧构成，在该数据集中，1秒钟的语音数据就包含有100帧（frames），每个音素由不同数量的帧组成。在该任务中，我们在数据预处理阶段，由于单个帧只有0.25ms数据不太可能表示一个完整的音素，因此我们需要连接目标帧前后各N个帧作为一个音素样本，其中目标帧的标签就是整个样本的标签。同时，该数据集已经通过频率倒谱系数Mel-Frequency Cepstral Coefficients（MFCCs）提取出每个音素样本数据的特征置入到数据集中。该作业就是利用DNNs对测试集中的每个音素样本进行分类，判断其是哪种音素元素。

> DNNs：DNNs（Deep Neural Network，深度神经网络）是最基础的深度学习模型，其通过堆叠多个神经网络层实现以往单个或几个神经网络层无法使用的强大能力。它们设计用来模仿人类大脑的处理方式，通过多层（即“深度”）的神经元结构处理数据，从而解决各种复杂的数据驱动问题。这些网络通过多个隐藏层连接输入和输出层，每层都包含多个神经元，这些神经元通过激活函数处理信号。

> 本次HW2作业的baseline主要分为：

1.  **导入库；**

1.  **数据准备与预处理；**

1.  **定义模型；**

1.  定义损失函数和优化器等其他配置；

1.  **训练模型与评估模型；**

1.  **进行预测。**

#### 导入所需要的库/工具包

包括以下代码在内的代码都是在导入所需要的库或工具包

```
#引入 Numpy工具，pytorch工具，和随机数工具random
import numpy as np
import torch
import random
#引入提供与操作系统交互的接口的标准库os
import os
#引入在终端显示进度条的tqdm库
from tqdm import tqdm
from torch.utils.data import Dataset
import torch.nn as nn
from torch.utils.data import DataLoader
#引入垃圾回收机制库
import gc
```

#### 数据准备与预处理

固定随机种子，可以使得我们接下来训练的模型保持统一的初始参数，从而使得每次训练完成的模型性能保持一致。

```
def same_seeds(seed):
    #为random,numpy,pytorch等设置固定下来的随机种子
    random.seed(seed) 
    np.random.seed(seed)  
    torch.manual_seed(seed)
    if torch.cuda.is_available():
        torch.cuda.manual_seed(seed)
        torch.cuda.manual_seed_all(seed) 
    #确保在使用CUDA时，运算更具有确定性，以增强实验结果的可重复性
    torch.backends.cudnn.benchmark = False
    torch.backends.cudnn.deterministic = True
```

通常来说，表示一个音素需要多个帧，且与其前后帧的存在息息相关，因此在进行训练集/测试集划分前，我们需要将目标帧相邻的音素连接起来作为一个样本，来提高预测的准确性。这里我们通过def concat_feat()函数将我们指定的左右各k帧（k值将在后面由训练者自己定义）连接成一个样本，而预测中间的第k+1帧，因此每个样本的总长度是n=2k+1。随后我们使用def preprocess_data()函数将数据集切分为训练集和测试集。

> 在预处理阶段我们可以加入很多自己的手段，如剔除噪声、信号变换、归一化、正则化和其他表示学习方法等等，但要注意的是，在该任务中处理音素这类数据时，尽量不要改变帧的总数，不然会使后续的训练出错。

```
def load_feat(path):
#加载数据
    feat = torch.load(path)
    return feat
    
#依次根据目标帧获得指定数量的左右帧并连接它们
def shift(x, n):
    if n &lt; 0:
        left = x[0].repeat(-n, 1)
        right = x[:n]
    elif n &gt; 0:
        right = x[-1].repeat(n, 1)
        left = x[n:]
    else:
        return x

    return torch.cat((left, right), dim=0)

#连接目标帧前后帧作为样本
def concat_feat(x, concat_n):
    assert concat_n % 2 == 1 # n 必须为奇数，因为我们需要预测中间帧，而前后帧数一致，所以每个样本都会是奇数帧。
    if concat_n &lt; 2:
        return x
    seq_len, feature_dim = x.size(0), x.size(1)
    x = x.repeat(1, concat_n) 
    x = x.view(seq_len, concat_n, feature_dim).permute(1, 0, 2) # concat_n, seq_len, feature_dim
    mid = (concat_n // 2)
    for r_idx in range(1, mid+1):
        x[mid + r_idx, :] = shift(x[mid + r_idx], r_idx)
        x[mid - r_idx, :] = shift(x[mid - r_idx], -r_idx)

    return x.permute(1, 0, 2).view(seq_len, concat_n * feature_dim)

#切分数据集为训练集和测试集
def preprocess_data(split, feat_dir, phone_path, concat_nframes, train_ratio=0.8, random_seed=1213):
    class_num = 41 # NOTE:这些参数包括类别数是预先定义的，不可以被修改。
    if split == &#x27;train&#x27; or split == &#x27;val&#x27;:
        mode = &#x27;train&#x27;
    elif split == &#x27;test&#x27;:
        mode = &#x27;test&#x27;
    else:
        raise ValueError(&#x27;Invalid \&#x27;split\&#x27; argument for dataset: PhoneDataset!&#x27;)

    label_dict = {}
    # 从文件中获取训练集和验证集数据
    if mode == &#x27;train&#x27;:
        for line in open(os.path.join(phone_path, f&#x27;{mode}_labels.txt&#x27;)).readlines():
            line = line.strip(&#x27;\n&#x27;).split(&#x27; &#x27;)
            label_dict[line[0]] = [int(p) for p in line[1:]]
        
        # 切分训练集和验证集数据
        usage_list = open(os.path.join(phone_path, &#x27;train_split.txt&#x27;)).readlines()
        random.seed(random_seed)
            #打乱样本，防止模型记住样本顺序
        random.shuffle(usage_list)
        train_len = int(len(usage_list) * train_ratio)
        usage_list = usage_list[:train_len] if split == &#x27;train&#x27; else usage_list[train_len:]
    
    # 从文件中获取测试集数据
    elif mode == &#x27;test&#x27;:
        usage_list = open(os.path.join(phone_path, &#x27;test_split.txt&#x27;)).readlines()

    usage_list = [line.strip(&#x27;\n&#x27;) for line in usage_list]
    print(&#x27;[Dataset] - # phone classes: &#x27; + str(class_num) + &#x27;, number of utterances for &#x27; + split + &#x27;: &#x27; + str(len(usage_list)))

    max_len = 3000000
    X = torch.empty(max_len, 39 * concat_nframes)
    if mode == &#x27;train&#x27;:
        y = torch.empty(max_len, dtype=torch.long)

    idx = 0
    for i, fname in tqdm(enumerate(usage_list)):
        feat = load_feat(os.path.join(feat_dir, mode, f&#x27;{fname}.pt&#x27;))
        cur_len = len(feat)
        feat = concat_feat(feat, concat_nframes)
        if mode == &#x27;train&#x27;:
            label = torch.LongTensor(label_dict[fname])

        X[idx: idx + cur_len, :] = feat
        if mode == &#x27;train&#x27;:
            y[idx: idx + cur_len] = label

        idx += cur_len

    X = X[:idx, :]
    if mode == &#x27;train&#x27;:
        y = y[:idx]

    print(f&#x27;[INFO] {split} set&#x27;)
    print(X.shape)
    if mode == &#x27;train&#x27;:
        print(y.shape)
        return X, y
    else:
        return X
```

这段代码主要是在原始数据处理完及划分完训练集和测试集后，利用Pytorch库的Dataset类实现一个加载音素数据的类，在该类中我们可以对数据进行进一步的处理，并获取训练集或测试集的部分信息，同时加载给后续的DataLoader使用。

```
class LibriDataset(Dataset):
    def __init__(self, X, y=None):
        self.data = X
        #将label值转化为LongTensor格式以便输入到其他方法中计算
        if y is not None:
            self.label = torch.LongTensor(y)
        else:
            self.label = None
    #获取每个样本及其标签
    def __getitem__(self, idx):
        if self.label is not None:
            return self.data[idx], self.label[idx]
        else:
            return self.data[idx]
    #获取数据集的长度
    def __len__(self):
        return len(self.data)
```

在以下代码中我们通过继承Pytorch中DataLoader类实现了自己的训练集加载类（train_loader）和验证集加载类（val_loader），加载类可以决定批量加载大小、是否打乱数据顺序、是否使用多线程加载以及是否固定内存地址等。同时以下代码还对刚才定义的固定种子函数、预处理函数、Dataset函数进行了实现，正式开始读取数据并进行以上数据处理和加载。

> 验证集是从训练集中划分出来的一部分数据集，它可以用于调整模型的超参数和用于对模型的能力进行初步评估。 通常用来在模型迭代训练时，用以验证当前模型泛化能力（准确率，召回率等），以决定是否停止继续训练。其和测试集是不一样的。

```
same_seeds(seed)
device = &#x27;cuda&#x27; if torch.cuda.is_available() else &#x27;cpu&#x27;
print(f&#x27;DEVICE: {device}&#x27;)

# 调用刚才定义的预处理函数对数据进行预处理
train_X, train_y = preprocess_data(split=&#x27;train&#x27;, feat_dir=&#x27;./libriphone/feat&#x27;, phone_path=&#x27;./libriphone&#x27;, concat_nframes=concat_nframes, train_ratio=train_ratio, random_seed=seed)
val_X, val_y = preprocess_data(split=&#x27;val&#x27;, feat_dir=&#x27;./libriphone/feat&#x27;, phone_path=&#x27;./libriphone&#x27;, concat_nframes=concat_nframes, train_ratio=train_ratio, random_seed=seed)

# 调用刚才定义的Dataset函数获取数据
train_set = LibriDataset(train_X, train_y)
val_set = LibriDataset(val_X, val_y)

# 调用垃圾回收函数删除已经使用过的train_X, train_y, val_X, val_y以节省内存
gc.collect()

# 定义数据加载类，其用于给模型提供以样本划分的数据及其类别标签
train_loader = DataLoader(train_set, batch_size=batch_size, shuffle=True)
val_loader = DataLoader(val_set, batch_size=batch_size, shuffle=False)
```

#### 定义模型

这段代码正式定义了两个模型的部分，它们都继承了PyTorch的nn.Module类，包含了定义一个神经网络层或者模块所必须的基础方法（如线性层和激活函数等）。

其中一个是 BasicBlock，是用于堆叠的基础神经网络块，它的结构（由 def __init__()函数定义）只包括了一个线性层和ReLU激活函数，它的数据流操作（由def forward()函数定义）只包括让输入数据经过该线性层和激活函数随后输出。

```
class BasicBlock(nn.Module):
    #定义模块结构
    def __init__(self, input_dim, output_dim):
        super(BasicBlock, self).__init__()

        # TODO: 你可以应用批归一化和dropout方法来获得更好的模型性能，这部分内容将在Step 5进行介绍
        # Reference: https://pytorch.org/docs/stable/generated/torch.nn.BatchNorm1d.html (batch normalization)
        #       https://pytorch.org/docs/stable/generated/torch.nn.Dropout.html (dropout)
        self.block = nn.Sequential(
        # 利用nn.Sequential函数将内部的结构固定为一个序列（Sequential），可以理解为封装
            #一个线性层
            nn.Linear(input_dim, output_dim),
            #一个激活函数
            nn.ReLU(),
        )
    #数据流操作
    def forward(self, x):
        #输入数据经过定义的block块并输出结果
        x = self.block(x)
        return x

```

另一个是Classifier，其是基于刚才定义的BasicBlock神经网络块进行堆叠，它的结构（由 def __init__()函数定义）包含了多个BasicBlock神经网络块和一个线性层用于将中间层权重输出为多个类别的权重来分类，它的数据流操作（由def forward()函数定义）只包括让输入数据经过这些BasicBlock模块和线性层输出分类结果。

```
class Classifier(nn.Module):
    #定义模块结构
    def __init__(self, input_dim, output_dim=41, hidden_layers=1, hidden_dim=256):
        super(Classifier, self).__init__()
        #通过调用刚才定义的序列进行叠加形成深度神经网络层，堆叠数由训练者定义的hidden_layers参数决定
        self.fc = nn.Sequential(
            BasicBlock(input_dim, hidden_dim),
            *[BasicBlock(hidden_dim, hidden_dim) for _ in range(hidden_layers)],
            #线性层用于将以上模块所输出的中间层参数处理为每个类别的权重，用于后续分类
            nn.Linear(hidden_dim, output_dim)
        )

    def forward(self, x):
        x = self.fc(x)
        return x
```

#### **定义损失函数和优化器等其他配置**

这段代码实现了音素分类模型的初始化和训练配置，目的是准备好训练环境和参数。它选择合适的设备（GPU或CPU），设置模型、批量大小、训练轮数、提前停止策略，定义了损失函数和优化器，为后续的模型训练奠定了基础。

```
# 数据集参数 TODO:你可以修改concat_nframes来看不同的分类效果
concat_nframes = 3   # 我们所定义的目标帧前后连接的帧数，连接后的帧数必须为奇数
train_ratio = 0.75   # 用于训练集的数据比例，其余数据将用于验证集

# 训练参数
seed = 1213          # 随机种子数
batch_size = 512        # 每批次输入的数据量大小，即批次大小
num_epoch = 10         # 训练的轮次数
learning_rate = 1e-4      # 定义学习率，决定了参数更新的快慢
model_path = &#x27;./model.ckpt&#x27;  # 模型训练的结果权重存储地点

# 模型参数
# TODO: 你可以更改&quot;hidden_layers&quot; 或 &quot;hidden_dim&quot; 来观察不同的隐藏层数量或隐藏层宽度对预测结果的影响
input_dim = 39 * concat_nframes  # 数据输入模型时的维度数，这里不可以修改
hidden_layers = 2          # 隐藏层的数量
hidden_dim = 64           # 隐藏层的维度宽度

#根据上面定义的模型类实现一个实例模型，同时定义所需的损失函数（这里是交叉熵损失函数）和优化器（这里是Adam优化器）
model = Classifier(input_dim=input_dim, hidden_layers=hidden_layers, hidden_dim=hidden_dim).to(device)
criterion = nn.CrossEntropyLoss() 
optimizer = torch.optim.Adam(model.parameters(), lr=learning_rate)
```

#### 训练模型与评估模型

这段代码实现了一个图像分类模型的训练和验证循环，目的是通过多轮训练（epochs）逐步优化模型的参数，以提高其在验证集上的性能，并保存效果最好的模型。训练阶段通过前向传播、计算损失、反向传播和参数更新来优化模型，验证阶段评估模型在未见过的数据上的表现。如果验证集的准确率超过了之前的最好成绩，保存当前模型，并在连续多轮验证性能未提升时（该数量由早停数决定，如果没有设置早停数则会训练到最大轮次数）提前停止训练。

训练完成后，需要在测试集上评估模型的性能。通过计算准确率来衡量模型在测试集上的表现（见第6步）。

```
#开始初始化训练参数，这些参数不能改动
best_acc = 0.0
for epoch in range(num_epoch):
    train_acc = 0.0
    train_loss = 0.0
    val_acc = 0.0
    val_loss = 0.0
    
    # 训练中
    model.train() # 这里是使用Pytorch中刚才继承的nn.Module中的train()方法确保模型处于训练模式
    for i, batch in enumerate(tqdm(train_loader)):#tqdm仅用于展示训练数据的进度
        #获取每个批次应有的数据及其对应的标签并将它们放在同一设备上
        features, labels = batch
        features = features.to(device)
        labels = labels.to(device)
        
        # 清除上一步中参数中存储的梯度
        optimizer.zero_grad() 
        # 前向传播数据，将数据输入实现的模型实例输出结果（确保数据和模型位于同一设备上）
        outputs = model(features) 
        
        #计算交叉熵损失。
        #在计算交叉熵之前不需要应用softmax，因为它会自动完成。
        loss = criterion(outputs, labels)
        # 通过损失值计算应当下降的参数梯度
        loss.backward() 
        # 使用计算出的参数梯度更新参数
        optimizer.step() 
        
        #根据模型输出的多个类别的权重获取概率（权重）最高的类别的索引，作为预测结果进行输出
        _, train_pred = torch.max(outputs, 1) 
        #记录损失和准确率
        train_acc += (train_pred.detach() == labels.detach()).sum().item()
        train_loss += loss.item()
    
    # 验证中
    model.eval() # 确保模型处于评估模式，以便某些模块如dropout能够正常工作
        # 我们在验证阶段不需要更新梯度来更新参数。
        # 使用 torch.no_grad() 加速前向传播过程
    with torch.no_grad():
        for i, batch in enumerate(tqdm(val_loader)):
            features, labels = batch
            features = features.to(device)
            labels = labels.to(device)
            outputs = model(features)
            
            #我们仍然可以计算损失用于观察该次训练的效果。
            loss = criterion(outputs, labels) 
            
            #记录损失和准确率
            _, val_pred = torch.max(outputs, 1) 
            val_acc += (val_pred.cpu() == labels.cpu()).sum().item() # get the index of the class with the highest probability
            val_loss += loss.item()

    #打印该轮次训练在验证集上的结果
    print(f&#x27;[{epoch+1:03d}/{num_epoch:03d}] Train Acc: {train_acc/len(train_set):3.5f} Loss: {train_loss/len(train_loader):3.5f} | Val Acc: {val_acc/len(val_set):3.5f} loss: {val_loss/len(val_loader):3.5f}&#x27;)

    # 如果模型在该轮次得到了改进，就保存该轮次训练后的模型参数
    # 如果相比上一轮次训练没有改进就不会保存该次训练的结果
    if val_acc &gt; best_acc:
        best_acc = val_acc
        torch.save(model.state_dict(), model_path)
        print(f&#x27;saving model with acc {best_acc/len(val_set):.5f}&#x27;)
```

#### **进行预测**

最后的代码块用于构建一个测试数据集和测试集DataLoader，以便高效地读取数据。实例化并加载预训练的分类器模型，并将其设置为评估模式。在不更新梯度及模型参数的情况下，遍历测试数据，使用训练完成后保存的模型进行预测，并将预测标签存储在列表中。将预测结果与测试集的ID生成一个DataFrame，并将其保存为prediction.csv。

```
# 构建测试数据集，同样需要预处理并构建测试集Dataset类和测试集DataLoader类来处理和读取各个测试集的样本
test_X = preprocess_data(split=&#x27;test&#x27;, feat_dir=&#x27;./libriphone/feat&#x27;, phone_path=&#x27;./libriphone&#x27;, concat_nframes=concat_nframes)
test_set = LibriDataset(test_X, None)
test_loader = DataLoader(test_set, batch_size=batch_size, shuffle=False)
```

```
# 加载刚才保存的训练时最优的模型参数到我们新定义的模型类中，
# 加载模型参数需要保证新构建的模型架构与保存的模型架构一致，否则会出错
model = Classifier(input_dim=input_dim, hidden_layers=hidden_layers, hidden_dim=hidden_dim).to(device)
model.load_state_dict(torch.load(model_path))
```

```
# 初始化一个空array，用于存储所有预测结果
pred = np.array([], dtype=np.int32)
# 同样是使模型进入验证模式
model.eval()
    # 我们在验证阶段不需要更新梯度来更新参数。
    # 使用 torch.no_grad() 加速前向传播过程
with torch.no_grad():
    for i, batch in enumerate(tqdm(test_loader)):
        # 按批次加载测试样本及其标签，并将它们放到同一个设备上用于计算
        features = batch
        features = features.to(device)
        # 将测试样本输入到训练好的模型中，输出分类结果
        outputs = model(features)
        
        #根据模型输出的多个类别的权重获取概率（权重）最高的类别的索引，作为预测结果进行输出
        _, test_pred = torch.max(outputs, 1)
        pred = np.concatenate((pred, test_pred.cpu().numpy()), axis=0)

```

```
#根据保存的预测csv文件打印出预测结果
with open(&#x27;prediction.csv&#x27;, &#x27;w&#x27;) as f:
    f.write(&#x27;Id,Class\n&#x27;)
    for i, y in enumerate(pred):
        f.write(&#x27;{},{}\n&#x27;.format(i, y))
```

# 🎉 恭喜你完成了选修实践作业！

## 👍🏻 快来给自己点赞！

## 拓展你专属的DNN分类模型

> 在前面的工作中，我们通过学习和定义了一个非常简单的深度神经网络完成了音素分类任务。

> 但是，即使是最简单的深度神经网络（DNNs），我们也可以通过增加一些模块和处理提高其在分类任务上的性能和稳定性。

> 接下来介绍的模块代码，学习者们可以通过复制这些模块替换代码中原有的模块，体验拓展baseline带来的效果变化，最终定制一个专属于你的DNN分类模型！

#### 拓展数据预处理阶段

在数据预处理阶段，除了上面已有的数据预处理操作，还可以加入一些自己的数据预处理操作，如去除声音信号中的噪声等。

在下面代码中，我们尝试对读取的数据施加基于FIR滤波器的去噪功能

首先在终端中安装scipy库

```
pip install scipy
```

随后在load_feat函数之前增加去噪函数

```
**#导入这**个库
from scipy.**signal**** im**port **firwin****, l**f**ilter

de**f de**noise_**with_fir(feat, cutoff=0.2, fs=1.0, n_taps=101):
    &quot;&quot;&quot;
    使用FIR滤波器去除高频噪声
    
    Parameters:
    - feat: (seq_len, feature_dim).
    - cutoff: 低通滤波器的截断频率.
    - fs: 采样频率.
    - n_taps: FIR滤波器的长度，必须为奇数
    
    Returns:
    - 过滤后的信号
    &quot;&quot;&quot;
    # 设定 FIR 滤波器的系数
    b = firwin(n_taps, cutoff, nyq=fs / 2)
    # 对输入数据进行去噪
    feat_filtered = lfilter(b, 1.0, feat.numpy(), axis=0)
    return torch.tensor(feat_filtered, dtype=torch.float32)

# 修改 load_feat 函数以包含去噪
def load_feat(path):
    feat = torch.load(path)
    feat = denoise_with_fir(feat)
    return feat
```

大家可以尝试修改去噪的参数或者引入不同的去噪方法(如H-P滤波器)来观察不同参数不同方法下去噪对分类结果的影响。

#### 拓展分类模型结构

在baseline中我们设计的DNNs结构非常简单，其基础块仅由线性层和激活函数堆叠而成。

事实上，我们还可以通过在基础块中调整激活函数、增加批归一化方法（Batch Normalization）和dropout方法来获得更好的模型性能。

在下面代码中，我们首先通过替换激活函数实现了新的DNNs模型，同时在其中增加了批归一化方法和dropout方法来提高其性能同时避免出现过拟合现象：

```
class BasicBlock(nn.Module):
    #定义模块结构
    def __init__(self, input_dim, output_dim):
        super(BasicBlock, self).__init__()

        # TODO: 你可以应用批归一化和dropout方法来获得更好的模型性能，这部分内容将在Step 5进行介绍
        # Reference: https://pytorch.org/docs/stable/generated/torch.nn.BatchNorm1d.html (batch normalization)
        #       https://pytorch.org/docs/stable/generated/torch.nn.Dropout.html (dropout)
        self.block = nn.Sequential(
        # 利用nn.Sequential函数将内部的结构固定为一个序列（Sequential），可以理解为封装
            #一个线性层
            nn.Linear(input_dim, output_dim),
            nn.BatchNorm1d(output_dim),  # 批归一化层
            #一个激活函数
            
            nn.Dropout(0.1)  # Dropout层，你可以调整其参数从0-1之间
        )
    #数据流操作
    def forward(self, x):
        #输入数据经过定义的block块并输出结果
        x = self.block(x)
        x = F.gelu(x), #替换ReLU激活函数
        return x

```

你可以用新的BasicBlock模块替代原始的BasicBlock模块，并观察这些方法分别对模型的预测精度和损失函数值产生了什么影响。

> 尝试使用不同的激活函数或增加/减少激活函数的总数量并观察其带来的变化。

> 尝试使用不同的dropout率并观察其对模型预测精度的影响。

> 激活**函数：是一种添加到深度**神经网络中**的函数，旨在帮助网络学****习数据中**的**复杂模式**。类似于人类大脑中基于神经元的模型，激活函数最终决定了要发射给下一个神经元的内容。其主要有两种作用：一是将神经元的输出值限制在一定的范围内，二是将非线性添加到神经网络中，而非线性决定了深度神经网络可以实现复杂的功能和拟合计算。ReLU是一种常用的激活函数，即修正线性单元，其数学表达式为：f(x) = max(0,x)，其可以很好地解决梯度消失的问题。而我们替换其所用的GeLU，则是另一种常用的基于高斯误差函数的激活函数。相较于 ReLU 等激活函数，GELU 更加平滑，有助于提高训练过程的收敛速度和性能。其表达式为GELU(x)=x∗P(X⩽x)=x∗Φ(x)。

> 批归一化：是一种在深度学习**中广泛**使用的**技术**，旨在通过归一化每一批数据的分布来加速神经网络的训练**并提高模**型的性**能。它由Goog**le**在2015年提出，**并迅速成为几乎所有卷积神经网络的标配技巧。其主要作用包括：1.加速训练；2.缓解梯度消失问题；3.提高模型泛化能力。

> **Dr**opout：是一种在深度**神经网**络中常用的正则化技术，旨在防止神经网络在训练过程中出现过拟合现象。该方法通过随机丢弃（或称为“丢弃”）神经网络中的一部分神经元及其连接，从而减少神经网络模型的复杂性，增加其泛化能力。具体来说，Dropout方法在训练阶段随机选择一部分神经元，将其输出设置为0，使其不参与前向和反向传播过程。这样做可以防止模型对训练数据中的噪声和特定特征过度拟合，从而提高模型在未见过的数据（测试数据）上的性能。

![](img/fbeb92e33da94821405464b21199ec3e.png)

## 参考资料

> 此处还可以放一些写作的参考资料，供学习者延伸阅读，另外也可以上传一些论文

## 更多QA详见[ QA & 其他彩蛋](https://datawhaler.feishu.cn/wiki/LTIMwCerZiF2nSkL1mVcTZsrnN1)！

> [ QA & 其他彩蛋](https://datawhaler.feishu.cn/wiki/LTIMwCerZiF2nSkL1mVcTZsrnN1)任何想法都可以在学习群里交流或在 进行填写哟~

>

> 本教程主要由    写作贡献，

> 融合了多位助教的内测体验和宝贵建议。

> 如果你觉得对你有价值和启发，请在下面点赞和评论哟~

>

### 另外别忘了写笔记打卡呀！！~

### 及时思考、沉淀和产出能让你的学习事半功倍！

>

> **注：本文采用 ****署名—非商业性使用**—相同方式共享 4.0 协议国际版许可协议进行许可。