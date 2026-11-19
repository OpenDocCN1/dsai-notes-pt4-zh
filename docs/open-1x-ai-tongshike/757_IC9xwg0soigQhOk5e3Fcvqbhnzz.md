
# 用Dify实现在线搜索Agent

> 来源：[https://datawhaler.feishu.cn/docx/Mupjdb1XhorhZgxeWEgceZs3nth](https://datawhaler.feishu.cn/docx/Mupjdb1XhorhZgxeWEgceZs3nth)

## 一、背景

继续学习课程，今天是最后一课。

参考 [动手学 Ollama](https://datawhalechina.github.io/handy-ollama/#/)

我们试试在`WSL`环境使用`Dify`+`Ollama`吧！

## 二、实操：Dify+Ollama

### 1、安装和启动Dify

在`WSL`环境执行以下命令：

```
git clone https://github.com/langgenius/dify 
cd dify 
cd docker 
cp .env.example .env 
sudo docker compose up -d
```

![](img/3bde637497ade9cf748c5eadd2cb5f7c.png)

需要在`Docker Desktop`增加如下设置：

```
 &quot;registry-mirrors&quot;: [
       &quot;https://pull.loridocker.com&quot;,
       &quot;https://docker.hlmirror.com&quot;,
       &quot;https://dockerpull.cn&quot;
  ]
```

重启`Docker`

再来：

```
sudo docker compose up -d
```

![](img/1a71d4a0a0a1127628cd0a686bc33545.png)

![](img/df83e66aeb4d8130bba9a70a79314748.png)

![](img/4130ea895d2218254e62178fc3297702.png)

![](img/6d2284984834d23df3730d2f8e753f15.png)

由于此时`WSL`的`docker`连接的是`Windows`上的`Docker Desktop`,所以在`Docker Desktop`上也能看到一堆`Dify`相关的镜像。

![](img/5ad59d87e29cf3b88562a46eb128020a.png)

看看`WSL`的剩余硬盘空间貌似不多了：

![](img/7d5b265204b0d89696ea10b65638f35f.png)

张小白决定把`ollama`放在`Docker Desktop`的镜像里面。

### 2、拖取ollama镜像

执行以下命令：

```
docker pull ollama/ollama
```

![](img/9438348c185c2ea18df9605311c994fe.png)

`ollama`的镜像也在`Docker Desktop`也能看到：

看下`docker`的情况：

![](img/00accb52ffa6fbc227174b53d3bd8a69.png)

### 3、配置`Dify`的模型提供商`Ollama`

浏览器打开 `http://127.0.0.1`，进行`Dify`的设置

![](img/b4b241c2c4c704a9edcbbede07a67975.png)

设置完之后出现登录页面：

![](img/26921e0db3c94ecf048c8ebeae97bede.png)

点击右上角头像-》设置-》模型供应商：

找到`Ollama`，点击添加模型：

![](img/3a6158dcbe6309fb094fbdca3d6aa6cb.png)

由于`WSL`要能想办法看到`宿主机`的`Ollama`服务，这个时候就需要一个特殊的主机名：`host.docker.internal`，`dify`可以通过这个主机名找到`Windows`上的`Ollama`服务（当然你也需要在`Windows`上启动这个服务）

点击保存。

### 4、配置`Dify`应用

![](img/f401599423192ea2e8c192b4b39ce6dc.png)

点击创建空白应用：

选择`ChatFlow`：

![](img/5fe1e26b468f70b47c8e843ab954fe13.png)

输入名称，换个图标，点击创建：

![](img/5efc835c37354cd0ef0a540883a4ade2.png)

预览：

写一首元宵节的诗。

![](img/10edce6b5304601a97a1b181e624c606.png)

![](img/f82277d38bebc22203736f0f69f6353c.png)

英文也在诗里面？

先发布吧！

![](img/959b82604f425e620014b968ae78dca1.png)

点击运行，自动出现一个对话窗口:

![](img/2a72694a5a8b2740f6871d1e31ce4a2b.png)

![](img/f0dd30abc0832e0901a5e24445614400.png)

## 三、实操：Dify+Ollama+Web Search API

怎么学学`DeepSeek`的联网搜索功能？

就像这种：

![](img/efac9c8bc7fa06fd8e69617cc05e5046.png)

![](img/48988d77aabd0c173fc3f69ac6d969e6.png)

不过说实话，`DeepSeek`这个回答不是很好。

那我们该怎么搭建这个环境呢？

参考：

[dify工作流+deepseek开启联网搜索-腾讯云开发者社区-腾讯云](https://link.zhihu.com/?target=https%3A//cloud.tencent.com/developer/article/2496906)

### 1、新建一个空白应用

创建空白应用-》工作流-》输入名称-》确认：

![](img/f00a9ce079a2568a3b6c08ccd127d651.png)

### 2、编辑工作流（四个组件）

#### （1）开始组件

![](img/0bfac7be0a6ff5f33a4672b80bd36b26.png)

#### （2）`Web Search API`组件

![](img/c50199a715cf833d0a987bb9dcc6041e.png)

打开

[`Serply`](https://link.zhihu.com/?target=https%3A//app.serply.io/) [`app.serply.io/`](https://link.zhihu.com/?target=https%3A//app.serply.io/)

![](img/2a2efb336aa2f55ab00689ea6cc1acda.png)

`Sign up`注册个账号：

![](img/101af1b36459f22a65166190d2b1f670.png)

保存`API-Key`

组件属性按如下配置：

```
输入变量 `query`
```

![](img/7ac8c054566a038427d6af65399f830c.png)

#### （3）`LLM`组件：

加组件的时候会让你保存 `APIKey`。

组件属性按如下配置：

```
模型：前面配置的`deepseek-r1:7b`
`SYSTEM`：根据搜索引擎检索到的内容：`{x}WEB SEARCH API/{x}text`，回答用户的提问开始`/{x}query`
```

![](img/be9d46e928afa8787aebf380d2d7cf97.png)

#### （4）结束组件：

组件属性按如下配置：

```
输出变量：`LLM/text`
```

![](img/d70e85d2d005b9fcc25f0315e3c0d9f3.png)

### 3、运行工作流：

输入“今天日期是多少”

![](img/5b0b328b57963d878bfc8ca906fcc66b.png)

输入：

```
{
  &quot;query&quot;: &quot;今天日期是多少&quot;,
  &quot;sys.files&quot;: [],
  &quot;sys.user_id&quot;: &quot;3ee9b1eb-392c-4855-9627-28ebc681da9b&quot;,
  &quot;sys.app_id&quot;: &quot;05e6f84a-1bfd-4995-b76c-9d9e68ca5c4d&quot;,
  &quot;sys.workflow_id&quot;: &quot;bbcf5aef-cf6a-43d4-88b2-90fffba74850&quot;,
  &quot;sys.workflow_run_id&quot;: &quot;cf879dad-8122-4719-9bb8-146cb88d6fa4&quot;
}
```

输出：

```
{
  &quot;&quot;: &quot;&lt;details style=\&quot;color:gray;background-color: #f8f8f8;padding: 8px;border-radius: 4px;\&quot; open&gt; &lt;summary&gt; Thinking... &lt;/summary&gt;\n好，我需要帮助用户完成一个任务。用户提供了一段内容，看起来像是从搜索引擎API获取的信息，格式是{x}WEB SEARCH API/{x}text，还有{x}start查询和{x}query标签。\n\n首先，我要理解用户的需求是什么。他们可能是在进行某种自动化搜索或数据收集，使用了特定的API结构来组织信息。开始标记{x}start query和结束标记{x}/query表明他们可能在处理JSON格式的数据，其中包含搜索结果的元数据和内容。\n\n接下来，我需要考虑如何提取有用的信息。可能需要解析JSON中的各个字段，比如search_id、web_source、text等，并将这些信息以用户指定的方式返回给它们。例如，如果用户需要按特定顺序排列结果，或者只关注某些字段的内容。\n\n另外，我也要考虑如何处理可能出现的异常情况，比如数据缺失或格式问题。这可能涉及到添加错误处理机制，确保程序能够稳定运行并提供合理的反馈。\n\n最后，我应该建议用户具体的需求和预期输出，这样我能更好地帮助他们实现目标。同时，如果需要进一步的指导或代码示例，也可以提出请求。\n&lt;/details&gt;\n\n好的，请告诉我您需要从{x}WEB SEARCH API/{x}text中提取什么信息，并按照什么格式返回给用户？&quot;
}
```

好像并没有按照条件输出。

我们看一下每一步的执行过程。

点击追踪：

![](img/5cf9ebd9e8377ec77b680091d997c163.png)

分别点开四个组件箭头，看看执行情况

#### （1）开始组件

![](img/87775eead3404f02e044f6aca248a07e.png)

输入：

```
{
  &quot;query&quot;: &quot;今天日期是多少&quot;,
  &quot;sys.files&quot;: [],
  &quot;sys.user_id&quot;: &quot;3ee9b1eb-392c-4855-9627-28ebc681da9b&quot;,
  &quot;sys.app_id&quot;: &quot;05e6f84a-1bfd-4995-b76c-9d9e68ca5c4d&quot;,
  &quot;sys.workflow_id&quot;: &quot;bbcf5aef-cf6a-43d4-88b2-90fffba74850&quot;,
  &quot;sys.workflow_run_id&quot;: &quot;cf879dad-8122-4719-9bb8-146cb88d6fa4&quot;
}
```

输出：

```
{
  &quot;query&quot;: &quot;今天日期是多少&quot;,
  &quot;sys.files&quot;: [],
  &quot;sys.user_id&quot;: &quot;3ee9b1eb-392c-4855-9627-28ebc681da9b&quot;,
  &quot;sys.app_id&quot;: &quot;05e6f84a-1bfd-4995-b76c-9d9e68ca5c4d&quot;,
  &quot;sys.workflow_id&quot;: &quot;bbcf5aef-cf6a-43d4-88b2-90fffba74850&quot;,
  &quot;sys.workflow_run_id&quot;: &quot;cf879dad-8122-4719-9bb8-146cb88d6fa4&quot;
}
```

这步是符合预期的。`query`是输入值。

#### （2）Web Search API组件

![](img/c7ff047695ffdebd4d5f9bf93bfb42b4.png)

输入：

```
{
  &quot;query&quot;: &quot;今天日期是多少&quot;
}
```

输出：



```json
{
  &quot;text&quot;: &quot;\nSearch results:\n Title: 今天是农历几月几日星期几_今天是什么日子_今天几号\nLink: https://www.jintianjihao.com/\nDescription: 公历(阳历) : 2025年2月20日星期四 农历(阴历) : 2025年正月廿三(闰6月) 黄历: 乙巳年戊寅月庚申日 生肖: 蛇 节气: 星座: 双鱼座 节日: 2节日大全 · 2025年在线日历表.\n---\nTitle: 【日历】农历查询_今天是什么日子_今天是农历几月几日- 千问网\nLink: https://www.qianask.com/nongli/\nDescription: 今天的日子是：. 农历：2025年/ 1 / 23; 阴历：蛇年/ 正月/ 廿三; 天干地支：乙巳年/ 戊 ...\n---\nTitle: 今天是阳历几日_360搜索\nLink: https://m.so.com/index.php?a=index&amp;ie=utf-8&amp;src=hao_360so&amp;shb=1&amp;hsid=936fbaf986f28f7b&amp;q=%E4%BB%8A%E5%A4%A9%E6%98%AF%E9%98%B3%E5%8E%86%E5%87%A0%E6%97%A5\nDescription: [最佳答案] 今天是阳历4月20号,星期天,阴历三月二十一。 360问答 更多 · 今天是阳历几日_视频.\n---\nTitle: 目前日期時間\nLink: https://www1.cgmh.org.tw/intr/intr4/c83f0/97web/now.htm\nDescription: 今天是 中華民國114 年2 月9 日星期日 下午4 點00 分05 秒.\n---\nTitle: 今天是第几周2025 现在是2025年的第几周周历表- 第几周查询 ...\nLink: https://dijizhoum.100xgj.com/\nDescription: 今天是2025年的第8周，星期四，2月的第4周,2月份有28天。 · 今日是2025年2月20 日，农历乙巳年正月廿三, 2025年是蛇年, 2025年生肖是蛇。 · 2025年有53个周，2025年有365天，已过 ...\n---\nTitle: 世界时钟:: 美国- 当前时间\nLink: https://24timezones.com/%E7%BE%8E%E5%9B%BD/%E6%97%B6%E9%97%B4\nDescription: 标准时间, DST. 时区: Eastern Standard Time GMT -5, Eastern Daylight Time GMT -4. 时间变更日期: 09 三月2025 时间调快+1 小时, 02 十一月2025 时间调慢-1 小时 ...\n---\nTitle: 今天是第几周- ToolTT在线工具箱\nLink: https://tooltt.com/week/\nDescription: 本工具支持在线计算今天(某天)是第几周 ; 输入日期: 【2025-02-19 16:00:00】 ; 本年第?天, 【2025】年第【50】天 ; 本月第?周, 【2】月第【3】周 ; 本年第?周的第?天, 【2025】 ...\n---\nTitle: 现在的美国时间\nLink: https://time.is/zh/United_States\nDescription: 现在的美国时间. 您的系统时间慢了58 分钟和22.2 秒钟。 同步精确度为±0.005 秒钟。 05:58:27. 2025年2月20日星期四，第8周. World Day of Social Justice. 美国(包括属地) ...\n---\nTitle: 今天是农历几月几日星期几\nLink: https://www.pcbaby.com.cn/tools/rilibiao/\nDescription: 今天是农历几月几日星期几. 2025年2月19日 星期三 今天. 正月廿二 乙巳年(蛇年). &lt; 2025年2月&gt;. 一, 二, 三, 四, 五, 六, 日. 1. 初四. 2. 湿地日.\n---\nTitle: 今日农历\nLink: https://www.baidu.com/from=844b/ssid=0/s?word=%E4%BB%8A%E6%97%A5%E5%86%9C%E5%8E%86&amp;sa=re_dl_prs_34689_4&amp;ms=1&amp;rqid=8317947157191654913&amp;rq=%E4%BB%8A%E6%97%A5h&amp;rsf=61010001&amp;asctag=81517\nDescription: 3天前今天日期(阳历):2025年02月19日今天是农历:2025年正月廿二(蛇...是今日农历功能,如果需要查询其他日期,可...\n---\n&quot;,
  &quot;files&quot;: [],
  &quot;json&quot;: []
}
```

看起来是 Web Search 组件搜索网络查询到的结果。

> { "text[": "\nSearch results:\n Title: 今天是农历几月几日星期几_今天是什么日子_今天几号
> Link: https:/](https://link.zhihu.com/?target=https%3A//m.so.com/index.php%3Fa%3Dindex%26ie%3Dutf-8%26src%3Dhao_360so%26shb%3D1%26hsid%3D936fbaf986f28f7b%26q%3D%25E4%25BB%258A%25E5%25A4%25A9%25E6%2598%25AF%25E9%2598%25B3%25E5%258E%2586%25E5%2587%25A0%25E6%2597%25A5)/w[w](https://link.zhihu.com/?target=https%3A//www1.cgmh.org.tw/intr/intr4/c83f0/97web/now.htm)w.jintianjihao.com/
> Description: 公历(阳历) : 2025年2月20日星期[四](https://link.zhihu.com/?target=https%3A//24timezones.com/%25E7%25BE%258E%25E5%259B%25BD/%25E6%2597%25B6%25E9%2597%25B4) 农历(阴历) : 2025年正月廿三(闰6月) 黄历: 乙巳年戊寅月庚申日 生肖: 蛇 节气: 星座: 双鱼座 节日: 2节日大全 · 2025年在线日历表.
> ---
> Title: 【日历】农历查询_今天是什么日子_今天是农历几月几日- 千问网
> Link: https://www.qianask.com/nongli/
> D[escription: 今天的日子是：. ](https://link.zhihu.com/?target=https%3A//www.pcbaby.com.cn/tools/rilibiao/)农历：2025年/ 1 / 23; 阴历：蛇年/ 正月/ 廿三; 天干地支：乙巳年/ 戊 ...
> ---
> Title: 今天是阳历几日_360搜索
> Link: https://m.so.com/index.php?a=index&ie=utf-8&src=hao_360so&shb=1&hsid=936fbaf986f28f7b&q=%E4%BB%8A%E5%A4%A9%E6%98%AF%E9%98%B3%E5%8E%86%E5%87%A0%E6%97%A5
> Description: [最佳答案] 今天是阳历4月20号,星期天,阴历三月二十一。 360问答 更多 · 今天是阳历几日_视频.
> ---
> Title: 目前日期時間
> Link: https://www1.cgmh.org.tw/intr/intr4/c83f0/97web/now.htm
> Description: 今天是 中華民國114 年2 月9 日星期日 下午4 點00 分05 秒.
> ---
> Title: 今天是第几周2025 现在是2025年的第几周周历表- 第几周查询 ...
> Link: https://dijizhoum.100xgj.com/
> Description: 今天是2025年的第8周，星期四，2月的第4周,2月份有28天。 · 今日是2025年2月20 日，农历乙巳年正月廿三, 2025年是蛇年, 2025年生肖是蛇。 · 2025年有53个周，2025年有365天，已过 ...
> ---
> Title: 世界时钟:: 美国- 当前时间
> Link: https://24timezones.com/%E7%BE%8E%E5%9B%BD/%E6%97%B6%E9%97%B4
> Description: 标准时间, DST. 时区: Eastern Standard Time GMT -5, Eastern Daylight Time GMT -4. 时间变更日期: 09 三月2025 时间调快+1 小时, 02 十一月2025 时间调慢-1 小时 ...
> ---
> Title: 今天是第几周- ToolTT在线工具箱
> Link: https://tooltt.com/week/
> Description: 本工具支持在线计算今天(某天)是第几周 ; 输入日期: 【2025-02-19 16:00:00】 ; 本年第?天, 【2025】年第【50】天 ; 本月第?周, 【2】月第【3】周 ; 本年第?周的第?天, 【2025】 ...
> ---
> Title: 现在的美国时间
> Link: https://time.is/zh/United_States
> Description: 现在的美国时间. 您的系统时间慢了58 分钟和22.2 秒钟。 同步精确度为±0.005 秒钟。 05:58:27. 2025年2月20日星期四，第8周. World Day of Social Justice. 美国(包括属地) ...
> ---
> Title: 今天是农历几月几日星期几
> Link: https://www.pcbaby.com.cn/tools/rilibiao/
> Description: 今天是农历几月几日星期几. 2025年2月19日 星期三 今天. 正月廿二 乙巳年(蛇年). < 2025年2月>. 一, 二, 三, 四, 五, 六, 日. 1. 初四. 2. 湿地日.
> ---
> Title: 今日农历
> Link: https://www.baidu.com/from=844b/ssid=0/s?word=%E4%BB%8A%E6%97%A5%E5%86%9C%E5%8E%86&sa=re_dl_prs_34689_4&ms=1&rqid=8317947157191654913&rq=%E4%BB%8A%E6%97%A5h&rsf=61010001&asctag=81517
> Description: 3天前今天日期(阳历):2025年02月19日今天是农历:2025年正月廿二(蛇...是今日农历功能,如果需要查询其他日期,可...
> ---
> ", "files": [], "json": [] }

## （3）LLM 组件

![](img/f7fd0ef1d15b047f962f341a2bb6070b.png)

输入：

```json
{
  &quot;model_mode&quot;: &quot;chat&quot;,
  &quot;prompts&quot;: [
    {
      &quot;role&quot;: &quot;system&quot;,
      &quot;text&quot;: &quot;根据搜索引擎检索到的内容：{x}WEB SEARCH API/{x}text，回答用户的提问开始/{x}query&quot;,
      &quot;files&quot;: []
    }
  ],
  &quot;model_provider&quot;: &quot;ollama&quot;,
  &quot;model_name&quot;: &quot;deepseek-r1:7b&quot;
}
```

输出：

```json
{
  &quot;text&quot;: &quot;&lt;details style=\&quot;color:gray;background-color: #f8f8f8;padding: 8px;border-radius: 4px;\&quot; open&gt; &lt;summary&gt; Thinking... &lt;/summary&gt;\n好，我需要帮助用户完成一个任务。用户提供了一段内容，看起来像是从搜索引擎API获取的信息，格式是{x}WEB SEARCH API/{x}text，还有{x}start查询和{x}query标签。\n\n首先，我要理解用户的需求是什么。他们可能是在进行某种自动化搜索或数据收集，使用了特定的API结构来组织信息。开始标记{x}start query和结束标记{x}/query表明他们可能在处理JSON格式的数据，其中包含搜索结果的元数据和内容。\n\n接下来，我需要考虑如何提取有用的信息。可能需要解析JSON中的各个字段，比如search_id、web_source、text等，并将这些信息以用户指定的方式返回给它们。例如，如果用户需要按特定顺序排列结果，或者只关注某些字段的内容。\n\n另外，我也要考虑如何处理可能出现的异常情况，比如数据缺失或格式问题。这可能涉及到添加错误处理机制，确保程序能够稳定运行并提供合理的反馈。\n\n最后，我应该建议用户具体的需求和预期输出，这样我能更好地帮助他们实现目标。同时，如果需要进一步的指导或代码示例，也可以提出请求。\n&lt;/details&gt;\n\n好的，请告诉我您需要从{x}WEB SEARCH API/{x}text中提取什么信息，并按照什么格式返回给用户？&quot;,
  &quot;usage&quot;: {
    &quot;prompt_tokens&quot;: 27,
    &quot;prompt_unit_price&quot;: &quot;0&quot;,
    &quot;prompt_price_unit&quot;: &quot;0&quot;,
    &quot;prompt_price&quot;: &quot;0E-7&quot;,
    &quot;completion_tokens&quot;: 267,
    &quot;completion_unit_price&quot;: &quot;0&quot;,
    &quot;completion_price_unit&quot;: &quot;0&quot;,
    &quot;completion_price&quot;: &quot;0E-7&quot;,
    &quot;total_tokens&quot;: 294,
    &quot;total_price&quot;: &quot;0E-7&quot;,
    &quot;currency&quot;: &quot;USD&quot;,
    &quot;latency&quot;: 38.77509629799897
  },
  &quot;finish_reason&quot;: &quot;stop&quot;
}
```

> "text": "<details style=\"color:gray;background-color: #f8f8f8;padding: 8px;border-radius: 4px;\" open> <summary> Thinking... </summary>\n好，我需要帮助用户完成一个任务。用户提供了一段内容，看起来像是从搜索引擎API获取的信息，格式是`{x}WEB SEARCH API/{x}text`，还有`{x}start查询`和`{x}query`标签。\n\n首先，我要理解用户的需求是什么。他们可能是在进行某种自动化搜索或数据收集，使用了特定的API结构来组织信息。开始标记`{x}start query`和结束标记`{x}/query`表明他们可能在处理JSON格式的数据，其中包含搜索结果的元数据和内容。\n\n接下来，我需要考虑如何提取有用的信息。可能需要解析JSON中的各个字段，比如`search_id`、`web_source`、`text`等，并将这些信息以用户指定的方式返回给它们。例如，如果用户需要按特定顺序排列结果，或者只关注某些字段的内容。\n\n另外，我也要考虑如何处理可能出现的异常情况，比如数据缺失或格式问题。这可能涉及到添加错误处理机制，确保程序能够稳定运行并提供合理的反馈。\n\n最后，我应该建议用户具体的需求和预期输出，这样我能更好地帮助他们实现目标。同时，如果需要进一步的指导或代码示例，也可以提出请求。\n</details>\n\n好的，请告诉我您需要从`{x}WEB SEARCH API/{x}text`中提取什么信息，并按照什么格式返回给用户？",

输入内容（Search的返回结果）也没有发送给LLM。

从返回能看出，现在只返回了思考过程。并没有提取信息。

## （4）结束组件

![](img/500ecd8bb8482a29ccb4f30bfbd16785.png)

输入：



```
{
  &quot;&quot;: &quot;&lt;details style=\&quot;color:gray;background-color: #f8f8f8;padding: 8px;border-radius: 4px;\&quot; open&gt; &lt;summary&gt; Thinking... &lt;/summary&gt;\n好，我需要帮助用户完成一个任务。用户提供了一段内容，看起来像是从搜索引擎API获取的信息，格式是{x}WEB SEARCH API/{x}text，还有{x}start查询和{x}query标签。\n\n首先，我要理解用户的需求是什么。他们可能是在进行某种自动化搜索或数据收集，使用了特定的API结构来组织信息。开始标记{x}start query和结束标记{x}/query表明他们可能在处理JSON格式的数据，其中包含搜索结果的元数据和内容。\n\n接下来，我需要考虑如何提取有用的信息。可能需要解析JSON中的各个字段，比如search_id、web_source、text等，并将这些信息以用户指定的方式返回给它们。例如，如果用户需要按特定顺序排列结果，或者只关注某些字段的内容。\n\n另外，我也要考虑如何处理可能出现的异常情况，比如数据缺失或格式问题。这可能涉及到添加错误处理机制，确保程序能够稳定运行并提供合理的反馈。\n\n最后，我应该建议用户具体的需求和预期输出，这样我能更好地帮助他们实现目标。同时，如果需要进一步的指导或代码示例，也可以提出请求。\n&lt;/details&gt;\n\n好的，请告诉我您需要从{x}WEB SEARCH API/{x}text中提取什么信息，并按照什么格式返回给用户？&quot;
}
```

输出：

```
{
  &quot;&quot;: &quot;&lt;details style=\&quot;color:gray;background-color: #f8f8f8;padding: 8px;border-radius: 4px;\&quot; open&gt; &lt;summary&gt; Thinking... &lt;/summary&gt;\n好，我需要帮助用户完成一个任务。用户提供了一段内容，看起来像是从搜索引擎API获取的信息，格式是{x}WEB SEARCH API/{x}text，还有{x}start查询和{x}query标签。\n\n首先，我要理解用户的需求是什么。他们可能是在进行某种自动化搜索或数据收集，使用了特定的API结构来组织信息。开始标记{x}start query和结束标记{x}/query表明他们可能在处理JSON格式的数据，其中包含搜索结果的元数据和内容。\n\n接下来，我需要考虑如何提取有用的信息。可能需要解析JSON中的各个字段，比如search_id、web_source、text等，并将这些信息以用户指定的方式返回给它们。例如，如果用户需要按特定顺序排列结果，或者只关注某些字段的内容。\n\n另外，我也要考虑如何处理可能出现的异常情况，比如数据缺失或格式问题。这可能涉及到添加错误处理机制，确保程序能够稳定运行并提供合理的反馈。\n\n最后，我应该建议用户具体的需求和预期输出，这样我能更好地帮助他们实现目标。同时，如果需要进一步的指导或代码示例，也可以提出请求。\n&lt;/details&gt;\n\n好的，请告诉我您需要从{x}WEB SEARCH API/{x}text中提取什么信息，并按照什么格式返回给用户？&quot;
}
```

> { "": "<details style=\"color:gray;background-color: #f8f8f8;padding: 8px;border-radius: 4px;\" open> <summary> Thinking... </summary>\n好，我需要帮助用户完成一个任务。用户提供了一段内容，看起来像是从搜索引擎API获取的信息，格式是`{x}WEB SEARCH API/{x}text`，还有`{x}start查询`和`{x}query`标签。\n\n首先，我要理解用户的需求是什么。他们可能是在进行某种自动化搜索或数据收集，使用了特定的API结构来组织信息。开始标记`{x}start query`和结束标记`{x}/query`表明他们可能在处理JSON格式的数据，其中包含搜索结果的元数据和内容。\n\n接下来，我需要考虑如何提取有用的信息。可能需要解析JSON中的各个字段，比如`search_id`、`web_source`、`text`等，并将这些信息以用户指定的方式返回给它们。例如，如果用户需要按特定顺序排列结果，或者只关注某些字段的内容。\n\n另外，我也要考虑如何处理可能出现的异常情况，比如数据缺失或格式问题。这可能涉及到添加错误处理机制，确保程序能够稳定运行并提供合理的反馈。\n\n最后，我应该建议用户具体的需求和预期输出，这样我能更好地帮助他们实现目标。同时，如果需要进一步的指导或代码示例，也可以提出请求。\n</details>\n\n好的，请告诉我您需要从`{x}WEB SEARCH API/{x}text`中提取什么信息，并按照什么格式返回给用户？" }

这个就是前面的结果。由于前面错了，这个也错了。

### 4、修正运行

所以我们更新下LLM组件配置，增加上下文`Web Search API/text`

并将`SYSTEM`修改为：

```
根据搜索引擎检索到的内容：{x}WEB SEARCH API/{x}text，回答用户的提问开始/{x}query，上下文也可以添加本地知识库
```

![](img/7fa5695b3f17ce984926a2b16f5818fc.png)

重新运行：

![](img/f333334af8ca8078100bf313700666cd9.png)

输入“今天日期是多少”

![](img/854db4afa47b9568e657daf5d5f8121e.png)

好像还是不对。

那就是LLM的`SYSTEM`有问题：

![](img/c552c097235b5a39c6945855d4091254.png)

注意 变量不是手工输入的，而是点击（x）选出来的。

重新运行：输入“今天日期是多少”

![](img/9bf8ae4950524a5fdde3fd88afaffea2.png)

这回终于对了。

### 5、发布，正式运行：

![](img/0e4a5c30467cb3f1da6bccb8bbedc67f.png)

今天南京会下雨吗？

看起来还不错。

只是：

![](img/46cf809f26b1f5c8275f1757a3366d76.png)

`API`已经调用七次了哦。

天下没有免费的午餐！！！

![](img/2b1e3eb0d66affb32f680deed068a3d9.png)

超过100次就要收费了！

---

> 本文作者：@<https://datawhaler.feishu.cn/sync/LfRzdAExlsoykRbUZymc0Jrfngh>

### 欢迎投稿参与共建、下次微学习就学你的内容！

<https://datawhaler.feishu.cn/sync/CPNrdTvwgshasbb2UjecLzVinPe>
