
# 3🈷️26日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/AdDwdeEtloy9NbxnK37c1DQ7n1d](https://v11enp9ok1h.feishu.cn/docx/AdDwdeEtloy9NbxnK37c1DQ7n1d)

> 🔔AI情报助手早报「03月26日」

> ✨✨✨✨✨✨✨✨

## 🤖 1️⃣ [Agent重塑软件与互联网产业新范式](https://mp.weixin.qq.com/s/c0LfxTOoFLKGKJ-qUpr4fA)

2026奇点智能技术大会初版日程出炉！

+ 2026奇点智能技术大会（4.17-18上海）由CSDN与奇点智能研究院联合举办，聚焦Agent工程攻坚与商业闭环，是原全球机器学习技术大会升级而来。
+ 大会云集BAT、英伟达等顶尖机构一线AI实践者，围绕Agent系统、AI原生应用、AI Infra等前沿议题展开分享。
+ 首日主会场含奇点智能李建忠（Agent重塑产业范式）、原阿里云黄非（AI发展周期）演讲，下午设多模态、智能体系统等专题。
+ 次日聚焦AI原生应用创新、具身智能、多模态落地等，覆盖企业级Agent与开源生态实践。
+ 注册可领取首发《AI原生软件研发成熟度模型（AISMM）》白皮书，定义五级跃迁路径助力企业AI转型。

关键词：Agent、奇点智能技术大会2026、AI原生应用、AISMM白皮书、大模型落地

🔗 文中引用的链接

+ [原文链接](https://mp.weixin.qq.com/s/c0LfxTOoFLKGKJ-qUpr4fA)

## 🤖 2️⃣ 我在🛰️免费养「龙虾」，玩到上瘾，这组C...

+ 腾讯构建“龙虾矩阵”，含WorkBuddy（自研Agent）、云端/本地虾等产品，覆盖办公多场景，支持多平台接入。
+ WorkBuddy可通过🛰️ClawBot远程操控，完成资讯整理、文件分类、报销处理、论文总结甚至写稿等任务。
+ 自研WorkBuddy无需云端部署，1分钟配置，支持多顶级大模型，提供零代码扩展和多Agent并行能力。
+ 🛰️ClawBot与WorkBuddy联动降低门槛，用户用文字/语音指令即可远程完成电脑操作，提升办公效率。
+ 黄仁勋称`OpenClaw`是“推理时代操作系统”，腾讯等大厂加速Agent布局，重构软件连接方式。

关键词：腾讯Agent（龙虾）、WorkBuddy、🛰️ClawBot、远程办公助手、AI自动化

🔗 文中引用的链接

+ [原文链接](https://mp.weixin.qq.com/s/YuKaeXNdKjX8TZWxRwmL-g)
+ WorkBuddy Credits领取
+ [百万Credits悬赏活动](https://mp.weixin.qq.com/s/gOOJuRzERBMwqyifaRYu1g)

## 🔔 3️⃣ [突发｜立即检查你的Python库！`LiteLLM`](https://mp.weixin.qq.com/s/VrQ65bEnXJWSKGjmYuyy9A)被投毒，Karpathy警告，马斯克关注

+ Python库`LiteLLM`（月下载超9700万）在`PyPI`被投毒，恶意版本为1.82.7/1.82.8，已撤回，需立即检查安装版本。
+ 攻击可窃取SSH密钥、云服务商凭证等敏感信息，还能横向移动渗透K8s并植入持久化后门。
+ 攻击因代码bug（分叉炸弹致机器崩溃）被开发者发现，Karpathy称若更隐蔽可能难察觉。
+ 攻击者用窃取的`PyPI`发布令牌直接上传毒包，绕开GitHub代码审查，官方仓库仍干净。
+ 应对需检查安装时间、移除毒包清缓存、排查持久化文件、重置所有泄露凭证。

关键词：`LiteLLM`，供应链攻击，`PyPI`，恶意代码，凭证泄露

🔗 文中引用的链接

+ FutureSearch事件详情博客
+ [Snyk事故详细报告](https://snyk.io/articles/poisoned-security-scanner-backdooring-litellm/)
+ [原文链接](https://mp.weixin.qq.com/s/VrQ65bEnXJWSKGjmYuyy9A)

## 🤖 4️⃣ [对抗KV Cache压缩的脆弱性](https://mp.weixin.qq.com/s/81wiPTdyebb9nGfAvoU2CQ)：两行代码以最坏风险控制防御底层假设崩塌

+ 中科大团队发现现有KV Cache压缩方法依赖的“重要性稳定”假设存在脆弱性，特定时刻平均指标会失效。
+ 提出`DefensiveKV`，仅两行代码（最坏风险估计+自适应先验修正），用“最坏风险控制”替代平均优化。
+ 实验显示其显著降低损失，`Layer-DefensiveKV`在`Llama-3.1-8B` 20% cache预算下损失仅2.1%（较基线提升4.6倍）。
+ 方法可与现有技术结合，开源代码含1小时复现Demo，纠正了之前评测缺陷。
+ 研究重新定义压缩目标，强调防御性策略，为长上下文推理鲁棒性提供新思路。

关键词：KV Cache压缩，最坏风险控制，`DefensiveKV`，大模型推理，ICLR 2026

🔗 文中引用的链接

+ [原文链接](https://mp.weixin.qq.com/s/81wiPTdyebb9nGfAvoU2CQ)
+ [论文链接](https://openreview.net/forum?id=nJgS06sX3O)
+ [代码链接](https://github.com/FFY0/DefensiveKV/tree/main)

## 🤖 5️⃣ [🛰️养虾，官配WorkBuddy真香！](https://mp.weixin.qq.com/s/8I2Rk9_ri6R7sa-LKXVDlA)141位专家24h待命，一个人单挑全公司

+ WorkBuddy是🛰️ClawBot官配，扫码直连🛰️后，可通过手机语音/文字远程操作电脑完成文件发送、日程安排等复杂任务。
+ 内置12大行业141位AI专家，7x24h待命，能输出具备行业深度的解决方案（如增长黑客策略、AI行业趋势报告）。
+ 支持创建多Agent协作的AI军团，并行推进任务，简单提示词即可完成复杂项目（如复现电影《Her》中的AI助手）。
+ 具备自动定时任务功能，设置后永久执行，无需手动触发，可定时提醒或生成成品文件。
+ 核心优势：本地执行数据安全、1分钟上手0部署、支持多IM直连，国内版免费且注册送5000 Credits。

关键词：WorkBuddy、🛰️ClawBot、AI办公助手、行业专家、自动定时任务

🔗 文中引用的链接

+ 福利1领取地址：
+ [https://www.codebuddy.cn/profile/usage](https://www.codebuddy.cn/profile/usage)
+ 百万Credits悬赏活动：百万 Credits 悬赏 · 秀出你的 WorkBuddy Claw 实战

## 📉 6️⃣ 美国2月9万裁员狂潮，超过去一年！亚马逊...

+ 2026年2月美国科技行业裁员达9万，超2025年全年（5.5万），亚马逊、甲骨文、Block为主要裁员企业。
+ 裁员核心驱动是AI替代人力，企业认为AI可让更少人完成更多工作，而非经济或行业周期因素。
+ 甲骨文为扩建AI数据中心计划筹资500亿美元，拟裁2-3万员工释放现金流用于AI基建。
+ Block裁员4000人（占40%），称智能工具改变运营模式，小团队借助AI可高效完成更多任务，股价上涨20%。
+ 流程型白领、经验浅成本高的知识工作者、中层管理者三类人群受AI裁员影响较大，未来职场竞争力取决于善用AI的能力。

关键词：AI裁员、科技行业、AI基建、职场变革、智能工具

🔗 文中引用的链接

+ [https://x.com/cryptopunk7213/status/2031914914768449817](https://x.com/cryptopunk7213/status/2031914914768449817)
+ [https://x.com/gothburz/status/2031778265958842541](https://x.com/gothburz/status/2031778265958842541)
+ [https://x.com/TechLayoffLover/status/2031904439653597419](https://x.com/TechLayoffLover/status/2031904439653597419)
+ [https://x.com/lukOlejnik/status/2031257644724342957?s=20](https://x.com/lukOlejnik/status/2031257644724342957?s=20)
