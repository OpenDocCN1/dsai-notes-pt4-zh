
# 101N0202 Micrograd C++ 代码自发实现

> 来源：https://v11enp9ok1h.feishu.cn/docx/ODkCdeSf4oNFfrx2Ignc6aZjnEf

## 引言

本文是对`micrograd` C++ 版本的代码解读。`micrograd` 项目是 Andrej Karpathy 101N 课程的一部分，旨在构建一个轻量级的自动微分引擎，帮助我们理解自动微分、梯度下降、反向传播和神经网络训练的基本原理。原项目是使用 Python 实现，我们自发对 101n 课程做了扩展，使用 C++ 复现了 `micrograd` 的核心代码，以便让大家了解 C++ 和 Python 在代码实现方式上的差异。下面让我开始一步步对代码解读。

> 阅读 Tips：阅读本文需要准备一些高数微积分基础知识。`C++11` 基础知识。

## 代码主要结构

整个代码只有一个 `micrograd.cpp` 文件，通过面向对象的方式实现了 Python 版本中的各个模块：

> `RNG` 类：自定义的随机数生成器，保证相同的 `seed` 情况下，生成的数据相同。

> `Value` 类：整个自动求导引擎的核心。它存储一个标量值及其梯度，并定义了基本的数学运算。

> `Module` 类：定义了神经网络模块的基本接口，是 `Neuron` `Layer` `MLP` 的基类。

> `Neuron` 类：实现单个神经元。

> `Layer` 类：实现神经网络层。

> `MLP` 类：实现了一个多层感知机。

> `main` 方法：主程序入口，使用上面的对象和工具，构建了一个具有 2 个输入节点，16 个隐藏层节点和 3 个输出节点的神经网络，跑通了整个训练流程。

以上 C++ 对象，和 Python 版本实现一一对应，都能在 Python 版本中找到相应的实现对象。

## 代码解读

### `RNG` 类

```
class RNG {
private:
    uint64_t state; // 随机数生成器的内部状态

public:
    // 构造函数，初始化随机数生成器的状态
    RNG(uint64_t seed) : state(seed) {}

    // 生成一个 32 位的随机整数
    uint32_t random_u32() {
        // 使用 xorshift 算法生成随机数
        state ^= (state >> 12) & 0xFFFFFFFFFFFFFFFF;
        state ^= (state << 25) & 0xFFFFFFFFFFFFFFFF;
        state ^= (state >> 27) & 0xFFFFFFFFFFFFFFFF;
        // 返回生成的 32 位随机数
        return static_cast<uint32_t>((state * 0x2545F4914F6CDD1D) >> 32);
    }

    // 生成一个 [0, 1) 区间内的随机浮点数
    float random() {
        // 将 32 位随机数右移 8 位后，除以 16777216.0 来生成 [0, 1) 区间内的随机浮点数
        return (random_u32() >> 8) / 16777216.0f;
    }

    // 生成一个 [a, b) 区间内的随机浮点数
    float uniform(float a = 0.0f, float b = 1.0f) {
        // 使用 [0, 1) 的随机数生成 [a, b) 区间内的随机浮点数
        return a + (b - a) * random();
    }
};

// 生成训练集、验证集和测试集
std::tuple<std::vector<std::pair<std::vector<double>, int>>,
           std::vector<std::pair<std::vector<double>, int>>,
           std::vector<std::pair<std::vector<double>, int>>>
gen_data(RNG &random, int n = 100) {
    // 定义存储数据点的向量
    std::vector<std::pair<std::vector<double>, int>> pts;

    // 生成 n 个随机数据点
    for (int i = 0; i < n; ++i) {
        // 生成 x 和 y 坐标，范围在 [-2.0, 2.0)
        float x = random.uniform(-2.0f, 2.0f);
        float y = random.uniform(-2.0f, 2.0f);

        // 根据 x 和 y 的位置确定标签
        int label = (x < 0) ? 0 : (y < 0) ? 1 : 2;

        // 将生成的点（向量和标签）存入 pts 向量中
        pts.emplace_back(std::vector<double>{x, y}, label);
    }

    // 计算训练集、验证集和测试集的大小 (80%, 10%, 10%)
    int tr_size = static_cast<int>(0.8 * n); // 训练集大小
    int val_size = static_cast<int>(0.1 * n); // 验证集大小

    // 将数据分为训练集、验证集和测试集
    std::vector<std::pair<std::vector<double>, int>> tr(pts.begin(), pts.begin() + tr_size); // 训练集
    std::vector<std::pair<std::vector<double>, int>> val(pts.begin() + tr_size, pts.begin() + tr_size + val_size); // 验证集
    std::vector<std::pair<std::vector<double>, int>> te(pts.begin() + tr_size + val_size, pts.end()); // 测试集

    // 返回训练集、验证集和测试集
    return std::make_tuple(tr, val, te);
}
```

> `RNG` 类：实现了一个简单的随机数生成器，使用 xorshift 算法生成随机数。这个类可以生成 32 位的随机整数，也可以生成 [0, 1) 区间内的随机浮点数。

> `gen_data` 函数：调用 `RNG` 类，生成 n 个随机的二维点，每个点根据其 x 和 y 坐标位置被赋予不同的标签（0、1 或 2）。然后，这些点被划分为训练集（80%）、验证集（10%）和测试集（10%）。

> 为什么要自己实现一个随机数生成器？

> 主要是为了可重复性和一致性。我们的项目主要是以学习与研究为目的，自己实现一个可控的随机数生成器，可以方便每次生成相同的数据集和网络权重，方便我们重复调试、跟踪 bug、重现实验结果，帮助开发者理解更底层的细节。

> 在原项目中，作者还实现了一个 `Pytorch.py` 版本，使用相同的数据集和网络权重，可以通过对比两个版本的输出结果是否一致，来校验两个版本的程序是否正确。

> 同样的，要验证我们的 C++ 版本改写的是否正确，也可以使用相同的数据集和网络权重参数以及训练超参数，来校验 C++ 版本和 Python 版本的输出结果是否一致来判断程序的正确性。

### `Value` 类

#### 成员变量

```
class Value {
public:
    double m;    // 一阶矩
    double v;    // 二阶矩
    double data; // 存储值
    double grad; // 存储梯度
    std::set<Value*> prev; // 存储前驱节点
    std::string op; // 操作符名称
    std::function<void(Value&)> backward; // 反向传播函数
}
```

> `m`、`v` 变量主要用于自适应学习率的调整，尤其是在 Adam 优化器中，可以记录梯度的平均值和变化幅度，帮助调整学习率，防止在陡峭的区域更新过大。后面的代码中，神经网络参数更新时，会有具体的使用方法，实现了 Adam 优化器算法。

> Python 版本中 `m`、`v` 是在训练时动态初始化的，这得益于 Python 语言的灵活性，可以在实例化对象后，随时为该对象添加新的属性。C++ 语言比较严谨，必须在类定义中明确定义好成员变量，后面才能使用。

#### 构造函数

```
Value(double data, std::initializer_list<Value*> children = {}, const std::string& op = "")
        : data(data), grad(0), op(op) {
        for (auto child : children) {
            prev.insert(child);
        }
        backward = [this](Value&) { }; // Default backward function does nothing
    }

    Value(const Value& other): data(other.data), grad(other.grad), prev(other.prev), op(other.op), backward(other.backward) {

    }
```

> `Value` 类主要实现了两个构造函数，需要注意的是拷贝构造函数。

#### 重载运算操作符

C++ 主要通过其自身的运算符重载机制，完成了 `Value` 类加减乘除运算，其中还包括 `+=`、`-=`、`*=`、`/=` 运算。

##### `+`、`+=` 运算

```
// 实现 Value 类的加法操作符重载
Value operator+(Value const& other) {
    // 创建一个新的 Value 对象 out，data 为 this 和 other 对象的 data 之和
    // 并将 this 和 other 的指针作为前驱节点传递给 out，用于梯度传播
    Value out(data + other.data, {this, &const_cast<Value&>(other)}, "+");

    // 设置 out 的反向传播函数
    out.setBackward([this, &other](Value& out) {
        // this 的梯度加上 out 的梯度，进行链式法则的梯度传递
        this->addGrad(out.getGrad());
        // other 的梯度也加上 out 的梯度
        const_cast<Value&>(other).addGrad(out.getGrad());
    });

    // 返回新生成的 Value 对象
    return out;
}

// 实现 Value 类的加等操作符重载
Value& operator+=(const Value& other) {
    // 创建当前对象（this）的副本 originalThis，用于保存当前状态
    Value* originalThis = new Value(*this);

    // 将 this 对象的 data 加上 other 的 data，完成加等操作
    this->data += other.data;

    // 设置操作符为 "+="，用于标记当前操作
    this->op = "+=";

    // 将 originalThis 作为前驱节点插入 this->prev 集合中
    this->prev.insert(originalThis);
    // 将 other 作为前驱节点插入 this->prev 集合中
    this->prev.insert(&const_cast<Value&>(other));

    // 设置反向传播函数
    this->setBackward([originalThis, &other](Value& out) {
        // originalThis 的梯度加上 out 的梯度
        originalThis->addGrad(out.getGrad());
        // other 的梯度加上 out 的梯度
        const_cast<Value&>(other).addGrad(out.getGrad());
    });

    // 返回修改后的当前对象
    return *this;
}
```

> 加法的梯度

> 我们可以把梯度理解为“变化的速度”。假设你有两个数，`a` 和 `b`，它们的和是 `c`。也就是说，`c = a + b`。

> 如果 `a` 增加了 1，`c` 会增加多少？答案是 1。同样的，如果 `b` 增加了 1，`c` 也会增加 1。因此，`c` 对 `a` 和 `b` 的变化都是 1，这就是所谓的梯度为 1。

> 简单来说，`a` 和 `b` 都对 `c` 有同样的影响，每增加一点，`c` 就增加一点。

> 在代码里，`self.grad` 是 `a` 的变化率，`other.grad` 是 `b` 的变化率，`out.grad` 是结果 `out` 的变化率。因为 `c = a + b`，`a` 和 `b` 对 `c` 的影响是一样的（变化率都是 1）。所以为了计算 `a` 和 `b` 的梯度，你需要把 `out` 的梯度加到它们各自的梯度上。简单来说，就是把结果的影响“传递”回 `a` 和 `b`。

##### `-`、`-=` 运算

