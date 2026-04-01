# （选修实践任务）：HW4(Self-Attention)自注意力机制 -声音分类

> 来源：[https://datawhaler.feishu.cn/docx/VFUtdxeCIouzjgxSdcPc8rgXn5g](https://datawhaler.feishu.cn/docx/VFUtdxeCIouzjgxSdcPc8rgXn5g)

# 20分钟速通Homework4 Self-Attention

###### HW4主要学习Transformer的使用

###### Transformer模型的变形

###### Self-Attention Pooling

######  Additive Margin Softmax。

### 本文基于《深度学习详解》的Homework4 Self-attention的内容，为学习者提供了

### 20分钟一键运行的体验和代码的详细解读并注释。

> Ps. 纸质版教材指路（享五折优惠~）：https://u.jd.com/muCV8tI

## [Step1：准备算力](https://university.aliyun.com/mobile?userCode=1h9ofupt)

<https://datawhaler.feishu.cn/sync/EK3adf2v3sULohbTxe8c2McXnRe>

## Step2：一键运行Notebook 了解 HW4(Attention)

> 下载资源仓库  （复制如下命令，在终端中执行）

```
git clone https://oauth2:8eVBBp_6nzVi951ncAA3@www.modelscope.cn/datasets/Datawhale/LeeDL-HW4-self_attention.git
```

![](img/a94a49509d514b74ce5000592d087bd6.png)

![](img/13d95a328a1ad38e0ae8a14aa8ecbc6a.png)

![](img/1a01b8878c530eef545ab342911c4bbd.png)

![](img/f098b10e2a209bf158447e27631d67a3.png)

![](img/523640f667fe3af8751a39d01abf434b.png)

![](img/178e329682f61ed6eb7e9db9a06f922c.png)

![](img/6a5f7fa352e8c1aff94184026be5e4bf.png)

![](img/b9eb94b2b334a36fbf784c48e7d3d577.png)

## Step3：运行完成后记得停止实例

> 一定要记得【停止实例】，否则会一直扣费消耗代金券！！！！

![](img/bbd56727295bee2d0640f56185f52f7c.png)

![](img/3ac423025271f1f1222c8d5804d145be.png)

![](img/3211cecbce41dfc388e156b235dd0e29.png)

![](img/14b5deca044f1812f04cf89bd495ba7c.png)

# 代码详解

## ** 任务描述**

+   对给定特征的演讲者进行分类。

+   主要目标：学习如何使用Transformer。

+   基线：

+   简单：运行示例代码并知道如何使用Transformer。

+   中等：知道如何调整Transformer的参数。

+   稍难：构造[另一种transf](https://www.google.com/url?q=https%3A%2F%2Farxiv.org%2Fabs%2F2005.08100)ormer的变体：conformer。

+   困难：实现 Self-Attention Pooling 和 Additive Margin Softmax，以进一步提高性能。

## 数据

> 数据集

> 总的来说，这段代码的目的是通过设置随机数种子来确保代码的可重复性，这在科学实验和机器学习任务中非常重要。设置一个全局的种子（seed）‌，以确保代码的可重复性。具体来说，它设置了NumPy、Python的random模块和PyTorch的随机种子。这样，无论代码运行多少次，只要使用相同的种子，生成的随机数序列都会是相同的。这对于调试和实验结果的可重复性非常重要。

+   原始数据集是[Voxceleb2](https://www.google.com/url?q=https%3A%2F%2Fwww.robots.ox.ac.uk%2F%7Evgg%2Fdata%2Fvoxceleb%2Fvox2.html)，这里有Voxceleb2的[许可证](https://www.google.com/url?q=https%3A%2F%2Fcreativecommons.org%2Flicenses%2Fby%2F4.0%2F)和[完整版本](https://www.google.com/url?q=https%3A%2F%2Fwww.robots.ox.ac.uk%2F%7Evgg%2Fdata%2Fvoxceleb%2Ffiles%2Flicense.txt)。

+   我们从Voxceleb2中随机选择了600名演讲者。，然后将原始波形预处理成mel-spectrograms。

+   Args：

+   data_dir：数据目录的路径。

+   metadata_path：元数据的路径。

+   segment_len：用于训练的音频段的长度。

+   数据目录的架构

+   数据目录
|----元数据.json
|---- testdata.json
|---- mapping.json
|---- uttr-{随机字符串}.pt

+   元数据中的信息

+   "n_mels"：mel-spectrogram的量度。

+   扬声器：一本字典。

+   键：扬声器ID。

+   值：“feature_path”和“mel_len”

> 为了提高效率，我们在跟踪步骤中将mel-spectrograms分割成部分。

```
 #导入需要的库
import numpy as np # 导入NumPy库，并简称为np。
import torch # 导入PyTorch库。
import random # 导入Python的random模块，用于生成随机数。

# 这个函数接受一个参数seed，用于设置随机数生成的种子。
def set_seed(seed): 
    np.random.seed(seed) # 设置NumPy的随机数生成种子。
    random.seed(seed) # 设置Python的random模块的随机数生成种子。
    torch.manual_seed(seed) # 设置PyTorch的随机数生成种子。
    
    # 分别设置CUDA的当前设备和所有设备的随机数生成种子。
    if torch.cuda.is_available():
        torch.cuda.manual_seed(seed)
        torch.cuda.manual_seed_all(seed)
        
    # 这两个设置用于确保PyTorch的卷积神经网络操作是确定性的，即使在不同的硬件或不同的CUDA版本上。
    torch.backends.cudnn.benchmark = False
    torch.backends.cudnn.deterministic = True

set_seed(87) # 调用set_seed函数，设置种子为87。
```

```
#导入需要的库
import os       # 导入os库，用于处理文件和目录操作。
import json     # 导入json库，用于处理JSON数据格式。
import torch    # 导入torch库，用于深度学习和神经网络相关的操作。
import random   # 导入random库，用于生成随机数。
from pathlib import Path  # 从pathlib库中导入Path类，用于处理文件路径。
from torch.utils.data import Dataset  # 从torch.utils.data库中导入Dataset类，用于创建自定义数据集。
from torch.nn.utils.rnn import pad_sequence  # 从torch.nn.utils.rnn库中导入pad_sequence函数，用于对序列进行填充以使其具有相同的长度。


class myDataset(Dataset):
    def __init__(self, data_dir, segment_len=128):
        self.data_dir = data_dir  # 数据目录
        self.segment_len = segment_len  # 片段长度

        # Load the mapping from speaker name to their corresponding id.
        mapping_path = Path(data_dir) / &quot;mapping.json&quot;  # 映射文件路径
        mapping = json.load(mapping_path.open())  # 加载映射文件
        self.speaker2id = mapping[&quot;speaker2id&quot;]  # 将映射文件中的说话者名称映射到ID

        # Load metadata of training data.
        metadata_path = Path(data_dir) / &quot;metadata.json&quot;  # 元数据文件路径
        metadata = json.load(open(metadata_path))[&quot;speakers&quot;]  # 加载元数据文件

        # Get the total number of speaker.
        self.speaker_num = len(metadata.keys())  # 获取说话者总数
        self.data = []  # 初始化数据列表
        for speaker in metadata.keys():  # 遍历每个说话者
            for utterances in metadata[speaker]:  # 遍历每个说话者的发言
                self.data.append([utterances[&quot;feature_path&quot;], self.speaker2id[speaker]])  # 将发言的特征路径和对应的说话者ID添加到数据列表中

    def __len__(self):
    # 返回数据集的长度，即数据的数量
    return len(self.data)

    def __getitem__(self, index):
    # 根据索引获取数据集中的元素
    feat_path, speaker = self.data[index]
    # 加载预处理的梅尔频谱图
    mel = torch.load(os.path.join(self.data_dir, feat_path))

    # 如果梅尔频谱图的长度大于segment_len，则对其进行分段处理
    if len(mel) &gt; self.segment_len:
        # 随机选择一个起始点进行分段
        start = random.randint(0, len(mel) - self.segment_len)
        # 获取长度为segment_len的分段
        mel = torch.FloatTensor(mel[start:start+self.segment_len])
    else:
        # 如果长度不足，直接转换为FloatTensor
        mel = torch.FloatTensor(mel)
    # 将说话者ID转换为长整型，以便后续计算损失
    speaker = torch.FloatTensor([speaker]).long()
    return mel, speaker

    def get_speaker_number(self):
    # 返回说话者的数量
    return self.speaker_num

```

## 数据加载器

+   将数据集拆分为训练数据集（90%）和验证数据集（10%）。

+   创建数据加载器来规范化加工数据。

```
#导入需要导库
import torch  # 导入PyTorch库
from torch.utils.data import DataLoader, random_split  # 导入DataLoader和random_split工具
from torch.nn.utils.rnn import pad_sequence  # 导入pad_sequence工具，用于填充序列

def collate_batch(batch):
    # 处理批次内的特征
    &quot;&quot;&quot;将一批数据整理成一个批次。&quot;&quot;&quot;
    mel, speaker = zip(*batch)
    # 因为我们是逐批次训练模型，所以需要对同一批次中的特征进行填充，使它们的长度相同。
    mel = pad_sequence(mel, batch_first=True, padding_value=-20)    # 使用log 10^(-20)进行填充，这是一个非常小的值。
    # mel: (批次大小， 长度， 40)
    return mel, torch.FloatTensor(speaker).long()

def get_dataloader(data_dir, batch_size, n_workers):
    &quot;&quot;&quot;生成数据加载器&quot;&quot;&quot;
    # 创建数据集实例
    dataset = myDataset(data_dir)
    # 获取说话者数量
    speaker_num = dataset.get_speaker_number()
    # 将数据集分为训练集和验证集
    trainlen = int(0.9 * len(dataset))
    lengths = [trainlen, len(dataset) - trainlen]
    # 随机划分数据集
    trainset, validset = random_split(dataset, lengths)

    train_loader = DataLoader(  # 创建一个训练数据加载器
    trainset,  # 使用训练数据集
    batch_size=batch_size,  # 设置每个批次的大小
    shuffle=True,  # 在每个epoch开始时打乱数据顺序
    drop_last=True,  # 如果数据集大小不能被批次大小整除，则丢弃最后一个不完整的批次
    num_workers=n_workers,  # 设置用于数据加载的子进程数量
    pin_memory=True,  # 将数据存储在固定内存区域，以便更快地访问
    collate_fn=collate_batch,  # 自定义如何将样本组合成一个批次
)
valid_loader = DataLoader(  # 创建一个验证数据加载器
    validset,  # 使用验证数据集
    batch_size=batch_size,  # 设置每个批次的大小
    num_workers=n_workers,  # 设置用于数据加载的子进程数量
    drop_last=True,  # 如果数据集大小不能被批次大小整除，则丢弃最后一个不完整的批次
    pin_memory=True,  # 将数据存储在固定内存区域，以便更快地访问
    collate_fn=collate_batch,  # 自定义如何将样本组合成一个批次
)


        return train_loader, valid_loader, speaker_num

```

## 模型

+   Transformer编码器层：

+   Attention Is All You Need中的Transformer编码器实现。

+   参数：

+   d_model：输入的预期特征数量（必填）。

+   nhead：多头注意力的数量（必填）。

+   dim_feedforward：feedforward网络模型的维度（默认=2048）。

+   dropout：dropout的比率（默认=0.1）。

+   激活函数：中间层、relu或gelu（默认=relu）。

+   Transformer编码器：

+   Transformer编码器是N个Transformer编码器层的堆叠。

+   参数：

+   encoder_layer：TransformerEncoderLayer（）类的实例（必填）。

+   num_layers：编码器中子编码层的数量（必填）。

+   规范：层归一化组件（可选）。

```
#导入需要导库
import torch  # 导入torch库
import torch.nn as nn  # 导入torch.nn模块，并将其命名为nn
import torch.nn.functional as F  # 导入torch.nn.functional模块，并将其命名为F

class Classifier(nn.Module):
    def __init__(self, d_model=80, n_spks=600, dropout=0.1):
        super().__init__()
        # 将输入特征的维度从40投影到d_model。
        self.prenet = nn.Linear(40, d_model)
        # TODO:
        #   将Transformer更改为Conformer。
        #   https://arxiv.org/abs/2005.08100
        self.encoder_layer = nn.TransformerEncoderLayer(
            d_model=d_model, dim_feedforward=256, nhead=2
        )
        # self.encoder = nn.TransformerEncoder(self.encoder_layer, num_layers=2)

        # 将特征的维度从d_model投影到说话者数量。
        self.pred_layer = nn.Sequential(
            nn.Linear(d_model, d_model),
            nn.Sigmoid(),
            nn.Linear(d_model, n_spks),
        )

    def forward(self, mels):
        &quot;&quot;&quot;
        args:
            mels: (batch size, length, 40)
        return:
            out: (batch size, n_spks)
        &quot;&quot;&quot;
        # out: (batch size, length, d_model)
        # 输出 (batch size, length, d_model)
        out = self.prenet(mels)
        # out: (length, batch size, d_model)
        #输出：(长度，批次大小，d_model)
        out = out.permute(1, 0, 2)
        # 编码器层期望特征的形状为(length, batch size, d_model)。
        out = self.encoder_layer(out)
        # out: (batch size, length, d_model)
        #输出 (batch size, length, d_model)
        out = out.transpose(0, 1)
        # 平均池化
        stats = out.mean(dim=1)

        # out: (batch, n_spks)
        out = self.pred_layer(stats)
        return out

```

## 学习率时间表

+   对于Transformer架构，学习速率时间表的设计与CNN不同。

+   以前的工作表明，学习率的预热对于使用Transformer架构训练模型很有用。

+   热身时间表

+   一开始将学习率设置为0。

+   在热身期间，学习率从0线性增加到初始学习率。

```

import math  # 导入数学库，提供数学运算函数和常量
import torch  # 导入PyTorch库，用于深度学习和张量计算
from torch.optim import Optimizer  # 从torch.optim模块导入Optimizer类，用于优化器的定义
from torch.optim.lr_scheduler import LambdaLR  # 从torch.optim.lr_scheduler模块导入LambdaLR类，用于自定义学习率调整策略


def get_cosine_schedule_with_warmup(
        optimizer: Optimizer,     #优化器对象，用于更新模型参数以最小化损失函数。
        num_warmup_steps: int,    #预热步数，即在前多少步中，学习率会线性增加，以便模型在训练开始时不会突然从0开始。
        num_training_steps: int,  #训练的总步数，通常等于数据集的批次数乘以训练轮数。
        num_cycles: float = 0.5,  #学习率调整周期的倍数，默认值为0.5，表示学习率会在每个周期内上升和下降。
        last_epoch: int = -1,     #上一个周期结束时的步数，默认值为-1，表示从头开始计算学习率。
):
   
       #创建一个学习率调度器，学习率在预热阶段线性增加，从0增加到优化器中设置的初始学习率，然后在预热阶段之后，根据余弦函数的值逐渐减小到0。
        #参数：
        #optimizer (:class:~torch.optim.Optimizer):
        #要安排学习率的优化器。
        #num_warmup_steps (:obj:int):
        #预热阶段的步数。
        #num_training_steps (:obj:int):
        #训练总步数。
        #num_cycles (:obj:float, optional, 默认为0.5):
        #余弦调度中的波数（默认是从最大值减小到0的一个半余弦）。
        #last_epoch (:obj:int, optional, 默认为-1):
        #恢复训练时最后一个epoch的索引。
        #返回：
        #:obj:torch.optim.lr_scheduler.LambdaLR，具有适当的调度。
    
    def lr_lambda(current_step):
    # 预热阶段
    if current_step &lt; num_warmup_steps:
        return float(current_step) / float(max(1, num_warmup_steps))  # 计算预热阶段的学习率
    # 衰减阶段
    progress = float(current_step - num_warmup_steps) / float(
            max(1, num_training_steps - num_warmup_steps)
    )  # 计算当前进度
    return max(
            0.0, 0.5 * (1.0 + math.cos(math.pi * float(num_cycles) * 2.0 * progress))
    )  # 根据余弦退火公式计算衰减阶段的学习率

    return LambdaLR(optimizer, lr_lambda, last_epoch)  # 返回一个LambdaLR对象，用于调整优化器的学习率


```

## 模型功能

###   Model Function

> 

> 模型前向传播函数（Model forward function.）

```
#导入需要导库
import torch  # 导入torch库 

def model_fn(batch, model, criterion, device):
    &quot;&quot;&quot;Forward a batch through the model.&quot;&quot;&quot;

    mels, labels = batch
    mels = mels.to(device)  # 将数据移动到指定设备（如GPU）上
    labels = labels.to(device)  # 将标签移动到指定设备（如GPU）上

    outs = model(mels)  # 通过模型进行前向传播

    loss = criterion(outs, labels)  # 计算损失值

    # Get the speaker id with highest probability.
    preds = outs.argmax(1)  # 获取概率最高的类别索引作为预测结果
    # Compute accuracy.
    accuracy = torch.mean((preds == labels).float())  # 计算准确率

    return loss, accuracy  # 返回损失值和准确率

```

## 验证

> 计算验证集的准确性。

```
# 导入进度条库tqdm
from tqdm import tqdm
# 导入PyTorch库
import torch

# 验证函数
def valid(dataloader, model, criterion, device):
    &quot;&quot;&quot;在验证集上进行验证。&quot;&quot;&quot;

    model.eval()  # 将模型设置为评估模式
    running_loss = 0.0  # 初始化累计损失
    running_accuracy = 0.0  # 初始化累计准确率
    pbar = tqdm(total=len(dataloader.dataset), ncols=0, desc=&quot;Valid&quot;, unit=&quot; uttr&quot;)  # 创建进度条

    for i, batch in enumerate(dataloader):  # 遍历数据加载器中的每个批次
        with torch.no_grad():  # 禁用梯度计算，因为我们不需要更新模型参数
            loss, accuracy = model_fn(batch, model, criterion, device)  # 计算损失和准确率
            running_loss += loss.item()  # 累加损失
            running_accuracy += accuracy.item()  # 累加准确率

        pbar.update(dataloader.batch_size)  # 更新进度条
        pbar.set_postfix(
            loss=f&quot;{running_loss / (i+1):.2f}&quot;,  # 设置进度条的损失显示
            accuracy=f&quot;{running_accuracy / (i+1):.2f}&quot;,  # 设置进度条的准确率显示
        )

    pbar.close()  # 关闭进度条
    model.train()  # 将模型恢复为训练模式

    return running_accuracy / len(dataloader)  # 返回平均准确率

```

## 主要功能

```
# 主要功能
from tqdm import tqdm  # 导入tqdm库，用于显示进度条

import torch  # 导入torch库，用于深度学习计算
import torch.nn as nn  # 导入torch.nn模块，用于构建神经网络模型
from torch.optim import AdamW  # 从torch.optim模块中导入AdamW优化器
from torch.utils.data import DataLoader, random_split  # 从torch.utils.data模块中导入DataLoader和random_split函数，用于数据加载和划分

def parse_args():
    &quot;&quot;&quot;解析参数并返回配置字典&quot;&quot;&quot;
    config = {
            &quot;data_dir&quot;: &quot;./Dataset&quot;,    # 数据集目录
            &quot;save_path&quot;: &quot;model.ckpt&quot;,  # 模型保存路径
            &quot;batch_size&quot;: 32,           # 批处理大小
            &quot;n_workers&quot;: 8,             # 工作线程数
            &quot;valid_steps&quot;: 2000,        # 验证步数
            &quot;warmup_steps&quot;: 1000,       # 预热步数
            &quot;save_steps&quot;: 10000,        # 保存步数
            &quot;total_steps&quot;: 70000,       # 总步数
    }

    return config

    
def main(
        data_dir,      # 数据目录
        save_path,     # 保存模型的路径
        batch_size,    # 批处理大小
        n_workers,     # 工作线程数
        valid_steps,   # 验证步数
        warmup_steps,  # 预热步数
        total_steps,   # 总步数
        save_steps,    # 保存步数
):
        &quot;&quot;&quot;主函数。&quot;&quot;&quot;
        device = torch.device(&quot;cuda&quot; if torch.cuda.is_available() else &quot;cpu&quot;)  # 选择设备（GPU或CPU）
        print(f&quot;[Info]: Use {device} now!&quot;)

        train_loader, valid_loader, speaker_num = get_dataloader(data_dir, batch_size, n_workers)  # 获取数据加载器
        train_iterator = iter(train_loader)  # 创建训练迭代器
        print(f&quot;[Info]: Finish loading data!&quot;,flush = True)

        model = Classifier(n_spks=speaker_num).to(device)  # 创建分类器模型并移动到设备上
        criterion = nn.CrossEntropyLoss()  # 定义损失函数
        optimizer = AdamW(model.parameters(), lr=1e-3)  # 定义优化器
        scheduler = get_cosine_schedule_with_warmup(optimizer, warmup_steps, total_steps)  # 定义学习率调度器
        print(f&quot;[Info]: Finish creating model!&quot;,flush = True)

        best_accuracy = -1.0  # 初始化最佳准确率
        best_state_dict = None  # 初始化最佳状态字典

        pbar = tqdm(total=valid_steps, ncols=0, desc=&quot;Train&quot;, unit=&quot; step&quot;)  # 创建进度条

        for step in range(total_steps):
                # 获取数据
                try:
                        batch = next(train_iterator)
                except StopIteration:
                        train_iterator = iter(train_loader)
                        batch = next(train_iterator)

                loss, accuracy = model_fn(batch, model, criterion, device)  # 计算损失和准确率
                batch_loss = loss.item()  # 获取批次损失
                batch_accuracy = accuracy.item()  # 获取批次准确率

                # 更新模型
                loss.backward()  # 反向传播
                optimizer.step()  # 更新参数
                scheduler.step()  # 更新学习率
                optimizer.zero_grad()  # 清空梯度

                # 记录日志
                pbar.update()
                pbar.set_postfix(
                        loss=f&quot;{batch_loss:.2f}&quot;,
                        accuracy=f&quot;{batch_accuracy:.2f}&quot;,
                        step=step + 1,
                )

                # 进行验证
                if (step + 1) % valid_steps == 0:
                        pbar.close()

                        valid_accuracy = valid(valid_loader, model, criterion, device)  # 计算验证准确率

                        # 保留最佳模型
                        if valid_accuracy &gt; best_accuracy:
                                best_accuracy = valid_accuracy
                                best_state_dict = model.state_dict()

                        pbar = tqdm(total=valid_steps, ncols=0, desc=&quot;Train&quot;, unit=&quot; step&quot;)

                # 保存到目前为止的最佳模型。
                if (step + 1) % save_steps == 0 and best_state_dict is not None:
                        torch.save(best_state_dict, save_path)  # 保存最佳模型
                        pbar.write(f&quot;Step {step + 1}, best model saved. (accuracy={best_accuracy:.4f})&quot;)

        pbar.close()

if __name__ == &quot;__main__&quot;:
        main(**parse_args())  # 解析命令行参数并运行主函数

```

## 推断

> 推理数据集

```
#导入需要的库
import os       # 导入os库，用于处理文件和目录操作。
import json     # 导入json库，用于处理JSON数据格式。
import torch    # 导入torch库，用于深度学习和神经网络相关的操作。
from pathlib import Path  # 从pathlib库中导入Path类，用于处理文件路径。
from torch.utils.data import Dataset  # 从torch.utils.data库中导入Dataset类，用于创建自定义数据集。

class InferenceDataset(Dataset):
    def __init__(self, data_dir):
        # 加载测试数据文件路径
        testdata_path = Path(data_dir) / &quot;testdata.json&quot;
        # 读取JSON文件中的元数据
        metadata = json.load(testdata_path.open())
        # 存储数据目录和utterances列表
        self.data_dir = data_dir
        self.data = metadata[&quot;utterances&quot;]

    def __len__(self):
        # 返回utterances列表的长度
        return len(self.data)

    def __getitem__(self, index):
        # 根据索引获取utterance
        utterance = self.data[index]
        # 获取特征文件路径
        feat_path = utterance[&quot;feature_path&quot;]
        # 加载特征文件并将其转换为torch张量
        mel = torch.load(os.path.join(self.data_dir, feat_path))

        return feat_path, mel

def inference_collate_batch(batch):
    &quot;&quot;&quot;Collate a batch of data.&quot;&quot;&quot;
    # 解压缩批次数据，分别获取特征文件路径和特征张量
    feat_paths, mels = zip(*batch)

    # 返回特征文件路径列表和堆叠后的特征张量
    return feat_paths, torch.stack(mels)

```

## 推理的主要功能

```
import json  # 导入json模块，用于处理json文件
import csv   # 导入csv模块，用于处理csv文件
from pathlib import Path  # 导入Path类，用于处理文件路径
from tqdm.notebook import tqdm  # 导入tqdm模块，用于显示进度条

import torch  # 导入torch模块，用于深度学习模型的训练和推理
from torch.utils.data import DataLoader  # 导入DataLoader类，用于加载数据集

&quot;&quot;&quot;解析命令行参数&quot;&quot;&quot;
def parse_args():

    config = {
        &quot;data_dir&quot;: &quot;./Dataset&quot;,  # 数据目录
        &quot;model_path&quot;: &quot;./model.ckpt&quot;,  # 模型文件路径
        &quot;output_path&quot;: &quot;./output.csv&quot;,  # 输出文件路径
    }

    return config
    
 &quot;&quot;&quot;主函数&quot;&quot;&quot;
def main(
        data_dir,
        model_path,
        output_path,
):
   
    device = torch.device(&quot;cuda&quot; if torch.cuda.is_available() else &quot;cpu&quot;)  # 判断是否有可用的GPU，如果有则使用GPU，否则使用CPU
    print(f&quot;[Info]: Use {device} now!&quot;)

    mapping_path = Path(data_dir) / &quot;mapping.json&quot;  # 映射文件路径
    mapping = json.load(mapping_path.open())  # 加载映射文件

    dataset = InferenceDataset(data_dir)  # 创建推理数据集
    dataloader = DataLoader(
        dataset,
        batch_size=1,
        shuffle=False,
        drop_last=False,
        num_workers=8,
        collate_fn=inference_collate_batch,
    )
    print(f&quot;[Info]: Finish loading data!&quot;,flush = True)

    speaker_num = len(mapping[&quot;id2speaker&quot;])  # 获取说话者数量
    model = Classifier(n_spks=speaker_num).to(device)  # 创建分类器模型并移动到设备上
    model.load_state_dict(torch.load(model_path))  # 加载模型权重
    print(f&quot;[Info]: Finish creating model!&quot;,flush = True)

    results = [[&quot;Id&quot;, &quot;Category&quot;]]  # 初始化结果列表
    for feat_paths, mels in tqdm(dataloader):  # 遍历数据加载器
        with torch.no_grad():  # 禁用梯度计算
            mels = mels.to(device)  # 将数据移动到设备上
            outs = model(mels)  # 进行模型推理
            preds = outs.argmax(1).cpu().numpy()  # 获取预测结果
            for feat_path, pred in zip(feat_paths, preds):  # 遍历特征路径和预测结果
                results.append([feat_path, mapping[&quot;id2speaker&quot;][str(pred)]])  # 将结果添加到结果列表中

    with open(output_path, &#x27;w&#x27;, newline=&#x27;&#x27;) as csvfile:  # 打开输出文件
        writer = csv.writer(csvfile)  # 创建csv写入器
        writer.writerows(results)  # 将结果写入csv文件

if __name__ == &quot;__main__&quot;:
    main(**parse_args())  # 调用主函数并传入解析后的参数

```

# 优化

> 代码段虽然结束，‌但通常接下来会执行反向传播、‌优化器步骤和调度器步骤，‌以及可能的模型保存和验证步骤。‌

> 

> 本次代码是训练深度学习模型（‌如说话者识别模型）‌的典型框架。‌它涵盖了从数据加载到模型初始化，‌再到训练循环执行的基本步骤。‌为了完整执行训练过程，‌还需要实现缺失的函数（‌如get_dataloader和get_cosine_schedule_with_warmup）‌，‌并在训练循环中添加反向传播、‌优化器更新和可能的模型保存逻辑。‌

##  【更多内容查看以下HW4外文pdf   可下载使用！！！】

![](img/4b04b49a611679eeb2582b1cbbfaf797.png)

[HW04.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/ALy2beZ1fo1FQfxPTOzc4c3Fnmg)

这是一份很有意思的课程～

# 🎉 恭喜你完成本次学习～！

## 👍🏻 快来给自己点赞！

# 更多QA详见[ QA & 其他彩蛋](https://datawhaler.feishu.cn/wiki/BdEMwwcDoiUf72k0zADchzO2n1e)！

> [ QA & 其他彩蛋](https://datawhaler.feishu.cn/wiki/BdEMwwcDoiUf72k0zADchzO2n1e)任何想法都可以在学习群里交流或在 进行填写哟~

>

> 本教程主要由   写作贡献，

> 融合了多位助教的内测体验和宝贵建议。

> 如果你觉得对你有价值和启发，请在下面点赞和评论哟~

>

### 另外别忘了写笔记打卡呀！！~

### 及时思考、沉淀和产出能让你的学习事半功倍！

>

> **注：本文采用 ****署名—非商业性使用**—相同方式共享 4.0 协议国际版许可协议进行许可。