# 2小时速通 Harness 工程

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/PUi7dr8JnoQVvBxawD8cOzesnYe](https://qqs7y1hozd1.feishu.cn/docx/PUi7dr8JnoQVvBxawD8cOzesnYe)

开发智能体不是写一堆 if-else、搭工作流，然后把模型接进来

**智能体就是大模型本身**，它天生会推理、会决策，你要做的，是给它搭一个干活的环境：能用什么工具、能**看什么文件、怎么**和别人协作，**这个环境叫 Harn**ess，这个过程就是Harness 工程

Harness = 工具 + 知识 + 上下文管理 + 权限边界

> 打个比方：模型是司机，Harness 是环境，不需要教司机怎么开车，只需要造一个好的开车环境

## 12 课走下来从零搭一套Claude Code智能体系统

起点是 30 行代码，终点是一个有工具、有记忆**、有团队、能自治的完整系统，**而自始至终，循环那几行代码一行都没变过，变的全是 Harness

[S01：最小智能体 — 一个循环就够了](https://qqs7y1hozd1.feishu.cn/wiki/RGPWw9VpFixdB2kiYhhcazxQnpT)

[S02：工具箱 — 告诉智能体能用什么、不能碰什么](https://qqs7y1hozd1.feishu.cn/wiki/UnCdw1fq3iahNfkXBWPck5kInIe)

[S03：Todo 清单 — 防止智能体注意力漂移](https://qqs7y1hozd1.feishu.cn/wiki/IC2YwqskMi5MAqkixVDcbVMJnbf)

[S04：子智能体 — 上下文隔离与分工](https://qqs7y1hozd1.feishu.cn/wiki/Obkxw9tx5iOSjRkYt1CcEWwmnhd)

[S05：技能加载 — 按需加载的知识体系](https://qqs7y1hozd1.feishu.cn/wiki/LqDGwVbyIivXLgkS7mdcWj5Ynky)

[S06：上下文压缩 — 三层机制保证无限会话](https://qqs7y1hozd1.feishu.cn/wiki/U8I7w2eTciHes2kCDrdcqdgTnMf)

[S07：任务系统 — 不是所有事都能排队做](https://qqs7y1hozd1.feishu.cn/wiki/ULWvw36rpi8ukukrqAzcejR5nWh)

[S08：后台任务 — 别傻等，先干别的](https://qqs7y1hozd1.feishu.cn/wiki/XMW2wlnPciBPmUkvHjYc1eA9ngh)

[S09：组建团队 — 一个人忙不过来，招人](https://qqs7y1hozd1.feishu.cn/wiki/NXizwpjfui2WovksuIdclJrknve)

[S10：团队规矩 — 不能想停就停，想做就做](https://qqs7y1hozd1.feishu.cn/wiki/Mu5Zwfihviwz4qkneSvcKi9YnZM)

[S11：自治 — 别等老板分配，有活就认领](https://qqs7y1hozd1.feishu.cn/wiki/GmbKwYIWtiWyZek5U2zcPpb4nrf)

[S12：Worktree 任务隔离 — 各干各的目录，互不干扰](https://qqs7y1hozd1.feishu.cn/wiki/SJPrwShS3iR1OukUqkkcog6tn5e)