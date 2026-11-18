
# Airbnb 房源过滤问题与图匹配算法

## 问题定义

我们尝试使用偶图匹配算法，例如 Hopcroft–Karp 算法到 Airbnb 房源过滤问题：

> 给定一个 Airbnb 房源（`H`）的偶图和过滤器（`F`），其中 `H` 的每个节点可以多于 `F` 的一个相邻节点（共享一个公共边）。寻找 `H` 的由节点构成的子集，该子集和 `F` 的子集的节点相邻。

## Hopcroft–Karp 算法分析

问题的定义很难理解，并且目前我们还不确定 Hopcroft-Karp 算法可以解决该问题，但我们可以在求解的过程中学到很多图算法的关键思想。这个过程不会很短，需要你有耐心。Hopcroft-Karp 算法以二分图为输入，并生成最大基数匹配的输出，该输出是一个包含尽可能多的边的集合，其中没有任何两条边共享同一个端点。熟悉该算法的读者已经注意到，这并不能解决我们的问题，因为匹配过程的条件是没有任何两条边共享同一个节点。我们来看一个示例展示，其中只有 4 个过滤器和 8 个房源（为简单起见）。这些房源用字母 A 到 H 标记，过滤器是随机选择的。从 A 到 H 的所有房源都是 50 美元每晚的价格和一张床，但很少有供应 WiFi 和/或电视的。因此以下的示例过程将尝试找到满足四个条件的房源（拥有 4 个过滤器）。

![](img/a4a52b8d4c6424d8cf39adfd49ca8008-fs8.png)

对该问题的求解需要找到和特定的房源连接的所有边，该房源节点和相同的过滤器子集关联。而 Hopcroft-Karp 算法将移除公共端点的边，并生成和两个子集都关联的边。

查看上图，我们需要寻找的是房源 D 和 G，它们满足了所有四个过滤器值。我们真正需要的是找到共享端点的所有匹配边。我们可以为该方法设计一个算法，但其处理时间可以说和用户需求并不相关（用户需求=快速）。也许创建一个平衡的多分类关键字的二值搜索树会更快，差不过类似于数据库索引文件，其将主关键字和外键映射到满足条件的记录集合。我们将在另一篇文章中独立讨论平衡二值搜索树和数据库索引，到时会再次返回到 Airbnb 房源问题上。

Hopcroft-Karp 算法（以及很多其它算法）都基于 DFS（深度优先搜索）和 BFS（广度优先搜索）的图遍历算法。说实话，这里介绍 Hopcroft-Karp 算法的真正原因是逐渐转换到图遍历算法的讨论，相比从二值树开始讨论会更好。

## 图遍历与二值树遍历

### 二值树的三种遍历方式

二值树遍历非常漂亮，这大多是因为它们的递归本质。有三种基本的遍历方式称为中序（in-order）、后序（post-order）和前序（pre-order）。如果你曾经遍历过连结串列，这些概念是很好懂的。在连结串列中，你只需要输出当前节点的值（在下方的代码中称为 `item`），并继续到达下一个节点。

```py
// struct ListNode {
//   ListNode* next;
//   T item;
// }; 

void TraverseRecursive(ListNode* node) // starting node, most commonly the list 'head'
{
  if (!node) return; // stop
  std::cout << node->item;
  TraverseRecursive(node->next); // recursive call
}

void TraverseIterative(ListNode* node)
{
  while (node) {
    std::cout << node->item;
    node = node->next;
  }
} 
```

这和二值树几乎相同，输出节点的值，然后到达下一个节点，但在这里，「下一个」指的是两个节点，左节点和右节点。因此你需要分别到达左节点和右节点。不过你有三个不同的选择：

* 输出节点值，然后到达左节点，再到达右节点。
* 到达左节点，然后输出节点值，再到达右节点。
* 到达左节点，然后到达右节点，再输出节点值。

```py
// struct TreeNode {
//   T item;
//   TreeNode* left;
//   TreeNode* right;
// }

// you cann pass a callback function to do whatever you want to do with the node's value
// in this particular example we are just printing its value.

// node is the "starting point", basically the first call is done with the "root" node
void PreOrderTraverse(TreeNode* node)
{
  if (!node) return; // stop
  std::cout << node->item;
  PreOrderTraverse(node->left); // do the same for the left sub-tree
  PreOrderTraverse(node->right); // do the same for the right sub-tree
}

void InOrderTraverse(TreeNode* node)
{
  if (!node) return; // stop
  InOrderTraverse(node->left);
  std::cout << node->item;
  InOrderTraverse(node->right);
}

void PostOrderTraverse(TreeNode* node)
{
  if (!node) return; // stop
  PostOrderTraverse(node->left);
  PostOrderTraverse(node->right);
  std::cout << node->item;
} 
```

![](img/019bb95f74585a8c505ffaeeab404ff0-fs8.png)

*前序遍历的细节追踪*

![](img/034cff1758f44e4595d53da20bc83bcb-fs8.png)

### 递归调用的开销

很明显递归函数的形式很优雅，虽然其计算成本很高。每次我们递归地调用一个函数，也就意味着我们调用了一个完全的新函数（如上图所示）。其中「新」的意思是函数变量和局域变量需要分配其它的堆栈内存空间。这正是为什么递归调用的成本如此高（额外的堆栈空间分配和多函数调用）和危险（堆栈溢出），很明显地使用迭代实现会更好。在关键任务（航空、NASA 探测车等）的系统编程中，递归调用是完全禁止的。

## 搜索系统设计：Netflix 与亚马逊

### 实例：Netflix

假设我们要将所有 Netflix 电影存储在二进制搜索树中，并将电影标题作为排序键。所以无论何时用户输入类似「Inter」的内容，我们都会返回一个以「Inter」开头的电影列表，举例，[「Interstellar」,「Interceptor」,「Interrogation of Walter White」]。如果我们将返回标题中包含「Inter」的所有电影（不仅仅是以「Inter」开头的电影）那就太好了，并且该列表将根据电影的评分或与该特定用户相关的内容进行排序（喜欢惊悚片比戏剧更多）。这个例子的重点在于对 `BST` 进行有效的范围查询，但像往常一样，我们不会深入探讨其余部分。基本上，我们需要通过搜索关键字进行快速查找，然后获得按关键字排序的结果列表，这很可能应该是电影评级和/或基于用户个性化数据的内部排名。我们会尽可能地坚持 KISK 原则（Keep It Simple，Karl）。

「KISK」或「让我们保持它的简单」或「为了简单起见」，这是教程编写者从真实问题中抽象出来的超级借口，并通过在伪代码中引入「abc」简单示例及其解决方案来做出大量假设，并且这些答案在很老的笔记本电脑上也能工作。

这个问题可以很容易地应用到亚马逊的产品搜索上，因为我们通常通过输入描述我们兴趣的文本（如「图算法」）来搜索亚马逊的东西，并根据产品的评分获得结果（我没有在亚马逊的个性化结果中体验过搜索结果，但我很确定亚马逊也是这样做的）。所以，为了公平将这个子标题改为...

### Netflix 和亚马逊

Netflix 和亚马逊。Netflix 提供电影服务，亚马逊提供产品，我们会将它们命名为「物品」，所以每当你阅读「物品」时，都会想到 Netflix 中的电影或亚马逊的任何 [合格] 产品。这些物品最常用的是解析其标题和描述（我们只处理标题），所以如果一个操作员（通常是一个人通过管理仪表板将项目的数据插入 Netflix / Amazon 数据库）插入新项目到数据库中，它的标题正在被一些 `ItemTitleProcessor` 处理以产生关键字。

![](img/a31f73af49dbe4a35a4d5d02c093c705-fs8.png)

*我知道这并不是最好的图例（而且有一个书写错误）*

每一个物品都有专属 `ID`，这个 `ID` 也链接到了标题之中的关键字。这也是搜索引擎在爬全世界的网站时做的。他们分析每个文档的内容，对其进行标记（将其分解为更小的实体和单词）并添加到表中，该表将每个标记（词）映射到标记已被「看到」的文档标识（网站）。因此，无论何时搜索「hello」，搜索引擎都会获取映射到关键字「hello」的所有文档（实际情况非常复杂，因为最重要的是搜索相关性，这就是为什么谷歌搜索非常棒）。所以 Netflix /亚马逊的类似表格可能看起来像这样（再次，在阅读物品时想一想电影或产品）。

![](img/199b01e7d45d81a270f84ade2ef433c1-fs8.png)

*倒排索引*

### 倒排索引与基于 BST 的检索

哈希表，再提一次。是的，我们将为此倒排索引（索引结构存储来自内容的映射）保留哈希表。哈希表会将关键字映射到物品的 `BST`。为什么选择 `BST`？因为我们希望保持它们的排序并同时提供连续排序的部分（响应前端请求），例如一次请求（分页）中的 100 个物品。这并不能说明 `BST` 的强大功能，但假设我们还需要在搜索结果中进行快速查找，例如，你需要关键字为「机器」的所有 3 星电影。

![](img/bdbfb07c988ba61dd26e16b0f90d6ba7-fs8.png)

请注意，可以在不同的树中复制物品，因为通常可以使用多个关键字找到物品。我们将使用如下面定义的物品进行操作：

```py
// Cached representation of an Item
// Full Item object (with title, description, comments etc.) 
// could be fetched from the database
struct Item
{
  // ID_TYPE is the type of Item's unique id, might be an integer, or a string
  ID_TYPE id;
  int rating;
}; 
```

每次将新物品插入数据库时，其标题都将被处理并添加到大型索引表中，该表将关键字映射到物品。可能有许多物品共享相同的关键字，因此我们将这些物品保存在按照评分排序的 `BST` 中。当用户搜索某个关键字时，他们会得到按其评分排序的物品列表。我们如何从排序的树中获取列表？通过按顺序遍历。

```py
// this is a pseudocode, that's why I didn't bother with "const&"'s and "std::"'s
// though it could have look better, forgive me C++ fellows

vector<Item*> GetItemsByKeywordInSortedOrder(string keyword)
{
  // assuming IndexTable is a big hashtable mapping keywords to Item BSTs
  BST<Item*> items = IndexTable[keyword]; 

  // suppose BST has a function InOrderProduceVector(), which creates a vector and 
  // inserts into it items fetched via in-order traversing the tree
  vector<Item*> sorted_result = items.InOrderProduceVector();
  return sorted_result;
} 
```

这里是一种 `InOrderProduceVector()` 实现：

```py
template <typename BlaBla>
class BST
{
public:
  // other code ...
  vector<BlaBla*> InOrderProduceVector()
  {
    vector<BlaBla*> result;
    result.reserve(1000); // magic number, reserving a space to avoid reallocation on inserts
    InOrderProduceVectorHelper_(root_, result); // passing vector by reference
    return result;
  }

protected:
  // takes a reference to vector
  void InOrderProduceVectorHelper_(BSTNode* node, vector<BlaBla*>& destination)
  {
    if (!node) return;
    InOrderProduceVectorHelper_(node->left, destination);
    destination.push_back(node->item);
    InOrderProduceVectorHelper_(node->right, destination);
  }

private:
  BSTNode* root_;
}; 
```

但是呢，我们首先需要最高评价的物品，来替换掉按顺序遍历生成最低评级的物品。这是因为它的性质，是从低到高的顺序遍历物品，「自下而上」。为了得到我们想要的东西，即列表按降序而不是升序排列，我们应该仔细查看顺序遍历实现。我们所做的是通过左节点，然后打印当前节点的值和通过右边的节点。当我们第一次通过左节点时，这就是为什么我们首先获得了「最左」节点（最左节点），这是具有最小值的节点。因此，简单地将实现更改为首先通过正确的节点将导致我们按照列表的降序排列。我们会像其他人一样将其命名，这是一种逆序的遍历。让我们更新上面的代码（引入单个列表、警告、错误）：
