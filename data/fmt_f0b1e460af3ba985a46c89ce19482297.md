
# 1🈷️25日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/NqOEdD86toRbJSxHFGKcgyBknbb](https://v11enp9ok1h.feishu.cn/docx/NqOEdD86toRbJSxHFGKcgyBknbb)

> 🔔AI情报助手晚报「01月25日」

## 1️⃣ 🎯 不读博士，照样进OpenAI！o1核心成...

- OpenAI研究员、o1核心贡献者Noam Brown分享非典型经历，证明无PhD也能进入OpenAI、DeepMind等前沿AI公司。
- 非典型研究员有共同特质：主动性强、公开研究、工程能力在线、会PR、不执着头衔。
- 举例：Keller Jordan（本科，推特学术+套瓷进OpenAI）、Sholto Douglas（本科，业余研究获谷歌邀请）等。
- AI大厂（如Stability AI）更多招无PhD者，看重实际做事能力而非学位。
- 业内建议：早进工业界更好，博士易错过研究范式变化，AI Lab缺工程能力强的人。

关键词：非典型AI研究员，OpenAI o1，工程能力，无PhD入职AI大厂，工业界优先。

🔗 文中引用的链接

- [Noam Brown的X帖子](https://x.com/polynoamial/status/2014084431062114744)
- [YouTube频道](https://www.youtube.com/channel/UC5xLV_gJAP9psKcyrJ3ZIcw)
- Marketing AI Show Episode74
- Sholto Douglas访谈

## 2️⃣ 🔮 DeepSeek V4 神秘代码曝光，智谱开源最强 Flash，字节扣子 2.0 重磅升级！| AI Weekly 1.19-1.25

- DeepSeek V4或2月中旬发布，代码库现`MODEL1`标识，支持Blackwell架构和Engram记忆系统，主打代码生成能力。
- 智谱开源GLM-4.7-Flash（30B参数，激活仅3B），比上代快40%，SWE-bench得分59.2超gpt-oss-20b，32GB Mac可本地运行。
- 字节扣子2.0升级为职场伙伴，Agent支持长周期任务主动汇报，新增技能商店一键安装模块。
- 百度文心5.0正式版上线，2.4万亿参数原生全模态，LMArena文本榜国内第一、全球第八。
- 阿里开源Qwen3-TTS，3秒克隆声音，支持10种语言，纯语言模型实现端到端建模。

关键词：DeepSeek V4，GLM-4.7-Flash，扣子2.0，文心5.0，Qwen3-TTS

🔗 文中引用的链接

- [原文链接](https://mp.weixin.qq.com/s/WXLp9DNgm0UUTicsevCKUA)
- [智谱开放平台](https://www.bigmodel.cn/)
- Qwen3-TTS on Hugging Face
- Qwen3-TTS on ModelScope

## 3️⃣ 🔥 OpenAI绝地反击！Codex大脑首曝，8亿用户极限架构硬刚Claude

- OpenAI针对Anthropic的Claude Code发布两大技术：Codex的Agent Loop架构揭秘与单PostgreSQL主库支撑8亿用户的基建方案，强势回应竞争。
- Codex的Agent Loop是核心"大脑"，通过观察-思考-行动-反馈闭环实现AI从聊天到独立干活的飞跃，包含Prompt构建、模型推理等五步流程。
- 为解决Agent开发痛点，OpenAI用Prompt Caching将长对话成本从平方级降为线性，用Compaction压缩对话保留隐性理解以突破上下文窗口限制。
- 单PostgreSQL主库+50只读副本支撑8亿用户，关键优化含`PgBouncer`连接池、缓存锁定机制、查询优化与负载隔离等，读请求远多于写请求。
- OpenAI通过此次更新展示Agent架构成熟度、强大基建能力，与Claude Code的竞争将推动AI编程工具迭代，最终受益开发者群体。

关键词：Agent Loop，Codex，PostgreSQL，Prompt Caching，Compaction

🔗 文中引用的链接

- [原文章链接](https://mp.weixin.qq.com/s/6TEOP0_jZvP8Lp3D9P7nhA)
- OpenAI Codex Agent Loop博客
- [OpenAI工程师X平台推文](https://x.com/gdb/status/2014744842941956606)

## 4️⃣ 🔔 挑战Claude Code？OpenAI Codex发布月将至，今先揭秘智能体循环

- OpenAI CEO山姆·奥特曼宣布下周起一个月将发布大量Codex相关内容，特别强调网络安全主题。
- OpenAI官方博客揭秘Codex CLI核心架构“智能体循环”，通过Responses API协调用户指令、模型推理与本地工具执行。
- 智能体循环含输入、推理、解码、决策（生成回复或工具调用）、执行重试等步骤，循环至生成助手消息结束一轮对话。
- Codex通过提示词前缀一致触发缓存优化性能，用自动压缩管理上下文窗口，保障零数据保留（ZDR）下的隐私与高效开发。
- 后续将深入CLI架构、工具调用实现及Codex沙箱模型等内容。

关键词：Codex智能体循环，Responses API，提示词缓存，上下文压缩，ZDR

🔗 文中引用的链接

- [原文链接](https://mp.weixin.qq.com/s/dEpQBtt3wHgzPz_HEIDVdA)
- [OpenAI官方博客地址](https://openai.com/index/unrolling-the-codex-agent-loop/)
- [Codex开源仓库](https://github.com/openai/codex)

## 5️⃣ 🏄 OpenAI单个PostgreSQL支撑8亿用户，是神作还是妥协？

- OpenAI用单主PostgreSQL+近50副本支撑ChatGPT8亿用户，未选分片因现有工作负载分片复杂耗时，需修改数百端点可能数月至数年。
- PostgreSQL仅处理核心读写负载，新功能/表默认用Azure CosmosDB等分片系统，当前部署不再允许加新表。
- 通过`PgBouncer`连接池（降连接时间）、缓存锁机制（防未命中风暴）、避免复杂多表连接等优化推高PostgreSQL极限。
- 处理数百万QPS读负载时P99延迟保持低两位数毫秒，达五个九可用性，但用户体验仍有侧边栏加载长等问题。
- 该案例是传统数据库优化的成果也是妥协选择，未来不排除分片可能，架构需从场景实际出发。

关键词：OpenAI，PostgreSQL，单主架构，ChatGPT，数据库优化

🔗 文中引用的链接

- [原文链接](https://mp.weixin.qq.com/s/vvZ4I_6m90r_C0MqQDDnlw)

## 6️⃣ 🔔 OpenAI 架构大揭秘：一台 PostgreSQL 撑起了 8 亿用户

- OpenAI的ChatGPT靠1台PostgreSQL主库+近50个全球只读副本支撑8亿用户，负载年增超10倍。
- 上游故障（缓存未命中、昂贵查询、写入风暴）引发负载飙升，重试机制放大成恶性循环。
- 迁移写密集型到Azure Cosmos DB，应用层减冗余写入，用缓存锁定防缓存未命中冲击。
- 优化连接池（`PgBouncer`降延迟）、工作负载隔离，测试级联复制，多层面限流防过载。
- 支撑百万QPS，p99延迟两位数毫秒，五个九可用性，仅1次SEV0故障（ImageGen发布时）。

关键词：OpenAI，ChatGPT，PostgreSQL，数据库优化，缓存锁定

🔗 文中引用的链接

- [OpenAI博客原文](https://openai.com/index/scaling-postgresql/)
- [视频解读](https://youtu.be/ubpUjovBMAM)
