
# EVM介绍

> 来源：[https://docs.feishu.cn/docx/VFindHp9Co0pAJxSzL2ciHRrnxe](https://docs.feishu.cn/docx/VFindHp9Co0pAJxSzL2ciHRrnxe)

![](img/f4d5d9203370e01ddb2b217109f00d6c.png)

[ethereum_e_trated.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/boxcnFzqWEhFmgzY4w4DOLFwcDg)

![](img/0845ddf7558b65e399d657d317b27945.png)

## 隔离

## 权限

## 转成`bytecode`给`EVM`跑

+   合约层
+   `EVM`
+   节点层
+   硬件层

![](img/7df7c4496beebf29648fedf6eac3ec35.png)

## 存储架构

### 数据

+   非易失
+   不可变
+   持久化
+   `EVM code`：链上不可变
+   `storage`：区块链全局变量
+   `Machine state`：易失的
+   `PC`：地址，指向当前指令
+   `calldata`：只读可寻址；外部调用的参数，函数调用；必须手动制定才能访问
+   `gas`：存储`gas`
+   `stack`：免费使用，无`gas`，可读写空间，过程数据，局部变量；只有16，只有值类型才能用，不能太深
+   `memory`：可读写空间。`字节级别`寻址内存
+   `内联汇编`需要，从`0x40`开始，指向`memory`；防止数据被覆盖
+   `stack`存储当前正在执行的`OPCode`和当前`OPCode`所处理的一些变量
+   `memory`执行时用于存储暂存的一些数据，这些数据之后可能被推入`stack`中进行处理

> `Memory`指的是用于在`EVM`中存储数据的连续空间，类似于计算机中的RAM（随机存储器）。`EVM`程序可以通过将数据存储在`Memory`中来进行数据处理和交换。`Memory`中的数据可以随时被读取和修改，但在`EVM`执行结束后，`Memory`中的所有数据都会被清空。

> `Stack`指的是一种数据结构，它采用后进先出（LIFO）的原则进行数据存储和访问。`EVM`程序可以使用`Stack`来存储中间结果和执行状态。每当`EVM`执行一个操作时，它会将结果存储在`Stack`中，并从`Stack`中取出所需的参数。当`EVM`执行完成后，`Stack`中的所有数据也会被清空。

> 因此，`Memory`和`Stack`都是`EVM`中用于存储和处理数据的重要概念，但它们的用途和功能有所不同。`Memory`主要用于存储和处理数据，而`Stack`主要用于存储和处理执行状态和中间结果。

![](img/e053c121e81f6d0340703cdb5d6425fa.png)

## 交易三种类型

+   普通：直接修改余额，不会进入`EVM`运行
+   合约创建：进入`EVM`，以`Message`为单位，生成`contract`对象；根据合约地址，在链上加载`EVM code`，根据`EVM`中解释器加载执行字节码
+   合约函数调用：进入`EVM`，以`Message`为单位，生成`contract`对象；根据合约地址，在链上加载`EVM code`，根据`EVM`中解释器加载执行字节码

`msg.data`不为空会启动`EVM`进行计算

首先`PC`从合约代码里读取`OP code`，计算`gas`

+   不够，失败，返回`error`
+   够，循环执行指令
+   分别对`stack`，`memory`，`storage`进行一些操作

![](img/94731af2065595890b8947352b2f0b75.png)

> `BSC`链是一个基于以太坊虚拟机（`EVM`）的区块链网络，但是它采用了一种称为`Proof of Staked Authority`（`PoSA`）的共识算法，而不是以太坊的`Proof of Work`（`PoW`）共识算法。`PoSA`算法允许`BSC`网络在出块速度和交易确认方面比以太坊更快。

> 此外，`BSC`链采用了一种称为链上智能合约的技术，这使得交易确认速度更快，并且与以太坊相比，`BSC`的手续费（即“`gas`费”）通常更便宜。这是因为`BSC`的交易量相对较少，因此链上交易竞争较小，手续费相对更低。

![](img/116cf683b88217cb63f44bae2c6b680a.png)
