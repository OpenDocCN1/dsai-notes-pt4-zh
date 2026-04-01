# 链表

> 来源：[https://datawhaler.feishu.cn/docx/JB0ndBLPVoceCkxUC3Ucs5mknEf](https://datawhaler.feishu.cn/docx/JB0ndBLPVoceCkxUC3Ucs5mknEf)

### 我们先来看看常用链表节点的定义：

##### 单链表：

```
class ListNode {
    // 数据域，存储节点的值
    val: number;
    // 指针域，指向下一个节点，初始为 null
    next: ListNode | null;
    constructor(val?: number, next?: ListNode | null) {
        // 若未传入值，默认值为 0
        this.val = (val === undefined ? 0 : val);
        // 若未传入下一个节点，默认指向 null
        this.next = (next === undefined ? null : next);
    }
}
```

##### 循环链表：

循环链表的节点结构和单链表一样，不过它的最后一个节点的 next 指针会指向链表的头节点，从而形成一个环。

##### 双向链表：

```
class DoublyListNode {
    // 数据域，存储节点的值
    val: number;
    // 指向前一个节点的指针，初始为 null
    prev: DoublyListNode | null;
    // 指向后一个节点的指针，初始为 null
    next: DoublyListNode | null;
    constructor(val?: number, prev?: DoublyListNode | null, next?: DoublyListNode | null) {
        // 若未传入值，默认值为 0
        this.val = (val === undefined ? 0 : val);
        // 若未传入前一个节点，默认指向 null
        this.prev = (prev === undefined ? null : prev);
        // 若未传入下一个节点，默认指向 null
        this.next = (next === undefined ? null : next);
    }
}
```

## 160 题. 相交链表

<https://leetcode.cn/problems/intersection-of-two-linked-lists/description/?envType=study-plan-v2&envId=top-100-liked>

### 第一种方法：思路分析

最简单粗暴的方式就是一一遍历，然后作对比，如果两个链表没有一个节点相同那就不会有相交的地方。

### 代码实现

```
function getIntersectionNode(headA: ListNode | null, headB: ListNode | null): ListNode | null {
    let currentA = headA;
    // 外层循环遍历链表 A
    while (currentA) {
        let currentB = headB;
        // 内层循环遍历链表 B
        while (currentB) {
            if (currentA === currentB) {
                return currentA;
            }
            currentB = currentB.next;
        }
        currentA = currentA.next;
    }
    return null;
}
```

这种办法是最容易想到的，但是我们来算一下他的时间复杂度和空间复杂度。

### 复杂度分析

+   时间复杂度：O（m+n），其中 m 和 n 分别是链表 A 和链表 B 的长度。因为每个指针最多遍历两个链表的长度。

+   空间复杂度：O（1），只使用了常数级的额外空间，仅用于存储两个指针变量。

那有没有优化的空间呢？有，我们可以使用双指针来做。

### 第二种方法：思路分析

假设有两个链表，一个长一个短，就像两条不同长度的跑道在某个位置汇合了。我们让两个人（指针）分别从两条跑道的起点开始跑，当一个人跑完自己的跑道后，接着去另一个跑道继续跑。由于他们的速度相同（每次移动一步），最终他们会在汇合点相遇（如果有汇合点），或者同时到达终点（如果没有汇合点）。

假设链表 A 的长度为 m，链表 B 的长度为 n，两个链表相交部分的长度为 k。从链表头到相交节点，链表 A 非相交部分的长度设为 a，链表 B 非相交部分的长度设为 b，那么有 m = a +  k，n = b + k。如果遍历完A，然后在A的指针再去遍历B，B的指针去遍历A，这时候就有 A 走过的路程(a+k)+n，B走过的路为(b+k)+m。这样在遍历过程中，两个指针最终会同时到达相交节点（如果存在）或者同时到达 null（如果不相交）。

### 代码实现

```
function getIntersectionNode(headA: ListNode | null, headB: ListNode | null): ListNode | null {
    // 如果其中一个链表为空，直接返回 null
    if (headA === null || headB === null) {
        return null;
    }
    // 初始化两个指针
    let pA: ListNode | null = headA;
    let pB: ListNode | null = headB;
    // 当 pA 和 pB 不相等时继续循环
    while (pA !== pB) {
        // 如果 pA 遍历到链表 A 的末尾，将它指向链表 B 的头节点
        pA = pA === null ? headB : pA.next;
        // 如果 pB 遍历到链表 B 的末尾，将它指向链表 A 的头节点
        pB = pB === null ? headA : pB.next;
    }
    // 当 pA 和 pB 相等时，返回 pA（此时 pA 可能是相交节点，也可能是 null）
    return pA;
}
```

### 复杂度分析

+   时间复杂度：O（m + n），其中 m和 n 分别是链表 A 和链表 B 的长度。因为每个指针最多遍历两个链表的长度。

+   空间复杂度：O（1），只使用了常数级的额外空间。

### 第三种办法：思路分析

假如我们把其中一个链表存到hashSet中，然后去检查链表 B 中的节点是否在hashSet里，如果存在，那么这个节点就是两个链表的相交节点；如果遍历完链表 B 都没有找到这样的节点，说明两个链表不相交。为什么会考虑这种办法呢？首先是考虑到hashSet的一些方法，会更方便一点。

### 代码实现：

```
function getIntersectionNode(headA: ListNode | null, headB: ListNode | null): ListNode | null {
    const nodeSet = new Set&lt;ListNode&gt;();
    let currentA = headA;
    // 将链表 A 的所有节点存入哈希集合
    while (currentA) {
        nodeSet.add(currentA);
        currentA = currentA.next;
    }

    let currentB = headB;
    // 遍历链表 B，检查节点是否在哈希集合中
    while (currentB) {
        if (nodeSet.has(currentB)) {
            return currentB;
        }
        currentB = currentB.next;
    }

    return null;
}
```

### 复杂度分析

+   时间复杂度：O（m + n），其中 m 是链表 A 的长度，n 是链表 b 的长度。需要先遍历链表 A 将所有节点存入哈希集合，时间复杂度为 O（m），然后遍历链表 B 检查节点是否在集合中，时间复杂度为 O（n）。

+   空间复杂度：O（m），主要用于存储链表 A 的所有节点，哈希集合的空间开销与链表 A 的长度成正比。

## 206 题. 反转链表

<https://leetcode.cn/problems/reverse-linked-list/description/?envType=study-plan-v2&envId=top-100-liked>

### 思路分析：

这里我们可以考虑使用迭代的方法，核心思想是遍历链表的每一个节点，然后把当前节点的 next 指针指向前一个节点，以此实现反转。

### 代码实现

```
function reverseList(head: ListNode | null): ListNode | null {
    let prev: ListNode | null = null;
    // 初始化一个变量 prev，用于存储当前节点的前一个节点，初始值设为 null，是因为在反转操作开始前，头节点的前一个节点不存在
    let curr: ListNode | null = head;
    // 初始化一个变量 curr，用于存储当前正在处理的节点，初始值为传入的头节点 head
    while (curr !== null) {
        const next: ListNode | null = curr.next;
        // 在每次循环中，首先保存当前节点 curr 的下一个节点到变量 next 中
        // 这样做是为了避免在修改 curr.next 时丢失后续节点的引用
        curr.next = prev;
        // 将当前节点 curr 的 next 指针指向前一个节点 prev
        // 这一步是实现链表反转的关键，改变了节点的指向方向
        prev = curr;
        // 将 prev 移动到当前节点 curr 的位置
        // 为处理下一个节点做准备，更新 prev 为当前已处理的节点
        curr = next;
        // 将 curr 移动到之前保存的下一个节点 next 的位置
        // 这样就可以继续处理链表中的下一个节点
    }
    return prev;
    // 当 while 循环结束时，说明链表已经遍历完所有节点，反转操作完成
    // 此时 prev 指向反转后链表的头节点，将其作为函数的返回值
}
```

### 复杂度分析：

+   时间复杂度为 O（n），其中 n 是链表的长度。

+   空间复杂度为 O（1）。

## 234 题. 回文链表

<https://leetcode.cn/problems/palindrome-linked-list/description/?envType=study-plan-v2&envId=top-100-liked>

### 思路分析：

回文链表一定是这类的，1->2->1或者是1->2->2->1这种的。那我们的步骤就可以这样

1.  找到链表的中间节点：可以使用快慢指针法，快指针每次移动两步，慢指针每次移动一步，当快指针到达链表末尾时，慢指针正好指向链表的中间节点。

1.  反转链表的后半部分：从慢指针指向的节点开始，反转链表的后半部分。

1.  比较前半部分和反转后的后半部分：分别用两个指针从链表的头部和反转后的后半部分的头部开始遍历，比较对应节点的值，如果有不相等的情况，则不是回文链表。

### 代码实现：

```
function isPalindrome(head: ListNode | null): boolean {
    if (head === null || head.next === null) {
        return true;
    }
    // 计算链表长度
    let length = 0;
    let current = head;
    while (current) {
        length++;
        current = current.next;
    }
    // 找到中间节点
    let middle = Math.floor(length / 2);
    current = head;
    for (let i = 0; i &lt; middle; i++) {
        current = current.next;
    }
    // 如果链表长度是奇数，跳过中间节点
    if (length % 2 === 1) {
        current = current.next;
    }
    // 反转后半部分链表
    let prev: ListNode | null = null;
    while (current) {
        const nextNode = current.next;
        current.next = prev;
        prev = current;
        current = nextNode;
    }
    // 从链表头和反转后的后半部分头开始比较
    let left = head;
    let right = prev;
    while (right) {
        if (left.val !== right.val) {
            return false;
        }
        left = left.next;
        right = right.next;
    }
    return true;
}
```

### 复杂度分析：

+   时间复杂度：O（n），其中 n 是链表的长度。需要遍历链表一次来计算长度，然后再遍历一半的链表来反转后半部分，最后比较前半部分和反转后的后半部分。

+   空间复杂度：O（1），只使用了常数级的额外空间。

## 141 题. 环形链表

<https://leetcode.cn/problems/linked-list-cycle/?envType=study-plan-v2&envId=top-100-liked>

### 思路分析：

就好比操场环形跑道一样，两个人跑步，如果要相遇，那就一定会有一个跑的慢的和一个跑得快的。所以我们可以采用快慢指针来判断是否有环。具体步骤如下：

1.  初始化两个指针，slow 和 fast，都指向链表的头节点 head。

1.  slow 指针每次移动一步，fast 指针每次移动两步。

1.  在移动过程中，如果 fast 指针到达链表末尾（即 fast 或者 fast.next 为 null），说明链表中不存在环，返回 false。

1.  如果 slow 指针和 fast 指针相遇，说明链表中存在环，返回true。

### 代码实现：

```
function hasCycle(head: ListNode | null): boolean {
    // 如果链表为空或者只有一个节点，肯定不存在环，直接返回 false
    if (head === null || head.next === null) {
        return false;
    }
    // 初始化慢指针，从链表头开始，每次移动一步
    let slow: ListNode | null = head;
    // 初始化快指针，从链表头开始，每次移动两步
    let fast: ListNode | null = head;
    // 当快指针和它的下一个节点都不为 null 时，继续循环
    while (fast !== null &amp;&amp; fast.next !== null) {
        // 慢指针移动一步
        slow = slow!.next;
        // 快指针移动两步
        fast = fast.next.next;
        // 如果慢指针和快指针相遇，说明链表中存在环
        if (slow === fast) {
            return true;
        }
    }
    // 循环结束都没有相遇，说明链表中不存在环，返回 false
    return false;
}
```

### 复杂度分析：

+   时间复杂度：O（n），其中 n 是链表中的节点数。在最坏情况下，需要遍历链表一次。

+   空间复杂度：O（1），只使用了常数级的额外空间。

### 第二种方法：思路分析

假定现在有一群人，他们其中的一部分围成了一个环，那我们怎么判断是否有环呢？从左往右数，就看之前有没有见过这个人，如果见过了就说明有环。所以我们可以给链表加一个标记位。用来记录是否见过它。

### 代码实现：

```
function hasCycle(head: ListNode | null): boolean {
    let current = head;
    while (current) {
        // 检查当前节点是否已经被标记
        if (current[&quot;visited&quot;]) {
            return true;
        }
        // 给当前节点做标记
        current[&quot;visited&quot;] = true;
        // 移动到下一个节点
        current = current.next;
    }
    return false;
}
```

但是这种方法会修改原链表的节点，因为要给节点添加 visited属性。如果不允许修改原链表结构，这种方法就不适用。

### 复杂度分析

+   时间复杂度是 O（n），其中 n 是链表的长度，因为需要遍历链表一次。

+   空间复杂度是 O（1），因为只使用了常数级的额外空间。

## 142.环形链表二

<https://leetcode.cn/problems/linked-list-cycle-ii/?envType=study-plan-v2&envId=top-100-liked>

### 思路分析：

遍历链表，类似上边 141 题，将每个节点的引用存储到一个哈希表 Set中。在遍历过程中，检查当前节点是否已经存在于哈希表中。若存在，表明链表有环；若遍历到链表末尾都没发现重复节点，就说明链表无环。

### 代码实现：

```
function detectCycle(head: ListNode | null): ListNode | null {
    // Set 集合的特性是存储的元素唯一，方便我们判断节点是否重复出现
    const nodeSet = new Set&lt;ListNode&gt;();
    let current = head;
    // 开始遍历链表，只要当前节点不为空，就继续循环
    while (current) {
        if (nodeSet.has(current)) {
            return current;
        }
        // 如果当前节点不存在于 Set 集合中，将其添加到集合中, 表示该节点已经被遍历过
        nodeSet.add(current);
         // 将指针移动到下一个节点，继续遍历链表
        current = current.next;
    }
    return null;
} 
```

### 复杂度分析：

+   **时间复杂度**：O（n），其中 n 是链表的长度。需要遍历链表一次，每次操作哈希表的时间复杂度为 O（1）。

+   **空间复杂度**：O（n），最坏情况下需要存储链表中的所有节点。

## 2.两数相加

<https://leetcode.cn/problems/add-two-numbers/description/?envType=study-plan-v2&envId=top-100-liked>

### 思路分析：

要解决两个逆序存储数字的链表相加问题，核心思路是逐位相加两个链表的对应节点值，并考虑进位情况。我们会创建一个新的链表来存储相加结果。具体步骤为：

1.  准备一个虚拟头节点，方便后续操作结果链表。

1.  用一个变量记录进位。

1.  同时遍历两个链表，将对应节点值与进位相加，得到当前位结果和新的进位。

1.  为当前位结果创建新节点并连接到结果链表。

1.  若某个链表先遍历完，继续处理另一个链表剩余节点。

1.  最后若还有进位，添加一个新节点。

### 代码实现：

```
function addTwoNumbers(l1: ListNode | null, l2: ListNode | null): ListNode | null {
    // 创建虚拟头节点
    const dummy = new ListNode();
    // 当前节点指针，初始指向虚拟头节点
    let current = dummy;
    // 进位初始化为 0
    let carry = 0;

    // 只要有一个链表没遍历完或者还有进位，就继续循环
    while (l1 || l2 || carry) {
        // 获取当前节点的值，如果节点为空则为 0
        const num1 = l1? l1.val : 0;
        const num2 = l2? l2.val : 0;

        // 计算当前位的和，包含进位
        const sum = num1 + num2 + carry;
        // 计算新的进位
        carry = Math.floor(sum / 10);
        // 当前位的值为和对 10 取余
        const digit = sum % 10;

        // 创建新节点存储当前位的值
        current.next = new ListNode(digit);
        // 移动结果链表的指针
        current = current.next;

        // 移动原链表的指针
        if (l1) {
            l1 = l1.next;
        }
        if (l2) {
            l2 = l2.next;
        }
    }

    // 返回结果链表的头节点
    return dummy.next;
}    
```

Math.floor 是 JavaScript（TypeScript 兼容 JavaScript 的内置函数）里的一个内置函数，其功能是对传入的数字进行向下取整操作。也就是说，它会返回小于或等于该数字的最大整数。在carry = Math.floor(sum / 10)里；sum是当前位上两个链表节点的值与进位相加的总和。由于每一位节点只能存储一位数字（范围是 0 - 9），当sum大于等于 10 时，就会产生进位。借助Math.floor(sum / 10)，能够得到进位的值。

### 复杂度分析：

+   **时间复杂度**：O（max（m， n）），这里m和n分别是两个链表的长度。因为要对两个链表进行逐位处理，直到较长的链表遍历结束。

+   **空间复杂度**：O（max（m， n）），主要的额外空间用于存储结果链表，其长度最大为max(m,n)+1（考虑最后有进位的情况）。

## 19.删除链表的倒数第 N 个节点

<https://leetcode.cn/problems/remove-nth-node-from-end-of-list/description/?envType=study-plan-v2&envId=top-100-liked>

### 思路分析：

我们知道链表的增删比较容易，就是通过修改指针的指向来操作。所以我们可以先用这种办法来解决一下问题。

+   首先遍历一次链表，计算出链表的总长度 length。

+   然后根据 length和 n计算出要删除节点的正数位置

+   接着再次遍历链表，找到要删除节点的前一个节点，修改其指针指向，跳过要删除的节点。

### 代码实现：

```
function removeNthFromEnd(head: ListNode | null, n: number): ListNode | null {
    // 创建虚拟头节点，方便处理删除头节点的情况
    const dummy = new ListNode(0);
    dummy.next = head;

    // 第一次遍历计算链表长度
    let length = 0;
    let current = head;
    while (current) {
        length++;
        current = current.next;
    }

    // 计算要删除节点的正数位置
    let pos = length - n;
    current = dummy;

    // 第二次遍历找到要删除节点的前一个节点
    while (pos &gt; 0) {
        pos--;
        current = current!.next;
    }

    // 删除节点
    if (current &amp;&amp; current.next) {
        current.next = current.next.next;
    }

    return dummy.next;
}    
```

### 复杂度分析：

+   **时间复杂度**：O（L），其中 L 是链表的长度。需要遍历链表两次，每次遍历的时间复杂度都是 O（L）。

+   **空间复杂度**：O（1），只使用了常数级的额外空间。

## 24.两两交换链表中的节点

<https://leetcode.cn/problems/swap-nodes-in-pairs/?envType=study-plan-v2&envId=top-100-liked>

### 思路分析：

+   首先创建一个虚拟头节点 dummy，其作用是简化边界条件的处理，尤其是当需要交换原链表的头节点时。

+   用 prev 指针指向当前要交换的两个节点的前一个节点，初始时 prev 指向 dummy。

+   借助 first 和 second指针分别指向要交换的两个相邻节点。

+   对指针进行修改，完成节点的交换，然后更新 prev、first和 second 指针，以便处理下一组相邻节点。

### 代码实现：

```
function swapPairs(head: ListNode | null): ListNode | null {
    const dummy = new ListNode(0);
    dummy.next = head;
    let prev = dummy;

    while (head &amp;&amp; head.next) {
        const first = head;
        const second = head.next;

        // 交换节点
        prev.next = second;
        first.next = second.next;
        second.next = first;

        // 更新指针
        prev = first;
        head = first.next;
    }

    return dummy.next;
}
```

### 复杂度分析：

+   **时间复杂度**：O（n），这里的 n 是链表的长度。因为需要遍历链表一次，对每两个相邻节点进行交换操作。

+   **空间复杂度**：O（1），只使用了常数级的额外空间，如几个指针变量。

## 138.随机链表的复制

<https://leetcode.cn/problems/copy-list-with-random-pointer/description/?envType=study-plan-v2&envId=top-100-liked>

### 思路分析：

要实现链表的深拷贝，可按以下步骤操作：

1.  复制节点：遍历原链表，为每个节点创建一个值相同的新节点，并将新节点插入到原节点之后，形成新的链表。

1.  设置随机指针：再次遍历链表，对于原链表的每个节点，若其随机指针不为空，则将其下一个节点（即复制节点）的随机指针指向原节点随机指针所指节点的下一个节点。

1.  拆分链表：最后一次遍历链表，将原链表和复制链表分离，恢复原链表的结构，同时得到复制链表。

### 代码实现：

```
function copyRandomList(head: Node | null): Node | null {
    if (!head) return null;

    // 第一步：复制节点并插入到原节点之后
    let current = head;
    while (current) {
        const newNode = new Node(current.val);
        newNode.next = current.next;
        current.next = newNode;
        current = newNode.next;
    }

    // 第二步：设置随机指针
    current = head;
    while (current) {
        if (current.random) {
            current.next!.random = current.random.next;
        }
        current = current.next!.next;
    }

    // 第三步：拆分链表
    const newHead = head.next;
    let original = head;
    let copy = newHead;
    while (original) {
        original.next = copy!.next;
        original = original.next;
        if (original) {
            copy!.next = original.next;
            copy = copy!.next;
        }
    }

    return newHead;
}
```

### 复杂度分析：

+   **时间复杂度**：三个步骤的时间复杂度都为 O（n），所以总的时间复杂度也是为 O（n）。

+   **空间复杂度**：除了返回的复制链表所需的额外空间外，没有使用其他与链表长度相关的额外空间。所以他的空间复杂度为 O（1）。

## 148.排序链表

<https://leetcode.cn/problems/sort-list/?envType=study-plan-v2&envId=top-100-liked>

### 思路分析：

我们知道数组的排序方式有很多种，而且好用，那么我们可不可以用数组来做这道题呢？当然可以，我们可以先把节点一个一个存进数组里边，存进去之后就可以调用sort来进行排序，排序完之后，在重新建一个虚拟头节点，把数组中的数据一个一个连上去，就是一个排序好的链表了。

### 代码实现：

```
function sortList(head: ListNode | null): ListNode | null {
    if (head === null) {
        return null;
    }
    const values: number[] = [];
    let current: ListNode | null = head;
    // 将链表节点的值存入数组
    while (current) {
        values.push(current.val);
        current = current.next;
    }
    // 对数组进行排序
    values.sort((a, b) =&gt; a - b);

    // 用排序好的数组元素构建新的链表
    const dummy = new ListNode();
    current = dummy;
    for (const val of values) {
        current.next = new ListNode(val);
        current = current.next;
    }
    return dummy.next;
}
```

### 复杂度分析：

+   **时间复杂度**：O（n log n）。主要时间开销在于数组的排序操作，数组的 sort方法的平均时间复杂度为 O（n log n），遍历链表和构建新链表的时间复杂度均为 O（n），因此总的时间复杂度为 O（n log n）。

+   **空间复杂度**：O（n）。需要额外的数组来存储链表节点的值，数组的长度为 n，因此空间复杂度为 O（n）。

### 方法二：归并排序

### 思路分析：

归并排序本质上是一种分治排序，如果我们把链表利用快慢指针进行分割然后递归的进行排序，每次排好序之后进行合并，全部完成之后就是已经排序好的链表了。所以我们需要有三个步骤，分割链表，排序链表，然后合并链表。感觉这种办法没有数组方法来得快。

### 代码实现：

```
// 对链表进行排序的主函数
function sortList(head: ListNode | null): ListNode | null {
    // 若链表为空或只有一个节点，直接返回
    if (head === null || head.next === null) {
        return head;
    }
    // 找到链表的中间节点
    const mid = getMiddle(head);
    // 中间节点的下一个节点作为右子链表的头节点
    const rightHead = mid.next;
    // 断开左右子链表的连接
    mid.next = null;

    // 递归地对左右两部分进行排序
    const left = sortList(head);
    const right = sortList(rightHead);

    // 合并两个有序链表
    return merge(left, right);
}

// 找到链表中间节点的函数
function getMiddle(head: ListNode): ListNode {
    let slow = head;
    let fast = head;
    let prev = null;
    // 快慢指针遍历链表，快指针每次走两步，慢指针每次走一步
    while (fast !== null &amp;&amp; fast.next !== null) {
        prev = slow;
        slow = slow.next!;
        fast = fast.next.next;
    }
    // 返回中间节点的前一个节点
    return prev!;
}

// 合并两个有序链表的函数
function merge(l1: ListNode | null, l2: ListNode | null): ListNode | null {
    // 创建虚拟头节点
    const dummy = new ListNode();
    let current = dummy;

    // 比较两个链表节点值，将较小值节点连接到结果链表
    while (l1 !== null &amp;&amp; l2 !== null) {
        if (l1.val &lt; l2.val) {
            current.next = l1;
            l1 = l1.next;
        } else {
            current.next = l2;
            l2 = l2.next;
        }
        current = current.next;
    }

    // 将剩余的链表连接到结果链表
    if (l1 !== null) {
        current.next = l1;
    } else {
        current.next = l2;
    }

    return dummy.next;
}
```

### 复杂度分析：

+   **时间复杂度**：O（n log n）。归并排序的时间复杂度由分割和合并两部分组成。每次分割将链表长度减半，需要进行 log n 次分割操作；每次合并操作需要遍历所有节点，时间复杂度为 O（n）。因此，总的时间复杂度为 O（n log n）。

+   **空间复杂度**：O（log n）。递归调用栈的深度为 log n，因为每次递归将链表长度减半，所以递归的最大深度为 log n。因此，递归调用栈的空间复杂度为 O（log n）。

## 146.LRU 缓存

<https://leetcode.cn/problems/lru-cache/description/?envType=study-plan-v2&envId=top-100-liked>

### 思路分析：

[什么是 LR](https://juejin.cn/post/6844903982779875335?searchId=20250317094943EEF112BCA56C97C38F36)U，我们可以看一下这篇文章缓存淘汰算法，这里我们可以可以利用 Map 对象的特性来实现。Map 对象能够记住键的插入顺序，并且它的 keys() 方法返回的迭代器按照插入顺序排列，这就使得我们可以较为方便地实现 LRU 缓存机制。

### 代码实现：

```
class LRUCache {
    private capacity: number;
    private cache: Map&lt;number, number&gt;;

    constructor(capacity: number) {
        this.capacity = capacity;
        this.cache = new Map();
    }

    get(key: number): number {
        if (this.cache.has(key)) {
            const value = this.cache.get(key)!;
            // 先删除原有的键值对，再重新插入，以更新顺序
            this.cache.delete(key);
            this.cache.set(key, value);
            return value;
        }
        return -1;
    }

    put(key: number, value: number): void {
        if (this.cache.has(key)) {
            // 若键已存在，先删除原有的键值对
            this.cache.delete(key);
        } else if (this.cache.size &gt;= this.capacity) {
            // 若缓存已满，移除最久未使用的键值对
            const firstKey = this.cache.keys().next().value;
            this.cache.delete(firstKey);
        }
        // 插入新的键值对
        this.cache.set(key, value);
    }
}    
```

### 复杂度分析：

+   **时间复杂度**：get 和 put操作的平均时间复杂度都是 O（1）。因为 Map 对象的 has、get、set和 delete 操作的时间复杂度均为 O（1）。

+   **空间复杂度**：O（capacity），主要用于存储 Map 中的键值对。

## 常见的一些链表类问题方法选择

### **操作类型**

+   节点遍历：如计算链表长度、寻找中间节点 → 单指针或快慢指针。

+   节点修改：如反转链表、删除节点 → 三指针迭代或双指针。

+   结构重组：如合并链表、交换相邻节点 → 递归或虚拟头节点辅助。

### **边界条件**

+   是否需要处理头节点变化？ → 使用虚拟头节点（如删除倒数第 n 个节点、交换相邻节点）。

+   链表是否可能为空或仅有一个节点？ → 在代码中提前判断。

## 做链表类题的一些通用办法

### 定义辅助节点

+   **虚拟头节点**：在处理涉及头节点操作（如删除头节点、交换头节点等）的问题时，创建一个虚拟头节点 dummy是非常有效的方法。它可以避免对头节点进行特殊处理，简化代码逻辑。例如，在删除链表倒数第 n个节点和两两交换链表相邻节点的问题中，都可以使用虚拟头节点来简化边界条件的处理。

```
const dummy = new ListNode(0);
dummy.next = head;
```

### 指针操作

+   **单指针**：在一些简单的链表遍历问题中，使用一个指针就可以完成任务。例如，计算链表的长度，只需要一个指针从头节点开始，依次遍历每个节点并计数。

```
let current = head;
let length = 0;
while (current) {
    length++;
    current = current.next;
}
```

+   **双指针**：双指针是解决链表问题的常用技巧，常见的有快慢指针和前后指针。

+   **快慢指针**：通常快指针每次移动两步，慢指针每次移动一步。可以用于寻找链表的中间节点、判断链表是否有环等问题。例如，在寻找链表中间节点时，快指针到达链表末尾时，慢指针正好指向中间节点。

```
let slow = head;
let fast = head;
while (fast &amp;&amp; fast.next) {
    slow = slow.next;
    fast = fast.next.next;
}
```

+   **前后指针**：两个指针保持一定的距离，用于解决删除倒数第 n个节点等问题。先将一个指针向前移动 n步，然后两个指针同时移动，当前面的指针到达链表末尾时，后面的指针就指向要删除的节点的前一个节点。

```
let prev = dummy, curr = dummy;
for (let i = 0; i &lt;= n; i++) curr = curr.next;
while (curr) {
    prev = prev.next;
    curr = curr.next;
}
prev.next = prev.next.next; // 删除节点
```

+   **三指针**：在反转链表等问题中，可能需要使用三个指针，分别记录当前节点、前一个节点和下一个节点，以便在反转过程中正确修改指针的指向。

```
let prev: ListNode | null = null;
let curr: ListNode | null = head;
while (curr) {
    const next = curr.next;
    curr.next = prev;
    prev = curr;
    curr = next;
}
```

### 递归方法

+   对于一些具有递归结构的链表问题，使用递归方法可以使代码更加简洁。递归的关键在于找到递归的终止条件和递归关系。例如，在两两交换链表相邻节点的问题中，递归的终止条件是链表为空或只有一个节点，递归关系是先交换当前两个节点，然后递归处理后续节点。

```
function swapPairs(head: ListNode | null): ListNode | null {
    if (!head || !head.next) {
        return head;
    }
    const first = head;
    const second = head.next;
    first.next = swapPairs(second.next);
    second.next = first;
    return second;
}
```

### 边界条件处理

+   在编写链表代码时，一定要考虑边界条件，如链表为空、链表只有一个节点等情况。这些边界条件可能会导致代码出现错误，因此需要在代码中进行特殊处理。例如，在删除链表节点时，如果链表为空或者要删除的节点不存在，需要进行相应的判断和处理。

> 刚开始写不出来算法题没什么关系，可以先抄抄别人的，看看别人怎么做的，多联系，思考每一步是怎么做的，为什么这么写，也可以发散一下思路，看看有没有其他解法。有思路，不会写还可以问问deepseek或者其他大模型，先弄明白几道题，多试几次，知道他们的套路之后再去做其他的题，就算是不会写，也会有一些思路，知道怎么去拆解分析他。做算法题就是，在模仿中学习，在学习中理解。加油！！！