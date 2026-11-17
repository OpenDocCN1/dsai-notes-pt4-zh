
# coze搭建周末去哪玩bot拆解

> 来源：[https://bluefocus.feishu.cn/docx/P0F2dh85voyVPcxCODtce7yGnqc](https://bluefocus.feishu.cn/docx/P0F2dh85voyVPcxCODtce7yGnqc)

bot效果

![](img/da1504f8631968c9519d3ce7c9475df2.png)

## 灵感乍现的瞬间

**Coze搭Bot**的玩法**很简单，难点**是「Bot的创意」，即「应用场景」，你打算用它来解决什么问题？

![](img/104c43bbc7d423e3afe53ad29edea545.png)

周三到了，相信很多人

**💡 咦，这灵感不就来了吗？用Coze搭个「提供周末好去处建议的Bot」，解决这个长期问题~**

## 随手建Bot

试下 **「仅通过提示词」，能否实现我们的需求**，新建Bot，随手写下功能介绍，「Dreamina」生成一个1:1的靓妹图标：

![](img/63567491097a61cc8b45c545a4131403.png)

简单写下提示词：

![](img/5454f30cbe58ecadf87b3df9cb9e8030.png)

让AI优化下提示词：

- 角色：你是一个周末活动推荐官，能根据用户提供的城市和区，推荐一些好去处，包括但不限于：景点、展览、音乐、戏剧、电影等。
- 技能：根据用户提供的信息，在周末活动推荐平台上搜索相关活动，并按类型整理好。从搜索结果中筛选出适合用户的活动，如：距离用户较近、评价较好、用户可能感兴趣的活动。将筛选出的活动推荐给用户，并提供活动的基本信息，如：时间、地点、费用等。
- 限制：只能推荐周末的活动。只接受用户提供的城市和区信息，不接受其他信息。

开场白也自动生成下：

![](img/7f7c97a323485cc99d6551321826cdff.png)

随便聊一句：

![](img/0f268e14bf41689e617c655e16f93af7.png)

😳 2333，一如既往的不靠谱，接着问下有什么展可以看：

![](img/e22d86741474ed785935b514b7155ab7.png)

😁 笑死，也不知道调的啥插件，单纯靠提示词来实现这个Bot明显不太行，还是的自己做下定制~

先捋下大概的期望：

> 发送「城市+区+类型(景点、展览、音乐、话剧等)」，Bot输出对应区域的「周末活动信息」，如：深圳市南山区-景点，Bot输出景点列表：【景点名称】景点地址；深圳市南山区-展览，Bot输出这周末的展览信息列表：【展名称】时间|票价|地点。
>
> ❞

**🤔 其实这种问答**，本质上就是「数据源的检索」而已，细分下我们这里的数据源，可以划分为两类：

- **「景点」**：很长一段时间不会改变，可以把这部分数据看成「不变」的，比如：深圳湾公园这个景点几十年都不会变。
- **「周末同城活动」**：这部分则是**「动态变化」**的，需要**「实时获取」**，比如：这周六有阿猫的演唱会，下周六有阿狗的脱口秀。

🤔 接着是写Bot时「数据源的获取思路」：

- 如果自己有相关的数据源，可以上传下「知识库」。
- 没有的话，可以看下「官方/商店是否提供了相关插件」，输入参数和输出结果是否能满足我们的诉求。
- 如果都没有或者都不满足，就需要自己捣鼓了，去哪里搞？无非这三个渠道：「开源/免费接口」、「付费接口」、「爬虫获取」（扣子里可以通过「自定义插件」或「代码节点中使用`request_async`库」来模拟请求获取数据。当然，如果想做得隐秘一些，可以将爬虫脚本部署到服务器上，把数据存到数据库中，暴露一个数据查询的API，写个Coze插件供Bot调用）。

😏 思路有了，接着就该捣鼓捣鼓，如何获得中意的数据源了~

## 1. 景点信息源获取

新建工作流，简单设置下形成和描述：

![](img/03dbf86b5cc353674ead8a9f63d313b5.png)

### 3.1. 官方插件工具能否满足需求？

在上节插件工具的汇总表搜下**「地图」**：

![](img/cb747d935193b5b3ea7e40c84cc5bb49.png)

试试 `searchLocation` 这个插件工具，打开看看输入参数：

![](img/605ea692e4e5fed905d25e11f8a92bfe.png)

😳 咦，没提供「分页参数」？该不会只有10条数据吧？工作流中添加下这个插件，试运行输入「深圳南山」看看：

![](img/c0d45ce6809263b09070bc4f7541db63.png)

果然，只返回了10条数据，不过是我们心仪的数据（景点名称+地址），只是数据量有点少，得想办法搞到更多的数据。

> 🤔️ 其中一个思路就是：让「地址的粒度更细些」，「城市-区」往下是「街道/乡镇级别」，先通过大模型获取所有某个区下所有的街道/乡镇，然后`searchLocation`插件执行批处理，最后写代码做下整合。
>
> ❞

#### 3.1.1. 大模型结点获得所有街道/乡镇

工作流中添加大模型结点，添加下述提示词：

```
提取{{input}}中的城市、区，然后检索<城市区>由哪些街道/乡镇组成，要全，不要遗漏任何一个。

约束：
严格按照这样的json格式输出：
{
  "city": <城市，需要以市结尾>,
  "district":<区，需要以区结尾>,
  "streeOrTown": <街道/乡镇列表>
}
```

试运行，输入**「珠海拱北」**试试：

![](img/cee39177902e6f560ed17c95b49990f1.png)

😆 可以，难得云雀大模型正确理解了我们的意图，并按照我们预设的约束进行回复。

#### 3.1.2. 选择器节点 + `searchLocation`批处理

上面的返回的街道/乡镇字段数据有14条，而批处理最多执行10次，所以需要「判断下结果长度」，超过10个，「拆成调用两个`searchLocation`工具」。这里做下简化处理，只要数据超过2条，就拆成两组，理想中的执行流程：

![](img/99c6861da0c1bb9221f05e4ce9888af6.png)

照着流程堆节点，但是运行结果并不符合预期，先是选择器节点的判定：

![](img/92211d2e275a6f8c1c056d8f43f75e56.png)

明明「数组长度>0」，还是走的`else`？？？没懂这里的长度具体指的啥...行吧，只能把选择条件为改成了「不为空」，测了下能跑通，就先这样吧，然后补全下节点：

![](img/4566333f6a2da009faf638bbbfb94d97.png)

看下结果节点的输入数据：

![](img/48fe108c9cc58439e228dc12be9ddb42.png)

行吧，数据都拿到了，前面再加个代码节点，做下数据合并去重，并格式化输出：

```
async def main(args: Args) -> Output:
    params = args.params
    if_condition_result1 = params["ifConditionResult1"]
    if_condition_result2 = params["ifConditionResult1"]
    else_condition_result1 = params["elseConditionResult"]
    利用字典key的可以去重，遍历三个结果合并
    result_dict = {}
    if if_condition_result1 and len(if_condition_result1) > 0:
        for condition_result in if_condition_result1:
            for place in condition_result["places"]:
                result_dict[place["name"]] = {"name": place["name"], "address": place["address"]}
    if if_condition_result2 and len(if_condition_result2) > 0:
        for condition_result in if_condition_result2:
            for place in condition_result["places"]:
                result_dict[place["name"]] = {"name": place["name"], "address": place["address"]}
    if else_condition_result1 and len(else_condition_result1) > 0:
        for condition_result in else_condition_result1:
            for place in condition_result["places"]:
                result_dict[place["name"]] = {"name": place["name"], "address": place["address"]}     
    ret: Output = {
        "result": list(result_dict.values())
    }
    return ret
```

运行后**「选择器节点」**又抽风了，我真的是裂开了🙃...

![](img/288117032483967a5fce55811f25bee4.png)

算了，放弃选择器节点，默认使用两个`searchLocation`插件，反正做了去重，不用的担心数据重复问题，改下代码：

```
import json

async def main(args: Args) -> Output:
    params = args.params
    input_json = json.loads(params["input"])
    stree_or_town_list = input_json['streeOrTown']
    list_len = int(len(stree_or_town_list))
    first_stree_or_town_List = []
    second_stree_or_town_List = []
    如果长度大于2，拆分成两组
    if list_len > 2:
        center_pos = int(list_len / 2)
        first_stree_or_town_List = stree_or_town_list[:center_pos]
        second_stree_or_town_List = stree_or_town_list[center_pos:]
    只有一条的话，两个列表都进行赋值
    else:
        first_stree_or_town_List = stree_or_town_list
        second_stree_or_town_List = first_stree_or_town_List
    ret: Output = {
        'city': input_json['city'],
        'district': input_json['district'],
        'cityDistrict': input_json['city'] + input_json['district'],
        'firstStreeOrTownList': first_stree_or_town_List,
        'secondStreeOrTownList': second_stree_or_town_List,
    }
    return ret
```

运行输出结果：

![](img/9e1c08d6e6eb46e705bd8b6bc8ad9b55.png)

接着回到**「编排-人设与回复逻辑」**，调用下工作流，然后随便发送：城市+区：

![](img/a4ed90b614db7f2368ac6f2aae4067d5.png)

Bot就会给我们回复对应城市区的所有景点信息啦。不过昨天早上是好的，下午想再试试，地图插件却一直报错，批处理、单词调用，人设与回复逻辑处通过提示词自动调用，都是报错：

![](img/0a5300ad3ebb2d04c92cb5af15ee6497.png)

😒 感觉跟昨天下午平台允许提交插件有关，唉，还是做下兜底，自己采集下数据，「搞个城市景点的知识库」，未雨绸缪嘛~

### 3.2. 自己采集数据弄知识库

😏 网上看了一圈没有提供景点查询的相关接口，那就只能自己写爬虫采集咯，去哪采？答：「地图站点/软件」。以某地图为例，搜下「深圳市南山区景点」，可以看到相关的景点，做了分页，一页10个：

![](img/1c5d1361abfa5b60ddff4831de2b0c36.png)

F12抓包，搜索「南山公园」定位到加载数据XHR，URL中包含了下述字符串：

```
newmap=1&reqflag=pcmap&biz=1&from=webmap&da_par=direct&pcevaname=pc4.1&qt=con&from=webmap
```

稍微改下上节写的油猴脚本，手动点下一页，直到最后一页，爬取后的json文件：

![](img/558eb5ccaa935c434976f6525cc889b0.png)

用Python写下处理脚本：过滤非景点或公园的数据，提取下景点名称、标签和地址，做下去重，最后保存为「csv文件」，具体代码如下：
