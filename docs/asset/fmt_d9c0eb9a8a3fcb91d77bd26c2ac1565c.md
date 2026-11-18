
# 支持向量机

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/N3BYd3VFBo4SvmxZklfcrmfSn4c](https://qqs7y1hozd1.feishu.cn/docx/N3BYd3VFBo4SvmxZklfcrmfSn4c)

## 支持向量机 概述

支持向量机(Support Vector Machines, SVM): 是一种监督学习算法

+   支持向量(Support Vector)就是离分隔超平面最近的那些点

+   机(Machine)就是表示一种算法，而不是表示机器

## 支持向量机 场景

+   要给左右两边的点进行分类

+   明显发现: 选择D会比B、C分隔的效果要好很多

![](img/b61627cd6f3fd545102c29e3a1fd2956.png)

## 支持向量机 原理

### SVM 工作原理

![](img/5b7be9f6e74affd8447158711ce16077.png)

对于上述的苹果和香蕉，我们想象为2种水果类型的炸弹（保证距离最近的炸弹，距离它们最远）

1.  寻找最大分类间距

1.  转而通过拉格朗日函数求优化的问题

+   数据可以通过画一条直线就可以将它们完全分开，这组数据叫线性可分(linearly separable)数据，而这条分隔直线称为分隔超平面(separating hyperplane)

+   如果数据集上升到1024维呢？那么需要1023维来分隔数据集，也就说需要`N-1`维的对象来分隔，这个对象叫做超平面(hyperlane)，也就是分类的决策边界

![](img/99e2f05a4f96448135444cd95d5d2e8a.png)

### 寻找最大间隔

#### 为什么寻找最大间隔

```
摘录地址: `http://slideplayer.com/slide/8610144`  (第12条信息)
Support Vector Machines: Slide 12 Copyright © 2001, 2003, Andrew W. Moore Why Maximum Margin? 

1.Intuitively this feels safest. 
2.If we’ve made a small error in the location of the boundary (it’s been jolted in its perpendicular direction) this gives us least chance of causing a misclassification. 
3.CV is easy since the model is immune to removal of any non-support-vector datapoints. 
4.There’s some theory that this is a good thing. 
5.Empirically it works very very well. 

* * *

1. 直觉上是最安全的
2. 如果我们在边界的位置发生了一个小错误（它在垂直方向上被颠倒），这给我们最小的可能导致错误分类。
3. CV（cross validation 交叉验证）很容易，因为该模型对任何非支持向量数据点的去除是免疫的。
4. 有一些理论表明这是一件好东西。
5. 从经验角度上说它的效果非常非常好。
```

#### 怎么寻找最大间隔

> 点到超平面的距离

+   分隔超平面函数间距: `y(x)=wTx+b`

+   分类的结果: `f(x)=sign(wTx+b)` (`sign`表示`>0`为1，`<0`为-1，`=0`为0)

+   点到超平面的几何间距: `d(x)=(wTx+b)/||w||` （`||w||`表示`w`矩阵的二范数=> `wT∗w`, 点到超平面的距离也是类似的）

![](img/bb71d4e5ef5c321f8082041bcd8a0565.png)

> 拉格朗日乘子法

+   类别标签用-1、1，是为了后期方便 `label∗(wTx+b)` 的标识和距离计算；如果 `label∗(wTx+b)>0` 表示预测正确，否则预测错误。

+   现在目标很明确，就是要找到`w`和`b`，因此我们必须要找到最小间隔的数据点，也就是前面所说的支持向量。

+   也就说，让最小的距离取最大.(最小的距离: 就是最小间隔的数据点；最大: 就是最大间距，为了找出最优超平面--最终就是支持向量)

+   目标函数: `arg:maxw,b(min[label∗(wTx+b)]∗1||w||)`

1.  如果 `label∗(wTx+b)>0` 表示预测正确，也称函数间隔，$$||w||$$ 可以理解为归一化，也称几何间隔。

1.  令 `label∗(wTx+b)>=1`， 因为`0～1`之间，得到的点是存在误判的可能性，所以要保障 `min[label∗(wTx+b)]=1`，才能更好降低噪音数据影响。

1.  所以本质上是求 `arg:maxw,b1||w||`；也就说，我们约束(前提)条件是: `label∗(wTx+b)=1`

+   新的目标函数求解: `arg:maxw,b1||w||`

+   => 就是求: `arg:minw,b||w||` (求矩阵会比较麻烦，如果`x`只是 `12∗x2` 的偏导数，那么同样是求最小值)

+   => 就是求: `arg:minw,b(12∗||w||2)` (二次函数求导，求极值，平方也方便计算)

+   本质上就是求线性不等式的二次优化问题(求分隔超平面，等价于求解相应的凸二次规划问题)

+   通过拉格朗日乘子法，求二次优化问题

+   假设需要求极值的目标函数 (objective function) 为 `f(x,y)`，限制条件为 `φ(x,y)=M # M=1`

+   设`g(x,y)=M-φ(x,y)` # 临时`φ(x,y)`表示下文中 `label∗(wTx+b)`

+   定义一个新函数: `F(x,y,λ)=f(x,y)+λg(x,y)`

+   `a`为`λ`（`a>=0`），代表要引入的拉格朗日乘子(Lagrange multiplier)

+   那么: `L(w,b,α)=12∗||w||2+∑i=1nαi∗[1−label∗(wTx+b)]`

+   因为: `label∗(wTx+b)>=1`,`α>=0` , 所以 `α∗[1−label∗(wTx+b)]<=0` , `∑i=1nαi∗[1−label∗(wTx+b)]<=0`

+   当 `label∗(wTx+b)>1` 则 `α=0` ，表示该点为非支持向量

+   相当于求解: `maxαL(w,b,α)=12∗||w||2`

+   如果求: `minw,b12∗||w||2` , 也就是要求: `minw,b(maxαL(w,b,α))`

+   现在转化到对偶问题的求解

+   `minw,b(maxαL(w,b,α)) >= maxα(minw,b L(w,b,α))`

+   现在分2步

+   先求: `minw,bL(w,b,α)=12∗||w||2+∑i=1nαi∗[1−label∗(wTx+b)]`

+   就是求`L(w,b,a)`关于`[w, b]`的偏导数, 得到`w`和`b`的值，并化简为: `L`和`a`的方程。

+   参考: 如果公式推导还是不懂，也可以参考《统计学习方法》李航-P103<学习的对偶算法>

![](img/8e127800e976a9cffaf086413dfa67bb.png)

+   终于得到课本上的公式: `maxα(∑i=1mαi−12∑i,j=1mlabeli∗labelj∗αi∗αj∗<xi,xj>)`

+   约束条件: `a>=0` 并且 `∑i=1mai∗labeli=0`

> 松弛变量(slack variable)

参考地址: `http://blog.csdn.net/wusecaiyun/article/details/49659183`

![](img/288695012be0e8d250568ddda30f5f61.png)

+   我们知道几乎所有的数据都不那么干净, 通过引入松弛变量来 允许数据点可以处于分隔面错误的一侧

+   约束条件: `C>=a>=0` 并且 `∑i=1mai∗labeli=0`

+   总的来说:

+    表示 松弛变量

+   常量`C`是 惩罚因子, 表示离群点的权重（用于控制“最大化间隔”和“保证大部分点的函数间隔小于1.0” ）

+   `label∗(wTx+b)>1` and `alpha = 0` (在边界外，就是非支持向量)

+   `label∗(wTx+b)=1` and `0< alpha < C` (在分割超平面上，就支持向量)

+   `label∗(wTx+b)<1` and `alpha = C` (在分割超平面内，是误差点 -> `C`表示它该受到的惩罚因子程度)

+   参考地址: `https://www.zhihu.com/question/48351234/answer/110486455`

+   `C`值越大，表示离群点影响越大，就越容易过度拟合；反之有可能欠拟合

+   我们看到，目标函数控制了离群点的数目和程度，使大部分样本点仍然遵守限制条件

+   例如: 正类有10000个样本，而负类只给了100个（`C`越大表示100个负样本的影响越大，就会出现过度拟合，所以`C`决定了负样本对模型拟合程度的影响！，`C`就是一个非常关键的优化点！）

+   这一结论十分直接，SVM中的主要工作就是要求解 `alpha`

### SMO 高效优化算法

+   SVM有很多种实现，最流行的一种实现是: 序列最小优化(Sequential Minimal Optimization, SMO)算法

+   下面还会介绍一种称为 核函数(kernel) 的方式将SVM扩展到更多数据集上

+   注意: SVM几何含义比较直观，但其算法实现较复杂，牵扯大量数学公式的推导

> 序列最小优化(Sequential Minimal Optimization, SMO)

+   创建作者: John Platt

+   创建时间: 1996年

+   SMO用途: 用于训练 SVM

+   SMO目标: 求出一系列 `alpha` 和 `b`,一旦求出 `alpha`，就很容易计算出权重向量 `w` 并得到分隔超平面

+   SMO思想: 是将大优化问题分解为多个小优化问题来求解的

+   SMO原理: 每次循环选择两个 `alpha` 进行优化处理，一旦找出一对合适的 `alpha`，那么就增大一个同时减少一个。

+   这里指的合适必须要符合一定的条件

1.  这两个 `alpha` 必须要在间隔边界之外

1.  这两个 `alpha` 还没有进行过区间化处理或者不在边界上

+   之所以要同时改变2个 `alpha`；原因是我们有一个约束条件: `∑i=1mai∗labeli=0`；如果只是修改一个 `alpha`，很可能导致约束条件失效

> SMO 伪代码大致如下:

```
创建一个 alpha 向量并将其初始化为0向量
当迭代次数小于最大迭代次数时(外循环)
    对数据集中的每个数据向量(内循环): 
        如果该数据向量可以被优化
            随机选择另外一个数据向量
            同时优化这两个向量
            如果两个向量都不能被优化，退出内循环
    如果所有向量都没被优化，增加迭代数目，继续下一次循环
```

### SVM 支持向量机流程

```
收集数据: 可以使用任意方法
准备数据: 需要数值型数据
分析数据: 有助于可视化分隔超平面
训练算法: SVM的大部分时间都源自训练，该过程主要实现两个参数的调优
测试算法: 十分简单的计算过程就可以实现
使用算法: 几乎所有分类问题都可以使用SVM，值得一提的是，SVM本身是一个二类分类器，对多类问题应用SVM需要对代码做一些修改
```

### SVM 算法特点

```
优点: 泛化（由具体的、个别的扩大为一般的，就是说: 模型训练完后的新样本）错误率低，计算开销不大，结果易理解
缺点: 对参数调节和核函数的选择敏感，原始分类器不加修改仅适合于处理二分类问题
使用数据类型: 数值型和标称型数据
```

## 支持向量机算法案例（无核函数）

### 项目概述

对小规模数据点进行分类

### 项目流程

#### 收集数据

文本文件格式:

```
3.542485        1.977398        -1
3.018896        2.556416        -1
7.551510        -1.580030        1
2.114999        -0.004466        -1
8.127113        1.274372        1
```

#### 准备数据

```
def loadDataSet(fileName):
    """
    对文件进行逐行解析，从而得到第行的类标签和整个特征矩阵
    Args:
        fileName 文件名
    Returns:
        dataMat  特征矩阵
        labelMat 类标签
    """
    dataMat = []
    labelMat = []
    fr = open(fileName)
    for line in fr.readlines():
        lineArr = line.strip().split('\t')
        dataMat.append([float(lineArr[0]), float(lineArr[1])])
        labelMat.append(float(lineArr[2]))
    return dataMat, labelMat
```

#### 分析数据: 无

#### 训练算法
