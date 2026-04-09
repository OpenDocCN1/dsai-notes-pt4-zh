# Git 关卡

> 来源：[https://aicarrier.feishu.cn/docx/RyOQdrMwWoHB88xaPkQcDtPcn9b](https://aicarrier.feishu.cn/docx/RyOQdrMwWoHB88xaPkQcDtPcn9b)

# 本节课目标

| 任务1 | 破冰活动：自我介绍 |
| --- | --- |
| 任务2 | 实践项目：构建个人项目 |

# Git

是一个开源的分布式版本控制系统，被广泛用于软件协同开发。程序员的必备基础工具。

官网：https://git-scm.com/

官方文档：[Git - Book](https://git-scm.com/book/en/v2)

Git 基础：[Git 基础知识](https://aicarrier.feishu.cn/wiki/YAXRwLZxPi8Hy6k3tOQcuwAHn5g)

## Git 安装

### **Windows 系统**

https://git-scm.com/download/win

1.  选择适合您 Windows 版本（32 位或 64 位）的安装程序进行下载。

![](img/d28afd7e54d37f559c21d51a568d85ad.png)

1.  在安装向导中，通常可以选择默认设置，一路点击“Next”（下一步）即可完成安装。

![](img/c88d66c20f582ace87c916ce50fc91ef.png)

3.检查是否安装完成。

打开终端（win+r—>cmd）

输入指令检查 git --version

（已经安装过，暂时不更新啦）

![](img/878b1e424b492459b6e6ad328157f507.png)

### **Linux 系统**

通过包管理器安装 Git

```
sudo apt update
sudo apt install git
# 如果使用的 Intern-Studio 可以跳过或去除sudo后执行。默认已安装
```

1.开机：

随便一台机器，仅测试。

![](img/1586388f669d418937bc3810e0fcb3d6.png)

2.检查git 版本

![](img/d0db68ea19da0a336c2f60c40cce079d.png)

如果没安装，可以按上面的安装教程完成安装。

## 常见的 Git 托管平台

### **GitHub**

https://github.com/

+   是全球最大的代码托管平台之一，拥有丰富的开源项目和活跃的开发者社区。它提供了版本控制、项目管理、协作开发等功能，并支持多种编程语言。

### **GitLab**

https://gitlab.com/

+   一个自托管或基于云的平台，提供了完整的 DevOps 工具链，包括代码托管、持续集成/持续部署（CI/CD）、问题跟踪等。

### **Gitee**

https://gitee.com/

+   国内的代码托管平台，提供了代码托管、项目管理、协作开发等功能，对国内开发者来说，访问速度可能更快，也更符合国内的使用习惯。

Github需要魔法，没条件的学员可以选择 Gitee 来使用。

## 常用的 Git 操作

1.  git init

+   初始化一个新的 Git 仓库，在当前目录创建一个 .git 隐藏文件夹来跟踪项目的版本历史。

1.  git clone <repository-url>

+   从指定的 URL 克隆一个远程仓库到本地。

1.  git add <file> 或 git add.

+   将指定的文件或当前目录下的所有修改添加到暂存区，准备提交。

1.  git commit -m "message"

+   提交暂存区的修改，并附带一个有意义的提交消息来描述更改的内容。

1.  git status

+   查看工作目录和暂存区的状态，包括哪些文件被修改、添加或删除。

1.  git log

+   查看提交历史，包括提交的作者、日期和提交消息。

1.  git branch

+   列出所有本地分支。

1.  git branch <branch-name>

+   创建一个新的分支。

1.  git checkout <branch-name>

+   切换到指定的分支。

1.  git merge <branch-name>

+   将指定的分支合并到当前分支。

1.  git push

+   将本地的提交推送到远程仓库。

1.  git pull

+   从远程仓库拉取最新的更改并合并到本地分支。

1.  git stash

+   暂存当前未提交的修改，以便在需要时恢复。

1.  git stash pop

+   恢复最近暂存的修改。

开始一个新的项目时，首先使用 git init 初始化仓库。在进行一些代码修改后，使用 git add. 添加所有修改，然后使用 git commit -m "Initial commit" 提交更改。

在团队工作中，完成自己的开发后，使用 git push 将更改推送到远程仓库，以便其他团队成员可以获取您的工作成果。

## 常用插件

在 VSCode 等软件中，插件是效率提升的利器。（根据自己的爱好进行选择）

![](img/468ae514db3cc48ce0be8a0c1b34f0b7.png)

| [GitLens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens) | 在代码行上显示 Git 提交信息，如提交人、时间及变更描述等，还能查看文件历史记录、比较视图、显示 Git blame 注释和团队成员的最新活动等 |
| --- | --- |
| [Git Graph](https://marketplace.visualstudio.com/items?itemName=mhutchie.git-graph) | 类似于 SourceTree 的可视化版本控制插件，可用于查看提交记录、审视代码等 |
| [Git History](https://marketplace.visualstudio.com/items?itemName=donjayamanne.githistory) | Git 日志查看器，能轻松查看 Git 提交历史记录，支持显示不同颜色的分支、提交信息和更改详细信息，以及搜索提交历史记录； |

# 作业

## 破冰活动：自我介绍

### 任务

提交自己的破冰介绍.md

### 要求

1.  命名<camp3_<id>.md>

1.  路径：./data/Git/task/

1.  【大家可以叫我】可以为github昵称，微信昵称等，或其他网名

1.  作业提交对应的PR链接

```
./data/Git/task/camp3_&lt;id&gt;.md
# id 为报名文件的ID
```

![](img/5328f2851115e564e42ef7e3163c4137.png)

（如果你常用，并且参与过项目开发，我相信下面的操作佬都会，可以快速跳过）

**动手操作**

1.查看报名问卷ID：

[报名问卷](https://www.wjx.cn/vm/PvefmG2.aspx?sojumpparm=MTU3NjczNzE1ODY=)

![](img/c3752507b096839bbb001ecfbb2b9279.png)

2.Frok项目

[项目链接](https://github.com/InternLM/Tutorial)

![](img/eed5a996e2292730ae929136dbdf482d.png)

3.获取仓库链接

![](img/8d59cde6cfc43889361da855ef0d5163.png)

```
git clone https://github.com/MrCatAI/Tutorial.git #修改为自己的
cd Tutorial/
git branch -a
git checkout -b camp3 origin/camp3
```

4.查看分支

![](img/f40004ae66bc971ef159e5eb5ed25846.png)

5.切换到第三期的分支

![](img/e5a7b0a20ebe0ed6761dbb0da7a8cdd5.png)

6.查看分支内容

![](img/e2cb4c50724eef286a1657213b3d9dd9.png)

```
git checkout -b camp3_577 # 自定义一个新的分支
```

![](img/4d1a7e722bf128f98f93665652d003ca.png)

示例：

![](img/e845097837f54d16dd7c15183a1daf41.png)

示例文件路径

```
./data/Git/task/camp3_&lt;id.md
```

7.创建自己的破冰文件

```
touch ./data/Git/task/camp3_557.md #修改为自己的问卷ID
```

![](img/332a334e06279c0a1b00a4672f6d2dbc.png)

8.提交更改到分支

```
git add .
git commit -m &quot;add git_557_introduction&quot; 提交信息记录
```

![](img/b908ac0d05fd1d467d5904df3c5b3acc.png)

```
git **push** origin camp3_577
```

（大家提交使用英文，避免仓库同步错误）

注：初始化时可能需要创建对应的token：（示例，可根据实际提示完成）

![](img/2ef1cb0bfc2494cb28ef831d622fccea.png)

![](img/a52ee6821febc33197c3fec1107aa136.png)

![](img/da35003a533c148c79a6caa74641399c.png)

提交成功

![](img/ef0b0b0e793003ecdfe6e1e46ce13629.png)

查看提交

![](img/4b9edf4bffe7b5827a1cce5177e3d4ef.png)

在github页面提交PR到Tutorial

![](img/90a3e3712f104292816e8af4b821a918.png)

按要求编写title

```
git_557_introduction
```

下面可以查看修改的内容

![](img/911ee67290161108bcb936e2b40c678c.png)

PR示例链接：https://github.com/InternLM/Tutorial/pull/790

也可以合并到自己仓库的代码

![](img/d63d6525bbe89035664967ad295dacd4.png)

自己的仓库，可以自行merge，作业提交的公开需要维护者审核。

![](img/848d5fd2992ebfa2bfbe438f74d883f7.png)

## 实践项目：构建个人项目（简版）

创建个人仓库，用于提交笔记、心得体会，或分享自己的项目和创意。我们特别欢迎与 LLM 训练和应用相关的内容，但不限于此。

### 任务

创建自己的仓库(拥有自己的仓库，记录笔记与心得

### 要求

1.  拥有自己的项目或笔记仓库（公开）。

1.  提交作业 github 仓库链接（如已经有项目的可以提交项目链接。

1.  github 使用少的，可参考下面的操作，提交到常用的其他代码管理平台，如gitee。

1.  [笔记或项目类仓库，](https://github.com/InternLM/Tutorial)添加超链接跳转GitHub 仓库（https://github.com/InternLM/Tutorial）

**动手操作**

1.先在github创建一个初始化仓库

![](img/662fc9d00da1db5e4040e604fdca56ff.png)

![](img/af6043844a06ecc5f972f8c20d0ef08f.png)

这样你就得到一个空白的仓库：

![](img/cd218852dc32f44212aaaedec3a23112.png)

然后按上面的操作，可以拉取仓库和提交对应的信息：

![](img/437692dc5fafc02f4559c102d91bb813.png)

![](img/c209908bbf832cfd0958e49ebd3a176b.png)

修改，并记录自己的项目信息等.....

![](img/70b6eba6d7016afc1f52e6b50e0d6419.png)

![](img/e215c8e74e3d5de1b9fa5b938e70ae27.png)

修改完后提交到仓库：

![](img/bda0b5bd28bd98628c8044d087f81272.png)

![](img/6c6a506b7fcdc841cf38aee7046885ac.png)

![](img/c430cfa54e06807363a141b2f4102289.png)

参考仓库(仅演示)：https://github.com/MrCatAI/March7thMuse1