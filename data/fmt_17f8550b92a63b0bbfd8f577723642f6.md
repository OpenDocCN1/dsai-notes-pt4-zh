
# OpenClaw Skill Website

> 来源：[https://gotoaiworld.feishu.cn/docx/RfgQdbnT6oGlSPxTHDickWCwnwc](https://gotoaiworld.feishu.cn/docx/RfgQdbnT6oGlSPxTHDickWCwnwc)

## ClawHub 技能市场

Skill Directory for OpenClaw

[github.com](https://github.com/openclaw/clawhub)

ClawHub is the public skill registry for Clawdbot: publish, version, and search text-based agent skills (a `SKILL.md` plus supporting files). It's designed for fast browsing + a CLI-friendly API, with moderation hooks and vector search.

onlycrabs.ai is the `SOUL.md` registry: publish and share system lore the same way you publish skills.

[clawhub.ai](https://clawhub.ai/)

https://clawhub.ai/

![](img/1d047b542c6c15c5f589c6832673b5ef.png)

ClawHub（clawhub.ai）是开源 AI Agent 项目 OpenClaw 的官方技能注册中心，定位为AI Agent 的 “应用商店”，类比 `npm` 之于 Node.js，核心为 AI Agent 提供可扩展的技能包管理能力，目前已有 3286 + 社区贡献技能，覆盖开发、生产力、通信等多领域，且无准入限制、注重有效技能信号传递。

### 核心功能与特性

1. **技能包全生命周期管理**：支持上传 AgentSkills 技能包，采用类 `npm` 的版本化管理，可回滚版本，安装的技能会记录在本地`/.clawhub/lock.json`文件中，还能一键发布、更新个人技能包；

1. **智能向量搜索**：基于向量技术实现语义化搜索，而非单纯关键词匹配，能精准理解用户需求，快速找到适配技能；

1. **多工具便捷安装**：支持 `npm`、`pnpm`、`bun` 等主流包管理器，可通过 `CLI` 命令一键安装技能文件夹，例如 `npx clawhub@latest install sonoscli`，新手也能快速上手；

1. **技能质量筛选**：区分**精选技能**和**热门技能**，精选技能为平台精选的高可信度技能，热门技能则是下载量高、无安全风险的技能，方便用户快速选择。

### 典型技能分类及代表技能

1. **精选技能**：经平台筛选，包含 Trello（管理看板 / 列表 / 卡片）、Slack（控制 Slack 消息互动）、Caldav Calendar（同步多平台日历，仅 Linux 可用）、Answer Overflow（检索 Discord 社区技术问答）；

1. **热门技能**：下载量靠前，涵盖 self-improving-agent（记录错误实现 Agent 持续优化）、Gog（Google Workspace 全功能 CLI）、Tavily Web Search（AI 优化的网页搜索）、Summarize（多格式内容摘要）、Github（操作 GitHub 各类功能）、Weather（无 `API_KEY` 获取天气）等，还包含 Proactive Agent（将 Agent 转化为主动式伙伴）、Sonoscli（控制 Sonos 音箱）、Notion（管理 Notion 内容）等实用技能。

### 核心使用方式

用户可通过 `ClawHub CLI` 命令行工具完成所有操作，支持自然语言搜索技能、一键安装 / 更新 / 发布，安装的技能会存于本地`./skills`目录（或 OpenClaw 工作区），在下次 Agent 会话中自动生效，且无需了解后端原理，新手友好。同时 OpenClaw 工作区技能优先级高于内置技能，保障用户自定义需求。

### 生态与安全

1. **生态定位**：是 OpenClaw 生态的核心组件，与 OpenClaw Gateway、Agent 等组件配合，让 AI Agent 从基础对话扩展到无限自动化场景，技能以 Markdown 格式的`SKILL.md`为核心，透明可修改；

1. **安全机制**：发布技能的 GitHub 账户需至少一周历史，超 3 次举报的技能会自动隐藏，管理员可对违规技能 / 用户进行管控，同时建议用户白名单安装、前置审查技能权限，避免风险。

## 附：OpenClaw 相关资源

#### 官网与工具

OpenClaw 官网：[openclaw.ai/](https://link.juejin.cn?target=https%3A%2F%2Fopenclaw.ai%2F)

[OpenClaw Github 仓库](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fopenclaw%2Fopenclaw)

[OpenClaw 技能合集](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2FVoltAgent%2Fawesome-openclaw-skills)

[OpenClaw 一键部署工具](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fmiaoxworld%2FOpenClawInstaller)

[OpenClaw 汉化版](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2F1186258278%2FOpenClawChineseTranslation)

[OpenClaw 钉钉插件](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2FDingTalk-Real-AI%2Fdingtalk-moltbot-connector)

[OpenClaw 飞书独立桥接器](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2FAlexAnys%2Ffeishu-openclaw)

#### 部署教程与资源

[阿里云：快速部署 OpenClaw](https://link.juejin.cn?target=https%3A%2F%2Fwww.aliyun.com%2Fbenefit%2Fscene%2Fmoltbot%3Fspm%3D5176.29832386.J_4VYgf18xNlTAyFFbOuOQe.17.5c71396cBFL6yi%26scm%3D20140722.M_10948942.P_120.MO_1774-ID_10948942-MID_10948942-CID_36705-ST_15536-V_1)

[腾讯云：在云端秒级部署 OpenClaw 全能助手](https://link.juejin.cn?target=https%3A%2F%2Fcloud.tencent.com%2Fact%2Fpro%2Flighthouse-moltbot)

[华为云：使用 OpenClaw（Moltbot）搭建个人 AI 助手（飞书）](https://link.juejin.cn?target=https%3A%2F%2Fsupport.huaweicloud.com%2Fbestpractice-flexusl%2Fflexusl_bp_0001.html)

[火山引擎：一键部署 OpenClaw](https://link.juejin.cn?target=https%3A%2F%2Fwww.volcengine.com%2Factivity%2Fclawdbot)

[百度云：极简部署 OpenClaw 打造专属 AI 助手](https://link.juejin.cn?target=https%3A%2F%2Fcloud.baidu.com%2Fproduct%2FBCC%2Fmoltbot.html)

[移动云：本地/云主机部署 OpenClaw 并接入移动云模型](https://link.juejin.cn?target=https%3A%2F%2Fecloud.10086.cn%2Fop-help-center%2Fdoc%2Farticle%2F98120)

[天翼云：天翼云 ×OpenClaw 行动 AI 新生态](https://link.juejin.cn?target=https%3A%2F%2Fwww.ctyun.cn%2Fact%2FOpenClaw)

[京东云：即刻部署 24 小时在线的 Moltbot](https://link.juejin.cn?target=https%3A%2F%2Fwww.jdcloud.com%2Fcn%2Fpages%2Fmoltbot)

[青云：Clawdbot 一键部署零门槛掌控 AI 超级助手](https://link.juejin.cn?target=https%3A%2F%2Fconsole.qingcloud.com%2Fapps%2Fapp-3bou002j)

[亚马逊 AWS：基于亚马逊云科技 Mac 实例部署 OpenClaw，深度苹果生态自动化的最佳选择](https://link.juejin.cn?target=https%3A%2F%2Faws.amazon.com%2Fcn%2Fblogs%2Fchina%2Fopenclaw-deployment-aws-mac%2F)

[Ollama 官方 OpenClaw 本地部署教程](https://link.juejin.cn?target=https%3A%2F%2Fdocs.ollama.com%2Fintegrations%2Fopenclaw)

作者：王吉伟频道

链接：https://juejin.cn/post/7602901195153997876

来源：稀土掘金

著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
