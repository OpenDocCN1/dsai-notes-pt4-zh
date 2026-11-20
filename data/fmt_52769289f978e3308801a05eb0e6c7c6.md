
# 2.2 命名实体识别（NER）

> 来源：[https://v11enp9ok1h.feishu.cn/docx/OlVVdYHymozjLSxXydicCbfXni4](https://v11enp9ok1h.feishu.cn/docx/OlVVdYHymozjLSxXydicCbfXni4)
>
> 命名实体识别（Named Entity Recognition，简称 NER）是自然语言处理（NLP）中的一项基础任务，它的目标是识别文本中具有特定意义的实体，并将其分类到预定义的类别中。

## 核心概念

+ **命名实体**：文本中表示特定对象的专有名词
+ **实体类别**：常见类型包括人名、地名、组织机构名、时间、日期、货币等

## NER的应用场景

### 实际应用领域

1. **信息提取**：从新闻中提取关键人物和事件
1. **搜索引擎优化**：增强搜索结果的语义理解
1. **客户支持**：自动识别用户查询中的关键实体
1. **医疗领域**：识别病历中的药物名称和疾病术语

### 行业价值

+ **金融领域：**自动分析财经新闻中的公司和股票信息
+ **法律领域：**快速定位合同中的关键条款和参与方
+ **电商领域：**从用户评论中提取产品特征和品牌名称

## NER的技术实现

### 基本方法分类

### 常用算法

1. **条件随机场（CRF）**
1. **双向LSTM**
1. **BERT等预训练模型**

## 实例

```
import spacy

# 加载英文模型
nlp = spacy.load("en_core_web_sm")

# 处理文本
text = "Apple is looking at buying U.K. startup for $1 billion"
doc = nlp(text)

# 输出识别结果
for ent in doc.ents:
    print(ent.text, ent.label_)
```

## NER的评估指标

### 关键性能指标

1. **精确率（Precision）**：识别正确的实体占所有识别实体的比例
1. **召回率（Recall）**：识别正确的实体占所有实际实体的比例
1. **F1分数**：精确率和召回率的调和平均数

### 评估示例

假设测试集中有100个实体：

+ 系统识别出90个，其中80个正确
+ 精确率 = 80/90 ≈ 89%
+ 召回率 = 80/100 = 80%
+ `F1 = 2*(0.89*0.8)/(0.89+0.8) ≈ 84%`

## NER的挑战与解决方案

### 常见挑战

1. **实体边界识别**：如"New York Times"应作为一个整体还是分开识别
1. **实体歧义**：如"Apple"可能指水果或公司
1. **领域适应**：医疗领域的实体识别需要专业词典

### 解决方案

+ **上下文建模**：利用周围词语判断实体类型
+ **领域迁移学习**：先在通用数据上预训练，再在专业领域微调
+ **多模型集成**：结合规则方法和统计方法提高鲁棒性

## 实践练习

### 练习1：使用现有工具

1. 安装spaCy库：`pip install spacy`
1. 下载语言模型：`python -m spacy download en_core_web_sm`
1. 尝试分析不同领域的文本（新闻、科技论文、社交媒体）

### 练习2：构建简单规则

```
import re

def rule_based_ner(text):
    # 匹配日期
    dates = re.findall(r'\d{1,2}[/-]\d{1,2}[/-]\d{2,4}', text)
    # 匹配货币
    currencies = re.findall(r'\$\d+\.?\d*', text)
    return {"日期": dates, "货币": currencies}

sample = "会议定于12/15/2023举行，预算为$5000"
print(rule_based_ner(sample))
```
