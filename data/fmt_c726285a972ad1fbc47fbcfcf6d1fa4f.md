
# Chainlink预言机

> 来源：[https://docs.feishu.cn/docx/TJn8dLA7kol5KCxwGtQcfUf2ndg](https://docs.feishu.cn/docx/TJn8dLA7kol5KCxwGtQcfUf2ndg)

区块链是确定性的系统，通过共识来确保

但是对于不确定性的数据，大量非确定性的数据无法通过共识来确保

单节点预言机/中心化预言机容易出现问题

DON（Decentralized Oracle Network）

Chainlink基于DON，可以做数据、计算、跨链服务

## 1 Data

### Data Feed

数据获取

![](img/ee87a05dc16498309be8421f9b87544b.png)

每一个网络有20多个节点，最后聚合通过OCR算法，给Chainlink合约。获取链下数据。

![](img/94e3f94a3cde4742e3069d8ae29405af.png)

### Chainlink Functions

![](img/3b2bf22a9839165740cc7cc833df4636.png)

用户request，合约event，然后回调

## 2 Compute

### VRF

Chainlink VRF provides cryptographically secure randomness for your blockchain-based applications.

![](img/dee4cc85c5f3d21e13ae4d048ae35997.png)

1. 链下预言机生成公钥私钥，公钥写到合约里，私钥自己保存。

2. 当用户request随机数的时候，用户会取得当前区块的一个block header hash，并通过自己的私钥生成一个随机数，生成一个proof，放到合约里，合约会检查这是不是由某个特定的区块和某个随机数通过某种特定的算法生成的。

### Automation

周期性的触发合约

![](img/5c2ee20be9ab1ab7ad73038174f12a3b.png)

![](img/32fdac5f336c4b4ef6c810bb5c631178.png)

## 3 Cross Chain

![](img/19000f363dfb303c47c94b4c0d0e91bf.png)

### Committing DON

![](img/26c08220ec39d2769d0bfb19e5cd870f.png)

写到commit store

### AFN

AFN是质押了很多的用户，他为了自己的资产，需要确保过程的安全性

![](img/43c2038d770c95df0c7a512e3cf8526a.png)

### Executing DON

![](img/e94d17657f74f6e07cd27c13d7313bd1.png)

![](img/045cfae7a06dfa85acaa4d88f90ad39e.png)

信息跨链协议
