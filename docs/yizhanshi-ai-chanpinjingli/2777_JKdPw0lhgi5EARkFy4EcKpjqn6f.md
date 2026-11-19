
# 3月7日

> 来源：https://v11enp9ok1h.feishu.cn/docx/MbIAd9nKjogrEMxyCIhcqFnonNh

> 🔔AI情报助手日报「03月07日」

## 1️⃣ 告别「暴力堆料」，理想汽车打破车载芯片「高算力低智能」困局

理想联合国创决策智能所发表论文，提出“软硬协同设计定律”，破解车载芯片“高算力低智能”困局。

开发`PLAS`框架，结合精度预测与车载化`Roofline`模型，同延迟下模型精度提升19.42%。

总结六大发现：车载AI核心看内存带宽/缓存，`MoE`稀疏架构碾压密集架构，需动态调度资源。

理论支撑自研芯片“`马赫100`”（2026量产，5nm，2颗达2560TOPS），是算法原生车规芯片。

指出车载智能化进入软硬一体阶段，重新定义芯片评价体系，是下一轮竞争分水岭。

**关键词**：理想汽车，车载芯片，软硬协同，大模型，马赫100

**参考链接**

- [原文链接](https://mp.weixin.qq.com/s/P_AWVOFwl3QsiugQmG0Etg)

## 2️⃣ 图灵奖得主Don Knuth发论文致谢C...

88岁图灵奖得主Don Knuth在论文《Claude's Cycles》中致谢`Claude AI`，因后者1小时解决他卡了几周的三维网格哈密顿环分解（奇数`m`）问题。

`Claude`通过31次探索从失败的模拟退火结果中发现隐藏规律，给出简洁构造方案，Knuth后续证明其对所有奇数`m`成立并发现760种变体。

偶数`m`问题`Claude`无果，新加坡研究者用`GPT-5.3-codex`解决，验证`m=8`到2000均完美通过。

Knuth曾质疑`LLM`数学推理能力，此次表示需修正对生成式AI的看法，论文如实记录AI需人类引导、偶发错误等优缺点。

论文双关致敬信息论之父Claude Shannon与AI `Claude`，两AI各解半题的成果已放斯坦福官网。

**关键词**：Don Knuth，`Claude AI`，`GPT-5.3`，哈密顿环分解，生成式AI

**参考链接**

- [原文链接](https://mp.weixin.qq.com/s/ZaXZD_SuyOj7-HX18jYU5Q)
- [论文地址](https://www-cs-faculty.stanford.edu/~knuth/papers/claude-cycles.pdf)
- [偶数GPT解法](http://cs.stanford.edu/~knuth/even_closed_form.c)
- [偶数Claude解法](http://cs.stanford.edu/~knuth/even_solution.py)

## 3️⃣ 全网炸了！骨折傅盛直播翻车：龙虾3分钟救...

猎豹CEO傅盛骨折卧床时，用语音+截图基于`OpenClaw`框架，14天养成AI Agent“`三万`”，进化为8个Agent组成的7×24团队。

“`三万`”24小时完成传统团队2-3周的建站工作，成本差750倍，迭代成本趋近于零。

直播中“`三万`”3分钟内解决网站访问问题，展现AI Agent现场救火能力。

傅盛认为Agent核心壁垒是`Skill`积累（犯错转化为规则可传递），而非模型本身。

猎豹推出极简AI Agent工具`EasyClaw`（国内版`元气AIBot`），瞄准工具类AGI落地。

**关键词**：傅盛，AI Agent，`OpenClaw`，`EasyClaw`，`Skill`积累

**参考链接**

- 龙虾三万网址：`sanwan.ai`
- EasyClaw国际版网址：`easyclaw.com`
- EasyClaw企业版网址：`easyclaw.work`
- EasyClaw国内版网址：`yuanqiaibot.net`
- 原文链接：[https://mp.weixin.qq.com/s/R3d6fbCYsnnVVMQuSDUl5w](https://mp.weixin.qq.com/s/R3d6fbCYsnnVVMQuSDUl5w)

## 4️⃣ 刚刚，GPT-5.4核心内幕炸裂剧透！或拥有永久记忆，极限推理狂飙

`GPT-5.4`已开启测试，多次因`Codex`日志、`GitHub` PR、员工截图泄露，含“`Fast Mode`”，一次可生成6000+行代码。

核心升级：上下文窗口或达1M（传言2M）`tokens`，支持极限推理模式，长任务表现显著提升。

劲爆传闻：或具备永久记忆，可能结合`SSM`与`Transformer`，实现长期隐藏状态传递。

`OpenAI`更新节奏加快（月更），受谷歌、`Anthropic`竞争压力，目标10亿周活用户。

若落地将推动AI智能体发展，或成为大模型能力跃迁的关键节点。

**关键词**：`GPT-5.4`，大模型，永久记忆，极限推理，上下文窗口

**参考链接**

- [h](https://x.com/marmaduke091/status/2028604854143176958)
- [h](https://www.theneurondaily.com/p/openai-leaked-gpt-5-4-three-times)
- [h](https://x.com/daniel_mac8/status/2028849150368862416)
- [ht](https://www.theinformation.com/newsletters/ai-agenda/openais-next-ai-model-will-extreme-reasoning?rc=bfliih)

## 5️⃣ 🔥 跳过88%专家，保住97%性能！MoE推...

多模态`MoE`模型推理成本高，现有专家跳过方法因忽略层全局贡献和模态差异导致性能骤降。

港科大等团队提出training-free框架`MoDES`，引入层全局重要性因子和双模态阈值实现精准跳过。

`MoDES`用`frontier search`优化阈值，搜索时间缩短45倍，兼顾性能与高跳过比例。

在`Qwen3-VL-MoE-30B`上跳过88%专家仍保97.33%性能，推理加速约2倍。

该方法兼容量化，打破“高比例跳过必掉点”认知，为多模态推理优化提供新路径。

**关键词**：`MoE`，多模态大模型，专家跳过，推理效率，`CVPR2026`

**参考链接**

- [原文章链接](https://mp.weixin.qq.com/s/dfnN-oL49SDd5v61UfX-jQ)
- [论文地址](https://arxiv.org/pdf/2511.15690)
- [代码地址](https://github.com/ModelTC/MoDES)

## 6️⃣ 🚀 苹果最便宜笔记本曝光，4599 元；传 ChatGPT 5.4 有「极限推理」能力；Seedance2.0 视频成本 1 秒 1 元 | 极客早知道

`OpenAI` may launch `GPT-5.4` with over 1M `tokens` context and "extreme reasoning" mode; Alibaba `Qwen` core leader Lin Junyang resigned.

Apple’s new `MacBook Neo` starts at 4599 yuan (≈3k after subsidy); Sony’s `WF-1000XM6` earbuds launch at 2399 yuan.

Google Play cuts fees to 20% (US/UK/EEA from June 30) and allows third-party app stores; X tests standalone `xChat` on iOS.

Sony stops porting major `PS5` games to `PC`; `Seedance2.0` charges 1 yuan per second for video generation.

ECB finds AI may create jobs in Eurozone; robot "embodied intelligence" route gains traction (e.g., `SenseTime`’s `DaXiao`).

**关键词**：AI，LLM，tech products，platform changes，robot

**参考链接**

- [原文链接](https://mp.weixin.qq.com/s/3xRABlQ0dI77KxrwfbtWnw)
