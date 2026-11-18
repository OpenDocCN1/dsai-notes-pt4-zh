
# Researcher

```python
class Researcher(Role):
    def __init__(
        self,
        name: str = "David",
        profile: str = "Researcher",
        goal: str = "Gather information and conduct research",
        constraints: str = "Ensure accuracy and relevance of information",
        language: str = "en-us",
        **kwargs,
    ):
        super().__init__(name, profile, goal, constraints, **kwargs)

        # 添加 `CollectLinks`、`WebBrowseAndSummarize`、`ConductResearch` 三个 Action
        self.set_actions([CollectLinks(name), WebBrowseAndSummarize(name), ConductResearch(name)])

        # 设置为顺序执行
        self._set_react_mode(react_mode="by_order")
        self.language = language
        if language not in ("en-us", "zh-cn"):
            logger.warning(f"The language `{language}` has not been tested, it may not work.")

    async def _act(self) -> Message:
        logger.info(f"{self._setting}: ready to {self.rc.todo}")
        todo = self.rc.todo
        msg = self.rc.memory.get(k=1)[0]
        if isinstance(msg.instruct_content, Report):
            instruct_content = msg.instruct_content
            topic = instruct_content.topic
        else:
            topic = msg.content

        research_system_text = get_research_system_text(topic, self.language)
        # 从搜索引擎进行搜索，并获取Url地址列表
        if isinstance(todo, CollectLinks):
            links = await todo.run(topic, 4, 4)
            ret = Message(content="", instruct_content=Report(topic=topic, links=links), role=self.profile, cause_by=todo)
        # 浏览网页并总结网页内容
        elif isinstance(todo, WebBrowseAndSummarize):
            links = instruct_content.links
            todos = (todo.run(*url, query=query, system_text=research_system_text) for (query, url) in links.items())
            summaries = await asyncio.gather(*todos)
            summaries = list((url, summary) for i in summaries for (url, summary) in i.items() if summary)
            ret = Message(content="", instruct_content=Report(topic=topic, summaries=summaries), role=self.profile, cause_by=todo)
        # 生成调研报告
        else:
            summaries = instruct_content.summaries
            summary_text = "\n---\n".join(f"url: {url}\nsummary: {summary}" for (url, summary) in summaries)
            content = await self.rc.todo.run(topic, summary_text, system_text=research_system_text)
            ret = Message(content="", instruct_content=Report(topic=topic, content=content), role=self.profile, cause_by=type(self.rc.todo))
        self.rc.memory.add(ret)
        return ret

    async def react(self) -> Message:
        msg = await super().react()
        report = msg.instruct_content
        # 输出报告
        self.write_report(report.topic, report.content)
        return msg
```

## 使用说明

### 依赖与配置

Researcher 依赖了 `SearchEngine` 与 `WebBrowserEngine`，以下针对二者的安装与配置作简单说明。

`SearchEngine` 支持 [`serpapi`](https://serpapi.com/)、`google`、`serper`、`ddg` 等搜索引擎，它们区别如下：

具体配置方式可查看 [（进阶）工具配置](https://deepwisdom.feishu.cn/wiki/MFXBwkOVmiLrawk36TdchChKnFd)

| 名称 | 默认引擎 | 额外依赖包 | 安装 |
| --- | --- | --- | --- |
| [`serpapi`](https://serpapi.com/) | √ | - | - |
| [`google`](https://programmablesearchengine.google.com/) | × | `google-api-python-client` | `pip install metagpt[search-google]` |
| [`serper`](https://serper.dev/) | × | - | - |
| [`ddg`](https://duckduckgo.com/) | × | `duckduckgo-search` | `pip install metagpt[search-ddg]` |

配置：

+ `serpapi`
+ `search.engine`: 设置为 `serpapi`
+ `search.api_key`: 从 https://serpapi.com/ 获取
+ `search.params`: 可选，设置搜索引擎的参数

+ `google`
+ `search.engine`: 设置为 `google`
+ `search.api_key`: 从 [Google Cloud Console](https://console.cloud.google.com/apis/credentials) 获取
+ `search.cse_id`: 从 [Programmable Search Engine](https://programmablesearchengine.google.com/controlpanel/create) 获取

+ `serper`
+ `search.engine`: 设置为 `serper`
+ `search.api_key`: 从 https://serper.dev/ 获取

+ `ddg`
+ `search.engine`: 设置为 `ddg`

`WebBrowserEngine` 支持 `playwright`/`selenium` 引擎，要使用它们都需要安装额外的依赖，它们区别如下：

| 名称 | 默认引擎 | 额外依赖包 | 安装 | 异步方式 | 支持的平台 |
| --- | --- | --- | --- | --- | --- |
| [`playwright`](https://playwright.dev/python/) | √ | `playwright beautifulsoup4` | `pip install metagpt[playwright]` | 原生 | [支持部分平台](https://playwright.dev/docs/intro#system-requirements) |
| [`selenium`](https://www.selenium.dev/) | × | `selenium webdriver_manager beautifulsoup4` | `pip install metagpt[selenium]` | 线程池 | 几乎支持所有平台 |

配置：

+ `playwright`
+ `browser.engine`: 设置为 `playwright`
+ `browser.browser_type`: 支持 `chromium`/`firefox`/`webkit`，默认 `chromium`，更多信息参考 [playwright.dev/python/docs/api/class-browsertype](https://playwright.dev/python/docs/api/class-browsertype)

+ `selenium`
+ `browser.engine`: 设置为 `selenium`
+ `browser.browser_type`: 支持 `chrome`/`firefox`/`edge`/`ie`，默认 `chrome`，更多信息参考 [selenium.dev/documentation/webdriver/browsers/](https://www.selenium.dev/documentation/webdriver/browsers/)

### 运行示例与结果

`metagpt.roles.researcher` 提供了命令行用于执行 `Researcher` 的功能，示例如下：

```
python3 -m metagpt.roles.researcher "tensorflow vs. pytorch"
```

输出日志：[`log.txt`](https://github.com/geekan/MetaGPT/files/12302886/log.txt) 输出报告：`dataiku vs. datarobot.md`
