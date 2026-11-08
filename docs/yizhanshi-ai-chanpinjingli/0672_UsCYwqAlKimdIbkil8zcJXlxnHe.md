# 一个案例教会你从数据准备到微调验证！

> 来源：[https://v11enp9ok1h.feishu.cn/docx/BKRbdCWvoozfAUxL176cTRjwn0b](https://v11enp9ok1h.feishu.cn/docx/BKRbdCWvoozfAUxL176cTRjwn0b)

> 大模型的微调数据从哪里来？到手的数据该怎么处理？什么样的数据适合微调？本文将通过一个个性化的项目，带大家一步步操作，从数据准备到微调验证，一文掌握大模型！

**天机**是 **SocialAI**（来**事儿AI）是**一**款免费使用**、非商业用途的人工**智能系统。您可以利**用它进**行涉及传统人情**世故**的任务，如如何敬酒、如何说好**话、如何会来事儿等，以提升您的情商和核心竞争能力。本文将用 Xtuner Qlora 讲解了如何基于 InternLM2 进行天机送祝福模块的从数据制造到推理微调、效果验证的全流程。如果有帮助欢迎star~ https://github.com/SocialAI-tianji/Tianji

![](img/dc9cb07188aa52da1e06b71183b3bd31.png)

其他更多大模型的微调方法推荐请参考这个项目：https://github.com/datawhalechina/self-llm

在本次微调示范选用的是 internlm2-chat-7b 模型，你需要准备一台24G显存的机器用于微调（NVIDIA RTX 3090 即可）。

## **数据处理**

微调一个模型的第一步是准备高质量的训练数据。对于一个送祝福模型，你需要收集各种祝福语的数据，数据来源可以是公开的祝福语数据集、社交媒体、电子书籍或者任何包含丰富祝福语的文本。

在**准备完成**数据来源以及获取到对应数据后，你需要使用该数据文本进行数据制造（比如下面演示的 few shot，但这只是最小的例子，真正意义的数据制造需要用一个数据“知识”切块去生成对应的QA对，这是才是我们最后期望得到的数据。

所以，理论上最好的数据是利用这些现有知识，通过更聪明的大模型基于这些知识得到高精度的回复QA对数据，也有的人是通过大模型抽取小说文本对话的方式来实现格式抽取，但总之你需要的是一个最好无限火力的聪明大模型来帮助你进行文本数据清洗。

当你成功打通微调后，你会发现真正复杂的工作都是在清洗数**据、处理、生**成数据、归类数据上，这些才是影响最后效果的最大难点问题。

这**里推荐使用本地的 LLM 去**进行数据**清洗（除**非你财大气粗），否则 api key 很容易分分钟用完，你可以通过部署本地 llama3-chinese 或者 qwen 等模型进行数据制造工作。

接下来我们来看看如何进行数据制造：

### **数据制造**

在清洗数据前，请确保你已经安装对应 SDK 如 zhipuai 以及 openai SDK，安装后直接运行即可。

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

**注意**，这里需要把 input 替换成 f"祝{name}{scene}" 的格式原因——是因为这里的 input 需要尽可能模拟人的输入，而不能是制造数据时候的输入。此外，我们在此设定了三种风格：正常、📕、严肃；期待当加上风格名触发后可以有预期的输出。

这里只是最简单的数据制造示例。根据祝福语的长短不同而生成对应的语法风格、生成更接近真人的风格，这些都依赖高质量的数据 + 更好的数据制造方式才可获得。

举个例子，如果在这里我们想要控制祝福语长短和语法风格，前者我们就要在之前的制造条件中（比如之前的对象、场景是一个条件）加入新的祝福语长短控制条件（比如我这里的📕和正常风），而且此时 few shot 参考的句子也应当有所不同，这样才能保证我们制造数据的 LLM 可以返回预期长度的结果；想要控制语法风格，我们就需要爬取大量文艺书籍、📕等真正人类写的文案进行清洗，然后利用这些作为 few shot 得到严格的返回，需要用严格的提示词让模型写出类似的语句或者单独微调一个属于该文艺范畴的模型版本用于制造对应数据（需要留心有时候 few shot 指令跟随不那么有用）。

💡代码中有 random_xxxxxxx_sentence 的部分表明这是一个随机性注入列表，我们可以维护一些语句用于提高随机性（比如附加条件的修改），让大模型返回的结果更具特色。

若成功运行，你将看到类似如下输出结果，等待片刻后将得到属于本地的 json文件  wishes_0501_5000.json ：

```
同学 家庭和睦 response: &quot;烟火年年，暖意洋洋，🏡❤️家是心之所向。&quot;                                           
当前生成数目： 914                                                                                        
同学 比赛取得好成绩 response: &quot;灿若星河，前程似锦🌟，所得皆所愿！&quot;                                        
当前生成数目： 915                                                                                        
同学 发财 response: &quot;春风得意马蹄疾，财源滚滚至君前🎉💰&quot;                                                  
当前生成数目： 916                                                                                        
同学 工作升职  response: &quot;升职之光，照亮星河，未来灿烂如霞。🌟🌈&quot;                                         
当前生成数目： 917                                                                                        
同学 康复祝福 response: &quot;挥别病痛，如花开坚强。🌱✨愿你前程，云蒸霞蔚，身心俱灿。&quot;                        
当前生成数目： 918                                                         
```

**💡注意，这里只是粗暴的进行**所有角色和场景的遍历，但并非所有角色都适配所有场景（很多是不合适的），这里为了改进应该做一个 heatmap 进行映射，若不合适生产该数据，将直接跳过；又或者是在得到数据后做一个匹配，如果同时满足不合适的角色 + 场景就去除该数据 QA 对。

### **数据合并**

因为我们之前的数据都是跑完一轮存一次（以防前功尽弃），所以可能你有多个 json 需要组合，这里提供了一个脚本合并一个文件夹中的所有 json，并把 json 格式清洗成和训练脚本一致适配的格式：

```
import os
import json

def extract_and_merge_conversations(folder_path, output_file):
    all_conversations = []

    # 遍历指定文件夹
    for filename in os.listdir(folder_path):
        if filename.endswith(&#x27;.json&#x27;):
            file_path = os.path.join(folder_path, filename)
            
            # 打开并读取JSON文件
            with open(file_path, &#x27;r&#x27;, encoding=&#x27;utf-8&#x27;) as file:
                data = json.load(file)
                # 提取需要的字段
                for item in data:
                    for conversation in item[&#x27;conversation&#x27;]:
                        extracted = {
                            &#x27;system&#x27;: conversation[&#x27;system&#x27;],
                            &#x27;input&#x27;: conversation[&#x27;input&#x27;],
                            &#x27;output&#x27;: conversation[&#x27;output&#x27;]
                        }
                        # 将每个对话包装在一个 &#x27;conversation&#x27; 键中，并作为独立对象加入列表
                        all_conversations.append({&#x27;conversation&#x27;: [extracted]})

    # 将合并后的所有对话数据写入一个新的JSON文件
    with open(output_file, &#x27;w&#x27;, encoding=&#x27;utf-8&#x27;) as file:
        json.dump(all_conversations, file, ensure_ascii=False, indent=4)

# 使用示例
folder_path = &#x27;tianji_wishes_datasets&#x27;  # 要扫描的文件夹路径
output_file = &#x27;tianji-wishes-chinese-v0.1.json&#x27;     # 输出文件的名称和路径
extract_and_merge_conversations(folder_path, output_file)
```

合并后就是我们所需要的微调数据集。

### **二次清洗**

得到最初数据后，可能还存在一些奇怪的东西，比如句子长度返回错误：如不是回答而是很短的一句 当前祝福语如下 ；加入语气助词后容易出现 ！啦~ 。哦！ 在前面出现标点的奇怪现象。需要利用清洗脚本对数据进行一定的筛选，由于比较冗长（还没有优雅的迭代），清洗脚本放在 https://github.com/SocialAI-tianji/Tianji，可以自行查看

**如**果有时间和精力，最准确的还是加入一道人工清洗程序。当然，最重要的还是在数据制造的时候就记录比较完好的信息和做出比较好的 output ，能大大节约二次清洗的时间。

> **为了方便大家使用，这里**提供已经清洗好的数据下载地址，大家可以自行从 huggingface 上获取：https://huggingface.co/datasets/sanbu/tianji-wishes-chinese/blob/main/tianji-wishes-chinese-v0.1.json

> 国内下载镜像：https://hf-mirror.com/datasets/sanbu/tianji-wishes-chinese

## **环境准备**

接下来我们准备微调的环境，由于时间关系（标准流程是一样的），这里只做省略快速操作，详细操作请参考 Xtuner 的官方教程 https://github.com/InternLM/Tutorial/tree/main/xtuner，或者是 self-llm 项目中关于 Xtuner Qlora 的部分：https://github.com/datawhalechina/self-llm/blob/master/InternLM2/04-InternLM2-7B-chat Xtuner Qlora 微调.md

💡 教程所有操作基于 P**ython 3.10 **构建，请注意 Python 和相关依赖版本。

1.  先基于 **Python 3.10** 创建一个虚拟环境。

1.  随后安装如下依赖：

```
python -m pip install --upgrade pip
pip install modelscope==1.9.5
pip install transformers==4.36.2
pip install streamlit==1.24.0
pip install sentencepiece==0.1.99
pip install accelerate==0.24.1
pip install transformers_stream_generator==0.0.4
pip install einops ujson
pip install protobuf
```

安装 Xtuner：

```
git clone -b v0.1.18 https://github.com/InternLM/xtuner
cd xtuner &amp;&amp; pip install -e &#x27;.[all]&#x27;

# 验证成功
xtuner version
```

1.  模型下载，保存下方命令到 Python 文件，在模型文件夹下运行该文件：

```
from modelscope import snapshot_download

model_dir = snapshot_download(&#x27;Shanghai_AI_Laboratory/internlm2-chat-7b&#x27;, cache_dir=&#x27;./model_temp&#x27;, revision=&#x27;master&#x27;)
```

完成上述过程后就可以正式进入微调阶段。

## **微调**

1.  为了微调自己的数据集配置，我们需要修改 Xtuner 的 config，首先查看有哪些配置：

```
xtuner list-cfg | grep internlm2

internlm2_7b_full_finetune_custom_dataset_e1
internlm2_7b_full_finetune_custom_dataset_e1_sequence_parallel_4
internlm2_7b_qlora_alpaca_e3
internlm2_7b_qlora_arxiv_gentitle_e3
internlm2_7b_qlora_code_alpaca_e3
internlm2_7b_qlora_colorist_e5
internlm2_7b_qlora_json_e3
internlm2_7b_qlora_lawyer_e3
internlm2_7b_qlora_msagent_react_e3_gpu8
internlm2_7b_qlora_oasst1_512_e3
internlm2_7b_qlora_oasst1_e3
internlm2_7b_qlora_sql_e3
```

```
# 新建一个用于微调工作的文件夹
mkdir /home/finetune
# 复制配置文件
cd /home/finetune &amp;&amp; xtuner copy-cfg internlm2_chat_7b_qlora_oasst1_e3 ./
```

1.  接下来我们需要修改配置文件，简单来说你要做这几处修改：

```
# 修改模型为本地路径
- pretrained_model_name_or_path = &#x27;internlm2/internlm2-chat-7b&#x27;
+ pretrained_model_name_or_path = &#x27;/home/model_temp/Shanghai_AI_Laboratory/internlm2-chat-7b&#x27;

# 修改训练数据集为本地路径
- data_path = &#x27;timdettmers/openassistant-guanaco&#x27;
+ data_path = &#x27;/home/merged_data.json&#x27;

# 修改Evaluate
- evaluation_freq = 500
- SYSTEM = &#x27;&#x27;
- evaluation_inputs = [
-     &#x27;请给我介绍五个上海的景点&#x27;, &#x27;Please tell me five scenic spots in Shanghai&#x27;
- ]
 
+ evaluation_freq = 50
+ SYSTEM = &#x27;你现在是一个送祝福大师，帮我针对不同人和事情、节日送对应的祝福&#x27;
+ evaluation_inputs = [
+     &#x27;祝姐姐生日快乐&#x27;, &#x27;祝妹妹谈判顺利&#x27;,&#x27;祝大家元宵节快乐&#x27;
+ ]

# 修改数据集加载
- dataset=dict(type=load_dataset, path=data_path),
+ dataset=dict(type=load_dataset, path=&#x27;json&#x27;, data_files=dict(train=data_path)),
```

1.  以下是修改后的结果，你可以直接复制（只要修改模型路径和训练集路径，以及 Evaluate 的 input ，就可以把他变为你自己的配置文件开始训练。）（你可能奇怪为什么都是 steps 而不是 epoch ，我猜测因为 LLM 通常推荐训练一轮，没必要多 epoch。）：

```
# Copyright (c) OpenMMLab. All rights reserved.
import torch
from datasets import load_dataset
from mmengine.dataset import DefaultSampler
from mmengine.hooks import (CheckpointHook, DistSamplerSeedHook, IterTimerHook,
                            LoggerHook, ParamSchedulerHook)
from mmengine.optim import AmpOptimWrapper, CosineAnnealingLR, LinearLR
from peft import LoraConfig
from torch.optim import AdamW
from transformers import (AutoModelForCausalLM, AutoTokenizer,
                          BitsAndBytesConfig)

from xtuner.dataset import process_hf_dataset
from xtuner.dataset.collate_fns import default_collate_fn
from xtuner.dataset.map_fns import oasst1_map_fn, template_map_fn_factory
from xtuner.engine.hooks import (DatasetInfoHook, EvaluateChatHook,
                                 VarlenAttnArgsToMessageHubHook)
from xtuner.engine.runner import TrainLoop
from xtuner.model import SupervisedFinetune
from xtuner.utils import PROMPT_TEMPLATE

#######################################################################
#                         PART 1  Settings                            #
#######################################################################
# Model
pretrained_model_name_or_path = &#x27;/home/model_temp/Shanghai_AI_Laboratory/internlm2-chat-7b&#x27;
use_varlen_attn = False

# Data
data_path = &#x27;/home/tianji-wishes-test_0502.json&#x27;
prompt_template = PROMPT_TEMPLATE.internlm2_chat
max_length = 2048
pack_to_max_length = True

# Scheduler &amp; Optimizer
batch_size = 1  # per_device
accumulative_counts = 16
dataloader_num_workers = 0
max_epochs = 3
optim_type = AdamW
lr = 2e-4
betas = (0.9, 0.999)
weight_decay = 0
max_norm = 1  # grad clip
warmup_ratio = 0.03

# Save
save_steps = 50
save_total_limit = 10  # Maximum checkpoints to keep (-1 means unlimited)

# Evaluate the generation performance during the training
evaluation_freq = 50
SYSTEM = &#x27;你现在是一个送祝福大师，帮我针对不同人和事情、节日送对应的祝福&#x27;
evaluation_inputs = [
    &#x27;祝姐姐生日快乐&#x27;,&#x27;祝姐姐生日快乐，严肃风格&#x27;,&#x27;祝姐姐生日快乐,小红书风格&#x27;, &#x27;祝妹妹谈判顺利，小红书风格&#x27;,&#x27;祝大家元宵节快乐&#x27;,&#x27;祝领导春节快乐，严肃风格&#x27;
]   

#######################################################################
#                     PART 2  Model &amp; Tokenizer                       #
#######################################################################
tokenizer = dict(
    type=AutoTokenizer.from_pretrained,
    pretrained_model_name_or_path=pretrained_model_name_or_path,
    trust_remote_code=True,
    padding_side=&#x27;right&#x27;)

model = dict(
    type=SupervisedFinetune,
    use_varlen_attn=use_varlen_attn,
    llm=dict(
        type=AutoModelForCausalLM.from_pretrained,
        pretrained_model_name_or_path=pretrained_model_name_or_path,
        trust_remote_code=True,
        torch_dtype=torch.float16,
        quantization_config=dict(
            type=BitsAndBytesConfig,
            load_in_4bit=True,
            load_in_8bit=False,
            llm_int8_threshold=6.0,
            llm_int8_has_fp16_weight=False,
            bnb_4bit_compute_dtype=torch.float16,
            bnb_4bit_use_double_quant=True,
            bnb_4bit_quant_type=&#x27;nf4&#x27;)),
    lora=dict(
        type=LoraConfig,
        r=64,
        lora_alpha=16,
        lora_dropout=0.1,
        bias=&#x27;none&#x27;,
        task_type=&#x27;CAUSAL_LM&#x27;))

#######################################################################
#                    PART 3  Dataset &amp; Dataloader                     #
#######################################################################
train_dataset = dict(
    type=process_hf_dataset,
    dataset=dict(type=load_dataset, path=&#x27;json&#x27;, data_files=dict(train=data_path)),
    tokenizer=tokenizer,
    max_length=max_length,
    dataset_map_fn=None,
    template_map_fn=dict(
        type=template_map_fn_factory, template=prompt_template),
    remove_unused_columns=True,
    shuffle_before_pack=True,
    pack_to_max_length=pack_to_max_length,
    use_varlen_attn=use_varlen_attn)

train_dataloader = dict(
    batch_size=batch_size,
    num_workers=dataloader_num_workers,
    dataset=train_dataset,
    sampler=dict(type=DefaultSampler, shuffle=True),
    collate_fn=dict(type=default_collate_fn, use_varlen_attn=use_varlen_attn))

#######################################################################
#                     PART 4  Scheduler &amp; Optimizer                   #
#######################################################################
# optimizer
optim_wrapper = dict(
    type=AmpOptimWrapper,
    optimizer=dict(
        type=optim_type, lr=lr, betas=betas, weight_decay=weight_decay),
    clip_grad=dict(max_norm=max_norm, error_if_nonfinite=False),
    accumulative_counts=accumulative_counts,
    loss_scale=&#x27;dynamic&#x27;,
    dtype=&#x27;float16&#x27;)

# learning policy
# More information: https://github.com/open-mmlab/mmengine/blob/main/docs/en/tutorials/param_scheduler.md  # noqa: E501
param_scheduler = [
    dict(
        type=LinearLR,
        start_factor=1e-5,
        by_epoch=True,
        begin=0,
        end=warmup_ratio * max_epochs,
        convert_to_iter_based=True),
    dict(
        type=CosineAnnealingLR,
        eta_min=0.0,
        by_epoch=True,
        begin=warmup_ratio * max_epochs,
        end=max_epochs,
        convert_to_iter_based=True)
]

# train, val, test setting
train_cfg = dict(type=TrainLoop, max_epochs=max_epochs)

#######################################################################
#                           PART 5  Runtime                           #
#######################################################################
# Log the dialogue periodically during the training process, optional
custom_hooks = [
    dict(type=DatasetInfoHook, tokenizer=tokenizer),
    dict(
        type=EvaluateChatHook,
        tokenizer=tokenizer,
        every_n_iters=evaluation_freq,
        evaluation_inputs=evaluation_inputs,
        system=SYSTEM,
        prompt_template=prompt_template)
]

if use_varlen_attn:
    custom_hooks += [dict(type=VarlenAttnArgsToMessageHubHook)]

# configure default hooks
default_hooks = dict(
    # record the time of every iteration.
    timer=dict(type=IterTimerHook),
    # print log every 10 iterations.
    logger=dict(type=LoggerHook, log_metric_by_epoch=False, interval=10),
    # enable the parameter scheduler.
    param_scheduler=dict(type=ParamSchedulerHook),
    # save checkpoint per save_steps.
    checkpoint=dict(
        type=CheckpointHook,
        by_epoch=False,
        interval=save_steps,
        max_keep_ckpts=save_total_limit),
    # set sampler seed in distributed evrionment.
    sampler_seed=dict(type=DistSamplerSeedHook),
)

# configure environment
env_cfg = dict(
    # whether to enable cudnn benchmark
    cudnn_benchmark=False,
    # set multi process parameters
    mp_cfg=dict(mp_start_method=&#x27;fork&#x27;, opencv_num_threads=0),
    # set distributed parameters
    dist_cfg=dict(backend=&#x27;nccl&#x27;),
)

# set visualizer
visualizer = None

# set log level
log_level = &#x27;INFO&#x27;

# load from which checkpoint
load_from = None

# whether to resume training from the loaded checkpoint
resume = False

# Defaults to use random seed and disable deterministic
randomness = dict(seed=None, deterministic=False)

# set log processor
log_processor = dict(by_epoch=False)
```

1.  接下来对新的配置直接开始训练：

```
xtuner train ./internlm2_chat_7b_qlora_oasst1_e3_copy.py  --deepspeed deepspeed_zero2
```

训练结束后，所有权重文件放置在训练目录下的 work_dirs 中，目录大致为：

```
drwxr-xr-x 3 root root       4096 May  2 12:23 20240502_122337/
-rw-r--r-- 1 root root       6413 May  2 12:24 internlm2_chat_7b_qlora_oasst1_e3_copy.py
-rw-r--r-- 1 root root 1886589762 May  2 12:43 iter_500.pth
-rw-r--r-- 1 root root 1886601474 May  2 12:50 iter_657.pth
-rw-r--r-- 1 root root         76 May  2 12:50 last_checkpoint
```

需要注意的是，通常只需要微调一轮就好，原因是 LLM 通常是过目不忘（有很多相关研究），容易过拟合。

> 如果你想查看更多超参数对结果带来的变动，请参考 《LoRA 和 QLoRA 微调语言大模型：数百次实验后的见解》https://zhuanlan.zhihu.com/p/664912829

## **效果验证**

### **命令行**

1.  先把微调后的模型转换格式为hf，再与原模型合并，这里提供了统一脚本：

```
HF_OUTPUT_DIR=&quot;./hf&quot; # lora转为hf格式后的输出地址
MERGE_OUTPUT_DIR=&quot;./merge&quot; # 与原模型合并后的输出地址
SCRIPT_PATH=&quot;./internlm2_chat_7b_qlora_oasst1_e3_copy.py&quot; # 训练配置文件
SRC_MODEL_PATH=&quot;/home/model_temp/Shanghai_AI_Laboratory/internlm2-chat-7b&quot; # 原模型地址
WEIGHTS_PATH=&quot;/home/finetune/work_dirs/internlm2_chat_7b_qlora_oasst1_e3_copy/iter_150.pth&quot; # lora权重地址
SYSTEM_PROMPT=&quot;你现在是一个送祝福大师，帮我针对不同人和事情、节日送对应的祝福&quot; # 默认system prompt

rm -rf $HF_OUTPUT_DIR
rm -rf $MERGE_OUTPUT_DIR
mkdir -p $HF_OUTPUT_DIR
mkdir -p $MERGE_OUTPUT_DIR

xtuner convert pth_to_hf &quot;${SCRIPT_PATH}&quot; &quot;${WEIGHTS_PATH}&quot; &quot;${HF_OUTPUT_DIR}&quot;
xtuner convert merge \
    &quot;${SRC_MODEL_PATH}&quot; \
    &quot;${HF_OUTPUT_DIR}&quot; \
    &quot;${MERGE_OUTPUT_DIR}&quot; \
    --max-shard-size &quot;2GB&quot;
```

如果这步报错，请检查 WEIGHTS_PATH 是否正确。

1.  启动对话：

```
# 如果想要多样性更强加上 --temperature 1
xtuner chat ./merge --prompt-template internlm2_chat --system &quot;你现在是一个送祝福大师，帮我针对不同人和事情、节日送对应的祝福&quot; --temperature 0.7 
```

此时你会看到如下显示，只需要输入之前的 prompt 和触发风格即可验证。为了去除上下文影响，记得每次输入新提示前需要输入 RESET 进行重置。

```
double enter to end input (EXIT: exit chat, RESET: reset history) &gt;&gt;&gt; 祝弟弟工作升职 ,小红书风格

弟弟呀，你的才华如同春日里的花朵，绽放出绚烂的光彩。工作如鱼得水，升职之路顺顺利利，愿你继续闪耀，成为职场的明星！加油哦！🌟🌈&lt;|im_end|&gt;

double enter to end input (EXIT: exit chat, RESET: reset history) &gt;&gt;&gt; RESET

Log: History responses have been removed!

double enter to end input (EXIT: exit chat, RESET: reset history) &gt;&gt;&gt; 我想送哥哥中秋节祝福,小红书风格

🌕🎉 哥，中秋月圆人团圆，愿你事业如月圆，家庭幸福似团圆，快乐满怀，幸福满满！🌟&lt;|im_end|&gt;

double enter to end input (EXIT: exit chat, RESET: reset history) &gt;&gt;&gt; RESET

Log: History responses have been removed!

double enter to end input (EXIT: exit chat, RESET: reset history) &gt;&gt;&gt; 我想送赵老师生日祝福,严肃风格

尊敬的赵老师，值此佳辰，恭祝您福寿安康，事业蒸蒸日上。愿您在未来的岁月里，如同春日之花，绽放出更加绚烂的光彩，为莘莘学子树立起崇高的榜样。愿您的生活如同诗篇般美好，每一天都充满着智慧与喜悦。愿您的工作如同乐章般动听，每一个音符都承载着对教育事业的热爱与执着。在此，衷心祝愿您生日快乐，万事胜意！&lt;|im_end|&gt;
```

### **网页 Demo**

为了方便测试，我们还可以使用 **streamlit** 制作一个网页版的demo，方法如下：

1.  先安装下载必要组件和依赖：

```
pip install streamlit==1.24.0`

# 找地方clone仓库
git clone https://github.com/InternLM/InternLM.git
```

1.  将 InternLM/web_demo.py 中 29 行和 33 行的模型路径更换为上文存放 Merge 后参数的路径 /home/finetune/merge

1.  按照如下命令运行 InternLM 目录下的 web_demo.py 文件即可, ：

```
streamlit run /root/personal_assistant/code/InternLM/web_demo.py --server.address 127.0.0.1 --server.port 6006
```

通过以上步骤，你就可以成功地微调出最初版本的天机送祝福模块并成功运行推理。

制造数据的质量仍有许多改善空间，如回答随机性注入、输入 input 的随机性加强等等。期待大家加入，一起来继续完善数据的制造 pipeline。送祝福完全可以变得更好、更智能、更通用，从而真正更接近中文下的使用场景，让LLM 对话更富有“人情味”！

最后，感谢你的阅读，祝福你有美好的一天。