# 大厂面试笔试算法精华题

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/ApwAdeiSYoklwgxZH8ach6Dvnhf](https://qqs7y1hozd1.feishu.cn/docx/ApwAdeiSYoklwgxZH8ach6Dvnhf)

# **二叉树经典问题**

## 什么是满二叉树、完全二叉树、完美二叉树、二叉搜索树和平衡二叉树？

下面逐个解释这些概念：

满二叉树（Full Binary Tree）是一种特殊的二叉树结构，其中每个节点要么是叶子节点（没有子节点），要么有两个子节点。 这意味着每一层上的节点都是完全填满的

![](img/1817d5c5a4c115cf74e118c519a258dc.png)

完美二叉树（Perfect Bina**ry Tree）是一种特殊的二叉树结构，其中**每个节点要么是叶子节点（没有子节点），要么有两个子节点，并且左右子树都完全相同。 这意味着每一层上的节点都是完全填满的。和满二叉树最大的不同在于，完美二叉树不是节点粒度上的填满，而是层粒度上的填满

![](img/d0d5f1e6050ec1bb38dfefb2a17881ee.png)

完全二叉树（Complete Bina**ry Tree）是一种特殊的二叉树结**构，其中除了最后一层节点，其他层节点都是满的，并且最后一层节点从左向右依次排布。 这是一种完美二叉树的弱化版本，因为完美二叉树尽管拥有很多很好的性质，但是最后一层的节点数量会指数上升。而完全二叉树是一种可以动态决定最后一层节点数量的二叉树结构，同时保证了完美二叉树的一些良好性质（平衡性），主要用于实现堆数据结构

![](img/2d7e7d531b0a128af243261ea982da38.png)

**二叉搜索树是一种有序的二叉树，其中每个节点的值都满足一定的**排序规则：对于任意一个节点，其左子树上所有节点的值都小于该节点的值，而右子树上所有节点的值都大于该节点的值。 二叉搜索树是一种常见的数据结构，其中每一个子树也是二叉搜索树，利用这个性质可以递归构造二叉搜索树

![](img/c3e5a872891fecfcb1ea5b07ff37764e.png)

平衡二叉树是一种特殊的二叉搜索树，它保持二叉树的高度尽可能小，从而确保**查找**、插入、删除操作的时间复杂度尽量接近 O(log⁡n) 。平衡二叉树需要定义平衡规则，而由于二叉树的操作时间复杂度和[树的高度直接相](https://blog.csdn.net/m0_51156601/article/details/133272699)关，所以平衡二叉树的规则基本都是限制住树的高度。最常见的两种平衡二叉树是 AVL 树和红黑树，在 AVL 树中，任何节点的两个子树的高度差最多为 1，而红黑树更加复杂，想了解更多可以参考深入理解红黑树

## **给定节点总数**** **，完全二叉树可能的最小高度是多少？

高度（从1开始）为 的完美二叉树有 个节点。那么可以得到高度为 的完全二叉树的节点数量 满足  。于是给定节点数量  ，我们试图找到第一个 ，其满足  ，那么就可以得到最小的高度

## **给定节点总数**** ****，这有可能是一棵满二叉树吗？**

满二叉树的节点数量 ，所以只需要判断 是否为偶数即可

## 二叉搜索树的前驱后继怎么找（迭代实现）？

下面给出二叉树的寻找前驱的迭代实现。

```
class TreeNode:
    def __init__(self, val=0, parent=None, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
        self.parent = parent

def find_preprocessor(root, val):
    cur = self.root
    # 找到val的位置
    while cur and cur.val != val:
        if val &lt; cur.val:
            cur = cur.left
        else:
            cur = cur.right
    if not cur:
        return None
    if cur.left:
        # 如果存在左子树，那么前驱就是左子树的最右节点
        cur = cur.left
        while cur.right:
            cur = cur.right
        return cur
    # 否则一直往上走，直到 cur != cur.parent.left
    pa = cur.parent
    while pa and pa.left != cur:
        cur = pa
        pa = cur.parent
    return pa
```

## **二叉搜索树的怎么删除一个节点？**

二叉搜索树的删除操作没有那么简单，总共可以分为以下几种情况：

1.  删除的节点是叶子节点，直接删除即可

1.  删除的节点只有一个子节点，那么直接将子节点替换到删除节点的位置即可

1.  删除的节点有两个子节点，那么需要找到这个节点的右子树中的最小节点，将这个节点的值替换到删除节点的位置，然后删除这个最小节点即可

![](img/f7b57ba18244d2678c8775aaac9771b3.png)

下面是实现的参考代码：

```
def tranplant(root, u, v):
    # 移植子树 v 到 u 的位置，并且调整关系
    if u.parent is None:
        root = v
    elif u == u.parent.left: 
        u.parent.left = v
    else:
        u.parent.right = v
    if v:
        v.parent = u.parent

def delete(root, val):
    cur = root
    # 找到 cur 的位置
    while cur and cur.val != val:
        if val &lt; cur.val:
            cur = cur.left
        else:
            cur = cur.right
    if not cur:
        return None
    if cur.left is None:
        tranplant(root, cur, cur.right)
    elif cur.right is None:
        tranplant(root, cur, cur.left)
    else:
        y = cur.right
        while y.left: 
            y = y.left
        if y.parent != cur:
            tranplant(root, y, y.right)
            y.right = cur.right
            y.right.parent = y
        tranplant(root, cur, y)
        y.left = cur.left
        y.left.parent = y
```

## **结点数量为** **n** **的二叉树，有多少种不同的结构？**

由于不同的二叉树结构决定了递归的顺序问题（出栈和入栈），令 表示进栈， 表示出栈，则可转化为求一个 \(  位、含 个 、  个 的二进制数，满足从左往右扫描到任意一位时，经过的 数不多于  数。显然，含  个 和 个 的 位二进制数共有 个，下面考虑不满足要求的数目

假设其中不合法的序列在位置  处，此时恰好 的数量比 多一位，那么必然后面的 的数量比 多一位，具体而言，  有 位， 有  位。我们将 及之后的序列进行反转，即可得到一个包含了  个 ，  个 的序列。注意这是一个双射的过程，即一个不合法的序列经过构造始终得到唯一的一个包含了 个 ，  个 的序列，而反过来该序列唯一对应一个不合法的序列。下面证明：

定义映射 ：从不满足条件的  个  和  个 的序列到  个  和 个  的序列。映射 的构造：找到第一个违反条件的位置（称为关键位置）将此位置之后的所有 变 ，  变

证明 是单射（一对一）：

假设两个不同的不满足条件的序列 和 映射到同一个序列：

+    和  的关键位置必然相同（否则映射结果会不同）

+   如果 和 在关键位置之前有任何不同，映射后仍然不同

+   如果  和 在关键位置之后有任何不同，由于 和 互换，映射后仍然不同

因此，不可能有两个不同的序列映射到同一个序列。

证明 是满射（映上）：

对于任何  个 和 个 的序列 ，从左到右扫描，必然存在一个位置， 的数量比 的数量多 2（因为总共多 2 个 。这个位置就是我们寻找的关键位置，将此位置之后的 和  互换，得到一个 个 和 个 的序列 。 在关键位置之前满足条件，在关键位置不满足条件，因此 是一个不满足原条件的序列，且

证明 的逆映射：

对于任何 个 和   个 的序列，找到 比  多 2 的位置（一定存在且唯一）。将此位置之后的   和 互换，这个过程是上述映射的逆过程

证毕

所以合法的序列（也就是二叉树不同结构数量）等于：

也就是卡特兰数。其实卡特兰数还满足以下的性质：

可以看成是左子树的数量， 可以看成是右子树的数量，根据乘法原理即可得到总的数量。

# 数组经典问题（双指针、滑动窗口、二分）

## 二分查找如何实现？开区间写法、闭区间写法？

二分查找是在有序数组中进行高效查找的简单算法，而且可以更加广义地使用在单调（非严格、严格）的函数上，于是可以得到算法模板常用的二分答案，其中最重要的事实在于证明计算的结果是具有某种单调性质的

不同于二分查找的思想，二分查找的实现具有大量的细节。下面主要以开闭区间为依据介绍三种常见的实现。

闭区间的写法是最为经典也是最为常见的实现，其中核心的形式为 ，其中 和 分别为左右边界， 为数组长度。

我们可以定义一个函数 ，如果 为真，则 属于右侧合法区间，否则 属于不合法区间。

如果区间的中点值 满足 ，那么更新 ，否则更新

基于此，我们可以观察到两个循环不变量   都是非法的， 都是合法的，而 是待确定的，那么结束的时候也可以知道 或者 是答案

```
# 举一个例子，我们寻找有序数组中第一个大于等于 target 的位置
# f(x) = nums[x] &gt;= target

def lower_bound(nums, target):
    n = len(nums)
    l, r = 0, n-1
    while l &lt;= r: # 保证区间不为空
        mid = (l+r)//2
        if nums[mid] &gt;= target:
            r = mid-1
        else:
            l = mid+1
    return l
```

练习题目：在排序数组中查找元素的第一个和最后一个位置

那么基于上述的闭区间的写法，我们可以推导得到等价的开区间写法和左闭右开区间写法

对于开区间而言，我们的循环不变量就是 都是非法的，  都是合法的，那么其中的 是不确定的

```
# 开区间写法

def lower_bound(nums, target):
    n = len(nums)
    l, r = -1, n  # 注意这里 l, r 的取值范围, l = l-1, r = r+1
    while l+1 &lt; r:
        mid = (l+r)//2
        if nums[mid] &gt;= target:
            r = mid
        else:
            l = mid
    return r
```

对于左闭右开区间而言，循环不变量就变成了 是非法的，  都是合法的，那么其中的 是不确定的

```
# 左闭右开写法

def lower_bound(nums, target):
    n = len(nums)
    l, r = 0, n
    while l &lt; r:
        mid = (l+r)//2
        if nums[mid] &gt;= target:
            r = mid
        else:
            l = mid+1
    return l # l == r
```

## **滑动窗口算法与二分答案算法**

题目：假设有一个长度为 的非负整数的数组，我们需要找到其中和不大于  的最长子数组的长度

上述题目是最经典的滑动窗口算法和二分答案算法的应用。先讲其中的二分答案算法

首先我们可以知道非负整数子数组的和总是越长和就越大，不会因为增加了长度反而和变小了。那么假设我们找到了一个长度为 的子数组，我们是否可以找到一个大小介于 的子数组呢？显然可以，因为我们只需要不断减少这个子数组的长度即可。因此我们可以知道这个答案是具有单调性质的，可以使用二分的方法加速寻找答案

```
# 使用二分答案算法

def get_longgest_subarray(nums, sum):
    n = len(nums)
    pre = [0] * (n+1)
    for i in range(1, n+1):
        pre[i] = pre[i-1] + nums[i-1]
    def check(val) -&gt; bool:
        # 检查是否存在一个长度为val的合法子数组
        nonlocal pre
        for i in range(1, n-val+2):
            # [i, i+val-1]
            if pre[i+val-1] - pre[i-1] &lt;= sum:
                return True
        return False
    
    l, r = -1, n+1 # 开区间写法
    while l+1 &lt; r:
        mid = (l+r)//2
        if check(mid):
            l = mid            
        else:
            r = mid
    return l  # 根据循环不变量 [0,l] 是合法的
```

使用了二分答案之后可以很明显感受到一点：这个最长的子数组是不是可以动态维护？比如说对于以 结尾的子数组，我们知道了它的左边界最远为 ，那么对于以 结尾的子数组，它的最远左边界能够是多少呢？因为刚才已经推导过了，对于长度越长的子数组，和会保持不减，所以其左边界最远就是  ，这可以使用反证法证明，这里就不啰嗦了。因此我们可以使用两个指针维护当前的最大子数组长度，也就是经典的双指针算法

```
# 双指针（滑动窗口）

def get_longgest_subarray(nums, sum):
    n = len(nums)
    l, s = 0, 0
    ans = 0
    for i in range(n):
        s += nums[i]
        while s &gt; sum:
            s -= nums[l]
            l += 1
        if i - l + 1 &gt; ans:
            ans = i - l + 1
    return ans
```

二分答案经常用来解决一种答案具有单调性质的问题，而根据计算复杂度理论，检查是否成立比求解简单。而滑动窗口算法则是一种全局的优化，本质上是动态规划的思想。二分答案在面试算法题中是一种非常重要的算法。

# **回溯算法经典问题**

回溯算法是一种通过穷举所有可能的解来求解问题的方法，广泛应用于各种经典的数学和计算机科学问题中。在面试中也是比较容易考察到的算法题目类型。而这种算法由于基于搜索，因此有很强的套路，需要熟练掌握。这类题目在思考的过程中可以遵循下面的思考步骤：

1.  确定搜索的空间

1.  确定搜素的策略

1.  确定搜素的终止条件

## **枚举子集**

定义：给定一个集合，求出其所有子集（或者统计其信息）

子集问题的核心在于每个元素都有两种状态：要么被选入当前子集，要么不被选入。因此，子集问题的规模为  ，即每个元素的选择组合

练习题目：[电话号码的字母组合](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/description/)

分析一下题目，我们对于其中的每一个数字按键都有多种选择，形式上如果我们知道了按键长度，是可以使用迭代方法获取所有方案的，但是由于我们不知道按键的长度，这个时候就需要回溯算法来实现动态的搜索。

```
choice = [&quot;&quot;, &quot;&quot;, &quot;abc&quot;, &quot;def&quot;, &quot;ghi&quot;, &quot;jkl&quot;, &quot;mno&quot;, &quot;pqrs&quot;, &quot;tuv&quot;, &quot;wxyz&quot;]

class Solution:
    def letterCombinations(self, digits: str) -&gt; List[str]:
        n = len(digits)
        ans = []
        cur = [&#x27;&#x27;] * n
        if n == 0:
            return ans
        def dfs(i):
            if i == n: # 确定好终止条件，这是很重要的，防止无限递归
                ans.append(&#x27;&#x27;.join(cur))
                return
            for num in choice[int(digits[i])]:
                cur[i] = num
                dfs(i+1)
        dfs(0)
        return ans
```

练习题目：[子集](https://leetcode.cn/problems/subsets/description/)

对于每一个数字我们可以选或者不选，因为每一个元素都是不相同的，所以这保证了所有方案的独立性，不需要去重

```
class Solution:
    def subsets(self, nums: List[int]) -&gt; List[List[int]]:
        n = len(nums)
        ans = []
        cur = []
        def dfs(i):
            if i == n:
                ans.append(cur.copy())
                return
            # 选择
            cur.append(nums[i])
            dfs(i+1)
            # 回溯
            cur.pop()

            # 不选择
            dfs(i+1)
        dfs(0)
        return ans
```

练习题目：[分割回文串](https://leetcode.cn/problems/palindrome-partitioning/description/)

回溯算法的思路是，对于每一个位置，我们尝试选择或者不选择，如果选择，那么判断是否为回文串，如果不是回文串，那么就直接剪枝

```
class Solution:
    def partition(self, s: str) -&gt; List[List[str]]:
        n = len(s)
        ans = []
        cur = []
        def dfs(i):
            if i == n:
                ans.append(cur.copy())
                return
            for j in range(i,n):
                t = s[i:j+1]
                if t == t[::-1]: # 判断回文串
                    cur.append(t)
                    dfs(j+1)
                    cur.pop()
        dfs(0)
        return ans
```

# **解决组合问题**

定义：从给定的元素中选取一定数量的元素，求出所有可能的组合。

**组合问题的核心在于，每次**选择时后面的元素不能再被重新选择，且每个组合的顺序不影响结果。这是区别于排列问题的最为关键的点。

练习题目：[组合](https://leetcode.cn/problems/combinations/description/)

这道题目就是最为经典的组合问题，我们需要在原有的子集问题基础上加多一个选择次数的限制，而利用这些限制，我们可以在原有的简单的搜索策略加上一些启发式的规则，统称为剪枝

```
class Solution:
    def combine(self, n: int, k: int) -&gt; List[List[int]]:
        ans = []
        cur = []
        def dfs(i):
            if len(cur) == k:
                ans.append(cur.copy())
                return
            # 剪枝, [i,n] 还有 n-i+1个数字
            if n-i+1 &lt; k-len(cur):
                return
            for j in range(i,n+1):
                cur.append(j)
                dfs(j+1)
                cur.pop()
        dfs(1)
        return ans
```

练习题目：[组合总和 III](https://leetcode.cn/problems/combination-sum-iii/description/)

这道题目和练习题目1十分类似，但是多出了一个限制，那就是最后选择的元素和要等于一个制定的数字，那么我们就可以将这个限制放在搜索的策略中，并且制定对应的剪枝方法，从而实现高效的搜索

```
class Solution:
    def combinationSum3(self, k: int, n: int) -&gt; List[List[int]]:
        ans = []
        cur = []
        s = 0 # 当前和
        def dfs(i):
            nonlocal s
            # 剪枝1, [i,n], 个数限制
            if n-i+1 &lt; k-len(cur):
                return
            # 剪枝2, i,i+1,i+2...n, 数字和限制
            if (n+i)*(n-i+1)//2 &lt; n-s or s &gt; n:
                return
            if len(cur)==k and s == n:
                ans.append(cur.copy())
                return
            for j in range(i,10):
                cur.append(j)
                s += j
                dfs(j+1)
                s -= j
                cur.pop()
        dfs(1)
        return ans
```

# **解决排列问题**

定义：从给定的元素中选取一定数量的元素，使得元素间满足一定的顺序，求出所有可能的排列

排列问题和组合问题的最大区别在于，排列问题需要考虑元素间的顺序，而组合问题只需要考虑元素是否被选择

练习题目1：[全排列](https://leetcode.cn/problems/permutations/description/)

```
class Solution:
    def permute(self, nums: List[int]) -&gt; List[List[int]]:
        n = len(nums)
        ans = []
        cur = []
        def dfs(s):
            if s == 0:
                ans.append(cur.copy())
                return
            for j in range(n):
                if s &amp; (1&lt;&lt;j):
                    cur.append(nums[j])
                    dfs(s ^ (1&lt;&lt;j))
                    cur.pop()
        dfs((1&lt;&lt;n)-1)
        return ans
```

这里使用了二进制集合的方法表示了当前的选择的数字以及还没有选择的数字，比如数字nums[i]就对应了 s 中的第 i 位，0表示已经选择了，否则就表示还没有选择。

练习题目：[N 皇后](https://leetcode.cn/problems/n-queens/description/)

此题的难点在于检查皇后是否处于同一行、同一列、同一对角线上。我们可以将对角线进行一个哈希编码，从而用于快速判断在某条对角线上是否存在皇后

```
class Solution:
    def solveNQueens(self, n: int) -&gt; List[List[str]]:
        ans = []
        board = [&quot;.&quot; * n for _ in range(n)]
        if n == 0:
            return ans
        column = [False] * n
        ldiag = [False] * (2 * n - 1)  # 左对角线
        rdiag = [False] * (2 * n - 1)  # 右对角线
        
        def backtrack(row):
            if row == n:
                ans.append(board.copy())
                return
            for col in range(n):
                # 计算左对角线和右对角线的索引
                l_diag_index = row - col + (n - 1)
                r_diag_index = row + col
                # 检查当前位置是否被攻击
                if column[col] or ldiag[l_diag_index] or rdiag[r_diag_index]:
                    continue
                # 放置皇后
                board[row] = board[row][:col] + &#x27;Q&#x27; + board[row][col+1:]
                column[col] = ldiag[l_diag_index] = rdiag[r_diag_index] = True
                # 递归到下一行
                backtrack(row + 1)
                # 移除皇后（回溯）
                board[row] = board[row][:col] + &#x27;.&#x27; + board[row][col+1:]
                column[col] = ldiag[l_diag_index] = rdiag[r_diag_index] = False
        
        backtrack(0)
        return ans
```

## **动态规划经典问题**

## 基础数据结构经典问题（链表、队列、栈）

## **字符串经典问题**

## **基础图论问题**

## **基础计算几何问题**