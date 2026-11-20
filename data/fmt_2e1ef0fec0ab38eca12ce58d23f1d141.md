
# 快速开始

那如何使用，第一步，搜索 `kimi code`：https://www.kimi.com/code

![](img/5dde58d7cb0f65588d54ebb06525ce4c.png)

第二步，购买优惠套餐 `plan`，说实话，目前 `OpenClaw` 消耗 token 还挺大的，最好买个套餐划算一些，我买的是 Allegretto 套餐。

第二步，打开控制台，创建 `API key`。名字随便取。

这个 `API key` 一定！一定！要复制下来，因为一般你点完“完成”之后，你就再也无法查看你的 `API key` 了，如果你忘记了你的 `API key`，那就只能重新创建一个了。复制完后，找地方先存起来，后续在“第四步：`OpenClaw` 配置”的时候会用到

![](img/1ba4e50ce81c2977843494a2309d5987.png)

## 配置步骤

1. **注册账号**

   访问：https://platform.moonshot.cn/
   注册并登录

1. **获取 API Key**

   进入“API 管理”
   点击“创建 API Key”
   复制 API Key

1. **配置到 OpenClaw**

   ```json
   {
     "models": {
       "mode": "merge",
       "providers": {
         "moonshot": {
           "baseUrl": "https://api.moonshot.cn/v1",
           "apiKey": "sk-你的 API 密钥",
           "auth": "api-key",
           "api": "openai-chat",
           "models": [
             {
               "id": "moonshot-v1-8k",
               "name": "Kimi k2.5",
               "contextWindow": 8000,
               "maxTokens": 4096
             }
           ]
         }
       }
     },
     "agents": {
       "defaults": {
         "model": {
           "primary": "moonshot/moonshot-v1-8k"
         }
       }
     }
   }
   ```

## 成本估算

+ 日常使用：10-20 元/月
+ 中度使用：20-50 元/月
+ 重度使用：50-100 元/月

## 其他国产大模型

## 国际模型配置（可选）

如果需要使用 Claude、GPT 等国际模型：

1. **直接使用官方 API**（需要魔法）
1. **使用第三方 API 服务**（国内直连）

### 推荐第三方 API

+ 价格便宜 50%-70%
+ 国内直连，无需魔法
+ 支持支付宝、🛰️ 支付

## 成本对比

💡 省钱技巧：

+ 日常对话用 DeepSeek（最便宜）
+ 长文档用 Kimi（长上下文）
+ 复杂任务用 Claude（质量最高）

## 常见问题解决

### 安装问题

**Q1: Node.js 版本不对**

```bash
# 检查版本
node --version
# 如果低于 22，升级
nvm install 22
nvm use 22
```

**Q2: 权限错误**

```bash
# macOS/Linux
sudo chown -R $USER ~/.openclaw

# Windows
# 以管理员身份运行 PowerShell
```

**Q3: 网络连接失败**

+ 检查网络连接
+ 尝试使用代理
+ 或使用云端部署

### API 配置问题

**Q1: API Key 无效**

+ 检查是否完整复制（包括 `sk-` 前缀）
+ 检查是否有多余空格
+ 检查账户余额是否充足

**Q2: 模型不可用**

+ 检查模型 ID 是否正确
+ 检查 API 服务是否正常
+ 尝试切换其他模型

**Q3: Token 消耗太快**

+ 使用更便宜的模型（DeepSeek）
+ 优化提示词
+ 定期清理会话历史

### Gateway 问题

**Q1: Gateway 无法启动**

```bash
# 查看日志
tail -f ~/.openclaw/logs/gateway.log

# 重启 Gateway
openclaw gateway restart
```

**Q2: 端口被占用**

```bash
# 查看端口占用
lsof -i :18789

# 修改端口
openclaw config set gateway.port 18790
```

## 2.X 版本升级指南

> 🔄 保持支持最新：定期升级 `OpenClaw` 以获得新功能、性能优化和安全修复。

> ⚠️ 重要提示：目前推荐使用 `2026.3.2` 版本。`2026.2.1` 版本存在已知 bug（Issue #15141），会导致 `heartbeat` 和消息处理功能异常。

### 推荐版本

**当前推荐版本**：`2026.3.2`

**已知问题版本**：

+ ❌ `2026.2.12`：`Session` 路径验证 bug，影响 `heartbeat` 和 Telegram/飞书消息处理

### 为什么要升级？

`OpenClaw` 支持续迭代更新，升级可以获得：

+ ✅ 新功能和改进
+ ✅ 性能优化
+ ✅ 安全修复
+ ✅ Bug 修复
+ ✅ 更好的兼容性

### 检查当前版本

```bash
# 查看当前版本
openclaw --version
# 查看配置文件版本
cat ~/.openclaw/openclaw.json | grep version
```

如果配置文件版本比当前版本新，说明需要升级。

### 升级方式选择

根据你的安装方式选择对应的升级方法：

### 方式一：npm 直接升级（推荐）

这是最可靠的升级方式，适用于通过 `npm` 安装的用户。

#### 升级步骤

**第一步：备份配置**

```bash
# 备份整个配置目录
cp -r ~/.openclaw ~/.openclaw.backup-$(date +%Y%m%d)
# 验证备份
ls -la ~/.openclaw.backup-*
```

**第二步：停止 `Gateway` 服务**

```bash
# 停止 Gateway
openclaw gateway stop

# 验证已停止
openclaw gateway status
```

**第三步：卸载旧版本**

```bash
# 卸载旧版本
npm uninstall -g openclaw

# 验证卸载
which openclaw  # 应该没有输出
```

**第四步：安装新版本**

```bash
# 安装推荐版本 2026.3.2（需要使用 --force 参数）
npm install -g openclaw@2026.3.2 --force
```

> ⚠️ 版本选择说明：

+ 推荐安装 `2026.3.2` 版本（稳定）
+ 避免安装 `2026.2.12` 版本（存在 `Session` 路径 bug）
+ 如果需要最新功能，等待 `2026.2.13+` 版本修复后再升级

> ⚠️ 为什么需要 `--force`？

> `npm` 会检测到已存在的文件，使用 `--force` 强制覆盖。

**第五步：修复配置**

```bash
# 运行 doctor 工具自动修复配置
openclaw doctor --fix
```

`doctor` 工具会自动：

+ 更新 `Gateway` 服务入口点路径
+ 检查配置兼容性
+ 修复已知问题
+ 重新安装 `LaunchAgent`（macOS）或 `systemd` 服务（Linux）

**第六步：重启 `Gateway`**

```bash
# 重启 Gateway
openclaw gateway restart

# 等待几秒后检查状态
sleep 5
openclaw gateway status
```

**第七步：验证升级**

```bash
# 检查版本
openclaw --version
# 检查 Gateway 状态
openclaw gateway status

# 测试连接
openclaw channels status
```

成功的输出应该显示：

```
2026.3.2

Runtime: running (pid xxxxx, state active)
RPC probe: ok
Listening: *:18789
Dashboard: http://127.0.0.1:18789/
```

#### 升级示例

以下是一次真实的升级过程：

![](img/95288f1b027abc590b2bf01e64a1360e.png)

图：升级前版本检查，配置文件版本 (`2026.2.6-3`) 比当前版本 (`2026.2.1-zh.3`) 新

![](img/d2b73ddc0aa85ce222e23f15cf936eb7.png)

图：使用 `npm install --force` 安装新版本

![](img/b656c5186a3a48e829fe29cca231d0a9.png)

图：升级完成，版本更新到 `2026.3.2`

### 方式二：官方脚本升级

使用官方提供的一键升级脚本。

```bash
# 运行升级脚本
curl -fsSL https://openclaw.ai/install.sh | bash
```

**优点**：

+ ✅ 一键完成
+ ✅ 自动处理依赖

**缺点**：

+ ❌ 可能遇到 `npm` 错误
+ ❌ 不如方式一可靠

**如果遇到错误**：

+ 切换到方式一（npm 直接升级）
+ 或查看下文的故障排查

### 方式三：Docker 升级

如果使用 Docker 部署，升级非常简单。

```bash
# 停止并删除旧容器
docker compose down

# 拉取最新镜像
docker compose pull

# 启动新容器
docker compose up -d openclaw-cn-gateway

# 查看日志
docker compose logs -f openclaw-cn-gateway
```

### 方式四：云端部署升级

#### 腾讯云 Lighthouse

1. 进入轻量应用服务器控制台
1. 选择你的 `OpenClaw` 实例
1. 点击“重置应用”
1. 选择最新的 `OpenClaw` 镜像
1. 等待重置完成
1. 重新配置 API（配置会保留）

### 升级后的变化

#### 配置文件自动迁移

升级后，`doctor` 工具会自动：

+ 更新配置文件格式
+ 迁移旧版本配置
+ 创建备份文件：`~/.openclaw/openclaw.json.bak`

#### 服务管理改进

**macOS**：

+ `LaunchAgent` 自动重新安装
+ 服务路径更新为新版本
+ 日志位置：`~/.openclaw/logs/gateway.log`

**Linux**：

+ `systemd` 服务自动更新
+ 服务路径更新为新版本
+ 日志位置：`~/.openclaw/logs/gateway.log`

#### 已知警告

升级后可能看到一些警告，这些通常不影响使用：

```
Config warnings:
- plugins.entries.feishu: plugin feishu: duplicate plugin id detected
```

说明：这是一个已知的插件重复警告，不影响正常使用。

### 升级故障排查

#### 问题 1：npm install 报错 EEXIST

**症状**：

```
npm error code EEXIST
npm error path /usr/local/bin/openclaw
npm error EEXIST: file already exists
```

**解决方案**：

```bash
# 使用 --force 参数强制覆盖
npm install -g openclaw@2026.3.2 --force
```

#### 问题 2：Gateway 启动失败

**症状**：

```
Gateway not running
```

**解决方案**：

```bash
# 运行 doctor 修复
openclaw doctor --fix
# 重启 Gateway
openclaw gateway restart

# 查看详细日志
tail -f ~/.openclaw/logs/gateway.log
```

#### 问题 3：配置文件版本不匹配

**症状**：

```
Config was last written by a newer OpenClaw (2026.2.6-3); 
current version is 2026.2.1-zh.3
```

**解决方案**：

```bash
# 升级到推荐版本 2026.3.2
npm install -g openclaw@2026.3.2 --force
openclaw doctor --fix
```

#### 问题 4：插件加载失败

**症状**：

```
plugin not found: xxx
```

**解决方案**：

```bash
# 重新安装插件
openclaw plugins install <plugin-name>

# 或禁用有问题的插件
openclaw config set plugins.allow []
```

#### 问题 5：端口被占用

**症状**：

```
Error: listen EADDRINUSE: address already in use :::18789
```

**解决方案**：

```bash
# 查找占用端口的进程
lsof -i :18789

# 停止旧的 Gateway 进程
kill -9 <PID>

# 或修改端口
openclaw config set gateway.port 18790
openclaw gateway restart
```

### 回滚到旧版本

如果升级后遇到问题，可以回滚到旧版本。

#### 方式一：从备份恢复

```bash
# 停止 Gateway
openclaw gateway stop

# 恢复配置
cp -r ~/.openclaw.backup-20260213/* ~/.openclaw/

# 降级到旧版本（以中文版为例）
npm uninstall -g openclaw
npm install -g @qingchencloud/openclaw-zh@2026.2.1-zh.3 --force

# 重启 Gateway
openclaw gateway restart
```

#### 方式二：安装指定版本

```bash
# 查看可用版本
npm view openclaw versions

# 安装指定版本
npm install -g openclaw@2026.2.8 --force

# 修复配置
openclaw doctor --fix

# 重启 Gateway
openclaw gateway restart
```

### 升级最佳实践

#### 升级前必做

1. **备份配置**：

   ```bash
   cp -r ~/.openclaw ~/.openclaw.backup-$(date +%Y%m%d)
   ```

1. **记录当前版本**：

   ```bash
   openclaw --version > ~/openclaw-version-before-upgrade.txt
   ```

1. **导出重要配置**：

   ```bash
   openclaw config get > ~/openclaw-config-backup.json
   ```

1. **检查磁盘空间**：

   ```bash
   df -h ~
   ```

#### 升级时注意

1. **使用 `--force` 参数**：

   ```bash
   npm install -g openclaw@2026.3.2 --force
   ```

1. **运行 `doctor` 修复**：

   ```bash
   openclaw doctor --fix
   ```

1. **检查 `Gateway` 状态**：

   ```bash
   openclaw gateway status
   ```

1. **查看日志**：

   ```bash
   tail -f ~/.openclaw/logs/gateway.log
   ```

#### 升级后验证

1. **检查版本号**：

   ```bash
   openclaw --version
   ```

1. **测试 `Gateway` 连接**：

   ```bash
   openclaw channels status
   ```

1. **验证插件功能**：

   ```bash
   openclaw plugins list
   ```

1. **测试频道连接**：

   + 发送测试消息
   + 验证 AI 回复
   + 检查 `Skills` 功能

1. **检查 API 消耗**：

   ```bash
   openclaw stats api
   ```

### 已知问题版本警告

#### 2026.2.12 版本问题
