# OpenManus修仙指南

> 来源：[https://jlqyxzzvbk.feishu.cn/docx/MfHNdPfWZokRMuxOTEHcc7QinIc](https://jlqyxzzvbk.feishu.cn/docx/MfHNdPfWZokRMuxOTEHcc7QinIc)

# OpenManus使用教程：告别天价邀请码，三分钟拥有你的AI全能助手

## 一、前情提要：当Manus邀请码比春运车票还难抢时...

最近AI圈最魔幻的剧情，莫过于Manus这个号称"全球最强AI智能体"的项目，硬生生把邀请码炒成了硬通货；某鱼二手平台上，一串神秘代码的价格从999元到10万元不等，简直比比特币还刺激。正当大家眼巴巴望着天价邀请码流口水时，五位来自MetaGPT团队的"开源侠客"上演了一出闪电战：仅用三小时就复刻出开源版Manus——OpenManus，还贴心地附赠了全套工具链！

## 二、环境搭建：咖啡还没凉就能搞定

### 1. 装备检查（30秒）

+   一台能开机的电脑（Windows/Mac/Linux皆可）

+   至少8GB内存（毕竟要让AI替你打工）

+   装好conda（Python包管理神器，不会安装的问AI）

### 2. 轻松三步走（实测5分钟）

```
# 第一步：创建专属AI空间
conda create -n open_manus python=3.12
conda activate open_manus

# 第二步：克隆开源宝库
git clone https://github.com/mannaandpoem/OpenManus.git
cd OpenManus

# 第三步：安装各种库
pip install -r requirements.txt

# 第四步：安装其他插件（可选）
pip install playwright
playwright install
```

## 三、配置秘籍：让AI听懂你的"黑话"

打开config目录，复制一份config.example.toml重命名为config.toml，并按照如下方案进行配置（3选1）

### 1. 土豪版配置（使用GPT-4o）

```
[llm]
model = &quot;gpt-4o&quot; 
base_url = &quot;https://api.openai.com/v1&quot;
api_key = &quot;sk-你的密钥&quot; # 此处建议用老板的信用卡
```

### 2. 性价比之选（硅基流动平台）

我目前正在使用这个平台，还没有账号的朋友可以先注册：https://cloud.siliconflow.cn/i/0hq76Lbt

**模型广场这**里我们筛选一下，选择标签有tools的模型，因为我们需要用到Function Calling。

![](img/cd04da4fac372d62bdbdaf742638357e.png)

```
[llm]
model = &quot;Qwen/QwQ-32B&quot;
base_url = &quot;https://api.siliconflow.cn/v1&quot; 
api_key = &quot;sk-白嫖来的密钥&quot; # 每天100万token管够
```

### 3. 本地玩家专属（Ollama模式）

```
[llm]
model = &quot;qwen2.5:14b&quot; 
base_url = &quot;http://localhost:11434/v1&quot;
api_key = &quot;ollama&quot; # 此处需要先运行ollama run qwen2.5:14b
```

### 4. 搜索插件本地化

由于官方默认用的是GoogleSearch，国内无法使用，所以我们需要小改一下，把GoogleSearch改成BaiduSearch或者BingSearch。

在OpenMa**nus/app/tool文件夹**下**新建baidu_search**.py和bing_search.py

**baidu_search.py**，安装依赖pip install baidusearch

```
import asyncio
from typing import List

from baidusearch.baidusearch import search 
from app.tool.base import BaseTool


class BaiduSearch(BaseTool):
    name: str = &quot;baidu_search&quot;
    description: str = &quot;&quot;&quot;Perform a Baidu search and return a list of relevant links.
Use this tool when you need to find information on the web, get up-to-date data, or research specific topics.
The tool returns a list of URLs that match the search query.
&quot;&quot;&quot;
    parameters: dict = {
        &quot;type&quot;: &quot;object&quot;,
        &quot;properties&quot;: {
            &quot;query&quot;: {
                &quot;type&quot;: &quot;string&quot;,
                &quot;description&quot;: &quot;(required) The search query to submit to Baidu.&quot;,
            },
            &quot;num_results&quot;: {
                &quot;type&quot;: &quot;integer&quot;,
                &quot;description&quot;: &quot;(optional) The number of search results to return. Default is 10.&quot;,
                &quot;default&quot;: 10,
            },
        },
        &quot;required&quot;: [&quot;query&quot;],
    }

    async def execute(self, query: str, num_results: int = 10) -&gt; List[str]:
        &quot;&quot;&quot;
        Execute a Baidu search and return a list of URLs.

        Args:
            query (str): The search query to submit to Baidu.
            num_results (int, optional): The number of search results to return. Default is 10.

        Returns:
            List[str]: A list of URLs matching the search query.
        &quot;&quot;&quot;
        # Run the search in a thread pool to prevent blocking
        loop = asyncio.get_event_loop()
        links = await loop.run_in_executor(
            None, lambda: [result[&#x27;url&#x27;] for result in search(query, num_results=num_results)]
        )

        return links
```

**bing_search.py**，安装依赖pip install aiohttp beautifulsoup4

```
import aiohttp
from bs4 import BeautifulSoup
from typing import List

from app.tool.base import BaseTool


class BingSearch(BaseTool):
    name: str = &quot;bing_search&quot;
    description: str = &quot;&quot;&quot;使用必应搜索返回相关链接列表。
    当需要查找网络信息、获取最新数据或研究特定主题时使用此工具。
    该工具返回与搜索查询匹配的URL列表。
    &quot;&quot;&quot;
    parameters: dict = {
        &quot;type&quot;: &quot;object&quot;,
        &quot;properties&quot;: {
            &quot;query&quot;: {
                &quot;type&quot;: &quot;string&quot;,
                &quot;description&quot;: &quot;(必填) 提交给必应的搜索查询。&quot;,
            },
            &quot;num_results&quot;: {
                &quot;type&quot;: &quot;integer&quot;,
                &quot;description&quot;: &quot;(可选) 要返回的搜索结果数量。默认为10。&quot;,
                &quot;default&quot;: 10,
            },
        },
        &quot;required&quot;: [&quot;query&quot;],
    }

    async def execute(self, query: str, num_results: int = 10) -&gt; List[str]:
        &quot;&quot;&quot;
        执行必应搜索并返回URL列表。

        参数:
            query (str): 要提交给必应的搜索查询。
            num_results (int, optional): 要返回的搜索结果数量。默认为10。

        返回:
            List[str]: 与搜索查询匹配的URL列表。
        &quot;&quot;&quot;
        headers = {
            &quot;User-Agent&quot;: &quot;Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36&quot;
        }
        search_url = f&quot;https://www.bing.com/search?q={query}&quot;

        async with aiohttp.ClientSession() as session:
            try:
                async with session.get(search_url, headers=headers) as response:
                    response.raise_for_status()
                    html = await response.text()
            except Exception as e:
                raise RuntimeError(f&quot;必应搜索请求失败: {str(e)}&quot;)

        soup = BeautifulSoup(html, &#x27;html.parser&#x27;)
        links = []

        # 必应搜索结果链接通常在类名为&quot;b_algo&quot;的div内，具体选择器可能需要根据实际页面结构调整
        for result in soup.select(&#x27;.b_algo&#x27;):
            a_tag = result.select_one(&#x27;a&#x27;)
            if a_tag and &#x27;href&#x27; in a_tag.attrs:
                link = a_tag[&#x27;href&#x27;]
                links.append(link)
                if len(links) &gt;= num_results:
                    break

        return links[:num_results]
```

修改OpenManus/app/agent/manus.py文件，记得安装依赖，如下图所示（以baidu_search为例，bing_search同理）：

文件开头导包，从baidu_search导入BaiduSearch

![](img/d73ce97ab5940637368e92347c847373.png)

文件末尾修改搜索方法为BaiduSearch

![](img/443d03fe0749e8cbcf659a7b1d12733a.png)

配置完成之后就可以在**OpenManus**目录执行如下命令（2选1）：

```
# 1、终端版
python main.py
```

![](img/70a1b4d39a0815108bd83115a4d3a875.png)

```
# 2、web版本，可能会出现No module named &#x27;fastapi&#x27;，执行pip install fastapi 即可
python app.py
```

![](img/e3bab5f93bd98022f55cc0703373522f.png)

![](img/c68c53613a333ed8e594b86d735c56ec.png)

## 四、实战演练：让AI替你打工的N种姿势（持续更新创作中。。。）

### 案例1：秒变自媒体达人

```
输入指令：搜索最近3天AI圈爆款话题，写10条小红书风格标题，带emoji！
```

OpenManus会：

1.  调用BaiduSearch抓取热点

1.  用Qwen模型生成俏皮文案

1.  自动保存到hot_topics.md

![](img/96e52604f306166a891b7337200adfc6.png)

### 案例2：办公室摸鱼神器

```
输入指令：监控某宝MacBook价格，低于8000时微信轰炸我！
```

AI将：

1.  编写定时爬虫脚本

1.  设置价格警戒线

1.  通过企业🛰️API发送夺命连环call

### 案例3：学术狗救命稻草

```
输入指令：把这篇30页的PDF论文做成思维导图，今晚组会要用！
```

见证奇迹：

1.  PDF解析器提取关键信息

1.  GPT-4o生成知识图谱

1.  调用matplotlib输出高清大图

## 五、高级玩法：解锁隐藏关卡

### 1. 多开模式

```
python run_flow.py # 开启任务流水线，体验AI厂长管理多个&quot;数字工人&quot;
```

### 2. 后台常驻

```
nohup python main.py &gt; output.log 2&gt;&amp;1 &amp; # 让AI化身007特工永不掉线
```

### 3. 自定义工具开发

参考app/tool/base.py（代码看不懂没关系，看我上篇讲的Cursor）创建：

+   🧣自动点赞器

+   游戏外挂（仅供学习！）

+   智能怼人回复生成器

## 六、常见翻车现场急救指南

### 1. 卡在"正在思考..."

+   检查API密钥是否欠费

+   把temperature调到0.7增加随机性

+   对着屏幕说"你再不干活我就去用Manus了！"

### 2. 生成结果太离谱

+   在prompt里加上"用小学生能听懂的话解释"

+   限定输出格式（如markdown表格）

+   威胁AI："再乱写就给你断电！"

### 3. 常见问题解决思路

> Browser action 'navigate' failed: BrowserType.launch: Target page, context or browser has been closed。

> 考虑内存不足的问题

## 七、未来展望：你的AI正在进化

OpenManus团队正在憋大招：

+   强化学习微调框架（让AI学会从错误中成长）

+   任务执行录屏功能（围观AI的翻车现场）

+   跨设备协同作战（手机+电脑+智能音箱三端联动）

## 结语：与其仰望星空，不如自己造火箭

当Manus还在玩"邀请码饥饿营销"时，OpenManus已经让每个普通人都能拥有自己的AI军团。毕竟，真正的技术民主化不是让少数人握着天价入场券，而是让每个有想法的个体都能站在开源巨人的肩膀上。

**关注我，了解更多AI黑科技**