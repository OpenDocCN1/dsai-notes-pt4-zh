# AI+药物研发：药物筛选与优化

> 来源：[https://datawhaler.feishu.cn/docx/CQhvdyOlqo1kAqxdT0ncALdonHf](https://datawhaler.feishu.cn/docx/CQhvdyOlqo1kAqxdT0ncALdonHf)

# 首届AI4S Cup学习赛 - 中枢神经系统药物研发:药物筛选与优化

> **h**ttps://nb.bohrium.dp.tech/competitions/detail/9169114995?tab=introduce

> **主办单位：北京科学智能研究院，举办单位：Data**whale，Deepmodeling社区

随着 Al for Science (AI4S) 从愿景走向流行,不同领域、不同背景的人们以一种前所未有的方式连接在了一起。我们共同推动着科学研究与工业研发的持续革新、从而深刻改变世界。AI4S Cup 系列比赛是由北京科学智能研究院主办的AI4S领域的重要赛事,通过提供真实的科学场景和数据,以比赛的形式促进选手们的交流与碰撞,继而推动AI for Science的发展。在这里,你可以在属于你的科学领域里大显身手,与众多志同道合的小伙伴一起,以AI为笔,谱写Science的新篇章吧!

学习赛将长期开放，旨在提供一次学习AI for science知识，上手实践具体案例的机会。本次赛题以药物研发为切入点，选手需要以AI为工具，进行药物筛选与优化的实践。为了帮助大家更好上手本次比赛，我们会提供学习资料，补充机器学习知识和必要的科学知识，确保你获得一场完整的学习赛体验。

多维表格（Y1aubQReXasE5YsanCGcy0Ssnqc_tblGQw0HInMW15if）

# 任务0：比赛报名与 Python 语法复习

## 在赛事平台报名比赛

#### 1.[Bohrium 案例广场](https://nb.bohrium.dp.tech/competitions/detail/9169114995?tab=introduce)点击此链接进行赛事报名，如下图：

#### 2.Python 基础薄弱的同学可以在  观看 Python 语法复习直播

## 直播回放

首届AI4S CUP学习赛-开营仪式_哔哩哔哩_bilibili

[首届AI4S CUP学习赛-第一讲：Pyth](https://www.bilibili.com/video/BV1Dz4y1F7pb/)on语法复习_哔哩哔哩_bilibili

# 任务1：理解赛题与跑通 Baseline

> **CNS 药物预测赛：****h**ttps://nb.bohrium.dp.tech/competitions/detail/9169114995?tab=introduce

> 举办方：Datawhale X Deepmodeling

> 官方 Baseline：https://nb.bohrium.dp.tech/detail/1151401039

> 选手操作指南：[https://nb.bohrium.dp.te](https://nb.bohrium.dp.tech/detail/2812984565)ch/detail/2812984565

> 提交参考：[AI4S Cup成绩提交参考文档](https://n1ucktqeg5i.feishu.cn/docx/PmEVd25uBoYjcqxRO74cqUDVnwA)

## **赛题背景**

统计数据显示，到2020年，美国65岁以上的人口占总人口的20%以上。中枢神经系统（CNS）是受衰老影响的主要区域。预计老年中枢神经系统疾病，例如阿尔茨海默病（AD）、帕金森病（PD）、脑癌和中风，治疗费用将达到数万亿美元。医疗保健支出将从目前的国内生产总值（GDP）的约15%（14.6万亿美元）增加到2040年的约29%（http://www.nber.org/aginghealth/2008no4/w14361.html），因此，获得高质量、有效的中枢神经系统药物仍然是制药行业的重要责任。​

在 CNS 药物发现过程中，实现适当的血脑屏障（BBB）渗透通常被认为是一个重要障碍，另一方面，BBB 渗透可能对许多非CNS药物靶点构成负担。​

明确了解中枢神经系统（CNS）药物和非中枢神经系统（non-CNS）口服药物在众多理化性质方面差异的理解可能有助于两个研究领域，这些性质可以通过常用的分子建模软件计算。

中枢神经系统包括大脑和脊髓，负责处理和调控身体的各种功能。非中枢神经系统则包括神经元以外的组织，如内分泌系统、免疫系统等。​

鉴于血脑屏障及血脑屏障上各类转运体的存在，中枢神经系统用药往往需要满足一些特定的特征。药物化学家会基于特定的指标判断一个药物是否有成为中枢神经系统药物的可能，如分子量和拓扑极性表面积 (TPSA) 不宜过大，N原子和极性H原子不宜过多，溶剂可及表面在特定范围内等，这些特征在传统意义上为药物发现和优化阶段考虑分子作为CNS药物的潜力提供启发。

## 赛题解析

### **1.数据说明**

赛题数据由700条训练集、367条测试集数据组成，训练集与测试集数据均为CSV格式文件，共有2个字段，分别是SMILES分子式与分类标签TARGET。

### **2.评估指标**

在机器学习二分类问题中，我们通常会用以下四个指标来衡量：

1.真正例（True Positives,TP）:预测为正，实际也为正；

2.假正例（False Positives,FP）:预测为正，实际为负；

3.真负例（True Negatives,TN）:预测为负，实际也为负；

4.假负例（False Negatives,FN）:预测为负，实际为正；

精确率（Precision）:指在所有被模型预测为正的样本中，有多少是真正的正例。

故 Precision = TP/(TP+FP)；

召回率（Recall）: 指在所有真正的正例中，有多少被模型成功地预测为正例。

故 Recall = TP/(TP+FN)；

为了综合权衡精确率和召回率这两个指标，我们会使用新的指标F1-score来进行评价，公式如下：

F1-score = [(1+β∧2)*Precision*Recall]/β∧2*Precision+Recall）

而在本次赛题针对药物的检测中，我们要在保证精确率的同时，尽量要提高召回率，因此召回率的权重要高于精准率，故令β的值要大于1。

我们取β=2，即得到F2-score = [5*Precision*Recall]/4*Precision+Recall）。

将在本赛题中采用F2-score进行评价（F2-score分数越高，效果也就更好，即表示我们的模型性能很好，模型在正确预测正例方面做得很好，同时尽量减少了假负例现象）。

### 3.**赛事任务**

根据赛题数据，我们要通过SMILES分子式来判断该药物是否为CNS药物。

## 解题思路

本**次赛事为A**I for Science赛事，聚焦医疗领域，旨在通过AI技术预测药物类别，从而降低不断上涨的医疗保健成本。本次赛题实质上为二分类任务，常规思路为使用机器学习，如LightGBM、XGBoost，或者使用深度学习进行实践，如GNN。

我们此次基础版Baseline将采用机器学习中的LightGBM进行模型训练。

> **如果你是第一次接**触机器学习，可以参考以下书籍了解一下什么是机器学习？

> [机器学习实战：基于Scikit-Learn、Ker](https://weread.qq.com/web/appreader/e8e327a07208f4d1e8ec475)as和TensorFlow(原书第2版) （前三章）

> 我们提供了以下视频和文档来帮助大家了解一下关于机器学习的竞赛流程。

> 文字教程：

> [Datawhale *  深势科技 教学赛-----机器学习（ML）竞赛流程](https://o1zam58jk40.feishu.cn/docx/G28HdiQLjo0y2ixNfnpcfLYintR)

> [竞赛实践路线分享](https://datawhaler.feishu.cn/docx/EJ2Edl0hXoIWwuxO15CcEj9Wnxn)

> 视频：

> [AI夏令营第三期开营仪式 | 竞赛](https://www.bilibili.com/video/BV1KP411s7Sv/?spm_id_from=333.337.search-card.all.click&vd_source=f2b5a3acec302f2de2a0295d4c313eeb)通用流程分享_哔哩哔哩_bilibili

> AI夏令营第二期开营仪式_哔哩哔哩_bilibili

## （基础版）Baseline代码精读

**安装所需第三方库**

```
!pip install lightgbm numpy pandas rdkit scikit-learn #在终端用pip(不用写感叹号)下载lightgbm库、numpy库、pandas库、rdkit库、scikit-learn库
```

**导入所需的库**

```
import lightgbm as lgb  # 导入机器学习模型LightGBM
import numpy as np  # 导入数值计算库NumPy
import pandas as pd  # 导入数据处理库Pandas

from rdkit import Chem  # 导入RDKit中的Chem模块，用于分子对象转换
from rdkit.Chem.rdMolDescriptors import GetMorganFingerprintAsBitVect  # 从RDKit中导入GetMorganFingerprintAsBitVect函数，用于生成分子指纹（位向量转换）

from sklearn.model_selection import train_test_split  # 从scikit-learn中导入train_test_split函数，用于拆分数据集
from sklearn.metrics import fbeta_score  # 从scikit-learn中导入fbeta_score函数，用于 F2 Score 计算
```

**加载所需的数据**

+   使用 Pandas 库的 **read_csv()** 函数分别读取训练集和测试集的数据。

+   **拆分训练数据为训练集与验证集**的目的是使用训练集进行模型的训练，使用验证集来评估模型的性能和调整模型的超参数，提高模型的泛化能力，从而防止过拟合。

+   **设置固定随机种子**的目的是为了保证每次运行代码时得到的随机结果是一样的，使得实验结果更加可靠。

```
DATA_PATH = &quot;&lt;设置为实际数据路径&gt;&quot;  # 数据路径

raw_data = pd.read_csv(f&quot;{DATA_PATH}/mol_train.csv&quot;)  # 读入训练数据
test_data = pd.read_csv(f&quot;{DATA_PATH}/mol_test.csv&quot;)  # 读入测试数据

# 拆分训练数据为训练集与验证集，验证集占比 20%，设定固定随机种子
train_data, valid_data = train_test_split(
    raw_data, test_size=0.2, random_state=hash(&quot;Datawhale&quot;) % 2023
)
```

**数据处理**

+   **定义****sm**ile2fingerprint函数，该函**数的目的是将 SMILES 分**子式转换为分子指纹。当调用该函数时，它使用MolFromSmiles() 将输入的 SMILES 分子式字符串作为参数，并将其转换为分子对象 mol，然后使用 GetMorganFingerprintAsBitVect 函数生成了一个分子式的 Morgan 指纹位向量并返回该向量。

+   **Morgan 指纹**是一种流行的分子指纹技术，可用于描述分子的结构和特征。

+   **Get**MorganFingerprintAsBitVect() 函数生成分子式的 Morgan 指纹位向量。该函数的第一个参数是分子对象 mol，第二个参数是考虑半径 r，默认为 2，第三个参数是指纹位向量的长度 nBits，默认为 2048。

```
def smile2fingerprint(smile: str):
    &quot;&quot;&quot;将 SMILE 分子式表示为指纹数据

    参数:
        smile (string): SMILE 分子式

    返回:
        fp (Explict BitVect): 分子式的 Morgan 指纹位向量
    &quot;&quot;&quot;
    molecular = Chem.MolFromSmiles(smile)  # 将字符串转换为分子式对象
    finger_print = GetMorganFingerprintAsBitVect(molecular, 5, nBits=1024)  # 获得分子式的 Morgan 指纹位向量
    return finger_print
```

+   批量将 SMILES 分子式的指纹位向量转换为特征矩阵，并存储在 train_X、valid_X 和 test_X 变量中。

```
# 批量将位向量转换为特征矩阵，形状为 (n, 1024)，n 代表数据个数，1024 在位向量转换时指定
train_X = np.array([smile2fingerprint(smile) for smile in train_data[&quot;SMILES&quot;]])
valid_X = np.array([smile2fingerprint(smile) for smile in valid_data[&quot;SMILES&quot;]])
test_X = np.array([smile2fingerprint(smile) for smile in test_data[&quot;SMILES&quot;]])
```

+   将特征矩阵转换为 LightGBM 指定的数据格式，并创建训练集 lgb_train 和验证集 lgb_valid。

+   **LightGBM **是一种高效的梯度提升决策树（Gradient Boosting Decision Tree）框架。

```
# 将数据特征矩阵转换为 LightGBM 指定格式，(特征向量，对应标签)
lgb_train = lgb.Dataset(train_X, label=train_data[&quot;TARGET&quot;])
lgb_valid = lgb.Dataset(valid_X, label=valid_data[&quot;TARGET&quot;])
```

**模型训练**

```
# 设定 LightGBM 训练参，查阅参数意义：https://lightgbm.readthedocs.io/en/latest/Parameters.html
lgb_params = {
    &quot;objective&quot;: &quot;binary&quot;,  # 指定任务类别为二分类
    &quot;seed&quot;: hash(&quot;Datawhale&quot;) % 2023,  # 设定随机种子
    &quot;verbose&quot;: -1,  # 禁用输出（可选）
}

# 训练模型，参数依次为：导入模型设定参数、导入训练集、设定模型迭代次数（100）、导入验证集
model = lgb.train(lgb_params, lgb_train, num_boost_round=100, valid_sets=lgb_valid)
```

+   分类的阈值设置为 0.5，即模型输出大于等于 0.5 的样本被判定为正类，而小于 0.5 的样本被判定为负类。

```
threshold = 0.5 # 模型输出的是类别概率，设定概率的判断阙值
```

**模型预测并生成标签**

```
# 用验证集进行模型预测（选择训练中最好的一次）
valid_pred = model.predict(valid_X, num_iteration=model.best_iteration)
# 生成预测标签结果，如果概率大于阈值则为 1，否则为 0
valid_result = [1 if x &gt; threshold else 0 for x in valid_pred]
# 计算验证集 F2 Score 分数
valid_score = fbeta_score(valid_data[&quot;TARGET&quot;], valid_result, beta=2)
print(f&quot;Valid Score: {valid_score}&quot;)
```

```
# 预测测试集数据并获得预测结果
pred = model.predict(test_X, num_iteration=model.best_iteration)
result = [1 if x &gt; threshold else 0 for x in pred]
```

**保存预测结果**

```
submission = pd.DataFrame()  # 创建预测结果 DataFrame
submission[&quot;SMILES&quot;] = test_data[&quot;SMILES&quot;]  # 复制对应的 SMILES 内容
submission[&quot;TARGET&quot;] = result  # 填写预测标签结果
submission.to_csv(&quot;./submission.csv&quot;, index=False)  # 保存最后的预测结果到 submission.csv
```

## 直播回放

[首届AI4S CUP学习赛-第二讲：赛题理解及](https://www.bilibili.com/video/BV1YC4y1j7nK)科学知识补充_哔哩哔哩_bilibili

# 任务2：特征探索与构建

特征工程指的就是把原始数据转变为模型训练数据的过程，目的是获取更好的训练数据特征。

特征工程能使得模型的性能得到提升，有时甚至在简单的模型上也能取得不错的效果。

> 那接下来让我们通过以下视频和文档来进行特征工程的学习，我们要学会如何提取好的特征并选用从而提高我们模型的F2-score分数。

> 文字教程：

> [特征工程](https://b1gvojr92cg.feishu.cn/docx/YMEjdwIVuoSo3Fxx5kFc3DCvnEc)

> 视频：

> [2023SEED大赛 - 新能源](https://www.bilibili.com/video/BV1WN4y1k7R1/?vd_source=f2b5a3acec302f2de2a0295d4c313eeb)赛道特征工程_哔哩哔哩_bilibili

# 任务3：模型迭代优化

[官方Baseline解析](https://iaspooaf477.feishu.cn/docx/Q7jVdiGZao3ua2xBTiccCKWrnxb)了进一步提高我们模型的F2-score分数，接下来通过文档  要学习模型融合和模型交叉验证的方法。

通过以上方法，我们就学会了如何使用机器学习中LightGBM进行二分类任务的技能，而在机器学习中，我们还能使用XGBoost进行二分类任务，那就让我们通过以下视频和文档了解一下XGBoost吧。

> Lightgbm 与 XGBoost 数学原理解读

> [【AI夏令营】LightGBM的](https://www.bilibili.com/video/BV1eM4y1p7t6/?spm_id_from=333.788&vd_source=f2b5a3acec302f2de2a0295d4c313eeb)数学原理解读_哔哩哔哩_bilibili Datawhale 贡献

> 【机器学习】XGBoost数学原理及详细实现过程-CSDN博客 Datawhale 贡献

# 任务4：欢迎来到 GNN 的世界

相信通过以上的学习，大家都掌握了使用机器学习进行二分类任务的技术，那除了用机器学习进行二分类任务，我们也可以用深度学习来进行二分类任务，比如使用GNN,那就接下来让我们一起走进图神经网络GNN的学习之旅吧！

> 首先让我们通过学习以下视频和文档明白什么是GNN，它的原理是什么，如何使用GNN等知识吧，我们一起学起来吧！

> 文字教程：

> 图神经网络 (GNN)-CSDN博客 Datawhale 贡献

> 视频：

> [斯坦福CS224W图机器学习、图神经网络、知识图谱【](https://www.bilibili.com/video/BV1pR4y1S7GA/?spm_id_from=333.337.search-card.all.click&vd_source=f2b5a3acec302f2de2a0295d4c313eeb)同济子豪兄】_哔哩哔哩_bilibili Datawhale 贡献

> [零基础多图详解图神经网络（GNN/GCN）](https://www.bilibili.com/video/BV1iT4y1d7zP/?vd_source=f2b5a3acec302f2de2a0295d4c313e)【论文精读】_哔哩哔哩_bilibili

> 书籍：

> [图深度学习(葡萄书)](https://datawhalechina.github.io/grape-book/#/) Datawhale 贡献

> [GNNBook@2023](https://graph-neural-networks.github.io/index.html)

# 任务5：上分就对辣！

运用所有你学到的知识，疯狂上分吧！

# 附录：

赛事成绩提交参考文档：[AI4S Cup成绩提交参考文档](https://n1ucktqeg5i.feishu.cn/docx/PmEVd25uBoYjcqxRO74cqUDVnwA)