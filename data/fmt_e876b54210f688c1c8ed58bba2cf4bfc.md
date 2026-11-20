
```
submit.zip
├── solution
│   ├── readme                                  ########## 介绍您的算法设计和执行流程 ##########
│   ├── requirements.txt                        ########## 第三方 pip 依赖库 ##########
│   └── ...
└── output
    ├── train.sh                                ########## 训练脚本 ##########
    ├── training_dirs
    │   ├── MGM-2B-Pretrain-xxxx                ########## 预训练好的模型 ##########
    │   │   └── pretrain.log                   ########## 预训练日志 ##########
    │   └── MGM-2B-Finetune-xxxx                ########## 微调好的模型 ##########
    │       └── finetune.log                    ########## 微调训练日志 ##########
    └── eval_results                            ########## 推理结果 ##########
        └── MGM-2B-Finetune-xxxx
```

## 来看看在速通指南里，我们具体做了什么

+ 下载基础模型，种子/微调/评测数据集  
+ 基础模型与微调数据集均存放于训练目录中指定位置  
+ 种子数据集存放于`input`目录  
+ 评测数据集存放于`toolkit/eval`目录  

```
bash download.sh # 数据集 + 多模态模型 Gemma-2B，这一步可能时间较长，大约1个小时
```

下载完成后依然会在终端中显示如下信息`Done`

![](img/eb2910a5288240df599361d48f867956.png)

+ 比赛要求使用[`data-juicer`](https://github.com/modelscope/data-juicer)基于上一步中下载的种子数据集进行数据处理与合成  
+ 数据处理与合成相关代码请存放于`solution`目录中，并在`solution/requirements.txt`中添加所需的依赖  
+ 最后也请在`solution/readme`中详细介绍所使用的数据处理与合成方案  
+ 处理后的数据集需按照如下结构存放：  

```
📦 output/
├── 📂 processed_data/
│   ├── 📂 synthetic_images/ (if any)
│   └── 📄 processed_data.jsonl
│ ...
```

+`processed_data.jsonl`需为标准的JSONL格式，例如：  

```
{&quot;images&quot;: [&quot;images/00237/002375592.jpg&quot;], &quot;text&quot;: &quot;&lt;image&gt;\nadorable pink and gray elephant themed party favour boxes with tissue fillers &lt;|__dj__eoc|&gt;&quot;, &quot;id&quot;: &quot;002375592&quot;}
{&quot;images&quot;: [&quot;images/00199/001999195.jpg&quot;], &quot;text&quot;: &quot;&lt;image&gt;\nbreccinano adult dog food for all ages with turkey, lamb and venisi &lt;|__dj__eoc|&gt;&quot;, &quot;id&quot;: &quot;001999195&quot;}
...
```

在本次的baseline中，我们采用最简单的数据处理方法`recaptioning`来作为一个示例

由于可能无法直连HuggingFace的原因需要提前下载`blip2-opt-2.7b`的图片描述模型

```
# model_download.py
from modelscope import snapshot_download

model_dir = snapshot_download(&#x27;goldsj/blip2-opt-2.7b&#x27;, cache_dir=&#x27;/root/autodl-tmp/downloads&#x27;, revision=&#x27;master&#x27;)
```

终端执行

```
python model_download.py
```

![](img/ccd6ad529a23e2c2d531cfb7251eb17b.png)

注意这里的`cache_dir`需要改成想要保存的目录

```
# prepare the recaptioning recipe
recipe = &#x27;&#x27;&#x27;
dataset_path: input/pretrain_stage_1_10k/mgm_pretrain_stage_1_10k.jsonl
export_path: output/image_captioning_output/res_10k.jsonl

process:
  - image_captioning_mapper:
      hf_img2seq: &#x27;Salesforce/blip2-opt-2.7b&#x27;  # You can replace this path to a local downloaded HF model
      keep_original_sample: false  # we only need the recaptioned captions
&#x27;&#x27;&#x27;

with open(&#x27;solution/image_captioning.yaml&#x27;, &#x27;w&#x27;) as fout:
    fout.write(recipe)
```

注意其中的`hf_img2seq`需要换成之前下载好的本地模型所在的目录，比如`/root/autodl-tmp/downloads/goldsj/blip2-opt-2___7b`

得到`image_captioning.yaml`文件如下

```
dataset_path: input/pretrain_stage_1_10k/mgm_pretrain_stage_1_10k.jsonl
export_path: output/image_captioning_output/res_10k.jsonl

process:
  - image_captioning_mapper:
      hf_img2seq: &#x27;/root/autodl-tmp/downloads/goldsj/blip2-opt-2___7b&#x27;  # You can replace this path to a local downloaded HF model
      keep_original_sample: false  # we only need the recaptioned captions
```

然后使用`data-jucier`来进行数据处理

```
# recaption the 10k data
dj-process --config solution/image_captioning.yaml
```

![](img/445fe7027869454145790b1a9f1f9996.png)

## 参考资料

## 更多QA详见[QA & 其他彩蛋](https://datawhaler.feishu.cn/wiki/OyA2w7JwFiydNtkh3hccyRZunsb)！

>[QA & 其他彩蛋](https://datawhaler.feishu.cn/wiki/OyA2w7JwFiydNtkh3hccyRZunsb)任何想法都可以在学习群里交流或在进行填写哟~

>本教程主要由[姜舒凡](https://github.com/Tsumugii24)、、、写作贡献，

>融合了多位助教的内测体验和宝贵建议。

>如果你觉得对你有价值和启发，请在下面点赞和评论哟~

>

### 另外别忘了写笔记打卡呀！！~

### 及时思考、沉淀和产出能让你的学习事半功倍！

>

> **注：本文采用 署名—非商业性使用—相同方式共享 4.0 协议国际版 许可协议进行许可。**
