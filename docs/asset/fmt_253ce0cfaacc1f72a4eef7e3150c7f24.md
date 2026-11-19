
```
// 打印交易解码后结果
 let parsedTx = iface.parseTransaction(tx)
 console.log("pending交易详情解码：")
 console.log(parsedTx);
```

1. 构建抢跑交易`txFrontrun`，原交易的目标地址`to`，发送以太坊数额`value`和发送数据`data`保持不变，然后将`gas`拉高：`maxPriorityFeePerGas` 和 `maxFeePerGas` 改为原先的 1.2 倍，`gasLimit`最高设为原先的 2 倍。然后通过 `sendTransaction()` 将交易发送到链上，完成抢跑！

```
// 构建抢跑tx
 const txFrontrun = {
     to: tx.to,
     value: tx.value,
     maxPriorityFeePerGas: tx.maxPriorityFeePerGas * 2n,
     maxFeePerGas: tx.maxFeePerGas * 2n,
     gasLimit: tx.gasLimit * 2,
     data: tx.data
 }
 // 发送抢跑交易
 var txResponse = await wallet.sendTransaction(txFrontrun)
 console.log(正在frontrun交易)
 await txResponse.wait()
 console.log(frontrun 交易成功)   
```

# 24_ERC20Check

这一讲，我们介绍如何用`ether.js`识别一个合约是否为`ERC20`标准，你会在链上分析，识别貔貅，抢开盘等场景用到它。

## `ERC20`

`ERC721` 是以太坊上最常用的代币标准，如果对这个标准不熟悉，可以阅读WTF Solidity第31讲 `ERC20`。`ERC20` 标准包含以下函数和事件:

```
 interface IERC20 {
     event Transfer(address indexed from, address indexed to, uint256 value);

     event Approval(address indexed owner, address indexed spender, uint256 value);

     function totalSupply() external view returns (uint256);

     function balanceOf(address account) external view returns (uint256);

     function transfer(address to, uint256 amount) external returns (bool);

     function allowance(address owner, address spender) external view returns (uint256);

     function approve(address spender, uint256 amount) external returns (bool);

     function transferFrom(address from, address to, uint256 amount) external returns (bool);
 }
```

## 识别 `ERC20` 合约

在之前的[教程](https://github.com/WTFAcademy/WTF-Ethers/blob/main/12_ERC721Check/readme.md)中，我们讲了如何基于 `ERC165` 识别 `ERC721` 合约。但是由于 `ERC20` 的发布早于 `ERC165`（20 < 165），因此我们没法用相同的办法识别 `ERC20` 合约，只能另找办法。

区块链是公开的，我们能获取任意合约地址上的代码（`bytecode`）。因此，我们可以先获取合约代码，然后对比其是否包含 `ERC20` 标准中的函数就可以了。

首先，我们用 `provider` 的 `getCode()` 函数来取得对应地址的 `bytecode`：

```
let code = await provider.getCode(contractAddress)
```

接下来我们要检查合约 `bytecode` 是否包含 `ERC20` 标准中的函数。合约 `bytecode` 中存储了相应的 [`函数选择器`](https://github.com/AmazingAng/WTF-Solidity/blob/main/29_Selector/readme.md)：如果合约包含 `transfer(address, uint256)` 函数，那么 `bytecode` 就会包含 `a9059cbb`；如果合约包含 `totalSupply()`，那么 [`bytecode`](https://github.com/AmazingAng/WTFSolidity/blob/main/Topics/Translation/DiveEVM2017) 就会包含 `18160ddd`。如果你不了解函数选择器，可以阅读 WTF Solidity的相应章节。如果想更深入的了解 `bytecode`，可以阅读深入EVM。

这里，我们仅需检测 `transfer(address, uint256)` 和 `totalSupply()` 两个函数，而不用检查全部6个，这是因为：

1.  `ERC20`标准中只有 `transfer(address, uint256)` 不包含在 `ERC721`标准、`ERC1155`和`ERC777`标准中。因此如果一个合约包含 `transfer(address, uint256)` 的选择器，就能确定它是 `ERC20` 代币合约，而不是其他。

1.  额外检测 `totalSupply()` 是为了防止[选择器碰撞](https://github.com/AmazingAng/WTFSolidity/blob/main/S01_ReentrancyAttack/readme.md)：一串随机的字节码可能和 `transfer(address, uint256)` 的选择器（4字节）相同。

代码如下

```
async function erc20Checker(addr){
    // 获取合约bytecode
    let code = await provider.getCode(addr)
    // 非合约地址的bytecode是0x
    if(code != "0x"){
        // 检查bytecode中是否包含transfer函数和totalSupply函数的selector
        if(code.includes("a9059cbb") && code.includes("18160ddd")){
            // 如果有，则是ERC20
            return true
        }else{
            // 如果没有，则不是ERC20
            return false
        }
    }else{
        return null;
    }
}
```

## 测试脚本

下面，我们利用 `DAI`（`ERC20`）和 `BAYC`（`ERC721`）合约来测试脚本是否能正确识别 `ERC20` 合约。

```
 // DAI address (mainnet)
 const daiAddr = "0x6b175474e89094c44da98b954eedeac495271d0f"
 // BAYC address (mainnet)
 const baycAddr = "0xbc4ca0eda7647a8ab7c2061c2e118a18a936f13d"
 
 const main = async () => {
     // 检查DAI合约是否为ERC20
     let isDaiERC20 = await erc20Checker(daiAddr)
     console.log(1. Is DAI a ERC20 contract: ${isDaiERC20})
 
     // 检查BAYC合约是否为ERC20
     let isBaycERC20 = await erc20Checker(baycAddr)
     console.log(2. Is BAYC a ERC20 contract: ${isBaycERC20})
 }
 
 main()
```

# 25_Flashbots

> 目前 `Flashbots Bundle` 仅支持 `ethers.js` v5 版本。

在以太坊转为 POS 之后，有超过 60% 的区块都是 `flashbots` 产出的，非常惊人，但是很多人并不了解它。这一讲，我们将介绍 `Flashbots`，包括:

1.  什么是 `Flashbots`?

1.  普通用户如何连接 `Flashbots` 节点发送隐私交易。

1.  开发者如何使用 `Flashbots Bundle` 打包多笔交易。

## `Flashbots`

`Flashbots` 是致力于减轻 MEV（最大可提取价值）对区块链造成危害的研究组织。目前有以下几款产品:

1.  `Flashbots RPC`: 保护以太坊用户受到有害 MEV（三明治攻击）的侵害。

1.  `Flashbots Bundle`: 帮助 MEV 搜索者（`Searcher`）在以太坊上提取 MEV。

1.  `mev-boost`: 帮助以太坊 POS 节点通过 MEV 获取更多的 ETH 奖励。

本教程中，我们主要介绍前两个产品。

## `Flashbots RPC`

`Flashbots RPC` 是一款面向以太坊普通用户的免费产品，你只需要在加密的钱包中将 `RPC`（网络节点）设置为`Flashbots RPC`，就可以将交易发送到`Flashbots`的私有交易缓存池（`mempool`）而非公开的，从而免受抢先交易/三明治攻击的损害。如果你不了解`mempool`或抢先交易，可以阅读之前的 [`mempool`](https://github.com/WTFAcademy/WTFEthers/blob/main/19_Mempool/readme.md) 和 [`抢先交易`](https://github.com/WTFAcademy/WTFEthers/blob/main/23_Frontrun/readme.md) 教程

下面我们演示一下如何用 `Metamask` 钱包连接 `Flashbots RPC`。

1.  点击 `Metamask` 顶部的网络按钮（默认显示 Ethereum Mainnet），然后点击底部的 `Add network` 按钮添加网络节点（新版 `Metamask` 还需要在下一个页面点击 `Add a network manually` 按钮）。

1.  依次输入网络参数:

```
 Network name: Flashbots RPC
 New RPC URL: https://rpc.flashbots.net
 Chain ID: 1
 Currency Symbol: ETH
 Block Explorer URL: https://etherscan.io
```

完成这两步，你的加密钱包就成功的连接到了 `Flashbots RPC`，之后你只需要像往常一样操作钱包就可以避免三明治攻击了！

## `Flashbots Bundle`

在区块链上搜索 MEV 机会的开发者被称为搜索者。`Flashbots Bundle`（交易包）是一款帮助搜索者提取以太坊交易中 MEV 的工具。搜索者可以利用它将多笔交易组合在一起，按照指定的顺序执行。

举个例子，搜索者在公共`mempool`发现一笔在 `Uniswap` 买入`PEOPLE`代币的交易有被三明治攻击的机会，他可以在这币交易前后各插入一笔买入和卖出的交易，组成交易 `Bundle` 发送给 `Flashbots`。在些交易将在指定的区块执行，不会改变顺序，且不用担心被别的MEV机器人攻击。

## `Flashbots Bundle` 脚本

`Flashbots` 提供了 `ethers-provider-flashbots-bundle`，一个建立在 `ethers.js` 之上帮助搜索者连接 `flashbots provider` 并发送 `flashbots bundle` 的 JavaScript 库。你可以通过 `npm` 命令安装它。

```
npm install --save @flashbots/ethers-provider-bundle
```

下面，我们利用它写一个脚本，给大家演示如何在 `Goerli` 测试网发送 `Flashbots Bundle`。代码开源在 `WTF-Ethers` repo。

1.  创建一个连接到非`Flashbots RPC`的普通`provider`，这里我们使用 `Alchemy` 提供的 `Goerli` 测试网节点。

```
// 1. 普通rpc （非flashbots rpc）
 const ALCHEMY_GOERLI_URL = 'https://eth-goerli.alchemyapi.io/v2/GlaeWuylnNM3uuOo-SAwJxuwTdqHaY5l';
 const provider = new ethers.providers.JsonRpcProvider(ALCHEMY_GOERLI_URL);
```

1.  创建 `Flashbots` 声誉私钥，用于建立“声誉”，[详情](https://docs.flashbots.net/flashbots-auction/searchers/advanced/reputation)

> 注意: 这个账户不要储存资金，它不是`flashbots`主私钥。

```
 const authKey = '0x227dbb8586117d55284e26620bc76534dfbd2394be34cf4a09cb775d593b6f2c';
 const authSigner = new ethers.Wallet(authKey, provider)
```

1.  创建 `Flashbots RPC` (测试网），用于发送交易，这里用到了普通`provider`和声誉私钥。

```
const flashbotsProvider = await FlashbotsBundleProvider.create(
     provider,
     authSigner,
     // 使用主网 Flashbots，需要把下面两行删去
     'https://relay-goerli.flashbots.net/', 
     'goerli'
     );
```

1.  创建一笔符合 `EIP1559` 标准的交易，交易内容: 发送 0.001 ETH 测试币到 WTF Academy 地址。这里用到了钱包私钥（含资产）以及普通`provider`

```
 const privateKey = '0x227dbb8586117d55284e26620bc76534dfbd2394be34cf4a09cb775d593b6f2c';
 const wallet = new ethers.Wallet(privateKey, provider)
 // EIP 1559 transaction
 const transaction0 = {
 chainId: CHAIN_ID,
 type: 2,
 to: "0x25df6DA2f4e5C178DdFF45038378C0b08E0Bce54",
 value: ethers.utils.parseEther("0.001"),
 maxFeePerGas: GWEI * 100n
 }
```

1.  创建交易`Bundle`，这里我们只打包了一笔交易，实际使用中可以打包多笔签名过或未签名的交易。

```
 const transactionBundle = [
     {
         signer: wallet, // ethers signer
         transaction: transaction0 // ethers populated transaction object
     }
     // 也可以加入mempool中签名好的交易（可以是任何人发送的）
     // ,{
     //     signedTransaction: SIGNED_ORACLE_UPDATE_FROM_PENDING_POOL // serialized signed transaction hex
     // }
 ]
```

1.  模拟交易并打印交易详情。`bundle` 要模拟成功后才能被执行。这里用到了`flashbots provider`的 `signBundle()` 和 `simulate()` 方法。注意，`simulate()` 方法需要指定交易执行的目标区块高度，这里用的下一个区块。

![](img/db298f01ec6218cb5a53b9e8fb191698.png)
