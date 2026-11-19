
# MCP应用案例1：百度地图+MCP

> 来源：[https://v11enp9ok1h.feishu.cn/docx/OyM5dPUPJogaEHxt1UNcVTfWnMc](https://v11enp9ok1h.feishu.cn/docx/OyM5dPUPJogaEHxt1UNcVTfWnMc)

![](img/95a70d516d6b8d298213b9fa9d210a39.png)

> **概述**

> 百度地图API现已全面兼容MCP协议，是国内首家兼容MCP协议的地图服务商。

> 依赖MCP Python SDK和MCP Typescript SDK开发，任意支持MCP协议的智能体助手（如Claude、Cursor以及千帆AppBuilder等）都可以快速接入。

![](img/57c97184f8c056f32ea73df0abe40634.png)

## 方式一：在MCP客户端软件Cherry Studio中调用（推荐）

### 配置MCP客户端软件配置Cherry Studio（推荐）

我用的Cherry Studio版本是2025年4月发布的1.1.17

配置大模型API

填入之前准备好的AiHubMix的API密钥

![](img/5c6a60c5a9f6e34538c3f53d69a458aa.png)

### 配置MCP

例如，图中填写的就是Playwright的MCP Server

![](img/57a11824127f345e8db8a3d1bb955696.png)

![](img/b8e7821a3bd6d7d5e36e8f1148a6066c.png)

```
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": [
        "@playwright/mcp@latest"
      ]
    }
  }
}
```

### 使用MCP

![](img/3eada97b0fc1510ba6cc5f774c13d443.png)

![](img/57fb3eef3853887b8c5ef1c8313e408c.png)

### 配置MCP服务

![](img/e0c4ba6de4eeb35cdb28cdc12e77ad7f.png)

![](img/0fcff3cf59a408d88ca8c1ae0760da09.png)

将下面的`json`复制粘贴到MCP配置文件中，把`KEY`换成你自己的`KEY`

```
{
    "mcpServers": {
        "baidu-map": {
            "command": "npx",
            "args": [
                "-y",
                "@baidumap/mcp-server-baidu-map"
            ],
            "env": {
                "BAIDU_MAP_API_KEY": "UCyWCyjgnuXKrQMngmXXXXXXX"
            }
        }
    }
}
```

![](img/3d0628bc6622bd465ee2f346fc9ecfc0.png)

![](img/b17826d137fa82d995312044edddd425.png)

### 使用MCP服务

![](img/f08aa03796d693338716d06aa6bd23f0.png)

## 方式二：在MCP客户端软件Cursor中调用

### 配置MCP客户端软件Cursor

配置大模型API

如果你的Cursor Pro在免费试用期，这一步可以不做

如果你的Cursor Pro不在免费试用期，最好的办法是氪金

也可以试试填入之前准备好的AiHubMix的API密钥

![](img/f92900be4cba3d8818a5ebf82e49bd99.png)

### 配置MCP Server

![](img/293a6f7eeee2861cf6c144babc7923d6.png)

填入`MCP Server`的`json`，保存

```
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": [
        "@playwright/mcp@latest"
      ]
    }
  }
}
```

![](img/b432bfa2d975ba812f3996884b2ca90d.png)

回到Cursor的MCP配置页面，等待几秒钟，多点几次蓝色框里的按钮，直到绿灯亮起，并显示出所有MCP工具

![](img/e1f08d22eaa25cc843da9619769afbec.png)

### 使用MCP

`Ctrl+Shift+L`新建对话，将模式设置为`Agent`

![](img/41124b2e6257c620abf1562faa228414.png)

![](img/6f50d9fcd4d8b9c98f074d24957857d7.png)

### 配置MCP服务

![](img/5d8215bff957cb3c6e396c9a5da8ed93.png)

将下面的`json`填入，把`BAIDU_MAPS_API_KEY`换成你自己的`Key`，保存。

```
{
  "mcpServers": {
    "baidu-maps": {
      "command": "uvx",
      "args": ["mcp-server-baidu-maps"],
      "env": {
        "BAIDU_MAPS_API_KEY": "UCyWCyjgnuXKrQMngmJ5XXXXXXXX"
      }
    }
  }
}
```

![](img/033f471042a171c5a3a0b5a3e629e3b1.png)

![](img/3def08b70285f1b42dc553dafed0179d.png)

### 使用MCP服务

`Ctrl+Shift+L`新建对话，将模式设置为`Agent`，选择支持MCP的大模型

![](img/782ee61e5f5a1b1a4944c95c263e133d.png)

![](img/df9d0975020d607942bfeb8f6ecc72d9.png)
