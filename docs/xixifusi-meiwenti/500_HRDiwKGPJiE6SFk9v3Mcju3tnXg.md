# Chainlink预言机

> 来源：[https://docs.feishu.cn/docx/GlfWdIyRNo1XYlxAChbc8mhunPc](https://docs.feishu.cn/docx/GlfWdIyRNo1XYlxAChbc8mhunPc)

+   区块链是确定性的系统，通过共识来确保

+   但是对于不确定性的数据，大量非确定性的数据无法通过共识来确保

单节点预言机/中心化预言机容易出现问题

DON（Decentralized Oracle Network）

Chainlink基于DON，可以做数据、计算、跨链服务

# 1 Data

+   Data Feed：数据获取

![](img/881f0f13ccb2bd8246c9f096a3aa1e31.png)

每一个网络有20多个节点，最后聚合通过OCR算法，给Chainlink 合约。获取链下数据。

![](img/f980f46181935814decc0ac018534ef4.png)

+   Chainlink Functions

![](img/7d186b31e6461453ed0d0283dd3e01cd.png)

用户request，合约event，然后回调

# 2 Compute

VRF

Chainlink VRF provides cryptographically secure randomness for your blockchain-based applications.

![](img/538824dd94256c81fbb374fceb6d2e87.png)

1.  链下预言机生成公钥私钥，公钥写到合约里，私钥自己保存。

1.  当用户request随机数的时候，用户会取得当前区块的一个block header hash，并通过自己的私钥生成一个随机数，生成一个proof，放到合约里，合约会检查这是不是由某个特定的区块和某个随机数通过某种特定的算法生成的。

Automation

周期性的触发合约

![](img/229ed0ffc17e4d3a93e847ad7a56a240.png)

![](img/963b015c1d7eb4e808a5dd7b9e3a07bf.png)

# 3 Cross Chain

![](img/2bfe39c739664cb1bcc26dba393b875e.png)

1.  Committing DON

![](img/156f56e97aff090bd721cd5faee2d019.png)

写到commit store

1.  AFN

1.  AFN是质押了很多的用户，他为了自己的资产，需要确保过程的安全性

![](img/94ae315546a56549194712e20756da33.png)

1.  Executing DON

![](img/fb7f5fefd8db0f9d0c7303aab6e91358.png)

![](img/2b9ecaaeea0d9295bddc3e81251a7448.png)

信息跨链协议