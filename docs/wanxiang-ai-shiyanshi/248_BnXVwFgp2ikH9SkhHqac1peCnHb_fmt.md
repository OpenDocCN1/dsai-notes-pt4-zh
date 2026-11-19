
# OpenClaw 部署故障排查汇总

> 来源：[https://yunyinghui.feishu.cn/docx/EUApdZJEXojFouxdctScvoeknpb](https://yunyinghui.feishu.cn/docx/EUApdZJEXojFouxdctScvoeknpb)

> [OpenClaw 学习手册（全网首发&持续更新 ing...](https://yunyinghui.feishu.cn/wiki/Ru7PwG82qiDGo1kGMFZcxqpGnnf?from=from_copylink)总各类 OpenClaw 故障问题，教程请查看：

> 可复制可下载，不要删除修改，谢谢配合

## ❌ OpenClaw 无法操作文件

```
openclaw config set tools.profile full
```

完成以后输入 `openclaw gateway restart` 重启网关

注意 Mac 系统有个弹窗提示 `node` 要求访问文件一定要选允许

## 🐛 npm error code 128

表现如图，如果有🪜，从上网工具复制环境变量粘贴到终端回车执行，再执行 OpenClaw 安装命令

![](img/8f6a6f422ee58ef5d81eb79f1fa19792.png)

如果没有🪜可以尝试下面的步骤，每一行命令复制出来执行，不要全部复制一次执行：

```
git config --global url."https://".insteadOf ssh://git@
git config --global url."https://".insteadOf ssh://git

npm cache clean --force

git --version

npm config get proxy

npm config delete proxy

npm config delete https-proxy
```

上面这些命令执行完以后，再次执行安装 OpenClaw 命令

## 🤖 Minimax 模型配置完不好使

`OpenClaw` 默认是接入国外版的 `BaseURL`，需要手动配置成国内的 `BaseURL`，在终端输入以下命令

```
openclaw config set models.providers.minimax.baseUrl "https://api.minimaxi.com/anthropic"
```

## 🐛 npm error code CERT_HAS_EXPIRED

可以暂时把 `SSL` 证书验证依赖关掉，在终端中执行下面的代码

```
npm config set strict-ssl false
```

## 🛜 WebUI 连接失败 1006

重启网关，终端输入：

```
openclaw gateway restart
```

## ❌ 错误代码 401

**原因**：身份验证失败（`Unauthorized`）。

**排查**：通常是 `API Key` 填写错误。请检查 `Key` 是否复制完整（有无首尾空格、漏字符），或者确认当前填写的 `Key` 是否对应所选择的 `API` 渠道/服务商（例如不要把 `GPT-4` 的 `Key` 用在 `Claude` 的接口上）。

## ❌ 错误代码 429 / 500

**原因**：余额不足、配额用尽或服务端异常。

**排查**：

+ `429`：通常表示免费额度用完或账户余额不足，请登录后台充值或检查额度。

+ `500`：虽然标准含义是服务器内部错误，但在中转 `API` 场景下，往往也意味着上游 `Key` 欠费、被封禁或失效。建议充值或更换一个新的 `Key` 再试。

## ❌ 错误代码 rate_limit

**原因**：请求速率超出限制（`Rate Limit Exceeded`）。

**排查**：发送请求的频率超过了服务商规定的阈值（如每分钟请求次数 `RPM` 或每分钟 `Token` 数 `TPM`）。建议降低并发请求速度，联系服务商提高并发额度，或者更换支持更高并发的服务商。

## ❌ 错误代码 404

**原因**：请求地址错误（`Not Found`）。

**排查**：通常是 `Base URL`（接口代理地址）填写不正确。请务必参考服务商文档，检查链接末尾是否需要加 `/v1`，或者是否存在拼写错误。地址不对，服务器就找不到对应的模型入口。

## ❌ 错误代码 403

**原因**：访问被拒绝（`Forbidden`）。

**排查**：通常涉及权限或地域限制。

地域问题：你的服务器 `IP` 所在的国家/地区不在 `API` 服务商的支持范围内（例如 `OpenAI` 对某些地区封锁），需要更换服务器节点。

模型权限：当前使用的 `Key` 没有权限调用该特定模型，建议更换模型测试。

## 💬 WebUI 发送消息返回内容为空

一般也是模型配置出问题了，可以终端输入：`openclaw tui` 对话，有更多信息可以展示。

## 🪟 Windows 连接不上打不开 WebUI 页面

`Windows` 的限制，不像 `mac` 和 `linux` 有这么方便的后台服务管理，解决办法：

1. 不要关闭自动打开的弹窗

![](img/39e15c9f9cca206fa3674ef16289ebe1.png)

⚠️`Windows` 部署成功以后不要关闭终端窗口，如果关闭运行 `c`

如果出现下面这个问题，就是因为你关闭了终端窗口！！！！！

![](img/fdf889e6013df9c57c4b42059a31e467.png)
