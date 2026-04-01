# Coze+多维表格：热点新闻抓取+文案创作

> 来源：[https://datawhaler.feishu.cn/docx/FDtbdktGjoWXD1xkBy1cCUpTnNf](https://datawhaler.feishu.cn/docx/FDtbdktGjoWXD1xkBy1cCUpTnNf)

> 原标题： 3分钟搞定热点新闻+文案创作！飞书多维表格+Coze神操作，小白也能秒上手！

![](img/9337d7a14f5f566f1a1b3d8e807f4d95.png)

在这个信息爆炸的时代，谁能更快抓住**热点，谁****就能抢占先机！ 今天，就教你**一招效率神器，让你彻**底告别手动刷**新闻的苦逼**日子，**一键搭建自动化爆款内容生产线！ 没错，说的就是 飞书Coze！ 一个 零代码 就能玩转的效率平台，让你轻松变身效率达人！

**想象一下，**** 每天早上，热腾腾的最新科技**新闻自动汇总到你的飞**书多维表格，** 标题、分类、**链接，一目了然！** 更惊喜的是，还能 一键文案改写， 热点素材+爆款文案，简直不要太爽！ 这，就是 Coze + 聚合数据 API + 飞书多维表格 联手打造的 自动化神级工作流！

# **效果演示**

![](img/2884974c7150c27a2d095b57d179e419.png)

[3月23日.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/DcbXbR7zWonwjWx4492cmo0Mnke)

# 搭建流程

##  Step 1： 开启你的“数据宝藏” - 聚合数据 API

**想要自动化获****取****新闻？ 首先**你需要一个强大的“数据源”。 聚合数据 (https://www.juhe.cn/) 就是你的宝藏！ 它提供了海量API接口， 免费又好用！ (良心推荐👍)

![](img/01ca0c0e7dd8cfc4c5c16d283e290b5c.png)

**注册登录后，** 在API**市场搜索 “****新闻头条”，** 选择 “免费调用”！ 每**天50次免费额度， 完全够用**！ 点击 “接口**文档”， 仔细阅读， **这里藏着 接口地址、请求方式、参数说明 等重要信息， 划重点！ 后面要用到！

![](img/1189a2db3d80866caa7cb08f05a5a71d.png)

然后查看接口文档，在这里我们可以看到接口地址，请求方式，以及返回的类型，最重要的是各种请求参数的说明，方便我们设置入参。

![](img/c5592141f988640b53ec867950fa3846.png)

## Step 2： Coze工作流， “拖拽”出自动化魔法

开始创建工作流

![](img/fcbd22b7046c3132d7a38d90c2b2d430.png)

配置工作流基础信息

![](img/9d13223c61ce086cc4f10cccafa8925c.png)

在工作流中拖入 **“HTTP请求”** 节点， 这可是连接聚合数据API的关键！

![](img/85c7bfc9ff309d76753afa94b4d18f30.png)

根据接口文档， 请求方式选择 **“GET”**， 接口地址填入 ht**tp://v.juhe.c**n/toutiao/index。 重点来了！ 请求参数设置！

+   **key**: 你的API密钥！ 去 **聚合数据 “我的API”** 页面找， **需要实名认证才能获取哦！** (放心，免费的！)

+   **type**: 新闻类型， 比如 keji (科技)、 yule (娱乐) 等等， 这里我们先用 keji (科技)。

+   **page_size**: 一次抓取多少条新闻？ 设置 10 条够用。

![](img/4f3553d4fc4e35281e227d5c908c45b0.png)

![](img/47fd2988df5fc2b1861c7c6b1fd3d3ac.png)

HTTP请求返回的是JSON数据，** 飞书多维表格需要数组格**式。 怎么办？ “代码**节点” 闪亮登场！ 复**制粘贴以下代码， 一键搞定数据格式转换！ (看不懂代码？ 没关系！ 直接用就行！😎)

![](img/bdfb52b04eec288f5d0fc19d18740268.png)

完整代码（python）：

```
import json
from typing import List, Dict, Any

async def main(args: Args) -&gt; Output:
    params = args.params
    input_json = params[&#x27;input&#x27;]  # 获取输入的 JSON 字符串

    try:
        # 解析 JSON 字符串
        data = json.loads(input_json)
        result_list = data[&#x27;result&#x27;][&#x27;data&#x27;]  # 获取数据列表

        # 构建输出数组
        ret: Output = [
            {
                &quot;fields&quot;: {
                    &quot;标题&quot;: item[&quot;title&quot;],  # 文本内容为 title
                    &quot;分类&quot;: item[&quot;category&quot;]  # 单选内容为 category
                }
            }
            for item in result_list
        ]
    except (KeyError, json.JSONDecodeError) as e:
        # 处理异常情况，例如 JSON 解析失败或字段缺失
        ret: Output = []

    return ret
```

最后一步！ **把新闻数据放**进飞**书多维表格！ 在**飞书中 新建多维表格， 务必创建 “标题” 和 “分类” 两个文本字段 (重要！和代码节点对应)。 创建完成后， 复制多维表格链接。

![](img/557d53e35953f82045a6cf38912d7db9.png)

回到Coze工作流， 拖入 **“飞书多维表格”** 节点， 粘贴刚才复制的 **多维表格链接**， **“Records”** 选择 “代码节点” 的 “对象数组输出”。

![](img/d08240836f1a65bc5d2b3f582fb289a9.png)

![](img/6acb0d0ce906dd4d81925c3180d92fbd.png)

最后的输出节点可以把输出变量都删除了，因为我们的输出放到飞书多维表格了不需要在扣子上做多余输出

![](img/f5bbc496bc6ae4159659014e77f9ce83.png)

## Step 3： 文案创作“加速器” - 快捷指令字段

**想要更上一层楼？** 给**你的多维表格添加一个 “快**捷指令字段”！ 一键文案改写， 灵感爆棚！

![](img/4a919dc6c5f673fdaf8b2a4b5d48c89d.png)

输入指令，并把标题字段引入作为参考，下方还可以获取更多信息包括推理过程以及输出结果等等

![](img/4b2e473dfc30f4abfe48eb40745f9e70.png)

这个工作流简直 **太！好！用！了！** 不仅能 **自动化聚合热点新闻**， 还能 **辅助文案创作**， **效率提升N倍！** 无论是新媒体运营、内容创作者，还是想要提升效率的打工人， 都强烈推荐尝试！

![](img/78a53d779ebb827f1e04b5be03b85ec4.png)

![](img/e42e025800554cd81555dc0d41731b4a.png)

> 嗨，我是Yeadon！

> 一名人工智能专业的大三学生，正在努力成为AI时代的超级个体~

+   ** **欢迎加我交流讨论！ 一起探索AI的无限可能！

+   个人主页：[Yeadon（伊登）的个人说明书](https://q05hlvbg0wa.feishu.cn/wiki/Nn5ZwIwv1iaYnEkDH6KckY7sn2g?from=from_copylink)

![](img/af6c9ad5236c0530b25699dc1d31d773.png)