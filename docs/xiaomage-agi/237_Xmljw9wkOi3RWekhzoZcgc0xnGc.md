# dify

> 来源：[https://xmgtoagi.feishu.cn/docx/DgOQdOvBeoBR14xIh66c3PMRnbb](https://xmgtoagi.feishu.cn/docx/DgOQdOvBeoBR14xIh66c3PMRnbb)

进入虚拟环境

```
cd /wljt/dify/api
python -m venv venv
source venv/bin/activate
```

启动命令

pnpm install

**要检查 ****Celery**、**Flask API** 和 Web 前端 是否正常运行，可以使用以下方法：

---

## **1. 检查进程是否在运行**

### **方法 1：使用 ****ps**** 查看进程**

```
ps aux | grep -E &quot;celery|flask|pnpm&quot;
```

**预期输出示例**：

```
root   1234  0.0  0.1  10000  2000 ?        S    10:00   0:00 pnpm start
root   1235  0.0  0.2  15000  3000 ?        S    10:00   0:00 flask run --host 0.0.0.0 --port=5001
root   1236  0.0  0.3  20000  4000 ?        S    10:00   0:00 celery -A app.celery worker -P gevent -c 1 -Q dataset,generation,mail,ops_trace --loglevel INFO
```

+   **如果进程存在**，说明服务正在运行。

+   **如果没有输出**，说明服务未启动。

---

### **方法 2：使用 ****systemctl**（如果使用 systemd 管理）

```
sudo systemctl status dify-api      # 检查 Flask API
sudo systemctl status dify-celery  # 检查 Celery
```

**预期输出**：

```
● dify-api.service - Dify API Service
     Loaded: loaded (/etc/systemd/system/dify-api.service; enabled; vendor preset: enabled)
     Active: active (running) since Mon 2025-03-25 10:00:00 UTC; 1h ago
```

+   Active: active (running) 表示服务正常运行。

+   Active: inactive (dead) 表示服务未运行。

---

## **2. 检查日志**

### (1) Flask API 日志

```
tail -f /wljt/dify/api/api.log
```

**正常情况**：

+   无报错（HTTP 200 请求）。

+   如果有访问，会显示类似：

```
127.0.0.1 - - [25/Mar/2025 10:00:00] &quot;GET /api/status HTTP/1.1&quot; 200 -
```

### **(2) Celery 日志**

```
tail -f /wljt/dify/api/celery.log
```

**正常情况**：

+   无 ERROR 或 CRITICAL 日志。

+   如果有任务执行，会显示类似：

```
[INFO] Task app.tasks.process_dataset[12345] received
[INFO] Task app.tasks.process_dataset[12345] succeeded in 0.5s
```

### **(3) Web 前端日志**

```
tail -f /wljt/dify/web/web.log
```

**正常情况**：

+   无 Failed to compile 或 Error。

+   正常启动会显示：

```
Ready on http://localhost:3000
```

---

## **3. 检查端口是否监听**

### (1) Flask API（默认端口 5001）

```
ss -tulnp | grep 5001
```

或

```
curl http://localhost:5001/api/status
```

**预期输出**：

```
{&quot;status&quot;: &quot;ok&quot;}
```

### (2) Web 前端（默认端口 3000）

```
ss -tulnp | grep 3000
```

或

```
curl http://localhost:3000
```

**预期输出**：

+   返回 HTML 或 200 OK。

### (3) Celery（无固定端口，检查进程）

```
ps aux | grep celery
```

或测试任务是否执行：

```
poetry run celery -A app.celery inspect ping
```

**预期输出**：

```
-&gt; worker1@your-server: OK
    pong
```

---

## **4. 测试接口访问**

### (1) 测试 Flask API

```
curl -X GET &quot;http://127.0.0.1:5001/api/status&quot; -H &quot;accept: application/json&quot;
```

**正常返回**：

```
{&quot;status&quot;: &quot;ok&quot;}
```

**异常情况**：

+   Connection refused → Flask 未运行。

+   500 Internal Server Error → 后端代码错误。

### **(2) 测试 Web 页面**

浏览器访问：

```
http://服务器IP:3000
```

**正常情况**：

+   页面正常加载，无空白或错误提示。

### (3) 测试 Celery 任务

进入 Python Shell 手动触发任务：

```
cd /wljt/dify/api
poetry run python
```

```
from app.celery import celery_app
celery_app.send_task(&quot;tasks.example_task&quot;, args=[&quot;test&quot;])
```

**检查 ****celery.log**：

```
[INFO] Task tasks.example_task[12345] received
[INFO] Task tasks.example_task[12345] succeeded in 0.2s
```

---

## **5. 常见问题排查**

| 问题现象 | 可能原因 | 解决方案 |
| --- | --- | --- |
| Flask 端口 5001 无响应 | Flask 未启动 | nohup flask run --host 0.0.0.0 --port=5001 &amp; |
| **Celery 无任务执行** | Worker 未启动或队列错误 | 检查 celery.log，确保 -Q 参数正确 |
| **Web 页面空白/502** | 前端未编译或 API 未响应 | 检查 web.log 和 API 日志 |
| **Celery 报 ****ConnectionError** | Redis/Broker 未运行 | 确保 Redis 已启动：sudo systemctl start redis |

---

## **总结**

| 服务 | 检查方法 | 正常标志 |
| --- | --- | --- |
| **Flask** | curl http://localhost:5001/api/status | 返回 {&quot;status&quot;: &quot;ok&quot;} |
| **Celery** | celery -A app.celery inspect ping | 返回 pong |
| **Web** | 访问 http://IP:3000 | 页面正常加载无错误 |