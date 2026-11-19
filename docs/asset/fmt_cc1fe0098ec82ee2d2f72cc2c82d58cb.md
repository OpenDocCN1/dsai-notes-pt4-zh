
# ERC165与ERC721详解

上面说到,为了防止NFT被转到一个没有能力操作NFT的合约中去,目标必须正确实现`ERC721TokenReceiver`接口：

```
interface ERC721TokenReceiver {
    function onERC721Received(address _operator, address _from, uint256 _tokenId, bytes _data) external returns(bytes4);
}
```

拓展到编程语言的世界中去，无论是Java的`interface`，还是Rust的`Trait`(当然solidity中和trait更像的是`library`)，只要是和接口沾边的，都在透露着一种这样的意味：接口是某些行为的集合(在solidity中更甚，接口完全等价于函数选择器的集合)，某个类型只要实现了某个接口`ERC721TokenReceiver`，就表明该类型拥有这样的一种功能。因此，只要某个[contract类型](https://eips.ethereum.org/EIPS/eip-165#how-to-detect-if-a-contract-implements-erc-165)实现了上述的`ERC721TokenReceiver`接口(更具体而言就是实现了`onERC721Received`这个函数),该contract类型就对外表明了自己拥有管理NFT的能力。当然操作NFT的逻辑被实现在该合约其他的函数中。 `ERC721`标准在执行`safeTransferFrom`的时候会检查目标合约是否实现了`onERC721Received`函数,这是一种利用`ERC165`思想进行的操作。

### 什么是ERC165？

`ERC165`是一种对外表明自己实现了哪些接口的技术标准。就像上面所说的，实现了一个接口就表明合约拥有种特殊能力。有一些合约与其他合约交互时，期望目标合约拥有某些功能，那么合约之间就能够通过`ERC165`标准对对方进行查询以检查对方是否拥有相应的能力。

以`ERC721`合约为例，当外部对某个合约进行检查其是否是`ERC721`时，怎么做？ 。按照这个说法，检查步骤应该是首先检查该合约是否实现了`ERC165`, 再检查该合约实现的其他特定接口。此时该特定接口是`IERC721`. `IERC721`的是`ERC721`的基本接口(为什么说基本，是因为还有其他的诸如`ERC721Metadata` `ERC721Enumerable`这样的拓展)：

```
/// 注意这个**0x80ac58cd**
/// ** **⚠⚠⚠ Note: the ERC-165 identifier for this interface is 0x80ac58cd. ⚠⚠⚠
interface ERC721 /* is ERC165 */ {
    event Transfer(address indexed _from, address indexed _to, uint256 indexed _tokenId);

    event Approval(address indexed _owner, address indexed _approved, uint256 indexed _tokenId);

    event ApprovalForAll(address indexed _owner, address indexed _operator, bool _approved);

    function balanceOf(address _owner) external view returns (uint256);

    function ownerOf(uint256 _tokenId) external view returns (address);

    function safeTransferFrom(address _from, address _to, uint256 _tokenId, bytes data) external payable;

    function safeTransferFrom(address _from, address _to, uint256 _tokenId) external payable;

    function transferFrom(address _from, address _to, uint256 _tokenId) external payable;

    function approve(address _approved, uint256 _tokenId) external payable;

    function setApprovalForAll(address _operator, bool _approved) external;

    function getApproved(uint256 _tokenId) external view returns (address);

    function isApprovedForAll(address _owner, address _operator) external view returns (bool);
}
```

**0x80ac58cd** = `bytes4(keccak256(ERC721.Transfer.selector) ^ keccak256(ERC721.Approval.selector) ^ ··· ^keccak256(ERC721.isApprovedForAll.selector))`，这是`ERC165`规定的计算方式。

那么，类似的，能够计算出`ERC165`本身的接口(它的接口里只有一个 `function supportsInterface(bytes4 interfaceID) external view returns (bool);` 函数，对其进行`bytes4(keccak256(supportsInterface.selector))` 得到`0x01ffc9a7`。此外，`ERC721`还定义了一些拓展接口，比如`ERC721Metadata` ，长这样：

```
///  Note: the ERC-165 identifier for this interface is 0x5b5e139f.
interface ERC721Metadata /* is ERC721 */ {
    function name() external view returns (string _name);
    function symbol() external view returns (string _symbol);
    function tokenURI(uint256 _tokenId) external view returns (string); // 这个很重要，前端展示的小图片的链接都是这个函数返回的
}
```

`0x5b5e139f` 的计算就是:

```
bytes4(keccak256(ERC721Metadata.name.selector)^keccak256(ERC721Metadata.symbol.selector)^keccak256(ERC721Metadata.tokenURI.selector))
```

solamte实现的`ERC721.sol`是怎么完成这些`ERC165`要求的特性的呢？

```
function supportsInterface(bytes4 interfaceId) public view virtual returns (bool) {
        return
            interfaceId == 0x01ffc9a7 || // ERC165 Interface ID for ERC165
            interfaceId == 0x80ac58cd || // ERC165 Interface ID for ERC721
            interfaceId == 0x5b5e139f; // ERC165 Interface ID for ERC721Metadata
}
```

没错就这么简单。当外界按照[link1](https://eips.ethereum.org/EIPS/eip-165#how-to-detect-if-a-contract-implements-erc-165) 的步骤去做检查的时候，如果外界想检查这个合约是否实现了165,好说，就是`supportsInterface`函数在入参是`0x01ffc9a7`时必须返回`true`，在入参是`0xffffffff`时，返回值必须是`false`。上述实现完美达成要求。

当外界想检查这个合约是否是`ERC721`的时候，好说，入参是`0x80ac58cd` 的时候表明外界想做这个检查。返回`true`。

当外界想检查这个合约是否实现`ERC721`的拓展`ERC721Metadata`接口时，入参是`0x5b5e139f`。好说，返回了`true`。

并且由于该函数是`virtual`的。因此该合约的使用者可以继承该合约，然后继续实现`ERC721Enumerable` 接口。实现完里面的什么`totalSupply` 啊之类的函数之后，把继承的`supportsInterface`重实现为

```
function supportsInterface(bytes4 interfaceId) public view virtual returns (bool) {
        return
            interfaceId == 0x01ffc9a7 || // ERC165 Interface ID for ERC165
            interfaceId == 0x80ac58cd || // ERC165 Interface ID for ERC721
            interfaceId == 0x5b5e139f || // ERC165 Interface ID for ERC721Metadata
            interfaceId == 0x780e9d63;   // ERC165 Interface ID for ERC721Enumerable
}
```

优雅，简洁，可拓展性拉满。

## 5 荷兰拍卖

介绍荷兰拍卖，并通过简化版Azuki荷兰拍卖代码，讲解如何通过荷兰拍卖发售`ERC721`标准的NFT。

### 荷兰拍卖

荷兰拍卖（Dutch Auction）是一种特殊的拍卖形式。 亦称“减价拍卖”，它是指拍卖标的的竞价由高到低依次递减直到第一个竞买人应价（达到或超过底价）时击槌成交的一种拍卖。

![](img/5248da9849c03b29b27c04fc80505b0a.png)

在币圈，很多NFT通过荷兰拍卖发售，其中包括Azuki和World of Women，其中Azuki通过荷兰拍卖筹集了超过8000枚ETH。

项目方非常喜欢这种拍卖形式，主要有两个原因

1. 荷兰拍卖的价格由最高慢慢下降，能让项目方获得最大的收入。

2. 拍卖持续较长时间（通常6小时以上），可以避免gas war。

### DutchAuction合约

代码基于Azuki的[代码](https://etherscan.io/address/0xed5af388653567af2f388e6224dc7c4b3241c544#code)简化而成。`DutchAuction`合约继承了之前介绍的`ERC721`和`Ownable`合约：

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;

import "@openzeppelin/contracts/access/Ownable.sol";
import "https://github.com/AmazingAng/WTFSolidity/blob/main/34_ERC721/ERC721.sol";

contract DutchAuction is Ownable, ERC721 {
```

#### DutchAuction状态变量

合约中一共有9个状态变量，其中有6个和拍卖相关，他们是：

+   `COLLECTOIN_SIZE`：NFT总量。

+   `AUCTION_START_PRICE`：荷兰拍卖起拍价，也是最高价。

+   `AUCTION_END_PRICE`：荷兰拍卖结束价，也是最低价/地板价。

+   `AUCTION_TIME`：拍卖持续时长。

+   `AUCTION_DROP_INTERVAL`：每过多久时间，价格衰减一次。

+   `auctionStartTime`：拍卖起始时间（区块链时间戳，`block.timestamp`）。

```
    uint256 public constant COLLECTOIN_SIZE = 10000; // NFT总数
    uint256 public constant AUCTION_START_PRICE = 1 ether; // 起拍价(最高价)
    uint256 public constant AUCTION_END_PRICE = 0.1 ether; // 结束价(最低价/地板价)
    uint256 public constant AUCTION_TIME = 10 minutes; // 拍卖时间，为了测试方便设为10分钟
    uint256 public constant AUCTION_DROP_INTERVAL = 1 minutes; // 每过多久时间，价格衰减一次
    uint256 public constant AUCTION_DROP_PER_STEP =
        (AUCTION_START_PRICE - AUCTION_END_PRICE) /
        (AUCTION_TIME / AUCTION_DROP_INTERVAL); // 每次价格衰减步长
    
    uint256 public auctionStartTime; // 拍卖开始时间戳
    string private _baseTokenURI;   // metadata URI
    uint256[] private _allTokens; // 记录所有存在的tokenId 
```

#### DutchAuction函数

荷兰拍卖合约中共有9个函数，与`ERC721`相关的函数我们这里不再重复介绍，只介绍和拍卖相关的函数。

+   设定拍卖起始时间：我们在构造函数中会声明当前区块时间为起始时间，项目方也可以通过`setAuctionStartTime()`函数来调整：

```
    constructor() ERC721("WTF Dutch Auctoin", "WTF Dutch Auctoin") {
        auctionStartTime = block.timestamp;
    }

    // auctionStartTime setter函数，onlyOwner
    function setAuctionStartTime(uint32 timestamp) external onlyOwner {
        auctionStartTime = timestamp;
    }
```

+   获取拍卖实时价格：`getAuctionPrice()`函数通过当前区块时间以及拍卖相关的状态变量来计算实时拍卖价格。

当`block.timestamp`小于起始时间，价格为最高价`AUCTION_START_PRICE`；

当`block.timestamp`大于结束时间，价格为最低价`AUCTION_END_PRICE`；

当`block.timestamp`处于两者之间时，则计算出当前的衰减价格。

```
    // 获取拍卖实时价格
    function getAuctionPrice()
        public
        view
        returns (uint256)
    {
        if (block.timestamp < auctionStartTime) {
        return AUCTION_START_PRICE;
        }else if (block.timestamp - auctionStartTime >= AUCTION_TIME) {
        return AUCTION_END_PRICE;
        } else {
        uint256 steps = (block.timestamp - auctionStartTime) /
            AUCTION_DROP_INTERVAL;
        return AUCTION_START_PRICE - (steps * AUCTION_DROP_PER_STEP);
        }
    }
```

+   用户拍卖并铸造NFT：用户通过调用`auctionMint()`函数，支付ETH参加荷兰拍卖并铸造NFT。

该函数首先检查拍卖是否开始/铸造是否超出NFT总量。接着，合约通过`getAuctionPrice()`和铸造数量计算拍卖成本，并检查用户支付的ETH是否足够：如果足够，则将NFT铸造给用户，并退回超额的ETH；反之，则回退交易。
