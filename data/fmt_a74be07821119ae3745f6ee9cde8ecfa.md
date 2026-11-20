
接下来让我们一起来研究一下`MultiCall`合约，它由 MakerDAO 的 [`MultiCall`](https://github.com/mds1/multicall/blob/main/src/Multicall3.sol) 简化而成。

`MultiCall` 合约定义了两个结构体：

+ `Call`: 这是一个调用结构体，包含要调用的目标合约 `target`，指示是否允许调用失败的标记 `allowFailure`，和要调用的字节码 `callData`。

+ `Result`: 这是一个结果结构体，包含了指示调用是否成功的标记 `success`和调用返回的字节码 `returnData`。

该合约只包含了一个函数，用于执行多重调用：

+ `multicall()`: 这个函数的参数是一个由`Call`结构体组成的数组，这样做可以确保传入的`target`和`callData`的长度一致。函数通过一个循环来执行多个调用，并在调用失败时回滚交易。

```
 // SPDX-License-Identifier: MIT
 pragma solidity ^0.8.19;

 contract Multicall {
     // Call结构体，包含目标合约target，是否允许调用失败allowFailure，和call data
     struct Call {
         address target;
         bool allowFailure;
         bytes callData;
     }

     // Result结构体，包含调用是否成功和return data
     struct Result {
         bool success;
         bytes returnData;
     }

     /// @notice 将多个调用（支持不同合约/不同方法/不同参数）合并到一次调用
     /// @param calls Call结构体组成的数组
     /// @return returnData Result结构体组成的数组
     function multicall(Call[] calldata calls) public returns (Result[] memory returnData) {
         uint256 length = calls.length;
         returnData = new Result[](length);
         Call calldata calli;
         
         // 在循环中依次调用
         for (uint256 i = 0; i < length; i++) {
             Result memory result = returnData[i];
             calli = calls[i];
             (result.success, result.returnData) = calli.target.call(calli.callData);
             // 如果 calli.allowFailure 和 result.success 均为 false，则 revert
             if (!(calli.allowFailure || result.success)){
                 revert("Multicall: call failed");
             }
         }
     }
 }
```

# 26_Swap

> https://github.com/AmazingAng/WTF-Solidity/tree/main/56_Swap

![](img/732a2b4ff1feb42d9a895f78d019062b.png)

![](img/293443c7dda6155e82048e88150c0059.png)

![](img/eaec9b2cf70a39e7bda32210ea637e88.png)

![](img/1682c4b5ac2ab1cb5f7bd7685533161a.png)

![](img/068329159d5763677bb83484242e1df0.png)

[Swap.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/QLM0bxgnboKj8Sxchu7c1e4OnOg)
