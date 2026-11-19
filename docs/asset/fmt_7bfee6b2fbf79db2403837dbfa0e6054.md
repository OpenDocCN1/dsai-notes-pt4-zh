
## 使用 send 和 call 发送 ETH

### send

+ 用法是接收方地址`.send(发送ETH数额)`。

+ `send()`的gas限制是2300，足够用于转账，但对方合约的`fallback()`或`receive()`函数不能实现太复杂的逻辑。

+ `send()`如果转账失败，不会revert。

+ `send()`的返回值是`bool`，代表着转账成功或失败，需要额外代码处理一下。

代码样例：

```
// send()发送ETH
function sendETH(address payable _to, uint256 amount) external payable{
    // 处理下send的返回值，如果失败，revert交易并发送error
    bool success = _to.send(amount);
    if(!success){
        revert SendFailed();
    }
}
```

### call

+ 用法是接收方地址`.call{value: 发送ETH数额}("")`。

+ `call()`没有gas限制，可以支持对方合约`fallback()`或`receive()`函数实现复杂逻辑。

+ `call()`如果转账失败，不会revert。

+ `call()`的返回值是`(bool, data)`，其中`bool`代表着转账成功或失败，需要额外代码处理一下。

代码样例：

```
// call()发送ETH
function callETH(address payable _to, uint256 amount) external payable{
    // 处理下call的返回值，如果失败，revert交易并发送error
    (bool success,) = _to.call{value: amount}("");
    if(!success){
        revert CallFailed();
    }
}
```

这一讲，我们介绍solidity三种发送ETH的方法：`transfer`、`send`和`call`。

+ `call`没有gas限制，最为灵活，是最提倡的方法；

+ `transfer`有2300 gas限制，但是发送失败会自动revert交易，是次优选择；

+ `send`有2300 gas限制，而且发送失败不会自动`revert`交易，几乎没有人用它。

> `send`失败不会导致整个`tx`失败批量转账时会用到

![](img/d06ae041f1264d71db246420d86123d2.png)

# 16 调用其他合约

## 调用已部署合约

开发者写智能合约来调用其他合约，这让以太坊网络上的程序可以复用，从而建立繁荣的生态。很多web3项目依赖于调用其他合约，比如收益农场（yield farming）。这一讲，我们介绍如何在已知合约代码（或接口）和地址情况下调用目标合约的函数。

## 目标合约

我们先写一个简单的合约`OtherContract`来调用。

```
contract OtherContract {
    uint256 private _x = 0; // 状态变量_x
    // 收到eth的事件，记录amount和gas
    event Log(uint amount, uint gas);
    
    // 返回合约ETH余额
    function getBalance() view public returns(uint) {
        return address(this).balance;
    }

    // 可以调整状态变量_x的函数，并且可以往合约转ETH (payable)
    function setX(uint256 x) external payable{
        _x = x;
        // 如果转入ETH，则释放Log事件
        if(msg.value > 0){
            emit Log(msg.value, gasleft());
        }
    }

    // 读取_x
    function getX() external view returns(uint x){
        x = _x;
    }
}
```

这个合约包含一个状态变量`_x`，一个事件`Log`在收到ETH时触发，三个函数：

+ `getBalance()`: 返回合约ETH余额。

+ `setX()`: `external payable`函数，可以设置`_x`的值，并向合约发送ETH。

+ `getX()`: 读取`_x`的值。

## 调用OtherContract合约

我们可以利用合约的地址和合约代码（或接口）来创建合约的引用：`_Name(_Address)`，其中`_Name`是合约名，`_Address`是合约地址。然后用合约的引用来调用它的函数：`_Name(_Address).f()`，其中`f()`是要调用的函数。

下面我们介绍4个调用合约的例子，在remix中编译合约后，分别部署`OtherContract`和`CallContract`：

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;

contract OtherContract {
    uint256 private _x = 0; // 状态变量x
    // 收到eth事件，记录amount和gas
    event Log(uint amount, uint gas);
    
    // 返回合约ETH余额
    function getBalance() view public returns(uint) {
        return address(this).balance;
    }

    // 可以调整状态变量_x的函数，并且可以往合约转ETH (payable)
    function setX(uint256 x) external payable{
        _x = x;
        // 如果转入ETH，则释放Log事件
        if(msg.value > 0){
            emit Log(msg.value, gasleft());
        }
    }

    // 读取x
    function getX() external view returns(uint x){
        x = _x;
    }
}

contract CallContract{
    function callSetX(address _Address, uint256 x) external{
        OtherContract(_Address).setX(x);
    }

    function callGetX(OtherContract _Address) external view returns(uint x){
        x = _Address.getX();
    }

    function callGetX2(address _Address) external view returns(uint x){
        OtherContract oc = OtherContract(_Address);
        x = oc.getX();
    }

    function setXTransferETH(address otherContract, uint256 x) payable external{
        OtherContract(otherContract).setX{value: msg.value}(x);
    }
}
```

1. 传入合约地址

我们可以在函数里传入目标合约地址，生成目标合约的引用，然后调用目标函数。以调用`OtherContract`合约的`setX`函数为例，我们在新合约中写一个`callSetX`函数，传入已部署好的`OtherContract`合约地址`_Address`和`setX`的参数`x`：

```
    function callSetX(address _Address, uint256 x) external{
        OtherContract(_Address).setX(x);
    }
```

复制`OtherContract`合约的地址，填入`callSetX`函数的参数中，成功调用后，调用`OtherContract`合约中的`getX`验证`x`变为123

1. 传入合约变量

我们可以直接在函数里传入合约的引用，只需要把上面参数的`address`类型改为目标合约名，比如`OtherContract`。下面例子实现了调用目标合约的`getX()`函数。

注意该函数参数`OtherContract _Address`底层类型仍然是`address`，生成的ABI中、调用`callGetX`时传入的参数都是`address`类型

```
    function callGetX(OtherContract _Address) external view returns(uint x){
        x = _Address.getX();
    }
```

复制`OtherContract`合约的地址，填入`callGetX`函数的参数中，调用后成功获取`x`的值

1. 创建合约变量

我们可以创建合约变量，然后通过它来调用目标函数。下面例子，我们给变量`oc`存储了`OtherContract`合约的引用：

```
    function callGetX2(address _Address) external view returns(uint x){
        OtherContract oc = OtherContract(_Address);
        x = oc.getX();
    }
```

复制`OtherContract`合约的地址，填入`callGetX2`函数的参数中，调用后成功获取`x`的值

1. 调用合约并发送ETH

如果目标合约的函数是`payable`的，那么我们可以通过调用它来给合约转账：`_Name(_Address).f{value: _Value}()`，其中`_Name`是合约名，`_Address`是合约地址，`f`是目标函数名，`_Value`是要转的ETH数额（以wei为单位）。

`OtherContract`合约的`setX`函数是`payable`的，在下面这个例子中我们通过调用`setX`来往目标合约转账。

```
    function setXTransferETH(address otherContract, uint256 x) payable external{
        OtherContract(otherContract).setX{value: msg.value}(x);
    }
```

复制`OtherContract`合约的地址，填入`setXTransferETH`函数的参数中，并转入10ETH

转账后，我们可以通过`Log`事件和`getBalance()`函数观察目标合约ETH余额的变化。

![](img/85e2ace7ed35710b1b398f7c0fcf11c9.png)

## Call调用合约

`call` 是 `address` 类型的低级成员函数，它用来与其他合约交互。它的返回值为 `(bool, bytes memory)` ，分别对应 `call` 是否成功以及目标函数的返回值。

+ `call` 是 solidity 官方推荐的通过触发 `fallback` 或 `receive` 函数发送 ETH 的方法。

+ 不推荐用 `call` 来调用另一个合约，因为当你调用不安全合约的函数时，你就把主动权交给了它。推荐的方法仍是[声明合约变量后调用函数](https://github.com/AmazingAng/WTFSolidity/tree/main/21_CallContract)，见 第21讲：调用其他合约

+ 当我们不知道对方合约的源代码或 ABI ，就没法生成合约变量；这时，我们仍可以通过 `call` 调用对方合约的函数。

### call 的使用规则

call 的使用规则如下：

```
目标合约地址.call(二进制编码);
```

其中 二进制编码 利用结构化编码函数 `abi.encodeWithSignature` 获得：

```
abi.encodeWithSignature("函数签名", 逗号分隔的具体参数)
```

函数签名 为 "函数名（逗号分隔的参数类型)" 。例如 `abi.encodeWithSignature("f(uint256,address)", _x, _addr)` 。

另外 `call` 在调用合约时可以指定交易发送的 ETH 数额和 gas ：

```
目标合约地址.call{value:发送数额, gas:gas数额}(二进制编码);
```

看起来有点复杂，下面我们举个 `call` 应用的例子。

### 目标合约

我们先写一个简单的目标合约 `OtherContract` 并部署，代码与第19讲中基本相同，只是多了 `fallback` 函数。

```
contract OtherContract {
    uint256 private _x = 0; // 状态变量x
    // 收到eth的事件，记录amount和gas
    event Log(uint amount, uint gas);
    
    fallback() external payable{}

    // 返回合约ETH余额
    function getBalance() view public returns(uint) {
        return address(this).balance;
    }

    // 可以调整状态变量_x的函数，并且可以往合约转ETH (payable)
    function setX(uint256 x) external payable{
        _x = x;
        // 如果转入ETH，则释放Log事件
        if(msg.value > 0){
            emit Log(msg.value, gasleft());
        }
    }

    // 读取x
    function getX() external view returns(uint x){
        x = _x;
    }
}
```

这个合约包含一个状态变量 `x` ，一个在收到 ETH 时触发的事件 `Log` ，三个函数：

+ `getBalance()` : 返回合约 ETH 余额。

+ `setX()` : `external payable` 函数，可以设置 `x` 的值，并向合约发送 ETH 。

+ `getX()` : 读取 `x` 的值。

### 利用 call 调用目标合约

#### 1. Response事件

我们写一个 `Call` 合约来调用目标合约函数。首先定义一个 `Response` 事件，输出 `call` 返回的 `success` 和 `data` ，方便我们观察返回值。

```
// 定义Response事件，输出call返回的结果success和data
event Response(bool success, bytes data);
```

#### 2. 调用setX函数

我们定义 `callSetX` 函数来调用目标合约的 `setX()` ，转入 `msg.value` 数额的 ETH ，并释放 `Response` 事件输出 `success` 和 `data` ：

```
function callSetX(address payable _addr, uint256 x) public payable {
    // call setX()，同时可以发送ETH
    (bool success, bytes memory data) = _addr.call{value: msg.value}(
        abi.encodeWithSignature("setX(uint256)", x)
    );

    emit Response(success, data); //释放事件
}
```

接下来我们调用 `callSetX` 把状态变量 `_x` 改为5，参数为 `OtherContract` 地址和 5 ，由于目标函数 `setX()` 没有返回值，因此 `Response` 事件输出的 `data` 为 `0x` ，也就是空。

![](img/cce60a9ed2e040a53702314aa5d6cfbf.png)

#### 3. 调用getX函数

下面我们调用 `getX()` 函数，它将返回目标合约 `_x` 的值，类型为 `uint256` 。我们可以利用 `abi.decode` 来解码 `call` 的返回值 `data` ，并读出数值。

```
function callGetX(address _addr) external returns(uint256){
    // call getX()
    (bool success, bytes memory data) = _addr.call(
        abi.encodeWithSignature("getX()")
    );

    emit Response(success, data); //释放事件
    return abi.decode(data, (uint256));
}
```

从 `Response` 事件的输出，我们可以看到 `data` 为 0x0000000000000000000000000000000000000000000000000000000000000005 。而经过 `abi.decode` ，最终返回值为 5 。

![](img/9ddcdc85d822cfa17c5ad397e118ad2b.png)

#### 4. 调用不存在的函数

如果我们给 `call` 输入的函数不存在于目标合约，那么目标合约的 `fallback` 函数会被触发。
