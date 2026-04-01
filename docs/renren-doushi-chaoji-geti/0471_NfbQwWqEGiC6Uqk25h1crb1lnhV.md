# W001丨一键提取无水印视频和文案，自动二创并写入飞书

> 来源：[https://xuqiwei1986.feishu.cn/docx/RyKndoFBEoZeJKxV3cKcrx7rnBc](https://xuqiwei1986.feishu.cn/docx/RyKndoFBEoZeJKxV3cKcrx7rnBc)

今年初写数字人实操的时候就提到AI智能体绝对是帮助我们打造“一人公司”的核弹级武器。

![](img/36590fae60c38055ed17c99928ecbfc5.png)

三个月过去了，我对此更加深信不疑，AI智能体（工作流）在帮我矩阵化创作方面，起码减少了90%以上的工作量！

最近项目进入稳定期，我觉得是时候开始着手写一些关于智能体/工作流的教程了。

> 我创建的工作流主要都是应用于我自己的项目流程中，更多的是和获客相关。

> 市面上还有很多类型的智能体和工作流，底层逻辑都相似，一通百通。

我把“提取无水印视频+文案”作为第一个分享的智能体，是因为我们有一定基础的伙伴都知道，现如今做自媒体或者短视频获客，最重要的就是找“对标”，并进行润色改写。

本文拆解70%技术向+30%底层逻辑思考，力求做到新手小白照着做也能搭建成功，有技术背景的同学也能从中收获一些火花。

最后我也会提供成品版的智能体供大家直接使用。

> 目录：

> 一、流程梳理

> 二、搭建过程

> 三、成品智能体

以下正文，enjoy

# 一、流程梳理

我们要实现的功能是：一键提取无水印视频和文案，自动二创并写入飞书

拆解一下：

+   短视频链接提取无水印视频+文案

+   文案二创

+   相关结果写入飞书

由此我们写画出一个智能体的工作流程图，这也是搭建工作流的核心，理清了流程，整个智能体的搭建也就完成了80%。

# 二、搭建过程

## 创建智能体

打开coze网站 www.coze.cn

按提示进行相关注册工作后登录完成后你看到的界面会是这样

+   点击左边栏的“工作空间”

+   创建你自己的工作空间，当然直接使用个人空间也可以

+   点击项目开发

+   点击创建按钮

![](img/79e019d5ce8db23554b3f3948d0c4dcd.png)

+   选择“创建智能体”

![](img/b86b0c706b157e126251b009b369a6f4.png)

+   输入智能体相关信息

![](img/e4104412cf88f3d77363640420fac203.png)

## 创建工作流

+   进入智能体编辑页面后，点击工作流后的+号

![](img/969f59486951aaacdda021f7fb2f231d.png)

+   这里我们创建一个对话流

![](img/2ee3f057d8192b06481a9de75c5bb07d.png)

+   老规矩，填入相关信息
工作流和对话流的名称都只能是英文字符及下标字符“_”，这里我一般习惯把工作流按用途编号，名称用英文或者拼音描述用途

![](img/2844866e5389bb8f247f6ff06cf85d79.png)

## 工作流编辑

### 3.0 开始节点

默认配置即可

![](img/757040103fa048c13a18559dca61dadf.png)

### 3.1 视频链接提取插件

+   找到插件"视频全平台下载"并连接到开始节点

![](img/1fd6be225da33df9517a48da720084db.png)

这个插件是我用下来最稳定好用的，原先是免费的，现在需要收费。

解析一次收费1分钱，当然市面上也有更便宜的解析API，因为我很多工作流都用的这个插件就懒得换了，费用也不算太高。

按提示去官网注册后充值即可，充值个10块钱可以用非常久了。

+   插件参数配置如下，url连接到开始节点的{{user_input}}，{{api_token}}就填入你刚刚在插件官网获取的API

![](img/f4560440671f3435140a26eafc43f0aa.png)

在输出参数中，我大概讲一下我们会用到哪些信息

{{photo}} 视频封面的url下载链接

{{title}}视频标题

{{url}}无水印视频下载链接

### 3.2 插入判断函数

主要目的是判断上个节点的插件是否解析成功

![](img/0bd962723649dbdbab5c179189aeae20.png)

IDE代码选择python语言

> 这里代码如果没有基础的同学可以直接让deepseek-R来帮你完成，我几乎90%的代码都是deepseek完成的

> 另外一定要把coze空间的初始代码喂给AI，让他学习这段代码的编写rule（规则），然后再生成对应的代码

![](img/b81a4ee8f9eff0d1603a294d694a3b64.png)

```

from typing import TypedDict, List, Dict, Union

# 定义输入参数类型
class Input(TypedDict):
    input: Union[List[str], None]  # 修改：允许 input 为 None

# 定义输出参数类型
class Output(TypedDict):
    array_num: int
    is_valid_input: int

async def main(args) -&gt; Output:
    params = args[&#x27;params&#x27;]
    input_data = params[&#x27;input&#x27;]

    if input_data is None:
        is_valid_input = 0
        array_num = 0  #  当 input 为 None 时，array_num 设为 0
    else:
        is_valid_input = 1
        array_num = len(input_data)

    return {
        &#x27;array_num&#x27;: array_num,
        &#x27;is_valid_input&#x27;: is_valid_input
    }
```

### 3.3 插入判断节点

> 为什么我会加入判断函数和判断节点，主要是有的时候我还会直接输入云端的视频链接。而插件只能解析抖音/快手/小红书/B站的视频链接，遇到这种情况就会输出空值。

> 而我希望我直接输入云端视频链接的时候也可以执行后续的工作。

根据上个节点的输出结果来执行对应的动作

![](img/39df930883bb1122d8e9edc297b72aef.png)

### 3.4 执行失败，本地视频转文案

![](img/e44ab48ad162b721665fb1e5b267a6d5.png)

![](img/0a6b6497abecb52f0f8d1fbd60a4fb1d.png)

{{api_key}}填写你的阿里云百炼模型API

阿里云百炼API获取

https://bailian.console.aliyun.com/?tab=model#/api-key

![](img/cb3e48ac925ededf0b0d778c41f93312.png)

这里我们用到的是一个自建插件，功能是调用阿里云的API来进行视频

> 给大家补一下自建IDE插件的相关知识，后面会把它单独整理成文档

1.  在工作空间-资源库中，新建插件

![](img/2f0e28e66ad4ee58ae3fa18b7e53c715.png)

1.  填写相关信息

![](img/a922e866814ec9607947b7fcd8532f8a.png)

1.  点击创建工具

![](img/0154a06a1152da328a193c298fd7b326.png)

1.  点击添加工具并输入信息

![](img/58e1de4060a20433b661f7d2c9776111.png)

![](img/5fb6bef033c251c32e60554c695f59c3.png)

1.  在代码区域复制以下代码，一样可以让deepseek帮你写和优化

```
&quot;&quot;&quot;
功能说明：通过阿里云的服务，将音频文件转换为文本内容。
&quot;&quot;&quot;

from typing import TypedDict
import dashscope
import requests

class Input(TypedDict):
    api_key: str
    file_url: str

class Output(TypedDict):
    data: str
    error_message: str

def handler(args) -&gt; Output:
    &quot;&quot;&quot;
    示例用法:
    ```python
    args = {
        &quot;input&quot;: {
            &quot;api_key&quot;: &quot;your_api_key&quot;,
            &quot;file_url&quot;: &quot;https://example.com/audio.mp3&quot;
        }
    }
    result = handler(args)
    print(result)
    ```
    &quot;&quot;&quot;
    try:
        api_key = args.input.api_key
        file_url = args.input.file_url

        dashscope.api_key = api_key

        task_response = dashscope.audio.asr.Transcription.async_call(
            model=&#x27;paraformer-v1&#x27;,
            file_urls=[file_url]
        )

        transcribe_response = dashscope.audio.asr.Transcription.wait(task=task_response.output.task_id)

        if transcribe_response.status_code == requests.codes.ok:
            transcription_data = transcribe_response.output
            transcription_url = transcription_data[&#x27;results&#x27;][0][&#x27;transcription_url&#x27;]

            response = requests.get(transcription_url)
            if response.status_code == requests.codes.ok:
                result = response.json()
                text = result[&#x27;transcripts&#x27;][0][&#x27;text&#x27;]
                return {&quot;answer&quot;: text, &quot;error_message&quot;: &quot;&quot;}
            else:
                raise Exception(f&quot;Failed to fetch transcription content from {transcription_url}&quot;)
        else:
            raise Exception(&quot;Transcription task failed&quot;)

    except Exception as e:
        error_message = f&quot;发生错误: {str(e)}&quot;
        return {&quot;answer&quot;: &quot;&quot;, &quot;error_message&quot;: error_message}
```

1.  添加相关依赖

![](img/107eeae2b4a8d5d819b005286713b6b2.png)

![](img/857f8f4abb46d7f1c81f13f00d8822b1.png)

1.  在元数据中填写对应输入参数，注意名称和类型都要正确

![](img/45bec9366b0256d1864460c470fcd4c7.png)

1.  点击测试代码

把你的阿里云api和插件解析后的视频链接填入

![](img/8892d896315e00d1416e5b4de96c0a9b.png)

1.  点击运行，确认插件运行成功

![](img/41b2232cf42be0d7d5714571b384d6ac.png)

1.  发布插件

![](img/a1da2eb40b24e7c2b24a02821ddec448.png)

然后就能在你的工作流中使用了

> 自建插件教程结束

### 3.5 消息输出

这一步的主要目的是在提取成功后，智能体给用户输出一段提醒，防止用户等待过久以为提取失败了

此节点可删除

![](img/e8fd22890ef47d15c6e64455c0245dce.png)

### 3.6 文案纠错

这里我插入了一个工作流，主要目的是文案纠错

![](img/3f4bd99b412e232fa827aba3d1a476ef.png)

![](img/6c7d9842db49316789e952690662d61c.png)

主要流程就是把长文案根据标点符号或者换行符，拆分成一段一段的数组，然后每段分别取用AI模型进行纠错

> 原先写这个的子工作流的时候，主要调用的claude模型支持一次输出的token数量有限，遇到千字长文案甚至万字长文案的时候就会报错，所以采用这个方案，理论上采用这个方案，10万字的文案也不会报错

当然现在coze内置了deepseek-v3，可以直接调整回复长度最大16384token，约等于25000个文字，足以应对99.99%的情况了

![](img/61955b606009cd09201c7ec56238f301.png)

因此我们可以直接插入大模型节点来代替原来的子流程

![](img/533ca9a6a32e27b6fa7b6a82d3a0ffa4.png)

系统提示词如下

```
 ## 角色(Role):  
 你是一位精通中文的文字校对专家,擅长识别和纠正各类文本中的错别字和基本语法错误。你对中文语言有深刻的理解,能在保持原文结构和意思的前提下,准确修正文字错误。  
 ## 背景(Background):  
 在当今快节奏的信息时代,许多文案(如抖音视频文案、会议录音转写稿等)常常因匆忙制作而存在错别字和基本语法错误。这些错误可能影响文案的专业性和可读性,因此需要一个专业的校对工具来提高文案质量。  
 ## 任务(Task): 
 你的任务是仔细阅读并理解提供的中文文案(可能包含少量英文),然后纠正其中的错别字和明显的语法错误。你需要保持原文的整体结构、语气和主要意思不变,只关注于纠正文字错误。不要对文案内容做任何解释、评论或回答文案中的问题。  
 ## 规则与限制(Rules &amp; Restrictions):  
 1、只纠正错别字和明显的语法错误或标点符号错误，一定不要不改变文的结构、语气或表达方式。 
 2、不要添加、删除或重组任何句子。 
 3、不对文案内容做任何解释或评论。 
 4、不回答文案中可能出现的任何问题。 
 5、如遇到不确定是否为错误的情况,保留原文。 
 6、对于英文部分,仅纠正明显的拼写错误。    
 ## 案例展示(Case Show):  
 输入文案示例: &quot;我们公司今年的营业鹅比去年赠长了百分之二十,这个成就来之不易,希望大家再接再里,为公司的发展做出更大的贡鞋。&quot; 
 输出修正示例: &quot;我们公司今年的营业额比去年增长了百分之二十,这个成就来之不易,希望大家再接再厉,为公司的发展做出更大的贡献。&quot;  
 ## 风格和语气(Style &amp; Tone):  
 保持中立、专业的语气,不添加个人观点或情感色彩。修正应当精准、客观,仅限于文字或标点符号错误的纠正。  
 ## 输出格式(Output format):  
 注意：直接输出修正后的纯文案,不需要标注修改之处或解释修改原因，不要输出和修改后文案任何无关的信息，下面xxx就表示修改后的文案内容； xxx  
 ## 工作流程(Workflow):  
 1、仔细阅读并理解提供的文案内容。 
 2、识别文案中的错别字和明显的语法错误。 
 3、在保持原文结构和意思不变的前提下,修正已识别的错误。 
 4、检查修正后的文案,确保未改变原意和结构。  
 ## 初始化(Initialization):
```

### 3.7 执行成功，短视频链接转文案

7号节点和4号节点一致，只有{{file_url}}参数来源有区别

![](img/2625e9e71e981d93df33a80ae4febf6b.png)

![](img/62812581386cc2343e2c178dcc8f3c8a.png)

### 3.8 重复3.5

### 3.9 重复3.6

{{input}}来源从7号节点中获取

![](img/00f9cf92bfb40db07b656b778da29132.png)

### 3.10 合并文案

把两条支线的纠错后的文案用合并字符串在一起

> 必须要这样做的原因是，只有一条支线会工作，另外一个支线输出为空

> 但coze有一个规则是，当某个节点输入参数为空的时候会报错

![](img/d167210229e194840792842905fd8d8c.png)

### 3.11 长文案润色

这里我也是使用自建插件，调用claude大模型来进行润色改写

![](img/79b600db78ed8c5bc9277c611d2070c2.png)

实际上使用coze自带的deepseek-v3足够，插入方法同3.6

润色提示词，当然你也可以使用自己的提示词来进行二创或者润色

```
#Role: 
爆款短视频文案资深编导 
#Background: 
你是爆款短视频文案资深编导，我现在需要改写一条爆款口播文案，改写的目的是继续踩中原文案的爆款因子，同时避免和原文案的雷同，根据抖音平台的受众的需求，生成一个优化后的口播文案版本。
#Skills: 
1.文案优化技能：   根据抖音平台和受众的特性进行口播文案优化，确保内容具有吸引力和互动性。 
2.数据分析与用户行为洞察：   分析抖音平台的用户行为和喜好，制定相应的优化策略。 
3.风格应用：    选择合适的文案风格，并应用于内容优化中。 
4.熟悉平台的风格、特点：   质量创意优先。用户群体属性标签是年轻、活力、热爱潮流、爱好广泛、乐于分享。 其流量机制和传播特征是:个性化推荐算法，根据用户的互动行为和历史记录进行兴趣推送。抖音的流量机制注重内容质量和创意，通过短视频的形式吸引用户关注和转发，从而扩大传播范围，热门内容大多为搞笑、舞蹈、美食、旅行、美妆等。 
#Goals: 
帮助用户根据提供的口播文案内容，结合抖音平台和受众的需求，生成优化后的口播文案版本，提高内容的曝光率和互动率。  
#Constraints: 
-优化后的文案内容字数与原文相差不超过10%。 
-要求符合自媒体平台的规则，对于敏感词、限制词要进行规避,不要出现emoji表情符号。 
-请直接输出优化后的文案，不需要任何解释
```

### 3.12 生成成品6项

主要目的是输出

+   封面标题

+   作品标题

+   视频描述

+   话题标签

+   上墙大字报

+   评论区首评

![](img/2783b2883983f8b8860d85048480d0d7.png)

```
你是爆款短视频文案资深编导，你现在需要对文案进行分析，并生成“成品6项”，请参考附件《”成品6项“模板及编写提示》

附件：《”成品6项“模板及编写提示》
““”
1. 封面标题
2. 作品标题
大标题：字数控制在4个字以内，核心是提炼出文案的核心词汇，抓住观众眼球，通常
具有强烈吸引力的词汇，如“揭秘”、“涨粉”、“流量”、“攻略”等，可以引发用户的点击欲
望。
小标题：不超过6个字，通常是对大标题的补充解释，帮助观众快速理解视频主题。例
如“大标题：涨粉秘籍，小标题：3个技巧助你快速涨粉”。小标题提供具体方向，让观
众知道观看该视频的实际收获。
3. 视频描述
4. 话题标签
5. 上屏大字报
6. 短视频评论区首评

&lt;作品标题&gt;一般是对视频内容的总结性描述，字符不超过16个，但应保持简洁，便于观众
搜索和理解内容。例如：“3步教你搞懂视频号推流机制，快速涨粉！”
&lt;视频描述&gt;是对主文案的简化版，字数较少，用于简明扼要地概括视频内容，内容可稍
长，通常在50个字符以内。用来进一步吸引观众点击观看，比如：“今天带你了解视频
号的推流机制，如何快速涨粉！”
&lt;话题标签&gt;是帮助视频被更广泛传播的重要元素。使用话题标签时，选择与视频内容紧密
相关的主题，结合行业热门话题和视频平台流行的关键词。例子包括：#思维力量 #情
绪管理 #健康与心理等，这样可以确保你的内容在相关话题中有曝光。
&lt;上屏&gt;：用于强调行动或结论，吸引用户继续观看或采取行动。例子：“3步教你搞懂推流
机制！”
&lt;首评&gt;是博主自己给视频评论，目的是引导观众的互动。一般首评提出问题或建议观众表
达看法，以带动讨论。例如：“你的视频号涨粉慢吗？留言
```

### 3.13 生成飞书文档文件名

插入函数插件，根据日期+随机编码生成文件名

![](img/b5dee196830055f54e22bae5c4c0e009.png)

```
import random
import string
from datetime import datetime

async def main(args: Args) -&gt; Output:
    params = args.params

    def generate_filename(
        prefix: str = &quot;文案二创&quot;,               # 默认前缀
        date_format: str = &quot;%Y%m%d&quot;,        # 默认日期格式
        random_length: int = 3              # 随机字符长度
    ) -&gt; str:
        &quot;&quot;&quot;生成自动命名文件
        Args:
            prefix: 文件名前缀（默认文案二创）
            date_format: 日期格式（默认年月日）
            random_length: 尾部随机字符数（默认6位）
        &quot;&quot;&quot;
        # 生成日期部分
        date_str = datetime.now().strftime(date_format)
        
        # 生成随机字符串（字母+数字）
        rand_str = &#x27;&#x27;.join(random.choices(
            string.ascii_uppercase + string.digits, 
            k=random_length
        ))
        
        return f&quot;{prefix}_{date_str}_{rand_str}&quot;

    # 从参数获取配置（没有参数则用默认值）
    filename = generate_filename(
        prefix=params.get(&quot;prefix&quot;, &quot;文案二创&quot;),
        date_format=params.get(&quot;date_format&quot;, &quot;%Y%m%d&quot;),
        random_length=int(params.get(&quot;random_length&quot;, 6))
    )

    return {&quot;filename&quot;: filename}
```

### 3.14~3.25 创建飞书云文档

这里从14号节点到25号节点都是采用自建插件的方式写入飞书

涉及的流程比较复杂，包括要在飞书开发平台创建机器人和申请相关权限，自建飞书API插件，自建相关工作流

如此复杂的原因是新的飞书文档API不支持markdown格式，因此标题和正文只能用不同的节点来输入

![](img/738c6890aee2d7b3e34b5f4dcbe02fad.png)

新手朋友可以直接使用飞书官方的插件来完成，支持markdown格式，一个插件就能替代上图繁琐的流程

![](img/bedfc9394a0c1677173f6e98a1f96d31.png)

> 为什么这么麻烦我还选择自建插件调用飞书API呢，因为官方API有个麻烦的地方是每个用户第一次调用都需要飞书授权（发布到飞书中使用）

> 如果是发布API的情况，我还没试过，有知道的朋友可以告诉我

如果是使用官方插件，我们需要先建立一个合并字符的节点（14.1），目的是把之前所有节点的相关信息整合，包括

+   无水印视频链接

+   视频封面链接

+   纠错后原文案

+   二创文案

+   成品六项

![](img/af2a35852e053fb05e086e80391b0ba6.png)

然后连接15.1号节点

![](img/849b7dbe8f36175d84ded0b0f5823807.png)

{{fold_token}}就是指定文档所在文件夹 的 Token，不传或传空表示根目录。支持输入文件夹 token 或者 文件夹 URL。

文件夹token就是下图红框区域内容，文件夹URL就是整段链接

![](img/7f65e22a795325e91b46c664f626cc86.png)

### 3.26 结束节点

如果是使用自建插件，{{output}}来源为14号节点的{{url}}参数

![](img/f2b1738ce6b419c4caed169ca9c5a5b2.png)

如果是使用官方飞书插件，则改为15.1号节点的{{url}}参数

## 工作流试运行和调试

点击下方试运行，关联一个智能体，点击开始调试

![](img/da51a25d5937614bdc2e5b235639358f.png)

我们随便复制一个金枪大叔的抖音视频链接，发送后查看运行结果

![](img/a6f4badf05c1489211e9b89df1da1d10.png)

每个节点全部跳出运行成功，看着就是舒服

![](img/72767a239dcaf8c7ea8320fe76311a0f.png)

看一下输出的内容（无水印链接和封面链接有时效性）

> 文件下载地址

>

> [文案二创_20250602_VR16B4](https://jinhuaclub.feishu.cn/docx/UrgadN1zFoLMoUxglqDcYCMhnlW)

![](img/c8e147c6b46355a0ca8142898f91d2c8.png)

点击右上角的发布

![](img/18d0623beab42e9979b623571896461f.png)

至此，我们整个工作流搭建已经全部完成

## 智能体发布

### 5.1 发布API

把开场白输入后，我们就可以点击右上角的发布按钮进行发布了

![](img/c462e7b35cf53b83dfa5758f00ee01da.png)

我们可以选择把智能体发布到如下途径，这里我选择发布API

具体操作可参考这份文档[扣子智能体如何发布API？](https://jinhuaclub.feishu.cn/wiki/IRqmw4G6mij0UXkk5amcmaSancd)

![](img/4a0c46f9d4895a5f41fcede411a4e416.png)

### 5.2 发布小程序

这里我们把API发布到微信小程序“小元智能体”

参考教程[扣子智能体如何发布到小元？](https://jinhuaclub.feishu.cn/wiki/EpY0w7CF2ibENgk91zUcRRNJnDd)

这样我们就可以直接在微信小程序中使用我们智能体了

> 目前小元智能体对创作者免费开放，可去小程序处申请创作者权限

![](img/ac0e9877d5c613ff201c3c7d8e04407f.png)

![](img/e4fe5eb24b3e4f30f1e1d757834496d7.png)

# 三、成品智能体

最后，我也把我搭建完成的智能体对外免费开放使用，欢迎大家体验和提意见

![](img/3dbe0fef7d31e9440d4434696a7b2dd6.png)