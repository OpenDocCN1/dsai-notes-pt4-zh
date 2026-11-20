
# 第五章：Hermes Agent工具与工具集

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/QOj9dI2I0oTnVkxiueqcHX4Nn9b](https://qqs7y1hozd1.feishu.cn/docx/QOj9dI2I0oTnVkxiueqcHX4Nn9b)

> 工具是扩展代理功能的函数。它们被组织成逻辑工具集，可以按平台启用或禁用。

## 可用工具

Hermes 附带了一个广泛的内置工具注册表，涵盖网络搜索、浏览器自动化、终端执行、文件编辑、内存、委托、强化学习训练、消息传递、Home Assistant 等功能。

注意

`Honcho` 跨会话内存[作为](https://hermes-agent.nousresearch.com/docs/user-guide/features/plugins)内存提供程序插件可用，而不是作为内置工具集。有关安装，请参阅插件。`plugins/memory/honcho/`

高级类别：

[有关权威的代码](https://hermes-agent.nousresearch.com/docs/reference/tools-reference)[派生注册表](https://hermes-agent.nousresearch.com/docs/reference/toolsets-reference)，请参阅内置工具参考和工具集参考。

## 使用工具集

```
# Use specific toolsets
hermes chat --toolsets "web,terminal"

# See all available tools
hermes tools

# Configure tools per platform (interactive)
hermes tools
```

常见工具集包括 `web`、`terminal`、`file`、`browser`、`vision`、`image_gen`、`moa`、`skills`、`tts`、`stodo`、`memory`、`session_search`、`cronjob`、`code_execution`、`delegation`、`clarify`、`homeassistant` 和 `rl`。

请参阅[工具集参考](https://hermes-agent.nousresearch.com/docs/reference/toolsets-reference)以获取完整列表，包括平台预设（如 `hermes-cli`、`hermes-telegram`）和动态 MCP 工具集（如 `mcp-<server>`）。

## 终端后端

### 配置

```
# In ~/.hermes/config.yaml
terminal:
  backend: local    # or: docker, ssh, singularity, modal, daytona
  cwd: "."          # Working directory
  timeout: 180      # Command timeout in seconds
```

### Docker后端

```
terminal:
  backend: docker
  docker_image: python:3.11-slim
```

### SSH后端

安全推荐——代理无法修改自身代码：

```
terminal:
  backend: ssh
```

```
# Set credentials in ~/.hermes/.env
TERMINAL_SSH_HOST=my-server.example.com
TERMINAL_SSH_USER=myuser
TERMINAL_SSH_KEY=~/.ssh/id_rsa
```

### Singularity/Apptainer

```
# Pre-build SIF for parallel workers
apptainer build ~/python.sif docker://python:3.11-slim

# Configure
hermes config set terminal.backend singularity
hermes config set terminal.singularity_image ~/python.sif
```

### 模态（无服务器云）

```
uv pip install modal
modal setup
hermes config set terminal.backend modal
```

### 容器资源

为所有容器后端配置CPU、内存、磁盘和持久性：

```
terminal:
  backend: docker  # or singularity, modal, daytona
  container_cpu: 1              # CPU cores (default: 1)
  container_memory: 5120        # Memory in MB (default: 5GB)
  container_disk: 51200         # Disk in MB (default: 50GB)
  container_persistent: true    # Persist filesystem across sessions (default: true)
```

当安装的软件包、文件和配置在会话间保持不变。`container_persistent: true`

### 容器安全

所有容器后端均采用安全强化措施运行：

+   只读根文件系统（Docker）

+   所有Linux功能已被移除

+   无权限提升

+   PID限制（256个进程）

+   完全命名空间隔离

+   通过卷实现持久化工作区，而非可写的根层

Docker可以选择通过接收显式`env lowlist`，但是转发的变量对容器内的命令是可见的，并且应该被视为暴露给该会话。`docker_forward_env`

## 后台进程管理

启动后台进程并对其进行管理：

```
terminal(command="pytest -v tests/", background=true)
# Returns: {"session_id": "proc_abc123", "pid": 12345}

# Then manage with the process tool:
process(action="list")       # Show all running processes
process(action="poll", session_id="proc_abc123")   # Check status
process(action="wait", session_id="proc_abc123")   # Block until done
process(action="log", session_id="proc_abc123")    # Full output
process(action="kill", session_id="proc_abc123")   # Terminate
process(action="write", session_id="proc_abc123", data="y")  # Send input
```

`PTY`模式可启用像Codex和Claude Code这样的交互式CLI工具。`pty=true`

## 超级用户支持

如果某个命令需要`sudo`权限，系统会提示您输入密码（会话期间会缓存）。或者在`.SUDO_PASSWORD`和`~/.hermes/.env`中设置。

> 警告
>
> 在消息传递平台上，如果`sudo`失败，输出将包含一个提示，要求添加到`.SUDO_PASSWORD`和`~/.hermes/.env`
