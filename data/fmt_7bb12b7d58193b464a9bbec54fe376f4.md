
# Web3 共识协议

> 来源：[https://docs.feishu.cn/docx/OznCdOnsOoI1mHx5NfncvkCRnl8](https://docs.feishu.cn/docx/OznCdOnsOoI1mHx5NfncvkCRnl8)

## 引言

我们能不能在去中心化下确保‘Trust’

如何使用协议确保Ledger的consistency

![](img/22d9f23cc60570cea49fce1adf5aa0e2.png)

## 分布式系统

- 同步独立执行
- 缺少世界时钟
- 每个部件都可以出现错误

![](img/94bfabbfe941d4e28d5226b067a611af.png)

- 网络是reliability
- 时间模型
- 同步
- 异步

## 错误类型

- Benign faults
- 大部分是机器的问题，节点is benign
- Byzantine faults
- 恶意节点
- BAR faults
- 理性节点，时好时坏，为了自己利益

![](img/50f9d605598544bc4999be82e63e34d9.png)

## 共识的定义

- agreement：pay attention to non-faulty
- termination

![](img/3325986455c96f220e4a0ff03d627025.png)

![](img/2f756de0c632ba842222fafd6d164818.png)

## 最简单的协议

但是现实存在恶意节点——Byzantine Generals Problem

![](img/7018808fd9c53d277d4ccd2eed04abb3.png)

relay

![](img/811dce13f06ab4e968458f6aef5e58fc.png)

- 三个将军的时候，如果存在一个坏人，共识就是不可能的
- 共识小于1/3，才有可能
- 忠诚节点，信息通路一定要达到一定程度

![](img/b325ac1bf4b1e341c1dc3591c66f506d.png)

## FLP 不可能

以下三个条件三个里，如果想达成共识协议，不能同时都取

- Determinism确定性：每一次执行都是一样的
- Asynchrony异步
- Fault-tolerance安全性，没有恶意节点
- Necessity of a practical distributed ledger system: fault-tolerance

## 共识算法框架

![](img/2ba48a4c7298817ec507fe5b2a3fc37d.png)

![](img/b3d805dbd6fb560d99b33c4484258814.png)

![](img/8c33c5a6ad8901d54dad79bfc0048e3d.png)
