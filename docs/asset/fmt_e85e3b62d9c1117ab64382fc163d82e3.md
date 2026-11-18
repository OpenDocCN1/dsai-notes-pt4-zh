
# TVM 仓库概况

> 来源：[https://learning.feishu.cn/docx/ETkBd6OLkofMyAxKQEKcDEuknpg](https://learning.feishu.cn/docx/ETkBd6OLkofMyAxKQEKcDEuknpg)

## 总结

TVM 代码分布：

1. TVM 最大的特点是 python 和 c++ 混合开发。一共 50 万行代码，2 个语言各 20 万左右。
2. 重要的子模块比较多，选了 9 个，分成 3 大类。
3. 基础模块：`relay`, `TIR`, `runtime`
4. 自动调度类：`meta_schedule`, `auto_scheduler`, `autotvm`
5. DSL 类（手写算子和模型）：`te`, `script`, `topi`
6. 模块代码分布的整体情况：
7. `Relay` 的代码最多，约 14 万行。c++ 处理 `relay` 后端，python 对接多种 dl 框架和 `op`。
8. 除了 `relay` 之外，c++ 代码主要是 `tir` 和 `runtime` 相关。需要高性能的。
9. 自动调度类的代码，c++ 和 python 各约 2 万行。其中，`autotvm` 5000 行，基本全是 python。
10. TVM 的精华，是自动调度的 4 万多行代码。包括：`meta_schedule`, `auto_scheduler`, `autotvm`。

## TODO

## 仓库概况

### 总体代码行数

忽略 `3rdparty`, `tests`, `build`, `docs`, `web` 目录，一共 50 万行代码。

需要分析的，主要是 `src` 和 `python` 2 个目录。

与 mlir 最大的区别是，大量的 python 代码。python 和 c++ 的代码量差不多。

- Python 17 万行
- c++: 21 万行
- C/C++ Header: 6 万。
- 小计: 42 万

```
cloc --exclude-dir=3rdparty,tests,build,docs,web .
```

![](img/9af0d76e712d401b0152d629130ab859.png)

### tvm/src 代码分析

一共 24 万行。

#### 整体情况

目录就反映了重要的模块，而且重要的模型，确实有点多。

`Relay`, `tir`, `runtime`, `target`

`meta_schedule`, `auto_scheduler`, `autotvm`

`te`, `script`, `topi`

`arith`, `driver`, `ir`

信息量：

1. `autotvm` 只有 558 行代码，怎么做到的？——都在 python 里。
2. `Relay` 代码量最多，7 万行。占 30%
3. `Tir` 其次，5 万行。
4. `Runtime` 4.5 万行。
5. `meta_schedule` 和 `auto_scheduler`，各 1 万行代码。并不多。

![](img/fae774c4a7121886e38ebe2d006d6015.png)

#### Relay

7 万行代码，主要是 `backend`, `transforms` 和 `op`。

特别的，还有 `quantize` 和 `qnn`

![](img/0e8797d0bbf566302f588b8ba7a24b44.png)

没看懂 `backend` 是在做什么。

![](img/479cbf31c959e46f5aa9b3556cb46796.png)

`backend/contrib` 下面是这些

![](img/3cc51869757c9588d0563b87262c42e0.png)

`transform`，就是 `relay` 上的 `pass`。每个 `pass` 最多 900 行代码，大多一二百行。

![](img/a01a7d2598cdae3525fd8061189dfc0d.png)

`Op` 实现了一小部分 `op`

比如，`nn` 类算子

![](img/fcedcd0ebea3d6ab27b7355c4b861f9e.png)

`Tensor` 类算子

![](img/f81dab00c15769166472f8dd9c840ced.png)

#### TIR

一共 5 万行代码，主要就是 `transforms` 和 `schedule`

在 `relay` 里，没有 `schedule`。

![](img/1a25a92b9ab2978b3ee456784c3829f2.png)

![](img/6c14951f394900729a2b178b520ea01b.png)

`tir/schedule`

![](img/3e09a4aa5ca69d4fdd4706895589ca1d.png)

#### runtime

一共 4.5 万行代码。支持多种第三方框架，大部分是在胶水第三方。

比如：

- Hexagon: Qualcomm 的 mobile devices 硬件加速器
- crt：C Run-Time library
- opencl:
- Vulkan: GPU 上的 3D graphics and compute API
- Metal: 可能与苹果硬件有关，low-level API provided by Apple for their GPUs
- cuda
- rocm

另外，还有 `vm` 和 `graph_executor`，可能是笔记重要的基础 API。

![](img/15698ba60182e0093072ac8396aa4df4.png)

### tvm/python/tvm 代码分析

一共 15 万行。跟 c++ 一样，子目录就是重要的模块，而且，模块也就是那些模块。

`relay` + `topi` 就 10 万行。占了大头。都是在支持不同的 dl 框架、算子。体力活代码。

特别注意的是，自动调度的代码量，不低。

`meta_schedule` 有 8000 行，`autotvm` 有 5000 行。`auto_scheduler` 有 3680 行。

![](img/37c168339725ed48fe9389f92f49fd5e.png)

#### 自动调度类

（无他，为了本文的目录对称，能看到 python 核心代码）

### TVM unity 分支

以上，分析的是 `main` 分支。活跃开发中的 `relax`，在 `unity` 分支，没有合入 `main`。

代码量变化

| | `main` 分支 | `unity` 分支 |
| --- | --- | --- |
| `tvm/src` | 24 万行 | 27 万行 |
| `tvm/python` | 15 万行 | 17 万行 |

结论：

1. 新增了 5 万行代码。3 万行 c++，2 万行 python。
2. 新增的代码主要是 `relax` 目录。python 下 1 万多。c++ 下 2.2 万多行。

c++ 的 `relax`，主要是 `transform`, `op` 和 `ir`。跟 `relay` 差不多的。

最大的区别是，`relay` 下 `backend` 代码量也不少。但，`relax` 下只有不到 3000 行。

![](img/56cb917643700f0c2aa281a3c2673128.png)

c++ 的 `relax/transform`

![](img/61b2251c4e86ba3ea78984fcfde23737.png)

Python 的 `relax`，代码分布如下

1. `Frontend` 下代码很少。只支持了 pytorch。

![](img/1366f143ada4dbf59693b1a9cfb3c60d.png)

`Transform` 的代码，也略显独特了一些

![](img/027a3f0f6716b1f36057c915660aa751.png)
