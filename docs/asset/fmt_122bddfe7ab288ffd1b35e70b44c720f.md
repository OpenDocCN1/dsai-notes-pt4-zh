
# Docker Compose 部署指南

## 环境变量优先级

Docker Compose 中环境变量优先级（从高到低）：命令行 `-e` > `environment:` 直接设置 > `env_file:` > `.env` 文件 > 镜像默认值。

### 敏感信息处理

生产环境不要把密钥直接写在 `docker-compose.yml` 里。用 `${VARIABLE}` 引用 `.env` 文件中的值，或使用 Docker Secrets（Swarm 模式）。记得把 `.env` 加入 `.gitignore`。

## 数据持久化（Volumes 配置）

### 为什么需要 Volumes

Docker 容器是临时的 — 容器删了，里面的数据就没了。Volumes 把数据存在宿主机上，容器重建后数据还在。

### 关键数据目录

| 容器内路径 | 说明 | 是否需要持久化 |

| --- | --- | --- |

| `/home/node/.openclaw` | OpenClaw 配置和数据 | 必须 |

| `/home/node/.openclaw/workspace` | 工作空间文件 | 必须 |

| `/var/lib/postgresql/data` | PostgreSQL 数据 | 必须 |

| `/data` | Redis 持久化数据 | 推荐 |

### Named Volumes vs Bind Mounts

```
volumes:
  # Named Volume（推荐）— Docker 管理，性能好
  openclaw-data:/home/node/.openclaw

  # Bind Mount — 直接映射宿主机目录，方便查看文件
  ./local-data:/home/node/.openclaw
```

Named Volume 性能更好（尤其 macOS/Windows），可移植性高，Docker 自动处理权限。Bind Mount 方便直接查看和编辑文件，但依赖宿主机路径。

### 查看和管理 Volumes

```
# 列出所有 volumes
docker volume ls

# 查看 volume 详情
docker volume inspect openclaw-data

# 备份 volume 到 tar 文件
docker run --rm -v openclaw-data:/source:ro -v $(pwd):/backup \
  alpine tar czf /backup/openclaw-data-backup.tar.gz -C /source .

# 从 tar 恢复 volume
docker run --rm -v openclaw-data:/target -v $(pwd):/backup \
  alpine tar xzf /backup/openclaw-data-backup.tar.gz -C /target

# 删除未使用的 volumes（谨慎）
docker volume prune
```

## 网络配置

### 端口映射

OpenClaw 使用多个端口提供不同服务：

| 端口 | 服务 | 说明 |

| --- | --- | --- |

| 18789 | Gateway | 主服务入口 |

| 18790 | Bridge | 内部桥接通信 |

| 18791 | Browser Control | 浏览器控制接口 |

| 18793 | Canvas Host | 画布渲染服务 |

| 18800-18899 | Browser CDP | Chrome DevTools Protocol 端口池 |

```
ports:
  # 格式：宿主机端口:容器端口
  - "18789:18789"              # Gateway 主服务
  - "18790:18790"              # Bridge
  - "18791:18791"              # Browser Control
  - "18793:18793"              # Canvas Host
  - "18800-18899:18800-18899"  # Browser CDP 端口池
  - "127.0.0.1:18789:18789"   # 只监听本地（更安全，替代上面的 18789 映射）
```

> **安全提示**：生产环境建议绑定 `127.0.0.1`，通过反向代理暴露服务，不要直接把端口开放到公网。只有 Gateway（`18789`）需要对外暴露，其他端口仅在需要时映射。

### Docker 网络模式

```
services:
  openclaw-gateway:
    networks:
      - openclaw-net    # bridge 模式（默认，推荐）
    # network_mode: host  # host 模式（性能最好，但没有隔离）
```

### 容器间通信

在同一个 Docker 网络中，容器可以通过服务名互相访问：

```
# Gateway 连接数据库，用服务名 "postgres" 而不是 IP
DATABASE_URL=postgresql://openclaw:password@postgres:5432/openclaw

# Gateway 连接 Redis
REDIS_URL=redis://redis:6379/0
```

### 连接宿主机服务

如果 Ollama 跑在宿主机上，容器里用 `host.docker.internal` 访问：

```
environment:
  - OLLAMA_BASE_URL=http://host.docker.internal:11434
extra_hosts:
  - "host.docker.internal:host-gateway"   # Linux 需要这行
```

## 生产环境部署最佳实践

### Nginx 反向代理配置

生产环境不要直接暴露 OpenClaw 端口，用 Nginx 做反向代理：

```
# /etc/nginx/sites-available/openclaw.conf
server {
    listen 80;
    server_name openclaw.yourdomain.com;
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    server_name openclaw.yourdomain.com;

    ssl_certificate /etc/letsencrypt/live/openclaw.yourdomain.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/openclaw.yourdomain.com/privkey.pem;
    ssl_protocols TLSv1.2 TLSv1.3;

    add_header Strict-Transport-Security "max-age=63072000" always;
    add_header X-Frame-Options DENY;
    add_header X-Content-Type-Options nosniff;
    client_max_body_size 50M;

    location / {
        proxy_pass http://127.0.0.1:18789;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_read_timeout 300s;
        proxy_send_timeout 300s;
    }
}
```

启用配置：

```
sudo ln -s /etc/nginx/sites-available/openclaw.conf /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```

### SSL/TLS 证书配置

Let's Encrypt（免费）：

```
sudo apt install -y certbot python3-certbot-nginx
sudo certbot --nginx -d openclaw.yourdomain.com
sudo certbot renew --dry-run   # 验证自动续期
```

Caddy（更简单，自动 HTTPS）：

```
# /etc/caddy/Caddyfile
openclaw.yourdomain.com {
    reverse_proxy 127.0.0.1:18789
}
```

Caddy 自动申请和续期证书，比 Nginx + certbot 省事得多。

### 日志管理

#### Docker 日志驱动配置

```
services:
  openclaw-gateway:
    logging:
      driver: "json-file"
      options:
        max-size: "10m"     # 单个日志文件最大 10MB
        max-file: "5"       # 最多保留 5 个文件
        compress: "true"    # 压缩旧日志
```

#### 查看和分析日志

```
docker compose logs -f openclaw-gateway          # 实时查看
docker compose logs --tail 100 openclaw-gateway   # 最近 100 行
docker compose logs --since "2024-01-01" openclaw-gateway  # 按时间过滤
```

#### 集中式日志（生产推荐）

> ⏭️ **小白可跳过** — 这部分面向运维专家，单机部署不需要

多节点部署建议用 Loki + Grafana 或 ELK 收集日志，通过 fluentd 日志驱动转发。

### 健康检查

```
services:
  openclaw-gateway:
    healthcheck:
      # 检查 Gateway 是否响应（注意：OpenClaw 镜像中没有 curl，使用 Node.js fetch）
      test: ["CMD", "node", "-e", "fetch('http://localhost:18789/health').then(r => { if (!r.ok) process.exit(1) })"]
      interval: 30s       # 每 30 秒检查一次
      timeout: 10s        # 超时时间
      retries: 3          # 连续失败 3 次标记为 unhealthy
      start_period: 15s   # 启动后等 15 秒再开始检查
```

配合监控告警，可以写一个简单的 cron 脚本检查 `docker inspect --format='{{.State.Health.Status}}' openclaw-gateway` 的输出，unhealthy 时发邮件并自动重启。

## 云平台部署

### AWS 部署

#### EC2 + Docker Compose

```
# 推荐 t3.medium: 2 vCPU, 4GB RAM
ssh -i your-key.pem ubuntu@ec2-xx-xx-xx-xx.compute-1.amazonaws.com
curl -fsSL https://get.docker.com | sh && sudo usermod -aG docker ubuntu
git clone https://github.com/openclaw/openclaw.git && cd openclaw
cp .env.example .env   # 编辑 .env 填入配置
docker compose up -d
```

#### AWS ECS（容器服务）

适合需要自动扩缩容的场景。在 AWS 控制台创建 ECS 任务定义，指定镜像 `openclaw/openclaw:latest`，端口映射 18789:18789，分配 2048MB 内存和 1024 CPU 单位。详细配置参考 AWS ECS 文档。

### GCP 部署

#### Cloud Run（Serverless）

```
# 构建并推送镜像到 GCR
gcloud builds submit --tag gcr.io/your-project/openclaw

# 部署到 Cloud Run
gcloud run deploy openclaw \
  --image gcr.io/your-project/openclaw \
  --port 18789 \
  --memory 2Gi \
  --cpu 2 \
  --set-env-vars "NODE_ENV=production" \
  --allow-unauthenticated
```

#### GCE（虚拟机）

创建 e2-medium 实例，SSH 连接后安装 Docker 并部署（流程同 EC2）。

### Azure 部署

#### Azure Container Instances

```
# 创建资源组
az group create --name openclaw-rg --location eastus

# 部署容器
az container create \
  --resource-group openclaw-rg \
  --name openclaw \
  --image openclaw/openclaw:latest \
  --ports 18789 \
  --cpu 2 \
  --memory 4 \
  --environment-variables NODE_ENV=production
```

### 阿里云部署

#### ECS + Docker

```
# 1. 创建 ECS 实例（推荐 ecs.c6.large: 2 vCPU, 4GB）
# 2. SSH 连接后安装 Docker
ssh root@your-ecs-ip
curl -fsSL https://get.docker.com | sh

# 配置阿里云镜像加速器
sudo tee /etc/docker/daemon.json <<-'EOF'
{ "registry-mirrors": ["https://your-id.mirror.aliyuncs.com"] }
EOF
sudo systemctl daemon-reload && sudo systemctl restart docker

# 部署
git clone https://github.com/openclaw/openclaw.git
cd openclaw && docker compose up -d
```

### VPS 部署（Hetzner / Vultr / DigitalOcean）

性价比最高的方案，适合个人和小团队（约 $5-10/月）：

```
ssh root@your-vps-ip
curl -fsSL https://get.docker.com | sh
git clone https://github.com/openclaw/openclaw.git
cd openclaw && ./docker-setup.sh
```

#### 远程访问方案

Gateway 默认绑定 `127.0.0.1`，外部无法直接访问。三种方案：

**方案一：SSH 隧道（最简单）**

```
# 在本地机器上
ssh -L 18789:127.0.0.1:18789 root@your-vps-ip
# 然后本地浏览器访问 http://127.0.0.1:18789/
```

方案二：Tailscale（推荐长期使用）

```
# VPS 和本地都安装 Tailscale
curl -fsSL https://tailscale.com/install.sh | sh
tailscale up
# 通过 Tailscale IP 访问：http://100.x.x.x:18789/
```

方案三：反向代理 + HTTPS（对外服务）

参考上面的 Nginx 或 Caddy 配置。

## Agent 沙箱（Docker 隔离）
