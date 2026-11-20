
# BAYC，但是 ERC1155

我们魔改下`ERC721`标准的无聊猿`BAYC`，创建一个免费铸造的`BAYC1155`。我们修改`_baseURI()`函数，使得`BAYC1155`的`uri`和`BAYC`的`tokenURI`一样。这样，`BAYC1155`元数据会与无聊猿的相同：

```
```solidity
// SPDX-License-Identifier: MIT
// by 0xAA
pragma solidity ^0.8.4;

import "./ERC1155.sol";

contract BAYC1155 is ERC1155{
    uint256 constant MAX_ID = 10000; 
    // 构造函数
    constructor() ERC1155("BAYC1155", "BAYC1155"){
    }

    //BAYC的baseURI为ipfs://QmeSjSinHpPnmXmspMjwiXyN6zS4E9zccariGR3jxcaWtq/ 
    function _baseURI() internal pure override returns (string memory) {
        return "ipfs://QmeSjSinHpPnmXmspMjwiXyN6zS4E9zccariGR3jxcaWtq/";
    }
    
    // 铸造函数
    function mint(address to, uint256 id, uint256 amount) external {
        // id 不能超过10,000
        require(id < MAX_ID, "id overflow");
        _mint(to, id, amount, "");
    }

    // 批量铸造函数
    function mintBatch(address to, uint256[] memory ids, uint256[] memory amounts) external {
        // id 不能超过10,000
        for (uint256 i = 0; i < ids.length; i++) {
            require(ids[i] < MAX_ID, "id overflow");
        }
        _mintBatch(to, ids, amounts, "");
    }
}
```

# 11 WETH

# 12 分账 PaymentSplit

这一讲，我们介绍分账合约，该合约允许将`ETH`按权重转给一组账户中，进行分账。代码部分由`oppenzepplin`库的`PaymentSplitter`合约简化而来。

## 分账

分账就是按照一定比例分钱财。在现实中，经常会有“分赃不均”的事情发生；而在区块链的世界里，Code is Law，我们可以事先把每个人应分的比例写在智能合约中，获得收入后，再由智能合约来进行分账。

## 分账合约

分账合约(`PaymentSplit`)具有以下几个特点：

1. 在创建合约时定好分账受益人`payees`和每人的份额`shares`。

2. 份额可以是相等，也可以是其他任意比例。

3. 在该合约收到的所有`ETH`中，每个受益人将能够提取与其分配的份额成比例的金额。

4. 分账合约遵循`Pull Payment`模式，付款不会自动转入账户，而是保存在此合约中。受益人通过调用`release()`函数触发实际转账。

### 事件

分账合约中共有3个事件：

+ `PayeeAdded`：增加受益人事件。

+ `PaymentReleased`：受益人提款事件。

+ `PaymentReceived`：分账合约收款事件。

```
```solidity
     // 事件
     event PayeeAdded(address account, uint256 shares); // 增加受益人事件
     event PaymentReleased(address to, uint256 amount); // 受益人提款事件
     event PaymentReceived(address from, uint256 amount); // 合约收款事件
```

### 状态变量

分账合约中共有5个状态变量，用来记录受益地址、份额、支付出去的`ETH`等变量：

+ `totalShares`：总份额，为`shares`的和。

+ `totalReleased`：从分账合约向受益人支付出去的`ETH`，为`released`的和。

+ `payees`：`address`数组，记录受益人地址

+ `shares`：`address`到`uint256`的映射，记录每个受益人的份额。

+ `released`：`address`到`uint256`的映射，记录分账合约支付给每个受益人的金额。

```
```solidity
     uint256 public totalShares; // 总份额
     uint256 public totalReleased; // 总支付
 
     mapping(address => uint256) public shares; // 每个受益人的份额
     mapping(address => uint256) public released; // 支付给每个受益人的金额
     address[] public payees; // 受益人数组
```

### 函数

分账合约中共有6个函数：

+ 构造函数：始化受益人数组`_payees`和分账份额数组`_shares`，其中数组长度不能为0，两个数组长度要相等。`shares`中元素要大于0，`payees`中地址不能为0地址且不能有重复地址。

+ `receive()`：回调函数，在分账合约收到`ETH`时释放`PaymentReceived`事件。

+ `release()`：分账函数，为有效受益人地址`_account`分配相应的`ETH`。任何人都可以触发这个函数，但`ETH`会转给受益人地址`account`。调用了`releasable()`函数。

+ `releasable()`：计算一个受益人地址应领取的`ETH`。调用了`pendingPayment()`函数。

+ `pendingPayment()`：根据受益人地址`_account`，分账合约总收入`_totalReceived`和该地址已领取的钱`_alreadyReleased`，计算该受益人现在应分的`ETH`。

+ `_addPayee()`：新增受益人函数及其份额函数。在合约初始化的时候被调用，之后不能修改。

```
```solidity
     /**
      * @dev 初始化受益人数组_payees和分账份额数组_shares
      * 数组长度不能为0，两个数组长度要相等。_shares中元素要大于0，_payees中地址不能为0地址且不能有重复地址
      */
     constructor(address[] memory _payees, uint256[] memory _shares) payable {
         // 检查_payees和_shares数组长度相同，且不为0
         require(_payees.length == _shares.length, "PaymentSplitter: payees and shares length mismatch");
         require(_payees.length > 0, "PaymentSplitter: no payees");
         // 调用_addPayee，更新受益人地址payees、受益人份额shares和总份额totalShares
         for (uint256 i = 0; i < _payees.length; i++) {
             _addPayee(_payees[i], _shares[i]);
         }
     }
 
     /**
      * @dev 回调函数，收到ETH释放PaymentReceived事件
      */
     receive() external payable virtual {
         emit PaymentReceived(msg.sender, msg.value);
     }
 
     /**
      * @dev 为有效受益人地址_account分帐，相应的ETH直接发送到受益人地址。任何人都可以触发这个函数，但钱会打给account地址。
      * 调用了releasable()函数。
      */
     function release(address payable _account) public virtual {
         // account必须是有效受益人
         require(shares[_account] > 0, "PaymentSplitter: account has no shares");
         // 计算account应得的eth
         uint256 payment = releasable(_account);
         // 应得的eth不能为0
         require(payment != 0, "PaymentSplitter: account is not due payment");
         // 更新总支付totalReleased和支付给每个受益人的金额released
         totalReleased += payment;
         released[_account] += payment;
         // 转账
         _account.transfer(payment);
         emit PaymentReleased(_account, payment);
     }
 
     /**
      * @dev 计算一个账户能够领取的eth。
      * 调用了pendingPayment()函数。
      */
     function releasable(address _account) public view returns (uint256) {
         // 计算分账合约总收入totalReceived
         uint256 totalReceived = address(this).balance + totalReleased;
         // 调用_pendingPayment计算account应得的ETH
         return pendingPayment(_account, totalReceived, released[_account]);
     }
 
     /**
      * @dev 根据受益人地址_account, 分账合约总收入_totalReceived和该地址已领取的钱_alreadyReleased，计算该受益人现在应分的ETH。
      */
     function pendingPayment(
         address _account,
         uint256 _totalReceived,
         uint256 _alreadyReleased
     ) public view returns (uint256) {
         // account应得的ETH = 总应得ETH - 已领到的ETH
         return (_totalReceived * shares[_account]) / totalShares - _alreadyReleased;
     }
 
     /**
      * @dev 新增受益人_account以及对应的份额_accountShares。只能在构造器中被调用，不能修改。
      */
     function _addPayee(address _account, uint256 _accountShares) private {
         // 检查_account不为0地址
         require(_account != address(0), "PaymentSplitter: account is the zero address");
         // 检查_accountShares不为0
         require(_accountShares > 0, "PaymentSplitter: shares are 0");
         // 检查_account不重复
         require(shares[_account] == 0, "PaymentSplitter: account already has shares");
         // 更新payees，shares和totalShares
         payees.push(_account);
         shares[_account] = _accountShares;
         totalShares += _accountShares;
         // 释放增加受益人事件
         emit PayeeAdded(_account, _accountShares);
     }
```

# 13 线性释放TokenVesting

这一讲，我们将介绍代币归属条款，并写一个线性释放`ERC20`代币的合约。代码由`OpenZeppelin`的`VestingWallet`合约简化而来。

## 代币归属条款

在传统金融领域，一些公司会向员工和管理层提供股权。但大量股权同时释放会在短期产生抛售压力，拖累股价。因此，公司通常会引入一个归属期来延迟承诺资产的所有权。同样的，在区块链领域，Web3初创公司会给团队分配代币，同时也会将代币低价出售给风投和私募。如果他们把这些低成本的代币同时提到交易所变现，币价将被砸穿，散户直接成为接盘侠。

所以，项目方一般会约定代币归属条款（`token vesting`），在归属期内逐步释放代币，减缓抛压，并防止团队和资本方过早躺平。

## 线性释放

线性释放指的是代币在归属期内匀速释放。举个例子，某私募持有365,000枚`ICU`代币，归属期为1年（365天），那么每天会释放1,000枚代币。

下面，我们就写一个锁仓并线性释放`ERC20`代币的合约`TokenVesting`。它的逻辑很简单：

+ 项目方规定线性释放的起始时间、归属期和受益人。

+ 项目方将锁仓的`ERC20`代币转账给`TokenVesting`合约。

+ 受益人可以调用`release`函数，从合约中取出释放的代币。

### 事件

线性释放合约中共有1个事件。

+ `ERC20Released`：提币事件，当受益人提取释放代币时释放。

```
```solidity
contract TokenVesting {
     // 事件
     event ERC20Released(address indexed token, uint256 amount); // 提币事件
```

### 状态变量

线性释放合约中共有4个状态变量。

+ `beneficiary`：受益人地址。

+ `start`：归属期起始时间戳。

+ `duration`：归属期，单位为秒。

+ `erc20Released`：代币地址->释放数量的映射，记录受益人已领取的代币数量。

```
```solidity
     // 状态变量
     mapping(address => uint256) public erc20Released; // 代币地址->释放数量的映射，记录已经释放的代币
     address public immutable beneficiary; // 受益人地址
     uint256 public immutable start; // 起始时间戳
     uint256 public immutable duration; // 归属期
```

### 函数

线性释放合约中共有3个函数。

+ 构造函数：初始化受益人地址，归属期(秒), 起始时间戳。参数为受益人地址`beneficiaryAddress`和归属期`durationSeconds`。为了方便，起始时间戳用的部署时的区块链时间戳`block.timestamp`。

+ `release()`：提取代币函数，将已释放的代币转账给受益人。调用了`vestedAmount()`函数计算可提取的代币数量，释放`ERC20Released`事件，然后将代币`transfer`给受益人。参数为代币地址`token`。

+ `vestedAmount()`：根据线性释放公式，查询已经释放的代币数量。开发者可以通过修改这个函数，自定义释放方式。参数为代币地址`token`和查询的时间戳`timestamp`。
