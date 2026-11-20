
# 我的Harness Engineering实践

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/HSb0dctUxoVsDlxVcMGcpHFZneY](https://qqs7y1hozd1.feishu.cn/docx/HSb0dctUxoVsDlxVcMGcpHFZneY)

所谓Harness Engineering，是通过强制性的工程约束和标准化的输入输出，将 AI 的不可预测性，收敛为确定性的交付。

> 最近接手了一个企业项目，代码都是AI写的，但由于有很好的规范，因此即使用`Cursor`/`Claude Code`给项目贡献代码，依然没崩过。

项目的业务信息已隐去，只用这个项目的相关技术为例，来讲讲怎么做Harness Engineering。

<https://player.bilibili.com/player.html?bvid=1Zk9FBwELs&share_source=copy_web&vd_source=03209ca27af1304a94333e65ff467e42>

---

这是运行了多年的企业级全栈项目，是一个`Monorepo`（前端后端代码都放在一个项目里），采用`Spring Boot` + `React` + `PostgreSQL`，部署在`Kubernetes`上，通过`Helm charts`管理多环境配置，`Jenkins pipeline`做`CI/CD`。

> 这套东西就基本上规范了开发者的开发流程，而这套规范，换到Agent生成，就成了Harness Engineering的一部分。总体架构大概就是这样

![](img/4e8dc9deba05001480a724b09e02c758.png)

## Spring Boot MVC 分层本身就是一种 Harness

说实话，我个人并不喜欢`Spring Boot`这套东西，很啰嗦，每次加点小`feature`，就搞个几千行的代码变更。

![](img/a158234d0e297188cd78eb4c045ab2a0.png)

这里只是加了一个很小的`feature`

但`Spring Boot`能在企业应用中风靡这么久，有它的道理。`Spring Boot`的规范性强，约定和分层让 50 人的团队写出结构一致的代码。使用`Spring Boot`开发的软件架构如下，代码应该怎么放，挺清晰的。

![](img/a833e56de12006a3d66354503e80ab77.png)

这个分层结构对`AI agent`来说，本身就构成了一套`harness`约束。

当然，这套约束对AI有强制效果吗？我只能说，有效果，但AI非要瞎写时，我也管不住。那么，怎么才能把这套指导，变成强制约束呢？我们团队设置了几层约束：

## 强制规范约束

项目配置了多层静态分析，确保：

+   `Spotless`（自动格式化）确保每行代码风格统一

+   [`Checkstyle`](https://zhida.zhihu.com/search?content_id=272054699&content_type=Article&match_order=1&q=Checkstyle&zhida_source=entity)（编码规范）约束类成员声明顺序、禁止`magic number`

+   [`SpotBugs`](https://zhida.zhihu.com/search?content_id=272054699&content_type=Article&match_order=1&q=SpotBugs&zhida_source=entity)（缺陷检测）在编译期捕获常见`bug`模式

+   [`JaCoCo`](https://zhida.zhihu.com/search?content_id=272054699&content_type=Article&match_order=1&q=JaCoCo&zhida_source=entity)（覆盖率）设定 90% 行覆盖率硬门槛

+   `ESLint` + `Prettier`（前端）保持`React`/`TypeScript`代码一致性

当 AI 生成的代码过不了`lint`、过不了覆盖率检查，它会被迫修正。这些质量检测系统构成了`harness`的第一道防线。

![](img/4b0bf3e84c67f2eef76f6a71bb770e7b.png)

（注：以上这套分析用到的一些工具，例如`JaCoCo`，虽然是分析`java`用的，但过程可以借鉴，例如你用`python`，就用`pytest-cov`）

## Pre-commit 约束

我在项目文件夹的 `/github/hooks`下面，放了三个代码：

+   `pre-commit-lint.sh`：运行`lint`检查

+   `pre-commit-test.sh` ：运行单元测试

+   `commit-msg.sh`：检查提交信息符合规范，必须带`branch`名称作为前缀，以方便`review`

![](img/f7a7f1fa07b3e78d6d41c75a0f9f25c1.png)

可以看到，我们这套代码规范还是挺严格的。这形成了一个反馈闭环：AI 写代码 → 提交 → `hooks` 拦截 → AI 修复 → 再提交。整个过程不需要人介入，但质量被牢牢把控。

## CI/CD约束

我可以在`AutoJenkins`文件里定义`buildFlow`，这样每次我提交代码，就会触发`build`。

```
        buildFlow            : [
                PULL_REQUEST: [&#x27;Build&#x27;, &#x27;Compile&#x27;, &#x27;Unit Test&#x27;, &#x27;Scan&#x27;, &#x27;scanAtSource&#x27;,&#x27;Sonar&#x27;, &#x27;Quality Gate&#x27;],
                QA          : [&#x27;Build&#x27;, &#x27;Compile&#x27;, &#x27;Unit Test&#x27;],
                RELEASE     : [&#x27;Build&#x27;, &#x27;Compile&#x27;, &#x27;Unit Test&#x27;],
        ],
```

可以看到，在不同的开发阶段，可以采用不同标准的质量管理方法啊，例如`PR`时由于要并入`Agent`的代码，需要更多步骤检查，而并入后，在其他阶段，只需要跑`Unit Test`就够了。

## 将工作流编进skills里进行约束

当然，`CI/CD`+软件工程规范并非Harness Engineering的全部，但也80%了。Harness Engineering的最终目的，是人搭好脚手架后，`Agent`能够在尽量少人工干预的情况下，尽可能多且好的完成工作。剩下的20%，可以通过最近很火的`skills`来实现。

我定义了几个`skills`，它们定义了：

+   本地开发环境怎么起（`Docker Compose` → 数据库 → `Backend` → `Frontend` 的启动顺序）

+   `Git` 工作流是什么（`rebase` 策略、冲突解决方式、分支命名）

+   `PR` 要满足什么质量才能合并

+   `CI/CD` 的 delta build 机制（只构建有变更的子项目）

当然，这些流程能不能构成约束？其实是可以的，例如本地环境怎么起这一块，它不`follow`我的流程，它就起不来。

## 达到的效果

通过这套Harness Engineering的方案，团队实现了「一劳永逸」的`Vibe Coding`体验，目前我们日常是这样的：

+   每周`plan`会议，讨论这周要实现的东西

+   在每个`ticket`里，描述我们要实现的`feature`

+   在`Cursor`里，通过`skils`，读取`ticket`里的内容，然后开始自动开发

+   开发完成，本地会跑`lint`，`JaCoCo`，`checkstyle`，单元测试等。

+   开发者在这一步介入，通过`skills`让代码在本地跑一下，看看`feature`实现得是否符合预期。

+   本地`review`一下，没问题，通过`skills`，自动创建`branch`（`branch`名字和`ticket`名字一样以方便管理），提交，创建`PR`

+   通知团队成员一起`review`。虽然多数时候大家给的评论都是「`LGTM`」，但我认为，还是很有必要`review`的，虽然AI写的代码你不一定每一行都看，但你得大概有个印象，这是`mental alignment`的一部分。

## 总结

可以看到，我们做Harness Engineering，有三层约束：

+   第一层：架构级约束（Architecture as Harness），例如`Spring Boot MVC`。

+   第二层：质量级约束（Quality as Harness），例如`Spotless`, `Checkstyle`, `JaCoCo`等，让`Agent`生成的代码质量在编译器和测试报告中进化。

+   第三层：流程级约束（Workflow as Harness）。整合`Git Hooks`和`Jenkins CI/CD`，实现反馈闭环（Feedback Loop），让`AI` 不只是写代码，它必须自己通过你设定的重重关卡。

没有Harness Engineering，技术债就会膨胀。以前我们设定软件工程规范，其实是需要取一个平衡值的，既不能没有规范导致技术债膨胀，也不能规范太多导致大家开发效率太低。而`Agentic coding`年代，可以再严格点，反正是`Agent`来写，随便压榨！
