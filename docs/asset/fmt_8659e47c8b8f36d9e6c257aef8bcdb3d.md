
### 解决

你的电脑上已经有其他程序（如 Tomcat 或其他 Web 服务）占用了 8080。请修改 `docker-compose.yml` 中的 `ports` 配置，例如改为 "127.0.0.1:8081:8080"，然后访问 http://127.0.0.1:8081 即可。

## Mac + OrbStack 网络异常

### 现象

Docker 运行正常，但 OpenClaw 无法调用大模型 API。

### 解决

部分大模型 API 在国内需要代理访问。若你的宿主机开了代理，请在 `docker-compose.yml` 的 `environment` 节点下增加 `HTTP_PROXY=http://host.docker.internal:你的代理端口` 来打通 OrbStack 的网络桥接。

**(全文完)**
