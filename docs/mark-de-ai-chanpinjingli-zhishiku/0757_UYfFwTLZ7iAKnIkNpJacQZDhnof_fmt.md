
# 手把手教你配置Skills

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/YGe3dPS24oqCbqxbXgscSX80nff](https://qqs7y1hozd1.feishu.cn/docx/YGe3dPS24oqCbqxbXgscSX80nff)

## TRAE上线Skills啦!手把手教你配置这个神器

哈喽大家好!最近字节跳动家的TRAE AI搞了个大动作,正式上线了Skills功能!这玩意儿简直不要太香,让咱们写代码的效率直接起飞。今天就手把手教大家咋个配置这个神器,保证包教包会!

> 🔗 宝子们注意啦!通过这个Claude Code 国内代理链接注册使用,可以送20美金抵扣券直接可用哦!这羊毛必须薅!

![](img/370064f45434e7ce0fe01e669b30cc06.png)

### 什么是TRAE Skills?

首先给大家摆一下,TRAE是字节跳动推出的一款AI原生IDE,专门给咱们中文开发者设计的。它集成了Claude 3.5和GPT-4o这些高级AI模型,能跟用聊天一样写代码。现在新上的Skills功能,就是让AI能够调用各种定制化的技能包,实现更加专业和标准化的开发流程。

简单来说呢,就好比你给AI装了个工具箱,里面有各种专业的工具,需要的时候随时拿出来用。比如你要写个前端页面,就能调用前端设计的Skills;要做个后端API,就调用后端开发的Skills。这就相当于你请了个全能助手,啥子都会!

### TRAE Skills好用在哪里?

#### 1. 标准化工作流程

Skills最大的好处就是能把咱们的开发流程标准化。比如说做个项目,从前端到后端到测试,每一步都有规范的流程。以前这些全靠脑壳记,现在直接让AI按流程走,简直不要太安逸!

#### 2. 大幅提升效率

以前写代码要到处查资料、看文档,现在有了Skills,AI直接调取相关的知识库和最佳实践,分分钟给你整出高质量的代码。这就相当于你有个24小时在线的技术顾问!

#### 3. 团队协作更方便

Skills可以分享给团队成员用,大家用同样的标准干活,代码风格统一,维护起来也不得头痛。这对团队来说简直不要太爽!

### 手把手配置教程来了

好了好了,废话不多说,直接上干货!配置TRAE Skills只需要四个步骤,跟着我一步步来,保准你会!

#### 步骤一:安装OpenSkills项目

TRAE本身不支持直接用Claude Skills,但咱们可以通过OpenSkills这个开源项目来实现。首先打开终端(terminal),输入下面这个命令:

`npm i -g openskills`

![](img/541e0fc29ad9f77f57f8f27d124e2b03.png)

这条命令是把OpenSkills安装到系统全局位置,只需要做一次就行了。安装完之后,咱们就可以进行下一步了。

#### 步骤二:安装Anthropic官方Skills

安装好OpenSkills之后,咱们就可以安装官方的Skills了。这里有两个选择:

安装到当前项目:

`openskills install anthropics/skills`

全局安装(推荐):

`openskills install anthropics/skills --global`

![](img/d1ce847d9bfa347807c9dd547202e588.png)

运行命令之后,OpenSkills会把Anthropic官方的Skills项目克隆下来。默认会全选,你也可以用空格键选择自己想要的Skills。安装成功后,在TRAE的文件管理区就能看到`.claude/skills`文件夹了。

如果你想安装其他第三方的Skills也是可以的,只需要把仓库地址换成对应的就行:

`openskills install your-org/custom-skills`

#### 步骤三:创建AGENTS.md文件并同步Skills

前两步只是把Skills安装到本地,如果想让TRAE这些Coding Agent发现和使用这些Skills,必须做这一步!

首先在项目根目录创建一个`AGENTS.md`文件,然后运行:

`openskills sync`

运行之后,再次选择你想写入`AGENTS.md`文件的Skills。确认后按回车,选择的Skills就会写入这个文档。这个文件将作为TRAE使用Skills的指导文件,相当重要哈!

#### 步骤四:在TRAE中调用Skills

配置好之后,你就可以在TRAE中使用Skills了!Skills可以被自动调用,如果你想手动调用,可以直接在提示词中指定。

比如你想用前端设计技能开发个页面:

`frontend-design skills`,用HTML开发一个视频剪辑软件的SaaS介绍页

或者开发一个博客网站:

`frontend-design skills` 用HTML创建一个现代化的个人博客网站原型,包含首页、文章详情页、关于页面的完整博客

TRAE会自动调用对应的Skills,按照标准化的流程帮你完成开发任务。这感觉简直不要太爽!

### 实战案例演示

来来来,给大家看个实际例子。咱们用Skills来开发一个视频剪辑软件的介绍页:

1. 明确需求: 在TRAE的Chat模式中输入"调用`frontend-design skills`,开发一个视频剪辑软件的SaaS介绍页"

2. AI分析: TRAE会调用`frontend-design skills`,分析页面需求和设计规范

3. 生成代码: AI会自动生成HTML、CSS代码,包括响应式设计、美观的UI组件

4. 实时预览: 点击预览按钮,直接在浏览器中查看效果

5. 迭代优化: 不满意的地方继续跟AI说,它会继续优化

整个过程下来,原本可能要几小时的活路,现在几十分钟就搞定了,而且代码质量还贼高!

### 使用Skills的小技巧

给大家分享几个使用Skills的心得:

#### 1. 先学官方Skills咋写的

创建自定义Skills之前,先把官方Skills仓库克隆到本地,让AI先学习一下官方的写法。这样无论它最开始懂不懂,都会通过官方仓库的`skill-creator`这个Skill快速学会。

#### 2. 梳理清楚你的工作流程

创建Skills的原因一定是你想要把某个工作流程标准化。这就需要你提前把工作流程梳理清楚,这个只有你自己最清楚,所以偷懒不得哈。但可以借助AI来拓展思路或者提前规避一些问题。

#### 3. 先做MVP再优化

工作流程可能没法一次性梳理到位,没关系,先做个最小可行产品(MVP)的Skills出来跑一遍,你就知道哪里有问题,然后针对性地调优。这个过程可以像管理工程文件那样多用`Git`。

#### 4. 复杂Skills分步实现

对于稍微复杂点的Skills,一次性生成可能会出问题。更稳妥的方法就是每个步骤都单独拆开测试。比如先写脚本,测试没问题后,再往前倒推,每一步都完成测试后,最后再一次性串联起来。

### TRAE与其他工具的对比

给大家简单对比一下TRAE和其他AI编程工具的区别:

从对比可以看出,TRAE在中文支持和免费使用方面有明显优势,特别适合咱们国内的开发者!

### 总结与建议

TRAE Skills的上线,让AI编程变得更加强大和标准化。通过四个简单步骤,就能让TRAE掌握各种专业技能,大幅提升开发效率。

#### 给大家的建议:

1. 从小项目开始: 先用简单的项目练手,比如待办清单、天气查询工具等

2. 多实践多总结: 每天花点时间实践小案例,逐步熟悉AI协作

3. 关注官方更新: TRAE还在快速迭代中,经常会有新功能上线

4. 分享交流: 把自己创建的Skills分享给社区,大家一起进步

随着AI编程工具的不断发展,掌握像TRAE这样的AI辅助工具已经成为开发者的必备技能。希望这篇教程能帮到大家,让咱们的开发效率起飞!

有问题欢迎在评论区交流哈,大家一起学习,一起进步!

---

### 相关资源:

+ TRAE国内版官网: https://www.trae.com.cn/

+ TRAE国际版官网: https://www.trae.ai/

+ OpenSkills项目: https://github.com/numman-ali/openskills

+ Anthropic官方Skills: https://github.com/anthropics/skills

### 参考资料

+ Trae AI 保姆级教程:从安装到调试全流程指南

+ 如何在Cursor、Trae等工具中使用Skills?

+ AI开发新范式:Trae一口气升级5大能力
