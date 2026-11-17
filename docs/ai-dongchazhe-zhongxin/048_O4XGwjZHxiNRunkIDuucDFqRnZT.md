# coze搭建周末去哪玩bot拆解

> 来源：[https://bluefocus.feishu.cn/docx/P0F2dh85voyVPcxCODtce7yGnqc](https://bluefocus.feishu.cn/docx/P0F2dh85voyVPcxCODtce7yGnqc)

bot效果

![](img/da1504f8631968c9519d3ce7c9475df2.png)

# 灵感乍现的瞬间

**Coze搭Bot**的玩法**很简单，难点**是 「Bot的创意」，即 「应用场景」，你打算用它来解决什么问题？

![](img/104c43bbc7d423e3afe53ad29edea545.png)

周三到了，相信很多人

**💡 咦，这灵感不就来了吗？用C**oze搭个 「提供周末好去处建议的Bot」，解决这个长期问题~

# 随手建Bot

试下 **「仅通过提示词」****，能否实现我们的需求**，新建Bot，随手写下功能介绍，「Dreamina」 生成一个1:1的靓妹图标：

![](img/63567491097a61cc8b45c545a4131403.png)

简单写下提示词：

![](img/5454f30cbe58ecadf87b3df9cb9e8030.png)

让AI优化下提示词：

+   角色
你是一个周末活动推荐官，能根据用户提供的城市和区，推荐一些好去处，包括但不限于：景点、展览、音乐、戏剧、电影等。

技能
根据用户提供的信息，在周末活动推荐平台上搜索相关活动，并按类型整理好。
从搜索结果中筛选出适合用户的活动，如：距离用户较近、评价较好、用户可能感兴趣的活动。
将筛选出的活动推荐给用户，并提供活动的基本信息，如：时间、地点、费用等。

限制
只能推荐周末的活动。
只接受用户提供的城市和区信息，不接受其他信息。

开场白也自动生成下：

![](img/7f7c97a323485cc99d6551321826cdff.png)

随便聊一句：

![](img/0f268e14bf41689e617c655e16f93af7.png)

😳 2333，一如既往的不靠谱，接着问下有什么展可以看：

![](img/e22d86741474ed785935b514b7155ab7.png)

😁 笑死，也不知道调的啥插件，单纯靠提示词来实现这个Bot明显不太行，还是的自己做下定制~

先捋下大概的期望：

> 发送 「城市+区+类型(景点、展**览、音乐、话剧等**)」 ，Bot输出对应区域的 「周末活动信息」，如：深圳市南山区-景点，Bot输出景点列表：【景点名称】景点地址；深圳市南山区-展览，Bot输出这周末的展览信息列表：【展名称】时间|票价|地点。

> ❞

**🤔 其实这种问答**，本质上就是 「数据源的检索」 而已，细分下我们这里的数据源，可以划分为两类：

+   **「景点」****：很长一**段时间不会改变，可以把这部分数据看成 「不变」 的，比如：深圳湾公园这个景点几十年都不会变。

+   **「周末同城活动」**：这部分则是 **「动态变化」** 的，需要 **「实时获取」**，比如：这周六有阿猫的演唱会，下周六有阿狗的脱口秀。

🤔 接着是写Bot时 「**数据源的获取思路」：**

+   **如果自己有**相关的数据源，可以上传下 「知识库」。

+   没有的话，可以看下 「官方/商店是否提供了相关插件」，输入参数和输出结果是否能满足我们的诉求。

+   **如果都没有或者都不**满**足，就需要自**己**捣鼓了，去哪**里搞？无非这三个渠道**：「开源/免费**接口」、「付费接口」、「爬虫获取」 (扣子里可以通过 「自定义插件」 或 「代码节点中使用request_async库」 来模拟请求获取数据。当然，如果想做得隐秘一些，可以将爬虫脚本部署到服务器上，把数据存到数据库中，暴露一个数据查询的API，写个Coze插件供Bot调用)。

😏 思路有了，接着就该捣鼓捣鼓，如何获得中意的数据源了~

1.  景点信息源获取

新建工作流，简单设置下形成和描述：

![](img/03dbf86b5cc353674ead8a9f63d313b5.png)

# 3.1. 官方插件工具能否满足需求？

在上节插件工具的汇总表搜下 "**「地图」**"：

![](img/cb747d935193b5b3ea7e40c84cc5bb49.png)

试试 「searchLocation」 这个插件工具，打开看看输入参数：

![](img/605ea692e4e5fed905d25e11f8a92bfe.png)

😳 咦，没提供 「**分页参数」？****该不会只有1**0条数据吧？工作流中添加下这个插件，试运行输入 "「深圳南山」" 看看：

![](img/c0d45ce6809263b09070bc4f7541db63.png)

果然，只返回了10条数据，不过是我们心仪的数据 (景点名称+地址)，只是数据量有点少，得想办法搞到更多的数据。

>

> 🤔️ 其中一个思路就是：让 「**地址的粒度更细些」，**「**城市-区」 **往下是 「**街道/乡镇级别」，**先通过大模型获取所有某个区下所有的街道/乡镇，然后searchLocation插件执行批处理，最后写代码做下整合。

> ❞

## 3.1.1. 大模型结点获得所有街道/乡镇

工作流中添加大模型结点，添加下述提示词：

```
提取{{input}}中的城市、区，然后检索&lt;城市区&gt;由哪些街道/乡镇组成，要全，不要遗漏任何一个。

约束：
严格按照这样的json格式输出：
{
  &quot;city&quot;: &lt;城市，需要以市结尾&gt;,
  &quot;district&quot;:&lt;区，需要以区结尾&gt;,
  &quot;streeOrTown&quot;: &lt;街道/乡镇列表&gt;
}
```

试运行，输入 **「"珠海拱北"」** 试试：

![](img/cee39177902e6f560ed17c95b49990f1.png)

😆 可以，难得云雀大模型正确理解了我们的意图，并按照我们预设的约束进行回复。

## **3.1.2. 选择器节点 +** 「searchLocation」批处理

上面的返回的街道/乡镇字段数据有1**4条，而批处理最多**执行10次，所以需要 「判断下结果长度」，超过10个，「拆成调用两个searchLocation工具」。这里做下简化处理，只要数据超过2条，就拆成两组，理想中的执行流程：

![](img/99c6861da0c1bb9221f05e4ce9888af6.png)

照着流程堆节点，但是运行结果并不符合预期，先是选择器节点的判定：

![](img/92211d2e275a6f8c1c056d8f43f75e56.png)

明明 **「数组长度>0」**，还是走的else？？？没懂这里的长度具体指的啥**... 行**吧，只能把选择条件为改成了 「不为空」，测了下能跑通，就先这样吧，然后补全下节点：

![](img/4566333f6a2da009faf638bbbfb94d97.png)

看下结果节点的输入数据：

![](img/48fe108c9cc58439e228dc12be9ddb42.png)

行吧，数据都拿到了，前面再加个代码节点，做下数据合并去重，并格式化输出：

```
async def main(args: Args) -&gt; Output:
    params = args.params
    if_condition_result1 = params[&quot;ifConditionResult1&quot;]
    if_condition_result2 = params[&quot;ifConditionResult1&quot;]
    else_condition_result1 = params[&quot;elseConditionResult&quot;]
    利用字典key的可以去重，遍历三个结果合并
    result_dict = {}
    if if_condition_result1 and len(if_condition_result1) &gt; 0:
        for condition_result in if_condition_result1:
            for place in condition_result[&quot;places&quot;]:
                result_dict[place[&quot;name&quot;]] = {&quot;name&quot;: place[&quot;name&quot;], &quot;address&quot;: place[&quot;address&quot;]}
    if if_condition_result2 and len(if_condition_result2) &gt; 0:
        for condition_result in if_condition_result2:
            for place in condition_result[&quot;places&quot;]:
                result_dict[place[&quot;name&quot;]] = {&quot;name&quot;: place[&quot;name&quot;], &quot;address&quot;: place[&quot;address&quot;]}
    if else_condition_result1 and len(else_condition_result1) &gt; 0:
        for condition_result in else_condition_result1:
            for place in condition_result[&quot;places&quot;]:
                result_dict[place[&quot;name&quot;]] = {&quot;name&quot;: place[&quot;name&quot;], &quot;address&quot;: place[&quot;address&quot;]}     
    ret: Output = {
        &quot;result&quot;: list(result_dict.values())
    }
    return ret
```

运行后 **「选择器节点」** 又抽风了，我真的是裂开了🙃...

![](img/288117032483967a5fce55811f25bee4.png)

算了，放弃选择器节点，默认使用两个searchLocation插件，反正做了去重，不用的担心数据重复问题，改下代码：

```
import json

async def main(args: Args) -&gt; Output:
    params = args.params
    input_json = json.loads(params[&quot;input&quot;])
    stree_or_town_list = input_json[&#x27;streeOrTown&#x27;]
    list_len = int(len(stree_or_town_list))
    first_stree_or_town_List = []
    second_stree_or_town_List = []
    如果长度大于2，拆分成两组
    if list_len &gt; 2:
        center_pos = int(list_len /2)
        first_stree_or_town_List = stree_or_town_list[:center_pos]
        second_stree_or_town_List = stree_or_town_list[center_pos:]
    只有一条的话，两个列表都进行赋值
    else:
        first_stree_or_town_List = stree_or_town_list
        second_stree_or_town_List = first_stree_or_town_List
    ret: Output = {
        &#x27;city&#x27;: input_json[&#x27;city&#x27;],
        &#x27;district&#x27;: input_json[&#x27;district&#x27;],
        &#x27;cityDistrict&#x27;: input_json[&#x27;city&#x27;] + input_json[&#x27;district&#x27;],
        &#x27;firstStreeOrTownList&#x27;: first_stree_or_town_List,
        &#x27;secondStreeOrTownList&#x27;: second_stree_or_town_List,
    }
    return ret
```

运行输出结果：

![](img/9e1c08d6e6eb46e705bd8b6bc8ad9b55.png)

接着回到 **「编排-人设与回复逻辑」**，调用下工作流，然后随便发送：城市+区：

![](img/a4ed90b614db7f2368ac6f2aae4067d5.png)

Bot就会给我们回复对应城市区的所有景点信息啦。不过昨天早上是好的，下午想再试试，地图插件却一直报错，批处理、单词调用，人设与回复逻辑处通过提示词自动调用，都是报错：

![](img/0a5300ad3ebb2d04c92cb5af15ee6497.png)

😒 感觉跟昨天下午平台允许提交插件有**关，唉，还是做下兜底，自**己采集下数据，「搞个城市景点的知识库」，未雨绸缪嘛~

# 3.2. 自己采集数据弄知识库

😏 网上看了一圈没有提供景点查询的相关接口，**那就只能自己写爬虫**采集咯，去哪采？答：「**地图站点/软件」。以**某地图为例，搜下"「深圳市南山区景点」"，可以看到相关的景点，做了分页，一页10个：

![](img/1c5d1361abfa5b60ddff4831de2b0c36.png)

F12抓包，搜索 "**「南山公园」**" 定位到加载数据XHR，URL中包含了下述字符串：

>

```
newmap=1&amp;reqflag=pcmap&amp;biz=1&amp;from=webmap&amp;da_par=direct&amp;pcevaname=pc4.1&amp;qt=con&amp;from=webmap
```

稍微改下上节写的油猴脚本，手动点下一页，直到最后一页，爬取后的json文件：

![](img/558eb5ccaa935c434976f6525cc889b0.png)

用Python写下处理脚本：过滤非景点或公园的数据，提取下景**点名称、标签和**地址，做下去重，最后保存为「csv文件」，具体代码如下：

```
提取城市_区所有景点信息列表
def fetch_attractions_info_list(city_county):
    json_dir = &quot;{}{}{}{}{}&quot;.format(os.getcwd(), os.sep, &quot;baidu_jd&quot;, os.sep, city_county, os.sep)
    attractions_info_dict = {}  地址为key，过滤重复景点，如（深圳野生动物园-豪猪、深圳野生动物园-北极狐）
    json_file_list = search_all_file(json_dir, &quot;.json&quot;)
    for json_file in json_file_list:
        data_list = json.loads(read_file_text_content(json_file))[&#x27;content&#x27;]
        for data in data_list:
            attractions_name = data[&#x27;name&#x27;]  景点名称
            attractions_tag = data[&#x27;std_tag&#x27;]  景点标签
            attractions_addr = data[&#x27;addr&#x27;]  景点地址
            过滤垃圾数据
            if attractions_tag:
                if &quot;景点&quot; in attractions_tag or &quot;公园&quot; in attractions_tag:
                    景点记录
                    attractions_dict = {&quot;景点名称&quot;: attractions_name, &quot;景点标签&quot;: attractions_tag,
                                        &quot;景点地址&quot;: attractions_addr}
                    if attractions_addr in attractions_info_dict:
                        cur_attractions_name = attractions_info_dict[attractions_addr][&#x27;景点名称&#x27;]
                        取景点名称较短的那一个
                        if len(attractions_name) &lt; len(cur_attractions_name):
                            attractions_info_dict[attractions_addr] = attractions_dict
                    else:
                        attractions_info_dict[attractions_addr] = attractions_dict
        保存为csv文件
        with open(os.path.join(output_dir, &quot;{}.csv&quot;.format(city_county)), &#x27;w+&#x27;, newline=&#x27;&#x27;, encoding=&#x27;utf-8&#x27;) as f:
            writer = csv.DictWriter(f, fieldnames=[&#x27;景点名称&#x27;, &quot;景点标签&quot;, &quot;景点地址&quot;])
            writer.writeheader()
            for item in list(attractions_info_dict.values()):
                writer.writerow(item)
```

**其它城市-区也**是如法炮制，接着打开Coze的知识库 → 「新建知识库」：

![](img/a15da96c038ba2dcf3b20fdb00b56c13.png)

然后上传每个csv，依次点击：**「新增单元」** → **「表格格式」** → **「本地文档」** → **「上传上面生成的csv文件」** → **「下一步」** → **「选择景点地址作为索引」** (用于提高知识库召回的准确率)：

![](img/a5792d05b707fa135f28e1ce083f5ef5.png)

、

**「下一步」** → **「等待数据处理完成」**，然后可以打开看下生成的单元了：

![](img/a36b9adc71cd40bcf93c093af9128365.png)

知识库准备就绪，接着新建一个工作流 (search_city_**attract**ion_info)，添加 「知识库节点」：

![](img/7b6ceb4c77c761c84941dc1af700a139.png)

知识库节点最多召回10条数据，这点数据量肯定是不够的，而且这个节点不支持批处理。那就靠数量来凑吧，直接CV五个知识库节点，然后试下加上分页查询，连接好的节点图如下：

![](img/784bd401c1621d1586500442dfcb3bff.png)

前面的代码节点用于生成分页Query字符串：

```
async def main(args: Args) -&gt; Output:
    params = args.params
    city_district = params[&#x27;cityDistrict&#x27;]
    ret: Output = {
        &#x27;firstQuery&#x27;: &#x27;{}的第1-10条数据&#x27;.format(city_district),
        &#x27;SecondQuery&#x27;: &#x27;{}的第11-20条数据&#x27;.format(city_district),
        &#x27;ThirdQuery&#x27;: &#x27;{}的第21-30条数据&#x27;.format(city_district),
        &#x27;ForthQuery&#x27;: &#x27;{}的第31-40条数据&#x27;.format(city_district),
        &#x27;FifthQuery&#x27;: &#x27;{}的第41-50条数据&#x27;.format(city_district)
    }
    return ret
```

后面的代码节点用于景点数据合并：

```
import json

合并列表的函数
def merge_list(input_param, origin_set):
    for data in input_param:
        data_json = json.loads(data[&#x27;output&#x27;])
        origin_set.add(&quot;-【{}】{}&quot;.format(data_json[&#x27;景点名称&#x27;], data_json[&#x27;景点地址&#x27;]))

async def main(args: Args) -&gt; Output:
    params = args.params
    sum_set = set()
    result = &#x27;&#x27;
    merge_list(params[&quot;input1&quot;], sum_set)
    merge_list(params[&quot;input2&quot;], sum_set)
    merge_list(params[&quot;input3&quot;], sum_set)
    merge_list(params[&quot;input4&quot;], sum_set)
    merge_list(params[&quot;input5&quot;], sum_set)
    for data in sum_set:
        result += data  + &quot;\n&quot;
    ret: Output = {
        &quot;result&quot;: result,
    }
    return ret
```

运行输出结果如下：

![](img/318349436acbf27e5a813bdd26ec11b9.png)

😳 **怎么才这点数**据？知识库里深圳南山区的景点数据可是有68条的，看了下数据合并代码节点的输入参数，发现很多数据是重复的，看来是知识库不支持分页Query。em... 试下给每条记录添加一个 「景点序号」 的列吧。配置表结构只支持删除列，不支持新增列，直接删掉重新导下csv吧，选择景点序号作为索引：

![](img/04d810a8e6909f81972873e10441a9cb.png)

改了下Query生成的字符串：

![](img/a9f8d1cdd02a0c57a45da7f8b17cce20.png)

😶 结果并没什么卵用，而且一个"深圳市_南山区"的数据都没命中，感觉也可能跟缓存有关...

![](img/5ba02ed359971983228e89488f5eab61.png)

😶 算了，直接使出最后一招吧，把景点数据输出为txt文件：

![](img/3b4c3788775125707f0a80ce838b69b9.png)

点击 **「新增单元」**，选中 **「文本格式-本地文档」**：

![](img/cc414dbb833ed0b5748392987c5bc3af.png)

以为数据结构比较简单，**「分段设置」** 这里直接选 **「自动分段与清洗」** 就好了：

![](img/b1c0e754e3c7161298ab55735a342b78.png)

**确定后等****它处理完**，再次打开我们新建的 「单元」，可以看到数据被自动拆分成3个 「分段」：

![](img/0426c4e72fbe46d74cc878d676a9730d.png)

吼，那就不需要 **「5个知识库节点」** 了，一个就够了，后面添加一个代码节点用于输出结果格式化：

![](img/2020043cd9049032083b9ee58dbe4184.png)

格式化代码如下：

```
async def main(args: Args) -&gt; Output:
    params = args.params
    input_param = params[&quot;inputArray&quot;]
    city_district = params[&quot;cityDistrict&quot;]
    result_str = &quot;【{}】有如下景点：\n&quot;.format(city_district)
    for param in input_param:
        info_list = param[&quot;output&quot;].split(&quot;\n&quot;)
        for info in info_list:
            result_str += &quot;- {}\n&quot;.format(info)
    ret: Output = {
        &quot;result_str&quot;: result_str,
    }
    return ret
```

试运行输入：**「深圳南山」**，输出结果：

![](img/b40bfab245aedc61a879fda78ab6ab6f.png)

发布下工作流，然后 **「人设与回复逻辑」** 调用下，输出结果如下：

![](img/419be5fda48e52ad0b626010e982a4d7.png)

输出结果和官方插件工具返回的基本一致，当然，速度会快很多，毕竟是基于已有数据的检索。行吧，景点信息源获取这块就折腾到这，接着搞下周末同城数据的获取。

1.  周末同城活动数据源获取

**😏 同样没找**着接口，那就写爬虫爬吧，以 「XX同城」 为例，F12打开抓包，请求页面发现数据直接返回了：

![](img/a2821c3d0f0ad6a3f401277bf53c48c2.png)

本**地写了段Pyt**hon代码，设置了User-Agent请求头，请求url，发现页面代码都有返回。行吧，试下创建 「自定义插件」 请求能否行得通，然而在调试时就报错了：

![](img/8a87fec6d8cabf1dd0ff75336b355f7f.png)

em... 貌似插件 「只支持json格式的返回数据」，那就只能在代码节点编写代码来模拟请求了，新建下工作流：

![](img/14f4b149c31ba124f19a5efff7ad9886.png)

新建一个代码结点，代码模拟请求下：

>

https://www.xxx.com/location/shenzhen/events/weekend-exhibition

❞

运行后输出结果如下：

![](img/993a34b072bd1fb257b080be58782c74.png)

复制保存下响应数据，本地写下 「提取活动名称、时间、地点、费用的正则」，提取一波数据：

```
import re

from util.file_util import read_file_text_content

activity_pattern = re.compile(
    r&#x27;temprop=&quot;summary&quot;&gt;(.*?)&lt;.*?时间：&lt;/span&gt;(.*?)&lt;.*?&lt;li title=&quot;(.*?)&quot;&gt;.*?地点：&lt;.*?费用：&lt;/span&gt;(.*?)&lt;/strong&gt;&#x27;, re.S)

if name == &#x27;__main__&#x27;:
    content = read_file_text_content(&quot;test.html&quot;)
    match_results = activity_pattern.findall(content)
    result_str = &quot;&quot;
    for result in match_results:
        activity_name = result[0].replace(&quot;\n&quot;, &quot;&quot;).strip() if result[0] else &quot;暂无数据&quot;
        activity_time = result[1].replace(&quot;\n&quot;, &quot;&quot;).strip() if result[1] else &quot;暂无数据&quot;
        activity_address = result[2].replace(&quot;\n&quot;, &quot;&quot;).strip() if result[3] else &quot;暂无数据&quot;
        activity_cost = result[3].replace(&quot;\n&quot;, &quot;&quot;).strip().replace(&quot;&lt;strong&gt;&quot;, &quot;&quot;) if result[3] else &quot;暂无数据&quot;
        result_str += &quot;-【{}】| {} | {} | {}\n&quot;.format(activity_name, activity_cost, activity_time, activity_address)
    print(result_str)
```

运行输出结果如下：

![](img/a962ddce0e1c00142298761a78037a69.png)

行吧，数据能提取到，接着就是匹配请求参数，拼接url了，比较简单，直接给出爬取代码：

```
import json
import requests_async
import re
import time

提取活动信息的正则
activity_pattern = re.compile(
    r&#x27;temprop=&quot;summary&quot;&gt;(.*?)&lt;.*?时间：&lt;/span&gt;(.*?)&lt;.*?&lt;li title=&quot;(.*?)&quot;&gt;.*?地点：&lt;.*?费用：&lt;/span&gt;(.*?)&lt;/strong&gt;&#x27;, re.S)


城市和区的Bean
class City:
    def init(self, name_cn, name_req_param, district_dict):
        self.name_cn = name_cn
        self.name_req_param = name_req_param
        self.district_dict = district_dict


发起请求
async def send_request(url):
    headers = {
        &quot;User-Agent&quot;: &quot;Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/122.0.0.0 Safari/537.36&quot;,
        &quot;Host&quot;: &quot;www.douban.com&quot;,
        &quot;Refer&quot;: url
    }
    请求响应结果
    response_data = await requests_async.get(url, headers=headers)
    提取响应结果中的活动信息
    match_results = activity_pattern.findall(response_data.text)
    result_str = &quot;&quot;
    for result in match_results:
        activity_name = result[0].replace(&quot;\n&quot;, &quot;&quot;).strip() if result[0] else &quot;暂无数据&quot;
        activity_time = result[1].replace(&quot;\n&quot;, &quot;&quot;).strip() if result[1] else &quot;暂无数据&quot;
        activity_address = result[2].replace(&quot;\n&quot;, &quot;&quot;).strip() if result[3] else &quot;暂无数据&quot;
        activity_cost = result[3].replace(&quot;\n&quot;, &quot;&quot;).strip().replace(&quot;&lt;strong&gt;&quot;, &quot;&quot;) if result[3] else &quot;暂无数据&quot;
        result_str += &quot;-【{}】| {} | {} | {}\n&quot;.format(activity_name, activity_cost, activity_time, activity_address)
    return result_str


async def main(args: Args) -&gt; Output:
    城市和区的请求参数
    city_list = [
        City(&quot;深圳市&quot;, &quot;shenzhen&quot;, {
            &quot;罗湖区&quot;: 130288, &quot;福田区&quot;: 130289, &quot;南山区&quot;: 130290, &quot;宝安区&quot;: 130291, &quot;龙岗区&quot;: 130292,
            &quot;盐田区&quot;: 130293, &quot;坪山区&quot;: 131682, &quot;龙华区&quot;: 131683, &quot;光明区&quot;: 131691,
        }), City(&quot;广州市&quot;, &quot;guangzhou&quot;, {
            &quot;从化&quot;: 130277, &quot;荔湾区&quot;: 130266, &quot;越秀区&quot;: 130267, &quot;海珠区&quot;: 130268, &quot;天河区&quot;: 130269, &quot;白云区&quot;: 130270,
            &quot;黄埔区&quot;: 130271, &quot;番禺区&quot;: 130272, &quot;花都区&quot;: 130273, &quot;南沙区&quot;: 130274, &quot;萝岗区&quot;: 130275, &quot;增城区&quot;: 130276
        }), City(&quot;上海市&quot;, &quot;shanghai&quot;, {
            &quot;黄浦区&quot;: 129242, &quot;徐汇区&quot;: 129244, &quot;长宁区&quot;: 129245, &quot;静安区&quot;: 129246, &quot;普陀区&quot;: 129247,
            &quot;闸北区&quot;: 129248, &quot;虹口区&quot;: 129249, &quot;杨浦区&quot;: 129250, &quot;闵行区&quot;: 129251, &quot;宝山区&quot;: 129252,
            &quot;嘉定区&quot;: 129253, &quot;浦东新区&quot;: 129254, &quot;金山区&quot;: 129255, &quot;松江区&quot;: 129256, &quot;青浦区&quot;: 129257,
            &quot;奉贤区&quot;: 129259, &quot;崇明县&quot;: 129260
        }), City(&quot;北京市&quot;, &quot;beijing&quot;, {
            &quot;东城区&quot;: 128519, &quot;西城区&quot;: 128520, &quot;朝阳区&quot;: 128523, &quot;丰台区&quot;: 128524, &quot;石景山区&quot;: 128525,
            &quot;海淀区&quot;: 128526, &quot;门头沟区&quot;: 128527, &quot;房山区&quot;: 128528, &quot;通州区&quot;: 128529, &quot;顺义区&quot;: 128530,
            &quot;昌平区&quot;: 128531, &quot;大兴区&quot;: 128532, &quot;怀柔区&quot;: 128533, &quot;平谷区&quot;: 128534, &quot;密云县&quot;: 128535, &quot;延庆县&quot;: 128536
        })
    ]
    活动类型参数
    category_dict = {
        &quot;音乐&quot;: &quot;music&quot;, &quot;戏剧&quot;: &quot;drama&quot;, &quot;讲座&quot;: &quot;salon&quot;, &quot;聚会&quot;: &quot;party&quot;, &quot;电影&quot;: &quot;film&quot;,
        &quot;展览&quot;: &quot;exhibition&quot;, &quot;运动&quot;: &quot;sports&quot;, &quot;公益&quot;: &quot;commonweal&quot;, &quot;旅行&quot;: &quot;travel&quot;,
        &quot;赛事&quot;: &quot;competition&quot;, &quot;课程&quot;: &quot;course&quot;, &quot;亲子&quot;: &quot;kids&quot;, &quot;其它&quot;: &quot;others&quot;
    }
    params = args.params
    input_json = json.loads(params[&quot;input&quot;])
    request_url = &quot;https://www.xxx.com/location/{}/events/weekend&quot;
    for city in city_list:
        if city.name_cn == input_json[&#x27;city&#x27;]:
            拼接城市参数
            request_url = request_url.format(city.name_req_param)
            拼接活动类型参数
            if input_json[&#x27;category&#x27;] is not None and len(input_json[&#x27;category&#x27;]) &gt; 0:
                categoty_req_param = category_dict[input_json[&#x27;category&#x27;]]
                if category_dict:
                    categoty_req_param = categoty_req_param
                else:
                    categoty_req_param = &quot;all&quot;
            else:
                categoty_req_param = &quot;all&quot;
            request_url += &quot;-{}&quot;.format(categoty_req_param)
            拼接区
            district = city.district_dict.get(input_json[&#x27;district&#x27;])
            if district:
                request_url += &quot;-{}&quot;.format(district)

    result_str = &quot;为您检索到【{}-{}-{}】的周末活动信息：\n&quot;.format(
        input_json[&#x27;city&#x27;], input_json[&#x27;district&#x27;],
        input_json[&#x27;category&#x27;] if len(input_json[&#x27;category&#x27;]) &gt; 0 else &quot;全部&quot;
    )
    请求三次接口获取前三页数据，休眠0.5秒防封
    r1 = await send_request(request_url)
    time.sleep(0.5)
    r2 = await send_request(request_url + &quot;?start=10&quot;)
    time.sleep(0.5)
    r3 = await send_request(request_url + &quot;?start=20&quot;)
    result_str += r1
    result_str += r2
    result_str += r3
    ret: Output = {
        &quot;result&quot;: result_str,
        &quot;request_url&quot;: request_url,
        &#x27;r1&#x27;: r1,
        &#x27;r2&#x27;: r2,
        &#x27;r3&#x27;: r3,
    }
    return ret
```

这里设置r1、r2、r3只是想看下请求的数据是否正常，实际使用可以干掉，测试代码那里粘贴下输入参数：

```
{
  &quot;input&quot;: &quot;{&quot;category&quot;:&quot;&quot;,&quot;city&quot;:&quot;深圳市&quot;,&quot;district&quot;:&quot;福田区&quot;}&quot;
}
```

运行后，看下输出结果：

![](img/e2244c4c0c61241e44d493b7ead2aeb9.png)

可以，输出结果符合我们的预期，接着在爬虫代码节点前加个大数据节点，提取下城市、区和活动类型，提示词：

```
提取{{input}}中的城市、区，活动类别 (非必要，可选值：音乐、戏剧、讲座、聚会、电影、展览、运动、公益、旅行、赛事、课程、亲子、其它)

约束：
严格按照这样的json格式输出：
{
  &quot;city&quot;: &lt;城市，需要以市结尾&gt;,
  &quot;district&quot;:&lt;区，需要以区结尾&gt;,
  &quot;category&quot;: &lt;活动类别，没有的话显示空字符串&gt;
}
```

**发布下工作流，然后**Bot添加下这个工作流，修改下 「人设与回复逻辑」 的提示词，调用对应的工作流：

+   角色

你是一个周末活动推荐官，如果识别到用户发送的信息里有城市和区，调用技能1，否则根据上下文回复一句带有颜文字的话。

技能
技能1：周末好去处
判断用户发送的信息中是否包含"景点"这个字符串：
如果包含，调用search_city_attraction_info工作流。
如果不包含，调用weekend_go_good_places工作流。


约束

信息中必须包含"景点"两个字才调用search_city_attraction_info工作流。

手敲下 **「开场白文案」**：

+   😘 你好！我是一名周末活动推荐官，能为你推荐一些城市区的景点和周末同城活动信息。查询指令示例：

景点 → "深圳市南山区景点"
同城活动 → "深圳市南山区音乐"，后面跟着的音乐是活动类型，不设置默认是全部，可选值有：音乐、戏剧、讲座、聚会、电影、展览、运动、公益、旅行、赛事、课程、亲子、其它。

🥰 赶紧试试吧~

自定义几个 **「开场白预置问题」**：

![](img/8acc1474b6bec514ef2e099ddac886b4.png)

顺带选个蛙蛙甜妹的 **「音色」**：

![](img/10ac369674438dddb91876fac5c8be87.png)

**预览和调试随便**输入一个城市区，如 "「北京朝阳区」"：

![](img/9625090d9081a65e35ec49bf509e7933.png)

行吧，查询景点也能正确返回，发布下Bot：

![](img/2f5179e5363c2fb683f5db894c14b345.png)

1.  Bot效果

发布到豆包后，就等审核了，期间自个是可以偷着乐的，发起聊天试试：

![](img/80e89ba3b06a1fa50013deedce6694e0.png)

😁 哇塞~ 效果是真不戳啊！！！信息**是真的全****，当然，美中不足**的地方可能是 「排版」 了，后续可以美化下数据的输出格式。最后，关于 「扣子里写爬虫」 说一嘴：

>

> **虽然上面使用了**休眠，但多次调用后站点大概率还是会 「**短暂封IP」，233**3，只是封的扣子服务器的IP，不是我**们自己的IP🤣，建议还是**设置下 「Cookie字段」，让用户设置下，通过扣子**提供的数据库进行关联。当然，更好**的方式**是，自**己整个 「云服务器部署爬虫脚本」，定时爬数据存数据库，提供API接口给Bot调用。这种方式有一定的技术门槛，读者也可以试试 「云存储方案 (如使用Bmob，通过API方式对云数据库进行增删改差)」 ，「Mock API + 写死返回数据 (如使用」「Apifox」 「)」 等，实现形式有很多种，自己怎么方便怎么来。

>

# 最后

> 如果本文对你有帮助或对AI Agent感兴趣，可以联系我们或者加入我们的AI洞察中心交流群～

+   当然我也很希望看到你根据本文亲手捏的应用，联系我！我一定为你点赞👍

> 参考资料：[AI Agent 智能体榜单](https://bluefocus.feishu.cn/wiki/TwW5wNDlfifjLgkE2micxthenNe)


官方群聊

![](img/fdac07c7d5cc140173e81bc5296f42a8.png)

智能体搭建交流群

![](img/84d5c74c59cd670d953b29437a16b905.png)

bot互助群

> 如果您想与高手切磋，与AI爱好者一起热烈**讨论，发**表观点，分享作品，欢迎加入我们的官方群聊

> **智能体搭建交流群**：智能体搭建的互助交流群，群里有coze、扣子等各路技术流大佬实时解答技术问题，想交流的欢迎进群

> 如果您想与更多让自己制作的智能体冲上热门推荐页，获得更多曝**光，欢迎加入**由bot创建爱好者自发建立的bot互助群