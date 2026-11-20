
# 豆包 Marscode 入门：案例

> 来源：[https://agijuejin.feishu.cn/docx/YjHgdvqvSoWXByxXppiclhoinag](https://agijuejin.feishu.cn/docx/YjHgdvqvSoWXByxXppiclhoinag)
>
> 作者：范文杰
>
> 链接：https://juejin.cn/post/7444574123420319770
>
> 来源：稀土掘金
>
> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

## 实例：实现并发控制函数

实例演示，手把手使用豆包MarsCode 实现“并发控制函数”。

熟悉基本操作方法后，接下来我们演示如何借助 豆包MarsCode 实现一个简单功能：并发控制函数。在业务编码场景中，我们常常需要限制同一时间某类任务的并发执行数量，以免触发诸如 QPS 限制或占用过多 CPU、内存等计算资源，导致系统奔溃。

在过去，实现这类函数并不是一件容易的事情，需要对语言、并发机制等有一个比较深入的理解，即使是专业开发者也需要花费一些时间。但借助 豆包MarsCode，只要一句自然语言即可生成相关核心代码：

![](img/f91a27a6526c55a5f17d12901f897e59.png)

```
class PQueue {
  private readonly concurrency: number;
  private readonly queue: (() =&gt; Promise&lt;any&gt;)[];
  private running: number;

  constructor(concurrency: number) {
    this.concurrency = concurrency;
    this.queue = [];
    this.running = 0;
  }

  public add(task: () =&gt; Promise&lt;any&gt;): Promise&lt;any&gt; {
    return new Promise((resolve, reject) =&gt; {
      this.queue.push(() =&gt; task().then(resolve).catch(reject));
      this.schedule();
    });
  }

  private schedule(): void {
    if (this.running &lt; this.concurrency &amp;&amp; this.queue.length &gt; 0) {
      this.running++;
      const task = this.queue.shift()!;
      task().then(() =&gt; {
        this.running--;
        this.schedule();
      }).catch(() =&gt; {
        this.running--;
        this.schedule();
      });
    }
  }
}

const pLimit = (concurrency: number) =&gt; {
  const queue = new PQueue(concurrency);
  return (task: () =&gt; Promise&lt;any&gt;) => queue.add(task);
};
```

并且，更强大的点在于我们可以使用 豆包豆包MarsCode 在此基础上不断迭代优化，例如，我们可以要求 豆包MarsCode 补充代码注释，帮助开发者理解代码意图：

也可以要求 豆包MarsCode 将代码中所有 `any` 类型替换为 `unknown`；或者从 `class` 风格切换为 FP 风格；又或者分析代码中可能存在的性能问题，给出修复意见等等。

甚至，可以用于生成单测代码：

1.  圈选需要生成单测的代码，点击 Unit Test 按钮：

![](img/13c4148503f43002c8b9f5417d668de6.png)

1.  等待右侧对话框完成代码生成：

![](img/65f673d42cc577c7de1146d9665a4f36.png)

1.  点击保存按钮，将单测代码保存到文件：

![](img/3ac412601fcdd85ad5f23fb0e4b88a88.png)

上述编码过程几乎都由 豆包MarsCode 完成，某种程度上开发者从编码角色转换为“编导”角色，主要职责从编写具体代码切换为指导、验收 LLM 的生成结果，甚至极端情况下，只需掌握若干语言最基础的调试、启动规则，就可以完成复杂应用开发，无论是编码新手还是专业开发者，都能从中获得效率提升。
