
# 一个案例教会你从数据准备到微调验证！

> 来源：[https://v11enp9ok1h.feishu.cn/docx/BKRbdCWvoozfAUxL176cTRjwn0b](https://v11enp9ok1h.feishu.cn/docx/BKRbdCWvoozfAUxL176cTRjwn0b)

> 大模型的微调数据从哪里来？到手的数据该怎么处理？什么样的数据适合微调？本文将通过一个个性化的项目，带大家一步步操作，从数据准备到微调验证，一文掌握大模型！

天机是 `SocialAI`（来事儿AI）是一款免费使用、非商业用途的人工智能系统。您可以利用它进行涉及传统人情世故的任务，如如何敬酒、如何说好话、如何会来事儿等，以提升您的情商和核心竞争能力。本文将用 `Xtuner` `Qlora` 讲解了如何基于 `InternLM2` 进行天机送祝福模块的从数据制造到推理微调、效果验证的全流程。如果有帮助欢迎star~ https://github.com/SocialAI-tianji/Tianji

![](img/dc9cb07188aa52da1e06b71183b3bd31.png)

其他更多大模型的微调方法推荐请参考这个项目：https://github.com/datawhalechina/self-llm

在本次微调示范选用的是 `internlm2-chat-7b` 模型，你需要准备一台24G显存的机器用于微调（NVIDIA RTX 3090 即可）。

## 数据处理

微调一个模型的第一步是准备高质量的训练数据。对于一个送祝福模型，你需要收集各种祝福语的数据，数据来源可以是公开的祝福语数据集、社交媒体、电子书籍或者任何包含丰富祝福语的文本。

在准备完成数据来源以及获取到对应数据后，你需要使用该数据文本进行数据制造（比如下面演示的 few shot，但这只是最小的例子，真正意义的数据制造需要用一个数据“知识”切块去生成对应的 QA 对，这才是我们最后期望得到的数据。

所以，理论上最好的数据是利用这些现有知识，通过更聪明的大模型基于这些知识得到高精度的回复 QA 对数据，也有的人是通过大模型抽取小说文本对话的方式来实现格式抽取，但总之你需要的是一个最好无限火力的聪明大模型来帮助你进行文本数据清洗。

当你成功打通微调后，你会发现真正复杂的工作都是在清洗数据、处理、生成数据、归类数据上，这些才是影响最后效果的最大难点问题。

这里推荐使用本地的 `LLM` 去进行数据清洗（除非你财大气粗），否则 `api key` 很容易分分钟用完，你可以通过部署本地 `llama3-chinese` 或者 `qwen` 等模型进行数据制造工作。

接下来我们来看看如何进行数据制造：

### 数据制造

在清洗数据前，请确保你已经安装对应 `SDK` 如 `zhipuai` 以及 `openai` `SDK`，安装后直接运行即可。

```
from zhipuai import ZhipuAI
import time
import json
import random 
import datetime 

# zhipuai
# 此处填写您自己的APIKey
# zhipu_api_key = &quot;&quot;
# client = ZhipuAI(api_key=zhipu_api_key)
# def get_data_zhipu(content):
#     response = client.chat.completions.create(
#         model=&quot;glm-4&quot;,  # 填写需要调用的模型名称
#         messages=[
#             {&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;你现在是一个精通言语表达、热爱他人、尊重长辈、富有文采的送祝福大师，请你编辑一条文本，表示对应场景的祝福语&quot;},
#             {&quot;role&quot;: &quot;user&quot;,
#             &quot;content&quot;: content,
#             &quot;temperature&quot;: 1} # 多样化输出
#         ],
#     )
#     res = response.choices[0].message.content
#     return res

# deepseek
from openai import OpenAI
deepseek_key = &quot;&quot;  #此处填写deepseek的key
client = OpenAI(api_key=deepseek_key, base_url=&quot;https://api.deepseek.com/v1&quot;)
def get_data_ds(content):
    response = client.chat.completions.create(
        model=&quot;deepseek-chat&quot;,
        messages=[
            {&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;你现在是一个精通言语表达、热爱他人、尊重长辈、富有文采的送祝福大师，请你编辑一条文本，表示对应场景的祝福语&quot;},
            {&quot;role&quot;: &quot;user&quot;,
             &quot;content&quot;: content,
             &quot;temperature&quot;: 1} 多样化输出
        ]
    )
    res = response.choices[0].message.content
    return res

# 可利用大模型补充不同对象  当前28种
name_list = [&#x27;赵老师&#x27;, &#x27;大舅&#x27;, &#x27;大伯&#x27;, &#x27;李总&#x27;, &#x27;邻居赵大妈&#x27;, &#x27;母亲&#x27;, &#x27;姐姐&#x27;, &#x27;妹妹&#x27;, &#x27;哥哥&#x27;, &#x27;弟弟&#x27;, &#x27;爷爷&#x27;, &#x27;奶奶&#x27;, &#x27;外公&#x27;,
        &#x27;外婆&#x27;, &#x27;伯母&#x27;, &#x27;叔叔&#x27;, &#x27;阿姨&#x27;, &#x27;堂兄&#x27;, &#x27;堂妹&#x27;, &#x27;表哥&#x27;, &#x27;表妹&#x27;, &#x27;导师&#x27;, &#x27;同学&#x27;, &#x27;同事&#x27;, &#x27;领导&#x27;,
        &#x27;邻居&#x27;, &#x27;老板&#x27;, &#x27;医生&#x27;, ]

# 可利用大模型补充对应场景 当前18种
scenes = [&#x27;生日&#x27;, &#x27;春节&#x27;, &#x27;元宵节&#x27;, &#x27;端午节&#x27;, &#x27;七夕节&#x27;, &#x27;中秋节&#x27;,
            &#x27;重阳节&#x27;, &#x27;除夕&#x27;, &#x27;腊八节&#x27;,&#x27;谈判顺利&#x27;,&#x27;乔迁新居&#x27;, &#x27;周年纪念&#x27; ,&#x27;新婚快乐&#x27; ,&#x27;家庭和睦&#x27;, &#x27;比赛取得好成绩&#x27; ,&#x27;发财&#x27;,&#x27;工作升职 &#x27;,&#x27;康复&#x27;, ]

# 可利用大模型补充不同风格，加入更多 fewshot 造出更好的数据
styles = {
    &quot;小红书&quot;:
    {
        &quot;style_temple&quot;:&quot;小红书风格，每条加入1-2个emoji表情包来增加趣味性。\n### 注意，你要参考下列句子的艺术风格进行祝福语撰写（注意！只看造句风格），祝福语结尾都带上语气助词词，参考句子为：{} ###&quot;,
        &quot;if_example&quot;:True,
        &quot;examples&quot;:
        [
    &#x27;默念你的名,祝你前途云蒸霞蔚，灿若星河。愿你度过的叫吉时，得到的叫如愿！&#x27;,
    &#x27;希望你岁末将至，敬颂冬绥，平安喜乐，万事胜意。&#x27;,
    &#x27;希望你不用奔赴大海，也能看到春暖花开；不用颠沛流离，也能遇到一生所伴！&#x27;,
    &#x27;祝我们好在春夏秋冬,祝你阔谈，祝你烂漫，祝你和自己相约在风里，此后只剩欢愉。&#x27;,
    &#x27;希望你可以明确地爱，直接的厌恶，真诚的喜欢，站在太阳下的坦荡，大声无愧地称赞自己，学会爱自己！&#x27;,
    &#x27;前方荣光万丈，身后温暖一方，凡是过往，皆为序章。&#x27;,
    &#x27;愿所念之人 平安喜乐。愿所想之事 顺心如意！&#x27;,
        ]
    },
    &quot;正常&quot;:
    {
        &quot;style_temple&quot;:&quot;正常风格，有礼貌即可&quot;,
        &quot;if_example&quot;:False,
        &quot;examples&quot;:[]
    },
    &quot;严肃&quot;:
    {
        &quot;style_temple&quot;:&quot;商业严肃风格，要求用在职场或长辈祝福上，显得有礼貌、干练,句子可以长一些&quot;,
        &quot;if_example&quot;:False,
        &quot;examples&quot;:[]
    }
}

random_finalprompt_sentence = [
    &#x27;&#x27;, #默认情况
    &#x27;回答中可以不出现对象称谓和场景信息，也不用出现“愿你”“祝你”（对自己的长辈需要出现对象称谓和祝你），&#x27;,
    &#x27;回答中可以不出现对象称谓和场景信息，&#x27;,
    &#x27;回答中不用出现“愿你”“祝你”&#x27;,
]
final_prompt = &quot;&quot;&quot;
该祝福语字数小于 {} 字。 \n 
请根据对象称谓及场景，写出符合对象的身份和场景气氛的祝福文案。要求的风格是：{} \n，注意不要有标题混在其中，对象称谓是：{}，祝福场景是：{}。 \n
{} 根据不同对象用不同的语气（尊敬、诙谐搞笑、亲近），请直接返回祝福文本，不要说任何其他话：
&quot;&quot;&quot;

if name == &quot;__main__&quot;:
    #### 此处配置 ####
    roop_count = 2
    now_count = 0
    stylename = &quot;小红书&quot; 小红书、正常、严肃
    output_number_limit = 50 限制回答输出长度，严肃的100，普通的小于20
    #### 此处配置 ####
    
    for roop in range(roop_count):
        conversations = []
        for name in name_list:
            for scene in scenes:
                try:
                    if styles[stylename][&#x27;if_example&#x27;]:
                        style_prompt = styles[stylename][&#x27;style_temple&#x27;].format(random.choice(styles[stylename][&#x27;examples&#x27;]))
                    else:
                        style_prompt = styles[stylename][&#x27;style_temple&#x27;]
                    input_prompt = final_prompt.format(output_number_limit, style_prompt, name, scene,random.choice(random_finalprompt_sentence))

                    response = get_data_ds(input_prompt)
                    now_count += 1
                    
                    if &#x27;\n&#x27; in str(response):
                        response = str(response).split(&#x27;\n&#x27;)[0]

                    print(name,scene,&#x27;response:&#x27;,response)
                    print(&quot;当前生成数目：&quot;, now_count)
                    if stylename == &#x27;正常&#x27;:
                        # 默认不加风格指定
                        _input_prompt = f&quot;祝{name}{scene}&quot; 
                    else:
                        _input_prompt = f&quot;祝{name}{scene},{stylename}风格&quot; 
                    print(&quot;input:&quot;,_input_prompt)

                    conversation = {
                        &quot;conversation&quot;: [
                            {
                                &quot;system&quot;: &quot;你现在是一个送祝福大师，帮我针对不同人和事情、节日送对应的祝福&quot;,
                                &quot;src_input&quot;:input_prompt,
                                &quot;style_name&quot;:stylename,
                                &quot;input&quot;: _input_prompt,
                                &quot;output&quot;: str(response).replace(&#x27;\&quot;&#x27;,&#x27;&#x27;)
                            }
                        ]
                    }
                    
                    # 将对话加入到列表中
                    conversations.append(conversation)
                except Exception as e:
                    print(e)
                    continue

        now_time = datetime.datetime.now().strftime(&quot;%Y-%m-%d-%H-%M-%S&quot;)
        file_path = f&quot;./wishes_{stylename}_{now_time}.json&quot;
        with open(file_path, &quot;w&quot;, encoding=&#x27;utf8&#x27;) as f:
            json.dump(conversations, f, ensure_ascii=False, indent=4)
```

注意，这里需要把 `input` 替换成 `f"祝{name}{scene}"` 的格式原因——是因为这里的 `input` 需要尽可能模拟人的输入，而不能是制造数据时候的输入。此外，我们在此设定了三种风格：正常、📕、严肃；期待当加上风格名触发后可以有预期的输出。

这里只是最简单的数据制造示例。根据祝福语的长短不同而生成对应的语法风格、生成更接近真人的风格，这些都依赖高质量的数据 + 更好的数据制造方式才可获得。
