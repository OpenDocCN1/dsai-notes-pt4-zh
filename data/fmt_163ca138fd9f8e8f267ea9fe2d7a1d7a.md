
```javascript
// 获得provider
 const provider = new ethers.BrowserProvider(window.ethereum)
 // 获得signer后调用signTypedData方法进行eip712签名
 const signature = await signer.signTypedData(domain, types, message);
 console.log(&quot;Signature:&quot;, signature);
```

### 链上验证

接下来就是 `EIP712Storage` 合约部分，它需要验证签名，如果通过，则修改 `number` 状态变量。它有 5 个状态变量。

1. `EIP712DOMAIN_TYPEHASH`: `EIP712Domain` 的类型哈希，为常量。

1. `STORAGE_TYPEHASH`: `Storage` 的类型哈希，为常量。

1. `DOMAIN_SEPARATOR`: 这是混合在签名中的每个域 (Dapp) 的唯一值，由 `EIP712DOMAIN_TYPEHASH` 以及 `EIP712Domain` （`name`, `version`, `chainId`, `verifyingContract`）组成，在 `constructor()` 中初始化。

1. `number`: 合约中存储值的状态变量，可以被 `permitStore()` 方法修改。

1. `owner`: 合约所有者，在 `constructor()` 中初始化，在 `permitStore()` 方法中验证签名的有效性。

另外，`EIP712Storage` 合约有 3 个函数。

1. 构造函数: 初始化 `DOMAIN_SEPARATOR` 和 `owner`。

1. `retrieve()`: 读取 `number` 的值。

1. `permitStore`: 验证 EIP712 签名，并修改 `number` 的值。首先，它先将签名拆解为 `r`, `s`, `v`。然后用 `DOMAIN_SEPARATOR`, `STORAGE_TYPEHASH`, 调用者地址，和输入的 `_num` 参数拼出签名的消息文本 `digest`。最后利用 `ECDSA` 的 `recover()` 方法恢复出签名者地址，如果签名有效，则更新 `number` 的值。

```
 // SPDX-License-Identifier: MIT
 // By 0xAA 
 pragma solidity ^0.8.0;
 
 import &quot;@openzeppelin/contracts/utils/cryptography/ECDSA.sol&quot;;
 
 contract EIP712Storage {
     using ECDSA for bytes32;
 
     bytes32 private constant EIP712DOMAIN_TYPEHASH = keccak256(&quot;EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)&quot;);
     bytes32 private constant STORAGE_TYPEHASH = keccak256(&quot;Storage(address spender,uint256 number)&quot;);
     bytes32 private DOMAIN_SEPARATOR;
     uint256 number;
     address owner;
 
     constructor(){
         DOMAIN_SEPARATOR = keccak256(abi.encode(
             EIP712DOMAIN_TYPEHASH, // type hash
             keccak256(bytes(&quot;EIP712Storage&quot;)), // name
             keccak256(bytes(&quot;1&quot;)), // version
             block.chainid, // chain id
             address(this) // contract address
         ));
         owner = msg.sender;
     }
 
     /**
      * @dev Store value in variable
      */
     function permitStore(uint256 _num, bytes memory _signature) public {
         // 检查签名长度，65是标准r,s,v签名的长度
         require(_signature.length == 65, &quot;invalid signature length&quot;);
         bytes32 r;
         bytes32 s;
         uint8 v;
         // 目前只能用assembly (内联汇编)来从签名中获得r,s,v的值
         assembly {
             /*
             前32 bytes存储签名的长度 (动态数组存储规则)
             add(sig, 32) = sig的指针 + 32
             等效为略过signature的前32 bytes
             mload(p) 载入从内存地址p起始的接下来32 bytes数据
             */
             // 读取长度数据后的32 bytes
             r := mload(add(_signature, 0x20))
             // 读取之后的32 bytes
             s := mload(add(_signature, 0x40))
             // 读取最后一个byte
             v := byte(0, mload(add(_signature, 0x60)))
         }
 
         // 获取签名消息hash
         bytes32 digest = keccak256(abi.encodePacked(
             &quot;\x19\x01&quot;,
             DOMAIN_SEPARATOR,
             keccak256(abi.encode(STORAGE_TYPEHASH, msg.sender, _num))
         )); 
         
         address signer = digest.recover(v, r, s); // 恢复签名者
         require(signer == owner, &quot;EIP712Storage: Invalid signature&quot;); // 检查签名
 
         // 修改状态变量
         number = _num;
     }
 
     /**
      * @dev Return value 
      * @return value of &#x27;number&#x27;
      */
     function retrieve() public view returns (uint256){
         return number;
     }    
 }
```

# 23_ERC20Permit

这一讲，我们介绍 ERC20 代币的一个拓展，`ERC20Permit`，支持使用签名进行授权，改善用户体验。它在 EIP-2612 中被提出，已纳入以太坊标准，并被 USDC，ARB 等代币使用。

## ERC20

我们在[31讲](https://github.com/AmazingAng/WTF-Solidity/blob/main/31_ERC20/readme.md)中介绍了ERC20，以太坊最流行的代币标准。它流行的一个主要原因是 `approve` 和 `transferFrom` 两个函数搭配使用，使得代币不仅可以在外部拥有账户（EOA）之间转移，还可以被其他合约使用。

但是，ERC20的 `approve` 函数限制了只有代币所有者才能调用，这意味着所有 ERC20 代币的初始操作必须由 EOA 执行。举个例子，用户 A 在去中心化交易所使用 USDT 交换 ETH，必须完成两个交易：第一步用户 A 调用 `approve` 将 USDT 授权给合约，第二步用户 A 调用合约进行交换。非常麻烦，并且用户必须持有 ETH 用于支付交易的 gas。

## ERC20Permit

EIP-2612 提出了 `ERC20Permit`，扩展了 ERC20 标准，添加了一个 `permit` 函数，允许用户通过 EIP-712 签名修改授权，而不是通过 `msg.sender`。这有两点好处：

1. 授权这步仅需用户在链下签名，减少一笔交易。

1. 签名后，用户可以委托第三方进行后续交易，不需要持有 ETH：用户 A 可以将签名发送给 拥有gas的第三方 B，委托 B 来执行后续交易。

![](img/6bda915b8741fdb9b45b9dd323f80b1c.png)

## 合约

### IERC20Permit 接口合约

首先，让我们学习下 `ERC20Permit` 的接口合约，它定义了 3 个函数：

+ `permit()`: 根据 `owner` 的签名, 将 `owenr` 的ERC20代币余额授权给 `spender`，数量为 `value`。要求：

+ `spender` 不能是零地址。

+ `deadline` 必须是未来的时间戳。

+ `v`，`r` 和 `s` 必须是 `owner` 对 EIP712 格式的函数参数的有效 secp256k1 签名。

+ 签名必须使用 `owner` 当前的 `nonce`。

+ `nonces()`: 返回 `owner` 的当前 `nonce`。每次为 `permit()` 函数生成签名时，都必须包括此值。每次成功调用 `permit()` 函数都会将 `owner` 的 `nonce` 增加 1，防止多次使用同一个签名。

+ `DOMAIN_SEPARATOR()`: [返回用](https://github.com/AmazingAng/WTF-Solidity/blob/main/52_EIP712/readme.md)于编码 `permit()` 函数的签名的域分隔符（domain separator），如 `EIP712` 所定义。

```
 // SPDX-License-Identifier: MIT
 pragma solidity ^0.8.0;
 
 /**
  * @dev ERC20 Permit 扩展的接口，允许通过签名进行批准，如 https://eips.ethereum.org/EIPS/eip-2612[EIP-2612]中定义。
  */
 interface IERC20Permit {
     /**
      * @dev 根据owner的签名, 将 owenr 的ERC20余额授权给 spender，数量为 value
      */
     function permit(
         address owner,
         address spender,
         uint256 value,
         uint256 deadline,
         uint8 v,
         bytes32 r,
         bytes32 s
     ) external;
 
     /**
      * @dev 返回 owner 的当前 nonce。每次为 {permit} 生成签名时，都必须包括此值。
      */
     function nonces(address owner) external view returns (uint256);
 
     /**
      * @dev 返回用于编码 {permit} 的签名的域分隔符（domain separator）
      */
     // solhint-disable-next-line func-name-mixedcase
     function DOMAIN_SEPARATOR() external view returns (bytes32);
 }
```

### ERC20Permit 合约

下面，让我们写一个简单的 `ERC20Permit` 合约，它实现了 `IERC20Permit` 定义的所有接口。合约包含 2 个状态变量:

+ `_nonces`: `address` -> `uint` 的映射，记录了所有用户当前的 `nonce` 值，

+ `_PERMIT_TYPEHASH`: 常量，记录了 `permit()` 函数的类型哈希。

合约包含 5 个函数:

+ 构造函数: 初始化代币的 `name` 和 `symbol`。

+ `permit()`: `ERC20Permit` 最核心的函数，实现了 `IERC20Permit` 的 `permit()` 。它首先检查签名是否过期，然后用 `_PERMIT_TYPEHASH`, `owner`, `spender`, `value`, `nonce`, `deadline` 还原签名消息，并验证签名是否有效。如果签名有效，则调用`ERC20`的 `_approve()` 函数进行授权操作。

+ `nonces()`: 实现了 `IERC20Permit` 的 `nonces()` 函数。

+ `DOMAIN_SEPARATOR()`: 实现了 `IERC20Permit` 的 `DOMAIN_SEPARATOR()` 函数。

+ `_useNonce()`: 消费 `nonce` 的函数，返回用户当前的 `nonce`，并增加 1。
