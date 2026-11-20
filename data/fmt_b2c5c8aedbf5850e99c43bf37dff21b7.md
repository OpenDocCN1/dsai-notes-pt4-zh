
记住你的 ModelScope 账号名称/模型库名称，如 `Shanghai_AI_Laboratory/internlm3-8b-instruct`，然后填写信息提交单等待成绩榜单更新吧！！！如果完成了测评，会在成绩榜单最下面的提交记录中，查找自己的 `uid` 进行查询。

## 补充

笔者11:30填完表单，午饭回来后14:00左右看到了成绩，以上时间仅供参考。快在群里用“你怎么知道我超了基线亿点点！”来分享你完成打榜的喜悦吧！（bushi

## 信息提交单

https://aicarrier.feishu.cn/share/base/form/shrcn0JkjbZKMeMPw04uHCWc5Pg

## 成绩榜单（动态更新中）

<https://aicarrier.feishu.cn/share/base/dashboard/shrcnqpXY6Uy9FodNF3It75GSNe?ccm_open=iframe&iframeFrom=docx>

各个模型的基线成绩如下图，看看你的分数如何吧！！

![](img/138dac8565dc14c44008a181a7620638.png)

# 刷榜

还等什么，快来刷榜吧！！！挑战米神！

如何更高效的拿分呢？

1. 从数据入手
2. 从算法入手
3. 从参数入手

从算法入手 比如很火的GRPO 很酷是吧，但貌似有些难度。

从数据入手 比如加大训练数据，有些累。

从参数入手 这个ez哈哈

![](img/58416314974836ec861194d3e84be9b9.png)

提交

```
swift export \
    --model /root/paper/config/swift_output/InternLM3-8B-Lora-SFT/v4-20250511-153230/checkpoint-45-merged \
    --push_to_hub true \
    --hub_model_id &#x27;zhangfc12345678/camp5-2&#x27; \
    --hub_token &#x27;03xx&#x27; \
    --use_hf false
```

重新提交表单发现 work了，都超基线了！

![](img/443affa828e659f8c16b34908f3229bf.png)

本文参考：

[L2G1-InternVL3部署微调实践](https://aicarrier.feishu.cn/wiki/CPCnwzp1qi2bBWka9TWcLjqSnad)

[InternLM论文分类微调实践（`swift` 版）](https://vwpecqie443.feishu.cn/wiki/AdRfwyZfWiJPJJkwnhucGavSnBe)

## 之前的 `conda` 管理（已更新）

```
conda create -n xtuner_513 python=3.10 -y
conda activate xtuner_513
pip install torch==2.4.0+cu121 torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/cu121
pip install xtuner timm flash_attn datasets==2.21.0 deepspeed==0.16.1
conda install mpi4py -y
```

```
conda create -n xtuner_513 python=3.10 -y
conda activate xtuner_513
pip install torch==2.4.0+cu121 torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/cu121
pip install xtuner timm flash_attn datasets==2.21.0 deepspeed==0.16.1
conda install mpi4py -y
#为了兼容模型，降级transformers版本
pip uninstall transformers -y
pip install transformers==4.48.0 --no-cache-dir -i https://pypi.tuna.tsinghua.edu.cn/simple
```
