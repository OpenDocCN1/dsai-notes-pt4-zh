
# PaddlePaddle LeNet-5 示例

## cnn.py

```py
# coding=utf-8
import paddle.v2 as paddle

# 卷积神经网络 LeNet-5, 获取分类器
def convolutional_neural_network():
 # 定义数据模型, 数据大小是 28*28, 即 784
 img = paddle.layer.data(name="pixel",
 type=paddle.data_type.dense_vector(784))
 # 第一个卷积--池化层
 conv_pool_1 = paddle.networks.simple_img_conv_pool(input=img,
 filter_size=5,
 num_filters=20,
 num_channel=1,
 pool_size=2,
 pool_stride=2,
 act=paddle.activation.Relu())
 # 第二个卷积--池化层
 conv_pool_2 = paddle.networks.simple_img_conv_pool(input=conv_pool_1,
 filter_size=5,
 num_filters=50,
 num_channel=20,
 pool_size=2,
 pool_stride=2,
 act=paddle.activation.Relu())
 # 以 softmax 为激活函数的全连接输出层，输出层的大小必须为数字的个数 10
 predict = paddle.layer.fc(input=conv_pool_2,
 size=10,
 act=paddle.activation.Softmax())
 return predict 
```

## train.py

`train.py` 代码：

```py
# encoding:utf-8
import os
import sys
import paddle.v2 as paddle
from cnn import convolutional_neural_network

class TestMNIST:
 def __init__(self):
 # 该模型运行在 CUP 上，CUP 的数量为 2
 paddle.init(use_gpu=False, trainer_count=2)

 # *****************获取训练器********************************
 def get_trainer(self):

 # 获取分类器
 out = convolutional_neural_network()

 # 定义标签
 label = paddle.layer.data(name="label",
 type=paddle.data_type.integer_value(10))

 # 获取损失函数
 cost = paddle.layer.classification_cost(input=out, label=label)

 # 获取参数
 parameters = paddle.parameters.create(layers=cost)

 """
 定义优化方法
 learning_rate 迭代的速度
 momentum 跟前面动量优化的比例
 regularzation 正则化, 防止过拟合
 :leng re
 """
 optimizer = paddle.optimizer.Momentum(learning_rate=0.1 / 128.0,
 momentum=0.9,
 regularization=paddle.optimizer.L2Regularization(rate=0.0005 * 128))
 '''
 创建训练器
 cost 分类器
 parameters 训练参数, 可以通过创建, 也可以使用之前训练好的参数
 update_equation 优化方法
 '''
 trainer = paddle.trainer.SGD(cost=cost,
 parameters=parameters,
 update_equation=optimizer)
 return trainer

 # *****************开始训练********************************
 def start_trainer(self):
 # 获取训练器
 trainer = self.get_trainer()

 # 定义训练事件
 def event_handler(event):
 lists = []
 if isinstance(event, paddle.event.EndIteration):
 if event.batch_id % 100 == 0:
 print "\nPass %d, Batch %d, Cost %f, %s" % (
 event.pass_id, event.batch_id, event.cost, event.metrics)
 else:
 sys.stdout.write('.')
 sys.stdout.flush()
 if isinstance(event, paddle.event.EndPass):
 # 保存训练好的参数
 model_path = '../model'
 if not os.path.exists(model_path):
 os.makedirs(model_path)
 with open(model_path + "/model.tar", 'w') as f:
 trainer.save_parameter_to_tar(f=f)
 # 使用测试进行测试
 result = trainer.test(reader=paddle.batch(paddle.dataset.mnist.test(), batch_size=128))
 print "\nTest with Pass %d, Cost %f, %s\n" % (event.pass_id, result.cost, result.metrics)
 lists.append((event.pass_id, result.cost, result.metrics['classification_error_evaluator']))

 # 获取数据
 reader = paddle.batch(paddle.reader.shuffle(paddle.dataset.mnist.train(), buf_size=20000),
 batch_size=128)
 '''
 开始训练
 reader 训练数据
 num_passes 训练的轮数
 event_handler 训练的事件, 比如在训练的时候要做一些什么事情
 '''
 trainer.train(reader=reader,
 num_passes=100,
 event_handler=event_handler)

if __name__ == "__main__":
 testMNIST = TestMNIST()
 # 开始训练
 testMNIST.start_trainer() 
```

## infer.py

`infer.py` 代码：

```py
# encoding:utf-8
import numpy as np
import paddle.v2 as paddle
from PIL import Image
from cnn import convolutional_neural_network

class TestMNIST:
    def __init__(self):
        # 该模型运行在 CUP 上，CUP 的数量为 2
        paddle.init(use_gpu=False, trainer_count=2)

    # *****************获取参数********************************
    def get_parameters(self):
        with open("../model/model.tar", 'r') as f:
            parameters = paddle.parameters.Parameters.from_tar(f)
        return parameters

    # *****************获取你要预测的参数********************************
    def get_TestData(self, path):
        def load_images(file):
            # 对图进行灰度化处理
            im = Image.open(file).convert('L')
            # 缩小到跟训练数据一样大小
            im = im.resize((28, 28), Image.ANTIALIAS)
            im = np.array(im).astype(np.float32).flatten()
            im = im / 255.0
            return im

        test_data = []
        test_data.append((load_images(path),))
        return test_data

    # *****************使用训练好的参数进行预测********************************
    def to_prediction(self, out, parameters, test_data):

        # 开始预测
        probs = paddle.infer(output_layer=out,
                             parameters=parameters,
                             input=test_data)
        # 处理预测结果并打印
        lab = np.argsort(-probs)
        print "预测结果为: %d" % lab[0][0]

if __name__ == "__main__":
    testMNIST = TestMNIST()
    # 开始预测
    out = convolutional_neural_network()
    parameters = testMNIST.get_parameters()
    test_data = testMNIST.get_TestData('../images/infer_3.png')
    testMNIST.to_prediction(out=out, parameters=parameters, test_data=test_data) 
```

## 项目代码

GitHub 地址: `https://github.com/yeyupiaoling/LearnPaddle`

## 参考阅读

- [专栏 | 百度深度学习平台 PaddlePaddle 框架解析](http://mp.weixin.qq.com/s?__biz=MzA3MzI4MjgzMw==&mid=2650739530&idx=3&sn=32bdfdd26a53210ea78097359f67fb65&chksm=871ad734b06d5e22029292b0a38f3a2c5e46b8c9f55d79865782daf7441eace6ea336831a7a8&scene=21#wechat_redirect)
- [专栏 | 新手入门？一步一步教你如何安装 PaddlePaddle](http://mp.weixin.qq.com/s?__biz=MzA3MzI4MjgzMw==&mid=2650739772&idx=3&sn=1d1a6cadfa042064928cab3191942df7&chksm=871ad042b06d59548a5406b8e45d05b48671e7cfd77c7633f6fb557ef783a2fb76cacbecf7a0&scene=21#wechat_redirect)

> 本文为机器之心专栏，转载请联系本👸号获得授权。

---

* 加入机器之心（全职记者/实习生）：`hr@jiqizhixin.com`
* 投稿或寻求报道：`editor@jiqizhixin.com`
* 广告&商务合作：`bd@jiqizhixin.com`
