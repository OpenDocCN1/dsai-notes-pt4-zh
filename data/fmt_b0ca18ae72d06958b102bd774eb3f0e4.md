
# 用：零代码创建群聊应用

> 来源：[https://gotoaiworld.feishu.cn/docx/GeW1dOYFQo0IstxMhMNcd0Lynvg](https://gotoaiworld.feishu.cn/docx/GeW1dOYFQo0IstxMhMNcd0Lynvg)

## 前置准备

创建推理接入点，并获取接入点id，推荐选择Doubao-1.5-pro-32k｜character-250715模型

![](img/069850b3c8b18af0d734cbdb43b39dbd.png)

进入在线推理

![](img/7dcc64b9097db2e747096b05d3d71b31.png)

输入接入点名称

![](img/ff2a3f477ad39cea67f37ca2157cc842.png)

选择模型和版本

![](img/fc8ec5416dbae17c8e5d2b18f4a8464f.png)

创建并接入

![](img/5042b65a9249d8861976999f7d33c255.png)

获取接入点id

![](img/c9903f9012999e44eb41fedc3211b15d.png)

## 创建零代码智能体

点击链接进入：[应用广场](https://console.volcengine.com/ark/region:ark+cn-beijing/application?utm_campaign=2025H2&utm_content=gongjuxiang&utm_medium=xianshang&utm_source=LBM-zhyx&utm_term=dyjx_wendang)，点击【创建应用】，并选择“零代码”，选择“单聊”创建方式，进入创建页面

![](img/9a4148ba3e00da504a241010e4829e93.png)

## 完成零代码智能体基本信息

### 基本信息

填入基本信息，名称（必填）、描述和封面

![](img/d98eabe2f22dbb340611a1ef97405e51.png)

### 用户信息

填入用户角色名

![](img/9fba852bfad6e48dccd74e2052ccc800.png)

### 角色信息

![](img/767623b7c47568893392903edf3af9cf.png)

### 场景信息

![](img/1c2d3fbb68d9b586b47c4c46a5f16cf1.png)

## 应用发布与体验

1. 点击右上角发布应用

![](img/13cca382668bd69b8234a60f1b517f7a.png)

2. 发布后，可进行对话体验与测试

![](img/8f00f55c92e76e4e2c8dd650ae6e0d98.png)

3. 可通过「我的应用」列表中查看、体验、编辑、管理已创建的应用

![](img/112f5ccf02ecc7a9ee3619a0d965665f.png)

## 调试模式

对于“我的应用”，用户可通过调试信息面板查看会话或插件的执行细节，从而帮助用户定位应用响应不符合预期的原因，并对应用配置进行优化与调整。具体操作步骤如下：

+ 选择“我的应用”，点击“编辑”，并进入预览与调试界面，发送消息内容开始与应用对话

+ 接收到应用响应后，点击消息卡片下方的调试按钮，开启调试面板查看执行细节

![](img/d8f557a11d805a77eefa0232a67e9af1.png)

+ 单击【参数设置】，可设置`max_tokens`、`frequency_penalty`、`temperature`、`top_p`等参数。

+ 修改后点击「更新」对应用进行更新

![](img/4873c3565879664febd442347f923c4d.png)

## BotAPI调用（进阶）

`BotAPI` V3 详见 [API文档](https://www.volcengine.com/docs/82379/1285207) 与 [SDK 调用说明](https://www.volcengine.com/docs/82379/1302007)，用户可点击已创建完成应用右上角【API调用指南】，进行`BotAPI`的调用

![](img/da3708164a3b2aae2d9e8c6ca5fbe7f4.png)
