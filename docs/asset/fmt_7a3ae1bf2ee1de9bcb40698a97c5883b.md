
# Loop Tiling

> 来源：[https://learning.feishu.cn/docx/DmUWdcUJGoualzxWlMqcONrknhh](https://learning.feishu.cn/docx/DmUWdcUJGoualzxWlMqcONrknhh)

## 简要介绍

也叫 `cache blocking`, `loop blocking`

针对 `nested loop` 的优化技术。

核心思想：通过改变 `loop` 内的执行顺序，提高 `cache` 命中率。

1. divide the iteration space of a nested loop into smaller rectangular blocks, or `"tiles"`.
1. These tiles are then processed in a specific order, rather than executing the entire loop nest in its original order.

The main benefits of loop tiling are:

1. Improved cache utilization: By processing a smaller set of data at a time (the tiles), the program is more likely to keep the required data in the cache, reducing the number of cache misses and improving overall performance.
1. Reduced memory access latency: When working with large data sets that do not fit entirely in the cache, tiling can help reduce the time spent accessing memory by keeping the working set smaller and more manageable.
1. Better CPU utilization: Tiling can also help improve the CPU's ability to execute instructions in parallel, as the smaller working set can better fit the CPU's registers and functional units.

The process of loop tiling generally involves the following steps:

1. Identifying the loop nest: The first step is to identify the nested loops in the program that would benefit from tiling.
1. Determining the tile size: The size of the tiles is a crucial parameter that affects the performance of the tiled loop. The tile size should be chosen based on the characteristics of the target hardware, such as the cache size and memory bandwidth.
1. Transforming the loop nest: The original loop nest is then transformed to process the tiles in the desired order, typically by adding additional loop levels to iterate over the tiles.
1. Optimizing the tile size: The tile size may need to be tuned iteratively to find the optimal balance between reducing cache misses and maintaining a manageable working set.

## 理论基础

### 背景

循环优化的 2 个目标：

1. locality
1. parallelization

loop tiling 可以提高 locality。

NPU 一般不存在 `L0` / `L1` 等 cache，只有片上的 buffer，访存的原理也和 CPU cache 不一样。

传统的 compiler 解不掉这里的 tiling 问题，AI compiler 需要设计新的算法和优化 pattern。

### Tiling 分类

根据级别不同可以分为

+ `L2 tiling` - 多核
+ `L1 tiling` - 单核

多核的 tiling 行为。比如，模型并行，数据并行。

-- 这一般也不靠 tiling 算法解决。

`L2 tiling` 的结果：部署在单个 core 上的数据量大小和位置；预加载的信息描述等。

单核单算子的 tiling 行为。将数据尽量缓存在 core 内 buffer 上。

可以只考虑搬运数据量的因素，忽略 `L2 Tiling` 的影响。

`L1 tiling` 的结果：单次加载的数据量，加载次序，`L1` 上的内存分配策略等。

### algorithm of auto-tiling

研究 tiling 的最终目标：开发一个算法，自动判定给定 nested loop 的最佳 tiling 策略 & tiling 收益。

tiling 的收益，可计算 `cache miss` 来评估。

寻找最佳 tiling 策略，很复杂，有 2 个原因：

1. loop 类型很丰富。有的不好 tiling。
1. Tiling 策略与 hardware Architecture 有关。

二者排列组合以后，搜索空间巨大。

复杂 loop 的典型例子：

+ nested loop
+ 不同循环层的 bound 之间有依赖关系
+ 循环之间有数据依赖

数据依赖是定义，如果两次访问访问的是同一个地址，而且至少有一个是写入，则存在依赖.

分 3 种情况:

1. `read-after-write`（`true dependence`）
1. `write-after-read`（`anti dependence`）
1. `write-after-write`（`output dependence`）

`Cost Model` 是解决 auto-tiling 的研究方向，参考思路：

1. 输入 tiling 策略 & hardware 参数。
1. 根据 Optimization 相关理论求解 or 搜索，输出最优的切分策略。

### 矩阵乘的 L1 Tiling

以矩阵乘法为例介绍 `L1 tiling`, `Left`: `[M,K]`, `Right`: `[K, N]` , `Out`: `[M,N]` , `L1` 大小为：`L1_SIZE` ;基本架构为

简单可以分为以下几种情形：

`Left + Right < L1_SIZE`

该情形不涉及 tiling

`Left + Right_K < L1_SIZE`

`Right` 对 `N` 边进行切分，此时遍历顺序将 `N` 边放在最外层，保证 `Left` 和 `Right` 均加载一次。这里暂没有考虑搬运和计算并行性，没有引入乒乓或者默认硬件支持细粒度同步。

`Left_K + Right < L1_SIZE`

`Left` 对 `M` 边进行切分，此时遍历顺序将 `M` 边放在最外层，保证 `Left` 和 `Right` 均加载一次。

`Left_K + Right_k < L1_SIZE`

`Left` 对 `M` 边进行切分，`Right` 对 `K` 和 `N` 进行切分，此时遍历顺序将 `M` 边放在最外层，保证 `Left` 加载一次。

`Left_k + Right_K < L1_SIZE`

`Left` 对 `K` 和 `M` 边进行切分，`Right` 对 `N` 进行切分，此时遍历顺序将 `N` 边放在最外层，保证 `Left` 加载一次。

`Left_k + Right_k < L1_SIZE`

`Left` 对 `K` 和 `M` 边进行切分，`Right` 对 `K` 和 `N` 进行切分，此时遍历顺序将 `K` 边放在最内层。

## 基础 Case 分析

### case - hello world

使用 Fortran 语法 (column major)

```
DO I = 1, N
  DO J = 1, M
    A(I) = A(I) + B(J)  
  END DO
END DO
```

计算 `cache miss`

假设

1. 每个 `cache line` 能容纳 `b` 个数组元素
1. `associativity` 是 `fully associative`

则

1. `A` 的 `cache miss` 次数是 `N / b`
1. `B` 的 `cache miss` 次数是 `N * M / b`

因此，给定 `M` 和 `N`，内层 `M` 小，则总的 `cache miss` 更低。

loop tiling 的基本思路：

loop re-order，使得一个 `cache line` 在被 `evict` 之前被再次使用。

tiling 一般从最内层循环开始。tile `B`，得到

```
DO J = 1, M, T
  DO I = 1, N
    DO jj = J, min(J+T-1, M)
      A(I) = A(I) + B(jj)
    END DO
  END DO
END DO
```

Tile `J` 之后，`J` 在最外层。

所以，loop tiling 又叫 `strip-mine-and-interchange`。

同样的道理，tile `A`，得到

```
DO I = 1, N, T
  DO J = 1, M
    DO ii = I, min(I+T-1, N)
      A(ii) = A(ii) + B(J)
    END DO
  END DO
END DO
```

`cache miss` 次数

|  | `A` | `B` | 总 | Notes |

| --- | --- | --- | --- | --- |

| tile 前 | `N/b` | `N * M/b` | `N/b + MN/b` |  |

| tile `B` | `(N/b) * (M/T)` | `T/b * M/T = M/b` | `M/b + MN/(bT)` | 1. 每一轮 `J` 循环，`B` miss 1 组 |

| tile `A` | `T/b * N/T` | `M/b * (N/T)` | `N/b + MN/(bT)` |  |

结论：

1. `M` 和 `N` 相当，且远大于 `T` 时，tile 使 `cache miss` 缩小 `T` 倍。
1. 先 Tile `A(N)` or `B(M)`，主要取决于 `M` 和 `N` 谁更小。

### case - 二维数据

`B` 换成二维数组，`A` 换成 `D`。

```
DO J = 1, M
  DO I = 1, N
    D(I) = D(I) + B(I, J)
  END DO
END DO
```

fortran 的 2 维数组是 `column major` layout。

`cache miss` 是 `M*N/b` + `M*N/b`（`D` 和 `B` 的 miss 都是 `M*N/b`）

tile 内层循环 `I`

```
DO I = 1, N, T
  DO J = 1, M
    DO ii = I, min(I+T-1, N)
      D(ii) = D(ii) + B(ii, J)
    END DO
  END DO
END DO
```

tile 外层循环 `J`

```
DO J = 1, M, T
  DO I = 1, N
    DO jj = J, min(J+T-1, M)
      D(I) = D(I) + B(I, jj)
    END DO
  END DO
END DO
```

`cache miss` 次数

|  | `D` | `B` | 总 | Notes |

| --- | --- | --- | --- | --- |

| tile 前 | `M*N/b` | `M*N/b` | `M*N/b + M*N/b` |  |

| tile 内层 `I` | `N/T * T/b` | `N/T * M * T/b` | `N/b + NM/b` | `J` 层循环每一轮一开始都会 miss `B` |

| tile 外层 `J` | `N/b * M/T` | `M/T * N/b * T` | `NM/(bT) + MN/b` | `jj` 层，`D` 和 `B` 的 `cache` 都 miss |

结论：

1. 由于 `M` 一般远大于 `T`，所以 tile 外层 `J` 一般总是效果更差

### case - 矩阵转置

```
DO J = 1, N
  DO I = 1, N
    A(I, J) = B(J, I)
  ENDDO
ENDDO
```

交换 `I`，`J` 之后

```
DO I = 1, N
  DO J = 1, N
    A(I, J) = B(J, I)
  ENDDO
ENDDO
```

无论是否做交换 `I`、`J` 循环，`cache miss` 都是 `N*N/b` + `N*N`。

`A` 和 `B`，总有一个不满足 locality。

利用 space locality 的 tiling 策略

```
DO I = 1, N, T
  DO J = 1, N, T
    DO ii = I, min(I+T-1, N)
      DO jj = J, min(J+T-1, N)
        A(ii, jj) = B(jj, ii)
      ENDDO
    ENDDO
  ENDDO
ENDDO
```

各层 `cache miss` 的 breakdown（外层的 `cache miss` 包含了内层的）

| loop/cache misses | `A` | `B` |

| --- | --- | --- |

| `jj` | `T` | `T/b` |

| `ii` | `T * T/b` | `T/b * T` |

| `J` | `T * T/b * N/T = NT/b` | `T/b * T * N/T = NT/b` |

| `I` | `NT/b * N/T = N*N/b` | `NT/b * N/T = N*N/b` |

## 复杂 Case 分析

### 数据依赖 - 不可直接 tiling

（注，fortran 是 column major）

```
DO J = 1, M
  DO I = 1, N
    A(I, J+1) = A(I+1, J) + B(I)
  ENDDO
ENDDO
```

`A(I+1, J)` 读入的值，是 `A(I, J+1)` 写入的。

即

+ `A(2, 1)` 读入 `A(1, 2)` 的写入
+ `A(2, 2)` 读入 `A(1, 3)` 的写入

即，任意时刻，需要先算 `A(I, J+1)`，再算 `A(I+1, J)`。

`I=1` 时，先做 `A(1, 2...N)` 再做 `A(2, 1...N)`

`I=2` 时，先做 `A(2, 2...N)` 再做 `A(3, 1...N)`

## Reference

1. 循环优化之循环分块 (loop tiling) https://zhuanlan.zhihu.com/p/292539074
1. 循环优化之循环分块 (loop tiling) 二 https://zhuanlan.zhihu.com/p/301905385
