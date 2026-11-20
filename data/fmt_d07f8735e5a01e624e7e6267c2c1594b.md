
# 豆包 MarsCode 入门：使用 Web IDE

> 来源：[https://agijuejin.feishu.cn/docx/Q3bTdS2zpoCAC8xtdhTcfkuXnCb](https://agijuejin.feishu.cn/docx/Q3bTdS2zpoCAC8xtdhTcfkuXnCb)

> 作者：范文杰

> 链接：https://juejin.cn/post/7444564311114186789

> 来源：稀土掘金

> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

书接上篇《豆包 MarsCode 入门：初识》，接下来，我们扩展了解一下 Web IDE 插件。

## Step1：启动项目

除了 IDE 插件外，为方便学习试用，豆包 MarsCode 还提供了 Web IDE 形态的产品入口，无需任何配置，进入 `Dashboard(www.marscode.cn/dashboard)` 页面即可一键开启试用。

不过，Web IDE 目前还不支持导入本地项目，首次进入需要创建新项目或导入 `git` 项目，出于教学目的，此处推荐创建一个 `HTML/CSS/JS` 模板项目：

![](img/ab5b0175b3b34f9d52a1c3baf8ba2e2c.png)

![](img/189c561c36f08a543eeb5388c466dda0.png)

## Step2：界面布局

进入 Web IDE 后，整体功能布局与 VS Code 非常接近：

+ 左上角菜单，其中的 M 字形 logo 与 VS Code 的各菜单项相对应，您可以在此处进行一系列的操作和设置。比如，您能够通过点击相关菜单项来打开或关闭特定的功能模块，调整软件的界面布局，访问插件管理页面以安装或卸载所需的插件，还能对代码编辑的一些基本参数进行个性化的配置。

![](img/7a0b30846f825c6f807944941e9c6698.png)

+ 左侧文件目录视图，以清晰直观的方式展示该项目下所有文件与目录的详细内容。通过层次化的结构呈现，用户可以轻松地浏览和定位所需的文件或目录。每个文件和目录都有对应的图标和名称，一目了然。并且，用户可以通过点击目录展开或收缩其下的子目录和文件，方便进行查看和操作。

![](img/90ac2441c7b733c6a021fac4dbfa4095.png)

+ 中间区域为编码区域，可在此编辑代码文件等

![](img/d53e371605fb38252343e9fc156b68bd.png)

+ 下边是 `terminal` 交互面板，用户可在此执行各类命令行命令，例如启动调试项目等

右边菜单栏比较重要，各项非常重要的交互入口，自上而下分别是：

+ `AI Assistant` 是主要交互入口，在这里可以问各种问题，整体交互上与 VS Code 相似，也包含 `generate`、`test` 等能力

![](img/b49cf7ff3b9b3add95fd9a7f1fc32ede.png)

+ 搜索、分支管理跟平常 VS Code 差不多，这里不着重介绍；

+ `Webview` 是个亮点，豆包 MarsCode 本质上是 cloud ide，因此 `terminal` 中启动的各类应用都不在本地执行，而是挂靠在 cloud 环境中，因此无法使用 `http://localhost:xxx` 方式访问，需要 豆包 MarsCode 做一层代理过去，在后续章节中我们会展开讨论；

![](img/e541ba295e46c0599996904c6aea7b13.png)

+ 扩展：在此可搜索到许多可在 豆包 MarsCode 上使用的插件，整体功能理论上与 VS Code 兼容，所以可以复用各类 VS Code 插件；

不过，注意 Web IDE 有相对宽松的容量限制（`docs.marscode.cn/docs/limitations` 等都有要求，使用时需要注意不可超出限制量。

## 与 VS Code 插件的差异

Web IDE 整体功能与 VS Code 插件趋近，同样提供了与 AI 对话的对话面板，同样支持生成代码、解释代码等能力，此处不再赘述。

与 VS Code 比较大的差异在于，在 Web IDE 中使用 `ctrl + i` 快捷键可打开行内交互面板：

![](img/474555857a5c02e65bc83b232ee88066.png)

我们可在此，就特定文件的内容与 LLM 做交互。其次，Web IDE 不需要设置认可本地环境，不需要提前安装其他软件，只需要有适当的浏览器即可开始使用 豆包 MarsCode，非常适合入门试用。
