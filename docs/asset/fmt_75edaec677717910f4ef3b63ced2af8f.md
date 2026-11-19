
```javascript
// provider.on("pending", listener)
import { ethers } from "ethers";

// 1. 创建provider和wallet，监听事件时候推荐用wss连接而不是http
// 准备 alchemy API 可以参考https://github.com/AmazingAng/WTFSolidity/blob/main/Topics/Tools/TOOL04_Alchemy/readme.md 
const ALCHEMY_MAINNET_WSSURL = 'wss://eth-mainnet.g.alchemy.com/v2/oKmOQKbneVkxgHZfibs-iFhIlIAl6HDN';
const provider = new ethers.WebSocketProvider(ALCHEMY_MAINNET_WSSURL);
let network = provider.getNetwork()
network.then(res => console.log(`[${(new Date).toLocaleTimeString()}] 连接到 chain ID ${res.chainId}`));

// 2. 创建interface对象，用于解码交易详情。
const iface = new ethers.Interface([
    "function transfer(address, uint) public returns (bool)",
])

// 3. 限制访问rpc速率，不然调用频率会超出限制，报错。
function throttle(fn, delay) {
    let timer;
    return function(){
        if(!timer) {
            fn.apply(this, arguments)
            timer = setTimeout(()=>{
                clearTimeout(timer)
                timer = null
            },delay)
        }
    }
}

const main = async () => {
    // 4. 监听pending的erc20 transfer交易，获取交易详情，然后解码。
    console.log("\n4. 监听pending交易，获取txHash，并输出交易详情。")
    provider.on("pending", throttle(async (txHash) => {
        if (txHash) {
            // 获取tx详情
            let tx = await provider.getTransaction(txHash);
            if (tx) {
                // filter pendingTx.data
                if (tx.data.indexOf(iface.getFunction("transfer").selector) !== -1) {
                    // 打印txHash
                    console.log(`\n[${(new Date).toLocaleTimeString()}] 监听Pending交易: ${txHash} \r`);

                    // 打印解码的交易详情
                    let parsedTx = iface.parseTransaction(tx)
                    console.log("pending交易详情解码：")
                    console.log(parsedTx);
                    // Input data解码
                    console.log("Input Data解码：")
                    console.log(parsedTx.args);
                }
            }
        }
    }, 100));
};

main()
```

# 21_VanityAddress

## 靓号地址

现实生活中，有人追求车牌号“888888”，而在区块链中，大家也追求“靓号地址”。靓号地址（Vanity Address）是个性化的地址，易于识别，并且具有与其它地址一样的安全性。比如以7个0开头的地址：

```
0x0000000fe6a514a32abdcdfcc076c85243de899b
```

是的，这也是知名做市商Wintermute被盗$1.6亿的靓号地址（[报道](https://www.blocktempo.com/head-market-maker-wintermute-hacked-loses-160-million-magnesium/)）。刚才我们说了，靓号和普通地址具有一样的安全性，那么这里为什么被攻击了呢？

问题出在生成靓号工具存在漏洞。Wintermute使用了一个叫Profinity的靓号生成器来生成地址，但这个生成器的随机种子有问题。本来随机种子应该有2的256次方可能性，但是Profinity使用的种子只有2的32次方的长度，可以被暴力破解。

## 靓号生成器

利用`ethers.js`，我们可以用10行代码就可以写出一个靓号生成器，它可能没有其它的工具快，但安全有保障。

### 生成随机钱包

我们可以利用下面的代码安全并随机的生成钱包：

```
const wallet = ethers.Wallet.createRandom() // 随机生成钱包，安全
```

### 正则表达式

我们需要用正则表达式来筛选出目标靓号地址。这里简单的讲一下正则表达式：

+ 开头几位字符匹配，我们用`^`符号，例如`^0x000`就会匹配以`0x000`开头的地址。

+ 最后几位字符匹配，我们用`$`符号，例如`000$`就会匹配以`000`结尾的地址。

+ 中间几位我们不关心，可以利用`.*`通配符，例如`^0x000.*000$`就会匹配任何以`0x000`开头并以`000`结尾的地址。

在`js`中，我们可以用下面的表达式筛选靓号地址：

```
const regex = /^0x000.*$/ // 表达式，匹配以0x000开头的地址
isValid = regex.test(wallet.address) // 检验正则表达式
```

### 靓号生成脚本

靓号生成器的逻辑非常简单，不断生成随机钱包，直到匹配到我们想要的靓号才结束。作为测试，以`0x000`开头的靓号仅需几秒就可以生成，每多一个`0`，耗时多16倍。

```
import { ethers } from "ethers";
var wallet // 钱包
const regex = /^0x000.*$/ // 表达式
var isValid = false
while(!isValid){
    wallet = ethers.Wallet.createRandom() // 随机生成钱包，安全
    isValid = regex.test(wallet.address) // 检验正则表达式
}
// 打印靓号地址与私钥
console.log(`靓号地址：${wallet.address}`)
console.log(`靓号私钥：${wallet.privateKey}`)
```

# 22_ReadAnyData

以太坊所有数据都是公开的，因此 `private` 变量并不私密。这一讲，我们将介绍如何读取智能合约的任意数据。

## 智能合约存储布局

以太坊智能合约的存储是一个 `uint256 -> uint256` 的映射。`uint256` 大小为 32 bytes，这个固定大小的存储空间被称为 slot （[插槽](https://docs.soliditylang.org/en/v0.8.17/internals/layout_in_storage.html?highlight=Layout%20of%20State%20Variables%20in%20Storage)）。智能合约的数据就被存在一个个的 `slot` 中，从 `slot 0` 开始依次存储。每个基本数据类型占一个`slot`，例如`uint`，`address`，等等；而数组和映射这类复杂结构则会更复杂，详见网址。

因此，即使是没有 `getter` 函数的 `private` 变量，你依然可以通过 `slot` 索引来读取它的值。

## getStorageAt

`ethersjs` 提供了 `getStorageAt()` 方便开发者读取特定 `slot` 的值：

```
const value = await provider.getStorageAt(contractAddress, slot)
```

`getStorageAt()` 有两个参数，分别是合约地址 `contractAddress` 和 想读取变量的 `slot` 索引。

## 读取任意数据脚本

下面，我们写一个脚本，利用 `getStorageAt()` 函数来读取 Arbitrum 跨链桥的合约所有者。该跨链桥为可升级代理合约，将 `owner` 存在了特定的 `slot` 避免发生变量碰撞，并且没有读取它的函数。这里，我们就可以利用`getStorageAt()` 来读取它。

```
合约地址: 0x8315177aB297bA92A06054cE80a67Ed4DBd7ed3a
slot索引: 0xb53127684a568b3173ae13b9f8a6016e243e63b6e8ee1178d6a717850b5d6103
```

代码：

```
import { ethers } from "ethers";

// 准备 alchemy API 可以参考https://github.com/AmazingAng/WTFSolidity/blob/main/Topics/Tools/TOOL04_Alchemy/readme.md 
const ALCHEMY_MAINNET_URL = 'https://eth-mainnet.g.alchemy.com/v2/oKmOQKbneVkxgHZfibs-iFhIlIAl6HDN';
const provider = new ethers.JsonRpcProvider(ALCHEMY_MAINNET_URL);

// 目标合约地址: Arbitrum ERC20 bridge（主网）
const addressBridge = '0x8315177aB297bA92A06054cE80a67Ed4DBd7ed3a'; // DAI Contract
// 合约所有者 slot
const slot = 0xb53127684a568b3173ae13b9f8a6016e243e63b6e8ee1178d6a717850b5d6103;

const main = async () => {
    console.log("开始读取特定slot的数据");
    const privateData = await provider.getStorage(addressBridge, slot);
    console.log("读出的数据（owner地址）: ", ethers.getAddress(ethers.dataSlice(privateData, 12)));
}

main()
```

# 23_FrontRun

这一讲，我们将介绍抢先交易（Front-running，抢跑）的脚本。据统计，以太坊上的套利者通过三明治攻击（sandwich attack）共获利12亿美元。在学习之前，请先阅读WTF Solidity教程 合约安全S11: 抢先交易。

## Freemint NFT合约

我们要抢跑的目标合约是一个ERC721标准的NFT合约`Frontrun.sol`，它拥有一个`mint()`函数进行免费铸造。

```
 // SPDX-License-Identifier: MIT
 // By 0xAA
 pragma solidity ^0.8.4;
 import "@openzeppelin/contracts/token/ERC721/ERC721.sol";

 // 我们尝试frontrun一笔Free mint交易
 contract FreeMint is ERC721 {
     uint256 public totalSupply;

     // 构造函数，初始化NFT合集的名称、代号
     constructor() ERC721("Free Mint NFT", "FreeMint"){}

     // 铸造函数
     function mint() external {
         _mint(msg.sender, totalSupply); // mint
         totalSupply++;
     }
 }
```

为了简化测试环境，我们将上述合约部署在`foundry`本地测试网，然后监听在`mempool`中的未决交易，筛选出符合标准的交易进行抢跑。

如果你不了解 `foundry`，可以阅读WTF Solidity中的Foundry教程。安装好 `foundry` 后，在命令行输入以下命令就可以启动本地测试网:

```
anvil
```

## 抢跑脚本

下面，我们详解一下抢跑脚本`frontrun.js`，这个脚本会监听链上的`mint()`交易，并发送一个gas更高的相同交易，进行抢跑。

1. 创建连接到`foundry`本地测试网的`provider`对象，用于监听和发送交易。`foundry`本地测试网默认url: `http://127.0.0.1:8545`。

```
import { ethers } from "ethers";

// 1. 创建provider
var url = "http://127.0.0.1:8545";
const provider = new ethers.WebSocketProvider(url);
let network = provider.getNetwork()
network.then(res => console.log(`[${(new Date).toLocaleTimeString()}] 连接到 chain ID ${res.chainId}`));
```

1. 创建一个包含我们感兴趣的`mint()`函数的`interface`对象，用于解码交易。如果你不了解它，可以阅读WTF Ethers极简教程第20讲：解码交易。

```
// 2. 创建interface对象，用于解码交易详情。
 const iface = new ethers.Interface([
     "function mint() external",
 ])
```

1. 创建测试钱包，用于发送抢跑交易，私钥是`foundry`测试网提供的，里面有10000 ETH测试币。

```
// 3. 创建钱包，用于发送抢跑交易
 const privateKey = '0x5de4111afa1a4b94908f83103eb1f1706367c2e68ca870fc3fb9a804cdab365a';
 const wallet = new ethers.Wallet(privateKey, provider)
```

1. 我们利用`provider.on`方法监听`mempool`中的未决交易，当交易出现时，我们会利用交易哈希`txHash`来读取交易详情`tx`，并筛选出调用了`mint()`函数且发送方不是自己钱包地址的交易（如果不筛选，会自己抢跑自己的交易，陷入死循环）。然后，再打印筛选出的交易哈希。

```
provider.on("pending", async (txHash) => {
     if (txHash) {
         // 获取tx详情
         let tx = await provider.getTransaction(txHash);
         if (tx) {
             // filter pendingTx.data
             if (tx.data.indexOf(iface.getFunction("mint").selector) !== -1 && tx.from != wallet.address ) {
                 // 打印txHash
                 console.log(`\n[${(new Date).toLocaleTimeString()}] 监听Pending交易: ${txHash} \r`);
```

1. 打印筛选出的未决交易详情

```
// 打印原始交易
 console.log("raw transaction")
 console.log(tx);
```

1. 打印解码后的交易详情，可以看到该交易调用的是`mint()`函数。
