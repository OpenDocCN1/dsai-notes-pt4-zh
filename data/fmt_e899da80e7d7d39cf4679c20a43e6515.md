
# DeepSeek-R1-0528模型评测流程与完整测评报告

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/Fl93d2md8ognxax7FmgcRVxdnfc](https://kq4b3vgg5b.feishu.cn/docx/Fl93d2md8ognxax7FmgcRVxdnfc)

我们可以借助`EvalScope`进行非常准确的模型性能评估，项目官网：`https://github.com/modelscope/evalscope`

![](img/f1ea78ba4b7e05956b39a0adab824130.png)

## EvalScope安装流程

为了避免和`Unsloth`的依赖产生冲突，`EvalScope`需要再单独创建虚拟环境，然后再进行安装，具体安装流程如下：

```
conda create --name evalscope python=3.11
conda init
source ~/.bashrc
conda activate evalscope

conda install jupyterlab
conda install ipykernel
python -m ipykernel install --user --name evalscope --display-name "Python evalscope"

pip install evalscope 
```

> 额外选项

```
pip install 'evalscope[opencompass]'   # 安装 OpenCompass backend
pip install 'evalscope[vlmeval]'       # 安装 VLMEvalKit backend
pip install 'evalscope[rag]'           # 安装 RAGEval backend
pip install 'evalscope[perf]'          # 安装 模型压测模块 依赖
pip install 'evalscope[app]'           # 安装 可视化 相关依赖
```

> 此外，也可以考虑源码安装：

```
git clone https://github.com/modelscope/evalscope.git
cd evalscope/

pip install omegaconf

pip install -e .                  # 安装 Native backend
# 额外选项
pip install -e '.[opencompass]'   # 安装 OpenCompass backend
pip install -e '.[vlmeval]'       # 安装 VLMEvalKit backend
pip install -e '.[rag]'           # 安装 RAGEval backend
pip install -e '.[perf]'          # 安装 模型压测模块 依赖
pip install -e '.[app]'           # 安装 可视化 相关依赖
pip install -e '.[all]'           # 安装所有 backends (Native, OpenCompass, VLMEvalKit, RAGEval)
```

## 进行模型性能评估

接下来即可进一步进行模型性能评估。这里我们先尝试对其进行初始状态下的性能评估，然后等微调结束后，再进行新一轮的评估，进而对比微调前后模型性能变化情况。这里需要在`Jupyter`中选择`evalscope` kernel：

![](img/645692ffb0ae56db954946cab80cfbaf.png)

### 2.1 创建测评数据集

在`Jupyter`中运行如下代码：

```
from evalscope.collections import CollectionSchema, DatasetInfo, WeightedSampler
from evalscope.utils.io_utils import dump_jsonl_data

schema = CollectionSchema(name='Qwen3', datasets=[
    CollectionSchema(name='English', datasets=[
        DatasetInfo(name='mmlu_pro', weight=1, task_type='exam', tags=['en'], args={'few_shot_num': 0}),
        DatasetInfo(name='mmlu_redux', weight=1, task_type='exam', tags=['en'], args={'few_shot_num': 0}),
        DatasetInfo(name='ifeval', weight=1, task_type='instruction', tags=['en'], args={'few_shot_num': 0}),
    ]),
    CollectionSchema(name='Chinese', datasets=[
        DatasetInfo(name='ceval', weight=1, task_type='exam', tags=['zh'], args={'few_shot_num': 0}),
        DatasetInfo(name='iquiz', weight=1, task_type='exam', tags=['zh'], args={'few_shot_num': 0}),
    ]),
    CollectionSchema(name='Code', datasets=[
        DatasetInfo(name='live_code_bench', weight=1, task_type='code', tags=['en'], args={'few_shot_num': 0, 'subset_list': ['v5_v6'], 'extra_params': {'start_date': '2025-01-01', 'end_date': '2025-04-30'}}),
    ]),
    CollectionSchema(name='Math&Science', datasets=[
        DatasetInfo(name='math_500', weight=1, task_type='math', tags=['en'], args={'few_shot_num': 0}),
        DatasetInfo(name='aime24', weight=1, task_type='math', tags=['en'], args={'few_shot_num': 0}),
        DatasetInfo(name='aime25', weight=1, task_type='math', tags=['en'], args={'few_shot_num': 0}),
        DatasetInfo(name='gpqa', weight=1, task_type='knowledge', tags=['en'], args={'subset_list': ['gpqa_diamond'], 'few_shot_num': 0})
    ])
])

# get the mixed data
mixed_data = WeightedSampler(schema).sample(100000000)  # set a large number to ensure all datasets are sampled
# dump the mixed data to a jsonl file
dump_jsonl_data(mixed_data, 'outputs/qwen3_test.jsonl')
```

> 也可以在以下网盘连接直接领取测评数据集：

> 链接: https://pan.baidu.com/s/1rphmJ2Qjp0apK9ngRt2FFA 提取码: bst7

### 2.2 模型评测流程

接下来即可继续在`Jupyter`中运行如下代码：

```
from evalscope import TaskConfig, run_task
task_cfg = TaskConfig(
    model='deepseek-reasoner',
    api_url='https://api.deepseek.com',
    eval_type='service',
    datasets=[
        'data_collection',
    ],
    dataset_args={
        'data_collection': {
            'dataset_id': 'modelscope/EvalScope-Qwen3-Test',
            'filters': {'remove_until': '</think>'}  # 过滤掉思考的内容
        }
    },
    eval_batch_size=128,
    generation_config={
        'max_tokens': 30000,  # 最大生成token数，建议设置为较大值避免输出截断
        'temperature': 0.6,  # 采样温度 (qwen 报告推荐值)
        'top_p': 0.95,  # top-p采样 (qwen 报告推荐值)
        'top_k': 20,  # top-k采样 (qwen 报告推荐值)
        'n': 1,  # 每个请求产生的回复数量
    },
    timeout=60000,  # 超时时间
    stream=True,  # 是否使用流式输出
    limit=2000,  # 设置为2000条数据进行测试
)

run_task(task_cfg=task_cfg)
```

此时，`Evalscope`就会围绕模型的推理、指令跟随、代理能力和多语言支持方面能力进行测试，该数据包含`mmlu_pro`、`ifeval`、`live_code_bench`、`math_500`、`aime24`等各著名评估数据集。

### 2.3 查看模型能力评测结果

测评代码运行结束后，即可输入如下命令查看

`evalscope app`

![](img/5a58eb88b7b3b6cfb05836a4929b304b.png)

然后打开浏览器输入网址`localhost:7860`，即可查模型测评结果了：

![](img/02dcbf68d1bbe9e66210200262479e30.png)

## DeepSeek-R1-0528模型详细评测报告

### 数据集概览

本页面用于展示模型再不同评测方向上的平均得分

![](img/18fe55cec4e1c7398578c76649b5a47c.png)

### 数据集详情

本页面用于展示模型再各个测评数据集上的最终评分

![](img/9ee1c413e8ce56d068b692150d671b26.png)
