
这里的损失函数使用的是负对数似然（Negative Log-Likelihood, NLL）算法。

NLL 源于最大似然估计（MLE），它是统计学中的一个重要方法。通过最大化似然函数来估计模型参数，使得数据的生成概率最大化。这种理论背景使得 NLL 损失具有坚实的统计学基础。在大样本情况下，NLL 损失的最小化将导致估计的模型参数趋于真实参数，从而使模型更接近真实数据生成分布。

NLL 损失不仅适用于二分类问题，也适用于多分类问题。在多分类情况下，它计算了目标类别的对数概率，并通过负号转化为损失，用于训练和评估模型，使得模型的预测尽可能接近真实数据。

对于多分类问题，交叉熵损失函数是计算 NLL 的一种常见方式。具体地，交叉熵损失函数用于衡量真实标签的概率分布与模型预测概率分布之间的差距。计算步骤包括：

1. 调整 `logits`：为了避免数值不稳定性，通常会先从 `logits` 中减去最大值。
1. 计算指数：对调整后的 `logits` 应用指数函数，得到每个类别的指数值。
1. 计算概率：通过归一化（即将每个指数值除以所有指数值的总和）得到类别概率。
1. 对数计算：对目标类别的概率取对数。
1. 负值：取对数概率的负值，得到 `NLL`。

## 评估数据集损失

```
// 评估数据集 split 的损失
double eval_split(MLP& model, const std::vector<std::pair<std::vector<double>, int>>& split) {
    // 初始化损失值为 0
    Value loss(0);
    
    // 遍历数据集中的每一个样本
    for (const auto& sample : split) {
        // 获取样本的特征向量 x 和真实标签 y
        const std::vector<double>& x = sample.first;
        int y = sample.second;
        
        // 将特征向量 x 转换为 Value 类型，并存储在 inputs 向量中
        std::vector<Value> inputs = { Value(x[0]), Value(x[1]) };
        
        // 使用模型对输入进行预测，得到 logits（预测的原始分数）
        std::vector<Value> logits = model(inputs);
        
        // 计算交叉熵损失
        Value cer = cross_entropy(logits, y);
        
        // 累加损失值
        loss = loss + cer;
    }
    
    // 对损失进行归一化处理，即将总损失除以样本数量
    loss = loss * (1.0 / split.size());
    
    // 返回归一化后的损失值
    return loss.getData();
}
```

### 解释

1. 初始化损失值：将 `loss` 变量初始化为 0，用于累加所有样本的损失。
1. 遍历数据集：遍历 `split` 中的每一个样本。每个样本由特征向量 `x` 和真实标签 `y` 组成。
1. 特征向量转换：将特征向量 `x` 中的每个值转换为 `Value` 类型，并存储在 `inputs` 向量中。
1. 模型预测：使用模型 `model` 对 `inputs` 进行预测，得到 `logits`（模型输出的原始分数）。
1. 计算交叉熵损失：使用 `cross_entropy` 函数计算当前样本的交叉熵损失 `cer`。
1. 累加损失值：将当前样本的损失值 `cer` 累加到总损失 `loss` 上。
1. 归一化损失：将总损失 `loss` 除以样本数量 `split.size()`，以计算平均损失。
1. 返回损失值：返回归一化后的损失值。

这个函数的作用是计算给定数据集 `split` 上模型的平均损失，以评估模型的性能。会在训练过程中每 10 步调用一次。

## 训练实验

```
int main() {
    // 数据生成
    // 生成数据集，100 是样本数量
    auto datasets = gen_data(_random, 100);
    std::vector<std::pair<std::vector<double>, int>> train_split = std::get<0>(datasets);  // 训练集
    std::vector<std::pair<std::vector<double>, int>> val_split  = std::get<1>(datasets); // 验证集
    std::vector<std::pair<std::vector<double>, int>> test_split  = std::get<2>(datasets); // 测试集

    // 创建一个具有 2 个输入节点，16 个隐藏层节点和 3 个输出节点的 MLP 模型
    MLP model(2, {16, 3});

    // 超参数设置
    double learning_rate = 1e-1;   // 学习率
    double beta1 = 0.9;           // Adam 优化器的 β1 参数
    double beta2 = 0.95;          // Adam 优化器的 β2 参数
    double weight_decay = 1e-4;   // 权重衰减（L2 正则化）

    // 参数初始化
    // 初始化模型参数的梯度、动量 m 和二次矩估计 v
    for (auto* p : model.parameters()) {
        p->grad = 0.0;  // 梯度初始化为 0
        p->m = 0.0;    // 动量初始化为 0
        p->v = 0.0;    // 二次矩初始化为 0
    }

    // 训练循环
    for (int step = 0; step < 1000; ++step) {
        if (step % 10 == 0) {
            // 每 10 步输出一次验证集上的损失
            double val_loss = eval_split(model, val_split);
            std::cout << "step " << step << ", val loss " << val_loss << std::endl;
        }

        // 计算训练集上的总损失
        Value loss(0.0);
        for (auto& data : train_split) {
            std::vector<Value> x = {Value(data.first[0]), Value(data.first[1])}; // 转换特征为 Value 类型
            std::vector<Value> logits = model(x); // 通过模型进行前向传播
            
            // 计算交叉熵损失
            Value ce(cross_entropy(logits, data.second));
            loss += ce; // 累加损失
        }
        // 计算平均损失
        loss = loss * (1.0 / train_split.size());

        // 反向传播计算梯度
        loss.backwardPass();

        // 参数更新（使用 AdamW 优化器）
        for (auto* p : model.parameters()) {
            // Adam 优化器的参数更新逻辑
            p->m = beta1 * p->m + (1 - beta1) * p->grad; // 更新动量
            p->v = beta2 * p->v + (1 - beta2) * p->grad * p->grad; // 更新二次矩
            double m_hat = p->m / (1 - pow(beta1, step + 1));  // 偏差修正
            double v_hat = p->v / (1 - pow(beta2, step + 1));  // 偏差修正
            // 更新参数值，加入权重衰减
            p->data -= learning_rate * (m_hat / (sqrt(v_hat) + 1e-8) + weight_decay * p->data);
        }

        // 清除所有参数的梯度
        model.zero_grad();

        // 输出训练集上的损失
        std::cout << "Step " << step << ", Train Loss: " << loss << std::endl;
    }

    return 0;
}
```

### 主要组件解释

1. 数据生成：
   + `gen_data(_random, 100)`：生成 100 个样本的数据集，并将其分为训练集、验证集和测试集。

1. 模型初始化：
   + `MLP model(2, {16, 3})`：初始化一个具有 2 个输入节点、一个隐藏层包含 16 个节点、输出层包含 3 个节点的多层感知机模型。

1. 超参数：
   + `learning_rate`、`beta1`、`beta2` 和 `weight_decay`：设置用于训练的学习率、`Adam` 优化器的 β1 和 β2 参数以及权重衰减（`L2` 正则化）。

1. 参数初始化：
   + 对模型的所有参数进行初始化，包括梯度、动量（`m`）和二次矩（`v`）。

1. 训练循环：
   + 每隔 10 步计算并输出验证集上的损失。
   + 对训练集进行前向传播，计算损失，进行反向传播，更新参数。

1. AdamW 优化器：
   + 使用 `Adam` 优化器更新模型参数，并在更新时加入权重衰减以防止过拟合。

1. 损失输出：
   + 每一步输出训练集上的损失，用于监控训练过程中的模型性能。

这个程序通过训练和优化 `MLP` 模型来最小化交叉熵损失函数，并使用 `AdamW` 优化器进行参数更新。

我们使用和 `Python` 版本相同的训练参数和随机种子，运行1000从训练过程，输出的结果应该是一样的：

![](img/a48a44c624612b9cb6256b25fbeac6fa.png)

## 总结

从以上代码可以看出 `C++` 相对于 `Python` 来说更加复杂，开发者需要处理诸如指针、内存管理和复杂的编译错误。与 `Python` 社区相比，`C++` 在深度学习领域的社区规模较小，相关的高层框架和工具支持相对较少，很多工具都要自己开发实现。

但 `C++` 最大的优点是性能比较高，在相同的数据集和训练参数配置下，都执行1000次训练，`C++` 版本耗时20s，`Python` 版本耗时接近120s，效率上相差6倍。所以 `C++` 更适合：性能要求高、大规模的计算密集型任务，资源受限的场景，其实许多底层深度学习库（如 `TensorFlow`、`PyTorch`）都使用 `C++` 来实现高效计算部分。
