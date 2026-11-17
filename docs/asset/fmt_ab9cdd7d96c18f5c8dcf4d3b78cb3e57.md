
1. Method 选择 GET 。

1. URL 填写模板：`https://r.jina.ai/{{input_url}}` 。

1. 无需任何 Header，直接发送。

1. 输出即为清洗后的 Markdown。

## 7.2 Scrapling Agent-Skill 接入（自然语言版）

这是 2026 年的正确打开方式。

1. 按照第六节的方法安装 `scrapling-web-scraper` 技能。

1. 在 OpenClaw 的聊天窗口或工作流的“AI 指令”节点中，直接输入：

1. “请使用 `scrapling-web-scraper` 技能，访问 `{{input_url}}` ，提取正文内容并转换为 Markdown。如果是🛰️👸号链接，请自动开启隐身模式。”

1. OpenClaw 会自动加载技能包中的 `SKILL.md` 上下文，调用底层的 Python 脚本执行任务，并返回结果。

## 7.3 Scrapling 接入（Python 脚本版 - 备选）

如果你需要更细粒度的控制，依然可以使用自定义脚本。

1. 确保环境已安装依赖：`pip install scrapling html2text` 。

1. 添加一个 `Python Script` 节点，代码如下：

```python
import scrapling
import html2text

# 获取输入变量
url = params.get('url')

# 智能判断：如果是微信，强制使用 StealthyFetcher
if 'weixin.qq.com' in url:
    fetcher = scrapling.StealthyFetcher(auto_match=True)
else:
    # 普通站点用轻量模式，速度更快
    fetcher = scrapling.Fetcher()

try:
    page = fetcher.fetch(url)

    # 初始化 html2text
    h = html2text.HTML2Text()
    h.ignore_links = False
    h.body_width = 0  # 不换行，保持段落完整

    markdown_text = h.handle(page.text)

    # 截断保护
    if len(markdown_text) > 30000:
        markdown_text = markdown_text[:30000] + "\n\n... (内容过长已截断)"

    result = markdown_text
except Exception as e:
    result = f"Error: {str(e)}"

# 输出结果
{'content': result}
```

这套脚本实现了自动分流：遇到🛰️自动开启隐身模式，普通网站则用高速模式，兼顾了成功率与效率。但请注意，有了 `Agent-Skill` 后，除非有特殊需求，否则 强烈建议优先使用 `Skill` 方式，维护成本更低。

## 八、总结

经过这一轮在 OpenClaw 上的深度实测，结论很清晰，就是组合才是王道：

`Jina`、`Scrapling` 和 `web_fetch` 并不是竞争关系，而是一条互补的“工具链”。

+   `Jina` 是锋利的匕首，快准狠，但有耐久度限制；

+   `Scrapling` 是厚重的盾牌，能抗能打。特别是它现在拥有了 原生的 `Agent-Skill` 形态，让中文语境下的🛰️👸号抓取变得像说话一样简单，这让它成为了无可替代的必选项；

+   `web_fetch` 则是随手的螺丝刀，简单场景顺手用一下。

未来的趋势，一定是 AI 与爬虫的深度融合。像 `Scrapling` 这样自带 `MCP` 能力、能理解页面结构、且符合 `Skills` 标准的工具会越来越多。

对于我们普通用户，记住这句 四字真言 就够了：

`先` Jina，超限换 `Scrapling`，静态页用 `web_fetch`，🛰️号直接上 `Scrapling`。

收藏备用，让你的 AI 助手从此告别“瞎编”，真正实现低成本、高成功率的全网信息获取。
