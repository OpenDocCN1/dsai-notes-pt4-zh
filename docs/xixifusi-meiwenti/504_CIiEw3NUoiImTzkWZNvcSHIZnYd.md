
# 去中心化存储

> 来源：[https://docs.feishu.cn/docx/RnYTdRCFrogqzBx3lxtcEsgSnfd](https://docs.feishu.cn/docx/RnYTdRCFrogqzBx3lxtcEsgSnfd)

## 01 什么是DSN？

DSN（Decentralized Storage Network）是一种去中心化的存储网络。在传统的Web2中，数据通常存储在中心化的服务器上，而对于一些需要永久存储的数据，如以太坊的第一层，也需要借助去中心化存储网络。

![](img/ca6d1307b0e3abf7d06602c856753bf4.png)

+ 传统的需要Web2中心化的访问，中心化的服务器；但是费用很低

![](img/dcf382cd07326248aa6189d0ace37dfe.png)

+ 每一个节点都有副本，但是同样就会导致成本很高
+ 不会受协议受限
+ 独立数据的所有权

DSN通过降低副本的数量来降低存储成本，并具备区块链的合约互操作性和去中心化访问方式。相比传统云服务，DSN让用户保留数据的所有权、访问权和控制权，避免数据丧失和审查问题。相比区块链，DSN的存储成本更为便宜，但需要应对降低副本数量带来的挑战。后续的一些设计也只要是为了解决降低副本数量带来的挑战。

在DSN中，存储节点可以选择存储自己感兴趣的数据，而不是强制存储同一份数据，与传统区块链不同，DSN实际上没有全局状态

![](img/8030c1fd791966468b4f6f79c5dbf5db.png)

+ Proof of Publication：节点都能拿到数据，并且选择存不存；核心就是如果只有一个节点私占了某个节点，他是没有动力广播的
+ Proof of Storage：因为三种攻击，所以需要Storage的一个Proof
+ Proof of Retrievability：检索矿工

## 02 Proof of Publication

问题1：

![](img/6b579d5bab3af13b1a0170f51a7fa56d.png)

+ Node has no incentive to broadcast data
+ 我们可不可以模仿传统区块链一样，在广播区块的时候顺便把交易也广播包含进去？
+ 不行，数据很大，共识性能很差，出块速度很慢；大部分区块里只放交易的hash来加速

![](img/c43a228afb4b8c4a9214aa62d2baa850.png)

数据发布的证明

确保所有的节点都能访问网络内的数据，同时选择自己存还是不存

确保有足够的副本实现永存

![](img/e53fb108d59dd477aa2f13792b8fbdfd.png)

![](img/5eb5e2c517daf012349cab64917aae07.png)

ETH的Danksharding，把数据打成碎片

只要有一半就可以恢复，一行内也只要有一半就可以恢复

![](img/f54b15ad8786a91ba4576d9a76eceb50.png)

Paradigm for publishing data

+ 发到以太坊的full node
+ Set data commitment到链上合约的storage
+ 成功之后，其他的DSN node就可以通过blockchain的RPC系统拿到数据；就不会存在一个node隐藏data

## 03 Proof of Storage

DSN can not execute settlement of storage fee because of the lack of a unified world state

![](img/b05adc74653a6a5cd692f28dd109d9a3.png)

放到blockchain执行，node执行之后会有一个proof of storage，会在链上被合约通过，然后会给对应的node支付存储费用（放在链上执行）

### DSN的多维度攻击？

![](img/92c67fee0242e295db2fc8f345a7a0f2.png)

(1) 女巫攻击，是指当系统要求旷工提供证明时，旷工利用别人那里存储数据生产的证明，提交给系统。然而，实际上，他并没有存储。区块链中本质上是一个只写不删的分布式数据库，通过多个节点的冗余数据达到网络的安全和不可篡改。女巫攻击，即为单一节点具有多个身份标识，通过控制系统的大部分节点来削弱冗余备份的作用。

![](img/22332d35d7883b36a2eea40d85cef24e.png)

(2)生成攻击，node2、3时不同实体，想去提交这个file的proof of storage的时候，从网络上去找这份数据后，拿过来在链上做一些编码计算，再提上去。那么这个时候如果能也能成功的去拿到reward，属于生成攻击。

相当于外包存储

![](img/f6c65934823f8b8869a69d118957f842.png)

(3) 外援攻击，外援攻击指的是你把计算的外包给了其他节点。node2 还没有存储prove这份数据。但是note4存了，通过让note4把这份数据顺便也一起编码好之后再一起传给我，然后我再把这份数据再送到链上Settlement的合约里面去，然后我再去结算这份数据依旧能拿到存储报酬，那么这个时候我们会认为这种攻击它是一种外援攻击，就是把计算跟存储全部都外包。

![](img/4b353a29dc411da04037dd02cc6acaf2.png)

### 如何避免攻击？

![](img/16bdc7ec41743cdcd948cb9445746855.png)

+ 每个storage provider只能有一个，避免Sybil Attack
+ VDF
+ 验证方法：随机采样

降低计算复杂度的措施：DSN中的数据序列化（encoding）过程会导致高计算成本，特别昂贵。为了降低计算复杂度，引入了延迟可验证执行函数VDF，允许在一段时间后请求函数执行，但需要节点在已知的时间段内执行计算。这样每个节点都会耗费一定的时间进行encoding。

激励存储数据的措施：为了鼓励矿工存储整个网络的数据，需要解决数据独占问题。目前共识机制存在一些问题，使得矿工倾向于独占数据。其中包括缺乏严格的数据分发和传播机制，导致矿工有动机制造大量垃圾数据来获取更多的权益。

数据检索证明机制：由于每个节点并不拥有整个网络的数据，需要一种检索证明机制来确保用户在网络中能够找到所需数据。目前并没有成熟的检索市场，通常数据检索是通过中心化的服务或服务器实现，缺乏工程化的实现。

### Filecoin

> https://www.bilibili.com/video/BV1Xx4y1X7MU/?spm_id_from=333.999.0.0&vd_source=b2c556c92eaad2e3330b8aa1ae47c210

+ PoSt Proof

![](img/5709070cb432b47720b07b881b9ba92c.png)

时空时间

## 04 Proof of Retrievability

![](img/6ded237d7bf8f58a0f70d49046262d73.png)

![](img/69abb7dd2f1d8049e955b026c30265ef.png)

暂时还没有实现

## 05 总结

![](img/a71011da492f123a9d7bf700066687a6.png)

![](img/0374b2b0f70852fc68ff8e675f21e549.png)
