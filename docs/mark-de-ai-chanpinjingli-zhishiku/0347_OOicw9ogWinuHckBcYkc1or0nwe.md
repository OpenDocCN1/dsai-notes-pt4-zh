# 深度学习12条调参经验

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/BBVCdHDFWoTsRux9RbQcpunPn3e](https://qqs7y1hozd1.feishu.cn/docx/BBVCdHDFWoTsRux9RbQcpunPn3e)

# 12条调参经验分享

1.调参一定要做好笔记，每次只调1个参数。

2.在 Validation筛选模型参数时，可以除loss函数“外，设置某种规则引导模型向某个想要的方向去更新参数。

3.关于Activation function:

(1)ReLu、Sigmoid、Softmax、Tanh是最常用的4个激活函数°

(2)对于输出层，常用sigmoid°和softMax激活函数，中间层常用 ReLu激活函数，RNN常用Tanh激活函数。

4.关于Weights initialization:

预训练参数是最好的参数初始化方式，其次是Xavir。

5.关于Regularization:

Dropout虽然思想很简单，但效果出奇的好，首选0.5。

6.Batch normalization很好用。

**7.如果在train模型时**，loss可以降到非常低那么模型的学习能力是够的;如果train能达到，validation达不到，则说明模型泛化能力不强。

**8.关于调参:**

有哪些参数可以调呢?

Loss function、Learning rate、Batch size、Epoch number、Optimizer、Activationfunction、Weights initialization、使用Regularization、Validation、使用的GPU个数。

9.关于 Loss function:

Loss function是Model和数据之外，第三重要的参数。具体使用MSE、Cross entropy、Focal还是其他自定义，需要具体问题具体分析。

10.关于Epoch number和early stopping

(1)Epochnumber和Early stopping是息息相关的，需要输出loss看一下，到底是什么epoch°时效果最好，及时early stopping。

(2)Epoch越大，会浪费计算资源;epoch太小，则训练模型提取特征没到极致。

(3)此外，也要明白Epoch、lteration、batchsize的关系。

**11.关于Optimizer:**

(1)Adam和SGDM是最常用的两个，前者能快速收敛°，后者收敛慢但最终精度更高。现在大家会先使用 Adam快速收敛，后面再用SGDM提升精度。

(2)如果必须二选一的话，我会推荐Adam。

12.关于 Learning rate和batch size:

(1)Learningrate和batch size“是两个重要的参数，而且二者也是相互影响的，在反向传播时直接影响梯度。一般情况下，先调batchsize，再调learning rate

(2)batchsize不能太大，也不能太小;太小会浪费计算资源，太大则会浪费内存;一般设置为 16的倍数。

(3)如果使用微调，则learningrate设置为0.0001较好。learningrate设置上有很多trick,包括 cosing learning rate等。