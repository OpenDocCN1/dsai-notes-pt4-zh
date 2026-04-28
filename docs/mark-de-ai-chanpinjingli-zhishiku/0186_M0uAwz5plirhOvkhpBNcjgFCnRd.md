# 06-03优化算法-BFGS

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/XzLIddq7Ho8A1DxSTTEcRovCnye](https://qqs7y1hozd1.feishu.cn/docx/XzLIddq7Ho8A1DxSTTEcRovCnye)

在机器学习和数值优化中，我们常常要最小化（或最大化）一个函数，比如在训练模型时要找到损失函数的最低点。

BFGS（Broyden–Fletcher–Goldfarb–Shanno）算法，就是一种“聪明地”往最低点走的迭代方法。

**为啥不能直接走直线？**

当你在一座山上找最低的谷底，最直观的做法是：

1.  站在当前位置，往坡度最陡下降的方向走（梯度下降法）。

1.  重复这个过程，直到感觉再也找不到更低的地方。

但是：

+   如果山谷很弯，单纯“负梯度”方向每次都得重算，走得很慢；

+   如果直线下降一次就跨过了最深处，还要再“折回来”，浪费很多步。

那么，有没有办法让我们“事先估计”一下地形弯曲程度（即二阶信息，Hessian矩阵），然后一次就走更合适的步子？直接计算 Hessian （二**阶导**）往往计算量巨大，尤其维度大时不现实。BFGS 就是“准牛顿”（Quasi‑Newton）方法里很有名的一员：它不用真 Hessian，却能在线上迭代中逐步构造一个“近似 Hessian 逆矩阵”，帮你更快、更稳地走向谷底。

### BFGS 的核心思路

1.  **初始猜一个单位矩阵** 作为 Hessian 逆（也就是把地形当成平地看待）。

1.  在第   次迭代时，你已有一点点对“地形弯度”的了解，表现为一个对称正定矩阵  （近似 Hessian 的逆）。

1.  计算当前位置的梯度  ，然后根据   给出下一步的搜索方向：
 
这一步等于告诉你：既考虑了“斜度”（梯度），也考虑了“弯度”（ ），通常能一步更准地往下走。

1.  用线搜索（line search）在方向   上找到一个合适步长  ，更新位置：

1.  更新 Hessian 逆的近似矩阵：

+   定义位置差  ；

+   梯度差  。

+   然后 BFGS 的更新公式是：
 
虽然看上去不简单，但本质是在用新得到的“走过的位移”   和“感觉到的斜度变化”   来校正对地形弯曲的认知。

1.  重复 2–5，直到梯度很小（表示快到谷底了）或迭代次数达限。

和过去的梯度下降类比：

+   **梯度下降** 就像：你戴上眼罩，用手电筒只能看一步的坡度，然后按坡度走；

+   **BFGS** 就像：除了手电筒，你还带了一个“简易地形图”，这个地形图会随着你走过的路不断“修正”，帮你判断下一步往哪边走更快地到谷底。

## 原理详解

### 牛顿法与拟牛顿方法

1. 牛顿法（Newton’s Method）

对无约束可微函数   寻找极小点，牛顿法每次迭代更新：

其中   是梯度，  是 Hessian 矩阵（即二阶导矩阵）。优点是若   足够光滑且初始点靠近极小点，收敛二阶超线性；缺点是

+   需要显式计算 Hessian，开销   空间、  时间；

+   Hessian 可能不正定，导致更新方向不一定为下降方向。

2. 拟牛顿（Quasi‑Newton）思想

目标：既想利用二阶信息（更快收敛），又不直接求 Hessian。

**核心：在每次迭代中，构造或**更新一个近似 Hessian 矩阵  （或者其逆  ），使它满足最少量信息（即“secant 条件”）并保持对称正定。

### Secant 条件与BFGS更新的出发点

1. Secant 条件（拟牛顿的必要条件）

记位移  ，梯度差  。理想** H**essian 满足

拟牛顿方法强制要求近似 Hessian   恰好满足

这就称为 secant 条件。

**2. 最小变化原理**

在满足 secant 条件和对称正定的前提下，选择与上一轮矩阵   “距离”最小的  。这里“距离”通常用加权 Frobenius 范数：

其中  ，权重矩阵   的不同选择给出 Broyden、DFP、BFGS 等不同配方。BFGS 对应特定的  ，导出下文公式。

### BFGS 的矩阵更新公式

我们分别给出直接构造 **Hessian 近似**   和 **Hessian 逆**   的常用形式。

Hessian 近似   的 BFGS 公式：

+   第一项保留旧信息；

+   第二项剔除与   方向上“过度”估计的成分；

+   第三项加入新的 secant 信息。

Hessian 逆   的 BFGS 公式：

在实际中更常用逆矩阵更新，避免矩阵求逆开销：

其中

+    ；

+   保证当  （或  ）为对称正定且   时，更新后仍对称正定。

BFGS 公式推导要点：

**1. 拉格朗日法**：对  s.t.   及  ，构建拉格朗日函数并对   和拉格朗日乘子求导。

**2. 选择权重 ** ：BFGS 对应的权重使得得解形式为上文所示。

**3. 对称正定保持**：关键在于  ，即**弧线搜索**需保证“充分下降”条件（Wolfe 条件）以满足这一不等式。

## 算法流程

```
输入：初始点 x₀，初始逆 Hessian 近似 H₀（通常取 I），容忍度 ε，最大迭代次数 K

for k = 0,1,2,…,K − 1 do
  1. 计算梯度 g_k = ∇f(x_k)
     若 ∥g_k∥ &lt; ε，则停止，返回 x_k 作为近似极小点。
  2. 计算搜索方向 p_k = − H_k · g_k
     （因 H_k 对称正定，p_k 保证为下降方向）
  3. 弧线搜索：沿 p_k 找步长 α_k 满足 Wolfe 条件（或强 Wolfe）：
     - 足够下降： f(x_k + α p_k) ≤ f(x_k) + c₁ α g_k^T p_k
     - 曲率条件：   ∇f(x_k + α p_k)^T p_k ≥ c₂ g_k^T p_k
     （典型取 0 &lt; c₁ &lt; c₂ &lt; 1）
  4. 更新位移与梯度差
     s_k = x_k + α_k p_k − x_k
     y_k = ∇f(x_k + α_k p_k) − g_k
  5. 计算 ρ_k = 1 / (y_k^T s_k)
  6. 更新逆 Hessian 近似：
     H_{k+1}
       = (I − ρ_k s_k y_k^T) H_k (I − ρ_k y_k s_k^T)
         + ρ_k s_k s_k^T
  7. 更新 x_{k+1} = x_k + α_k p_k
end for

返回 x_K
```

## 完整案例

本案例选用经典的 **Rosenbrock 函数**（banana 函数）作为测试目标：

它具有一个位于   的全局最小点，但其狭长弯曲的谷底使得普通梯度下降迭代速度极慢。

示例中，我们：

1.  实现 **BFGS** 算法（拟牛顿），并与**固定步长梯度下降**做对比。

1.  通过**等高线图** 和 **3D 曲面图** 展示优化路径。

1.  通过 **收敛曲线** 比较两者训练效率。

1.  测量并展示两者在相同初始点下的迭代次数与运行时间。

1.  **给出针对**本案例及通用优化场景的 优化建议。

```
import numpy as np
import matplotlib.pyplot as plt
import time
from mpl_toolkits.mplot3d import Axes3D

# 1. 定义 Rosenbrock 函数及其梯度
def rosen(x):
    &quot;&quot;&quot;Rosenbrock 函数&quot;&quot;&quot;
    return 100 * (x[1] - x[0]**2)**2 + (1 - x[0])**2

def grad_rosen(x):
    &quot;&quot;&quot;Rosenbrock 函数的梯度&quot;&quot;&quot;
    dx = -400 * x[0] * (x[1] - x[0]**2) - 2 * (1 - x[0])
    dy = 200 * (x[1] - x[0]**2)
    return np.array([dx, dy])

# 2. 回溯线搜索（Armijo 条件）
def line_search_armijo(f, grad, x, p, alpha0=1.0, rho=0.8, c=1e-4):
    fx = f(x)
    gx = grad(x)
    alpha = alpha0
    # 当不满足 Armijo 条件时，缩小步长
    while f(x + alpha * p) &gt; fx + c * alpha * np.dot(gx, p):
        alpha *= rho
    return alpha

# 3. BFGS 算法实现
def bfgs(f, grad, x0, max_iter=1000, tol=1e-6):
    x = x0.copy()
    n = len(x0)
    H = np.eye(n)  # 初始 Hessian 逆近似
    path = [x.copy()]
    fvals = [f(x)]
    
    for k in range(max_iter):
        gk = grad(x)
        if np.linalg.norm(gk) &lt; tol:
            break
        # 搜索方向
        pk = -H.dot(gk)
        # 线搜索确定步长
        alpha = line_search_armijo(f, grad, x, pk)
        sk = alpha * pk
        x_new = x + sk
        yk = grad(x_new) - gk
        rho = 1.0 / np.dot(yk, sk)
        I = np.eye(n)
        # BFGS 更新公式
        H = (I - rho * np.outer(sk, yk)).dot(H).dot(I - rho * np.outer(yk, sk)) + rho * np.outer(sk, sk)
        x = x_new
        
        path.append(x.copy())
        fvals.append(f(x))
    
    return np.array(path), fvals

# 4. 梯度下降法作为对比
def gradient_descent(f, grad, x0, alpha=1e-3, max_iter=50000, tol=1e-6):
    x = x0.copy()
    path = [x.copy()]
    fvals = [f(x)]
    
    for k in range(max_iter):
        gk = grad(x)
        if np.linalg.norm(gk) &lt; tol:
            break
        x = x - alpha * gk
        path.append(x.copy())
        fvals.append(f(x))
    
    return np.array(path), fvals

# 5. 实际运行与性能测量
x0 = np.array([-1.5, 1.5])

start = time.time()
path_bfgs, fvals_bfgs = bfgs(rosen, grad_rosen, x0)
time_bfgs = time.time() - start

start = time.time()
path_gd, fvals_gd = gradient_descent(rosen, grad_rosen, x0)
time_gd = time.time() - start

print(f&quot;BFGS converged in {len(fvals_bfgs)-1} iterations, time: {time_bfgs:.4f}s&quot;)
print(f&quot;GD converged   in {len(fvals_gd)-1} iterations, time: {time_gd:.4f}s&quot;)

# 6. 可视化 1：等高线与优化路径
fig, ax = plt.subplots(figsize=(8, 6))
X = np.linspace(-2, 2, 400)
Y = np.linspace(-1, 3, 400)
XX, YY = np.meshgrid(X, Y)
ZZ = 100 * (YY - XX**2)**2 + (1 - XX)**2

levels = np.logspace(-0.5, 3.5, 20)
contour = ax.contour(XX, YY, ZZ, levels=levels, cmap=&#x27;viridis&#x27;)
ax.plot(path_bfgs[:, 0], path_bfgs[:, 1], marker=&#x27;o&#x27;, color=&#x27;red&#x27;, label=&#x27;BFGS Path&#x27;)
ax.plot(path_gd[:, 0], path_gd[:, 1], marker=&#x27;x&#x27;, color=&#x27;blue&#x27;, label=&#x27;GD Path&#x27;)
ax.legend()
ax.set_title(&#x27;Optimization Path on Rosenbrock Function&#x27;)
ax.set_xlabel(&#x27;x&#x27;)
ax.set_ylabel(&#x27;y&#x27;)
plt.show()

# 7. 可视化 2：收敛曲线
fig, ax = plt.subplots(figsize=(8, 6))
ax.plot(fvals_bfgs, label=&#x27;BFGS&#x27;, color=&#x27;magenta&#x27;, linewidth=2)
ax.plot(fvals_gd, label=&#x27;Gradient Descent&#x27;, color=&#x27;cyan&#x27;, linewidth=2)
ax.set_yscale(&#x27;log&#x27;)
ax.set_title(&#x27;Function Value vs Iteration&#x27;)
ax.set_xlabel(&#x27;Iteration&#x27;)
ax.set_ylabel(&#x27;f(x)&#x27;)
ax.legend()
plt.show()

# 8. 可视化 3：3D 曲面与 BFGS 路径
fig = plt.figure(figsize=(10, 8))
ax3 = fig.add_subplot(111, projection=&#x27;3d&#x27;)
ax3.plot_surface(XX, YY, ZZ, cmap=&#x27;plasma&#x27;, alpha=0.7)
ax3.plot(path_bfgs[:, 0], path_bfgs[:, 1], fvals_bfgs, color=&#x27;red&#x27;, marker=&#x27;o&#x27;, label=&#x27;BFGS&#x27;)
ax3.set_title(&#x27;3D Surface and BFGS Path&#x27;)
ax3.set_xlabel(&#x27;x&#x27;)
ax3.set_ylabel(&#x27;y&#x27;)
ax3.set_zlabel(&#x27;f(x)&#x27;)
plt.show()
```

### 等高线与优化路径 （等高线图）

+   **背景**：等高线展示 Rosenbrock 函数不同函数值所在的曲线

+   **BFGS 路径**（红色圆点）

+   **GD 路径**（蓝色叉号）

![](img/7ca51e355c4c09ad1c5b7d8f7a28f669.png)

+   BFGS 只用 45 步就收敛到  ，路径直接而有力。

+   固定步长梯度下降需要 32385 步，路径在谷底附近反复震荡。

### 收敛曲线 （Function Value vs Iteration）

+   Y 轴对数刻度

+   BFGS（magenta）与 GD（cyan）

![](img/5552ba580c90f7f15853dddf2dabb40a.png)

+   BFGS 在第 1–2 步就迅速下降到极小值附近，展开超线性收敛。

+   GD 缓慢线性下降，迭代数与运行时间都高出数百倍。

### 三维曲面与 BFGS 路径 （3D Surface）

+   **曲面**：plasma 配色，凸显山谷弯曲

+   **路径**：红色折线、圆点

![](img/d3a5ab74dc9ce8ef7e205a54ffdd4e8b.png)

+   直观展示 BFGS 如何“跳跃”穿过高坡，快速抵达谷底

+   凸显拟牛顿利用二阶近似的优势

### 性能对比

| 方法 | 迭代次数 | 运行时间 (s) |
| --- | --- | --- |
| BFGS | 45 | 0.0033 |
| Gradient Descent | 32385 | 0.3164 |

+   **迭代速度**：BFGS ~700× 少

+   **时间开销**：BFGS ~100× 少

### 算法优化与实用建议

1.  **线搜索策略**：**本例使用 A**rmijo 条件，若改用 Wolfe 或强 Wolfe，可进一步提高收敛稳定性。对于高维问题，建议引入精细化线搜索或信赖域策略。

1.  **初始 Hessian 逆近似**：除了单位矩阵，可根据问题先验设定对角预调，改善方向估计。对极度非凸或病态问题，可考虑Levenberg–Marquardt式阻尼。

1.  **内存与规模**：BFGS 需存储   矩阵，适用于中小规模（ ）。对于超高维（ ），请使用 **L‑BFGS**（保留最近   对  ），空间降到  。

1.  **预处理与缩放****：对变量尺****度差异**较大的问题，先做变量归一化，增强 Hessian 逆近似精度。或者在 BFGS 中加入预条件，类似 Preconditioned Gradient.

1.  **混合策略**：前期用梯度下降快速降低到较好区域，再切换到 BFGS 进行精炼超线性收敛。对非凸问题，可在多起点并行运行、汇总最优结果。

1.  **止步准则**：除了梯度范数，可加入函数值变化阈值、相对步长阈值，多重止步条件使算法更鲁棒。

## 模型分析

### BFGS 算法的优缺点

**优点**：

1.  **收敛速度快**：BFGS 利用一阶梯度差分构造二阶信息近似，通常能实现超线性收敛，比单纯梯度下降迭代次数少、时间开销小。

1.  **无需直接计算 Hessian**：完全避免显式计算、存储和求逆真正的 Hessian 矩阵，降低了计算复杂度，同时又保留了牛顿法优良的收敛性质。

1.  **路径稳定、鲁棒性好**：在大多数凸、接近凸的问题中，即使 Hessian 近似存在偏差，也能保证下降方向为“正交下降方向”，不易发散。

1.  **易于实现与调试**：仅需线搜索与矩阵 rank‑2 更新公式，代码结构清晰，参数也相对少（主要是线搜索的参数）。

**缺点**：

1.  **存储与计算开销随维度平方增长**：每次要维护并更新一个   的矩阵，内存和矩阵乘法开销对高维（如  ）问题并不友好。

1.  **对非凸问题敏感**：当目标函数存在多个鞍点或深坑，secant 条件   不易保证，可能导致近似矩阵丢失正定性，甚至“走歪”。

1.  **线搜索依赖性**：需要借助合适的线搜索（Wolfe／Armijo 等）才能保证 secant 条件和收敛性，增加工程实现和调试难度。

1.  **不适合噪声或随机梯度场景**：在随机、嘈杂的梯度估计下，差分构建的   会很不稳定，BFGS 更新难以奏效。

### 与相似算法的对比

| 算法 | Hessian 信息 | 存储成本 | 收敛率 | 适用场景 |
| --- | --- | --- | --- | --- |
| BFGS | 近似二阶（无 Hessian） | 存储 | 超线性（≍牛顿） | 中小规模、光滑凸优化 |
| L‑BFGS | 近似二阶（有限记忆） | ， | 超线性（有限内存） | 高维无约束优化 |
| Newton’s Method | 完整二阶 | 存储 +   求逆 | 二阶收敛 | 低维、Hessian 易算 |
| Gradient Descent | 无二阶 | $ | 线性 | 大规模／随机场景或粗优化 |
| Conjugate Gradient | 无需存 Hessian | $ | 超线性（对称正定） | 大规模线性系统或二次型 |

### 何时优选 BFGS，何时考虑其它算法

1.  **优选 BFGS 的场景**

+   **中小规模（** ** 从几十到几千）** 的无约束连续、光滑、近凸优化问题。

+   对**收敛速度**和**优化精度**有较高要求，且可以承受   的内存消耗。

+   目标函数梯度可准确计算或评估成本较低，不存在严重噪声／随机性。

+   需要相对“开箱即用”的算法实现，又能在多数问题上取得超线性收敛效果。

1.  **应考虑其他算法的场景**

+   **高维极大规模问题**（ ）：建议用 **L‑BFGS** 或 **梯度下降**、**随机梯度** 方法，以降低内存与计算负担。

+   Hessi**an 可解析且维度很小：直接用** Newton’s Method，可获得更快的二阶收敛且代码更简洁。

+   **存在强非凸或鞍点**：可尝试 **信赖域法**（Trust‑Region）或 Levenberg–Marquardt，引入阻尼项更稳定。

+   **随机／嘈杂梯度****（如深**度**学习大规**模**批量训练）：使**用 SGD、Adam、AdaGrad 等自适应一阶方法，更能抵抗噪声。

## 总结

BFGS 在中等维度、光滑凸优化中有着“无需 Hes**sian** **却享受牛**顿法收敛”的优势，适合精细调优和快速收敛。

面对**更高维度**或**噪声梯度**场景，则应权衡存储、计算和收敛性能，选择 L‑BFGS、随机梯度或信赖域等更合适的方法。