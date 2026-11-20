
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
        """
        args:
            mels: (batch size, length, 40)
        return:
            out: (batch size, n_spks)
        """
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

+ 对于Transformer架构，学习速率时间表的设计与CNN不同。
+ 以前的工作表明，学习率的预热对于使用Transformer架构训练模型很有用。
+ 热身时间表
+ 一开始将学习率设置为0。
+ 在热身期间，学习率从0线性增加到初始学率。

```
import math  # 导入数学库，提供数学运算函数和常量
import torch  # 导入PyTorch库，用于深度学习和张量计算
from torch.optim import Optimizer  # 从torch.optim模块导入Optimizer类，用于优化器的定义
from torch.optim.lr_scheduler import LambdaLR  # 从torch.optim.lr_scheduler模块导入LambdaLR类，用于自定义学习率调整策略


def get_cosine_schedule_with_warmup(
        optimizer: Optimizer,     # 优化器对象，用于更新模型参数以最小化损失函数。
        num_warmup_steps: int,    # 预热步数，即在前多少步中，学习率会线性增加，以便模型在训练开始时不会突然从0开始。
        num_training_steps: int,  # 训练的总步数，通常等于数据集的批次数乘以训练轮数。
        num_cycles: float = 0.5,  # 学习率调整周期的倍数，默认值为0.5，表示学习率会在每个周期内上升和下降。
        last_epoch: int = -1,     # 上一个周期结束时的步数，默认值为-1，表示从头开始计算学习率。
):

    # 创建一个学习率调度器，学习率在预热阶段线性增加，从0增加到优化器中设置的初始学习率，然后在预热阶段之后，根据余弦函数的值逐渐减小到0。
    # 参数：
    # optimizer (:class:~torch.optim.Optimizer):
    # 要安排学习率的优化器。
    # num_warmup_steps (:obj:int):
    # 预热阶段的步数。
    # num_training_steps (:obj:int):
    # 训练总步数。
    # num_cycles (:obj:float, optional, 默认为0.5):
    # 余弦调度中的波数（默认是从最大值减小到0的一个半余弦）。
    # last_epoch (:obj:int, optional, 默认为-1):
    # 恢复训练时最后一个epoch的索引。
    # 返回：
    # :obj:torch.optim.lr_scheduler.LambdaLR，具有适当的调度。

    def lr_lambda(current_step):
        # 预热阶段
        if current_step < num_warmup_steps:
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

### Model Function

> 模型前向传播函数（Model forward function。）

```
import torch  # 导入torch库 

def model_fn(batch, model, criterion, device):
    """Forward a batch through the model."""

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
    """在验证集上进行验证。"""

    model.eval()  # 将模型设置为评估模式
    running_loss = 0.0  # 初始化累计损失
    running_accuracy = 0.0  # 初始化累计准确率
    pbar = tqdm(total=len(dataloader.dataset), ncols=0, desc="Valid", unit=" uttr")  # 创建进度条

    for i, batch in enumerate(dataloader):  # 遍历数据加载器中的每个批次
        with torch.no_grad():  # 禁用梯度计算，因为我们不需要更新模型参数
            loss, accuracy = model_fn(batch, model, criterion, device)  # 计算损失和准确率
            running_loss += loss.item()  # 累加损失
            running_accuracy += accuracy.item()  # 累加准确率

        pbar.update(dataloader.batch_size)  # 更新进度条
        pbar.set_postfix(
            loss=f"{running_loss / (i+1):.2f}",  # 设置进度条的损失显示
            accuracy=f"{running_accuracy / (i+1):.2f}",  # 设置进度条的准确率显示
        )

    pbar.close()  # 关闭进度条
    model.train()  # 将模型恢复为训练模式

    return running_accuracy / len(dataloader)  # 返回平均准确率
```

## 主要功能
