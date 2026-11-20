
### 多次跳转后在导航历史中前进和后退

在 Mac 系统中使用 `Cmd+[` / `Cmd+]` 键，在 Windows 或 Linux 系统中使用 `Ctrl+Alt+Left` / `Ctrl+Alt+Right` 键。

更多细节，参见官方文档：https://www.jetbrains.com/help/pycharm/tutorial-exploring-navigation-and-search.html。

## PyCharm 中的版本控制

版本控制系统（如 `Git` 和 `Mercurial`）是现代软件开发世界中最重要的工具之一。因此，IDE 必须支持版本控制。`PyCharm` 在这方面做得很好，它集成了大量流行的版本控制系统，如 `Git`（和 `Github` (https://github.com/)）、`Mercurial`、`Perforce` 和 `Subversion`。注：以下示例中使用的版本控制系统为 `Git`。

### 配置版本控制系统（`VCS`）

要想实现 `VCS` 集成，你需要在顶部菜单点击 `VCS → VCS Operations Popup…`，或者在 Mac 系统中使用 `Ctrl+V` 键，在 Windows 或 Linux 系统中使用 `` `Alt+`` ` `` 键。选择 `Enable Version Control Integration…`，你将看到以下窗口：

![](img/e1ed6c85cba6619b2efbf481ef3c22d7.jpg)

从下拉列表中选择 `Git`，点击 `OK`，这样你就为项目设置好了 `VCS`。（注意，如果你打开的已有项目已经具备版本控制系统，`PyCharm` 将会发现并自动使用该版本控制系统。）这时如果你前往 `VCS Operations Popup…`，你会发现一个不同的弹窗，它具备选项 `git add`、`git stash`、`git branch`、`git commit`、`git push` 等等：

![](img/4b39f4d78c6ef1b89ab9e826220c328c.jpg)

如果你找不到所需要的选项，你可以在顶部菜单中点击 `VCS`，选择 `Git`，在这里你可以创建和查看 `pull request`。

### 提交和冲突处理

这是 `PyCharm` 中 VCS 集成的两大特征，我个人经常使用并且非常喜欢。假如你完成了工作，打算提交，前往 `VCS → VCS Operations Popup… → Commit…`，或者在 Mac 系统中使用 `Cmd+K` 键，在 Windows 或 Linux 系统中使用 `Ctrl+K` 键。你将看到如下窗口：

![](img/2b7710d2d270593ccef157bb391cfa30.jpg)

在该窗口中，你可以：

*   选择要提交的文件
*   写下提交信息
*   在提交前执行各项检查
*   查看更改
*   点击右下角 `Commit` 按钮旁边的箭头，选择 `Commit and Push…`，从而一次性完成提交和 push。

是不是感觉很神奇很迅速？特别是如果你以前经常通过命令行手动执行这些任务时。团队合作中会出现合并冲突（`merge conflict`）。当一个人对你正在处理的文件提交更改时，你们二人更改了同一行导致更改重叠，这时 VCS 无法决定选择你的更改还是队友的更改。那么你可以使用以下箭头和符号来解决这个问题：

![](img/f360bda93dc8fb8a932e2af240af1207.jpg)

看起来很奇怪，我们很难分辨应该删除哪些更改、保留哪些更改。不要怕，`PyCharm` 来了！它可以用更好、更简洁的方法解决冲突。在顶部菜单中前往 `VCS`，选择 `Git`，然后选择 `Resolve conflicts…`。选择存在冲突的文件，点击 `Merge`，出现以下窗口：

![](img/16ee612964d386499a3138ffbd86d19e.jpg)

在左侧列中，你可以查看自己做的更改。在右侧列中，可以查看队友做的更改。而中间列则显示结果。存在冲突的代码行被高亮显示，你可以在它们旁边看到 `X` 和 `>>`/`<<`。点击箭头表示接受更改，点击 `X` 则表示拒绝更改。解决所有冲突后，点击 `Apply` 按钮：

![](img/83fcb85c209c21dcfc9315416720e8ba.jpg)

在上图中，对于第一个冲突行，作者选择拒绝自己的更改，接受队友的更改。而在第二个冲突行中，作者接受了自己的更改，拒绝了队友的更改。使用 `PyCharm` 中的 VCS 集成还可以执行很多操作。详情参见 https://www.jetbrains.com/help/pycharm/version-control-integration.html。

## 在 PyCharm 中使用插件和外部工具

在 `PyCharm` 中你可以找到开发所需的几乎所有功能。如果没找到，那么很可能存在一个插件，向 `PyCharm` 提供你需要的功能。例如，它们可以：

*   添加多语言和多框架支持
*   使用快捷键提示（`shortcut hint`）、文件监视器（`file watcher`）等提升你的生产效率
*   利用代码练习，帮助你学习新的编程语言

例如，`IdeaVim` 插件向 `PyCharm` 添加 Vim 模拟。如果你喜欢 Vim，这个插件可以实现不错的结合。`Material Theme UI` 插件可将 `PyCharm` 的外观改变为 Material Design 的外观：

![](img/6a53d56d4733782b9f02f95803af74cc.jpg)

`Vue.js` 插件使 `PyCharm` 支持 `Vue.js` 项目。`Markdown` 插件使得在 IDE 内可以编辑 `Markdown` 文件，并实时预览渲染后的 HTML。在 Mac 系统上点击 `Preferences → Plugins`，在 Windows 或 Linux 系统中点击 `Settings → Plugins`，你可以在 `Marketplace` 标签下找到和安装所有可用插件：

![](img/02e40b5031ea9602cedc77959e20971a.jpg)

如果仍然没找到所需插件，你甚至可以自己开发一个。如果你找不到合适的插件，又不想自己开发，因为 `PyPI` 上有可用的包，你可以将这个包作为外部工具添加到 `PyCharm`。以代码分析器 `Flake8` 为例。首先，在所选 `Terminal app` 中键入 `pip install flake8`，从而在虚拟环境中安装 `Flake8`。或者也可以使用 `PyCharm` 集成的 `Terminal`：

![](img/3bc5d0b97854dd97383af419a5a9f77e.jpg)

在 Mac 系统上点击 `Preferences → Tools`，在 Windows 或 Linux 系统中点击 `Settings → Tools`，选择 `External Tools`。然后点击底部 (1) 处的 + 按钮。在弹出的窗口中，输入细节并在两个窗口中点击 `OK`，如下图所示：

![](img/692c3077ef735719411985d24217a739.jpg)

上图中，`Program (2)` 指 `Flake8`，你可以在虚拟环境文件夹（`bin`）中找到它。`Arguments (3)` 表示你想用 `Flake8` 分析的文件。`Working directory` 表示项目目录。你可以把这里所有项的绝对路径写死，但这就意味着你无法在其他项目中使用该外部工具，只能在一个项目中针对一个文件使用该工具。因此你需要使用 `Macros`。它是`$name$`格式的变量，根据语境而变化。例如，当你编辑 `first.py` 时，`$FileName$` 为 `first.py`，当你编辑 `second.py` 时，`$FileName$` 为 `second.py`。你可以查看它们的列表，点击 `Insert Macro…` 按钮将其中一个插入。此处你使用了 `macros`，它们的值会根据你目前处理的项目而改变，`Flake8` 将继续准确执行其工作。要想使用它，你需要创建文件 `example.py`，并在其中写入以下代码：

![](img/6e784fa8d069bcdd261c7c1fa6ba2513.jpg)

上述代码故意破坏了 `Flake8` 的一些规则。右键单击文件背景，选择 `External Tools → Flake8`。`Flake8` 分析结果将出现在窗口底部：

![](img/651236d9e56716b9c3b036163dbf6405.jpg)

为了使效果更好，你可以为其添加快捷键。在 Mac 系统中选择 `Preferences`，在 Windows 或 Linux 系统中选择 `Settings`。然后，点击 `Keymap → External Tools → External Tools`。双击 `Flake8`，选择 `Add Keyboard Shortcut`，出现以下窗口：

![](img/47d2ce745815006d1bda5799e014e0d4.jpg)

上图中，快捷键是 `Ctrl+Alt+A`（本教程使用该快捷键）。你可以在文本框中添加喜欢的快捷键，然后在两个窗口中点击 `OK`。然后，你就可以用该快捷键，在 `Flake8` 的帮助下分析目前在处理的文件了。

## PyCharm Professional 功能

`PyCharm Professional` 是 `PyCharm` 的付费版本，具备更多开箱即用的功能和集成。这部分将概览其主要功能，以及官方文档链接（其中详细介绍了每一项功能）。记住，以下功能在 `PyCharm Community` 版本中均不可用。

### Django 支持

`Django` 是最流行和最受喜爱的 Python web 框架，`PyCharm` 对 `Django` 提供广泛的支持。要确保对 `Django` 的支持，需要执行以下步骤：

1.  在 Mac 系统中打开 `Preferences`，在 Windows 或 Linux 系统中打开 `Settings`。
2.  选择 `Languages and Frameworks`。
3.  选择 `Django`。
4.  检查复选框 `Enable Django support`。
5.  应用更改。

现在确保了对 `Django` 的支持，你在 `PyCharm` 中的 `Django` 开发之旅将轻松很多。具体而言在创建项目时，你会得到一个专用的 `Django` 项目类型。这表示，当你选择该类型时，你将拥有所有必要文件和设置。这等同于使用 `django-admin startproject mysite`。你也可以在 `PyCharm` 内直接运行 `manage.py` 命令。目前支持的 `Django` 模板，包括以下一些：

*   语法和错误高亮显示
*   代码补全
*   导航
*   `block` 名称补全
*   自定义标签和过滤器补全
*   标签和过滤器的快速文档
*   模板 debug 能力

除此之外，我们还可以在其他 `Django` 部分（如视图、URL 和模型）中执行代码补全、对 `Django ORM` 提供代码追踪支持（`code insight support`）、对 `Django` 模型提供模型依赖项关系图。更多细节，参见官方文档：https://www.jetbrains.com/help/pycharm/django-support7.html。

### 数据库支持

现代数据库开发是一个复杂的任务，需要多个支持系统和工作流。这也是 `JetBrains` 开发独立 IDE `DataGrip` 的原因。`DataGrip` 是独立于 `PyCharm` 的产品，二者的应用场景和授权都不相同。但幸运的是，通过 `Database tools and SQL` 插件（该插件默认开启），`PyCharm` 可以支持 `DataGrip` 中的所有特性。在该插件的帮助下，你可以查询、创建和管理数据库，不管数据库在本地、服务器，还是在云端。该插件支持 `MySQL`、`PostgreSQL`、`Microsoft SQL Server`、`SQLite`、`MariaDB`、`Oracle`、`Apache Cassandra` 等。关于该插件的更多用途，请查看文档：https://www.jetbrains.com/help/pycharm/relational-databases.html。

### 线程并发可视化（`Thread Concurrency Visualization`）

`Django Channels`、`asyncio` 和近期框架（如 `Starlette` (https://www.starlette.io/)）表明异步 Python 编程正逐渐成为趋势。异步编程具备很多好处，但很难写，也很难 debug。在此类案例中，`Thread Concurrency Visualization` 就是医生，帮助你全面管理多线程应用并进行优化。更多细节，参见文档：https://www.jetbrains.com/help/pycharm/thread-concurrency-visualization.html。

### `Profiler`

说到优化，`profiling` 是另一种代码优化方法。`profiling` 可以帮助你查看代码的哪一部分占用了最多的执行时间。`profiler` 运行的优先级如下：1. `vmprof` 2. `yappi` 3. `cProfile` 如果你没有安装 `vmprof` 或 `yappi`，则运行标准 `cProfile`。更多细节，参见：https://www.jetbrains.com/help/pycharm/profiler.html。

### 科学模式

Python 不仅是通用和 web 编程语言，由于 `NumPy`、`SciPy`、`scikit-learn`、`Matplotlib`、`Jupyter` 等库和工具的加持，Python 成为数据科学和机器学习领域的最优工具。有了这些强大工具，你还需要一个强大的 IDE 来支持这些库所具备的绘图、分析等所有功能。关于科学模式的更多详情，参见 https://www.jetbrains.com/help/pycharm/matplotlib-support.html。

### 远程开发

很多应用出现 bug 的一个常见原因是，开发环境和生产环境不同。尽管在大多数情况下，开发时完美复制生产环境并不现实，但力求实现完美复刻是值得追寻的目标。在 `PyCharm` 的帮助下，你可以使用另一台计算机（如 `Linux VM`）上的解释器对应用进行 debug。这样，你就可以拥有与生产环境一样的解释器了，从而避免很多因开发环境和生产环境差异导致的 bug。详情参见：https://www.jetbrains.com/help/pycharm/remote-debugging-with-product.html。

### 结论

`PyCharm` 是最好的 Python 开发 IDE 之一。它提供大量优势，帮助执行例行任务，从而节约大量时间。学完本教程，现在你知道如何利用 `PyCharm` 提高生产效率了吗？

*原文地址：https://realpython.com/pycharm-guide/*

本文为机器之心编译，**转载请联系本👸号获得授权**。

✄------------------------------------------------
**加入机器之心（全职记者 / 实习生）：`hr@jiqizhixin.com`**
**投稿或寻求报道：**`content`@jiqizhixin.com`
**广告 & 商务合作：**`bd`@jiqizhixin.com`
