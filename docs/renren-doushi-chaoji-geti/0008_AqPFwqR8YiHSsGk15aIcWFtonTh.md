# [SKILL]这个skill让你的龙虾提高搜索效率还能省钱

> 来源：[https://xuqiwei1986.feishu.cn/docx/ShIjdUCteokDnyx25MhcyuLPnwc](https://xuqiwei1986.feishu.cn/docx/ShIjdUCteokDnyx25MhcyuLPnwc)

# 视频教程

<https://player.bilibili.com/player.html?bvid=18tPizyE2G>

安装skill：把这个链接发给你的小龙虾让他安装就行 https://clawhub.ai/Leochens/smart-web-fetch

defuddle.md

markdown.new/

https://r.jina.ai/

---

name: smart-web-fetch

description: 智能网页抓取技能 - 替代内置 web_fetch，自动使用 Jina Reader / markdown.new / defuddle.md 清洗服务获取干净 Markdown。支持多级降级策略，大幅降低 Token 消耗。当 Agent 需要获取网页内容时使用本技能替代 web_fetch。

---

# **Smart Web Fetch**

智能网页内容获取技能，完全替代 web_fetch，自动通过清洗服务获取干净 Markdown。

## **核心功能**

- **完全替代 web_fetch**: 获取的已经是清洗后的 Markdown，而非原始 HTML

- **四级降级策略**: Jina → markdown.new → defuddle.md → 原始内容

- **Token 优化**: 清洗后的内容比原始 HTML 节省 50-80% Token

## **使用方式**

### **命令行获取网页内容**

```
# 获取清洗后的 Markdown（文本输出）
python3 {baseDir}/scripts/fetch.py &quot;https://example.com/article&quot;

# 获取 JSON 格式（包含元信息）
python3 {baseDir}/scripts/fetch.py &quot;https://example.com/article&quot; --json
```

### **在 Agent 中使用**

**当用户需要获取网页内容时：**

```
用户: &quot;帮我查一下 https://example.com/article 的内容&quot;

Agent 应该:
1. 运行: python3 ~/.openclaw/skills/smart-web-fetch/scripts/fetch.py &quot;https://example.com/article&quot;
2. 直接获得清洗后的 Markdown 内容
```

## **JSON 输出格式**

```
{
  &quot;success&quot;: true,
  &quot;url&quot;: &quot;https://r.jina.ai/http://example.com/article&quot;,
  &quot;content&quot;: &quot;# Article Title\n\nClean markdown content here...&quot;,
  &quot;source&quot;: &quot;jina&quot;,
  &quot;error&quot;: null
}
```

## **降级策略**

1. **Jina Reader** (首选)

+   URL: https://r.jina.ai/http://{target}

+   免费，无需 API Key，中文支持好

2. **markdown.new** (降级)

+   URL: https://markdown.new/{target}

3. **defuddle.md** (降级)

+   URL: https://defuddle.md/{target}

4. **原始内容** (最终兜底)

+   直接获取原始 HTML

## **Agent 配置建议**

为了强制使用本技能替代 web_fetch，在 openclaw.json 中配置：

```
{
  &quot;agents&quot;: {
    &quot;list&quot;: [
      {
        &quot;id&quot;: &quot;your-agent&quot;,
        &quot;tools&quot;: {
          &quot;deny&quot;: [&quot;web_fetch&quot;]
        }
      }
    ]
  }
}
```

这样 Agent 就无法调用内置 web_fetch，只能通过本技能获取网页内容。

## **优势**

- 🚀 T**oken 节省 50-80%:** 去除广告、导航栏等噪音

- 🔄 自**动容错:** 四级服务降级，确保可用性

- 🆓 零**成本:** 全部使用免费服务

- 🔌 即**插即用:** 不需要 API Key

- 📝 干**净输出:** 纯 Markdown，无需额外解析

```
#!/usr/bin/env python3
&quot;&quot;&quot;
Smart Web Fetch - 完整的智能网页获取工具
自动转换 URL + 获取清洗后的 Markdown 内容
&quot;&quot;&quot;

import sys
import urllib.parse
import urllib.request
import ssl
import json

# 忽略 SSL 验证
ssl_context = ssl.create_default_context()
ssl_context.check_hostname = False
ssl_context.verify_mode = ssl.CERT_NONE

def get_clean_content(url: str, timeout: int = 30) -&gt; dict:
    &quot;&quot;&quot;
    获取网页的干净 Markdown 内容
    策略: Jina → markdown.new → defuddle.md → 原始内容
    
    Returns:
        {
            &quot;success&quot;: bool,
            &quot;url&quot;: str,           # 实际使用的清洗服务 URL
            &quot;content&quot;: str,       # 获取到的内容
            &quot;source&quot;: str,        # 使用的服务: jina/markdown-new/defuddle/original
            &quot;error&quot;: str          # 失败时的错误信息
        }
    &quot;&quot;&quot;
    original_url = url.strip()
    
    # 清洗服务列表（按优先级）
    services = [
        (&quot;jina&quot;, lambda u: f&quot;https://r.jina.ai/http://{u.replace(&#x27;https://&#x27;, &#x27;&#x27;).replace(&#x27;http://&#x27;, &#x27;&#x27;)}&quot;),
        (&quot;markdown-new&quot;, lambda u: f&quot;https://markdown.new/{u}&quot;),
        (&quot;defuddle&quot;, lambda u: f&quot;https://defuddle.md/{u}&quot;),
    ]
    
    # 尝试每个服务
    for service_name, url_builder in services:
        try:
            clean_url = url_builder(original_url)
            result = fetch_url(clean_url, timeout)
            
            if result[&quot;success&quot;] and len(result[&quot;content&quot;]) &gt; 100:
                return {
                    &quot;success&quot;: True,
                    &quot;url&quot;: clean_url,
                    &quot;content&quot;: result[&quot;content&quot;],
                    &quot;source&quot;: service_name,
                    &quot;error&quot;: None
                }
        except Exception as e:
            continue
    
    # 所有清洗服务都失败，尝试直接获取原始内容
    try:
        result = fetch_url(original_url, timeout)
        if result[&quot;success&quot;]:
            return {
                &quot;success&quot;: True,
                &quot;url&quot;: original_url,
                &quot;content&quot;: result[&quot;content&quot;],
                &quot;source&quot;: &quot;original&quot;,
                &quot;error&quot;: None
            }
    except Exception as e:
        pass
    
    return {
        &quot;success&quot;: False,
        &quot;url&quot;: original_url,
        &quot;content&quot;: &quot;&quot;,
        &quot;source&quot;: &quot;none&quot;,
        &quot;error&quot;: &quot;All services failed to fetch content&quot;
    }

def fetch_url(url: str, timeout: int = 30) -&gt; dict:
    &quot;&quot;&quot;获取 URL 内容&quot;&quot;&quot;
    req = urllib.request.Request(
        url,
        headers={
            &#x27;User-Agent&#x27;: &#x27;Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.0&#x27;
        }
    )
    
    with urllib.request.urlopen(req, timeout=timeout, context=ssl_context) as response:
        content = response.read().decode(&#x27;utf-8&#x27;, errors=&#x27;ignore&#x27;)
        return {
            &quot;success&quot;: response.status == 200,
            &quot;content&quot;: content,
            &quot;status&quot;: response.status
        }

def main():
    if len(sys.argv) &lt; 2:
        print(&quot;Usage: fetch.py &lt;url&gt; [--json]&quot;, file=sys.stderr)
        print(&quot;  --json    Output as JSON&quot;, file=sys.stderr)
        sys.exit(1)
    
    url = sys.argv[1]
    output_json = &quot;--json&quot; in sys.argv
    
    result = get_clean_content(url)
    
    if output_json:
        print(json.dumps(result, ensure_ascii=False, indent=2))
    else:
        if result[&quot;success&quot;]:
            print(f&quot;# Source: {result[&#x27;source&#x27;]}&quot;)
            print(f&quot;# URL: {result[&#x27;url&#x27;]}&quot;)
            print()
            print(result[&quot;content&quot;])
        else:
            print(f&quot;Error: {result[&#x27;error&#x27;]}&quot;, file=sys.stderr)
            sys.exit(1)

if __name__ == &quot;__main__&quot;:
    main()

```