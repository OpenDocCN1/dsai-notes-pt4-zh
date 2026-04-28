# 豆包·角色扮演模型的n种玩法

> 来源：[https://gotoaiworld.feishu.cn/docx/VysOdbF7CoabgUx9oNXcuJ72nhg](https://gotoaiworld.feishu.cn/docx/VysOdbF7CoabgUx9oNXcuJ72nhg)

[豆包·角色扮演模型的n种玩法](https://bytedance.larkoffice.com/wiki/QHoxw31txiOVFukgVr3cCKi8ngg)

> 开始之前，我们先理解：大模型本身只是一个充满知识和逻辑能力的“大脑”，它本身没有个性、没有目的，而“系统提示词”，就是为这个大脑注入记忆、定义其人格的“蓝图”

> 我们可以这样理解：

+   大模型熟知人类的一切知识、语言和情感模式，但 ta 并不知道自己是谁

+   系统提示词是你递给 ta 的剧本。这个剧本里写明了 ta 的身份、性格、说话风格、过往经历等信息

> 当“演员”拿到了“剧本”，一个虚拟角色就诞生了～

本文档将展开[豆包·角色扮演模型](https://console.volcengine.com/ark/region:ark+cn-beijing/model/detail?Id=doubao-1-5-pro-32k&utm_campaign=2025H2&utm_content=gongjuxiang&utm_medium=xianshang&utm_source=LBM-zhyx&utm_term=dyjx_wendang)在社交陪聊上的玩法，这是一个强大的文本模型，能够精准理解和扮演特定角色，进行高度拟人化和符合角色设定的文本对话。一切精彩，始于你的想象力。

| 模型名称 | model_id |
| --- | --- |
| 豆包·角色扮演模型 | doubao-1-5-pro-32k-character-250715 ✅ 最新版本 |
|  | doubao-1-5-pro-32k-character-250228 |

### 基础对话

**最简单的角色 = 大模型 (能力**基座) + 人设提示词 (即系统提示词)

**人设提示词为模型创建人格上下文**

模型会将自己生成的所有内容约束在这个上下文中，使其所有回应都符合你设定的角色，而非其中立

** 👇 教程**

[用：零代码创建专属角色](https://bytedance.larkoffice.com/docx/FxT6dm3gfoXdezxTY3Mcq6CDnug)

### 进阶玩法

> 当你有了一个简单的虚拟角色，它就能和你聊天。但如果只停留在这一层，角色往往会显得单薄、机械。要让它更像一个真实的人，就需要叠加“进阶玩法”

#### 主动陪聊

真实的人不会一直等你开口，会主动找话题

通过“用户画像抽取 + 聊天上下文总结”，让角色基于你最近的对话内容或兴趣点，主动发起对话

** 👇 教程**

[最佳实践：开场白提示词](https://bytedance.larkoffice.com/docx/EjcrdM2ZPo4JZxxSvRhc62ECnjc)

[最佳实践：用户画像抽取提示词](https://bytedance.larkoffice.com/docx/PClpdnTOuoEzItxt6yHckrDGnXe)

#### 群聊互动

**群聊能带来更加丰富的社交体验**

通过多个虚拟角色，不同的虚拟角色可以在同一个群聊中各自发言，让对话氛围更像真实的朋友圈，避免单一角色的单调感

** 👇 教程**

[用：零代码创建群聊应用](https://bytedance.larkoffice.com/docx/MrcbdqLwWoST4jxvg9ccoUgxnMf)

#### 灵感回复

**在聊天中，常常会有多种回应方式**

一次生成多条回复，让对话更接近真实的人类交流，同时也能给用户提供不同的剧情分支，帮助他们主动选择想要的对话走向

**👇 教程**

关注豆包大模型工具箱，敬请期待！

#### 亲密度

养成系虚拟角色，关系会随着交流逐渐加深

将人设提示词与{亲密度}变量结合，随着互动深入，虚拟角色的语气、关心程度和分享意愿都会逐渐升级

**👇 教程**

关注豆包大模型工具箱，敬请期待！

#### 长期陪伴

真实的人会记得你们的过往、喜好和小秘密

**配套工具：**[**Viking长期记忆库**](https://console.volcengine.com/vikingdb/region:vikingdb+cn-beijing/home?projectName=default)

通过大模型抽取重要事件和用户画像，并储存于记忆库，虚拟角色能在对话中自然地提及过去的点滴，形成连续的情感纽带，让陪伴更加细腻贴心

**👇 教程**

[最佳实践：用户画像抽取提示词](https://bytedance.larkoffice.com/docx/PClpdnTOuoEzItxt6yHckrDGnXe)

[用：本地部署一个长记忆应用](https://bytedance.larkoffice.com/docx/Tn4edPMEhoXyChxBbOecfKHln0e)

#### 辅助聊天

**帮你更好地与他人聊天**

模型基于历史聊天记录生成推荐回复，帮助用户更自然地接话或扩展话题，与他人交流时更加轻松自信

**👇 教程**

关注豆包大模型工具箱，敬请期待！

### 没有灵感怎么办

别担心，我们有现成的工具：[最佳实践：虚拟角色生成提示词](https://bytedance.larkoffice.com/docx/NTPadcY3foJCqwxrqANcNKtVnhc)帮你生成一个虚拟角色的系统提示词草稿。就像“抽卡”，能快速帮你拿到一个角色原型

### 社交陪聊应用落地

点击白皮书，查看详情：[大模型解决方案白皮书——社交陪伴场景全流程落地指南 .pdf](https://bytedance.larkoffice.com/wiki/QtZKwSPSEidLXbkkufacynWunth)

### 欢迎投稿你的玩法或建议

<https://bytedance.larkoffice.com/share/base/form/shrcn9GHSbC0oMWOvaUE4yaXSux?iframeFrom=docx&ccm_open=iframe>