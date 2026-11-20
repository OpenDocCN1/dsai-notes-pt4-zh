
Play with Docker 是一个 Docker 学习平台，可以在浏览器上免费体验 Alpine Linux 虚拟机。不过需要登录和注册，而且每次登录创建的实例都有时间限制，到时间还需要重新登录。

你可以在 Play with Docker 中构建和运行 Docker 容器，提升自己操作 Docker 的能力（默认已搭建好环境），甚至在 Docker Swarm 模式下创建集群，`Docker-in-Docker` (`DinD`) 用于提供多个 VM/PC 的效果。

地址：https://github.com/play-with-docker/play-with-docker

# 16 开发框架

## 16.1 Java 应用程序开发平台

JHipster 是一个全栈的 Java 应用程序开发平台，可用来创建基于 Maven+Spring+AngularJS 的项目，提供完全热加载的 Java 和 JavaScript 代码。它还提供了包含 CLI 工具在内的许多组件，用于处理多种技术栈的脚手架生成，比如用 Vue.js UI 为前端 MongoDB 作为后端，以 React 为前端的 Postgres 等等...

地址：https://github.com/jhipster/generator-jhipster

## 16.2 Java Web 框架

Vaadin 是一个可在 Java 中实现 Web 用户界面的 Web 框架，无需编写任何 HTML 或 JavaScript 代码。它包含一个服务器端，主要的应用逻辑都在服务器端运行，然后浏览器端通过 Ajax 技术跟服务器端进行交互。

地址：https://github.com/vaadin/platform

# 17 AI Debug

https://github.com/shobrook/adrenaline

# 18 NVM

控制不同版本的 nodejs

要使用 `nvm`（Node Version Manager）管理多个 Node.js 环境，请按照以下步骤进行操作：

1. 安装 `nvm`：在终端中运行以下命令安装 `nvm`：

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
```

2. 安装 Node.js：使用 `nvm` 安装所需的 Node.js 版本，例如：

```
nvm install 14.17.5
```

3. 使用 Node.js：切换到所需的 Node.js 版本，例如：

```
nvm use 14.17.5
```

4. 管理 Node.js：您可以使用 `nvm` 安装其他 Node.js 版本、卸载 Node.js 版本和列出已安装的版本，例如：

```
nvm install 16.13.0 // 安装 Node.js 16.13.0 版本
nvm uninstall 14.17.5 // 卸载 Node.js 14.17.5 版本
nvm ls // 列出已安装的所有 Node.js 版本
```

注意：在每个新的终端窗口中，您需要运行 `nvm use` 以使用所需的 Node.js 版本。

# 19 VSC 快捷键

`cmd+K+I` 光标移到变量上，可以显示属性

# 20 脚手架

脚手架是一种快速构建项目的工具，它可以帮助开发人员自动生成一些项目骨架代码，并为开发人员提供了一些预定义的代码结构，从而使开发人员不用从头开始写代码。脚手架可以大大缩短开发时间，提高开发效率。

# 21 tmux

https://github.com/theniceboy/.config/blob/master/.tmux.conf

https://tmuxcheatsheet.com/

# 22 neovim

`Lazyvim`

https://www.lazyvim.org/configuration/examples

+   `空格`：提示

+   `E`：NeoTree

+   `a`：新增一个

# 23 ripgrep

https://github.com/BurntSushi/ripgrep

`ripgrep` 是一种命令行搜索工具，用于在文本文件中查找模式

# 24 fd

https://github.com/sharkdp/fd

`fd` 搜索

# 25 终端文件管理器Joshuto

https://github.com/kamiyaa/joshuto

+   `joshuto` 打开

+   `cw`：改名

+   `yy`：复制

+   `pp`：粘贴

+   `hjkl`：上下左右

+   到某个目录下，：`shell zsh` 打开 `terminal`
