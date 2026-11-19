
```solidity
/// 将单个签名从打包的签名分离出来
/// @param signatures 打包签名
/// @param pos 要读取的多签index.
function signatureSplit(bytes memory signatures, uint256 pos)
    internal
    pure
    returns (
        uint8 v,
        bytes32 r,
        bytes32 s
    )
{
    // 签名的格式：{bytes32 r}{bytes32 s}{uint8 v}
    assembly {
        let signaturePos := mul(0x41, pos)
        r := mload(add(signatures, add(signaturePos, 0x20)))
        s := mload(add(signatures, add(signaturePos, 0x40)))
        v := and(mload(add(signatures, add(signaturePos, 0x41))), 0xff)
    }
}
```

1. `encodeTransactionData()`：将交易数据打包并计算哈希，利用了`abi.encode()`和`keccak256()`函数。这个函数可以计算出一个交易的哈希，然后在链下让多签人签名并收集，再调用`execTransaction()`函数执行。

```solidity
/// @dev 编码交易数据
/// @param to 目标合约地址
/// @param value msg.value，支付的以太坊
/// @param data calldata
/// @param _nonce 交易的nonce.
/// @param chainid 链id
/// @return 交易哈希bytes.
function encodeTransactionData(
    address to,
    uint256 value,
    bytes memory data,
    uint256 _nonce,
    uint256 chainid
) public pure returns (bytes32) {
    bytes32 safeTxHash =
        keccak256(
            abi.encode(
                to,
                value,
                keccak256(data),
                _nonce,
                chainid
            )
        );
    return safeTxHash;
}
```

# 21_ERC4626

我们经常说 DeFi 是货币乐高，可以通过组合多个协议来创造新的协议；但由于 DeFi 缺乏标准，严重影响了它的可组合性。而 `ERC4626` 扩展了 `ERC20` 代币标准，旨在推动收益金库的标准化。这一讲，我们将介绍 DeFi 新一代标准 `ERC4626`，并写一个简单的金库合约。教学代码参考 `openzeppelin` 和 `solmate` 中的 `ERC4626` 合约，仅用作教学。

## 金库

金库合约是 DeFi 乐高中的基础，它允许你把基础资产（代币）质押到合约中，换取一定收益，包括以下应用场景:

+ 收益农场: 在 Yearn Finance 中，你可以质押 USDT 获取利息。
+ 借贷: 在 AAVE 中，你可以出借 ETH 获取存款利息和贷款。
+ 质押: 在 Lido 中，你可以质押 ETH 参与 ETH 2.0 质押，得到可以升息的 stETH。

## ERC4626

![](img/a237f4d7dafb38c95c670e5e7f60cdba.png)

由于金库合约缺乏标准，写法五花八门，一个收益聚合器需要写很多接口对接不同的 DeFi 项目。`ERC4626` 代币化金库标准（Tokenized Vault Standard）横空出世，使得 DeFi 能够轻松扩展。它具有以下优点:

1. 代币化: `ERC4626` 继承了 `ERC20`，向金库存款时，将得到同样符合 `ERC20` 标准的金库份额，比如质押 ETH，自动获得 stETH。
1. 更好的流通性: 由于代币化，你可以在不取回基础资产的情况下，利用金库份额做其他事情。拿 Lido 的 stETH 为例，你可以用它在 Uniswap 上提供流动性或交易，而不需要取出其中的 ETH。
1. 更好的可组合性: 有了标准之后，用一套接口可以和所有 `ERC4626` 金库交互，让基于金库的应用、插件、工具开发更容易。

总而言之，`ERC4626` 对于 DeFi 的重要性不亚于 `ERC721` 对于 NFT 的重要性。

### ERC4626 要点

`ERC4626` 标准主要实现了一下几个逻辑：

1. `ERC20`: `ERC4626` 继承了 `ERC20`，金库份额就是用 `ERC20` 代币代表的：用户将特定的 `ERC20` 基础资产（比如 WETH）存进金库，合约会给他铸造特定数量的金库份额代币；当用户从金库中提取基础资产时，会销毁相应数量的金库份额代币。`asset()` 函数会返回金库的基础资产的代币地址。
1. 存款逻辑：让用户存入基础资产，并铸造相应数量的金库份额。相关函数为 `deposit()` 和 `mint()`。`deposit(uint assets, address receiver)` 函数让用户存入 `assets` 单位的资产，并铸造相应数量的金库份额给 `receiver` 地址。`mint(uint shares, address receiver)` 与它类似，只不过是以将铸造的金库份额作为参数。
1. 提款逻辑：让用户销毁金库份额，并提取金库中相应数量的基础资产。相关函数为 `withdraw()` 和 `redeem()`，前者以取出基础资产数量为参数，后者以销毁的金库份额为参数。
1. 会计和限额逻辑：`ERC4626` 标准中其他的函数是为了统计金库中的资产，存款/提款限额，和存款/提款的基础资产和金库份额数量。

### IERC4626 接口合约

`IERC4626` 接口合约共包含 2 个事件:

+ `Deposit` 事件: 存款时触发。
+ `Withdraw` 事件: 取款时触发。

`IERC4626` 接口合约还包含 16 个函数，根据功能分为 4 大类：元数据，存款/提款逻辑，会计逻辑，和存款/提款限额逻辑。

+ 元数据
+ `asset()`: 返回金库的基础资产代币地址，用于存款，取款。
+ 存款/提款逻辑
+ `deposit()`: 存款函数，用户向金库存入 `assets` 单位的基础资产，然后合约铸造 `shares` 单位的金库额度给 `receiver` 地址。会释放 `Deposit` 事件。
+ `mint()`: 铸造函数（也是存款函数），用户存入 `assets` 单位的基础资产，然后合约给 `receiver` 地址铸造相应数量的金库额度。会释放 `Deposit` 事件。
+ `withdraw()`: 提款函数，`owner` 地址销毁 `share` 单位的金库额度，然后合约将相应数量的基础资产发送给 `receiver` 地址。
+ `redeem()`: 赎回函数（也是提款函数），`owner` 地址销毁 `shares` 数量的金库额度，然后合约将相应单位的基础资产发给 `receiver` 地址。
+ 会计逻辑
+ `totalAssets()`: 返回金库中管理的基础资产代币总额。
+ `convertToShares()`: 返回利用一定数额基础资产可以换取的金库额度。
+ `convertToAssets()`: 返回利用一定数额金库额度可以换取的基础资产。
+ `previewDeposit()`: 用于用户在当前链上环境模拟存款一定数额的基础资产能够获得的金库额度。
+ `previewMint()`: 用于用户在当前链上环境模拟铸造一定数额的金库额度需要存款的基础资产数量。
+ `previewWithdraw()`: 用于用户在当前链上环境模拟提款一定数额的基础资产需要赎回的金库份额。
+ `previewRedeem()`: 用于链上和链下用户在当前链上环境模拟销毁一定数额的金库额度能够赎回的基础资产数量。
+ 存款/提款限额逻辑
+ `maxDeposit()`: 返回某个用户地址单次存款可存的最大基础资产数额。
+ `maxMint()`: 返回某个用户地址单次铸造可以铸造的最大金库额度。
+ `maxWithdraw()`: 返回某个用户地址单次取款可以提取的最大基础资产额度。
+ `maxRedeem()`: 返回某个用户地址单次赎回可以销毁的最大金库额度。
