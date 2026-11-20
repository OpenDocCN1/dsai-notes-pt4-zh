
# 2023-09-04大厂的Git代码管理规范是怎样的？

> 来源：[https://xuqiwei1986.feishu.cn/docx/N0YddW3Onoe6hvxJ82uc🐧Ecnxe](https://xuqiwei1986.feishu.cn/docx/N0YddW3Onoe6hvxJ82uc🐧Ecnxe)
>
> 🔗 原文链接：https://mp.weixin.qq.com/s/LWQolvFQ...
>
> ⏰ 剪存时间：2023-09-11 19:52:47 (UTC+8)
>
> ✂️ 本文档由 [飞书剪存](https://www.feishu.cn/hc/zh-CN/articles/606278856233?from=in_ccm_clip_doc)一键生成

程序员鱼皮 2023-09-04 19:10 发表于 上海

以下文章来源于码农参上，作者Dr Hydra

码农参上。

专注后端技术分享，有趣、深入、直接，与你聊聊技术。

## 分支命名

### master 分支

`master` 为主分支，也是用于部署生产环境的分支，需要确保 `master` 分支稳定性。`master` 分支一般由 `release` 以及 `hotfix` 分支合并，任何时间都不能直接修改代码。

### develop 分支

`develop` 为开发环境分支，始终保持最新完成以及bug修复后的代码，用于前后端联调。一般开发的新功能时，`feature` 分支都是基于 `develop` 分支创建的。

### feature 分支

开发新功能时，以 `develop` 为基础创建 `feature` 分支。

分支命名时以 `feature/` 开头，后面可以加上开发的功能模块，命名示例：`feature/user_module`、`feature/cart_module`

### test 分支

`test` 为测试环境分支，外部用户无法访问，专门给测试人员使用，版本相对稳定。

### release 分支

`release` 为预上线分支（预发布分支），UAT测试阶段使用。一般由 `test` 或 `hotfix` 分支合并，不建议直接在 `release` 分支上直接修改代码。

### hotfix 分支

线上出现紧急问题时，需要及时修复，以 `master` 分支为基线，创建 `hotfix` 分支。修复完成后，需要合并到 `master` 分支和 `develop` 分支。

分支命名以 `hotfix/` 开头的为修复分支，它的命名规则与 `feature` 分支类似。

## 分支与环境对应关系

在系统开发过程中常用的环境：

+ `DEV` 环境（Development environment）：用于开发者调试使用

+ `FAT` 环境（Feature Acceptance Test environment）：功能验收测试环境，用于测试环境下的软件测试者测试使用

+ `UAT` 环境（User Acceptance Test environment）：用户验收测试环境，用于生产环境下的软件测试者测试使用

+ `PRO` 环境（Production environment）：生产环境

对应关系：

| 分支 | 功能 | 环境 | 可访问 |

| --- | --- | --- | --- |

| `master` | 主分支，稳定版本 | `PRO` | 是 |

| `develop` | 开发分支，最新版本 | `DEV` | 是 |

| `feature` | 开发分支，实现新特性 |  | 否 |

| `test` | 测试分支，功能测试 | `FAT` | 是 |

| `release` | 预上线分支，发布新版本 | `UAT` | 是 |

| `hotfix` | 紧急修复分支，修复线上bug |  | 否 |

### 分支合并流程规范

业界常见的两大主分支（`master`、`develop`）、三个辅助分支（`feature`、`release`、`hotfix`）的生命周期：

![](img/bd7841d206960edbdd76dca262762c61.png)

以上生命周期仅作参考，不同开发团队可能有不同的规范，可自行灵活定义。

例如我们团队在开发时，至少需要保证以下流程：

+ `develop` 分支和 `hotfix` 分支，必须从 `master` 分支检出

+ 由 `develop` 分支合并到 `test` 分支

+ 功能测试无误后，由 `test` 分支合并到 `release` 分支

+ `UAT` 测试通过后，由 `release` 分支合并到 `master` 分支

+ 对于工作量小的功能开发（工时小于1天），可以直接在 `develop` 分支进行开发，否则由 `develop` 分支检出 `feature` 分支进行开发，开发完后合并到 `develop` 分支

## Git Commit Message规范

Git commit message 规范指提交代码时编写的规范注释，编写良好的 Commit messages 可以达到 3 个重要的目的：

+ 加快代码 review 的流程

+ 帮助我们编写良好的版本发布日志

+ 让之后的维护者了解代码里出现特定变化和 `feature` 被添加的原因

### Angular Git Commit Guidelines

业界应用的比较广泛的是 Angular Git Commit Guidelines：

```
<type>(<scope>): <subject>
<BLANK LINE>
<body>
<BLANK LINE>
<footer>
```

+ `type`：提交类型

+ `scope`：可选项，本次 `commit` 波及的范围

+ `subject`：简明扼要的阐述下本次 `commit` 的主旨，在 Angular Git Commit Guidelines 中强调了三点。使用祈使句，首字母不要大写，结尾无需添加标点

+ `body`：同样使用祈使句，在主体内容中我们需要把本次 `commit` 详细的描述一下，比如此次变更的动机

+ `footer`：描述下与之关联的 `issue` 或 break change

### 简易版

项目中实际可以采用简易版规范：

```
<type>(<scope>):<subject>
```

### type 规范

Angular Git Commit Guidelines 中推荐的 `type` 类型如下：

+ `feat`：新增功能

+ `fix`：修复 bug

+ `docs`：仅文档更改

+ `style`：不影响代码含义的更改（空白、格式设置、缺少分号等）

+ `refactor`：既不修复 bug 也不添加特性的代码更改

+ `perf`：改进性能的代码更改

+ `test`：添加缺少的测试或更正现有测试

+ `chore`：对构建过程或辅助工具和库（如文档）的更改

除此之外，还有一些常用的类型：

+ `delete`：删除功能或文件

+ `modify`：修改功能

+ `build`：改变构建流程，新增依赖库、工具等（例如 `webpack`、`gulp`、`npm` 修改）

+ `test`：测试用例的新增、修改

+ `ci`：自动化流程配置修改

+ `revert`：回滚到上一个版本

### 单次提交注意事项

+ 提交问题必须为同一类别

+ 提交问题不要超过 3 个

+ 提交的 `commit` 发现不符合规范，`git commit --amend -m "新的提交信息"` 或 `git reset --hard HEAD` 重新提交一次

## 配置 .gitignore 文件

`.gitignore` 是一份用于忽略不必提交的文件的列表，项目中可以根据实际需求统一 `.gitignore` 文件，减少不必要的文件提交和冲突，净化代码库环境。

通用文件示例：

```
HELP.md
target/
!.mvn/wrapper/maven-wrapper.jar
!**/src/main/**/target/
!**/src/test/**/target/

### STS ###
.apt_generated
.classpath
.factorypath
.project
.settings
.springBeans
.sts4-cache

### IntelliJ IDEA ###
.idea
*.iws
*.iml
*.ipr

### NetBeans ###
/nbproject/private/
/nbbuild/
/dist/
/nbdist/
/.nb-gradle/
build/
!**/src/main/**/build/
!**/src/test/**/build/

### VS Code ###
.vscode/

# Log file
*.log
/logs*

# BlueJ files
*.ctxt

# Mobile Tools for Java (J2ME)
.mtj.tmp/

# Package Files #
*.jar
*.war
*.ear
*.zip
*.tar.gz
*.rar
*.cmd
```

## 其他

此外，还有一些其他建议：

+ `master` 分支的每一次更新，都建议打 `tag` 添加标签，通常为对应版本号，便于管理

+ `feature` 分支、`hotfix` 分支在合并后可以删除，避免分支过多管理混乱

+ 每次 `pull` 代码前，提交本地代码到本地库中，否则可能回出现合并代码出错，导致代码丢失

---

欢迎学编程的朋友们加入鱼皮的 [编程导航](https://mp.weixin.qq.com/s?__biz=MzI1NDczNTAwMA==&mid=2247544329&idx=1&sn=8c268a0a40befcc2b470c75ced1cb1d6&chksm=e9c2c9fedeb540e848aa18c484a9674dc9dd28fc45bb510dc4be52b147dd4c1d8fd7fe6899df&token=1562751299&lang=zh_CN&scene=21#wechat_redirect)，和 2 万多名编程学习者共享知识、交流进步，学习鱼皮全程直播开发的原创项目、上千篇优质编程学习求职经验分享、并获取 1 对 1 答疑指导服务。

### 往期推荐

[我的学习小圈子](https://mp.weixin.qq.com/s?__biz=MzI1NDczNTAwMA==&mid=2247539132&idx=2&sn=45af016dee0c03491750f76ba8fdbd25&chksm=e9c2be4bdeb5375d3253155b4053263109a631620b7cb9074e2fe1b4a5b1604ef92c522b606e&scene=21#wechat_redirect)

[我坚持 3 年了！](https://mp.weixin.qq.com/s?__biz=MzI1NDczNTAwMA==&mid=2247549995&idx=1&sn=922bc61111e9b5e6ea05d8b48979f8b8&chksm=e9c2e3dcdeb56acacc8eb9cc7279f0ea50e7e736366e6db7edd6ab6396d6d41a97cd8112afc2&scene=21#wechat_redirect)

推送数据？也许你不需要 WebSocket

公司代码质量不高，是普遍现象么？

[什么是负载均衡？](https://mp.weixin.qq.com/s?__biz=MzI1NDczNTAwMA==&mid=2247549957&idx=2&sn=71f07cdcc6a9050ef03136fe4379f57a&chksm=e9c2e3f2deb56ae453aa1d80fe9503ff20abaa52642666e7626d714d3d7521c2d243f1116f3f&scene=21#wechat_redirect)

[又一个新项目完结，炸裂！](https://mp.weixin.qq.com/s?__biz=MzI1NDczNTAwMA==&mid=2247549858&idx=1&sn=dee63c9111558c81b8ee8921f871ea6f&chksm=e9c2e455deb56d43d79e01c5ff310487fb66e16baef27f664e43a0e8779abd6506194fc216aa&scene=21#wechat_redirect)
