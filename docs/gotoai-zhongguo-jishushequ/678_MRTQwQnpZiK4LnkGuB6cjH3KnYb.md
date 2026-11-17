# 用：零代码创建专属角色

> 来源：[https://gotoaiworld.feishu.cn/docx/FzY8dPwFTo6odIxJM6ic0Wm2nre](https://gotoaiworld.feishu.cn/docx/FzY8dPwFTo6odIxJM6ic0Wm2nre)

### 创建零代码智能体

点击链接进入：[应用广场](https://console.volcengine.com/ark/region:ark+cn-beijing/application?utm_campaign=2025H2&utm_content=gongjuxiang&utm_medium=xianshang&utm_source=LBM-zhyx&utm_term=dyjx_wendang)，点击【创建应用】，并选择“零代码”，选择“单聊”创建方式，进入创建页面

![](img/3f06abf8eb141b4350539f28725863eb.png)

[录屏2025-08-20 16.16.47.mov](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/IioLbO0fko0iNCxgsQDcxg89nAh)

### 完成零代码智能体基本信息

#### 基本信息

填入基本信息，名称（必填）、描述和封面

![](img/8b5427a9f4893a882f559a47b99186f9.png)

#### 配置信息

##### 推理接入点

> 开通赠送500,000 tokens免费额度，超[过部分按 tok](https://www.volcengine.com/docs/82379/1544681)en 使用量付费，详见 模型服务计费说明

+   输入“character”，建议选择「豆包角色扮演模型」——专为AI陪伴与虚拟角色互动打造，具备高度拟人化的表达能力与强上下文感知，能够灵活适配各类社交虚拟角色扮演场景

+   推荐Pro模型，效果更好。

![](img/5f183865f8cd66a36a679edf0b3e7fb9.png)

##### 插件选择

创建角色扮演零代码智能体默认可不开启所有插件。

##### 提示词 System Prompt

根据自己的设定输入人物提示词，可以点击「提示词优化」，将自动将提示词调整为规范格式，也可以根据自身需要手动对优化后提示词进行微调。

优化前：

![](img/dfee05f2cdaf0efb141d28d7c23b0967.png)

优化后：

![](img/02788b0b790afade4dac827aaa6c9869.png)

#### 开场白（可选）

开场白部分内容可以自定义，发布后内容展示在详情

![](img/86518876db3250d2a8efc15dc8110061.png)

### 应用发布与体验

1.  点击右上角发布按钮即可体验对话

![](img/2942618697ad717d92039c0f9e587617.png)

1.  发布后，可进行对话体验与测试

![](img/8597ee25a8fb753c02376e18cbffc16c.png)

1.  用户也可通过「我的应用」列表中查看、体验、管理已创建的应用

![](img/8595864ab8382fb15d673d1860094e63.png)

### 调试模式

对于“我的应用”，用户可通过调试信息面板查看会话或插件的执行细节，从而帮助用户定位应用响应不符合预期的原因，并对应用配置进行优化与调整。具体操作步骤如下：

+   选择“我的应用”，点击“编辑”，并进入预览与调试界面，发送消息内容开始与应用对话

+   接收到应用响应后，点击消息卡片下方的调试按钮，开启调试面板查看执行细节


+   单击【参数设置】，可设置 max_tokens、frequency_penalty、temperature 、top_p等参数。

+   修改后点击「更新」对应用进行更新。

![](img/40ce3a083c3594a55c79dfa86379ce33.png)

### BotAPI调用（进阶）

BotAPI V3 详见 [API文档](https://www.volcengine.com/docs/82379/1285207) 与 [SDK 调用说明](https://www.volcengine.com/docs/82379/1302007)，用户可点击已创建完成应用右上角【API调用指南】，进行BotAPI的调用

![](img/345c892d5e8b688d33c86c1a681aa879.png)

![](img/f84fbcca48b7ab50ed019abd65d0655f.png)