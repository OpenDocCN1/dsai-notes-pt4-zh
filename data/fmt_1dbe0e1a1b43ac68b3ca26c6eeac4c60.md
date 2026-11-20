
# 医学大模型推理与微调实验

## 一、模型原始状态下的问答示例

&lt;think&gt;
Okay, so I have a patient presenting with sudden-onset chest pain that's radiating to the neck and left arm. That makes me think of a heart attack because those symptoms are classic—angina or myocardial infarction. The left arm pain, neck pain, and sometimes jaw or back pain can all be related to coronary artery issues.

Looking at the past medical history, the patient has hypercholesterolemia, which is high cholesterol, and coronary artery disease. Those are both risk factors for atherosclerosis, which can lead to blockages in the coronary arteries. The elevated troponin I levels are a big clue because troponin is a cardiac enzyme released when the heart muscle is damaged, which is a sign of a heart attack. Also, the patient is experiencing tachycardia, which means their heart is beating faster than usual. In a heart attack, the heart might beat faster as it tries to pump blood to compensate for the blocked artery.

Now, considering the coronary arteries, the left main coronary artery supplies blood to the entire left side of the heart, including the left ventricle, which is a large muscle that's crucial for pumping blood. If there's a blockage here, it can lead to a more severe heart attack because the left ventricle is so vital. The right coronary artery supplies the right ventricle and the inferior wall of the left ventricle. Blockages here are possible too, but the left main is more commonly associated with the symptoms described, especially when troponin is elevated.

So putting it all together, the most likely coronary artery involved is the left main coronary artery. The combination of the patient's history, the elevated troponin, and the typical chest pain radiation points to this artery being the culprit.
&lt;/think&gt;

好的，我有一位患者，突然出现胸痛，并放射到颈部和左臂。这让我想到了心脏病发作，因为这些症状很经典——心绞痛或心肌梗死。左臂痛、颈部痛，有时还会伴随下颌或背部的疼痛，这些都可能与冠状动脉问题相关。

从病史来看，患者有**高胆固醇血症**（即高胆固醇）和**冠状动脉疾病**，这两个因素都是**动脉粥样硬化**的风险因素，可能导致冠状动脉发生堵塞。肌钙蛋白I升高是一个很大的线索，因为肌钙蛋白是心肌受损时释放的心脏酶，通常表明发生了心肌梗死。另外，患者还出现了心动过速，即心跳比平常快。在心肌梗死时，心脏可能会加速跳动，以试图通过增加心脏输出量来补偿被阻塞的冠状动脉。

考虑到冠状动脉，**左主冠状动脉（LMCA）**为整个左侧心脏提供血液，包括左心室，而左心室是一个关键的泵血肌肉。如果这里发生堵塞，可能导致更严重的心肌梗死，因为左心室至关重要。右冠状动脉为右心室和左心室下壁提供血液，这里的堵塞也是可能的，但左主冠状动脉通常与上述症状更相关，尤其是当肌钙蛋白升高时。

**所以，将所有因素**综合考虑，最可能受累的冠状动脉是左主冠状动脉（LMCA）。患者的病史、肌钙蛋白升高以及典型的胸痛放射症状都指向了这一动脉作为罪魁祸首。

&lt;/think&gt;

最可能受累的冠状动脉是**左主冠状动脉（LMCA）**。

**解释：**

+ **症状：** 患者突发胸痛并放射至颈部和左臂，以及肌钙蛋白升高，提示急性冠状动脉综合症，可能是心肌梗死。
+ **病史：** 高胆固醇血症和冠状动脉疾病病史是动脉粥样硬化的风险因素，可能导致冠状动脉堵塞。
+ **心动过速：** 心率增加可能是心脏为补偿心肌血流减少而产生的反应。
+ **冠状动脉考虑：** 左主冠状动脉供应左心室，这个肌肉对心脏功能至关重要。与右冠状动脉相比，左主冠状动脉的堵塞会导致更严重且危及生命的心肌梗死，右冠状动脉通常供应的是不那么关键的区域。

**因此，症状、肌钙蛋白升高以及患者的病史强烈指向左主冠状动脉（LMCA）作为最可能的罪魁祸首。**

标准答案：

根据患者表现出的突然胸痛并放射至颈部和左臂，结合其有高胆固醇血症和冠状动脉疾病的病史，肌钙蛋白升高和心动过速，临床症状强烈提示左前降支（LAD）动脉受累。该动脉通常是引发此类症状的罪魁祸首，因为它供应了心脏的大部分区域。放射性疼痛和肌钙蛋白升高的组合表明心肌受损，这使得LAD成为最可能的致病动脉。然而，在没有进一步的诊断检查（如心电图）的情况下，最终的确诊仍需等待确认。

能够看出，在原始状态下，模型能够进行推理并给出回复，但实际上第一个回答过程并不符合医学规范，而第二个问题则直接回答错误。由此可见，在初始状态下，模型对于`medical-o1-reasoning-SFT`数据集问答效果并不好。

接下来尝试进行微调，并测试微调后模型问答效果。

## 二、最小可行性实验

接下来我们尝试进行模型微调，对于当前数据集而言，我们可以带入原始数据集的部分数据进行微调，也可以带入全部数据并遍历多次进行微调。对于大多数的微调实验，我们都可以从最小可行性实验入手进行微调，也就是先尝试带入少量数据进行微调，并观测微调效果。若微调可以顺利执行，并能够获得微调效果，再考虑带入更多的数据进行更大规模微调。

### 1.数据集准备

这里我们直接从huggingface上下载`medical-o1-reasoning-SFT`数据集。

+ 设置代理环境

由于huggingface网络受限，下载数据集前需要先进行网络环境设置。若是AutoDL服务器，则可以按照如下方式开启学术加速，从而顺利连接huggingface并进行数据集下载：

```
import subprocess
import os

result = subprocess.run('bash -c "source /etc/network_turbo && env | grep proxy"', shell=True, capture_output=True, text=True)
output = result.stdout
for line in output.splitlines():
    if '=' in line:
        var, value = line.split('=', 1)
        os.environ[var] = value
```

+ 下载数据集

接下来使用`datasets`进行数据集下载

```
!pip install datasets
```

```
import os
from datasets import load_dataset
```

再次确认提示词模板：

```
train_prompt_style = """Below is an instruction that describes a task, paired with an input that provides further context. 
Write a response that appropriately completes the request. 
Before answering, think carefully about the question and create a step-by-step chain of thoughts to ensure a logical and accurate response.

### Instruction:
You are a medical expert with advanced knowledge in clinical reasoning, diagnostics, and treatment planning. 
Please answer the following medical question. 

### Question:
{}

### Response:
<think>
{}
</think>
{}"""
```

然后提取并设置文本生成结束的标记：

```
EOS_TOKEN = tokenizer.eos_token
tokenizer.eos_token
```

```
'<｜end▁of▁sentence｜>'
```

然后定义函数，用于对`medical-o1-reasoning-SFT`数据集进行修改，`Complex_CoT`列和`Response`列进行拼接，并加上文本结束标记：

```
def formatting_prompts_func(examples):
    inputs = examples["Question"]
    cots = examples["Complex_CoT"]
    outputs = examples["Response"]
    texts = []
    for input, cot, output in zip(inputs, cots, outputs):
        text = train_prompt_style.format(input, cot, output) + EOS_TOKEN
        texts.append(text)
    return {
        "text": texts,
    }
```

![](img/003e14500c67bae12a7a60e8fcd5df95.png)

在最小可行性实验中，我们可以只下载500条数据进行微调即可看出效果：

```
dataset = load_dataset("FreedomIntelligence/medical-o1-reasoning-SFT","en", split = "train[0:500]",trust_remote_code=True)
```

```
Using the latest cached version of the dataset since FreedomIntelligence/medical-o1-reasoning-SFT couldn't be found on the Hugging Face Hub
Found the latest cached dataset configuration 'en' at /root/.cache/huggingface/datasets/FreedomIntelligence___medical-o1-reasoning-sft/en/0.0.0/4c9573e7de1e8660b88158db2efa7c7204bbd269 (last modified on Wed Feb  5 01:06:32 2025).
```

```
dataset[0]
```

