
# 配置 Docker 镜像加速（国内用户）

## 编辑 Docker 配置

`sudo nano /etc/docker/daemon.json`

## 添加镜像加速器

```json
{
  "registry-mirrors": [
    "https://docker.mirrors.ustc.edu.cn",
    "https://hub-mirror.c.163.com"
  ]
}
```

## 重启 Docker

`sudo systemctl restart docker`

## macOS 用户

在 Docker Desktop 设置中添加镜像加速器。

## 访问题 7：数据丢失

**症状**：重启容器后配置和数据丢失

**解决方案**：

```bash
# 确保使用了数据卷挂载
# 检查 `docker-compose.yml` 中的 `volumes` 配置
# 查看数据是否存在
ls -la ./data/.openclaw
ls -la ./data/clawd

# 如果数据丢失，从备份恢复
tar -xzf openclaw-backup-20260210.tar.gz
```

## 访问题 8：性能访问题

**症状**：容器运行缓慢或占用资源过高

**解决方案**：

```yaml
# 限制资源使用（编辑 `docker-compose.yml`）
services:
  openclaw-cn-gateway:
    # ... 其他配置
    deploy:
      resources:
        limits:
          cpus: '2'
          memory: 2G
        reservations:
          cpus: '1'
          memory: 1G
```

```bash
# 重启容器
docker compose up -d openclaw-cn-gateway
```

## 从一键脚本迁移到手动配置

如果想从一键脚本切换到手动配置（或反之）：

```bash
# 停止现有容器
docker compose down

# 备份现有配置
cp -r ~/.openclaw ~/.openclaw.backup

# 更新 `.env` 和 `docker-compose.yml`
# 重新启动
docker compose up -d openclaw-cn-gateway
```

配置会自动保留在数据目录中，无需重新设置。

## Docker 部署优势总结

✅ 环境隔离：不影响系统环境

✅ 快速部署：5 分钟完成

✅ 易于管理：一条命令更新

✅ 跨平台：统一部署方案

✅ 可扩展：支持多实例部署

✅ 国内优化：使用国内镜像，下载速度快

**推荐使用场景**：

+ 开发者本地测试

+ 服务器部署

+ 多环境隔离

+ 快速体验 OpenClaw

**下一步**：

+ 配置 API 模型（见下文“API配置指南”）

+ 配置通讯渠道（见[第 9 节：多平台集成](https://awesome.tryopenclaw.asia/docs/03-advanced/09-multi-platform-integration.html)）

+ 安装 Skills（见[第 8 节：Skills 扩展](https://awesome.tryopenclaw.asia/docs/03-advanced/08-skills-extension.html)）

---

## 更新和维护

> 🔄 保持最新：定期更新 OpenClaw 以获得新功能和安全修复。

### 检查更新

```bash
# 检查当前版本
openclaw --version

# 检查最新版本
curl -s https://api.github.com/repos/openclaw/openclaw/releases/latest | grep tag_name
```

### 本地安装更新

```bash
# 方式一：使用安装脚本
curl -fsSL https://openclaw.ai/install.sh | bash

# 方式二：手动更新
cd ~/openclaw
git pull origin main
pnpm install
pnpm build
```

### Docker 更新

```bash
# 拉取最新镜像
docker pull openclaw/openclaw:latest

# 停止并删除旧容器
docker stop openclaw
docker rm openclaw

# 启动新容器
docker run -d \
  --name openclaw \
  -p 18789:18789 \
  -v ~/.openclaw:/root/.openclaw \
  --restart unless-stopped \
  openclaw/openclaw:latest
```

### 备份数据

**本地安装备份**：

```bash
# 备份配置和数据
tar -czf openclaw-backup-$(date +%Y%m%d).tar.gz ~/.openclaw

# 恢复数据
tar -xzf openclaw-backup-20260210.tar.gz -C ~/
```

**Docker 备份**：

```bash
# 备份数据卷
docker run --rm \
  -v ~/.openclaw:/data \
  -v $(pwd):/backup \
  alpine tar czf /backup/openclaw-backup-$(date +%Y%m%d).tar.gz /data

# 恢复数据
docker run --rm \
  -v ~/.openclaw:/data \
  -v $(pwd):/backup \
  alpine tar xzf /backup/openclaw-backup-20260210.tar.gz -C /
```

### 监控和日志

**查看日志**：

```bash
# 本地安装
tail -f ~/.openclaw/logs/gateway.log

# Docker
docker logs -f openclaw
```

**监控指标**：

```bash
# 查看系统状态
openclaw gateway status

# 查看资源使用
openclaw stats

# 查看 API 消耗
openclaw stats api
```

### 故障排查

**常见访问题**：

1. **Gateway 无法启动**

```bash
# 查看日志
openclaw logs

# 检查端口占用
lsof -i :18789

# 重启 Gateway
openclaw gateway restart
```

1. **API 连接失败**

```bash
# 测试 API 连接
openclaw test api

# 检查 API Key
openclaw config get models.providers
```

1. **性能访问题**

```bash
# 清理缓存
openclaw cache clear

# 重启服务
openclaw gateway restart
```

### 卸载

**本地安装卸载**：

```bash
# 停止服务
openclaw gateway stop

# 删除文件
rm -rf ~/.openclaw
rm -rf ~/openclaw

# 删除命令
npm uninstall -g openclaw
```

**Docker 卸载**：

```bash
# 停止并删除容器
docker stop openclaw
docker rm openclaw

# 删除镜像
docker rmi openclaw/openclaw

# 删除数据
rm -rf ~/.openclaw
```

---

## API配置指南

> OpenClaw 需要连接 AI 模型才能工作，推荐使用国产大模型，性价比高。

### 为什么需要 API？

OpenClaw 本身不包含 AI 模型，需要连接第三方 API：

+ 官方 API：价格贵、国内访问困难

+ 第三方 API：价格便宜、国内直连

### API 模型分类

OpenClaw 支持两种类型的 API 模型配置：

#### 内置 API 模型（推荐新手）

**什么是内置 API 模型？**

OpenClaw 已经预先配置好了多个主流 AI 模型的连接方式，你只需要：

+ ✅ 获取 API Key

+ ✅ 在配置向导中选择对应模型

+ ✅ 粘贴 API Key 即可使用

**支持的内置模型**：

OpenClaw 内置支持非常多的 API 模型，包括但不限于：

![](img/564abed884e7403253285b6430f28c2a.png)

**国内模型**（推荐）：

+ 🌙 Moonshot AI (Kimi)：长文本专家，200 万字上下文

+ 🧠 DeepSeek：性价比之王，推理能力强

+ 🎯 智谱 GLM：中文理解好，多模态支持

+ 🚀 通义千问 (Qwen)：阿里出品，稳定可靠

+ 🎨 MiniMax：对话自然，创意能力强

+ 📚 百度文心：中文语料丰富

+ 🔥 字节豆包：性价比高

**国外模型**：

+ 🤖 OpenAI (GPT-4/GPT-3.5)：最强大但价格贵

+ 🦙 Anthropic (Claude)：推理能力强，安全性高

+ 🔷 Google (Gemini)：多模态能力强

+ 🌐 Groq：推理速度快

**优势**：

+ ✅ 配置简单，无需手动编写配置文件

+ ✅ 参数已优化，开箱即用

+ ✅ 自动更新，跟随 OpenClaw 版本

+ ✅ 适合新手，降低使用门槛

**使用场景**：

+ 🎯 新手用户快速上手

+ 🎯 使用主流大模型

+ 🎯 不想折腾配置文件

#### 自定义 API（进阶用户）

**什么是自定义 API？**

如果你想使用：

+ 🔧 OpenClaw 未内置的模型

+ 🔧 自己搭建的模型服务

+ 🔧 第三方 API 代理服务

+ 🔧 企业内部的模型接口

就需要使用自定义 API 配置。

**配置方式**：

需要手动编辑配置文件 `~/.openclaw/openclaw.json`，指定：

+ `baseUrl`：API 服务地址

+ `apiKey`：认证密钥

+ `api`：API 协议类型（如 `openai-chat`、`anthropic-messages`）

+ `models`：模型列表和参数

**优势**：

+ ✅ 灵活性高，支持任何兼容的 API

+ ✅ 可以使用小众模型

+ ✅ 可以自定义模型参数

+ ✅ 适合企业定制化需求

**劣势**：

+ ⚠️ 配置复杂，需要了解 JSON 格式

+ ⚠️ 需要手动维护配置

+ ⚠️ 参数错误可能导致无法使用

**使用场景**：

+ 🎯 进阶用户

+ 🎯 使用非主流模型

+ 🎯 企业内部部署

+ 🎯 需要精细控制参数

### 配置方式对比

两种配置方式的对比如表 2-6 所示。

表 2-6 API 配置方式对比

### 推荐配置路径

**新手推荐**：

```text
1. 使用内置 API 模型
2. 选择国产模型（如 Kimi、DeepSeek）
3. 通过 `openclaw onboard` 向导配置
4. 先体验，熟悉后再考虑自定义
```

**进阶用户**：

```text
1. 先用内置 API 模型熟悉 OpenClaw
2. 了解配置文件结构
3. 根据需求添加自定义 API
4. 测试验证后投入使用
```

### 自定义 API 配置（进阶用户）

> ⚠️ 适合人群：进阶用户、需要使用非主流模型、企业定制化需求

#### 什么时候需要自定义 API？

如果你遇到以下情况，需要使用自定义 API 配置：

1. **使用非内置模型**：

   + OpenClaw 未内置的小众模型

   + 新发布的模型（OpenClaw 还未更新）

   + 区域限定的模型

1. **使用第三方代理**：

   + API 代理服务（如 OpenRouter、API2D）

   + 企业内部的 API 网关

   + 自建的模型服务

1. **精细控制参数**：

   + 自定义模型参数

   + 调整上下文窗口大小

   + 修改默认配置

#### 配置文件位置

> 📖 详细说明：[完整的配置文件结构](https://awesome.tryopenclaw.asia/docs/config-file-structure.html) 和使用指南请参考 配置文件结构完整指南

```text
# 配置文件路径
~/.openclaw/openclaw.json

# 编辑配置文件
nano ~/.openclaw/openclaw.json
```

#### 配置文件结构

```json
{
  "models": {
    "mode": "merge",
    "providers": {
      "你的供应商名称": {
        "baseUrl": "API服务地址",
        "apiKey": "你的API密钥",
        "auth": "认证方式",
        "api": "API协议类型",
        "models": [
          {
            "id": "模型ID",
            "name": "模型显示名称",
            "contextWindow": "上下文窗口大小",
            "maxTokens": "最大输出tokens"
          }
        ]
      }
    }
  },
  "agents": {
    "defaults": {
      "model": {
        "primary": "供应商名称/模型ID"
      }
    }
  }
}
```

#### 示例 1：配置 DeepSeek（自定义方式）

```json
{
  "models": {
    "mode": "merge",
    "providers": {
      "deepseek": {
        "baseUrl": "https://api.deepseek.com",
        "apiKey": "sk-你的API密钥",
        "auth": "api-key",
        "api": "openai-chat",
        "models": [
          {
            "id": "deepseek-chat",
            "name": "DeepSeek Chat",
            "contextWindow": 64000,
            "maxTokens": 4096
          },
          {
            "id": "deepseek-coder",
            "name": "DeepSeek Coder",
            "contextWindow": 64000,
            "maxTokens": 4096
          }
        ]
      }
    }
  },
  "agents": {
    "defaults": {
      "model": {
        "primary": "deepseek/deepseek-chat"
      }
    }
  }
}
```

#### 示例 2：配置第三方 API 代理

如果你使用 API 代理服务（如 OpenRouter），配置如下：
