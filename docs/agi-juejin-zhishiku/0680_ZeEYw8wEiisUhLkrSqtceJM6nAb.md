
# 用了 `bolt`，我决定抛弃 `v0.dev` 了，是 `Cursor` 好搭档

> 来源：[https://agijuejin.feishu.cn/docx/E094d2bSWorvURx5eIkc90vNn0c](https://agijuejin.feishu.cn/docx/E094d2bSWorvURx5eIkc90vNn0c)

> 作者：老码小张

> 链接：https://juejin.cn/post/7443658566701514752

> 来源：稀土掘金

> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

[朋友们，今天](https://link.juejin.cn?target=https%3A%2F%2Fv0.dev%2F)我体验了下 `bolt` 之后，我决定暂时抛弃 `v0.dev` 了，不是说 `v0` 不牛逼，只能说 `bolt` 更性感。

先啥也不说，咱们来试试一个例子：让他生成一个球场预定的 SAAS

![](img/40baf84a881368d6c5b9d7777d0f70bc.png)

基于 `bolt-nextjs-shadcn`，界面还是非常 Nice 的。

然后生成的代码如下，代码直接提供了一个完整的仓库，可以download 下来。

使用 `bolt` 打一个底稿之后，这样，我们在配合 `cursor`，哈哈，效率直[接飞起，他比起 ](https://link.juejin.cn?target=https%3A%2F%2Fwww.copycoder.ai%2F)`v0`来说，完成度我感觉更加高一点，然后，配合我们上篇文章介绍的，`copycode`。

![](img/4f2c403953813d0a69ad6297460cbecd.png)

提供的高质量的 `prompts`，直接把你的项目进展带到了一个 7-80% 的程度，此时，你在使用 `Cursor` 补补刀，三两天一个小 SAAS，看起来也不是不可能呀。

[当然，我们](https://link.juejin.cn?target=https%3A%2F%2Fwww.reweb.so%2F)团队的小伙伴说，再配合上 `reweb`

对 UI 的配色和组件进行可视化的编辑，最后将代码导出到 `Cursor` 中进行修修补补，也是一件非常惬意的事情。

## 接下来，我们了解下 `Bolt.new` 是什么？

简单来说，`Bolt.new` 是基于 `StackBlitz` 的 `WebContainer` `API` 构建的在线开发工具，集成了 `AI` 和全栈开发功能。它允许用户：

+ 编写、运行和调试全栈应用程序。

+ 安装 `npm` 包，比如 `Vue`、`React`、`Next.js`。

+ 直接在浏览器中运行 `Node.js` 服务器。

+ 部署项目到生产环境（支持 `Vercel`、`Netlify` 等）。

+ 用 `AI` 辅助生成和优化代码。

与其他基于云的 `IDE` 不同的是，`Bolt.new` 的开发环境完全在浏览器内完成，消除了传统虚拟机的开销，同时提升了安全性。

然后，`bolt` 支持的技术栈还挺全的。

![](img/2c3449c774bb9f0adcc47861d2bb3407.png)

几乎比较流行的靠谱的都在。

### `Bolt.new`：更全面的全栈环境

`Bolt.new` 是一个更偏向开发者的工具，不仅能生成代码，还提供一个完整的开发环境。你不仅可以运行 `Node.js` 服务器、调用 `API`，还能安装 `npm` 包、调试代码，甚至直接部署上线。

如果说 `v0.dev` 是一个 “快餐店”，`Bolt.new` 就是一个 “全功能厨房”，提供更多灵活性和技术控制。

#### 核心特点：

+ 全栈支持：不仅能生成前端，还支持后端开发，适合构建更复杂的应用。

+ `AI` 辅助但非自动：生成代码的过程需要用户更多的交互和优化。

+ 一站式开发环境：包括调试、部署等功能。

### 与 `v0` 选型对比表

如果你的目标是快速生成一个网页应用原型，几乎不需要编程知识，`v0.dev` 是更好的选择；而如果你需要一个功能强大的开发环境，能兼顾前后端并快速部署，`Bolt.new` 则更胜一筹。
