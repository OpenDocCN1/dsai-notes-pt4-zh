
# 大模型 API 配置

> 来源：[https://deepwisdom.feishu.cn/docx/DYk2dpGgfoQkzexE78fcrW1Qn0e](https://deepwisdom.feishu.cn/docx/DYk2dpGgfoQkzexE78fcrW1Qn0e)

> 本节中，我们将学习如何配置大模型 API，为智能体注入灵魂

## OpenAI API

我们以 OpenAI API 为例说明配置过程，其他大模型的 API 配置过程是相同的。

### 使用 config2.yaml

> 如果您按照 [安装](https://deepwisdom.feishu.cn/wiki/C2H3wIluSiVPClkPddqcVDyOnzf) 中的 `git clone` 方法，则 `config/config2.yaml` 文件已存在。

> 只需编辑它或创建一个名为 `~/.metagpt/config2.yaml` 的副本进行编辑。这样您就不会意外地使用 git 提交和共享您的 API 密钥。

> MetaGPT 将按照以下优先顺序读取您的配置：

> `~/.metagpt/config2.yaml` > `config/config2.yaml`

1. （如果没有）在当前工作目录中创建一个名为 `config` 的文件夹，并在其中添加一个名为 `config2.yaml` 的新文件。

1. 将示例 [`config2.yaml`](https://github.com/geekan/MetaGPT/blob/main/config/config2.yaml) 文件的内容复制到您的新文件中。

1. 在 yaml 文件中填入：

```yaml
llm:
  api_type: 'openai' # or azure / ollama / groq etc. Check LLMType for more options
  model: 'gpt-4-turbo' # or gpt-3.5-turbo
  base_url: 'https://api.openai.com/v1' # or forward url / other llm url
  api_key: 'YOUR_API_KEY'
  # proxy: 'YOUR_LLM_PROXY_IF_NEEDED' # Optional. If you want to use a proxy, set it here.
  # pricing_plan: 'YOUR_PRICING_PLAN' # Optional. If your pricing plan uses a different name than the `model`.
```

结果将类似如下：

![](img/cd830aa64864a0dd312214efb31d0303.png)

### OpenAI o1 模型配置

```yaml
llm:
  api_type: 'openai'
  api_key: 'sk-...'
  model: 'o1-mini'
  use_system_prompt: false
  stream: false
```

### 测试效果

可以创建一个文件并运行如下代码，若终端中打印出回复，则说明配置成功：

```python
from metagpt.llm import LLM
import asyncio
llm = LLM()
asyncio.run(llm.aask("你是什么模型"))
```

MetaGPT 还支持各种 LLM 模型，可以根据您的需求配置模型 API 的密钥。

## Anthropic / Claude API

支持模型名列表在 `token_counter.py` 的 `claude-*`

```yaml
llm:
  api_type: 'claude' # 或 anthropic
  base_url: 'https://api.anthropic.com'
  api_key: 'YOUR_API_KEY'
  model: 'claude-3-opus-20240229'
```

## 智谱 API

访问 [`llm_config.py`](https://github.com/geekan/MetaGPT/blob/main/metagpt/configs/llm_config.py) 获得更新详情。

```yaml
llm:
  api_type: 'zhipuai'
  api_key: 'YOUR_API_KEY'
  model: 'MODEL_NAME'
```

## 科大讯飞的大模型 Spark API

访问 [`llm_config.py`](https://github.com/geekan/MetaGPT/blob/main/metagpt/configs/llm_config.py) 获得更新详情。星火认知大模型 Web API 文档 | 讯飞开放平台文档中心

```yaml
llm:
  api_type: 'spark'
  app_id: 'YOUR_APPID'
  api_key: 'YOUR_API_KEY'
  api_secret: 'YOUR_API_SECRET'
  domain: 'generalv3.5'
  base_url: 'ws(s)://spark-api.xf-yun.com/v3.5/chat'
```

## Azure OpenAI API

访问 [`llm_config.py`](https://github.com/geekan/MetaGPT/blob/main/metagpt/configs/llm_config.py) 获得更新详情。

```yaml
llm:
  api_type: 'azure'
  base_url: 'YOUR_AZURE_BASE_URL'
  api_key: 'YOUR_API_KEY'
  api_version: 'YOUR_API_VERSION' # such as '2024-03-01-preview'
  model: 'YOUR_MODEL_NAME' # or YOUR_DEPLOYMENT_NAME
```

## 谷歌 Gemini

默认支持模型 `gemini-pro`

```yaml
llm:
  api_type: 'gemini'
  api_key: 'YOUR_API_KEY'
  model: 'gemini-pro'
```

## 百度 千帆 API

支持 `ERNIE-*` 模型和部分主流开源模型，见 `token_counter.py`

### 使用 model 进行调用

[如何获取 Access Key](https://cloud.baidu.com/doc/Reference/s/9jwvz2egb)

```yaml
llm:
  api_type: 'qianfan'
  access_key: 'YOUR_ACCESS_KEY'
  secret_key: 'YOUR_SECRET_KEY'
  model: 'ERNIE-4.0-8K-0329'
```

### 使用 endpoint 进行调用

```yaml
llm:
  api_type: 'qianfan'
  access_key: 'YOUR_ACCESS_KEY'
  secret_key: 'YOUR_SECRET_KEY'
  endpoint: 'YOUR_ENDPOINT_NAME like ernie_speed'
```

## 阿里云 灵积 DashScope API

支持 `qwen-*` 模型和部分主流开源模型，见 `token_counter.py`

```yaml
llm:
  api_type: 'dashscope'
  api_key: 'YOUR_API_KEY'
  model: 'qwen2.5-coder-32b-instruct'
```

## 月之暗面 Moonshot / Kimi API

支持 `moonshot-v1-*` 模型，见 `token_counter.py`

```yaml
llm:
  api_type: 'moonshot'
  base_url: 'https://api.moonshot.cn/v1'
  api_key: 'YOUR_API_KEY'
  model: 'moonshot-v1-8k'
```

## Fireworks API

支持 Fireworks models 模型，见 `token_counter.py`

```yaml
llm:
  api_type: 'fireworks'
  base_url: 'https://api.fireworks.ai/inference/v1'
  api_key: 'YOUR_API_KEY'
  model: 'accounts/fireworks/models/llama-v2-13b-chat'
```

## Mistral API

支持 Mistral models 模型，见 `token_counter.py`

```yaml
llm:
  api_type: 'mistral'
  base_url: 'https://api.mistral.ai/v1'
  api_key: 'YOUR_API_KEY'
  model: 'open-mixtral-8x7b'
```

## Yi / 零一万物 API

支持模型名列表，见 `token_counter.py`

```yaml
llm:
  api_type: 'yi'
  base_url: 'https://api.lingyiwanwu.com/v1'
  api_key: 'YOUR_API_KEY'
  model: 'yi-large'
  max_token: 4000
```

## ollama API

```yaml
llm:
  api_type: 'ollama'
  base_url: 'http://127.0.0.1:11434/api'
  model: 'llama2'
```

## Openrouter

### WizardLM-2 8x22B

```yaml
llm:
  api_type: 'openrouter'
  base_url: 'https://openrouter.ai/api/v1'
  api_key: 'sk...'
  model: microsoft/wizardlm-2-8x22b
```

### Llama 3 70B Instruct

```yaml
llm:
  api_type: 'openrouter'
  base_url: 'https://openrouter.ai/api/v1'
  api_key: 'sk...'
  model: meta-llama/llama-3-70b-instruct:nitro
```

### Claude 3.5 Sonnet

```yaml
llm:
  api_type: 'openrouter'
  base_url: 'https://openrouter.ai/api/v1'
  api_key: 'sk...'
  model: "anthropic/claude-3.5-sonnet"
```

## [groq](https://groq.com/)

### llama3-70b

```yaml
llm:
  # Visit https://console.groq.com/keys to create api key
  base_url: "https://api.groq.com/openai/v1"
  api_key: "YOUR_API_KEY"
  model: "llama3-70b-8192"  # llama3-8b-8192，llama3-70b-8192，llama2-70b-4096 ，mixtral-8x7b-32768，gemma-7b-it
```

## Amazon Bedrock API

```yaml
llm:
  api_type: 'bedrock'
  model: 'meta.llama3-70b-instruct-v1:0'
  region_name: 'REGION' # e.g. us-east-1
  access_key: 'YOUR_ACCESS_KEY'
  secret_key: 'YOUR_SECRET_KEY'
```

## [DeepSeek](https://platform.deepseek.com/)

DeepSeek API 使用与 OpenAI 兼容的 API 格式，通过修改配置，您可以使用 OpenAI SDK 来访问 DeepSeek API，或使用与 OpenAI API 兼容的软件。

```yaml
llm:
  api_type: 'openai'
  model: 'deepseek-chat' # or deepseek-chat / deepseek-coder
  base_url: 'https://api.deepseek.com/v1'
  api_key: 'sk-'
```

注意：`deepseek-chat` 后端模型已更新为 `DeepSeek-V2`，无需修改模型名称即可访问 `DeepSeek-V2`

## [豆包/讯飞](https://console.volcengine.com/ark/region:ark+cn-beijing/model)

```yaml
llm:
  api_type: "ark"
  model: "ep-"  # your model endpoint like ep-xxx
  base_url: "https://ark.cn-beijing.volces.com/api/v3"
  api_key: "" # your api-key……
```

## 硅基流动/siliconflow

```yaml
llm:
  api_type: "openai"
  model: "Model_Name"  # or deepseek-ai/DeepSeek-V2.5
  base_url: "https://api.siliconflow.cn/v1"
  api_key: "YOUR_API_KEY"
```

## What's Next!

+ 试试 [“多智能体软件公司”](https://deepwisdom.feishu.cn/wiki/RjugweZ2liBiNokCRSwcztKFnjh)，一句话生成项目仓库

+ [体验 MetaGPT 另一个核心功能 “数据解释器”](https://deepwisdom.feishu.cn/wiki/Praxw5RxEi4NgGkrofJc54QdnXg)，让 AI 自动化完成数据科学、网页爬取等任务。

+ 如果想体验 [“研究调研员”](https://deepwisdom.feishu.cn/wiki/NTenwMLT0i68X5kEuzjce4K8ncg) 智能体，还需要进行进一步的 [工具配置](https://deepwisdom.feishu.cn/wiki/MFXBwkOVmiLrawk36TdchChKnFd)
