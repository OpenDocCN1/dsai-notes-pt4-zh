
# 如何调用DeepSeek R1模型的API

> 来源：[https://feicaiclub.feishu.cn/docx/XnemdcHC7of8LwxSjJJcWD9Vn1g](https://feicaiclub.feishu.cn/docx/XnemdcHC7of8LwxSjJJcWD9Vn1g)

![](img/19a78aff6681706ec057f182a2ee3ffc.png)

> **视频教程**
>
> **B站：**`https://b23.tv/f9ElKyl`
>
> **📕：**`http://xhslink.com/a/DkRuZD9GxqB4`
>
> **抖音：**`https://v.douyin.com/ifH9WTYc/`
>
> **youtube：**`https://www.youtube.com/watch?v=YrtkMisUhco&t=122s`

DeepSeek R1模型的开源发布让AI行业掀起了一波新热潮。如果你想灵活地使用这个模型，调节其参数来满足特定需求，调用API是最佳方式。本教程将详细介绍如何通过调用DeepSeek R1的API，设置和调整参数

## 一、准备工作：注册并创建API密钥

访问DeepSeek官网 `www.deepseek.com`

![](img/c039e73058d5f1421300cccf8e295306.png)

首先，打开DeepSeek的官网。在官网上，你可以直接使用DeepSeek R1模型，但如果你想更自由地调整模型的参数，则需要调用API。

`创建API密钥`

在官网右上角找到“API开放平台”并点击进入。

进入API平台后，你将看到关于账户余额和使用情况的数据。

如果是新用户，DeepSeek会赠送10元的免费额度，这足以让你体验模型的强大功能。

![](img/231781dc77e2f6f34433c632b8fa3112.png)

在左侧导航栏找到“API Keys”，点击进入后，选择“Create New API Key”创建一个新的API密钥。

![](img/bfb62c984ab79340c1b599f84bd3d4ce.png)

给API密钥取个名字，点击“Create API”即可生成密钥。

![](img/9b3c7f72010a6936efe4398843eae39d.png)

记得复制并保存好这个密钥，你将需要它来进行后续操作。

![](img/5f2f9a7ccc054fed3a60ee7cb9a2c967.png)

## 二、配置API并选择模型

进入ChatBox官网 `https://chatboxai.app/en`

接下来，我们需要使用ChatBox平台来调用DeepSeek R1模型。在ChatBox官网，你有两种选择：可以下载客户端，也可以直接使用网页版。在这里，我们选择网页版进行演示。

![](img/898eb00b12b3df26965c653e6863ad99.png)

首次使用时，ChatBox会引导你进行基础配置。当然点击左侧导航栏中的“设置”也可以进入到设置页面。

![](img/0e6e13b61af0acda71dec11f95676131.png)

1. 配置模型
1. 在“模型提供方”下拉菜单中选择DeepSeek。
1. 粘贴刚才复制的API密钥到输入框中。
1. 在“模型选择”项中选择“推理”模型，即DeepSeek的R1模型。
1. 调整参数设置
1. `上下文消息数量上限`：将其设置为“无限制”，让模型在生成内容时参考对话框内的所有消息。
1. `温度值`：温度值决定生成内容的创意程度。你可以根据需求调整：
   + 温度值小（0-0.5）：生成内容更严谨、规则。
   + 温度值高（1-2）：生成内容更具创意，适合写作、创作类任务。

![](img/eca8cb7c4df75eb4c35e79d95e8a5781.png)

1. `TopP值`：TopP控制生成文本的多样性和质量。TopP值越小，内容越保守；值越大，内容则更具随机性和创意。

插图建议：展示温度值、`TopP值`等设置的界面，标注如何调整这些参数。

1. 保存设置

完成配置后，点击“保存”按钮，设置生效。如果以后需要切换模型或修改设置，可以随时在页面右下角进行调整。

## 三、总结

DeepSeek R1模型凭借其高性价比和强大的功能，成为了当前AI领域的一颗新星。通过API的灵活调用，你可以轻松调整模型参数，生成更加个性化、创意十足的内容。如果你还没有体验过DeepSeek R1，赶快行动起来，探索这个强大的AI助手吧！
