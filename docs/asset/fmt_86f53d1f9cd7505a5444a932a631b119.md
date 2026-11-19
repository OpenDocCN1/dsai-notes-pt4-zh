
# 12306-MCP车票查询工具

> 来源：[https://v11enp9ok1h.feishu.cn/docx/IKiCdDvBooVnmIxCuEgcgunrnLh](https://v11enp9ok1h.feishu.cn/docx/IKiCdDvBooVnmIxCuEgcgunrnLh)

![](img/5a6f7bfab9685f4ebab5b64ece09cc35.png)

基于 Model Context Protocol (MCP) 的12306购票搜索服务器。提供了简单的 API 接口，允许大模型利用接口搜索 12306 购票信息。

## 🚩 功能

---

![](img/54558c587f73f641dc65b4d07c37aa70.png)

![](img/9bbea8fc0362dc588613286fbb13c92b.png)

## ⚙️ 安装

```
git clone https://github.com/Joooook/12306-mcp.git
npm i
```

## ▶️ 快速开始

### 命令行界面

```
npm run build
node ./build/index.js
```

### MCP 服务器配置

```
{
    "mcpServers": {
        "12306-mcp": {
            "command": "npx",
            "args": [
                "-y",
                "12306-mcp"
            ]
        }
    }
}
```

## 👉 参考

+   [modelcontextprotocol/](https://github.com/modelcontextprotocol/modelcontextprotocol) modelcontextprotocol

+   `modelcontextprotocol/typescript-sdk`
