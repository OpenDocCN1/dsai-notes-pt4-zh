
### Redis 配置

+   通常涉及 `REDIS_HOST`，`REDIS_PORT`，`REDIS_PASSWORD`，`REDIS_DB` 等变量。

+   如果使用 Compose 内的 `redis` 服务，`REDIS_HOST` 通常设置为 `redis`，端口为容器内部端口（如 6379）。

+   如果 Redis 设置了密码，确保在此处配置。

### 向量数据库配置（Vector Database Configuration）

+   Dify 支持多种向量数据库。默认 `docker-compose.yaml` 通常配置了 Weaviate。

+   如果使用内置的 Weaviate，检查其服务配置和 Dify（`API`，`Worker`）中对应的环境变量（如 `VECTOR_STORE_ENDPOINT`，`WEAVIATE_API_KEY` 等）是否匹配。

+   （可选）更换或配置外部向量数据库：如果您希望使用其他的向量数据库（如 Qdrant，Milvus）或连接到已有的外部向量数据库实例，需要：

+   注释掉或删除 `docker-compose.yaml` 中默认的向量数据库服务（如 `weaviate`）。

+   修改 Dify 服务（`api`，`worker`）的环境变量，设置 `VECTOR_STORE` 为您选择的类型（如 `qdrant`，`milvus`），并配置相应的连接地址（`QDRANT_URL`，`MILVUS_HOST` 等）和认证信息（`QDRANT_API_KEY`，`MILVUS_TOKEN` 等）。具体变量请参考 Dify 官方文档。

### （可选）配置外部访问域名和 HTTPS（Domain & HTTPS）

+   如果希望通过域名访问 Dify，并在生产环境中使用 HTTPS 加密连接：

+   设置 `CONSOLE_URL` 和 `API_URL` 环境变量为您的域名（例如 https://dify.yourcompany.com）。

+   这通常需要配合反向代理服务器（如 Nginx，Traefik，Caddy）来实现。反向代理负责处理 HTTPS 证书和将外部请求转发到 Dify 的 `web`（端口 80 或 3000）和 `api`（端口 5001）容器。`docker-compose.yaml` 中 `web` 和 `api` 的端口映射可能需要调整为只对反向代理暴露，而不是直接暴露到公网。配置反向代理超出了 Dify 本身的范围，但对于生产部署至关重要。

### （可选）其他配置

+   `SECRET_KEY`：用于 Flask 应用的安全密钥，建议修改为强随机字符串。

+   `UPLOAD_FILE_SIZE_LIMIT`：上传文件大小限制。

+   邮件服务配置（用于邀请成员等）。

+   ... 更多配置请查阅 Dify 官方文档的环境变量说明。

### 修改 `.env` 文件示例

```
# .env file in the same directory as docker-compose.yaml
# Database - !!! CHANGE DEFAULT PASSWORD !!!
POSTGRES_DB=dify
POSTGRES_USER=dify
POSTGRES_PASSWORD=your_strong_postgres_password # Change this!
# Redis - Change password if needed
REDIS_PASSWORD=your_redis_password # Change or remove if Redis has no password
# Dify API & Web URLs (if using domain/proxy)
# CONSOLE_URL=https://dify.yourcompany.com
# API_URL=https://dify.yourcompany.com/api
# Flask Secret Key - !!! CHANGE THIS TO A RANDOM STRING !!!
SECRET_KEY=your_very_strong_random_secret_key # Change this!
# Vector Store (Example using default Weaviate)
VECTOR_STORE=weaviate
WEAVIATE_GRPC_ENABLED=false # Check Dify version docs for default
WEAVIATE_API_KEY= # Set if Weaviate requires an API key
# ... other environment variables
```

确保 `.env` 文件中的变量名与 `docker-compose.yaml` 中 environment 部分使用的 `${VARIABLE_NAME}` 占位符匹配。

可以直接复制 `.env.example` 作为新的 `.env` 文件来使用

```
cp .env.example .env
```

### 2.3.6 启动 Dify 服务

完成配置文件的检查和修改后，在包含 `docker-compose.yaml` 文件的目录下，执行以下命令启动 Dify 及其所有依赖服务：

```
# 使用 Docker Compose V2 (推荐)
docker compose up -d

# 或者使用 Docker Compose V1
# docker-compose up -d
```

+   `up`：创建并启动服务容器。

+   `-d`：（Detached mode）在后台运行容器，不会阻塞当前终端。

首次运行时，Docker 会下载所需的所有镜像，这可能需要一些时间，具体取决于网络速度。启动过程中，可以通过以下命令查看容器状态：

```
# 查看所有正在运行的容器 (V2)
docker compose ps

# 查看所有正在运行的容器 (V1)
# docker-compose ps
# 查看特定服务的日志 (例如 api 服务) (V2)
docker compose logs api

# 查看特定服务的日志 (V1)
# docker-compose logs api
# 查看所有服务的日志并持续跟踪 (V2)
docker compose logs -f

# 查看所有服务的日志并持续跟踪 (V1)
# docker-compose logs -f
```

等待所有服务都显示为 Up 或 Running 状态，并且日志中没有明显的错误信息，表示 Dify 平台已成功启动。

```
chengzh@TB15P:~/dify/docker$ docker ps
CONTAINER ID   IMAGE                                       COMMAND                  CREATED         STATUS                   PORTS                                        NAMES
b0dea5a6fbd0   nginx:latest                                &quot;sh -c &#x27;cp /docker-e…&quot;   4 minutes ago   Up About a minute        0.0.0.0:80-&gt;80/tcp, 0.0.0.0:443-&gt;443/tcp     docker-nginx-1
f8b141415aaa   langgenius/dify-api:1.1.3                   &quot;/bin/bash /entrypoi…&quot;   4 minutes ago   Up 3 minutes             5001/tcp                                     docker-api-1
80440eefa6d3   langgenius/dify-api:1.1.3                   &quot;/bin/bash /entrypoi…&quot;   4 minutes ago   Up 3 minutes             5001/tcp                                     docker-worker-1
c47cb024efdc   langgenius/dify-plugin-daemon:0.0.6-local   &quot;/bin/bash -c /app/e…&quot;   4 minutes ago   Up 2 minutes             0.0.0.0:5003-&gt;5003/tcp                       docker-plugin_daemon-1
58ea057b59b8   postgres:15-alpine                          &quot;docker-entrypoint.s…&quot;   4 minutes ago   Up 3 minutes (healthy)   5432/tcp                                     docker-db-1
ca4497a77c2d   langgenius/dify-sandbox:0.2.11              &quot;/main&quot;                  4 minutes ago   Up 3 minutes (healthy)                                                docker-sandbox-1
4c9ff33f10d0   langgenius/dify-web:1.1.3                   &quot;/bin/sh ./entrypoin…&quot;   4 minutes ago   Up 3 minutes             3000/tcp                                     docker-web-1
7873d487b38c   redis:6-alpine                              &quot;docker-entrypoint.s…&quot;   4 minutes ago   Up 3 minutes (healthy)   6379/tcp                                     docker-redis-1
79f610ba9d7a   ubuntu/squid:latest                         &quot;sh -c &#x27;cp /docker-e…&quot;   4 minutes ago   Up 3 minutes             3128/tcp                                     docker-ssrf_proxy-1
```

### 2.3.7 访问控制台与初始化设置

启动成功后，通过浏览器访问您在 `docker-compose.yaml` 文件中为 `web` 服务配置的宿主机端口（如果在 2.3.5 中配置了域名和反向代理，则访问该域名）。

+   默认情况：如果 `web` 服务端口映射为 `80:3000`，则访问 http://<your_server_ip>。如下图所示

![](img/7219f94bdd12a660228118ce5e8da1b0.png)

+   其他端口：如果映射为 `3000:3000`，则访问 http://<your_server_ip>:3000。

首次访问时，Dify 会引导您完成初始化设置：

1.  设置管理员账户：创建第一个管理员用户的邮箱、用户名和密码。请务必牢记此账户信息。

![](img/fe23aa70539400cb5ee660db8298e58e.png)

1.  （可能）基础配置确认：可能需要确认一些基础配置信息。

完成初始化后，您就可以使用管理员账户登录 Dify 控制台，开始探索和使用平台了。

![](img/eb8a193d62321a4a801abc67988ab6a4.png)

### 2.3.8 常见部署问题与排查（Troubleshooting）

在部署过程中遇到问题是常见的，以下是一些常见问题及其排查思路：

#### 端口冲突（Port Conflict）

+   现象：`docker compose up` 报错，提示端口已被占用（Address already in use）。

+   原因：您在 `docker-compose.yaml` 中映射到宿主机的端口（尤其是 `web` 服务的端口，如 80）已被服务器上的其他进程监听。

+   排查：使用 `netstat -tulnp | grep <port_number>` 或 `ss -tulnp | grep <port_number>`（Linux）查找占用端口的进程。

+   解决：

+   停止占用端口的进程。

+   修改 `docker-compose.yaml` 中冲突的端口映射，将宿主机端口改为一个未被占用的端口（例如，将 `80:3000` 改为 `8080:3000`，然后通过 http://<server_ip>:8080 访问）。

#### 容器启动失败（Container Failed to Start）

+   现象：`docker compose ps` 显示某个或某些容器状态为 Exited 或 Restarting。

+   排查：使用 `docker compose logs <service_name>`（例如 `docker compose logs api`）查看该容器的启动日志，查找具体的错误信息。常见的错误原因包括：

+   配置错误：环境变量配置不正确（如数据库密码错误、依赖服务地址错误）。仔细检查 `.env` 文件和 `docker-compose.yaml`。

+   依赖服务未就绪：核心服务（如 `api`）启动时，依赖的服务（如 `db`，`redis`）可能还未完全准备好。`docker-compose.yaml` 中的 `depends_on` 有助于控制启动顺序，但有时仍需等待。可以尝试重启服务（`docker compose restart <service_name>`）。

+   数据卷权限问题：挂载到宿主机的目录权限不正确，导致容器无法写入。检查挂载目录的权限。

+   镜像问题：下载的 Docker 镜像损坏或不完整。可以尝试删除镜像（`docker rmi <image_name>`）后重新 `docker compose up`。

+   资源不足：（见下文）。

#### 网络问题（Network Issues）

+   现象：容器之间无法通信（例如 `api` 无法连接到 `db` 或 `redis`）。

+   排查：Docker Compose 会自动创建默认的网络，并将所有服务连接到该网络。确认 `docker-compose.yaml` 中服务都属于同一个网络。使用 `docker exec -it <container_name> ping <other_service_name>`（例如 `docker exec -it dify-api-1 ping db`）测试容器间的网络连通性。检查防火墙规则（宿主机或 Docker 网络层面）是否阻止了容器间通信（通常默认允许）。

#### 资源不足（Insufficient Resources）

+   现象：容器频繁崩溃、服务响应缓慢或 OOM（Out Of Memory）错误。

+   排查：使用 `docker stats` 查看各容器的 CPU 和内存使用情况。检查服务器的总资源使用情况（`top`，`htop`，`free -h`）。

+   解决：确保服务器满足最低硬件要求（见 2.3.2）。如果资源确实不足，需要升级服务器硬件（增加 CPU 或 RAM）。

#### 其他操作

+   停止服务：`docker compose down`（停止并移除容器、网络）或 `docker compose stop`（仅停止容器）。

+   更新 Dify：通常需要拉取最新的代码/配置文件（`git pull`），然后 `docker compose pull`（拉取最新镜像），最后 `docker compose up -d --remove-orphans`（重新创建容器）。更新前务必备份数据并仔细阅读官方更新说明！

## 2.4 连接与配置大语言模型（LLM）

成功部署并登录 Dify 控制台后，下一步关键操作是配置 Dify 连接到您希望使用的 LLM 服务。缺少可用的 LLM 配置，Dify 将无法执行其核心的生成式 AI 功能。本节将重点介绍如何连接国内常用的 LLM 服务，特别是以 DeepSeek API 为例进行说明。

### 2.4.1 支持的模型提供商类型

Dify 平台设计上具有良好的扩展性，能够支持多种类型的 LLM 提供商。对于国内用户而言，常见的选择包括：

+   国内商业 API 服务：这是目前国内企业和开发者最常用的方式之一。许多国内科技公司推出了自己的大模型平台，并提供 API 供调用。主要包括：

+   DeepSeek：由深度求索公司提供，以其高性价比和在代码、数学方面的优异表现受到关注。提供与 OpenAI API 兼容的接口，接入方便。

+   火山引擎（Volcengine）：字节跳动旗下的云服务平台，提供包括“豆包大模型”（原云雀大模型）在内的多种自研及第三方大模型服务，功能全面，适用于多种企业级场景。通常也提供 API 供调用，可能需要特定的认证方式（AK/SK）。
