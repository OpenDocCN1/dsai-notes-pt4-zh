# 腾讯版龙虾OpenClaw实测！

> 来源：[https://aiwarts101.feishu.cn/docx/EYisdvs5noTfWrxmMY2cYIsunId](https://aiwarts101.feishu.cn/docx/EYisdvs5noTfWrxmMY2cYIsunId)

原文：

---

下载地址和 5000 额度领取地址

🔗 codebuddy.cn/work/

🔗 codebuddy.cn/profile/usage

## 提示语

> 以后我每次说“安装某个 skill”，先不要安装。

> 如果没有地址，用find-skills在clawdhub上找，

> 接着用skill-vetter做审查并输出：

> 风险评分（0-100），触发的风险项，最小权限替代方案，是否建议安装（是/否） 只有当评分 <= 60 且我明确回复“确认安装”时才执行安装。

第一个提示语执行完成之后，就可以一口气把下面所有提示语需要的 Skill 全部都安装完毕

> 【skills安装】

> find-skills

> skill-vetter

> 【youtube长视频】

> openclaw-youtube-transcript

> youtube-watcher

> summarize

> 【个人知识库】

> Obsidian Official CLI

> markdown-converter

> qmd

> 【想法验证器】

> Multi Search Engine

> github

> 【自我迭代】

> self-improving-agent

> session-logs

> 读取 channels.txt，每天 08:30 执行：

1.  找到过去 24 小时新视频

1.  提取字幕并总结每条视频 3-5 个要点

1.  生成一份 Markdown 日报，包含标题、链接、摘要、建议是否值得完整观看

1.  保存到 daily-youtube-digest/YYYY-MM-DD.md

> 帮我建立本地知识库流程：

> 本地目录【/Users/carl/Downloads/个人知识库初级版】

> 扫描 inbox_docs/ 里的新文件

> 转换为 Markdown 到 kb_docs/

> qmd更新索引

> 回答问题时必须附带 Top3 来源路径和关键引用

> 如果信息不足必须明确说明

> 我给你一个产品想法后，请执行“构建前验证”：

1.  检索同类产品，GitHub 项目，讨论帖

1.  汇总差异点，定价，目标用户

1.  评估拥挤度（高/中/低）

1.  给出结论：继续做/改方向/停止

> 每天 20:30 复盘今天所有 OpenClaw 会话：

> 找出至少3个最关键错误模式

> 找出至少3个最有效动作

> 输出明天要执行的至少3条具体规则（可检查、可量化）

> 生成简短总结并写入复盘日志