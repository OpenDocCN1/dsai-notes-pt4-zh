
# 港大神器，让AI一条命令操控桌面软件！

> 来源：[https://my.feishu.cn/docx/IbMhdHST6oqxgmxrVR5cRDQ1n7c](https://my.feishu.cn/docx/IbMhdHST6oqxgmxrVR5cRDQ1n7c)

你有没有遇到过这种情况：

让AI帮你做个视频剪辑，或者批量处理一堆图片。

结果AI的操作方式是：截屏，识别按钮位置，模拟鼠标点击。

稍微换个分辨率就崩了。

窗口弹出来挡住了关键按钮也崩了。

软件更新了界面也崩了。

老金我之前用各种Agent操控桌面软件，踩的最大的坑就是这个。

GUI自动化太脆了。

截图识别是能用，但用过的人都知道那种一言难尽的感觉。。。

上周刷GitHub Trending的时候，看到一个港大的项目。

名字很直白：`CLI-Anything`。

口号更直白：Making ALL Software Agent-Native。

翻译成人话就是：一条命令，把任何软件变成AI能直接操控的命令行工具。

它的Star在猛增！昨儿还是6500，今天就9500了！

![](img/1188ab9d297fffc52a003e9ebab43fd1.png)

这与老金之前去看黑客松冠军的Everything claude code时候的万物皆可Cli化的想法不谋而合。

难得的是，他们真的做出来了！

![](img/dbcf35a0430fef51f7b826a0fe3423e5.png)

## 它到底是什么

`CLI-Anything` 是港大数据智能实验室（`HKUDS`）开源的一个项目。

![](img/29959ef733ca0b5d193e56f0a48eaea8.png)

这个实验室你可能没听过名字，但他们的项目你大概率见过。

`LightRAG`，29000+星。

`nanobot`，32000+星。

`RAG-Anything`，14000+星。

说白了就是港大搞AI研究的一个王牌实验室。

CLI-Anything要解决的问题很简单。

`让AI Agent不通过GUI截图点击`，而是通过命令行直接操控桌面软件。

你指给它任何一个软件的源代码，它会自动跑一个7阶段的流水线。

最后生成一套完整的命令行工具。

生成出来的CLI有几个特点：

每条命令都带 `--json` 参数

AI可以直接解析结构化数据，不用再OCR截图猜内容。

### 双模式运行

`REPL`模式，就是交互式对话，像聊天一样一步步操作。

子命令模式，一条命令直接搞定。

### 带撤销/重做

状态持久化，操作可以回退。

### 生成真实文件

`LibreOffice`生成`PDF`，`Blender`渲染3D场景，`Audacity`处理音频。

不是模拟，是真的调用了底层软件。

### 7阶段流水线怎么跑的

这是整个项目最核心的技术。

你把一个软件的源代码丢给它，它自动走7步：

1、分析软件架构和数据模型

2、设计CLI命令结构，把GUI操作映射成命令

3、实现核心模块，处理状态管理

4、写单元测试

5、跑端到端测试

6、验证生成的CLI符合标准

7、打包发布，`pip install`直接用

![](img/36a495e4445c6b5eec5f9b99605966f6.png)

整个过程全自动。

你只需要一条命令：

```
/cli-anything ./gimp
```

等它跑完，你就有了一个完整的`GIMP`命令行工具。

老金我看到这个流程的时候，第一反应是：这也太暴力了吧。。。

直接把GUI软件的源代码当输入，自动生成CLI。

不是调`API`，不是截图识别，是直接从源代码层面理解这个软件能做什么。

## 11款软件全部跑通，超1500个测试零失败

这是让老金我从怀疑变成真香的关键数据。

### 创意工具

![](img/b3a9b31f692cc3e26b9c7db7be75269b.png)

1,508个测试（1,011单元测试 + 425端到端测试），100%通过率。

这个数据量说明什么？

说明这不是PPT项目。

每个软件都有大量的单元测试和端到端测试验证，真的能跑。

而且生成的不是简化版的CLI。

`LibreOffice`能操控`Writer`文档、`Calc`表格、`Impress`幻灯片。

`OBS Studio`能控制流媒体场景。

`Stable Diffusion`、`ComfyUI`这些AI绘图工具也能通过命令行操控。

所有生成的文件都是真实格式：`ODF`、`MLT XML`、`SVG`。

这一点很重要。

它不是在模拟软件的行为，而是真的在调用软件。

## 怎么装怎么用

### 方式一：Claude Code插件（推荐）

如果你在用`Claude Code`（Anthropic的AI编程工具），这是最简单的方式：

添加市场：

```
# Add the CLI-Anything marketplace
/plugin marketplace add HKUDS/CLI-Anything
```

安装插件

```
# Install the cli-anything plugin from the marketplace
/plugin install cli-anything
```

用一条命令构建 CLI

```
# /cli-anything:cli-anything <software-path-or-repo>
# Generate a complete CLI for GIMP (all 7 phases)
/cli-anything:cli-anything ./gimp

# Note: If your Claude Code is under 2.x, use "/cli-anything" instead.
```

完善和改进 CLI

```
# Broad refinement — agent analyzes gaps across all capabilities
/cli-anything:refine ./gimp

# Focused refinement — target a specific functionality area
/cli-anything:refine ./gimp "I want more CLIs on image batch processing and filters"
```

生成的命令行界面

```
# Install any generated CLI
cd <software>/agent-harness
pip install -e .

# Verify
which cli-anything-<software>

# Use
cli-anything-<software> --help
cli-anything-<software>                    # enters REPL
cli-anything-<software> --json <command>   # JSON output for agents
```

运行测试

```
# Run tests for a specific CLI
cd <software>/agent-harness
python3 -m pytest cli_anything/<software>/tests/ -v

# Force-installed mode (recommended for validation)
CLI_ANYTHING_FORCE_INSTALLED=1 python3 -m pytest cli_anything/<software>/tests/ -v -s
```

### 方式二：手动安装插件

```
git clone https://github.com/HKUDS/CLI-Anything.git
cp -r CLI-Anything/cli-anything-plugin ~/.claude/plugins/cli-anything
```

在`Claude Code`里跑一下 `/reload-plugins` 就行。

### 生成的CLI怎么用

不管用哪种方式装好插件，指向任何软件源码就能生成CLI。

生成完成后，进到对应目录跑 `pip install -e .`，CLI就直接上PATH了。

```
cli-anything-gimp --help
```

这样就能看到`GIMP`的所有可用命令。

## 为什么CLI流比截图流好

老金我帮你理一下这两种方式的本质区别。

### 截图流（传统GUI自动化）

AI截屏，OCR识别界面元素，计算坐标，模拟鼠标点击。

问题：依赖分辨率、窗口位置、界面版本，任何一个变了就崩。

### CLI流（CLI-Anything的方式）

AI发命令，CLI调用软件底层功能，返回结构化`JSON`结果。

优势：不依赖界面，不怕分辨率变化，输出可直接解析。

![](img/c760b30508711eb9f7590fc232b4418c.png)

打个比方。

截图流就像你雇了个人远程桌面帮你操作软件，他看得到什么取决于屏幕显示。

CLI流就像你直接给软件装了个遥控器，按哪个键干什么事，清清楚楚。

遥控器当然比远程桌面稳定一万倍。

举个实际的例子：

```
cli-anything-blender --json scene list
```

返回的是结构化的场景列表`JSON`，`Agent`直接拿来用。

不是一张截图让AI去猜这里有几个场景。

这个 `--json` 的设计思路很聪明。

人用的时候看到的是格式化的表格，机器用的时候拿到的是结构化数据。

一套CLI，人机通吃。

## 它的边界在哪

说了好的，也得说说限制。

![](img/596f30248016e4e0105ed715874fc98c.png)

### 需要真实软件安装在本地

`CLI-Anything`不是模拟器。

你要操控`Blender`，本地就得装`Blender`。

没装的话测试直接失败，不会假装通过。

这是它的设计哲学：零妥协，真实软件是硬性要求。

### 只能处理有源代码的软件

闭源的商业软件没法用。

`Photoshop`、`Final Cut Pro`这些，因为拿不到源代码。

这也意味着它天然偏向开源生态。

### 不是所有软件都能一把过

老金我翻了一下`Issue`区，有人用它包装`gedit`失败了。

说明复杂度低或者架构特殊的软件，7阶段管线可能不适配。

目前跑通的11款都是成熟的大型开源软件，代码结构规范。

另外第一次生成可能覆盖不全，官方建议跑 `/refine` 命令多迭代几次提升覆盖率。

### 还在快速增长阶段

9500+星说明社区高度认可，增长速度很猛。

但相比同实验室的`LightRAG`（29000星），`CLI-Anything`还是个年轻项目。

`API`可能会变，文档可能不全，踩坑的心理准备要有。。。

## 这件事意味着什么

往大了说，`CLI-Anything`代表的是一个方向转变。

AI `Agent`与软件的交互方式，从`GUI`自动化转向`CLI`原生。

以前你要让AI操控一个软件，要么写专门的`API`接口（没几个软件有）。

要么搞`GUI`自动化（截图点击那一套）。

现在有了第三条路：自动把任何软件变成CLI。

对AI `Agent`生态来说，这意味着理论上任何开源软件都可以变成AI的工具。

不需要软件开发者专门适配。

你有源代码就够了！

想象一下这个场景。

AI Agent一条命令让`Blender`渲染一个3D场景。

再一条命令让`Stable Diffusion`生成一张图。

然后让`Inkscape`导出`SVG`。

最后一条命令让`LibreOffice`生成报告。

全程没有一张截图。

全程稳定可靠。

全程结构化数据交互。

这才是`Agent`操控软件该有的样子。

## 老金我的建议

`Claude Code`用户直接试

两行命令的事。

装个插件，体验一下AI操控`Blender`是什么感觉。

不好用再卸，没什么成本。

### 有批量自动化需求的

如果你经常需要批量处理图片、视频、文档，这个工具的思路值得关注。

比截图自动化稳定太多了。

### 纯观望的

如果你不用这些桌面软件，或者没有自动化需求。

先关注着就行。

这个项目还在快速迭代，等生态更成熟了再上手也来得及。

说到底，让AI操控软件这件事，截图流终究是个过渡方案。

说白了就是用一个很笨的方式（看屏幕猜按钮）去做一件本该很聪明的事。

直接发指令就完了，为什么要绕一圈截图？

CLI原生才是正道。

`CLI-Anything`可能不是最终形态，但方向是对的。

你们觉得呢？用AI操控桌面软件，你们踩过什么坑？评论区聊聊。

GitHub仓库：https://github.com/HKUDS/CLI-Anything

官方主页：https://clianything.org/

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>
