
# L2G2-GraphGen：训练数据合成实践

> 来源：[https://aicarrier.feishu.cn/docx/FqMMdz2smogTGhxnzmNc0WvAnOh](https://aicarrier.feishu.cn/docx/FqMMdz2smogTGhxnzmNc0WvAnOh)
> 文档尚处于草稿阶段，完善中

在基础课程第4节里，大家已经知道了如何使用XTuner微调InternLM3系列模型。

然而，在垂域模型（如医疗、农业）训练中，常常会面临数据短缺的问题。

用 `SFT`（Supervised Fine-Tuning）给模型注入新知识，需要数十万的样本量。同时，领域数据的标注需要专业背景，普通人难以胜任。此时数据合成手段是刚需。

## GraphGen介绍

GraphGen是一个基于知识图谱的 `SFT` 数据生成框架。

该框架通过构建细粒度知识图谱、识别 `LLMs` 的知识盲点、并生成多风格的内容来提高模型在知识密集型任务上的表现。GraphGen 适用于三种关键问答场景：原子问答（atomic QA）、聚合问答（aggregated QA）和多跳问答（multi-hop QA）。

+ Atomic QA：最基础的问答形式，专注于单一知识点或事实。
+ Aggregated QA：涉及多个知识点或复杂信息的整合。
+ Multi-hop QA：需要通过多步推理，结合多个知识点才能回答。

### Workflow图

![](img/55cc33c92fff49fa2279c500598e0d6f.png)

Github仓库：`https://github.com/open-sciencelab/GraphGen`

## GraphGen流程解析

### 1. 知识图谱构建

+ **文本分割**：将原始文档分割成语义连贯的小片段。
+ **实体与关系提取**：使用 `Synthesizer Model` 从片段中识别和提取实体及其关系。
+ **知识图谱聚合**：将不同片段中的相同实体或关系的描述自动合并，形成知识图谱。

### 2. 理解能力评估

+ **知识表示：**将知识图谱中的每个关系视为一个知识陈述 `Ri`，并生成其改写版本及否定版本。
+ **置信度量化：**通过 `Trainee Model` 对这些陈述的置信度进行评估，计算其对每个知识点的理解程度。
+ **理解损失计算：**通过计算真实分布与预测分布之间的交叉熵，来衡量 `Trainee Model` 对知识的理解差距。

### 3. 图组织

+ **子图提取：**执行 k-hop 子图提取，以有效组织知识图谱。采用深度策略、长度策略和选择策略来平衡子图的复杂性、相关性和计算可行性。

### 4. 问答对生成

+ **原子 QA 生成：**从单个节点或边生成基础问答对。
+ **聚合 QA 生成：**组织和改写子图中的数据，生成涉及多个实体和关系的综合性问答对。
+ **多跳 QA 生成：**明确实体间关系，生成需要多步推理的问答对。

## 环境

### 1. 使用`conda`创建环境

```
conda create -n graphgen python=3.10 -y
conda activate graphgen
```

### 2. 下载GraphGen仓库，并安装依赖

```
git clone https://github.com/open-sciencelab/GraphGen.git
cd GraphGen
pip install -r requirements.txt
```

## 运行GraphGen

### 3.1 使用Gradio Demo进行简单数据合成

1. 运行Gradio Demo，执行下述命令

```
python webui/app.py
```

运行成功时会显示：

![](img/779952869595af4ef6583d3d24973db2.png)

打开 `http://0.0.0.0:7860`，能够看到如下界面

![](img/6b827fc7c137a914fd716c35e34dc59a.png)

2. 填入调用LLM接口的base url、model和api key。

以下两种方式二选一即可：

一、使用gitee AI

`https://ai.gitee.com/serverless-api`

前往 Gitee AI 工作台 - 设置 - 访问令牌，创建你的 `API Key`

![](img/cc939e644e31673488e503292649a7a9.png)

使用 `InternLM3-8B-Instruct` 完成数据生成，分别修改下面三处：

`Synthesizer URL`: `https://ai.gitee.com/v1`

`Synthesizer Model`: `InternLM3-8B-Instruct`

`API Key`: 申请的 `API Key`

![](img/3820c739e62b0372392a757faab7aa0d.png)

二、使用硅基流动

`https://siliconflow.cn/`

进入[API密钥](https://cloud.siliconflow.cn/account/ak)页面，点击“新建API密钥”，创建您的 `API key`

![](img/ed6969648fa329bdc231786f7b235731.png)

使用 `internlm/internlm2_5-7b-chat` 进行数据生成，分别修改下面三处：

`Synthesizer URL`: `https://api.siliconflow.cn/v1`

`Synthesizer Model`: `internlm/internlm2_5-7b-chat`

`API Key`: 申请的 `API Key`

3. 选择任意一个 `example file`，或者上传格式相同的文件

4. 点击 `Run GraphGen`

等待生成完成后，将会得到一个结果文件。

![](img/499f2e1760ad1f26b556ecd187065980.png)

### 3.2 运行源码进行数据合成

#### 3.2.1 使用`vllm`部署 `Synthesizer Model` 和 `Trainee Model`

1. 创建一个新的 `python` 环境，安装 `vllm` 库

```
conda create -n vllm python=3.12 -y
conda activate vllm
pip install vllm==0.8.5.post1
```

2. 部署 `internlm/internlm3-8b-instruct` 的 OpenAI Compatible Server

```
vllm serve internlm/internlm3-8b-instruct
```

`vllm serve` 会默认部署在8000端口。如果资源允许，可以同时部署两个模型，并指定不同端口号作为 `Synthesizer Model` 和 `Trainee Model`。为了方便起见，本示例中只部署一个模型同时作为 `Synthesizer Model` 和 `Trainee Model`。

#### 3.2.2 完成配置文件

1. 在根目录中创建一个 `.env` 文件

```
cp .env.example .env
```

2. 填入环境变量

```
SYNTHESIZER_MODEL=your-path/internlm3-8b-instruct
SYNTHESIZER_BASE_URL=http://0.0.0.0:8000/v1
SYNTHESIZER_API_KEY=
TRAINEE_MODEL=your-path/internlm3-8b-instruct
TRAINEE_BASE_URL=http://0.0.0.0:8000/v1
TRAINEE_API_KEY=
```

3. 检查/修改配置文件 `configs/graphgen_config.yaml`

```
data_type: raw
input_file: resources/examples/raw_demo.jsonl
tokenizer: cl100k_base
quiz_samples: 2
traverse_strategy:
  qa_form: aggregated
  bidirectional: true
  edge_sampling: max_loss
  expand_method: max_width
  isolated_node_strategy: ignore
  max_depth: 1
  max_extra_edges: 2
  max_tokens: 256
  loss_strategy: only_edge
web_search: false
re_judge: true
```

#### 3.2.3 运行数据生成脚本

```
bash scripts/generate.sh
```

等待生成完成后可以在指定目录查看生成的数据：

```
ls cache/data/graphgen
```

![](img/a7c4bbd0aff8fc1957f7ccc0a62dd227.png)

![](img/25d47b9efccd4db3dd2b74f2c68abfc9.png)
