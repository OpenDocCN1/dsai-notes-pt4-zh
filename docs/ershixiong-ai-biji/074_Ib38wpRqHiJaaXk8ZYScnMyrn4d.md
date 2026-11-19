
# Cursor 的进阶用法

> 来源：[https://qianniucity.feishu.cn/docx/CtttdMYdloFd5gxu487cWKBHnZf](https://qianniucity.feishu.cn/docx/CtttdMYdloFd5gxu487cWKBHnZf)

在高频使用 Cursor 一个月之后，这条推文用来记录 Cursor 的进阶用法。

## `⌘ + Enter` 选中整个代码库作为 Context

在 `⌘ + Enter` 命令中，支持代码库嵌入，以提供更好的回答。

发散一下思维，代码库也可以是文本或其他数字资产。比如这个例子，在 cursor 中打开 obsidian 笔记本，然后基于当前的笔记，进行问答。

![](img/51a7972c1b7ce93b2968bcd57e471663.png)

## `@` 符号

与 cursor 对话中，键入 `@` 可以添加多种类型的资料作为 Context。

比如，`@Files` 在对话的时候指定一个或多个文件。比如我们的代码是模块化的，有时候改动一个功能，需要跟另一个文件联动，就可以用这个方法。

`@Files` 引用某个文件

`@Folders` 引用文件夹

`@Code` 引用特定的代码片段

`@Docs` 访问外部文档

`@Git` 引用 `git` 的 commits、diffs 或 PR

`@Codebase` 跟 `⌘ + Enter` 一样，但是提供更多的配置

`@Web` 支持互联网内容搜索

## 使用 Claude 3.5 Sonet 200k

新建对话的时候可以选择 Long Context Chat 模式，但是使用时发现经常要等待，然后 `Connect Failed!`，可能是因为我不是付费用户的原因。

在对话过程中，也可以通过 `⌘ + .` 随时切换。

## Composer 生成应用骨架

Cursor 的 Composer 能力隐藏得很深，需要 `⌘ + I` 键唤起。有点类似于 https://v0.dev，可以直接生成应用骨架。

## `⌘ + K`

这个是非常频繁的功能，在与 cursor 对话生成代码之后，可以非常方便地对代码细节进行修改。

![](img/724c9a04c46df85a471cc5a12b300dba.png)

## 一个不应该公布的情报

Cursor 新账号支持 2 周的免费试用，目前搜集到的白嫖的方式有：

+ 切换邮箱账号注册：没有那么多个人邮箱怎么办，你可能知道有个东西叫一次性邮箱账号

+ 注销，然后重新注册

![](img/55ef5ba53d1e799fe5207ede264ae796.png)

## `.cursorrules`

Cursor Rules 可以在对话时提供 Context，可以添加一些编码规范，最佳实践和要求。

不知道怎么写？参考这个网站，给出了很多示例，抄一抄，改一改即可 https://cursor.directory

不同项目要支持不同的 Cursor Rules，就把 `.cursorrules` 加到项目根目录底下。

![](img/2da34b0ea0ee092f4108175fbc7e9cfc.png)

## 提交 `Commit Message`

终端也可以使用 `⌘ + K` 唤起对话，比如操作一些 `commit` 的事情。

勉强能用，但我觉得不如自动补全来得方便些。

## AI Review

Review 代码的能力目前需要在 Beta 功能中开启，然后重启应用之后，在对话框的左侧会出现一个 Review 的 Tab。

操作也简单，提供一些 Context，或者无脑点底下几个按钮。

![](img/ff73f59e5fa83a688a267bb00d649026.png)

## 如何避免 `Apply` 覆盖现有的代码

在和 Cursor 对话的时候，有个问题很难受，就是如果它给出的代码你不采纳，后续的对话很可能继续使用你未采纳的代码来覆盖你已经写好逻辑。

我当前的做法是：

+ 重启一个新对话

+ 尽可能使用 `⌘ + K` 进行对代码进行微调

## 让 Composer 帮助你一次性修改多个文件

对话不会自动帮你生成文件，但是 Composer 可以做到跨文件批量修改，在某些场景下非常有用。比如：

+ 代码重构

+ 批量替换、批量重写等等操作

## 生成 `README.md`

让 Cursor 阅读你的代码库，然后帮你生成 `README.md`，可能比自己写的要好多了。

### 索引外部链接

除了使用 `@Docs` 来索引文档外，你还可以直接在对话的时候输入链接，增加 Context。

有一些比较适合的场景：

+ API 文档链接

+ Github 仓库的具体代码实现的链接
