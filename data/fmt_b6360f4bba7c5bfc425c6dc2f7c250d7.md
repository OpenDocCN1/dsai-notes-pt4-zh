
```
    // 拍卖mint函数
    function auctionMint(uint256 quantity) external payable{
        uint256 _saleStartTime = uint256(auctionStartTime); // 建立local变量，减少gas花费
        require(
        _saleStartTime != 0 && block.timestamp >= _saleStartTime,
        "sale has not started yet"
        ); // 检查是否设置起拍时间，拍卖是否开始
        require(
        totalSupply() + quantity <= COLLECTOIN_SIZE,
        "not enough remaining reserved for auction to support desired mint amount"
        ); // 检查是否超过NFT上限

        uint256 totalCost = getAuctionPrice() * quantity; // 计算mint成本
        require(msg.value >= totalCost, "Need to send more ETH."); // 检查用户是否支付足够ETH
        
        // Mint NFT
        for(uint256 i = 0; i < quantity; i++) {
            uint256 mintIndex = totalSupply();
            _mint(msg.sender, mintIndex);
            _addTokenToAllTokensEnumeration(mintIndex);
        }
        // 多余ETH退款
        if (msg.value > totalCost) {
            payable(msg.sender).transfer(msg.value - totalCost); //注意一下这里是否有重入的风险
        }
    }
```

+ 项目方取出筹集的ETH：项目方可以通过`withdrawMoney()`函数提走拍卖筹集的ETH。

```
    // 提款函数，onlyOwner
    function withdrawMoney() external onlyOwner {
        (bool success, ) = msg.sender.call{value: address(this).balance}(""); // call函数的调用方式详见第22讲
        require(success, "Transfer failed.");
    }
```

# 6 Merkle Tree

介绍Merkle Tree，以及如何利用Merkle Tree发放NFT白名单。

### Merkle Tree

Merkle Tree，也叫默克尔树或哈希树，是区块链的底层加密技术，被比特币和以太坊区块链广泛采用。Merkle Tree是一种自下而上构建的加密树，每个叶子是对应数据的哈希，而每个非叶子为它的2个子节点的哈希。

Merkle Tree允许对大型数据结构的内容进行有效和安全的验证（`Merkle Proof`）。对于有`N`个叶子结点的Merkle Tree，在已知`root`根值的情况下，验证某个数据是否有效（属于Merkle Tree叶子结点）只需要`log(N)`个数据（也叫`proof`），非常高效。如果数据有误，或者给的`proof`错误，则无法还原出`root`根植。 下面的例子中，叶子`L1`的`Merkle proof`为`Hash 0-1`和`Hash 1`：知道这两个值，就能验证`L1`的值是不是在Merkle Tree的叶子中。为什么呢？ 因为通过叶子`L1`我们就可以算出`Hash 0-0`，我们又知道了`Hash 0-1`，那么`Hash 0-0`和`Hash 0-1`就可以联合算出`Hash 0`，然后我们又知道`Hash 1`，`Hash 0`和`Hash 1`就可以联合算出`Top Hash`，也就是`root`节点的hash。

![](img/799d00ebc711f37f74d809571afa7f92.png)

### 生成Merkle Tree

我们可以利用[网页](https://lab.miguelmota.com/merkletreejs/example/)或者Javascript库`merkletreejs`来生成Merkle Tree。

这里我们用网页来生成4个地址作为叶子结点的Merkle Tree。叶子结点输入：

```
[
    "0x5B38Da6a701c568545dCfcB03FcB875f56beddC4", 
    "0xAb8483F64d9C6d1EcF9b849Ae677dD3315835cb2",
    "0x4B20993Bc481177ec7E8f571ceCaE8A9e22C02db",
    "0x78731D3Ca6b7E34aC0F824c42a7cC18A495cabaB"
]
```

在菜单里选上`Keccak-256`、`hashLeaves`和`sortPairs`选项，然后点击`Compute`，Merkle Tree就生成好了。Merkle Tree展开为：

```
└─ 根: eeefd63003e0e702cb41cd0043015a6e26ddb38073cc6ffeb0ba3e808ba8c097
   ├─ 9d997719c0a5b5f6db9b8ac69a988be57cf324cb9fffd51dc2c37544bb520d65
   │  ├─ 叶子0：5931b4ed56ace4c46b68524cb5bcbf4195f1bbaacbe5228fbd090546c88dd229
   │  └─ 叶子1：999bf57501565dbd2fdcea36efa2b9aef8340a8901e3459f4a4c926275d36cdb
   └─ 4726e4102af77216b09ccd94f40daa10531c87c4d60bba7f3b3faf5ff9f19b3c
      ├─ 叶子2：04a10bfd00977f54cc3450c9b25c9b3a502a089eba0097ba35fc33c4ea5fcb54
      └─ 叶子3：dfbe3e504ac4e35541bebad4d0e7574668e16fefa26cd4172f93e18b59ce9486
```

![](img/96b48af1ab9d860290805819a1c98668.png)

### Merkle Proof验证

通过网站，我们可以得到地址`0`的`proof`如下，即图2中蓝色结点的哈希值：

```
[
  "0x999bf57501565dbd2fdcea36efa2b9aef8340a8901e3459f4a4c926275d36cdb",
  "0x4726e4102af77216b09ccd94f40daa10531c87c4d60bba7f3b3faf5ff9f19b3c"
]
```

![](img/4cf68c0f8d821adac04f125a918d8c6f.png)

我们利用`MerkleProof`库来验证：

```
library MerkleProof {
    /**
     * @dev 当通过`proof`和`leaf`重建出的`root`与给定的`root`相等时，返回`true`，数据有效。
     * 在重建时，叶子节点对和元素对都是排序过的。
     */
    function verify(
        bytes32[] memory proof,
        bytes32 root,
        bytes32 leaf
    ) internal pure returns (bool) {
        return processProof(proof, leaf) == root;
    }

    /**
     * @dev Returns 通过Merkle树用`leaf`和`proof`计算出`root`. 当重建出的`root`和给定的`root`相同时，`proof`才是有效的。
     * 在重建时，叶子节点对和元素对都是排序过的。
     */
    function processProof(bytes32[] memory proof, bytes32 leaf) internal pure returns (bytes32) {
        bytes32 computedHash = leaf;
        for (uint256 i = 0; i < proof.length; i++) {
            computedHash = _hashPair(computedHash, proof[i]);
        }
        return computedHash;
    }

    // Sorted Pair Hash
    function _hashPair(bytes32 a, bytes32 b) private pure returns (bytes32) {
        return a < b ? keccak256(abi.encodePacked(a, b)) : keccak256(abi.encodePacked(b, a));
    }
}
```

`MerkleProof`库有三个函数：

1. `verify()`函数：利用`proof`数组来验证`leaf`是否属于根为`root`的Merkle Tree中，如果是，则返回`true`。它调用了`processProof()`函数。

1. `processProof()`函数：利用`proof`和`leaf`依次计算出Merkle Tree的`root`。它调用了`_hashPair()`函数。

1. `_hashPair()`函数：用`keccak256()`函数计算非根节点对应的两个子节点的哈希（排序后）。

我们将地址`0`，`root`和对应的`proof`输入到`verify()`函数，将返回`ture`。因为地址`0`在根为`root`的Merkle Tree中，且`proof`正确。如果改变了其中任意一个值，都将返回`false`。

### 利用Merkle Tree发放NFT白名单

一份拥有800个地址的白名单，更新一次所需的gas fee很容易超过1个ETH。而由于Merkle Tree验证时，`leaf`和`proof`可以存在后端，链上仅需存储一个`root`的值，非常节省gas，项目方经常用它来发放白名单。很多`ERC721`标准的NFT和`ERC20`标准代币的白名单/空投都是利用Merkle Tree发出的，比如optimism的空投。

这里，我们介绍如何利用`MerkleTree`合约来发放NFT白名单：

```
contract MerkleTree is ERC721 {
    bytes32 immutable public root; // Merkle树的根
    mapping(address => bool) public mintedAddress;   // 记录已经mint的地址

    constructor(string memory name, string memory symbol, bytes32 merkleroot)
    ERC721(name, symbol)
    {
        root = merkleroot;
    }

    function mint(address account, uint256 tokenId, bytes32[] calldata proof)
    external
    {
        require(_verify(_leaf(account), proof), "Invalid merkle proof"); // Merkle检验通过
        require(!mintedAddress[account], "Already minted!"); // 地址没有mint过
        _mint(account, tokenId); // mint
        mintedAddress[account] = true; // 记录mint过的地址
    }

    function _leaf(address account)
    internal pure returns (bytes32)
    {
        return keccak256(abi.encodePacked(account));
    }

    function _verify(bytes32 leaf, bytes32[] memory proof)
    internal view returns (bool)
    {
        return MerkleProof.verify(proof, root, leaf);
    }
}
```

`MerkleTree`合约继承了`ERC721`标准，并利用了`MerkleProof`库。

#### 状态变量

合约中共有两个状态变量：

+ `root`存储了Merkle Tree的根，部署合约的时候赋值。

+ `mintedAddress`是一个`mapping`，记录了已经`mint`过的地址，某地址`mint`成功后进行赋值。

#### 函数

合约中共有4个函数：

+ 构造函数：初始化NFT的名称和代号，还有Merkle Tree的`root`。

+ `mint()`函数：利用白名单铸造NFT。参数为白名单地址`account`，铸造的`tokenId`，和`proof`。首先验证`address`是否在白名单中，验证通过则把序号为`tokenId`的NFT铸造给该地址，并将它记录到`mintedAddress`。此过程中调用了`_leaf()`和`_verify()`函数。

+ `_leaf()`函数：计算了Merkle Tree的叶子地址的哈希。

+ `_verify()`函数：调用了`MerkleProof`库的`verify()`函数，进行Merkle Tree验证。

# 7 数字签名

我们将简单的介绍以太坊中的数字签名`ECDSA`，以及如何利用它发放NFT白名单。代码中的`ECDSA`库由`OpenZeppelin`的同名库简化而成。

### 数字签名

如果你用过opensea交易NFT，对签名就不会陌生。下图是小狐狸（metamask）钱包进行签名时弹出的窗口，它可以证明你拥有私钥的同时不需要对外公布私钥。

![](img/c9f494d0af6843abf565d3f880abd009.png)

以太坊使用[的数字签名算法](https://en.wikipedia.org/wiki/Digital_signature)叫双椭圆曲线数字签名算法（`ECDSA`），基于双椭圆曲线“私钥-公钥”对的数字签名算法。它主要起到了三个作用：

1. **身份认证**：证明签名方是私钥的持有人。

1. **不可否认**：发送方不能否认发送过这个消息。

1. **完整性**：消息在传输过程中无法被修改。

### ECDSA合约

`ECDSA`标准中包含两个部分：

1. 签名者利用私钥（隐私的）对消息（公开的）创建签名（公开的）。

1. 其他人使用消息（公开的）和签名（公开的）恢复签名者的公钥（公开的）并验证签名。 我们将配合`ECDSA`库讲解这两个部分。本教程所用的私钥，公钥，消息，以太坊签名消息，签名如下所示：

```
私钥: 0x227dbb8586117d55284e26620bc76534dfbd2394be34cf4a09cb775d593b6f2b
公钥: 0xe16C1623c1AA7D919cd2241d8b36d9E79C1Be2A2
消息: 0x1bf2c0ce4546651a1a2feb457b39d891a6b83931cc2454434f39961345ac378c
以太坊签名消息: 0xb42ca4636f721c7a331923e764587e98ec577cea1a185f60dfcc14dbb9bd900b
签名: 0x390d704d7ab732ce034203599ee93dd5d3cb0d4d1d7c600ac11726659489773d559b12d220f99f41d17651b0c1c6a669d346a397f8541760d6b32a5725378b241c
```

#### 创建签名

1. **打包消息：** 在以太坊的`ECDSA`标准中，被签名的消息是一组数据的`keccak256`哈希，为`bytes32`类型。我们可以把任何想要签名的内容利用`abi.encodePacked()`函数打包，然后用`keccak256()`计算哈希，作为消息。我们例子中的消息是由一个`address`类型变量和一个`uint256`类型变量得到的：
