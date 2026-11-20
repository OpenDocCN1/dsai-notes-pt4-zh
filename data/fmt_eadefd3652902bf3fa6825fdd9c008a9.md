
1.  创意写作： `Top P = 0.95`

```python
response = client.messages.create(
    model="claude-sonnet-4-6",
    max_tokens=2048,
    top_p=0.95,  # 允许更多创意选择
    messages=[{
        "role": "user",
        "content": "写一首关于星空的诗"
    }]
)
```

**推荐做法**： 选择其中一个参数调整，另一个保持默认值（1.0）。

**参数调优最佳实践**

1.  **从默认值开始**： `Temperature = 1.0`, `Top P = 1.0`, `Max Tokens = 合理值`
2.  **单一变量调整**： 每次只改变一个参数
3.  **小步迭代**： 每次调整幅度不要太大（如 `Temperature` 每次调整 0.1-0.2）
4.  **多次测试**： 对同一配置运行多次，观察输出的一致性和质量
5.  **记录结果**： 保存不同配置的效果，建立最佳实践库

**常见参数配置模板**

```python
PARAMETER_PRESETS = {
    "factual_qa": {
        "temperature": 0.0,
        "top_p": 0.1,
        "max_tokens": 200,
        "description": "事实性问答，追求准确性"
    },
    "code_generation": {
        "temperature": 0.0,
        "top_p": 1.0,
        "max_tokens": 2048,
        "description": "代码生成，确保正确性"
    },
    "customer_service": {
        "temperature": 0.3,
        "top_p": 1.0,
        "max_tokens": 512,
        "description": "客服对话，友好且一致"
    },
    "content_creation": {
        "temperature": 0.7,
        "top_p": 1.0,
        "max_tokens": 2048,
        "description": "内容创作，平衡创意和连贯性"
    },
    "creative_writing": {
        "temperature": 0.9,
        "top_p": 0.95,
        "max_tokens": 4096,
        "description": "创意写作，最大化多样性"
    },
    "data_extraction": {
        "temperature": 0.0,
        "top_p": 0.1,
        "max_tokens": 1024,
        "description": "数据提取，精确输出"
    },
}

def get_preset(task_type):
    """获取预设配置"""
    return PARAMETER_PRESETS.get(task_type, PARAMETER_PRESETS["customer_service"])

# 使用示例
config = get_preset("code_generation")
response = client.messages.create(
    model="claude-sonnet-4-6",
    max_tokens=config["max_tokens"],
    temperature=config["temperature"],
    top_p=config["top_p"],
    messages=[{"role": "user", "content": "写一个快速排序算法"}]
)
```

**总结**： 参数调优是一门艺术，需要根据具体任务和需求进行实验和调整。`Temperature` 控制随机性，`Max Tokens` 控制长度和成本，`Top P` 提供更精细的采样控制。掌握这三个参数的使用，可以让 Claude 在各种场景下都发挥最佳性能。

---

通过本章的学习，你应该已经掌握了 Claude 4.6 系列模型的特点、性能对比、场景化选择策略、多模型混合使用技巧，以及参数调优方法。在实际应用中，建议从 Claude Sonnet 4.6 开始，根据具体需求逐步优化模型选择和参数配置，在性能、成本和速度之间找到最佳平衡点。

**关键要点总结**：

1.  Sonnet 4.6 是大多数场景的最佳选择：在计算机使用、编码、办公生产力和代理任务上接近或超越 Opus，但成本仅为其 20%
2.  Opus 4.6 保留用于极限任务：仅在需要最高级别抽象推理时使用
3.  Haiku 4.5 适合高并发场景：成本最低，速度最快，适合明确定义的任务
4.  **积极使用成本优化策略**：`Prompt Caching` 可节省 90% 输入成本，`Batch API` 可节省 50% 总成本
5.  **多模型混合使用**：根据任务复杂度动态选择模型，可节省 40-60% 成本
6.  **参数调优**：`Temperature` 控制创意性，`Max Tokens` 控制成本，合理配置可显著提升效果

---

# 第三部分：实战篇

## 第 6 章 踩坑经验大全

> 本章汇总了 OpenClaw 使用过程中最常见的问题和解决方案，涵盖安装部署、API 调用、Prompt 工程、性能优化、数据安全和成本控制六大领域。所有案例均来自真实用户反馈和社区最佳实践。

---

### 6.1 安装部署常见问题

#### 6.1.1 依赖冲突解决

**问题 1: Node.js 版本过低**

**症状：**

```text
$ npm install -g openclaw
npm ERR! code EBADENGINE
npm ERR! engine Unsupported engine
npm ERR! engine Not compatible with your version of node/npm
```

**根本原因：**

OpenClaw 要求 **Node.js 22**+，但很多用户系统中安装的是 Node 16.x 或 18.x。这些旧版本不支持 OpenClaw 使用的现代 JavaScript 语法（如 `Top-level await`、某些 ES2023 特性）。

**诊断方法：**

```bash
# 检查当前 Node.js 版本
node --version

# 如果显示 v16.x 或 v18.x,就是问题所在
```

**解决方案：**

**方案 A: 使用 nvm 管理多版本（推荐）**

```bash
# 安装 nvm (如果还没有)
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash

# 安装 Node.js 22
nvm install 22

# 设置为默认版本
nvm alias default 22

# 验证
node --version  # 应该显示 v22.x.x
```

**方案 B: 直接升级（适合单一项目）**

```bash
# Ubuntu/Debian
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt-get install -y nodejs

# macOS (使用 Homebrew)
brew update
brew upgrade node

# Windows (使用 Chocolatey)
choco upgrade nodejs
```

**验证安装：**

```bash
node --version  # v22.x.x
npm --version   # 10.x.x
openclaw --version  # 应该能正常显示版本号
```

[citation](https://lumadock.com/tutorials/openclaw-troubleshooting-common-errors) [citation](https://eastondev.com/blog/en/posts/ai/20260205-openclaw-troubleshooting-guide/)

---

**问题 2: npm/pnpm 依赖安装失败**

**症状：**

```text
$ npm install -g openclaw
npm ERR! network timeout
npm ERR! network This is a problem related to network connectivity
```

**根本原因：**

+   国内网络访问 npm 官方源速度慢或超时
+   某些依赖包（如 Go 模块、Python 包）需要额外的系统依赖
+   防火墙或代理配置问题

**解决方案：**

**步骤 1: 切换到国内镜像源**

```bash
# 使用淘宝镜像(推荐)
npm config set registry https://registry.npmmirror.com

# 或使用腾讯云镜像
npm config set registry https://mirrors.cloud.tencent.com/npm/

# 验证配置
npm config get registry
```

**步骤 2: 安装系统依赖**

```bash
# Ubuntu/Debian
sudo apt update
sudo apt install -y build-essential python3 python3-dev golang-go

# macOS
xcode-select --install
brew install go python3

# Windows (使用 Chocolatey)
choco install python golang
```

**步骤 3: 重试安装（带详细日志）**

```bash
# 清除缓存
npm cache clean --force

# 重新安装,显示详细日志
npm install -g openclaw --verbose

# 如果还是失败,尝试使用 yarn
npm install -g yarn
yarn global add openclaw
```

**技巧： 处理 Skill 依赖安装慢**

某些 Skills（如 Go 相关的）首次安装需要 5-10 分钟编译：

```bash
# 安装 Skill 时不要中断
openclaw skill install <skill-name>

# 查看安装进度(另开终端)
docker compose logs openclaw-gateway -f | grep -i "skill"

# 看到持续的日志输出 = 正常安装中,不要 Ctrl+C!
</skill-name>
```

[citation](https://eastondev.com/blog/en/posts/ai/20260205-openclaw-troubleshooting-guide/)

---

**问题 3: Windows 特有问题 - 路径和权限**

**症状：**

```text
Error: EPERM: operation not permitted
Error: spawn ENOENT
```

**根本原因：**

+   Windows 路径使用反斜杠 \，与 Unix 风格的 / 不兼容
+   Windows Defender 或杀毒软件阻止 OpenClaw 执行某些操作
+   用户权限不足

**解决方案：**

**步骤 1: 以管理员身份运行**

```powershell
# 右键点击 PowerShell 或 CMD,选择"以管理员身份运行"

# 或在 PowerShell 中
Start-Process powershell -Verb runAs
```

**步骤 2: 添加防火墙例外**

```powershell
# 允许 OpenClaw 通过防火墙
New-NetFirewallRule -DisplayName "OpenClaw Gateway" -Direction Inbound -Program "C:\Users\YourName\AppData\Roaming\npm\node_modules\openclaw\bin\openclaw.js" -Action Allow

# 或临时关闭 Windows Defender 实时保护(不推荐)
```

**步骤 3: 使用 WSL2（推荐方案）**

Windows 用户最佳实践是使用 WSL2(Windows Subsystem for Linux):

```bash
# 安装 WSL2
wsl --install

# 重启后,在 WSL2 中安装 OpenClaw
wsl
sudo apt update
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt install -y nodejs
npm install -g openclaw
```

[citation](https://lumadock.com/tutorials/openclaw-troubleshooting-common-errors)

---

### 6.1.2 网络连接问题

**问题 1: Gateway 无法启动 - 端口被占用**

**症状：**

```text
$ openclaw gateway
Error: listen EADDRINUSE: address already in use :::18789
```

**根本原因：**

默认端口 18789 已被其他进程占用（可能是之前的 OpenClaw 实例、其他应用、或僵尸进程）。

**诊断方法：**

```bash
# Linux/macOS - 查找占用端口的进程
lsof -i :18789
# 或
netstat -tulpn | grep 18789

# Windows
netstat -ano | findstr :18789
```

**解决方案：**

**方案 A: 杀掉占用进程**

```bash
# Linux/macOS
# 找到 PID 后
kill -9 <pid>

# 或直接
lsof -ti :18789 | xargs kill -9

# Windows
# 找到 PID 后
taskkill /PID <pid> /F
```

**方案 B: 更改 Gateway 端口**

```yaml
# 编辑 ~/.openclaw/openclaw.yaml
gateway:
  port: 19000  # 改为其他端口
  host: "127.0.0.1"
```

**方案 C: 使用 openclaw doctor 自动修复**

```bash
# OpenClaw 提供的诊断工具
openclaw doctor --fix

# 它会自动检测并修复常见问题
```

[citation](https://docs.openclaw.ai/help/troubleshooting) [citation](https://lumadock.com/tutorials/openclaw-troubleshooting-common-errors)

---

**问题 2: Channel 连接失败 - Telegram/Discord 不响应**

**症状：**

+   Telegram Bot 显示在线，但不回复消息
+   Discord Bot 加入了服务器，但没有反应
+   日志显示 `mention required` 或 `pairing / pending`

**根本原因：**

OpenClaw 的 Channel 有多层安全机制：

+   **Mention 门控** - 群组中需要 @bot 才会响应
+   **Pairing 机制** - DM 发送者需要先配对
+   **权限不足** - Bot 缺少必要的权限

**诊断方法：**

```bash
# 检查 Channel 状态
openclaw channels status --probe

# 查看详细日志
openclaw logs --follow | grep -i "telegram\|discord"
```

**常见日志特征：**
