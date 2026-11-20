
# Cursor `code`/`cursor` 命令

> 来源：https://v11enp9ok1h.feishu.cn/docx/S8jldeFvPov355xHnHlcTIW9nRe

Cursor 是一款基于 Visual Studio Code (VS Code) 的现代化代码编辑器，所以它继承了 VS Code 的强大功能，如本文要介绍的 `code` 命令。

在 Visual Studio Code 中，`code` 或 `cursor` 命令是一个命令行工具，用于快速打开 VS Code 并执行一些与代码相关的操作。

`code` 命令直接可以帮助开发者从终端或命令提示符中直接启动 VS Code 或处理特定的任务。

最常用的方式就是使用 `code` 或 `cursor` 命令直接从命令行中打开文件目录，此时需要先安装 `code` 命令。

> Cursor 支持将终端启动命令设置为 `code` 或 `cursor`，以便快速启动编辑器，如果本地已经安装了 VS Code，`code` 命令一般用在 VS Code 中，Cursor 使用 `cursor` 命令。

## 安装命令

启用 Cursor 的 `code` 命令非常简单，先打开命令面板：

+ macOS 系统快捷键：⇧⌘P（command + shift + p）

+ Windows/Linux 快捷键: Ctrl + Shift + P

搜索安装 `shell command`：

![](img/109f2e8c9c70817c0cc2c471cafda61a.png)

然后选择 在 `PATH` 中 安装 `cursor` 命令 - Shell Command：Install `cursor` command in `PATH` 即可为系统 `PATH` 路径添加了 `cursor` 命令的引用。

我们可以通过命令行打开文件、安装扩展、修改显示语言，甚至查看诊断信息。

通过 `cursor --version` 查看版本信息：

```
# cursor --version
0.45.14
906121b8c0bdf041c14a15dac228e66ab5505260
arm64
```

## 常用命令行选项

以下是一些常用的命令行选项，可以通过 `cursor --help` 命令查看：

```
Cursor 0.45.14

Usage: cursor [options][paths...]

To read from stdin, append &#x27;-&#x27; (e.g. &#x27;ps aux | grep code | cursor -&#x27;)

Options
  -d --diff &lt;file&gt; &lt;file&gt;                    Compare two files with each
                                             other.
  -m --merge &lt;path1&gt; &lt;path2&gt; &lt;base&gt; &lt;result&gt; Perform a three-way merge by
                                             providing paths for two modified
                                             versions of a file, the common
                                             origin of both modified versions
                                             and the output file to save merge
                                             results.
  -a --add &lt;folder&gt;                          Add folder(s) to the last active
                                             window.
  -g --goto &lt;file:line[:character]&gt;          Open a file at the path on the
                                             specified line and character
                                             position.
  -n --new-window                            Force to open a new window.
  -r --reuse-window                          Force to open a file or folder in
                                             an already opened window.
  -w --wait                                  Wait for the files to be closed
                                             before returning.
  --locale &lt;locale&gt;                          The locale to use (e.g. en-US or
                                             zh-TW).
  --user-data-dir &lt;dir&gt;                      Specifies the directory that user
                                             data is kept in. Can be used to
                                             open multiple distinct instances
                                             of Code.
  --profile &lt;profileName&gt;                    Opens the provided folder or
                                             workspace with the given profile
                                             and associates the profile with
                                             the workspace. If the profile
                                             does not exist, a new empty one
                                             is created.
  -h --help                                  Print usage.
```

我们可以在命令行中使用 `cursor .` 命令让文件夹在 Cursor 中打开：

![](img/4ecc8f28b782b23448e12e4f863516e7.png)
