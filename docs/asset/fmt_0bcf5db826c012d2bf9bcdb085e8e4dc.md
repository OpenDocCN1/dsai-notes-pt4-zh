
# 使用VLMEvalKit测评

创建开发机，选择`cuda12.2`的镜像，选择50%的A100。

## 安装依赖

### 创建环境

```
# 基于conda环境
conda create -n VLMEvalKit python=3.10 -y
```

### 安装依赖

```
conda activate VLMEvalKit
pip install idna requests
pip install torch==2.4.0 torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121

git clone https://ghfast.top/https://github.com/open-compass/VLMEvalKit.git
cd VLMEvalKit
pip install -e .
pip install timm einops accelerate lmdeploy openai datasets==2.19.2
```

根据文档介绍`https://github.com/open-compass/VLMEvalKit/blob/main/docs/zh-CN/ConfigSystem.md`，默认情况下，`VLMEvalKit`通过在`run.py`脚本中使用`--model`和`--data`参数设置模型名称（在`/vlmeval/config.py`中定义）和数据集名称（在`vlmeval/dataset/__init__.py` 或 `vlmeval/dataset/video_dataset_config.py` 中定义）来启动评估。

由于测评需要用`/root/train/merged-9336`这个模型，用`/root/train/SanguoVLM-testset`这个测试集。

## 添加新测评集

根据`https://github.com/open-compass/VLMEvalKit/blob/main/docs/zh-CN/Development.md`，添加新的benchmark。

需要一步一步修改`VLMEvalKit`的代码，把自己的一个数据集整理为`VLMEvalKit`支持的测评集。我把修改整理到了自己的`VLMEvalKit`仓库里`https://github.com/HelloWarcraft/VLMEvalKit`，可以直接从这个仓库里下载`VLMEvalKit`源码，就可以跳过3.3节的新增测评集的内容了：

```
cd /root && mkdir temp
cd /root/temp
git clone https://github.com/HelloWarcraft/VLMEvalKit
```

之后执行`python run.py`之时，`--data SanguoDataset`就可以生效了。

## 添加新模型

`/root/temp/VLMEvalKit/vlmeval/config.py`文件中设置在`VLMEvalKit`中支持的 VLM 名称，以及模型路径。

由于`InternVL3-9B`的模型在本地已经有了，这里在`VLMEvalKit/vlmeval/config.py`下第802行，新增模型名称和模型路径：

```
    "InternVL3-9B-merged-9336": partial(
        InternVLChat, model_path="/root/train/merged-9336", version="V2.0"
    ),
```

这样即可通过`InternVL3-9B-merged-9336`来调用`/root/train/merged-9336`。

其实也可以使用`lmdeploy`把`/root/train/merged-9336`模型部署为`InternVL3-9B-merged-9336`，命令如下：

```
conda activate VLMEvalKit
pip install lmdeploy
cd /root/temp/VLMEvalKit
lmdeploy serve api_server /root/train/merged-9336 --model-name InternVL3-9B-merged-9336
```

之后执行`python run.py`之时，`--model InternVL3-9B-merged-9336`就可以生效了。

## 开始测评

### 使用`internlm2_5-7b-chat`的api作为测评LLM

由于是开放型问题，待测试的新模型根据测评集的问题会得到一个`prediction`，`prediction`跟标准答案`answer`比对。

目前的VLM是输出一段文本，不是分类或者选择题，直接判别`prediction==answer`肯定不合适，那么如何评判`prediction`是否跟`answer`匹配？

这时候就另外需要一个LLM来当测评的老师，比对`prediction`和`answer`，`VLMEvalKit`支持用`GPT-4`、`Gemini`等在线LLM进行测评，只需要环境变量里设置：

```
export OPENAI_API_KEY=
export GOOGLE_API_KEY=
```

也可以用本地LLM经过`lmdeploy`部署，然后，使用本地部署的LLM来当测评老师。

因此在实际开始测评之前，要给测评老师的这个LLM安排到位。

这里使用本地的`internlm2_5-7b-chat`当测评老师。新开终端，将其部署为api格式：

```
conda activate VLMEvalKit
cd /root/temp/VLMEvalKit
lmdeploy serve api_server /share/new_models/Shanghai_AI_Laboratory/internlm2_5-7b-chat --server-port 23333 --api-keys sk-123456 --cache-max-entry-count 0.2
```

> 密钥必须以 "sk-" 开头，后跟任意数字。（`https://github.com/open-compass/VLMEvalKit/blob/main/docs/zh-CN/Quickstart.md`）

之后要新开一个终端，指定这个老师的api地址和api密钥：

```
cd /root/temp/VLMEvalKit
touch temp.txt
```

在`temp.txt`里粘贴如下内容：

```
OPENAI_API_KEY=sk-123456
OPENAI_API_BASE=http://0.0.0.0:23333/v1/chat/completions
LOCAL_LLM=/share/new_models/Shanghai_AI_Laboratory/internlm2_5-7b-chat
```

之后修改`temp.txt`的名称为`.env`：

```
cp /root/temp/VLMEvalKit/temp.txt /root/temp/VLMEvalKit/.env
```

### 开始测评

#### 使用`SanguoDataset`测评集

`SanguoDataset`里有186条测试数据，为了降低测评时间，设置了一个`SanguoDataset_MINI`的小测评集，里面有62条数据。只测评`SanguoDataset_MINI`即可，很省时间，耗时会缩短`SanguoDataset`的1/3。

使用以下命令，用新测评集，先测评`InternVL3-2B`，观察没微调的`InternVL3-2B`模型：

```
conda activate VLMEvalKit
cd /root/temp/VLMEvalKit
python run.py --data SanguoDataset --model InternVL3-2B --verbose --work-dir /root/temp/VLMEvalKit/results
```

测评完毕，总分186分，`InternVL3-2B`拿了108.6分，得分率是58.39%：

![](img/6d28b5f0f8bcc7f5f16f8d5cec477d76.png)

使用以下命令开始用`InternVL3-9B-merged-9336`测评新数据集：

```
conda activate VLMEvalKit
cd /root/temp/VLMEvalKit
python run.py --data SanguoDataset --model InternVL3-9B-merged-9336 --verbose --work-dir /root/temp/VLMEvalKit/results
```

这个模型的测评，前半段生成`prediction`过程就需要给5小时，`prediction`跟`answer`比对过程又估计需要2小时，一共7小时。所以这个目前不做了，除非是真心准备了测评集，在严格测评模型的推理能力。

#### 使用`SanguoDataset_MINI`测评集

186条测评数据的`SanguoDataset.tsv`已经被简化为`SanguoDataset_MINI.tsv`的39条数据了。然后添加到了`VLMEvalKit`里，之后`--data SanguoDataset_MINI`命令生效。

开始测评`InternVL3-2B`模型：

```
conda activate VLMEvalKit
cd /root/temp/VLMEvalKit
python run.py --data SanguoDataset_MINI --model InternVL3-2B --verbose --work-dir /root/temp/VLMEvalKit/results
```

`prediction`生成完毕，之后开始`prediction`跟`answer`的比对：

![](img/0f5b2fe46ccaab54b5256705d1d970b7.png)

比对完成，`InternVL3-2B`得分是16.5分，得分率42.31%：

![](img/4cc4ed50e8a402abfdd7ba6ad0f13164.png)

测评`InternVL3-9B-merged-9336`模型：

```
conda activate VLMEvalKit
cd /root/temp/VLMEvalKit
python run.py --data SanguoDataset_MINI --model InternVL3-9B-merged-9336 --verbose --work-dir /root/temp/VLMEvalKit/results
```

评分完成，`InternVL3-9B-merged-9336`得分是19.7分，得分率50.51%：

![](img/0a2d06277bee3a6df23dc5a6f169127a.png)

其中**人物识别**类题目9道，微调前的回答全都是错的，微调后有74.4%的正确率。训练集跟测评集里的图完全不一样，相当于考试考新题，人物识别74.4%的正确率，效果很不错了。

**剧情问答**类题目，提问的时候就已经剧透了，微调前的`InternVL3-2B`都能回答个七七八八。微调后分数反而下降，大概是因为`prediction`跟`answer`比对的时候，不太严谨。

**画面解释**类题目，则纯粹是解释画面，是否微调几乎没影响。

#### 使用`SanguoDataset_AB_MINI`测评集

新增的`AB_MINI`测评集，划分为3个分类各20个问题，更加平均。

开始测评`InternVL3-2B`模型：

```
conda activate VLMEvalKit
cd /root/temp/VLMEvalKit
python run.py --data SanguoDataset_AB_MINI --model InternVL3-2B --verbose --work-dir /root/temp/VLMEvalKit/results
```

`InternVL3-2B`测评结果如下：

![](img/343ed1171bfbf99f54a90d3d31ed72f8.png)

在`VLMEvalKit/results`里有`InternVL3-2B_SanguoDataset_AB_MINI_gpt-4-turbo.xlsx`的文件，里面记录了每一条的判分结果：

![](img/a43252c5dd9d5d77eb7782ac8dca7a29.png)

测评`InternVL3-9B-merged-9336`模型：

```
conda activate VLMEvalKit
cd /root/temp/VLMEvalKit
python run.py --data SanguoDataset_AB_MINI --model InternVL3-9B-merged-9336 --verbose --work-dir /root/temp/VLMEvalKit/results
```

`InternVL3-9B-merged-9336`的测评效果如下：

![](img/74f8278655ec27ead5c809a1eb933586.png)

每一条的判分结果如下：

![](img/8cb1a32d8074b66dc70fd99eafbcec18.png)
