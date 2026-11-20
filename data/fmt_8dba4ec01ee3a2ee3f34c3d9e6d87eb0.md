
# DeepSeek R1 API调用指南

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/Zns2dxpPkooAqmxGdGjcbqrRnCc](https://kq4b3vgg5b.feishu.cn/docx/Zns2dxpPkooAqmxGdGjcbqrRnCc)

## 课程说明

体验课内容节选自《2025大模型Agent智能体开发实战》完整版付费课程

体验课时间有限，若想深度学习大模型技术，欢迎大家报名由我主讲的《2025大模型Agent智能体开发实战》：

![](img/397fd7fcf323db756f81bf732d562cdb.png)

![](img/aa3031a3b998795818553ff764cea3a8.png)

此外，公开课全套学习资料，已上传至网盘（https://pan.baidu.com/s/11haiNlTgDxa0Iu6TWRRtGg?pwd=rnf5）

需要更系统深入学习大模型可扫码⬆️添加助教咨询喔～

---

## 《2025大模型Agent智能体开发实战》体验课

## DeepSeek R1 API调用指南

### DeepSeek R1账号注册与API获取

DeepSeek-R1正式版已于2025年1月20号正式上线，在后训练阶段大规模使用了强化学习技术，在仅有极少标注数据的情况下，极大提升了模型推理能力。在数学、代码、自然语言推理等任务上，性能比肩 OpenAI o1 正式版。

![](img/35f1f2fa923e3f7f71ef0368d50641a1.png)

DeepSeek R1模型性能解读视频：https://www.bilibili.com/video/BV1UBwbe4E1D/

![](img/f2870ef1b426c3581f225ab067f4e94b.png)

DeepSeek官网：https://www.deepseek.com/

![](img/98faebd573838163634e7cbf5986daad.png)

![](img/118f02f17c30b991f4443fa31e129f8d.png)

新用户注册即赠送10元额度，约500万token额度。

价格方面，DeepSeek R1价格约为OpenAI o1正式版模型的1/50：

![](img/26e8635238a949537af3d3af8c6f2853.png)

以下是OpenAI o1模型API调用价格：

![](img/0fd9c2adb97263e97e4e7b35d20b8524.png)

目前DeepSeek R1模型调用不限速：

![](img/1a49fb73a1fd3e2ebc8c81630c7206ba.png)

并且调用风格和OpenAI完全一致：提示词缓存、Json Output等功能完全相同，暂不支持多模态和Function calling功能：

![](img/7e95900a497cedf9142af53fbf0449e8.png)

![](img/fce678dcff0cf8ce423a4de10fe1fcdb.png)

### DeepSeek R1调用流程

对比OpenAI GPT4o调用流程：

```
!pip install openai
```

```
Requirement already satisfied: openai in /root/anaconda3/lib/python3.12/site-packages (1.57.2)
Requirement already satisfied: anyio<5,>=3.5.0 in /root/anaconda3/lib/python3.12/site-packages (from openai) (4.2.0)
Requirement already satisfied: distro<2,>=1.7.0 in /root/anaconda3/lib/python3.12/site-packages (from openai) (1.9.0)
Requirement already satisfied: httpx<1,>=0.23.0 in /root/anaconda3/lib/python3.12/site-packages (from openai) (0.28.1)
Requirement already satisfied: jiter<1,>=0.4.0 in /root/anaconda3/lib/python3.12/site-packages (from openai) (0.8.2)
Requirement already satisfied: pydantic<3,>=1.9.0 in /root/anaconda3/lib/python3.12/site-packages (from openai) (2.5.3)
Requirement already satisfied: sniffio in /root/anaconda3/lib/python3.12/site-packages (from openai) (1.3.0)
Requirement already satisfied: tqdm>4 in /root/anaconda3/lib/python3.12/site-packages (from openai) (4.66.4)
Requirement already satisfied: typing-extensions<5,>=4.11 in /root/anaconda3/lib/python3.12/site-packages (from openai) (4.11.0)
Requirement already satisfied: idna>=2.8 in /root/anaconda3/lib/python3.12/site-packages (from anyio<5,>=3.5.0->openai) (3.7)
Requirement already satisfied: certifi in /root/anaconda3/lib/python3.12/site-packages (from httpx<1,>=0.23.0->openai) (2024.6.2)
Requirement already satisfied: httpcore==1.* in /root/anaconda3/lib/python3.12/site-packages (from httpx<1,>=0.23.0->openai) (1.0.7)
Requirement already satisfied: h11<0.15,>=0.13 in /root/anaconda3/lib/python3.12/site-packages (from httpcore==1.*->httpx<1,>=0.23.0->openai) (0.14.0)
Requirement already satisfied: annotated-types>=0.4.0 in /root/anaconda3/lib/python3.12/site-packages (from pydantic<3,>=1.9.0->openai) (0.6.0)
Requirement already satisfied: pydantic-core==2.14.6 in /root/anaconda3/lib/python3.12/site-packages (from pydantic<3,>=1.9.0->openai) (2.14.6)
[33mWARNING: Running pip as the 'root' user can result in broken permissions and conflicting behaviour with the system package manager. It is recommended to use a virtual environment instead: https://pip.pypa.io/warnings/venv[0m[33m
[0m
```

```
import openai
```

```
openai.__version__
```

```
'1.57.2'
```

```
from openai import OpenAI
```

```
openai_api_key = 'YOUR_API_KEY'
```

```
# 实例化客户端
client = OpenAI(api_key=openai_api_key, 
                base_url="https://ai.devtool.tech/proxy/v1")
```

```
response = client.chat.completions.create(
    model="o1-preview",
    messages=[
        {"role": "user", "content": "请问，9.8和9.11哪个更大？"}
    ]
)
```

```
response
```

```
ChatCompletion(id='chatcmpl-As3CtGiIhcFVMpNEW9H170lzm1xVa', choices=[Choice(finish_reason='stop', index=0, logprobs=None, message=ChatCompletionMessage(content='9.8 比 9.11 大。\n\n在比较这两个数字时，9.8 等于 9.80，而 9.11 等于 9.11。因为 9.80 大于 9.11，所以 9.8 比 9.11 大。', refusal=None, role='assistant', audio=None, function_call=None, tool_calls=None))], created=1737445403, model='o1-preview-2024-09-12', object='chat.completion', service_tier='default', system_fingerprint='fp_6722463bcd', usage=CompletionUsage(completion_tokens=592, prompt_tokens=21, total_tokens=613, completion_tokens_details=CompletionTokensDetails(accepted_prediction_tokens=0, audio_tokens=0, reasoning_tokens=512, rejected_prediction_tokens=0), prompt_tokens_details=PromptTokensDetails(audio_tokens=0, cached_tokens=0)))
```

```
print(response.choices[0].message.content)
```

```
9.8 比 9.11 大。

在比较这两个数字时，9.8 等于 9.80，而 9.11 等于 9.11。因为 9.80 大于 9.11，所以 9.8 比 9.11 大。
```

上述代码中 `model` 参数指定了使用的模型（`o1-preview`），`messages` 列表定义了对话内容，其中 `role` 为 `user` 表示用户的输入内容。

DeepSeek R1调用流程:

首先在官网申请`API-KEY`：https://platform.deepseek.com/api_keys

![](img/34221be832e04cd2414dd37f327b8fa6.png)

![](img/29ca7ca49f6cd1b73428fd1fc16fbc3c.png)

```
ds_api_key = "YOUR_DS_API_KEY"
```

```
# 实例化客户端
client = OpenAI(api_key=ds_api_key, 
                base_url="https://api.deepseek.com")
```

```
# 调用 deepseekv3 模型
response = client.chat.completions.create(
    model="deepseek-reasoner",
    messages=[
        {"role": "user", "content": "请问，9.8和9.11哪个更大？"}
    ]
)
```

```
# 最终回复
response.choices[0].message.content
```

```
'在比较数值大小时，9.8 和 9.11 需要统一小数位数来逐位对比：\n\n- **9.8** 可以写作 **9.80**  \n- **9.11** 保持原格式 **9.11**\n\n**比较步骤：**  \n1. **整数部分**：均为 9，相等。  \n2. **十分位**（小数点后第一位）：9.80 的 **8** > 9.11 的 **1**。\n\n此时已能判定大小，无需继续比较后续位数。\n\n**结论：**\n\n**9.8 > 9.11**\n\n注意：如果是版本号（如软件版本），可能遵循不同规则（如 `9.11` 表示第9版第11次更新，大于 `9.8`），但纯数学比较时，**9.8 更大**。'
```

```
# 思考链
response.choices[0].message.reasoning_content
```

```
'嗯，用户问的是9.8和9.11哪个更大。这看起来像是一个小数比较的问题，但可能有一些细节需要注意。首先，我应该明确这两个数的位数。9.8和9.11，表面上看起来，可能有人会直接比较小数点后的数字，比如8和11，然后觉得11更大，所以9.11更大。不过其实这可能是个误区，因为小数的比较需要逐位来看。\n\n首先，我要确认这两个数的整数部分是否相同。是的，都是9，所以接下来比较小数部分。对于9.8，可以写成9.80，这样小数部分有两位，和9.11对齐。这样的话，小数部分分别是80和11。这时候比较的话，80显然比11大，所以9.80（即9.8）大于9.11。可能用户会混淆小数点的位数，比如误以为9.11的小数点后两位比9.8的一位大，但其实需要补齐位数再比较。\n\n或者，用户可能是在不同的上下文中使用这些数字，比如版本号，这时候9.11可能代表的是第九版的第11次更新，而9.8则是第九版的第8次，那么在这种情况下，9.11确实更大。但通常数学比较中，还是按照数值大小来判断。因此需要确认用户的意图，但根据问题本身，可能更偏向数值比较。\n\n所以，正确的做法是将两个数的小数位数统一，比较每一位的数字，得出9.8更大的结论。不过要确保用户没有特殊的上下文，比如软件版本或其他非数值比较的情况。如果没有特别说明，数学上9.8确实大于9.11。'
```

### DeepSeek R1参数解释

(1). `max_tokens`：最终回答的最大长度（不含思维链输出），默认为 4K，最大为 8K。请注意，思维链的输出最多可以达到 32K tokens，控思维链的长度的参数（`reasoning_effort`）将会在近期上线。

(2). `reasoning_content`：思维链内容，与 `content` 同级

(3). 上下文长度：API 最大支持 64K 上下文，输出的 `reasoning_content` 长度不计入 64K 上下文长度中

(4). 支持的功能：对话补全，对话前缀续写 (Beta)

(5). 不支持的功能：Function Call、Json Output、FIM 补全 (Beta)

(6). 不支持的参数：`temperature`、`top_p`、`presence_penalty`、`frequency_penalty`、`logprobs`、`top_logprobs`。请注意，为了兼容已有软件，设置这些参数不会报错，但也不会生效。

### DeepSeek R1思维链执行过程

![](img/84bd4d990e8b46063da8ed6f21172e9e.png)

### 尝试借助DeepSeek R1编写完整开发项目
