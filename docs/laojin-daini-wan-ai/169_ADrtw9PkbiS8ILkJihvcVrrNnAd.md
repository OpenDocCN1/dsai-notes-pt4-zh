# 实战：以不懂代码的小白视角，创建个人网页手把手教学

> 来源：[https://my.feishu.cn/docx/DaEnd7SZOoVZNTx4idRcM8pPnYe](https://my.feishu.cn/docx/DaEnd7SZOoVZNTx4idRcM8pPnYe)

讲了好久各种AI编程之类的内容，但从来也没发过实战系列。

虽说过很多次，但老金在写本文时，还是再次强调。

老金我一点儿代码不会（至今为止），英语没过4级。

**简而言之，我行，你也行。**

**注：**本项目实现过程中，老金尽可能把走过的坑都讲一遍，为毫无经验的小白玩家快速入门使用。

老金本次还是用Cursor来操作，至于你用的是如Claude Code、Gemini、QWEN等各种Cli、或者使用Windsurf、Codebuddy、Qoder等各类IDE都行，按自己喜好来。

记住老金在之前讲 [**三大心法**](https://tffyvtlai4.feishu.cn/wiki/UqAowXLkTi1bFfkE0wAcIs0Wnzg) 时候说的话，每个工具执行逻辑大差不差，功能略微有些差距。

钻研一个就好，除非有一个跨时代的产品诞生，才值得你去更换，否则只是在不断更换，尝试新产品浪费时间。

---

## 第一步 - 准备工作

我们直接开始，老金唯独比大家多的，应该是产品经理的经验。

**万事开头先做流程**，这个是做产品的铁律，当然，以老金的视角来看，任何的事项，皆需要流程。

直接用GPT5输出。

![](img/90405c72793473239b32b728ae59855a.png)

不用着急，老金会给你最简单的方法进行。

直接给GPT5输入以下提示词，老金用小白视角直接来做提示词。

相信大家大部分都能知道是什么，那就对了。

```
我将以一个不懂代码的小白视角，使用cursor，制作个人网页。
你需要一步步教我制作，并放到网上让别人可以进入查看。 
现在我们应该以小白的路径，先做PRD（需求文档），然后按照PRD（需求文档）一步步执行，最后帮我放到网上。
现在你需要作为一名有着顶级产品思维、代码能力，以及UI（界面）和UX（交互）设计能力的导师，一步步教我完成。 
请按它变成todolist，让我们一个个完成，每完成一个你需要回来标记完成。
```

它的输出很长，我们逐个来拆分段落来看，一些概括型的内容，以及解决思路、需求工具等等的罗列。

没什么需要注意的，当然对于没经验的小白来说，这些专属名词或工具/网站等等，只要多用一用自然就会记住了。

**先强调以下，老金为什么一直说GPT5很强，以下内容均是一次**输出，是老金为了让大家跟上，拆分截图了。

![](img/64e788797ce1737ee922a99cd4e0bb5c.png)

按照上图所看第四步，它写的PRD，老金直接复制成代码块。

```
文档标题：个人主页（Lite 版）— PRD v1.0
版本：v1.0
最后更新：——

一、目标（Goal）
在 2–3 小时内上线一个颜值在线、移动端友好的个人网站，包含「关于我 / 作品或项目 / 联系我」，支持后续无代码改内容与自动再部署。

二、用户画像（Personas）
1) 访客：对我感兴趣的潜在读者、合作方、招聘方。
2) 我（站点主人）：想快速建立可信度、展示作品、便捷联系；后续希望不写代码即可更新内容。

三、范围与信息架构（Scope &amp; IA）
- 顶部导航：Logo/名字｜About｜Projects｜Contact
- 首页（单页或多段）结构：
  1. Hero：头像 + 一句话定位 + 行动按钮（联系我）
  2. About：3–5 句简介 + 关键技能标签
  3. Projects：项目卡片栅格（图、标题、标签、描述、外链）
  4. Contact：邮箱与社媒外链（支持 mailto）
- 页脚：社媒图标（X / 公众号 / 邮箱 / LinkedIn 等）+ 版权年

四、必须功能（Must Have Requirements）
1) 响应式布局：移动优先（≤360px 仍不崩），桌面/平板自适配。
2) 基础动效：按钮与卡片悬停、滚动入场（轻量、不过度）。
3) 文案与数据配置化：/content/data.json（或 /content/*.md）；无需改代码也能改站点内容。
4) 联系方式：主按钮使用 mailto 直邮；可选集成第三方表单（Formspree）。
5) SEO 基础：&lt;title&gt;、&lt;meta description&gt;、Open Graph、favicon、语义化标签。

五、非功能需求（NFR）
- 性能：首屏加载 ≤1.5s（Vercel CDN 默认加速）；避免大资源阻塞。
- 可访问性：对比度满足 WCAG AA；键盘可聚焦与操作。
- 稳定性：主路径（首页渲染、导航、跳转、表单/邮件唤起）100% 成功。

六、UI/UX 设计预设（Design System）
- 色板：
  - 主色：#5B8DEF
  - 辅色：#7BDAA1
  - 文本：#111827
  - 背景：#F8FAFC
- 字体：系统字体栈（减少额外配置）；标题半粗、正文常规。
- 栅格：容器最大宽 1100px；项目卡片 3（桌面）/2（平板）/1（手机）列自适应。
- 组件规范：
  - 按钮：主按钮使用主色、圆角 8px、悬停轻微放大与阴影。
  - 卡片：圆角 12px、阴影层级随悬停增强、过渡 200ms。
  - 导航：滚动吸顶、当前区块高亮（可选）。

七、技术选型（Tech Stack）
- 框架：Next.js（App Router）
- 样式：Tailwind CSS
- 数据：/content/data.json（后续可扩展为 Markdown/MDX）
- 部署：Vercel（主推，一键部署，免费子域名）
- 可选增强：Plausible/GA4（统计）、Formspree（表单）

八、数据结构示例（/content/data.json）
{
  &quot;hero&quot;: {
    &quot;name&quot;: &quot;你的名字&quot;,
    &quot;tagline&quot;: &quot;一句话定位：我用 AI/自动化解决实际问题&quot;,
    &quot;ctaText&quot;: &quot;联系我&quot;,
    &quot;ctaLink&quot;: &quot;mailto:you@example.com&quot;,
    &quot;avatar&quot;: &quot;/avatar.jpg&quot;
  },
  &quot;about&quot;: {
    &quot;bio&quot;: &quot;用 3–5 句话介绍你：现在做什么、擅长方向、过往亮点、正在寻找的合作或职位等。&quot;,
    &quot;skills&quot;: [&quot;AI 工作流&quot;, &quot;内容自动化&quot;, &quot;产品设计&quot;]
  },
  &quot;projects&quot;: [
    {
      &quot;title&quot;: &quot;项目 A&quot;,
      &quot;desc&quot;: &quot;一句话价值 + 一句话做了啥，例如：用自动化将周报生成效率提升 80%。&quot;,
      &quot;tags&quot;: [&quot;AI&quot;, &quot;Automation&quot;],
      &quot;link&quot;: &quot;https://example.com&quot;,
      &quot;image&quot;: &quot;/projects/a.jpg&quot;
    },
    {
      &quot;title&quot;: &quot;项目 B&quot;,
      &quot;desc&quot;: &quot;例如：把 CSV 数据自动转为图表网页，零手工排版。&quot;,
      &quot;tags&quot;: [&quot;Data&quot;, &quot;Frontend&quot;],
      &quot;link&quot;: &quot;https://example.com/b&quot;,
      &quot;image&quot;: &quot;/projects/b.jpg&quot;
    }
  ],
  &quot;contact&quot;: {
    &quot;email&quot;: &quot;you@example.com&quot;,
    &quot;socials&quot;: [
      {&quot;name&quot;: &quot;X&quot;, &quot;url&quot;: &quot;https://x.com/yourid&quot;},
      {&quot;name&quot;: &quot;LinkedIn&quot;, &quot;url&quot;: &quot;https://linkedin.com/in/yourid&quot;},
      {&quot;name&quot;: &quot;WeChat/公众号&quot;, &quot;url&quot;: &quot;https://your-wechat-link&quot;}
    ]
  }
}

九、验收标准（Acceptance Criteria）
1) 访问首页可见：头像、简介、项目卡、联系按钮与社媒链接。
2) 移动端/桌面端均无布局断裂或文本溢出；导航可正常跳转。
3) Lighthouse（简测）性能/可访问性/SEO ≥ 90（允许±5 的环境波动）。
4) 公共 URL（Vercel）可匿名访问；邮件按钮能唤起默认邮件客户端。
5) 修改 /content/data.json 并推送后，Vercel 自动触发再部署且内容更新生效。

十、出范围（Out of Scope）
- 后台、数据库、登录注册、评论系统。
- 复杂博客系统、富文本编辑与媒体库管理。
- 多语言国际化（后续可迭代）。

十一、里程碑（Milestones）
M1：本地开发可预览（Hero/About/Projects/Contact 就绪）  
M2：连接 GitHub 并首次部署到 Vercel（获得公共链接）  
M3：完成一次无代码内容更新流程（仅改 data.json 即可上线）

十二、风控与回退（Risk &amp; Rollback）
- 依赖安装失败：切换 Node.js LTS 版本或删除 node_modules 重装。
- 构建失败：检查 next.config 与 Tailwind 配置，回滚到上一次可用提交。
- 样式错乱：回滚最近一次样式改动或用 Tailwind 预设替换自定义样式。

十三、状态（Status）
- PRD v1.0：已完成，可进入开发与部署。

```

## 第二步 - 环境部署

然后我们进入第二步，来看看GPT5的回复。

![](img/2e10c99d14e6103690cd57041b48521d.png)

看不懂是吧。

没关系，熟悉老金的都知道，老金向来用AI打败AI。

跟AI编程有关的东西，交给AI编程工具解决。

直接打开Cursor，创建一个**【个人主页】空文件夹**，打开进入。

如果你没和老金之前学过，不要紧，直接打开 Cursor.com 下载即可。

你也可以查看老金之前写的教学文章，除了破解已经失效以外，功能说明都可以学一遍，你可以点击这里查看

[Cursor](https://tffyvtlai4.feishu.cn/wiki/AHKgwiM6Eip76kkzfHDcJWkxncb)。

![](img/4e6f8cf765ea04151dd4db5e79aeda60.png)

下一步，直接和Cursor说，“项目需要这些准备环境，你帮我解决”，把第二步的所有内容复制进来。

甭管什么注册与否，Cursor会提醒你要做什么，其他的它自己都能做。

老金这里是之前都让它安装过了，所以它会自己检测之后，告我安装过了。

方便大家操作，直接放入下面代码块中，方便大家复制。

对，眼尖的人看到老金把待办清单也放进来了，因为Cursor也会记录这些。

```
项目需要这些准备环境，你帮我解决
第2步｜准备环境（你来执行，我给明确动作） 
目标：安装 Node**.js L**TS、Git、Cursor；注册 GitHub &amp; Vercel。 
安装 Node.js LTS（包含 npm）。 
访问 nodejs.org 下载 LTS 安装包并安装（一路“继续”即可）。 
安装后，打开 Cursor 内置终端（或系统终端）输入： 
node -v 
npm -v 
正常输出版本号即可。 
安装 Git 
访问 git-scm.com 下载并安装。 
终端输入：git --version 有版本号即可。 
安装 Cursor 
下载并安装，登录账号即可。 
注册 GitHub 与 Vercel 
github.com 与 vercel.com 注册（用同一邮箱更顺手）。 
Vercel 里使用 GitHub 登录并授权。 

待办清单：
第1步｜输出 PRD v1.0（已完成，本条下方即正文）
第2步｜准备环境（Node.js LTS、Git、Cursor、GitHub、Vercel 账号）
第3步｜在 Cursor 创建「个人主页」项目（Next.js + Tailwind）
第4步｜生成页面与样式（About/Projects/Contact + 响应式 + 动效）
第5步｜本地预览 &amp; 自测（按验收标准逐项过）
第6步｜接入 GitHub &amp; 推送首版代码
第7步｜Vercel 一键上线（拿到公开链接）
第8步｜可选增强：自定义域名 / 访问统计 / 表单投递
第9步｜内容托管与后续迭代（无需写代码的更新方法）
```

我们来看看Cursor的输出，有点长，但对大家应该是有帮助的，老金直接都贴上来。

![](img/c67b56ae10b8659aaf3a5b9034d6cfb8.png)

![](img/2fb0e80f257cd593e8e279662463e550.png)

这里它甚至会贴心的**告你网址和操作步骤**。

![](img/cce7e82358dd70fa66520b35883a20ae.png)

### 创建Github和Vercel账户

#### Github账户

进入https://github.com/，点击右上角的注册。

![](img/7afc2927b59bcac9073de03b28a0b00b.png)

老金以前说过，海外逃不掉谷歌，有个Gmail在手，注册起来很方便省事儿。

![](img/d2319d4f13c2b844ffea023361d0316f.png)

#### Vercel账户

然后注册Vercel账户，直接拿Github账户关联注册就行，一键完成。

![](img/c73ab727aa645422ac3974739f80632a.png)

![](img/f7f4687199dddf82c68ceb8253192c89.png)

## 第三步 - 创建个人主页

告它第二步我们完成了，可以进行第三步了，老金直接把GPT5输出的第3步、第4步、第5步放到下面。

```
GitHub账号 和 Vercel 账号，我注册完了。

第3步｜在 Cursor 生成项目骨架（我给可复制提示词）
打开 Cursor，新建空文件夹并在该文件夹中打开。
在 Cursor 聊天框粘贴下面提示词，一次跑完即可：
请帮我在这个空目录用 Next.js（App Router）+ Tailwind CSS 生成一个“个人主页”项目。
要求：
- Pages：/ （首页单页，包含 Hero、About、Projects、Contact 四段）
- 数据来自 /content/data.json（若不存在请生成示例）
- 组件化：Hero.tsx、About.tsx、Projects.tsx、Contact.tsx、Navbar、Footer
- 响应式：移动优先；卡片 1/2/3 列自适应
- 交互：轻量滚动入场；按钮悬停有微动效；卡片悬停阴影
- SEO：基础 title/meta，OpenGraph，favicon
- 可访问性：语义标签、可聚焦、合理 aria
- 脚本：package.json 里加 &quot;dev&quot;、&quot;build&quot;、&quot;start&quot;、&quot;lint&quot;
- 在 README.md 写运行与部署方法（Vercel 优先）

生成后请自动执行：安装依赖、初始化 Tailwind、创建 /content/data.json 的示例、提供占位图片位。
让 Cursor 执行安装（如未自动安装，终端运行）：
npm i
npm run dev
浏览器打开 http://localhost:3000 预览。

第3步｜在 Cursor 生成项目骨架（我给可复制提示词）
打开 Cursor，新建空文件夹并在该文件夹中打开。
在 Cursor 聊天框粘贴下面提示词，一次跑完即可：
请帮我在这个空目录用 Next.js（App Router）+ Tailwind CSS 生成一个“个人主页”项目。
要求：
- Pages：/ （首页单页，包含 Hero、About、Projects、Contact 四段）
- 数据来自 /content/data.json（若不存在请生成示例）
- 组件化：Hero.tsx、About.tsx、Projects.tsx、Contact.tsx、Navbar、Footer
- 响应式：移动优先；卡片 1/2/3 列自适应
- 交互：轻量滚动入场；按钮悬停有微动效；卡片悬停阴影
- SEO：基础 title/meta，OpenGraph，favicon
- 可访问性：语义标签、可聚焦、合理 aria
- 脚本：package.json 里加 &quot;dev&quot;、&quot;build&quot;、&quot;start&quot;、&quot;lint&quot;
- 在 README.md 写运行与部署方法（Vercel 优先）

生成后请自动执行：安装依赖、初始化 Tailwind、创建 /content/data.json 的示例、提供占位图片位。
让 Cursor 执行安装（如未自动安装，终端运行）：
npm i
npm run dev
浏览器打开 http://localhost:3000 预览。

第4步｜填充内容 &amp; 美化样式（完全照做即可）
将你的头像放到 public/avatar.jpg（任意图即可，名称与 PRD 对应）。
打开 /content/data.json，把示例替换成你的真实信息（照我给的 JSON 结构）。
想换色/风格，给 Cursor 发指令：
把主色调改为 #5B8DEF，辅色 #7BDAA1，按钮与链接用主色。
卡片做 8px 圆角，悬停加 8px 阴影，过渡 200ms。
确保暗色模式下对比度达标（可先不做切换按钮）。
保存后浏览器刷新查看效果。

第5步｜本地自测（对照验收单）
手机与桌面都浏览一次（Chrome DevTools 切换尺寸也行）。
点每个外链、按钮；光标悬停看看动效。
若有错位/断行，直接对 Cursor 说：“修复这个问题：{描述现象/附截图}”。
```

以下是Cursor的回复。

![](img/41c6424f99691b349c408c84c2e10847.png)

![](img/e8235e0dfd60425cab9babb1de9ad4f9.png)

![](img/fdd118b372ace481bf207cf9e047bed3.png)

它兢兢业业已经完成了，并且自行安装依赖，并部署了服务器。

![](img/a8403671c07625bbe94ea632cf0c3892.png)

**然后**我们会有大概率发现，左侧的目录里，有一些 红 的文件，如果没红，你需要挨个点一遍看看。

**红 **通常代表 **重点** 或 **错误**。

显而易见，这里应该是错误了，因为我们把**鼠标移到**红色波浪线的地方，我们会发现有 错误提示。

![](img/f98d3bf6c73db0f73bc45cb1bff3a197.png)

这时候该怎么办呢？

把所有红色的文件圈给Cursor，这样比较精准。

但是老金我比较懒，我会直接说。

```
好多文件红了，你帮我都解决了。
自己Review（检查）一遍代码，不要有任何不对的地方。
```

然后老金我发现我没有给他我的个人信息。

因此，我让GPT5帮我总结一份，和上面的差错一起给到Cursor。

再次运行后，发现刚刚还能开启的网页打不开了，遇到这种情况，直接输入“网页打不开了”就行。

它会自己查看原因并进行修复。

![](img/0b6f06488c2f3e93c3cb8a977fc827fa.png)

**一顿修复之后，打开网页**没问题就好了，如果有问题，把你的需求描述清楚即可。

![](img/15ac10d79469a1045c085eae433b1f5a.png)

## 第四步 - 推送到Github，部署至Vercel

#### 推送Github

不知道怎么操作，直接问Cursor，把以下内容直接复制给它。

```
然后我需要进行这两步，推送到Vercel，我需要怎么操作？

第6步｜接入 GitHub &amp; 推送首版代码
第7步｜Vercel 一键上线（拿到公开链接）
```

**不懂的均选“**是”就行，这个弹出来的是个提交忽略配置。

![](img/061e57386efccd669d408506329beaef.png)

我们并没有输入账号，因此它在扫描后，给了两种方案。

直接登录网页即可，Cli这种命令符的不适合小白玩家。

![](img/eaafe1105de3d08a1d26e66a2059c760.png)

进入Github，新建仓库。

![](img/c3f8cc7a2ae582ebc43012ac6f6a2434.png)

![](img/95eda669d56e63a559a9a36222a0c4b1.png)

回到Cursor，粘贴仓库地址。

这里因为老金登录过Github账号，所以它没要求登录，直接就推送好了。

![](img/23be860070f88bc691627e9e9056780d.png)

第一次没有登录时候，它会要求你这样获取个链接密钥，在右上角点开头像后，依次点击创建。

![](img/1039cc66b8da8272a7897acb56de5aa6.png)

#### 部署至Vercel

打开https://vercel.com/，点击新建。

![](img/a3692d7576c1f93b37f18f45664ae955.png)

添加Github账号，并选择我们刚刚在Github上创建的仓库。

![](img/3104312a19cb2e0941273e83d8d4ca61.png)

什么都不用改，直接部署，下方可查看进度。

![](img/7047136b09c726072b8185210ad437be.png)

完成，点击前往仪表盘。

![](img/e75ca4f77e732fc897be5b1676fecdcc.png)

创建完成。

![](img/9d6b567f4f52076d35cca8c534ef70a1.png)

最后我们查看网页即可，页面老金没有进行优化，我们只看过程即可。

访问Vercel给的网址：https://gerenzhuye-iota.vercel.app/

![](img/d850aec55df2af6b1d384a1fcefb0f25.png)

![](img/55ad34338552a91db7ccfa649cb5d6e7.png)

![](img/893a6aef11b01ce8b2b30c504964ea9e.png)

![](img/d5e00b35940f99c3f587c97ff3fd6cef.png)

是不是像那么回事？

之后可以继续让Cursor进行优化，比如跳转链接啊，跳转的项目啊，或者需要的接口啊等等。

**当然，记住老金一开**始所说的 - 万事开头先做流程 , 老金我现在几乎所有事儿都要问问GPT5。

然后也要记得老金在 [**三大心法**](https://tffyvtlai4.feishu.cn/wiki/UqAowXLkTi1bFfkE0wAcIs0Wnzg) 中，依据查理芒格的理念所规划的内容，每次多问问AI是好的。

我们不需要知道我们怎么做才能成功，因为这里除了努力，运气也占了一部分。

但是我们一定要知道我们会怎么失败，那些地方是坚决要规避的。

## 老金有话说

如果你跟老金做完了整个流程，请你在最终点击那个 Vercel 链接，看到属于自己的网页在屏幕上亮起时，暂停片刻。

深呼吸，感受这一瞬间的宁静与回响。

我们构建的，真的只是一堆代码和像素的集合吗？

不。我们跨越了一条看似无法逾越的鸿沟——那条横亘在“想法”与“实现”之间的技术壁垒。

代码，曾是少数人的“咒语”，英语，曾是通往世界的“密钥”。

而今天，我们借助 AI 这个“通用翻译器”，将最纯粹的意图，直接灌注到了数字世界之中。

这个网页，是你在浩瀚信息海洋里点亮的一座灯塔，

是你无声的宣言，是你向世界发出的第一声数字化的“你好”。

它证明了，创造力与执行力，不再被深奥的语法所束缚，而是回归到了思想本身。

所以，老金所说的“我行，你也行”，其深意并非指复制代码的动作，而是指点燃创造火种的能力。

你所掌握的，不是一项搭建网站的“术”，而是驾驭 AI 将蓝图变为现实的“道”。

这趟旅程的终点，从来不是那个网址。

而是你内心升起的信念：只要流程清晰，逻辑正确，万物皆可创造。

今天你构建的是一个网页，明天你或许就能构建一个工具，一个系统，一个属于你自己的解决方案。

忘掉代码，忘掉门槛。

从此刻起，你不是在编程，你是在思想。

**而世界，正聆听你的回响。**

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>