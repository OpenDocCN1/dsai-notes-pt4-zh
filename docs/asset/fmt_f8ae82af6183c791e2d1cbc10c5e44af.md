
```bash
cd /root/MindSpeed-Core-MS/MindSpeed-MM/

python checkpoint/common/merge_base_lora_weight.py --base_save_dir /root/MindSpeed-Core-MS/MindSpeed-MM/ckpt/mm_path/internvl3_5_1b/ \
--lora_save_dir /root/MindSpeed-Core-MS/MindSpeed-MM/outputs/internvl3_5_1_B/ \
--merge_save_dir /root/MindSpeed-Core-MS/MindSpeed-MM/outputs-merged/400_iter \
--lora_target_modules linear_qkv linear_proj linear_fc1 linear_fc2 mlp1 projector \
--lora_r 16 \
--lora_alpha 32 \
--tp_size 1 \
--pp_size 1
```

结果如下，成功合并数是114，文本框架每一层我们微调了 4 个模块：`qkv`, `proj`, `fc1`, `fc2`，文本模型有28层，最后加上两个视觉模型与文本模型连接处的线性层正好是 114 。其他报错的层是模型 `lora` 训练时保存的元数据，用来记录训练过程，不会影响模型。

![](img/eb98e60b7cfcf1e1f91f2af3d4868913.png)

## HF 权重还原

合并权重完成之后，运行以下指令将 MM 格式的权重转换为 HF 格式权重。再次注意，默认会按照`latest_checkpointed_iteration.txt`中的数字对相应的 iter 文件进行转换。如果你想转换的权重不是最后一次更新的权重一定要修改为自己想要的数字。

+   `--cfg.hf_config.hf_dir`：源 HF 权重路径。
+   `--cfg.mm_dir`：`lora` 合并权重后的 MM 权重路径。
+   `--cfg.save_hf_dir`：转换后 HF 权重保存的路径，也是我们上传模型的路径。

```bash
mm-convert InternVLConverter mm_to_hf   --cfg.hf_config.hf_dir /root/share/new_models/InternVL3.5/InternVL3_5-1B \   
--cfg.mm_dir /root/MindSpeed-Core-MS/MindSpeed-MM/outputs-merged/400_iter/ \  
--cfg.save_hf_dir /root/MindSpeed-Core-MS/MindSpeed-MM/ckpt/hf_path/lora_fine_400 \
--cfg.parallel_config.tp_size 1 \
--cfg.parallel_config.ep_size 1 \
--cfg.parallel_config.llm_pp_layers [28] \
--cfg.parallel_config.vit_pp_layers [24]
```

## 模型评估

这一部分建议用另一个环境进行，因为会用到 torch 和其他依赖，容易破坏我们脆弱的微调环境。

因此新建一个环境用于评估。

```bash
create -n eval_model python==3.10
conda activate eval_model
```

### 克隆仓库

```bash
cd ~
git clone https://github.com/JimmyMa99/VLM-formula-recognition-dataset.git
cd VLM-formula-recognition-dataset-main
```

### 安装依赖

安装依赖，每次服务器启动都需要安装一次：

```bash
apt-get update
apt-get install -y texlive-latex-base texlive-binaries dvipng texlive-latex-extra
```

### 验证

```bash
latex --version
dvipng --version
```

防止微调时的环境变量影响，需要去除环境变量。并且引入Ascend工具。

```bash
unset `PATHONEPATH`
source /usr/local/Ascend/ascend-toolkit/set_env.sh 
```

### 评估启动

+   `--model-path`：要评估的模型目录。
+   `--input-dir`：输入模型的图片目录。
+   `--output-dir`：公式文本结果保存的目录。
+   `--report-path`：评估报告保存的目录。
+   `--model-type`：模型类型，`vl` 即可。

```bash
source /usr/local/Ascend/ascend-toolkit/set_env.sh 

python eval.py --model-path /root/MindSpeed-Core-MS/MindSpeed-MM/ckpt/hf_path/lora_fine_400/ \
--input-dir ./data/output_eval/ \
--output-dir ./results/ \
--report-path ./evaluation_report.txt \
--model-type vl
```

### 评估结果

这个可以不用管，因为好像所有模型好像都无法通过，本地的哈希值可能判定过于严格导致大部分还原的图片被判定为哈希值不同。

![](img/b895cf511eb04092f3b8959cd75cb87e.png)

主要看这个，一定程度上代表用你模型生成的 `Latex`公式 还原的图片与原图片有多像。但是评估函数仅供参考，主要以线上评估得分为主。括号里的 0.99 是相似度阈值，代表相似度超过多少才算分。可以自己调整。

![](img/451e55b75637fb26814972c1153c6aef.png)

原始的 `HF` 模型评估出来的相似度普遍在 0.68-0.72 之间，如果微调出来的模型测评出来不优于这个分数的话，代表微调模型与原始模型在公式识别上的能力相差不大。

## 模型上传

这一步可以在微调环境中，也可以在测评环境中，推荐在测评环境中。

### 安装依赖

安装依赖

```bash
pip install modelscope
```

### 上传脚本

运行上传脚本 `python upload.py`

```python
from modelscope.hub.api import HubApi
from modelscope.hub.constants import Licenses, ModelVisibility

# 配置基本信息
YOUR_ACCESS_TOKEN = "ms-77xx"  # 填写自己的 api token ，获取方式点击：https://modelscope.cn/my/myaccesstoken
api = HubApi()
api.login(YOUR_ACCESS_TOKEN)

# 取名字
owner_name = "用户名"  # ModelScope 的用户名
model_name = "上传后模型叫什么"  # 为模型库取个响亮优雅又好听的名字，需根据自己情况修改
model_id = f"{owner_name}/{model_name}"

# 创建模型仓库
api.create_model(
    model_id,
    visibility=ModelVisibility.PUBLIC,
    license=Licenses.APACHE_V2,
    chinese_name=f"{owner_name}的书生大模型实战营-公式微调分类比赛",
)

# 上传模型到仓库
api.upload_folder(
    repo_id=f"{owner_name}/{model_name}",
    folder_path="模型路径",  # 微调后模型的文件夹名称
    commit_message="upload model folder to repo",  # 写上传信息
)
```

在[魔搭社区主页](https://modelscope.cn/)找到上传的模型，复制自己模型的名称：

![](img/127787f573c79de1f16000e6165c3ce3.png)

在[书生大模型公式识别微调练习赛](https://aicarrier.feishu.cn/share/base/form/shrcnXjK6zERK7z4pPU85PLO80c?chunked=false)提交页面进行提交

![](img/737b19f073cfafb8de22bb7358c0df6f.png)

在[查询页面](https://aicarrier.feishu.cn/share/base/query/shrcnM5VzC2jnErQig5Wk88kgoe)查询结果：

### 提示词如下

````text
请识别图片中的数学公式并转换为 LaTeX 格式。
            【重要约束】：
            1. 直接输出 LaTeX 代码，不要包含任何结束语或公式解释。
            2. 使用 ```latex \n ``` 包裹LaTex公式。

            示例1:

            ```latex
            {E} = mc^2
            ```

````

优化提示词应该是最简单、提分最快的选择了，笔者在这里仅给出一个比初始提示词好用一点的提示词。大家可以在提示词上面多多优化。然后再尝试数据预处理和模型调参。祝大家在比赛中取得一个好成绩。

![](img/38bb5d56c8fd8d927657457ac10a3e17.png)

### 参考文章

1.  [Mindspeed-mm仓库](https://gitcode.com/Ascend/MindSpeed-MM)。

1.  [CANN 安装文档](https://www.hiascend.com/document/detail/zh/CANNCommunityEdition/82RC1/softwareinst/instg/instg_quick.html?Mode=PmIns&InstallType=netconda&OS=openEuler&Software=cannToolKit)。
