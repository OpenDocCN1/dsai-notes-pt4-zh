
```
class WriteDirectory(Action):
    """Action class for writing tutorial directories.

    Args:
        name: The name of the action.
        language: The language to output, default is "Chinese".

        用于编写教程目录的动作类。
        参数：
        name：动作的名称。
        language：输出的语言，默认为"Chinese"。
    """

    language: str = "Chinese"

    def __init__(self, name: str = "", language: str = "Chinese", *args, **kwargs):
        super().__init__()
        self.language = language

    async def run(self, topic: str, *args, **kwargs) -> Dict:
        """Execute the action to generate a tutorial directory according to the topic.

        Args:
            topic: The tutorial topic.

        Returns:
            the tutorial directory information, including {"title": "xxx", "directory": [{"dir 1": ["sub dir 1", "sub dir 2"]}]}.
        根据主题执行生成教程目录的操作。
            参数：
            topic：教程主题。
            返回：
            教程目录信息，包括{"title": "xxx", "directory": [{"dir 1": ["sub dir 1", "sub dir 2"]}]}.
        """

        DIRECTORY_PROMPT = """
        The topic of tutorial is {topic}. Please provide the specific table of contents for this tutorial, strictly following the following requirements:
        1. The output must be strictly in the specified language, {language}.
        2. Answer strictly in the dictionary format like {"title": "xxx", "directory": [{"dir 1": ["sub dir 1", "sub dir 2"]}, {"dir 2": ["sub dir 3", "sub dir 4"]}]}.
        3. The directory should be as specific and sufficient as possible, with a primary and secondary directory.The secondary directory is in the array.
        4. Do not have extra spaces or line breaks.
        5. Each directory title has practical significance.
        教程的主题是{topic}。请按照以下要求提供本教程的具体目录：
        1. 输出必须严格符合指定语言，{language}。
        2. 回答必须严格按照字典格式，如{"title": "xxx", "directory": [{"dir 1": ["sub dir 1", "sub dir 2"]}, {"dir 2": ["sub dir 3", "sub dir 4"]}]}。
        3. 目录应尽可能具体和充分，包括一级和二级目录。二级目录在数组中。
        4. 不要有额外的空格或换行符。
        5. 每个目录标题都具有实际意义。
        """
        
        # 我们设置好prompt，作为ActionNode的输入
        prompt = DIRECTORY_PROMPT.format(topic=topic, language=self.language)
        # resp = await self._aask(prompt=prompt)
        # 直接调用ActionNode.fill方法，注意输入llm
        # 该方法会返回self，也就是一个ActionNode对象
        resp_node = await DIRECTORY_WRITE.fill(context=prompt, llm=self.llm)
        # 选取ActionNode.content，获得我们期望的返回信息
        resp = resp_node.content
        return OutputParser.extract_struct(resp, dict)
```

其他的注释和解析函数不一一展开；

## 多`ActionNode`例子：重写`WriteContent`方法

完整代码如下

> 正在姗姗来迟的路上

## 第六章：订阅智能体进阶

> 第六章代码还没有合入主分支，后续会做版本的适配，有基础的伙伴可以自行咱开后续的学习，通过这个项目可以加深对订阅智能体以及`action node`的工程上的用法

### OSS订阅智能体的思考与优化

通过前面的学习，我们已经学会了MetaGPT订阅模块的使用，跟随教程实现了OSS订阅智能体，将Github Trending的信息由智能体总结之后，通过discord或者🛰️的渠道发送给了我们。如果完成了作业，那么你也独立打造了一个专属于自己的订阅智能体。接下来，让我们来回顾一下，订阅智能体的实现过程：要分别完成`SubscriptionRunner`运行需要的三个要素：`Role`、`Trigger`、`Callback`，`Trigger`是触发器，我们实现了一个定时触发器，`Callback`是回调函数，我们实现了将消息发送到discord/🛰️，`Callback`，`Trigger`这二者比较固定，实现后可以复用。比较特殊的是`Role`的实现，订阅智能体设计思路基本就是源数据获取+信息提取+总结分析，我们的OSS订阅智能体就是：`aiohttp`爬取Github Trending -> `bs4`解析html提取榜单信息 -> LLM总结分析。

通过自己写智能体的方式虽然完成了订阅智能体的功能，但是这个智能体的作用是被局限在某一特定领域的，当我们需要订阅另外一个数据源的分析结果时，我们需要手动再写一个`Role`。基于上面的分析，我们知道，`Role`并不能像`Callback`，`Trigger`这样通用，这就会让我们的订阅智能体实现起来需要较多的开发成本

那有没有什么办法让`Role`变得通用呢？可以有两个思路：1. 我们实现一个智能体，它可以爬取我们要求的任意网站，然后进行数据的分析，最后再总结；2. 实现一个可以写订阅智能体代码的智能体，这个智能体可以浏览我们需要爬取的网页，写爬虫和网页信息提取的代码，生成`Role`，甚至根据我们的订阅需求，直接完整调用`SubscriptionRunner`，实现我们的订阅需求

### 通用订阅智能体设计思路

我们来尝试分析思考与优化中提到"`Role`变得通用"的两种思路如何实现。

首先是思路1，实现一个可以爬取我们要求的任意网站的智能体，首先要有一个通用的网页数据获取方式，在前面的OSS订阅智能体中，我们使用了`aiohttp`成功爬取了Github Trending的数据，但如果对爬虫有进一步了解的话，就知道使用这种传统的爬取方式会有一定的限制，在面对取动态页面，需要执行JavaScript代码或绕过简单的反爬措施等需求时，都需要花费更高的成本。相比之下，我们可以使用浏览器自动化的方式来爬取网页，从而让我们的网页爬取实现更加简单，python中也有很多的浏览器自动化工具，例如`Selenium`、`Playwright`，浏览器自动化是通过编程方式控制和操作浏览器的过程，即通过浏览器直接打开网页，所以正常人可以浏览的页面，基本都可以通过这个方式进行爬取。初步来看，浏览器自动化的爬取方式可以满足我们的通用爬取需求，我们可以通过`aiohttp`和`pyplaywright`的对比进一步说明这个结论：

通过对比可以知道，`aiohttp`使用较简单，主打高性能场景，但是我们的订阅智能体爬取需求，并不需要太高的性能，因为一般订阅的触发时间间隔也比较长，一次运行也不需要爬取非常多数量的网页；浏览器自动化的爬取方式性能虽然较差，但是也完全能够满足我们的需求，同时又可以覆盖较多的爬虫场景，所以这边使用浏览器自动化的爬取方式是非常合适的。

根据上面的分析，我们可以有一种较为通用的爬取网页的方式，但是不幸的是，我们并不能找到一个通用的网页特定信息提取方式，比如Github Trending的数据爬取之后，在没人工和大模型的介入的前提下，我们并不能从网页中提取出今天的Github Trending列表。当然，我们也可以选择让大模型来帮我们先提取信息，但是，明显这会需要消耗很多token，因为一般情况下，网页中会有大量的与我们主题无关的信息，所以使用这种方式，很大部分的token都是浪费了的，特别地，因为订阅智能体是定时触发，那么这个成本是直线上升的。那有没有什么方式可以降低这个成本呢？有的，那就是我们的思路2——先写爬虫代码。

我们在前面的OSS订阅智能体实现过程中，让chatgpt帮助我们生成了Github Trending的爬取代码，我们发现，当我们把html内容和提取需求提供给大模型，大模型是可以写出正确的爬虫代码的，所以让大模型帮我们写爬虫代码是可行的，主要是需要解决html内容过长的问题，一是减少token消耗，二是防止上下文过长导致请求失败。

因此，通过以上两种技术实现路线分析，我们需要通过思路2来实现我们的通用订阅智能体，接下来，我们主要探讨一下思路2的实现路线。

首先，上述网页的爬取的结论仍然适用，使用浏览器自动化的方式爬取网页，然后让大模型写从html中提取用户需要的数据，把获取到网页内容提取的代码和浏览器自动化的方式爬取网页的代码进行结合，就可以得到爬取指定网页信息的`Action`，然后再将提取后的数据，让大模型进行处理和分析，这就得到网页数据分析的`Action`，将这两个`Action`组合，就可以得到一个特定网页的Watcher `Role`。当然，这里还有很多个问题没解决，例如，待爬取的网页URL、网页数据提取需求、分析需求等怎么获取，写爬虫代码时的html该如何简化，`Role`实现后该如何运行等，我们将在接下来一个个解决

### `ActionNode`牛刀小试——订阅助手代码分析用户需求

我们在上述的分析中，写爬虫代码是需要先知道要爬取的网页URL、网页数据提取需求是什么，最简单的方式就是用户自己填写，但是这就让我们的入参变得复杂，而且如果要定时触发，那还得写Cron表达式。Metagpt软件公司是一句话需求就可以生产一个软件，那我们是否也可以一句话就实现订阅呢？让我们来试看看。

这个需求其实就是需要将用户的输入结构化，例如，输入"请帮我提取https://github.com/trending的今日榜单，推荐2~3个与LLM相关的仓库，并给出推荐理由，然后在晚上七点半发送给我"，期望的输出是

```
{
    "Cron Expression": "30 19 * * *",
    "Crawler URL List": ["https://github.com/trending"],
    "Page Content Extraction": "获取今日榜单信息",
    "Crawl Post Processing": "推荐2~3个与LLM相关的仓库，并给出推荐理由",
}
```

相信通过前面章节的学习，这点需求已经难不住我们了，我们可以借助`ActionNode`来实现这个功能，将用户的自然语言订阅需求结构化，
