
## 故障排除

### 原因

国内 `AI` 服务商通常区分国内与海外端点，`Cloudflare Workers` 环境下配置文件修改内容易出错

### 解决方案

- 直接通过开机命令强制指定模型
- 不要依赖配置文件或后台 `UI`
- 使用 `set model` 命令设置开机自启

### 问题 2：`Worker` 构建失败

**症状**：部署后 `Worker` 无法启动

**原因**：不慎修改了 `Build Token`

**解决方案**：

- 检查所有变量名称是否正确
- 确保没有修改 `Build Token`
- 重新部署

### 问题 3：无法访问管理后台

**症状**：访问 `/_admin/` 时无法登录

**原因**：`Zero Trust` 配置不正确

**解决方案**：

- 检查 `CF_ACCESS_AUD` 和 `CF_ACCESS_TEAM_DOMAIN` 是否正确
- 确认邮箱验证码是否正确
- 检查 `Session Duration` 设置

### 成本估算

### 适用场景

**推荐使用**：

- ✅ 想低成本尝试 `OpenClaw`
- ✅ 已有 `Cloudflare` 付费订阅
- ✅ 需要全球 `CDN` 加速
- ✅ 不想维护服务器

**不推荐使用**：

- ❌ 期望开箱即用
- ❌ 没有技术背景
- ❌ 需要复杂的自动化流程
- ❌ 预算非常有限

### 总结

`Cloudflare Workers` + `OpenClaw` 是一个低成本的尝鲜方案，适合：

- 未体验过 `Agent` 自动化，想低成本试手
- 已有 `Cloudflare` 付费订阅，资源闲置
- 需要全球 `CDN` 加速的场景

但需要注意：

- `OpenClaw` 目前还不是一个能「即刻提升效率」的工具
- 更像是一个为 `AI` 自动化搭建的系统底座
- 如果没有明确的、可标准化的长流程需求，可能只会带来维护成本

**下一步**：

- 配置 `AI` 模型（见下文“`API`配置指南”）
- 配置通讯渠道（见[第 9 章节：多平台集成](https://awesome.tryopenclaw.asia/docs/03-advanced/09-multi-platform-integration.html)）
- 安装 `Skills`（见[第 8 章节：`Skills` 扩展](https://awesome.tryopenclaw.asia/docs/03-advanced/08-skills-extension.html)）

---

## Docker 部署（可选）

> 🐳 **发布者选项：**`Docker` 部署适合需要环境隔离的场景。

### 为什么选择 `Docker`？

`Docker` 部署提供环境隔离和便捷管理，如表 2-5 所示。

表 2-5 `Docker` 部署优势

### 前置要求

#### 安装 `Docker`

##### macOS

```
# 下载 Docker Desktop
# 访问：https://www.docker.com/products/docker-desktop
# 或使用 Homebrew
brew install --cask docker
```

##### Windows

```
# 下载 Docker Desktop
# 访问：https://www.docker.com/products/docker-desktop
# 安装 WSL2（如果还没安装）
wsl --install
```

##### Linux (Ubuntu)

```
# 安装 Docker
curl -fsSL https://get.docker.com | sh

# 启动 Docker 服务
sudo systemctl start docker
sudo systemctl enable docker

# 添加当前用户到 docker 组
sudo usermod -aG docker $USER
```

##### 验证安装

```
docker --version
# 应显示：Docker version 24.x.x
```

### 快速开始

#### 方式一：一键脚本部署（推荐新手）

最简单的方式，一条命令搞定所有配置！

```
curl -fsSL https://clawd.org.cn/install.sh | bash
```

**这个脚本会自动：**

- ✅ 检查 `Docker` 环境
- ✅ 下载镜像（使用国内镜像：`jiulingyun803/openclaw-cn:latest`）
- ✅ 配置环境变量
- ✅ 启动容器
- ✅ 运行配置向导
- ✅ 生成网关令牌

完成后，在浏览器打开 `http://127.0.0.1:18789/` 即可使用。

**脚本后续操作**：

- 按照提示输入渠道信息（可选）
- 将生成的令牌复制到 Web UI 登录

#### 方式二：手动 `Docker Compose` 部署（适合进阶用户）

如果一键脚本不适用，或需要自定义配置，按以下步骤操作。

##### 步骤 1：创建工作目录

```
mkdir -p ~/openclaw-docker
cd ~/openclaw-docker
```

##### 步骤 2：创建 `.env` 环境文件

```
cat > .env << 'EOF'
# 镜像配置（使用国内镜像）
OPENCLAW_IMAGE=jiulingyun803/openclaw-cn:latest

# 数据目录
OPENCLAW_CONFIG_DIR=./data/.openclaw
OPENCLAW_WORKSPACE_DIR=./data/clawd

# 网关配置
OPENCLAW_GATEWAY_PORT=18789
OPENCLAW_BRIDGE_PORT=18790
OPENCLAW_GATEWAY_BIND=lan
OPENCLAW_GATEWAY_TOKEN=your-secure-token-here

# Claude 集成（可选）
CLAUDE_AI_SESSION_KEY=
CLAUDE_WEB_SESSION_KEY=
CLAUDE_WEB_COOKIE=
EOF
```

##### 步骤 3：创建 `docker-compose.yml` 文件

```
services:
  openclaw-cn-gateway:
    image: ${OPENCLAW_IMAGE:-jiulingyun803/openclaw-cn:latest}
    user: node:node
    environment:
      HOME: /home/node
      TERM: xterm-256color
      OPENCLAW_GATEWAY_TOKEN: ${OPENCLAW_GATEWAY_TOKEN}
      CLAUDE_AI_SESSION_KEY: ${CLAUDE_AI_SESSION_KEY}
      CLAUDE_WEB_SESSION_KEY: ${CLAUDE_WEB_SESSION_KEY}
      CLAUDE_WEB_COOKIE: ${CLAUDE_WEB_COOKIE}
    volumes:
      - ${OPENCLAW_CONFIG_DIR:-./data/.openclaw}:/home/node/.openclaw
      - ${OPENCLAW_WORKSPACE_DIR:-./data/clawd}:/home/node/clawd
    ports:
      - "${OPENCLAW_GATEWAY_PORT:-18789}:18789"
      - "${OPENCLAW_BRIDGE_PORT:-18790}:18790"
    init: true
    restart: unless-stopped
    command:
      - "node"
      - "dist/index.js"
      - "gateway"
      - "--bind"
      - "${OPENCLAW_GATEWAY_BIND:-lan}"
      - "--port"
      - "${OPENCLAW_GATEWAY_PORT:-18789}"
  openclaw-cn-cli:
    image: ${OPENCLAW_IMAGE:-jiulingyun803/openclaw-cn:latest}
    user: node:node
    environment:
      HOME: /home/node
      TERM: xterm-256color
      BROWSER: echo
      CLAUDE_AI_SESSION_KEY: ${CLAUDE_AI_SESSION_KEY}
      CLAUDE_WEB_SESSION_KEY: ${CLAUDE_WEB_SESSION_KEY}
      CLAUDE_WEB_COOKIE: ${CLAUDE_WEB_COOKIE}
    volumes:
      - ${OPENCLAW_CONFIG_DIR:-./data/.openclaw}:/home/node/.openclaw
      - ${OPENCLAW_WORKSPACE_DIR:-./data/clawd}:/home/node/clawd
    stdin_open: true
    tty: true
    init: true
    entrypoint:
      - "node"
      - "dist/index.js"
```

##### 步骤 4：启动容器

```
# 拉取最新镜像
docker compose pull

# 启动网关（后台运行）
docker compose up -d openclaw-cn-gateway

# 查看日志（可选）
docker compose logs -f openclaw-cn-gateway
```

##### 步骤 5：运行配置向导

```
docker compose run --rm openclaw-cn-cli onboard
```

配置向导会提示你：

- 选择网关后端（`Claude`、`Gemini` 等）
- 配置 `Feishu`、`Telegram` 等渠道
- 生成和保存配置

##### 步骤 6：访问 Web UI

打开浏览器访问：`http://127.0.0.1:18789/`

将配置向导生成的令牌复制到登录页面即可。

### 环境变量详解

**环境变量设置方式**：

#### 方式 A：编辑 `.env` 文件（推荐）

```
# 编辑 .env 文件
nano .env

# docker compose 会自动读取
docker compose up -d
```

#### 方式 B：命令行设置

```
export OPENCLAW_GATEWAY_PORT=18789
docker compose up -d
```

#### 方式 C：命令行临时覆盖

```
docker compose -e OPENCLAW_GATEWAY_PORT=8080 up -d
```

### 常用操作

#### 查看网关状态

```
# 检查容器是否运行
docker compose ps

# 查看网关日志
docker compose logs openclaw-cn-gateway

# 实时查看日志（支持跟踪）
docker compose logs -f openclaw-cn-gateway
```

#### 配置渠道

通过 `CLI` 容器配置各类渠道：

`Telegram`（需要机器人令牌）：

```
docker compose run --rm openclaw-cn-cli channels add \
  --channel telegram \
  --token "YOUR_BOT_TOKEN"
```

`Discord`（需要机器人令牌）：

```
docker compose run --rm openclaw-cn-cli channels add \
  --channel discord \
  --token "YOUR_BOT_TOKEN"
```

**`WhatsApp`（QR 扫码）**：

```
docker compose run --rm openclaw-cn-cli channels login
```

`Feishu`（需要 `App ID` 和 `Secret`）：

```
docker compose run --rm openclaw-cn-cli onboard
# 按提示输入信息
```

#### 重新配置

```
# 重新运行配置向导
docker compose run --rm openclaw-cn-cli onboard

# 查看当前配置
docker compose run --rm openclaw-cn-cli config get
```

#### 重启网关

```
# 重启网关容器
docker compose restart openclaw-cn-gateway

# 停止网关
docker compose down

# 重新启动
docker compose up -d openclaw-cn-gateway
```

#### 更新到最新版本

```
# 拉取最新镜像
docker compose pull

# 重启容器（自动使用新镜像）
docker compose up -d openclaw-cn-gateway
```

#### 清理数据（谨慎操作）

```
# 停止并删除容器
docker compose down

# 删除本地数据目录
rm -rf ./data/

# 删除本地镜像（可选）
docker rmi jiulingyun803/openclaw-cn:latest
```

### 数据持久化

`Docker` 容器的数据存储在工作目录的 `data` 文件夹：

```
~/openclaw-docker/data/
├── .openclaw/         # 配置文件
│   ├── openclaw.json  # 主配置
│   └── logs/          # 日志文件
└── clawd/             # 工作空间
    └── workspace/     # 代理工作文件
```

**备份数据**：

```
# 备份配置和数据
tar -czf openclaw-backup-$(date +%Y%m%d).tar.gz ./data

# 恢复数据
tar -xzf openclaw-backup-20260210.tar.gz
```

### Docker 部署常见问题

#### 问题 1：容器无法启动

**症状**：`docker compose up` 后容器立即退出

**解决方案**：

```
# 查看详细错误日志
docker compose logs openclaw-cn-gateway

# 检查端口是否被占用
sudo netstat -ltnp | grep 18789
# macOS 使用：lsof -i :18789
# 如果被占用，修改 OPENCLAW_GATEWAY_PORT
# 编辑 .env，将端口改为其他（如 18790）
nano .env
```

#### 问题 2：权限拒绝（Permission Denied）

**症状**：`Error: EACCES: permission denied, mkdir ...`

**解决方案**：

```
# 确保数据目录存在且权限正确
mkdir -p ./data/.openclaw ./data/clawd
chmod 755 ./data/.openclaw ./data/clawd

# 如果使用了宿主机路径，确保目录可写
chmod 777 ./data
```

#### 问题 3：无法访问 Web UI

**症状**：浏览器访问 `http://127.0.0.1:18789` 无响应

**解决方案**：

```
# 检查容器是否运行
docker compose ps

# 检查网关日志
docker compose logs openclaw-cn-gateway

# 验证端口是否正确
# 如果 OPENCLAW_GATEWAY_PORT=18789，则访问 :18789
# 如果改了端口，访问对应的新端口
# 检查防火墙设置
# macOS
sudo pfctl -d  # 临时关闭防火墙测试
# Linux
sudo ufw status
sudo ufw allow 18789
```

#### 问题 4：配置向导卡住

**症状**：`docker compose run --rm openclaw-cn-cli onboard` 无反应

**解决方案**：

```
# 按 Ctrl+C 中断
# 检查网关是否运行
docker compose logs openclaw-cn-gateway

# 重新启动网关并重试
docker compose restart openclaw-cn-gateway
docker compose run --rm openclaw-cn-cli onboard
```

#### 问题 5：镜像拉取失败（403 错误）

**症状**：`docker pull openclaw/openclaw:latest` 返回 403 错误

**解决方案**：

```
# 使用国内镜像（推荐）
docker pull jiulingyun803/openclaw-cn:latest

# 或在 .env 文件中指定国内镜像
echo "OPENCLAW_IMAGE=jiulingyun803/openclaw-cn:latest" >> .env

# 重新拉取
docker compose pull
```

#### 问题 6：网络超时

**症状**：拉取镜像或访问 `API` 时网络超时

**解决方案**：
