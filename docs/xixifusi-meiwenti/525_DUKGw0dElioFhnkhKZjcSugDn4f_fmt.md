
# 开发者工具

> 来源：[https://docs.feishu.cn/docx/ZHRIdI30boT9nOx6hsyc2pKVnMh](https://docs.feishu.cn/docx/ZHRIdI30boT9nOx6hsyc2pKVnMh)

## 子页面目录

### TOOL01_Remix

[Remix.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/AaQtbYTsuoIf8wxFfLrca8yincc)

### TOOL02_Infura

Infura 是 Consensys（小狐狸钱包母公司）开发的区块链基础设施，帮助用户/开发者更好的与以太坊区块链交互。

#### 连接应用和区块链

在以太坊上开发的 Dapp 应用（链下）需要与区块链（链上）交互。早期，以太坊上的基础设施很少，开发者需要在本地部署以太坊节点来完成链下和链上的交互，非常麻烦，且耗时数日。

Infura 在链下、链上之间搭了一座桥，让两者的交互变得简单。它为用户提供对以太坊和 IPFS 网络的即时、可扩展 API 访问。开发者在 Infura 官网注册后，就可以免费申请以太坊 API KEY，就可以利用 Infura 的节点与区块链交互。另外，小狐狸 Metamask 钱包内置了 Infura 服务，方便用户访问以太坊网络。

#### 使用 Infura API Key

##### Javascript (`ethers.js`)

在 `ethers.js` 中，我们可以利用 Infura API Key 来创建 `JsonRpcProvider`，与链上交互。

```
const { ethers } = require("ethers");
// 填上你的 Infura API Key
const INFURA_ID = ''
const provider = new ethers.providers.JsonRpcProvider(https://mainnet.infura.io/v3/${INFURA_ID})
```

##### Metamask 小狐狸钱包

进入小狐狸钱包设置 `Setting` 页面，点击网络 `Network`，点击添加网络 `Add Network`。你可以利用下面的参数在小狐狸中添加 Optimism Layer2 链：

```
网络名称（Network Name）: Optimism
RPC URL：填在 Infura 申请的 optimism rpc 链接
链ID (Chain ID): 10
符号 (Chain Symbol): ETH
区块链浏览器URL (Blockchain Explorer URL): https://optimistic.etherscan.io
```

### TOOL03_Ganache

`Ganache` 是一个适用于“开发场景”（或开发过程中的）的 以太坊区块链网络，它使开发以太坊应用程序的工作更快、更容易、更安全。它包括所有流行的 RPC 功能和特性（如事件），并且可以确定性地运行以使开发变得轻而易举。

> 可以理解 `ganache` 是一个以太坊区块链“模拟器”，在开发过程中“模拟”生产环境、方便的获取合约调用的上下文。

> ⚠️ 注意：请不要在生产环境中作为区块链网络使用。如果需要，请使用 `geth` 或 `openethereum` 等节点应用。

`Ganache` 同时有 GUI 版本，适合于新手快速配置使用。

[Ganache.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/HQTQbFrTLoEnStxXfB9ceuLLnoc)

### TOOL04_Alchemy

[Alchemy.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/PiQqbTblcodgKsxwGWic6bAQnmc)

### TOOL05_Dune

[Dune.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/FTBMb6PHHo5WtExrjv9ccmedn5B)

### TOOL06_Hardhat

以太坊智能合约开发环境

![](img/abb35ea240a1704e2633a1c23687e36a.png)

[Hardhat.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/D4ZVb3XnXoNr3txxd7pcbxagnpf)

#### 1 Why Hardhat?

![](img/fbcf663f1dc009cd40534f668a19d49f.png)

![](img/d7312a1d521c28b39f09c6cb1eaf2646.png)

![](img/153dbc71c87c5e834351349030e36d31.png)

![](img/85134c381c503d0b0552ad5dc7e1f2d7.png)

![](img/169a0f93b9aedb659b6ec49b2e3182a7.png)

![](img/e9c38ebae8dd60aa0db5e7aeb87599fb.png)

#### 2 准备工作

![](img/42bd9d0b6d68a83294d535a308d3ce15.png)

![](img/906542a7efe40518010e09778cf5e839.png)

![](img/2653a354e35aac0d751336f34a2e461b.png)

#### 3 介绍

![](img/89e5b8aa4edce81df9b8e5fff32c7880.png)

`hardhat.config.ts`

![](img/91ff9fe43179b90073546dac9da12722.png)

![](img/215560c676648abe7bab115f24b39099.png)

私钥助记词需要加密

![](img/fe693ea0dfa42de4ccdf71a43f11d399.png)

`Mocha` 测试框架

`package.json`

存着

+ 项目依赖
+ 自定义脚本

#### 测试

列测试需求

![](img/2fa04a90c1283abcf9d5a0c7e36ede66.png)

![](img/d1fbc98b7de5f8531d9de45a8e191662.png)

异步

![](img/726b264983c031756dbb266dfcbee703.png)

![](img/271b59057f025abd33023acb5cc1e5d4.png)

![](img/27024221c8a6d62f5340d73d851fcc26.png)

![](img/6a5c39e2262f52ab9ed177d55c596995.png)

![](img/44ffa52bedff67339bb7a5877991890b.png)

![](img/2c4958dfa0d6ae6adab5978ef77d393b.png)

![](img/5155d8346462d87e784e45097c27fbad.png)

#### 部署

![](img/61420d1bb8f233ab865c879e5303fdaf.png)

![](img/b074a50cc9e65e0e0504a4463a5ab35a.png)

![](img/dac1e9a2b59630381f7c12c8d65a7764.png)

> `task` 是一种自定义命令，允许开发人员使用命令行运行脚本或任务。开发人员可以使用 `task` 来创建自定义工具或流程，或执行一系列命令或操作。

`task` 更灵活，名字，参数

`hre` 是运行时，获取运行时的一些状态

需要导入

![](img/d4ea74798916ee720c419f05029bece7.png)

+ 加载运行时
+ 加载合约
+ 发行交易

![](img/cb8570a880a0425aae54f39f089d42e3.png)

![](img/42ca52476f7867320f9c1e650f190e6a.png)

![](img/748ef38bdb75728b003ed3fe2742cacb.png)

#### 4 细节

+ `hardhat`+`ethers`，是从 `hardhat` import 而不是从 `ethers` import：`import { ethers } from "hardhat";`

+ `hardhat` 知道我们项目的合约

### TOOL07_Foundry

![](img/d98612f400002c04356a551d9a9a8c69.png)

[Foundry.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/EsClbYkDeoFLKex0V4Nc2aRUnTc)
