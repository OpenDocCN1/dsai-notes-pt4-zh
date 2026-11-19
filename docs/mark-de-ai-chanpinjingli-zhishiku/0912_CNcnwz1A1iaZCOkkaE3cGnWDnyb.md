
# `Pencil` + `Claude Code`：快速原型图

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/YIlEdNSGhoz1MexXeN7cGSxonsc](https://qqs7y1hozd1.feishu.cn/docx/YIlEdNSGhoz1MexXeN7cGSxonsc)

## 一、`Pencil`

在 `Claude Code` 里说一句"给我画一个登录页面"，`Pencil` 就能自动画出来。

## 二、下载安装与配置

官网直接下载桌面端，`Windows`/`Mac` 都支持。

（之前不支持`windows`我就没折腾，最近开始支持`intel`/`AMD`x64了）

![](img/4cfa88365adc97b2f73000ccb3ecfa9e.png)

### 关键配置

装好后，`Pencil` 会自动配置好 `MCP` 服务。

可以在`claude`窗口中输入 `/mcp` 验证一下

![](img/15488deab9add7e2ab76c9d06f20cb7f.png)

但要在 `Claude` 的配置文件里加个权限设置，不然每次使用时，一直弹窗确认：

配置路径：`C:\Users\你的用户名\.claude\settings.json`

```
{ &quot;permissions&quot;: { &quot;allow&quot;: [&quot;mcp__pencil&quot;] } }
```

![](img/bec9ea8ce436ff666721b507ed101d8b.png)

## 三、在 `Pencil` 中使用 AI 作图

### 手动作图

`pencil`和常用的原型工具类似，比如`Axture`和`Figma`，都可以手动在画布上去拖动生成组件。

但是小松鼠并不会手搓UI，或者说审美很差~~

**所以我们需要借助AI的力量。**

**用语言描述来生成UI原型图。**

### 桌面端AI配置（可不做）

注意，这一步即使你不做，完全不影响我们后续的使用。

只是多了一种更直观的使用方式~~

在`pencil`桌面端的右上角，有一个设置按钮。

![](img/fe9bc30dff9b0b614e14ba7cc9457271.png)

2选1配置好后，就可以在左下角点开AI对话框。

![](img/c2640f0b096aab0647bb08bfbe6d0c12.png)

可以看到`pencil`天然支持`claude`模型

当你配置完成后，就可以通过文字的能力来让AI生成完整设计图。

**这里依然不是我们的重点，**

我们的目的是借助`Claude Code`来完全打通原型图和代码。

## 四、`MCP` 打通的核心能力

上述我们提到了，`pencil`安装过程中，自动配置好了AI编码工具的`MCP`

这是最重要的部分。

我们需要了解`MCP`有哪些接口，才能理解他后续能做哪些事情。

`Pencil` `MCP` 提供了 14 个 API，我把它们分成 5 类：

**查询类** → 了解当前状态

+   `get_editor_state()`获取选中元素

+   `get_screenshot()`截图验证效果

**读取类** → 理解设计稿

+   `batch_get()` 批量搜索节点

+   `get_variables()` 获取设计变量

**修改类** → 改设计稿

+   `batch_design()`**核心！增删改节点**

+   `set_variables()` 设置设计变量

**批量操作类** → 全局修改

+   `replace_all_matching_properties()` 批量替换属性（比如全局换肤）

**辅助类** → 设计灵感

+   `get_guidelines()` 获取设计规范

+   `get_style_guide()` 获取风格指南

### 核心 API：`batch_design`

支持 6 种操作：

```
I(parent, {...})  → 插入节点
C(nodeId, ...)    → 复制节点
U(nodeId, {...})  → 更新属性
R(nodeId, {...})  → 替换节点
M(nodeId, ...)    → 移动节点
D(nodeId)         → 删除节点
```

### 核心 API：`get_editor_state`

获取选中元素,这意味着，你可以点选某个组件，然后用语言组织去修改。

### 核心 API：`get_screenshot`

截图验证效果,这意味着，你可以让`Agent`自己去判断生成的设计图是否符合要求。

比如可以组建一个`Agent`团队，其中职责为设计师的`SubAgent`自己去做这件事情。

这些组合起来，就能完全控制设计稿。

## 五、两个关键`Skills`

于是，我用`find-skill`，寻找到两个技能来串联流程。

```
用find-skill，寻找pencil相关技能。解决以下问题：
如何给出美观的设计图提示词，并且风格统一。
生成原型图后，如何优雅的转化为代码直接执行。
```

### `pencil-design` 技能

作用：帮你生成设计提示词 + 调用 `MCP` 创建设计稿

工作流程：

1.  你描述想要什么界面

1.  技能帮你理清需求（可结合 `brainstorming` 做头脑风暴）

1.  调用 `MCP` `API` 创建设计

这里会放大使用者的差距，如何描述需求是一门学问。

### `ui-skills` 技能

作用：把设计稿导出成代码

它会读取设计稿节点，按 `Tailwind()` + `shadcn/ui` 规范生成 `React` 代码。

生成的不是玩具代码，是可以直接跑的。

## 六、代码导出与运行

### 导出流程

1.  告诉 `Claude` 你要导出哪个界面

1.  `Claude` 读取设计稿，分析结构

1.  自动生成项目代码

生成的代码结构：

```
src/
├── app/          # 页面
├── components/   # 组件
└── lib/          # 工具函数
```

### 运行

```
npm install && npm run dev
```

访问 `localhost:3000` 就能看到效果。

**像素级还原，不是吹的。**

而且 `Claude` 会自动加上页面跳转逻辑，不是静态页面。

## 七、修改组件的两种方式

### 方式一：在 `Pencil` 中直接改

1.  打开 `Pencil` 桌面端

1.  点选要修改的组件

1.  在右侧属性面板修改

1.  用 `MCP` 导出代码

适合：可视化调整、设计师习惯

### 方式二：在 `Claude Code` 中用自然语言改

直接告诉 `Claude`：

+   "把所有卡牌背景色改成蓝色"

+   "把界面上的英文都翻译成中文"

`Claude` 会自动找到目标节点并修改。

适合：批量修改、程序员习惯、快速迭代

两种方式改的是同一份 `.pen` 文件，可以混着用。

## 八、版本管理：可以用 `Git`

特别提一下：

`Pencil` 的文件格式是 `.pen`，本质上是 `JSON`。

![](img/faface535397e26749bbf1c8ea9955c4.png)

这意味着：

+   可以放进 `Git` 仓库

+   追踪每次设计变更

+   多人协作做版本控制

+   代码和设计稿放同一个仓库管理

传统的 `Figma` 文件是二进制格式，`Git` 很难追踪变更。

另外，`Pencil`是支持直接导入`Figma`文件的。

`Pencil` 的 `JSON` 格式解决了这个问题。

设计稿也可以像代码一样做 `Code Review` 了。

## 九、完整工作流

![](img/06c2528a3eceea34f469edece4c1fdb7.png)

> **非常适合用来**

+   快速验证想法

+   一个人既做设计又做开发

+   需要频繁迭代的原型

+   AI 辅助开发的工作流

可以把原型图的流程，作为`spec`开发的一环。

这样能够让AI自主去评审和迭代。
