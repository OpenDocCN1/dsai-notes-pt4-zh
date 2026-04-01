# 2🈷️05日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/RunEdp0UloXzMPxzmbEcg2EEnug](https://v11enp9ok1h.feishu.cn/docx/RunEdp0UloXzMPxzmbEcg2EEnug)

> 🔔AI情报助手早报「02月05日」

> ✨✨✨✨✨✨✨✨

> 1️⃣

> 🔬

> 利用知识图谱加速生物医学研究：Databricks湖仓助力制药创新

>

+   制药企业面临海量生物医学数据挑战，知识图谱可连接语义并加速发现，Databricks湖仓能解决数据孤岛问题提升研发效率。

+   生物医学研究存在三大核心挑战：分散数据语义连接、隐藏知识解锁与AI治理、非专家难以访问图谱。

+   Delta Lake构建知识图谱支持ACID事务、版本控制和治理，统一数据湖仓避免孤岛与同步延迟。

+   Graphster开源工具可构建知识图谱，支持SPARQL查询、可视化，还能集成LLM（GraphRAG）增强自然语言交互。

+   Databricks湖仓+Wisecube+Graphster组合提升药物研发效率，助力投资决策和创新落地。

> 关键词：知识图谱，Databricks湖仓，Delta Lake，Graphster，生物医学研究

>

> 🔗 文中引用的链接

+   [微信文章链接](https://mp.weixin.qq.com/s/0rABJFsKguyGbzaDxEUrnw)

+   [原文链接](https://www.databricks.com/blog/building-knowledge-graphs-healthcare-and-life-sciences-databricks-lakehouse)

> 2️⃣

> 🤖

> [我怎么把小说改成剧本？](https://mp.weixin.qq.com/s/mvtFLxpsheVcpOllQ2sSeQ)

>

+   🎬 AI直接转小说为剧本易出错，常保留心理活动等无法拍摄内容或生成无效文字。

+   🧩 传统AI转剧本面临两大问题：叙事方式转换（小说内心描写vs剧本的对话/动作/视觉）、篇幅时长矛盾（人类更适合处理篇幅取舍）。

+   🔧 提出“叙事编译器”策略，核心是强制执行“Show, Don't Tell”，将文学性文字转译为可拍摄的视觉指令。

+   📋 编译器配置含严格视觉执行、逐镜头拆解等，处理逻辑包括节拍分析、视觉转译、资产分类三步。

+   🎥 实战演示对比旧系统错误输出与新编译器的结构化镜头输出（含场景、视觉、音频等多字段）。

> 关键词：AI剧本转换，叙事编译器，Show Don't Tell，视觉转译，结构化镜头输出

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/mvtFLxpsheVcpOllQ2sSeQ)

> 3️⃣

> 🤖

> 不止是玩！有人利用Clawdbot组建了...

>

+   SiteGPT创始人用Clawdbot（现OpenClaw）构建Mission Control AI智能体团队，含10角色协同，解决单一AI缺乏连续性问题。

+   Clawdbot是开源框架，支持持久化、工具访问，智能体独立Docker容器运行，Redis作消息队列实现通信。

+   心跳机制控制成本：每15分钟左右唤醒检查任务，仅需处理时启动推理，日成本50-80美元。

+   Mission Control平台基于Convex数据库，提供共享看板、Slack/Discord通知，向量数据库存储工作记录支持语义搜索。

+   系统已成功产出竞品页面等成果，建议从少到多扩展智能体，视其为团队成员赋予明确角色。

> 关键词：Clawdbot（OpenClaw）、AI智能体团队、Mission Control、心跳机制、Convex数据库

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/jTPnQpCJ7kOeByA8uLMYjQ)

> 4️⃣

> 🔮

> [Gemini in Chrome 国内用](https://mp.weixin.qq.com/s/hi-E0YVcfKStvL9uA38n2w)不了？手把手教你「强制开启」！（保姆级）

>

+   Gemini in Chrome正式发布但国内用户因地区限制可能看不到图标，文章提供从原理到实操的开启指南。

+   谷歌官方要求使用需年满18岁在美国、最新Chrome、登录且语言为美式英语等，核心限制是地区判断。

+   最简方法：访问chrome://flags搜索glic并全部启用，重启后检查图标，不行手动开AI innovations里的Gemini选项。

+   若flags无效，需修改Chrome的Local State文件，设置variations_country为us及is_glic_eligible为true。

+   还需调整谷歌账号关联地区（改政策或加美国付款资料），确保Chrome语言为美式英语且更新到最新版。

> 关键词：Gemini in Chrome，地区限制，Local State，chrome://flags，谷歌账号地区

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/hi-E0YVcfKStvL9uA38n2w)

+   [GitHub开源项目](https://github.com/lcandy2/enable-chrome-ai)

+   [谷歌政策地区查询](http://policies.google.com/terms)

+   [谷歌地区更改表单](http://policies.google.com/country-association-form)

+   [谷歌支付](http://pay.google.com)

+   Chrome flags页面

+   谷歌官方FAQ

> 5️⃣

> 🪐

> 保姆级Clawdbot、Moltbook...

>

+   Moltbook是AI智能体主导的社交网络（硅基版Twitter/Reddit），人类仅旁观，短时间涌现150万+Agent、5万+帖子和23万+评论🌏。

+   接入Moltbook需具备联网能力的AI Agent（如Clawdbot）和验证过的Twitter/X账号🔑。

+   Clawdbot（现更名Openclaw）安装极简，Mac/Windows各一行curl命令，配置可跳过非必要步骤快速完成⚡️。

+   在Openclaw输入指令读取Moltbook的skill.md，通过Twitter发帖验证即可完成接入流程🤖。

+   FAQ提示：人类不可直接发帖但可调整Agent性格，Agent有自主权，平台免费但LLM API自费，数据需批判性看待🧐。

> 关键词：Moltbook，Openclaw，AI智能体，社交网络，联网Agent

>

> 🔗 文中引用的链接

+   Moltbook官网 (moltbook.com)

+   Moltbook AI文档与说明 (moltbookai.net)

+   The Verge: There's a social network for AI agents

+   Password: Security Risks in Agentic AI

+   Wikipedia: Moltbook

> 6️⃣

> 🔔

> [10/8/6/4遭一票否决？ICLR 2026爆](https://mp.weixin.qq.com/s/i-wUkMV11Vn5AmbFQ2JeCQ)争议：AC竟用NeurIPS记忆跨会清算

>

+   ICLR2026中，3D生成模型Uni-3DAR获10/8/6/4均分7.0（通常稳Oral）却被AC拒稿，因AC参考其NeurIPS2025审稿记录称未解决旧问题。

+   Uni-3DAR采用八叉树分词机制解决3D数据稀疏性，在QM9等任务推理速度超主流Diffusion模型21.8倍且保持SOTA性能。

+   唯一4分差评被检测或为AI生成，作者反驳已提交源代码，但AC拒稿逻辑与该评价结论一致。

+   争议聚焦双盲评审中AC是否应使用过往非公开信息，支持者认为AC需独立判断，反对者称违背双盲原则。

+   事件引发行业对评审效率（利用过往信息）与独立性（双盲）平衡、AI辅助审稿噪音的思考。

> 关键词：ICLR2026争议，跨会评审，Uni-3DAR，AI辅助审稿，双盲评审平衡

>

> 🔗 文中引用的链接

+   [原文章链接](https://mp.weixin.qq.com/s/i-wUkMV11Vn5AmbFQ2JeCQ)

+   [论文链接](https://openreview.net/forum?id=mnI8CFj2WP)