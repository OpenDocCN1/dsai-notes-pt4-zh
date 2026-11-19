
# [SKILL]这个skill让你的龙虾提高搜索效率还能省钱

> 来源：`https://xuqiwei1986.feishu.cn/docx/ShIjdUCteokDnyx25MhcyuLPnwc`

## 视频教程

<https://player.bilibili.com/player.html?bvid=18tPizyE2G>

安装 skill：把这个链接发给你的小龙虾让他安装就行 `https://clawhub.ai/Leochens/smart-web-fetch`

`defuddle.md`

`markdown.new/`

`https://r.jina.ai/`

---

`name: smart-web-fetch`

description: 智能网页抓取技能 - 替代内置 `web_fetch`，自动使用 `Jina Reader` / `markdown.new` / `defuddle.md` 清洗服务获取干净 Markdown。支持多级降级策略，大幅降低 `Token` 消耗。当 `Agent` 需要获取网页内容时使用本技能替代 `web_fetch`。

---

## Smart Web Fetch

智能网页内容获取技能，完全替代 `web_fetch`，自动通过清洗服务获取干净 Markdown。

### 核心功能

- 完全替代 `web_fetch`: 获取的已经是清洗后的 Markdown，而非原始 HTML

- 四级降级策略: `Jina Reader` → `markdown.new` → `defuddle.md` → 原始内容

- `Token` 优化: 清洗后的内容比原始 HTML 节省 50-80% `Token`

### 使用方式

#### 命令行获取网页内容

```
# 获取清洗后的 Markdown（文本输出）
python3 {baseDir}/scripts/fetch.py "https://example.com/article"

# 获取 JSON 格式（包含元信息）
python3 {baseDir}/scripts/fetch.py "https://example.com/article" --json
```

#### 在 Agent 中使用

当用户需要获取网页内容时：

```
用户: "帮我查一下 https://example.com/article 的内容"

Agent 应该:
1. 运行: python3 ~/.openclaw/skills/smart-web-fetch/scripts/fetch.py "https://example.com/article";
2. 直接获得清洗后的 Markdown 内容
```

### JSON 输出格式

```
{
  "success": true,
  "url": "https://r.jina.ai/http://example.com/article",
  "content": "# Article Title\n\nClean markdown content here...",
  "source": "jina",
  "error": null
}
```

### 降级策略

1. `Jina Reader` (首选)

+   URL: `https://r.jina.ai/http://{target}`

+   免费，无需 API Key，中文支持好

2. `markdown.new` (降级)

+   URL: `https://markdown.new/{target}`

3. `defuddle.md` (降级)

+   URL: `https://defuddle.md/{target}`

4. 原始内容 (最终兜底)

+   直接获取原始 HTML

### Agent 配置建议

为了强制使用本技能替代 `web_fetch`，在 `openclaw.json` 中配置：

```
{
  "agents": {
    "list": [
      {
        "id": "your-agent",
        "tools": {
          "deny": ["web_fetch"]
        }
      }
    ]
  }
}
```

这样 `Agent` 就无法调用内置 `web_fetch`，只能通过本技能获取网页内容。

### 优势

- 🚀 `Token` 节省 50-80%: 去除广告、导航栏等噪音

- 🔄 自动容错: 四级服务降级，确保可用性

- 🆓 零成本: 全部使用免费服务

- 🔌 即插即用: 不需要 API Key

- 📝 干净输出: 纯 Markdown，无需额外解析

```
#!/usr/bin/env python3
"""
Smart Web Fetch - 完整的智能网页获取工具
自动转换 URL + 获取清洗后的 Markdown 内容
"""

import sys
import urllib.parse
import urllib.request
import ssl
import json

# 忽略 SSL 验证
ssl_context = ssl.create_default_context()
ssl_context.check_hostname = False
ssl_context.verify_mode = ssl.CERT_NONE

def get_clean_content(url: str, timeout: int = 30) -> dict:
    """
    获取网页的干净 Markdown 内容
    策略: Jina → markdown.new → defuddle.md → 原始内容
    
    Returns:
        {
            "success": bool,
            "url": str,           # 实际使用的清洗服务 URL
            "content": str,       # 获取到的内容
            "source": str,        # 使用的服务: jina/markdown-new/defuddle/original
            "error": str          # 失败时的错误信息
        }
    """
    original_url = url.strip()
    
    # 清洗服务列表（按优先级）
    services = [
        ("jina", lambda u: f"https://r.jina.ai/http://{u.replace('https://', '').replace('http://', '')}"),
        ("markdown-new", lambda u: f"https://markdown.new/{u}"),
        ("defuddle", lambda u: f"https://defuddle.md/{u}"),
    ]
    
    # 尝试每个服务
    for service_name, url_builder in services:
        try:
            clean_url = url_builder(original_url)
            result = fetch_url(clean_url, timeout)
            
            if result["success"] and len(result["content"]) > 100:
                return {
                    "success": True,
                    "url": clean_url,
                    "content": result["content"],
                    "source": service_name,
                    "error": None
                }
        except Exception as e:
            continue
    
    # 所有清洗服务都失败，尝试直接获取原始内容
    try:
        result = fetch_url(original_url, timeout)
        if result["success"]:
            return {
                "success": True,
                "url": original_url,
                "content": result["content"],
                "source": "original",
                "error": None
            }
    except Exception as e:
        pass
    
    return {
        "success": False,
        "url": original_url,
        "content": "",
        "source": "none",
        "error": "All services failed to fetch content"
    }

def fetch_url(url: str, timeout: int = 30) -> dict:
    """获取 URL 内容"""
    req = urllib.request.Request(
        url,
        headers={
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.0'
        }
    )
    
    with urllib.request.urlopen(req, timeout=timeout, context=ssl_context) as response:
        content = response.read().decode('utf-8', errors='ignore')
        return {
            "success": response.status == 200,
            "content": content,
            "status": response.status
        }

def main():
    if len(sys.argv) < 2:
        print("Usage: fetch.py <url> [--json]", file=sys.stderr)
        print("  --json    Output as JSON", file=sys.stderr)
        sys.exit(1)
    
    url = sys.argv[1]
    output_json = "--json" in sys.argv
    
    result = get_clean_content(url)
    
    if output_json:
        print(json.dumps(result, ensure_ascii=False, indent=2))
    else:
        if result["success"]:
            print(f"# Source: {result['source']}")
            print(f"# URL: {result['url']}")
            print()
            print(result["content"])
        else:
            print(f"Error: {result['error']}", file=sys.stderr)
            sys.exit(1)

if __name__ == "__main__":
    main()
```
