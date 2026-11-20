
# 办公软件集成攻略

> 来源：[https://geekbang.feishu.cn/docx/KAeVd9WX3ojfN9xDqC2ckzCEn6e](https://geekbang.feishu.cn/docx/KAeVd9WX3ojfN9xDqC2ckzCEn6e)

喂饭级飞书（Feishu）集成攻略，打造数字员工

## 一、前提准备

1、OpenClaw 已安装并升级到最新版

```
openclaw upgrade          # 或 openclaw update
openclaw version          # 确认 ≥ 2026.2.2
```

2、安装官方飞书插件（必须执行，即使你觉得已内置）

```
openclaw plugins install @openclaw/feishu
openclaw plugins list     # 确认 `feishu` 已 loaded
```

3、准备飞书账号（没有的自行注册一个）

## 二、在飞书开放平台创建应用

1、登录飞书开放平台->开发者后台（`https://open.feishu.cn/app`）

2、创建企业自建应用

◆ 应用名称：随便填（如 “我的 OpenClaw 助手”）

◆ 应用描述：随意

◆ 图标：可选，上传一个清晰的图标，点击 创建

3、获取凭证（**重要！**）

左侧菜单 → `凭证与基础信息`

复制：

◆ `App ID`（格式 `cli_xxxxxxxx`）

◆ `App Secret`（一串长字符串，务必复制后立即保存到密码管理器，切勿泄露）

4、添加机器人能力

左侧 → 添加应用能力 → 选择 机器人 → 添加

设置机器人名称（如 “OpenClaw 助手”）并保存

5、批量导入权限（**关键！**）

左侧 → 权限管理 → 点击 批量导入导出权限 → 导入

粘贴以下官方推荐 JSON（直接复制）：

```
{  "scopes": {    "tenant": [      "aily:file:read",      "aily:file:write",      "application:application.app_message_stats.overview:readonly",      "application:application:self_manage",      "application:bot.menu:write",      "contact:user.employee_id:readonly",      "corehr:file:download",      "event:ip_list",      "im:chat.access_event.bot_p2p_chat:read",      "im:chat.members:bot_access",      "im:message",      "im:message.group_at_msg:readonly",      "im:message.p2p_msg:readonly",      "im:message:readonly",      "im:message:send_as_bot",      "im:resource"    ],    "user": [      "aily:file:read",      "aily:file:write",      "im:chat.access_event.bot_p2p_chat:read"    ]  }}
```

点击导入 → 全选 → 粘贴

点击下一步 → 申请开通 → 保存成功

6、发布应用

左侧 → 版本管理与发布 → 创建版本

填写版本号 → 填写更新说明 → 点击发布（首次创建免审核，保存提交后即可上线使用）。

![](img/2990000ddbaf59c495532e3fae544f5f.png)

![](img/1a4330d16aac54b553abbe34b7ce23ac.png)

## 三、在 OpenClaw 配置飞书通道

推荐使用交互式向导（最简单）：

```
openclaw channels add
```

◆ 选择 `Feishu / Lark`

◆ 输入 `App ID`

◆ 输入 `App Secret`

◆ 是否启用群聊：推荐 是

◆ 是否需要 @ 提及：推荐 是（防止刷屏）

◆ 完成（Finish）

## 四、启动 Gateway 并配置飞书事件订阅（关键步骤）

1、启动 OpenClaw Gateway（必须先启动！）

```
openclaw gateway --port 18789 --verbose   # 前台运行方便看日志
# 或后台守护进程（推荐）
openclaw gateway install --daemon
openclaw gateway start
```

浏览器打开 `http://你的服务器IP:18789` 查看控制面板。

2、回到飞书开放平台配置事件订阅

◆ 应用详情页左侧 → 事件与回调

![](img/4849d46a23dd4bbca55c04364efeff5a.png)

◆ 添加事件：搜索并勾选 `im.message.receive_v1`

◆ 保存（此时 Gateway 必须在运行，否则保存失败）

3、重启 Gateway（让配置生效）

```
openclaw gateway restart
openclaw logs --follow     # 实时看日志
```

## 五、测试与配对使用

1、在飞书搜索你的机器人，发起单聊或拉进群。

机器人会回复一段配对码（如 `PAIR-ABC123`）

2、在终端审批配对（仅首次 DM 需要）

```
openclaw pairing list feishu          # 查看待审批
openclaw pairing approve feishu PAIR-ABC123
```

3、测试对话

直接在你的飞书发送消息：比如“你好，每天早上 8 点发晨报” 看到回复即为成功！

至此，飞书（Feishu）与 OpenClaw 的集成就完成了。

提供一份完整的官方文档：`https://docs.openclaw.ai/channels/feishu`

以及插件仓库：`https://github.com/openclaw/feishu`（可查看最新更新）

现在，你的「数字员工」就已经成功孵化并随时待命了！！！

> 来源：https://mp.weixin.qq.com/s/UhQSCacrzkyxrFp2uGgzjQ
