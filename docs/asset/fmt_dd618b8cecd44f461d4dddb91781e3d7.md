
+ 预期： 模型不再说是 DeepSeek，而是可能会根据 Alpaca 数据集的内容，回答自己是一个 AI 助手，或者语气发生变化。

+ 测试 2：指令遵循 （Instruction）

+ 输入：请把“自动驾驶”写成一首藏头诗。

+ 预期： 微调后的模型通常能更严格地遵守格式要求。

+ 测试 3：反向验证 （Reasoning Degradation）

+ 输入：鸡兔同笼，头35脚94，各多少？ 或复杂的数学推理题。

+ 预期： 你可能会发现 DeepSeek-R1 引以为傲的 `<think>` 思考过程消失了，甚至做错了。

+ 教学点： 这说明用简单的对话数据微调推理模型，会导致“灾难性遗忘”。若要保留智商，需使用 OpenR1 等包含思维链的数据集。

![](img/7a32021f13228d57608bec8f2baf3a16.png)

---

### 4.3 （选做）：批量评估与指标计算 （Evaluate）

如果你希望用科学指标（如 BLEU/ROUGE 分数）来量化模型的训练效果，或者想批量查看模型的输出结果，可以使用此功能。

**操作区域：** Web UI -> Evaluate & Predict （评估与预测） 选项卡

**1. 基础配置**

+ `检查点路径` （Checkpoint Path）: 必须选择 你训练好的 Checkpoint（不要选空，否则测的是原版模型）。

+ **数据集：** 选择 `alpaca_zh`。

**2. 关键参数 （极速设置）**

为了演示速度，我们只测 50 条数据：

+ 找到页面中部的 `对话模板` （Template） 下拉框。将其从 `deepseek3` 改为 `qwen`。

+ `截断长度`： 1024

+ `最大样本数` （Max Samples）: 50 （强烈建议改成 50 或 100，填 1000 会跑很久）

+ `批处理大小` （Batch Size）: 8 （推理比训练省显存，可以开大点）

+ `保存预测结果`： ✅ （这样能看到模型生成的具体文本）

![](img/f1d3d9003a1b80d69949aaaca7b4a2ff.png)

**3. 执行与观察**

+ 点击 **“开始 （Start）”**。

+ **观察控制台/终端：** 进度条跑完后，会打印出类似 `{'BLEU-4': 45.2, 'ROUGE-L': 58.1}` 的分数。

+ BLEU/ROUGE 分数越高，说明模型生成的答案越接近标准答案。

+ **查看结果文件：**

+ 进入 输出目录（界面上显示的路径，如 `eval_2026-xx-xx...`）。

+ 打开 `generated_predictions.jsonl`，你可以看到每一道题模型的回答 vs 标准答案的对比。

## 5. 模型导出 （Export）

**操作区域：** Web UI -> **Export （导出）** 选项卡

如果你想把这个模型打包带走（例如放到 Ollama 里跑），需要进行导出。

1. `检查点路径`： 选择刚才训练好的 Checkpoint。

1. `导出目录`： 输入一个路径，例如 `/root/my_first_model`。

1. 点击 “开始导出 （Start Export）”。

+ 系统会将 LoRA 的外挂参数融合进基础模型，生成一个完整的模型文件。

![](img/50b656bea2cb3b6634dde437c5c663fa.png)

终端上的结果

```
[INFO|2026-01-25 04:27:36] llamafactory.model.model_utils.attention:157 >> Using torch SDPA for faster training and inference.
[INFO|2026-01-25 04:27:43] llamafactory.model.adapter:157 >> Merged 1 adapter(s).
[INFO|2026-01-25 04:27:43] llamafactory.model.adapter:157 >> Loaded adapter(s): saves/DeepSeek-R1-1.5B-Distill/lora/train_2026-01-25-04-15-43
[INFO|2026-01-25 04:27:43] llamafactory.model.loader:157 >> all params: 1,777,088,000
[INFO|2026-01-25 04:27:43] llamafactory.train.tuner:157 >> Convert model dtype to: torch.bfloat16.
[INFO|configuration_utils.py:420] 2026-01-25 04:27:43,048 >> Configuration saved in /root/my_first_model/config.json
[INFO|configuration_utils.py:909] 2026-01-25 04:27:43,049 >> Configuration saved in /root/my_first_model/generation_config.json
[INFO|modeling_utils.py:2988] 2026-01-25 04:27:49,234 >> Model weights saved in /root/my_first_model/model.safetensors
[INFO|tokenization_utils_base.py:2491] 2026-01-25 04:27:49,234 >> tokenizer config file saved in /root/my_first_model/tokenizer_config.json
[INFO|tokenization_utils_base.py:2500] 2026-01-25 04:27:49,234 >> Special tokens file saved in /root/my_first_model/special_tokens_map.json
[INFO|2026-01-25 04:27:49] llamafactory.train.tuner:157 >> Saved ollama modelfile to /root/my_first_model.
```

---

好的，这是为您优化后的第六节：模型服化部署 （API）。

我已经修正了命令中的模板参数（改为 `qwen`），并添加了必要的原理解释，格式上也与之前的文档保持一致，可以直接插入到完整手册的末尾。

---

## 6. 模型服化部署 （API）

**操作区域：** 终端 （Terminal）

**模型导出**只是第一步，要让它真正产生价值，我们需要把它变成一个在线服务。LLaMA Factory 内置了 API Server 功能，可以启动一个完全兼容 OpenAI 接口规范的服务端。这意味着，你可以直接用任何支持 OpenAI 的软件（如 LangChain、AutoGPT 或 Python 代码）来调用你亲手训练的模型。

### 6.1 释放显存 （必要步骤）

由于显卡显存有限，我们必须先关闭之前的训练界面，腾出资源给推理服务使用。

+ **动作：** 在运行 Web UI 的终端窗口中，按下 `Ctrl + C`，确保之前的服务已停止。

### 6.2 启动 API 服务

复制并运行以下命令。

> **💡 关键知识点：** 为什么模板是 `qwen`？

> 虽然模型叫 DeepSeek，但我们使用的是 DeepSeek-R1-Distill-Qwen 版本。它的底层架构和对话格式其实是基于 Qwen（通义千问）的。因此，指定 `--template qwen` 才能让模型正确理解用户的话。

```
CUDA_VISIBLE_DEVICES=0 llamafactory-cli api \
    --model_name_or_path /root/my_first_model \
    --template qwen \
    --infer_backend huggingface \
    --vllm_enforce_eager false
```

+ **成功标志：**

当终端显示 `Uvicorn running on http://0.0.0.0:8000` 时，说明服务已启动，正在监听 `8000` 端口。

（注意：该终端窗口会被占用，请不要关闭它。）

### 6.3 调用测试

保持上面的服务窗口开启，**新建一个终端窗口**，通过以下两种方式验证服务是否可用。

#### 方式 A：命令行快速测试 （Curl）

这是最直接的测试方法，模拟发送一个 HTTP 请求。

```
curl http://localhost:8000/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gpt-3.5-turbo",
    "messages": [
      {"role": "user", "content": "你好，你是谁？"}
    ]
  }'
```

（注：这里的 `"model": "gpt-3.5-turbo"` 只是占位符，服务器实际调用的就是你加载的 DeepSeek 模型。）

+ **预期输出：** 你将看到一段 JSON 格式的回复，其中 `content` 字段包含模型的回答。

#### 方式 B：Python 代码集成 （应用开发）

这是开发实际 AI 应用（如聊天机器人）的标准写法。需先运行 `pip install openai`。

```
from openai import OpenAI

# 指向本地 API，无需真实 Key
client = OpenAI(api_key="empty", base_url="http://localhost:8000/v1")

print("模型正在思考...")
response = client.chat.completions.create(
    model="deepseek",
    messages=[{"role": "user", "content": "请把'机器学习'写成一首藏头诗。"}],
    temperature=0.7
)
print(response.choices[0].message.content)
```

好的，这是为您格式化好的 **6.3 C 项** 内容。我保持了与实验手册一致的排版风格，强调了关键配置项，并加入了针对 AutoDL 环境的连接提示。

您可以直接复制以下内容，粘贴到实验手册的 6.3 调用测试 章节末尾。

---

#### 方式 C：图形化客户端集成 （Cherry Studio）

如果你希望在本地电脑上像使用 ChatGPT 一样与你的私有模型对话，可以使用支持 OpenAI 兼容协议 的客户端软件（如 Cherry Studio、Chatbox 等）。

**1. 准备配置信息**

在配置客户端之前，请先记录以下三个核心参数：

+ `服务类型`： OpenAI Compatible （OpenAI 兼容）

+ `API 域名` （Base URL）: `http://117.50.198.63:8000/v1`

+ 注意：必须以 `/v1` 结尾；请确保 IP 地址是你服务器的公网 IP。

+ `API 密钥` （API Key）: `sk-123456`

+ 注意：本地服务通常不校验密钥，但客户端不允许为空，随便填一个 `sk-` 开头的字符串即可。

+ `模型名称` （Model ID）: `deepseek`

2. 在 Cherry Studio 中配置

1. 打开 Cherry Studio，点击左下角的 设置 （Settings） 图标。

1. 选择 模型服务 （Model Providers） -> OpenAI Compatible。

1. 点击 **添加** ，填入上述的 `API 域名` 和 `API 密钥`。

1. 在下方的 `模型列表` 区域，点击 添加模型，在 ID 栏填入 `deepseek`，保存设置。

1. 点击 “检查” 按钮，如果显示绿色连通标志，说明连接成功。

**3. 开始对话**

回到聊天主界面，切换模型为刚才添加的 `deepseek`，即可开始远程调用你的私有大模型进行对话。

> **💡 网络连接提示：**

> 如果配置后显示“连接超时”或“失败”，通常是因为服务器端口被云平台的安全组拦截。

+ 请确保云服务器控制台（如阿里云/腾讯云/AutoDL）的安全组规则中，入站规则 已放行 `TCP:8000` 端口。

+ 如果是 AutoDL 默认环境，建议使用 SSH 隧道映射端口到本地，或者检查其特定的端口开放策略。

## 📚 附录：

### 云实例连接与防火墙配置指南 （CompShare 版）

本实验基于 CompShare （恒源云） 算力平台。为了能从本地浏览器直接访问实验界面和 API，我们需要进行 SSH 连接并配置安全组防火墙。

#### A. 获取实例登录信息

1. 登录控制台，进入 [我的实例] 列表。

![](img/6220a430c9b3ee24840df24c66808a0b.png)

1. 找到正在运行的实例，复制以下核心信息：

+ `公网 IP` （例如 `117.50.198.63`）

+ `SSH 端口` （通常不是 22，例如 `23`）

+ `登录密码` （点击“复制密码”或“重置密码”）

#### B. 使用 SSH 连接实例 （Terminal）

此步骤用于执行实验手册中的训练命令。

1. 打开本地电脑的终端 （Windows PowerShell 或 Mac/Linux Terminal）。

1. 输入登录命令（请替换为你实际的端口和 IP）：

```
# 格式：ssh -p [SSH端口] root@[公网IP]
ssh -p 23 root@117.50.198.63
```

1. 按回车后，输入实例密码（注意：输入密码时屏幕不会显示任何字符，输完直接回车）。

1. 当出现 `root@...:~#` 提示符时，表示连接成功。

#### C. 开放防火墙端口 （关键步骤）

此步骤用于解决“浏览器打不开界面”的问题。我们需要允许外部网络访问 LLaMA Factory 的 WebUI （`7860`） 和 API （`8000`）。

1. 在控制台实例列表中，点击实例名称进入 [更多操作] 页面。

1. 点击顶部的 [配置防火墙] 选项卡。

![](img/a8949d021835a8c560cb9b21174e0838.png)

1. 点击 [编辑防火墙规则] 按钮，然后在防火墙规则窗口中，点击 [添加规则] 依次添加以下两条规则：

| 端口范围 | 协议 | 策略 | 说明 |

| --- | --- | --- | --- |

| `7860` | TCP | 允许 | 用于访问 LLaMA Factory 网页界面 |

| `8000` | TCP | 允许 | 用于访问模型 API 接口 |

1. 添加完成后，确保状态为“启用”。

![](img/fc9641a93bdc8879536516f1971b20f2.png)

#### D. 访问实验环境

配置完成后，您无需使用复杂的 SSH 隧道，直接通过浏览器或客户端即可访问：

**1. 访问 Web UI 界面**

+ 在启动 Web UI 后（确保未报错），直接在本地浏览器输入：

+ `http://[你的公网IP]:7860`

+ 示例：`http://117.50.198.63:7860`

**2. 连接 API 服务**

+ 在 Cherry Studio 或代码中配置 Base URL 为：

+ `http://[你的公网IP]:8000/v1`

+ 示例：`http://117.50.198.63:8000/v1`

这是一份为您准备的学术资源网络加速指南（Linux 版）。

#### E. 学术资源加速 （Linux）

此步骤用于解决 GitHub / HuggingFace 下载速度慢或连接超时的问题。

CompShare 平台通过专用 DNS 服务器提供加速服务。请在终端执行以下命令进行配置：

**1. 修改 DNS 配置**

直接复制并运行以下两条命令，将系统 DNS 指向平台的加速服务器：

```
# 写入主加速 DNS
 echo "nameserver 100.90.90.90" | sudo tee /etc/resolv.conf

# 写入备用加速 DNS
echo "nameserver 100.90.90.100" | sudo tee /etc/resolv.conf
```

**2. 验证加速是否生效**

运行以下命令测试 GitHub 的解析地址：

```
nslookup github.com
```

+ **成功标志：** 如果返回的 Address 是 `100.90.90.xx` 或其他非公共 IP，说明加速已生效。

**3. 支持的加速域名列表**

配置生效后，访问以下域名将自动走高速通道：
