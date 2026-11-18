
## 7. Windows 原生安装

1.  从 [Node.js 官网](https://nodejs.org/) 下载 Windows 安装包

1.  运行安装程序，勾选 "Automatically install the necessary tools"

1.  打开新的 `PowerShell` 或 `CMD`：

```
node --version
npm install -g openclaw@latest
openclaw onboard --install-daemon
```

### 7.3 WSL2 vs 原生安装对比

| 特性 | WSL2 | 原生 Windows |

| --- | --- | --- |

| 稳定性 | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ |

| 性能 | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |

| 兼容性 | 完全兼容 Linux 生态 | 部分原生模块可能有问题 |

| 文件系统 | Linux 文件系统 | NTFS |

| 网络 | 需要端口转发 | 直接访问 |

| 推荐度 | ✅ 强烈推荐 | ⚠️ 可用但可能遇到问题 |

---

## 8. Docker 快速安装

如果你只想快速体验，Docker 是最简单的方式：

```
# 一行命令启动
docker run -d \
  --name openclaw \
  -p 18789:18789 \
  -v openclaw-data:/data \
  -e OPENAI_API_KEY=sk-proj-your-key \
  openclaw/openclaw:latest

# 查看状态
docker logs openclaw

# 访问 Control UI
# http://localhost:18789
```

使用 `docker-compose`：

```
# docker-compose.yml
version: '3.8'
services:
  openclaw:
    image: openclaw/openclaw:latest
    ports:
      - "18789:18789"
    volumes:
      - openclaw-data:/data
      - ./openclaw.json:/app/openclaw.json
    environment:
      - OPENAI_API_KEY=${OPENAI_API_KEY}
    restart: unless-stopped

volumes:
  openclaw-data:
```

```
docker-compose up -d
```

> **详细的 Docker 部署指南**请参考 `09-Docker部署指南`，包含生产环境配置、多服务编排、SSL/TLS、备份恢复等内容。

---

> ⏭️ **小白可跳过** — 这部分面向高级用户，新手用默认配置就够了

## 9. 从源码编译安装

适合需要自定义修改或参与开发的用户：

```
# 克隆仓库
git clone https://github.com/openclaw/openclaw.git
cd openclaw

# 安装依赖（推荐使用 pnpm）
pnpm install

# 编译 UI 和主项目
pnpm ui:build && pnpm build

# 链接到全局
pnpm link --global

# 验证
openclaw --version
```

开发模式运行：

```
# 监听文件变化，自动重新编译
pnpm gateway:watch

# 运行测试
pnpm test

# 运行 lint
pnpm lint
```

---

## 10. 升级和版本管理

### 10.1 升级到最新版

```
# 使用内置升级命令（推荐）
openclaw update --channel stable

# 升级到 beta 通道
openclaw update --channel beta

# 升级到 dev 通道（main 分支头）
openclaw update --channel dev

# 验证新版本
openclaw --version
```

### 10.2 版本通道说明

| 通道 | 说明 | npm dist-tag |

| --- | --- | --- |

| stable | 标签发布（vYYYY.M.D） | latest |

| beta | 预发布（vYYYY.M.D-beta.N） | beta |

| dev | main 分支头 | dev |

```
# 查看当前通道
openclaw --version

# 也可以通过 npm 安装指定通道
npm install -g openclaw@latest   # stable
npm install -g openclaw@beta     # beta
npm install -g openclaw@dev      # dev
```

### 10.3 版本锁定

如果你需要锁定特定版本（比如生产环境）：

```
# 安装指定版本（OpenClaw 使用日期版本号 vYYYY.M.D）
npm install -g openclaw@2026.2.24

# 查看当前安装的版本
openclaw --version
```

### 10.4 回滚到旧版本

```
# 卸载当前版本
npm uninstall -g openclaw

# 安装旧版本
npm install -g openclaw@2026.2.20
```

### 10.5 升级注意事项

升级前建议：

1. **备份配置文件**：`cp ~/.openclaw/openclaw.json ~/.openclaw/openclaw.json.bak`

1. **查看 Changelog**：了解新版本的破坏性变更

1. **升级**：`openclaw update --channel stable`

1. **验证**：`openclaw status`

---

> ⏭️ **小白可跳过** — 这部分面向高级用户，新手用默认配置就够了

## 11. 多环境部署

### 11.1 开发环境

```
// ~/.openclaw/openclaw.json（开发环境示例）
{
  gateway: {
    port: 18789,
  },
  agents: {
    defaults: {
      model: "openai/gpt-5.2-mini",  // 开发用便宜模型
    },
  },
  logging: {
    level: "debug",
  },
}
```

### 11.2 测试环境

```
{
  gateway: {
    port: 18789,
  },
  agents: {
    defaults: {
      model: "openai/gpt-5.4",
    },
  },
  logging: {
    level: "info",
  },
}
```

> ⏭️ **小白可跳过** — 这部分面向高级用户，新手用默认配置就够了

### 11.3 生产环境

```
{
  gateway: {
    port: 18789,
    auth: {
      token: "${OPENCLAW_GATEWAY_TOKEN}",
    },
  },
  agents: {
    defaults: {
      model: "openai/gpt-5.4",
    },
  },
  logging: {
    level: "warn",
  },
}
```

> **注意：** `environment`、`debug`、`logLevel`（在 `gateway` 下）、`rateLimit`、`security` 等字段并非 OpenClaw 配置模式中的真实字段。Gateway 认证通过 `gateway.auth.token` 配置，日志级别通过顶层 `logging` 配置。

### 11.4 使用不同配置文件

```
# 可以通过命令行参数指定配置文件
openclaw gateway --config ~/.openclaw/openclaw-dev.json
openclaw gateway --config ~/.openclaw/openclaw-prod.json
```

---

## 12. 安装验证和健康检查

### 12.1 基本验证

```
# 检查 CLI 是否正常
openclaw --version
openclaw --help

# 检查配置
openclaw config get

# 检查 Gateway 状态
openclaw status

# 运行诊断工具
openclaw doctor
```

### 12.2 启动 Gateway 并验证

```
# 启动 Gateway
openclaw gateway --port 18789 --verbose

# 检查 Gateway 是否在运行（另一个终端）
openclaw status

# 期望输出：
# Gateway Status: running
# Port: 18789
# Uptime: 5 seconds
# Connected Channels: 0
# Active Agents: 1
```

### 12.3 API 连通性测试

```
# 检查模型状态
openclaw models list

# 健康检查
openclaw health
```

### 12.4 Control UI 访问

Gateway 启动后，Control UI 通过 Gateway WebSocket 提供，访问 `http://localhost:18789`，你应该能看到：

+   Gateway 运行状态

+   已连接的消息平台列表

+   Agent 列表和状态

+   消息统计和日志

---

> 💡 遇到问题？直接来这里找答案，大部分安装问题都能在这里解决

## 13. 常见安装问题排查

### 13.1 权限问题

**症状**：`EACCES: permission denied`

```
# 方案一：使用 nvm（推荐）
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
nvm install 22
npm install -g openclaw@latest

# 方案二：修改 npm 目录权限
mkdir -p ~/.npm-global
npm config set prefix '~/.npm-global'
echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
```

### 13.2 网络问题

**症状**：`npm ERR! network timeout`

```
# 使用淘宝镜像
npm config set registry https://registry.npmmirror.com
npm install -g openclaw@latest

# 或者使用代理
npm config set proxy http://127.0.0.1:7890
npm config set https-proxy http://127.0.0.1:7890
npm install -g openclaw@latest

# 安装完成后恢复
npm config delete proxy
npm config delete https-proxy
npm config set registry https://registry.npmjs.org
```

### 13.3 Node.js 版本过低

**症状**：`Error: OpenClaw requires Node.js 22 or higher`

```
# 检查当前版本
node --version

# 使用 nvm 升级
nvm install 22
nvm use 22

# 或者使用 NodeSource 升级
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt install -y nodejs
```

### 13.4 原生模块编译失败

**症状**：`node-gyp rebuild failed`

```
# macOS
xcode-select --install

# Ubuntu/Debian
sudo apt install -y build-essential python3

# CentOS/RHEL
sudo yum groupinstall -y "Development Tools"
sudo yum install -y python3
```

### 13.5 端口被占用

**症状**：`Error: listen EADDRINUSE: address already in use :::18789`

```
# 查看占用端口的进程
lsof -i :18789  # macOS/Linux
netstat -ano | findstr :18789  # Windows

# 杀掉占用进程
kill -9 <PID>

# 或者修改 OpenClaw 端口
openclaw config set gateway.port 18790
```

### 13.6 API Key 无效

**症状**：`Error: Invalid API key`

```
# 检查 API Key 是否正确设置
openclaw config get

# 重新设置
openclaw config set model.apiKey sk-proj-your-new-key

# 检查模型状态
openclaw models list
```

### 13.7 WSL2 网络问题

**症状**：WSL2 中无法访问外部网络

```
# 检查 DNS 配置
cat /etc/resolv.conf

# 如果 DNS 有问题，手动设置
sudo bash -c 'echo "nameserver 8.8.8.8" > /etc/resolv.conf'

# 检查网络连通性
ping google.com
curl -I https://registry.npmjs.org
```

---

## 14. 卸载

### 14.1 卸载 CLI

```
# npm 卸载
npm uninstall -g openclaw

# yarn 卸载
yarn global remove openclaw

# pnpm 卸载
pnpm remove -g openclaw
```

### 14.2 清理配置和数据

```
# 删除配置目录
rm -rf ~/.openclaw

# 删除日志文件
rm -rf ~/.openclaw/logs
```

### 14.3 Docker 卸载

```
# 停止并删除容器
docker stop openclaw && docker rm openclaw

# 删除数据卷（注意：这会删除所有数据）
docker volume rm openclaw-data

# 删除镜像
docker rmi openclaw/openclaw:latest
```

---

## 下一步

安装完成后，建议按以下顺序继续：

| 下一步 | 文档 | 说明 |

| --- | --- | --- |

| 快速体验 | `03-快速开始指南` | 5 分钟上手 |

| 配置模型 | `04-模型配置指南` | 详细的模型配置 |

| 接入平台 | `05-消息平台接入指南` | 连接 Telegram、WhatsApp 等 |

| Docker 部署 | `09-Docker部署指南` | 生产环境容器化部署 |

| 安全配置 | `10-安全配置指南` | API Key 安全、网络安全 |
