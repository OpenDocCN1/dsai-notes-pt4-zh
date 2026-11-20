
# Web3 共识协议

> 来源：[https://docs.feishu.cn/docx/Ic1Ud5cTloGj4Jx5M76cjSXunne](https://docs.feishu.cn/docx/Ic1Ud5cTloGj4Jx5M76cjSXunne)

我们能不能在去中心化下确保`Trust`

如何使用协议确保`Ledger`的consistency

![](img/0e1f7dfd83778aff249fb58140243c10.png)

## 分布式系统

+ 同步独立执行
+ 缺少世界时钟
+ 每个部件都可以出现错误

![](img/626e7308838fea6a38f0bddf0576a9e6.png)

+ 网络是reliability
+ 时间模型
+ 同步
+ 异步

## 错误类型

+ Benign faults
+ 大部分是机器的问题，节点is benign
+ Byzantine faults
+ 恶意节点
+ BAR faults
+ 理性节点，时好时坏，为了自己利益

![](img/18fd9a737b24c045b31abb911c8cd49d.png)

## 共识的定义

+ agreement：pay attention to non-faulty
+ termination

![](img/78124d6ac0309ea989c672e35f1bed9e.png)

![](img/678682a2b8b9ca5329c0f12162b117f5.png)

## 最简单的协议

但是现实存在恶意节点——Byzantine Generals Problem

![](img/320e93e1d767737c35a51dc35cfa8005.png)

`relay`

![](img/5660c32bd451e60bab11b49429c594ce.png)

+ 三个将军的时候，如果存在一个坏人，共识就是不可能的
+ 共识小于1/3，才有可能
+ 忠诚节点，信息通路一定要达到一定程度

![](img/927d520e74d68f3eaaa3222ebb130653.png)

## FLP不可能

以下三个条件三个里，如果想达成共识协议，不能同时都取

+ Determinism确定性：每一次执行都是一样的
+ Asynchrony异步
+ Fault-tolerance安全性，没有恶意节点
+ Necessity of a practical distributed ledger system: fault-tolerance

## 共识算法框架

![](img/799691959edc2a58f3152b7a62cfd318.png)

![](img/631f1221230238ddf5f24d2394f5aebf.png)

![](img/6e10be20705e719ebdca965861be2abf.png)
