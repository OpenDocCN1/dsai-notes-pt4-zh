
# 【单Agent】ModelScope-Agent学习指南

> 来源：[https://datawhaler.feishu.cn/docx/DqaydpsFdovWonxDrYxcrBYxnkf](https://datawhaler.feishu.cn/docx/DqaydpsFdovWonxDrYxcrBYxnkf)

![](img/0d2a249dc74082245b6207806bc883aa.png)

## AI Agent `Engineer`认证

1. 认证地址：https://certs.datawhale.cn/agent/quiz/intro
2. 评分标准
   - 满分 100 分，成功获得 80 分及以上，可获得 Datawhale x 魔搭社区 联合颁发的 Agent Engineer 证书
   - 可反复考试认证，通过学习以下内容来提升分数
3. 证书查询：https://certs.datawhale.cn/agent/quiz/intro，点击查看证书

## 必知必会

> 阅读时，请注意文字左方的小三角，意味着可以 展开 查看详情

![](img/b9cac85f3961bf0314e4641073fc7db1.png)

### 🧠 Agent概念入门

![](img/d5cd2db8328c7148033d3e63b2d455c5.png)

![](img/fcdf2873b87eb07cccfa7be8ae0d6891.png)

<https://player.bilibili.com/player.html?bvid=1cu4y1k7Pg>

### [零代码动手创建ModelScope Agent](https://datawhaler.feishu.cn/wiki/URhIwhft4iUPcRkx7BOcBc6anhe)

> 目前Agent创建项目已有一定更新，界面可能存在差异，但整体步骤没有大变动

> 另外魔搭创空间已支持直接进行Agent应用创建，本教程可能存在一定的滞后，期待你能一起共创更新

> 有意维护可联系 （wx：aomferni）

#### [创建通义千问API](https://dashscope.console.aliyun.com/overview) (新注册用户有一定的限时免费额度)

1. 登录阿里云账号，打开 [DashScope管理控制台](https://dashscope.console.aliyun.com/overview)，开通 DashScope灵积模型服务

![](img/6cda6156f3c009cb6ff4a9089311ab46.png)
![](img/6bbdfca7adf377706df8a21ea9509f1c.png)
![](img/a7107a9fb243fe3688d517aa093753c7.png)

2. 回到控制台，创建 `API-KEY`，并记得复制保存！！！！

![](img/b01a9f5b1844d93980e43e6430d91161.png)
![](img/f53dba30c8a70f0ad6843d40ba2da5e8.png)
![](img/a25c000c39ca140f61ef4793ec1c36f6.png)

3. 后续就可以快乐地使用 `API-KEY` 啦~~~~

4. 最新限时免费的[模型列表](https://dashscope.console.aliyun.com/billing)如下

![](img/be59cc89088c2c4f2227e2b3c4177717.png)

#### 使用 [魔搭Agent创建器](https://modelscope.cn/studios/modelscope/AgentFabric/summary) 快速创建Agent

<https://datawhaler.feishu.cn/sync/BvTeduzYysxOxtbH4AqcUNEZn3e>

#### 发布 属于自己的Agent

<https://datawhaler.feishu.cn/sync/Y9cYdEu2WsA8WUbkNV8cr7UBnMg>

#### 可选：深入代码魔改 (仅简单介绍)

<https://datawhaler.feishu.cn/sync/CdxTdBsWLsubrTbIKILcOfdXnFc>

## 更多学习内容

### 学习教程

- [最简单的教程跳转](https://datawhaler.feishu.cn/docx/DqaydpsFdovWonxDrYxcrBYxnkf#S1e6dHOGvodk5YxgmsscgTTQnTg)
- 手把手 Agent开发教程（觉得不错，star一下）：https://github.com/datawhalechina/agent-tutorial
- 魔搭Agent框架源代码（觉得不错，star一下）：https://github.com/modelscope/modelscope-agent/blob/master/README_CN.md
- [魔搭官方Agent实操系列](https://modelscope.cn/brand/view/agent)教程：
  - 0代码创建、发布并分享一个专属Agent
  - 低代码调用API创建更加酷炫的Agent
  - 将API注册为tool，成为smart API
  - Agent自带的Code Interpreter如何使用
  - 如何在本地/云端创建并发布更定制化的Agent

### 直播分享

- 直播系列分享（记得三连）：https://space.bilibili.com/431850986/channel/collectiondetail?sid=1958604
- 嘉宾PPT：https://datawhaler.feishu.cn/drive/folder/ITikffDkilmehKdLUjdc3PaKntd?from=from_copylink

| 直播名称 | 分享嘉宾 | 视频录播（记得 三连 ） | PPT |
| --- | --- | --- | --- |
| 动手学Agent应用开发 - 开营仪式 | 成晨：魔搭社区技术运营负责人 | https://www.bilibili.com/video/BV1Vg4y1Z73Q | |
| Agent设计 - Prompt和Function Call | 陈嘉诺：Datawhale成员 | https://www.bilibili.com/video/BV1pc411y7f9 | [Agent设计案例及思路.pdf](https://datawhaler.feishu.cn/file/MdQTbUft2o4GNdxdn4nc7Sdvnyf) |
| 魔搭Agent框架原理与实践 | 周文猛：魔搭社区模型建设技术负责人 | https://www.bilibili.com/video/BV1Ma4y1R7GZ | |
| 通义千问大模型原理与实践 | 林俊旸:通义千问开源大模型负责人 | https://www.bilibili.com/video/BV1Ba4y197pv | |
| 通义千问基础语言模型进展 | 周畅：通义千问技术负责人 | https://www.bilibili.com/video/BV1Ce411i7NN | [通义千问基础语言模型进展-周畅.pdf](https://datawhaler.feishu.cn/file/ANyRb1fvzo318PxQKXncWJU2nug) |
| NVIDIA NeMo 助力 RAG在 AI Agent 中的研发与应用 | 金国强：NVIDIA 解决方案架构师 | https://www.bilibili.com/video/BV1Je411e7kQ | [NVIDIA NeMo助力RAG在AI Agent中的研发与应用-金国强.pdf](https://datawhaler.feishu.cn/file/H82PbOEC5oUdphx3wJqc3DiDnUh) |

## 常见QA & 注意事项

> 持续更新回答中，未被回答的问题可以作为思考题思考，
> 也可以通过评论进行讨论或回答，开源贡献帮助到更多人~

### 如果没有Agent思路怎么办？

![](img/3fa198e6a937af1f52b9a4f755a6fe93.png)

> 可以参考上图的流程，详情可以阅读 `agent-tutorial/3.1 Agent应用通用创作思路`

### 如何保护个人创意？防止Prompt泄露？

> 目前可能没有过多的保障措施，如果是零代码构建的Agent，可以通过在Prompt中强调不要泄露
> 如果是开源项目构建的Agent，较为麻烦，在Prompt中强调不要泄露的同时，可能需要再将主Prompt隐藏到环境变量中(这个方案目前没有尝试过)
> 但总得来说，创意本身很重要，但思考、分享并实现的过程更重要，且目前仅是简单的Demo形态，如果后续有更多的优化想法，可以脱离魔搭平台，去构建更强、更有意思的应用
> 目前建议学习者主要将之作为开源贡献的一部分，进行学习和交流使用，如果拿到还不错的名字，影响力提升带来的价值可能更大

### API额度有多少？

> 学习活动中 notebook，服务器，一些通义千问模型都是限时免费的；
> 通义大模型(qwen-max)在3月18日即将开启收费，大家需注意；
> 但如果要用GPU或者更高需求的产品，可能要付费；
> 另外文生图的API每人只有500次额度，请谨慎使用；
> 更多额度计费详情，见[灵积模型服务-计费管理](https://dashscope.console.aliyun.com/billing)

### 需要自己准备算力吗？

> 创空间目前有免费的 CPU环境可供使用，具体可在https://modelscope.cn/my/mynotebook/preset 进行查看，可以在本地运行代码，还可以通过个人云账号授权阿里云的付费实例，
> 其他的自己构建的API服务等可以自行思考算力解决方案

![](img/79e8d8cb8848f69cdf6e9b7a1c12b174.png)

### 如何做Prompt工程？

1. [如果是零代码构建](https://www.modelscope.cn/studios/modelscope/AgentFabric/summary)的Agent，建议直接在 Agent创建器 的界面进行体验和Prompt工程，效果合适后点击【构建】-【发布】，但仅获取 `AGENT_URL`，并在已创建的Agent创空间中，替换这个环境变量，再【重启空间展示】即可

![](img/a3b9c6951b6ce1dd529aa7a45cc34e38.png)
![](img/7b3362bca4011108dfc866f7af822000.png)

2. 如果是代码级的修改，则需要在本地，`config/builder_config.json` 文件中，对相应的字段进行修改并调试，效果合适后 commit 并 push 至魔搭云平台，然后【重启空间展示】

### 创建Agent并发布后，尝试进行对话显示Error

![](img/5682d1f10a234cf8c0613d2ce500dd83.png)

> 这是由于勾选了【高德天气】的tools，但没有在发布的环境变量中添加 高德天气的 `API-KEY`，
> [需要](https://github.com/Jin-Zhang-Yaoguang/agent-tutorial/blob/main/notebook/%E7%AC%AC%E4%BA%8C%E7%AB%A0%EF%BC%9AAgent%E5%AE%9E%E8%B7%B3/%E9%AB%98%E5%BE%B7%E5%BC%80%E6%94%BEAPI%E5%AE%9E%E8%B7%B3.md)先申请高德开放平台，注册后申请 `ak`，参考教程进行高德 `API-KEY` 的申请，并将对应的key设置为环境变量 `AMAP_TOKEN`

### 如何针对已有的Agent代码进行魔改？

1. 下载发布的Agent的代码：直接复制链接环境变量中的 `AGENT_URL` ，粘贴到浏览器地址栏下载

2. 解压并修改对应代码

3. 将创空间的代码`git clone`到本地：复制对应的git命令，在本地找一个文件夹，打开终端界面，粘贴命令并使用

![](img/4086ff3fbf377593c648764d29ca4dca.png)
![](img/6affce8dc2bd3347ddee0908698d57b7.png)
![](img/3d82dc8afc26fa708a87351f7f05e26c.png)

4. 将第二步修改后的代码复制到新的目录下，并修改`ReadMe.md`中的启动文件

![](img/7b02372d27942582b0d9b275840b0043.png)

5. 最后将代码push到创空间中

```
git add .
git commit -m "随便写点啥"
git push
```

![](img/e9554dfbcc5fc5b757dd0a782919c395.png)
![](img/ad9272225bb3195a538272480154b395.png)
![](img/3b6938a34a04bcc7d287b6bb753981bc.png)

6. 最后再刷新创空间，并重新上线空间即可

![](img/653a25299cd5eca5ad084d6444cf308d.png)

## 学习简介(学习活动已结束)

| 学习阶段 | 学习内容 | 建议动作 |
| --- | --- | --- |
| Part1 | Agent入门，自由组队 | 多看、多听、多学、多想、多讨论 |
| Part2 | Agent进阶，磨炼能力 | 学习相关技术，提升个人实力 |
| Part3 | 开发Agent，提交作品 | 完成比完美重要 |

- ModelScopeAgent框架原理与实践
- [通义大模型原理与实践](https://www.bilibili.com/video/BV1Ba4y197pv/)
- Agent的function call与tools设计
- Agent的Prompt设计

### 面向对象

- 有一定创新思维与Python基础
- 希望在实际中运用Agent框架与AIGC技术，构建创新应用
- 有足够的精力投入，有创新想法，热爱分享

### 学习亮点

- 学习内容：
- 魔搭开源的Agent框架，通义大模型鼎力支持
- 零门槛上手Agent应用体验 与 手把手的Agent开发教程
- 学习支持：
- 助教团队全程答疑解惑，带你玩转Agent
- 刻意练习，磨炼Agent开发技能，提供免费 API 与部署平台
- 组队学习，遇见志同道合的伙伴，让学习不再孤独

## Agent作品汇总

### 获奖作品

赛事所有获奖作品公示：https://tianchi.aliyun.com/competition/entrance/532168/customize429

![](img/4474c4a7e624dbc213630a0ecd99942b.png)
![](img/815c3a5de28a3d9a3944d1bcbce29027.png)

### 所有作品展示

> 本手册由  编写，觉得不错在下方点个赞吧，更多建议与反馈，欢迎评论或联系玉鑫（加V：a1434088958）

## 祝大家都能享受 Agent 带来的乐趣！！！

扫描下方二维码关注👸号：@Datawhale

更多组队学习和赛事等你参与~

![](img/3395e7c3be7c42d90d9810ba3e7dc0d1.png)

Datawhale 是一个专注于数据科学与 AI 领域的开源组织，汇集了众多领域院校和知名企业的优秀学习者，聚合了一群有开源精神和探索精神的团队成员。

Datawhale 以“ for the learner，和学习者一起成长”为愿景，鼓励真实地展现自我、开放包容、互信互助、敢于试错和勇于担当。

同时 Datawhale 用开源的理念去探索开源内容、开源学习和开源方案，赋能人才培养，助力人才成长，建立起人与人，人与知识，人与企业和人与未来的联结。
