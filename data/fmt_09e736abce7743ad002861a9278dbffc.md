
# 全新 AI IDE：Trae，Claude 免费用

> 来源：[https://agijuejin.feishu.cn/docx/WESidWNTgotRBmxvbN7czjOznMY](https://agijuejin.feishu.cn/docx/WESidWNTgotRBmxvbN7czjOznMY)

> 作者：五月君

> 链接：https://juejin.cn/post/7461968661012873250

> 来源：稀土掘金

> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

> 如有侵权，请联系删除。

![](img/e0714bed9cc95cd2af56764f03efb518.png)

字节的 AI 产品是真的高产！最近，字节跳动又发布了一款全新的 AI 编程 IDE：Trae。从此 Cursor 又多了一个竞争对手。

目前网红大模型 Claude-3.5、和 OpenAI 的 GPT-4o 模型通通免费使用！

先上视频链接，上面有 Demo，可以看下！

[ 👉 视频点这里](https://link.juejin.cn?target=https%3A%2F%2Fwww.xiaohongshu.com%2Fexplore%2F678e46c00000000028035384%3Fxsec_token%3DABdN0IDRg0zOl13rAkVyl8IuLkhKVwBvKLOpsbpY38P3E%3D%26xsec_source%3Dpc_user)

AI智见录，今天也体验了下，还可以，目前主要的功能有AI问答（Chat模式）、类似 Cursor 的 Agent 支持一键生成项目（Builder 模式）、内联聊天、代码补全，基本上 AI辅助编程工具该有的都有了。

Trae 也是基于 VS Code 魔改，用户习惯如果从 VS Code 过来的朋友们不用太担心，差别不大，很快就能入手。

## 安装

打开网站 [www.trae.ai/](https://www.trae.ai/?utm_source=juejin&utm_medium=juejin_trae&utm_campaign=techcall)，点击 Download 按钮下载，目前仅支持 Mac。

![](img/a8e5b8dbd5d0f7c7670e119ba2b71a81.png)

选择主题，语言选择中文，中文开发者非常友好

![](img/f8a373e59abd9ed25bfbee90519683e4.png)

支持从 VS Code、Cursor 中一键导入 IDE 配置，看来是直冲 Cursor 来的哈 😄

接下来需要登陆，用 Google 或 Github 都可以。

![](img/43bab02139a559ec439370b0064c9fd1.png)

## 两种模式

使用 `Command + U` 命令打开和 AI 对话窗口，有两个模式 Chat、Builder

### Chat 模式

Chat 模式更像一个 AI 助手，可以帮助我们回答一些问题，生成的代码不会直接写入文件，需要手动应用。

Chat 模式支持 4 种引用：代码、文件、文件夹、工作区

### Builder 模式

Builder 模式可以一键完成项目搭建，对文件的任何改动也都会自动保存。更像 Cursor 中的 Composer Agent 模式。还有 Windsurf Cascade Write 模式。（名字不一样，功能类似）

还支持设计图或截图直接转位代码。

![](img/19f660c6eef75fe33d5bc184d9d6a0d4.png)

做了一个 Demo，让 Trae 帮我实现一个 Todo List，从项目搭建到完成，全程也是没写一行代码

![](img/d52072cedb1fb7d0feac556250b2ffbd.png)

以下是生成的效果（浏览器端打开）。

![](img/4cf6581fc0c34f0687e0355b22eb1a6e.png)

还支持在 `Webview` 内打开，不用离开编辑器窗口

![](img/1dffcc7fcd7f6c27f02d6e33890f0e7f.png)

例如，这个前端项目，默认用的技术栈是 React + Vite，最开始我是让 Trae 用 Next.js 技术栈，试了好几次、好几个 Demo 都通过不了。Next.js 技术栈很不友好，这里先替你们踩坑

![](img/9dbabdf750c2bc72936d9afc2a60d2f9.png)

## 终端错误到聊天窗口

终端报错信息，支持一键添加到对话框，免去了复制/粘贴。

![](img/f2785c8a9cf50be827908bdceae8e52f.png)

## 内联聊天

通过 `Command + i` 命令，唤醒内联聊天

![](img/54250b6e5d6c62a488a04bdea20d7564.png)

[ 👉 视频点这里](https://link.juejin.cn?target=https%3A%2F%2Fwww.xiaohongshu.com%2Fexplore%2F678e46c00000000028035384%3Fxsec_token%3DABdN0IDRg0zOl13rAkVyl8IuLkhKVwBvKLOpsbpY38P3E%3D%26xsec_source%3Dpc_user)

## 总结

AI IDE，国外的 Cursor、Windsurf 近期很火，国内能出一个类似的 AI IDE 也挺好的，Trae 中文开发者特别友好。有竞争总比一家独大的好，这样用户才能受益，目前免费使用 Claude 3.5 Sonnet、GPT4o，想尝试的可以试下哦！
