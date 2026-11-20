
# Solidity

> 来源：[https://docs.feishu.cn/docx/doxcnabqnggMHB4pIdssAyWkAvg](https://docs.feishu.cn/docx/doxcnabqnggMHB4pIdssAyWkAvg)

## 0 前言

[英文文档](https://docs.soliditylang.org/en/latest/index.html)

[中文文档](https://learnblockchain.cn/docs/solidity/index.html)

Solidity by Example

[他人做的笔记](https://learnblockchain.cn/article/3512)

## 1 Solidity基础

### 1.1 代码结构

下面是铸造，生成代币的代码。

```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity ^0.8.4;

contract Coin {
    // 关键字“public”让这些变量可以从外部读取
    address public minter;
    mapping (address => uint) public balances;

    // 轻客户端可以通过事件针对变化作出高效的反应
    event Sent(address from, address to, uint amount);

    // 这是构造函数，只有当合约创建时运行
    constructor() {
        minter = msg.sender;
    }

    function mint(address receiver, uint amount) public {
        require(msg.sender == minter);
        balances[receiver] += amount;
    }

    // Errors allow you to provide information about
    // why an operation failed. They are returned
    // to the caller of the function.
    error InsufficientBalance(uint requested, uint available);

    function send(address receiver, uint amount) public {
        if (amount > balances[msg.sender])
            revert InsufficientBalance({
                requested: amount,
                available: balances[msg.sender]
            });

        balances[msg.sender] -= amount;
        balances[receiver] += amount;
        emit Sent(msg.sender, receiver, amount);
    }
}
```

> `mapping`不能获得映射的所有键的列表，也不能获得所有值的列表。

> 需要记住添加到`mapping`中的数据，要么在不需要键列表或值列表的上下文中使用它

> `event Sent(address from, address to, uint amount)` 这行声明了一个所谓的“事件（event）”，它会在 `send` 函数的最后一行被发出。用户界面（当然也包括服务器应用程序）可以监听区块链上正在发送的事件，而不会花费太多成本。一旦它被发出，监听该事件的`listener`都将收到通知。而所有的事件都包含了 `from`、`to` 和 `amount` 三个参数，可方便追踪交易。 为了监听这个事件，你可以使用如下JavaScript代码（假设 `Coin` 是已经通过 `web3.js` 创建好的合约对象 ）：

```javascript
Coin.Sent().watch({}, '', function(error, result) {
    if (!error) {
        console.log("Coin transfer: " + result.args.amount +
            " coins were sent from " + result.args.from +
            " to " + result.args.to + ".");
        console.log("Balances now:\n" +
            "Sender: " + Coin.balances.call(result.args.from) +
            "Receiver: " + Coin.balances.call(result.args.to));
    }
})
```

> 这里请注意自动生成的 `balances` 函数是如何从用户界面调用的。

> 在 `Coin` 合约中，构造函数永久存储创建[合约的人的地址](https://learnblockchain.cn/docs/solidity/units-and-global-variables.html#special-variables-functions): `msg` (类似的还有 `tx` 和 `block` ) 是一个特殊的全局变量， 参考 特殊变量和函数 ，这些变量允许我们访问区块链的属性。 `msg.sender` 始终记录当前（外部）函数调用是来自于哪一个地址。

> [`Errors`](https://learnblockchain.cn/docs/solidity/contracts.html#errors)[` 用来向调用者描述`](https://learnblockchain.cn/docs/solidity/control-structures.html#revert-statement)错误信息。`Error`与 `revert` 语句一起使用。 `revert` 语句无条件地中止执行并回退所有的变化，类似于 `require` 函数，它也同样允许你提供一个错误的名称和额外的数据，这些额外数据将提供给调用者(并最终提供给前端应用程序或区块资源管理器），这样就可以更容易地调试或应对失败。

### 1.2 版本标识

`pragma` 版本标识，是pragmatic information的简称，用于启动编译器检查，避免因为`solidity`更新后造成的不兼容和语法变动的错误。只对本文件有效，如果导入其他文件，版本标识不会被导入，而是采用工作的文件自身的版本标识

```solidity
pragma solidity ^0.5.2;
```

这里`^`表示从0.5.2到0.6（不含）的版本

### 1.3 导入其他文件

`import "filename";` 这种导入方式会把导入文件的所有全局符号都导入到工作文件的全局作用域，会污染命名空间，不建议这么使用。

```solidity
import * as symbolName from "filename";
// 等价于
import "filename" as symbolName;
```

这样所有的全局符号都以`symbolName.symbol`的格式提供。

我们还可以设置别名，别名和重定义的符号名，都可以表示导入的文件里的全局符号。

```solidity
import { symbol1 as alias, symbol2 } from "filename";
```

支持从网络中导入，如：`import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/release-v3.3/contracts/cryptography/ECDSA.sol";`

### 1.4 路径

路径的形式和Linux下的完全一致，但是要避免使用`..`。我们可以引入指定路径的文件，如`import "./filename" as simbolName`，是当前目录下的文件。引用的文件除了本地文件，也可以是网络资源。

实际`solc`编译器使用的时候可以指定路径的重映射，编译器可以从重映射的位置读取文件。尤其是使用网络文件的时候 例如，可以使`github.com/ethereum/dapp-bin/library` 会被重映射到 `/usr/local/dapp-bin/library` ，格式如下。

```bash
solc github.com/ethereum/dapp-bin/=/usr/local/dapp-bin/ source.sol
```

更具体地会在solc编译器地部分说明。而truffle框架和remix就相对智能，可以通过网络获取文件。

### 1.5 注释

单行注释`//`,多行注释`/*......*/`

一种natspec注释，他是用`///`或者`/** ... */`，它里面可以使用Doxygen样式来给出相关地信息。

Doxygen样式地注释可以使特殊地注释形式变得可识别，方便读取和自动提取信息。主要有

```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.4.21 <0.9.0;

/**
 * @title Shape calculator.
 * @file 文件名
 * @author John Doe <jdoe@example.com>
 * @version 1.0
 * @details 细节
 * @date 年-月-日
 * @license 版权协议
 * @brief 类的简单概述
 * @section LICENSE 这一段的主要内容
 * @param Description of method's or function's input parameter 形式参数说明
 * @return Description of the return value 返回说明
 * @retval 返回值说明
 * @attention 注意
 * @warning 警告
 * @var 变量声明
 * @bug 代码缺陷
 * @exception 异常
 */
contract ShapeCalculator {
    /// @dev Calculates a rectangle's surface and perimeter.
    /// @param w Width of the rectangle.
    /// @param h Height of the rectangle.
    /// @return s The calculated surface.
    /// @return p The calculated perimeter.
    function rectangle(uint w, uint h) public pure returns (uint s, uint p) {
        s = w * h;
        p = 2 * (w + h);
    }
}
```

特别地，可以使用 `pragma abicoder v1` 或者 `pragma abicoder v1` 指定 ABI 的编码器和解码器版本，一般而言 0.8.0 以后，默认使用 v2 版本。

### 1.6 全局变量

状态变量是永久地存储在合约存储中的值，它具有数据的类型，也有可见性的属性。在函数外的都是`storage`全局变量。

```solidity
pragma solidity >=0.4.0 <0.9.0;

contract TinyStorage {
    uint storedXlbData; // 状态变量
    // ...
}
```

### 1.7 函数

函数是代码的可执行单元。函数通常在合约内部定义，但也可以在合约外定义。

```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity >0.7.0 <0.9.0;

contract TinyAuction {
    function Mybid() public payable { // 定义函数
    }
}

/**
 * Helper function defined outside of a contract
 */
function helper(uint x) pure returns (uint) {
    return x * 2;
}
```

函数调用可发生在合约内部或外部，且函数有严格的可见性[限制，对于谁可以调用它有着明确的规定（可见性和 `getter` 函数）]。

函数的返回值可以是元组，接收时需要一一对应。

### 1.8 函数修饰

函数修饰符用来修饰函数，比如添加函数执行前必须的先决条件。这样可以方便地实现代码复用。

```solidity
contract Owner {
    modifier onlyOwner {
        require(msg.sender == owner);
        _;
    }
    modifier costs(uint price) {
        if (msg.value >= price) {
            _;
        }
    }
}
```

函数体会插入在修饰函数的下划线`_`的位置。所以只有当修饰条件满足之后才能执行这个函数，否则报错。

> 下面参考 [solidity函数修饰符](https://blog.csdn.net/qq_35784487/article/details/123324276)

#### 1.8.1 可见性修饰符

> 决定函数何时和被谁调用。

`private` ：只能被合约内部调用；

`internal` ：只能被合约内部调用或者被继承的合约调用；

`public` ：可以在任何地方调用，不管是内部还是外部；

`external`：只能从合约外部调用；

#### 1.8.2 状态修饰符

> 用来表示函数如何和区块链交互。

`view` ：运行这个函数不会更改和保存任何数据；

`pure` ：运行这个函数不仅不会往区块链写数据，甚至不从区块链读取数据；

这两种在被合约外部调用的时候都不花费任何`gas`，但是它们在被内部其他函数调用的时候将会耗费`gas`。

#### 1.8.3 自定义modifiers

如`onlyowner`、`aboveLevel`等自定义的`modifiers`，对于这些修饰符我们可以自定义其对函数的约束逻辑。

```solidity
/**
 * @dev Throws if called by any account other than the owner.
 */
modifier onlyOwner() {
    require(msg.sender == owner);
    _;
}
```

这些修饰符可以同时作用于一个函数定义上，如：

```solidity
function test() external view onlyOwner anotherModifier { /* ... */ }
```

#### 1.8.4 payable修饰符

可以接收以太的特殊函数

```solidity
contract OnlineStore {
  function buySomething() external payable {
    require(msg.value == 0.001 ether);
    transferThing(msg.sender);
  }
}
```

`msg.value` 是一种可以查看向合约发送了多少以太的方法，另外 `ether` 是一个內建单元。

> 注意： 如果一个函数没标记为`payable`， 而你尝试利用上面的方法发送以太，函数将拒绝你的事务。

注意下面的用法。实际上常常会被继承，作为模块复用。

使用的格式

`function funcName(params) 可见性修饰 函数属性修饰 函数修饰器 returns(params)`

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8;

contract Test {
    uint public a;
    uint public b;
    function set(uint _a, uint _b) public {
        a = _a;
        b = _b;
    }
    modifier Func(uint _a) {
        require(a > _a, "error:a is so small.");
        _;
    }
    function f(uint _a) public view Func(_a) returns(uint) {
        return _a;
    }
}
```
