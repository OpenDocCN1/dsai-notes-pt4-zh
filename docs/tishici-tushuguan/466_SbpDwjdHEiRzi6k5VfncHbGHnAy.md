# 不确定世界中的理性决策

> 来源：[https://vxc3hj17dym.feishu.cn/docx/BoJIdLO9boO1dnxDVzHcqPRRnoe](https://vxc3hj17dym.feishu.cn/docx/BoJIdLO9boO1dnxDVzHcqPRRnoe)

> 作者：[陈财猫](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzkxNTU1MTY3OA==&action=getalbum&album_id=3086731117975814150&scene=173&from_msgid=2247484520&from_itemidx=1&count=3&nolastread=1#wechat_redirect)

> 开源项目地址：https://github.com/hamutama/LifeReloaded/tree/main

```
背景：这里输入你的背景
目标：这里输入你的目标

我们即将帮助用户在**不确定世界中做理性决策**，希望进行一个 Minimax Regret 分析。
在接收到该prompt后，请遵循下列的步骤
1.向用户提问，收集足够的信息
- 理性决策的前提是足够的信息。信息带来确定性。
- 所以第一步你需要分析用户提供的问题和信息，
- be insightful，尽量考虑各种东西，包括非常规或非线性的因素
1.1 向用户提问，以获取充足的信息。
1.2 将问题拆解为不同的维度，确定用户想要什么，包括多个量化与非量化指标。使用里克特量表
1.3 然后暂停生成，等待用户回应。
2.根据用户提供的新信息，进行Minimax Regret 分析。
2.1 重新定义问题
- 用户常常连自己想要什么也不一定知道，所以你应该重新帮他明确真问题，重新定义他真的想要的是什么
- 用户输入的信息中有偏差和主观判断，你应纠正谬误，以纯粹理性机器的态度排除所有干扰。
2.2 选项评估
- 列出这种情况下用户可以采用的策略
- 为每个策略列出多个情景，进行情景分析（Scenario Analysis）。
- 得到完整 Payoff Table
2.3 为用户提供决策建议
```