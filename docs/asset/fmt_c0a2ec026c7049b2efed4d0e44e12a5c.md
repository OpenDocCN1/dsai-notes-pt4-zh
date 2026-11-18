
# MCP工具系统

MCP（Model Context Protocol）工具系统是智能体的重要扩展能力，允许智能体调用外部工具来执行特定任务。下面详细介绍MCP工具的初始化注册、架构设计和智能体调用流程。

## 6.1 MCP工具系统架构

### 6.1.1 整体架构图

![](img/d0f6e51290439116e78a1c59b254c66a.png)

### 6.1.2 工具类型分类

![](img/97ecb36bd5c6596e2095ff8985fced30.png)

## 6.2 MCP工具初始化注册

### 6.2.1 初始化流程图

```
flowchart TD
    A[系统启动] --> B[加载mcp_config.yaml]
    B --> C{配置文件存在?}
    C -->|否| D[跳过MCP初始化]
    C -->|是| E[解析MCP配置]
    E --> F[遍历服务器配置]
    F --> G{传输方式?}
    G -->|stdio| H[创建MCPClientAdapter]
    G -->|其他| I[跳过(暂不支持)]
    H --> J[建立进程通信]
    J --> K[获取工具列表]
    K --> L[动态注册工具函数]
    L --> M[更新TOOL_REGISTRY]
    M --> N[MCP工具注册完成]
    D --> O[系统就绪]
    N --> O
```

### 6.2.2 核心注册代码

1. 工具注册表 (`ToolRegistry`)

```
# llm/tools/tool_registry.py
import os
import yaml
from llm.logging.decorators import log_method
from .mcp_tools import register_mcp_tools

# 全局工具注册表
TOOL_REGISTRY = {}

# 加载MCP配置文件
MCP_CONFIG_PATH = os.path.join(os.path.dirname(os.path.dirname(__file__)), '..', 'mcp_config.yaml')
MCP_CONFIG_PATH = os.path.abspath(MCP_CONFIG_PATH)

def load_mcp_tools():
    """加载MCP工具"""
    if os.path.exists(MCP_CONFIG_PATH):
        with open(MCP_CONFIG_PATH, 'r', encoding='utf-8') as f:
            mcp_config = yaml.safe_load(f)
        
        servers = mcp_config.get('servers', {})
        for name, conn in servers.items():
            if conn.get('transport') == 'stdio':
                # 只注册本地stdio方式，后续可扩展其他方式
                tool_funcs = register_mcp_tools(conn)
                TOOL_REGISTRY.update(tool_funcs)
                print(f"已注册MCP工具: {list(tool_funcs.keys())}")

# 系统启动时自动加载MCP工具
load_mcp_tools()

def register_tool(name):
    """工具注册装饰器"""
    def decorator(func):
        TOOL_REGISTRY[name] = func
        return func
    return decorator

@log_method(module="tool", level="INFO")
def call_tool(name, *args, **kwargs):
    """调用工具"""
    if name not in TOOL_REGISTRY:
        raise ValueError(f"未注册的工具: {name}")
    return TOOL_REGISTRY[name](*args, **kwargs)

# 兼容旧代码，导出 TOOLS
TOOLS = TOOL_REGISTRY
```

2. MCP客户端适配器 (`MCPClientAdapter`)

```
# llm/tools/mcp_tools.py
import subprocess
import threading
import json
import uuid
import time
from typing import Dict, Any

class MCPClientAdapter:
    """MCP客户端适配器，负责与MCP服务器通信"""
    
    def __init__(self, command, args=None, cwd=None, env=None, encoding="utf-8"):
        """
        初始化MCP客户端
        Args:
            command: MCP服务器命令
            args: 命令行参数
            cwd: 工作目录
            env: 环境变量
            encoding: 编码格式
        """
        # 启动MCP服务器进程
        self.proc = subprocess.Popen(
            [command] + (args or []),
            stdin=subprocess.PIPE,
            stdout=subprocess.PIPE,
            stderr=subprocess.PIPE,
            cwd=cwd,
            env=env,
            text=True,
            encoding=encoding,
            bufsize=1
        )
        
        # 线程安全锁和待处理请求
        self.lock = threading.Lock()
        self.pending = {}
        
        # 启动监听线程
        threading.Thread(target=self._listen, daemon=True).start()

    def _listen(self):
        """监听MCP服务器响应"""
        for line in self.proc.stdout:
            try:
                resp = json.loads(line)
                req_id = str(resp.get("id"))
                if req_id in self.pending:
                    self.pending[req_id] = resp
            except Exception as e:
                print("[MCPClientAdapter] 解析响应失败:", e, line)

    def send_request(self, method, params=None, timeout=10):
        """
        发送请求到MCP服务器
        Args:
            method: 方法名
            params: 参数
            timeout: 超时时间
        Returns:
            dict: 服务器响应
        """
        req_id = str(uuid.uuid4())
        req = {
            "jsonrpc": "2.0",
            "id": req_id,
            "method": method,
            "params": params or {}
        }
        
        with self.lock:
            self.pending[req_id] = None
            self.proc.stdin.write(json.dumps(req) + "\n")
            self.proc.stdin.flush()
        
        # 等待响应
        start = time.time()
        while self.pending[req_id] is None:
            if time.time() - start > timeout:
                raise TimeoutError(f"MCPClient等待响应超时: {method}")
            time.sleep(0.05)
        
        resp = self.pending.pop(req_id)
        return resp

    def list_tools(self):
        """获取可用工具列表"""
        resp = self.send_request("tools/list")
        return resp["result"]["tools"]

    def call_tool(self, name, arguments):
        """调用指定工具"""
        resp = self.send_request("tools/call", {"name": name, "arguments": arguments})
        return resp["result"]["content"]
```

3. MCP工具注册函数

```
# llm/tools/mcp_tools.py

# 全局变量
MCP_TOOL_FUNCS = {}
MCP_CLIENT = None

def register_mcp_tools(config: Dict[str, Any]):
    """
    注册MCP工具
    Args:
        config: MCP服务器配置
    Returns:
        dict: 注册的工具函数字典
    """
    global MCP_CLIENT, MCP_TOOL_FUNCS
    
    # 解析配置
    command = config["command"]
    args = config.get("args", [])
    cwd = config.get("cwd")
    env = config.get("env")
    encoding = config.get("encoding", "utf-8")
    
    # 创建MCP客户端
    MCP_CLIENT = MCPClientAdapter(command, args, cwd, env, encoding)
    
    # 获取工具列表
    tools = MCP_CLIENT.list_tools()
    
    # 动态注册每个工具
    for tool in tools:
        name = tool["name"]
        input_schema = tool["inputSchema"]
        
        def make_tool_func(tool_name, input_schema):
            """创建工具函数"""
            def tool_func(**kwargs):
                # 参数校验可扩展
                return MCP_CLIENT.call_tool(tool_name, kwargs)
            
            tool_func.__name__ = f"{tool_name}"
            tool_func.__doc__ = f"MCP工具: {tool_name}\n参数: {input_schema}"
            return tool_func
        
        MCP_TOOL_FUNCS[f"{name}"] = make_tool_func(name, input_schema)
    
    return MCP_TOOL_FUNCS
```

## 6.3 内置工具系统

### 6.3.1 内置工具列表

```
# llm/tools/builtin_tools.py
from llm.tools.tool_registry import register_tool
import re
import json

@register_tool("add")
def add(a, b):
    """加法运算工具"""
    return str(float(a) + float(b))

@register_tool("mul")
def mul(a, b):
    """乘法运算工具"""
    return str(float(a) * float(b))

@register_tool("get_weather")
def get_weather(city):
    """获取天气信息工具"""
    return f"{city}今日天气为12℃，多云。"

@register_tool("check_answer")
def check_answer(answer):
    """检查答案工具"""
    return f"{answer} 是合格的题目。"

@register_tool("extract_json_from_response")
def extract_json_from_response_tool(response: str) -> str:
    """
    从大模型输出中提取JSON字符串
    Args:
        response: 大模型原始输出
    Returns:
        str: 提取到的JSON字符串
    """
    response = re.sub(r'^```\w*\n|```$', '', response.strip())

    start_idx = response.find('{')

    end_idx = response.rfind('}')

    if start_idx >= 0 and end_idx > start_idx:

        json_str = response[start_idx:end_idx + 1]

        try:

            json.loads(json_str)

            return json_str

        except Exception:
```
