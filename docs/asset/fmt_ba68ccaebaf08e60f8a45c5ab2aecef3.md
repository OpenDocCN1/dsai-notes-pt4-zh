
```py
# create validation set
split_size = int(train_x.shape[0]*0.7)

train_x, val_x = train_x[:split_size], train_x[split_size:]
train_y, val_y = train_y[:split_size], train_y[split_size:] 
```

## 第 2 步：构建模型

a）下面是模型的主体，我们定义的神经网络共有三层，即输入层、隐藏层和输出层。输入层和输出层的神经元数量是固定的，即 28 x 28 和 10 x 1，它们分别代表了输入图像的像素和类别。我们在隐藏层采用了 50 个神经元，并采用 `Adam` 作为最优化算法。

```py
import torch
from torch.autograd import Variable
# number of neurons in each layer
input_num_units = 28*28
hidden_num_units = 500
output_num_units = 10

# set remaining variables
epochs = 5
batch_size = 128
learning_rate = 0.001 
```

b）以下将开始训练模型。

```py
# define model
model = torch.nn.Sequential(
  torch.nn.Linear(input_num_units, hidden_num_units),
  torch.nn.ReLU(),
  torch.nn.Linear(hidden_num_units, output_num_units),
)
loss_fn = torch.nn.CrossEntropyLoss()

# define optimization algorithm
optimizer = torch.optim.Adam(model.parameters(), lr=learning_rate)
## helper functions
# preprocess a batch of dataset
def preproc(unclean_batch_x):
  """Convert values to range 0-1"""
  temp_batch = unclean_batch_x / unclean_batch_x.max()

  return temp_batch

# create a batch
def batch_creator(batch_size):
  dataset_name = 'train'
  dataset_length = train_x.shape[0]

  batch_mask = rng.choice(dataset_length, batch_size)

  batch_x = eval(dataset_name + '_x')[batch_mask]
  batch_x = preproc(batch_x)

  if dataset_name == 'train':
    batch_y = eval(dataset_name).ix[batch_mask, 'label'].values

  return batch_x, batch_y
# train network
total_batch = int(train.shape[0]/batch_size)

for epoch in range(epochs):
  avg_cost = 0
  for i in range(total_batch):
    # create batch
    batch_x, batch_y = batch_creator(batch_size)

    # pass that batch for training
    x, y = Variable(torch.from_numpy(batch_x)), Variable(torch.from_numpy(batch_y), requires_grad=False)
    pred = model(x)

    # get loss
    loss = loss_fn(pred, y)

    # perform backpropagation
    loss.backward()
    optimizer.step()
    avg_cost += loss.data[0]/total_batch

  print(epoch, avg_cost)
# get training accuracy
x, y = Variable(torch.from_numpy(preproc(train_x))), Variable(torch.from_numpy(train_y), requires_grad=False)
pred = model(x)

final_pred = np.argmax(pred.data.numpy(), axis=1)

accuracy_score(train_y, final_pred)
# get validation accuracy
x, y = Variable(torch.from_numpy(preproc(val_x))), Variable(torch.from_numpy(val_y), requires_grad=False)
pred = model(x)
final_pred = np.argmax(pred.data.numpy(), axis=1)

accuracy_score(val_y, final_pred) 
```

* 训练准确度为：0.8779008746355685

* 测试准确度为：0.867482993197279

这些分数非常令人满意，因为我们只是用简单的神经网络训练了 5 个 Epoch。以上，本文介绍了简单的 PyTorch 入门概念，并利用简单的案例熟悉 PyTorch 的使用。读者可以继续阅读 PyTorch 的文档以了解更多信息。![](img/b4ff9f2fe02305b4ef55715abe470b8e-fs8.png)

原文链接：`https://www.analyticsvidhya.com/blog/2018/02/pytorch-tutorial/`

本文为机器之心编译，转载请联系本👸号获得授权。

✄------------------------------------------------

加入机器之心（全职记者/实习生）：hr@jiqizhixin.com

投稿或寻求报道：editor@jiqizhixin.com

广告&商务合作：bd@jiqizhixin.com
