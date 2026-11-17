# 电商智能客服Agent

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/YDZedTmmtosH2mxlV3Uc5sGTngc](https://qqs7y1hozd1.feishu.cn/docx/YDZedTmmtosH2mxlV3Uc5sGTngc)

## 一、概要设计

### 1.系统架构全景图

**模块组成与层级关系：**

> 智能客服API层：系统唯一入口，处理多协议请求

> 需求感知模块：用户需求解析核心模块

> 规划模块：决策与解决方案生成中心

> 规划-工具模块：业务能力扩展接口

> 用户交互模块：对话管理与个性化交互

> 辅助监控及调试模块：全链路追踪与保障

![](img/035b76d2ee4dc57db3fc6132ecbf535b.png)

### 2.智能客服API层

#### 功能与数据流

![](img/8cbf4437685a61a474be0d0b5a616464.png)

> **技术实现**：

+   协议支持：FastAPI（REST） + gRPC-Gateway（Protobuf）

+   安全认证：OAuth2.1 + JWT双因子验证

+   限流策略：基于Redis的滑动窗口算法（3000 QPS）

##### **核心代码**

```
# api/gateway.py
@app.post(&quot;/v1/chat&quot;)
@rate_limit(limits=[RateLimit(per_minute=100)])
async def handle_chat(request: ChatRequest):
    # 请求处理链
    chain = (
        validate_request
        | authenticate
        | route_to_demand_module
    )
    return await chain.run(request)

```

### 3.需求感知模块

#### 功能分解

![](img/9f7449b9949f48ca082c82002e9364bb.png)

#### **数据处理流程**：

![](img/67a64c091dd8c1072499317ae5a3995e.png)

#### **核心实现**：

```
# core/demand/processing.py
class DemandProcessor:
    def __init__(self):
        self.pipeline = Pipeline([
            TextCleaner(),
            EmotionAnalyzer(threshold=0.7),
            IntentClassifier(model=&quot;bert-tv-intent-v3&quot;),
            EntityExtractor(patterns=TV_PATTERNS),
            SlotFiller(storage=RedisSlotStorage())
        ])
    
    def process(self, text: str) -&gt; DemandContext:
        return self.pipeline.execute(text)

```

### 4.规划模块

#### 双引擎决策架构

![](img/c99325af11999b9d8f187e9700020c33.png)

**技术实现**：

1.  **规则引擎**：Drools 8.x + 彩电DSL语法扩展

```
rule &quot;4K电视HDR异常处理&quot;
  when
    $tv : TVParams(resolution == &quot;4K&quot;, hdr == &quot;HDR10&quot;)
    $prob : Problem(type == &quot;显示故障&quot;, symptom contains &quot;色偏&quot;)
  then
    insert(new Solution(&quot;HDR参数重置&quot;, priority=HIGH));
end

```

1.  **AutoGPT生成**：基于LangChain的ReAct模式

```
class TVProblemSolver(AgentExecutor):
    tools = [OrderTool(), RepairTool()]
    prompt = ChatPromptTemplate.from_messages([
        (&quot;system&quot;, &quot;你是一名彩电维修专家...&quot;),
        (&quot;human&quot;, &quot;{input}&quot;)
    ])

```

### 5.工具模块

#### 功能矩阵

![](img/5ed44949e85d3c89525ddd8f8583719d.png)

**知识库检索流程**：

```
# tools/rag.py
class TVKnowledgeRetriever:
    def __init__(self):
        self.encoder = SentenceTransformer(&quot;paraphrase-multilingual-mpnet-base-v2&quot;)
        self.milvus = MilvusClient
    
    def search(self, query: str) -&gt; List[KnowledgeItem]:
        vector = self.encoder.encode(query)
        return self.milvus.search(
            collection=&quot;tv_knowledge&quot;,
            vectors=[vector],
            params={&quot;nprobe&quot;: 64}
        )

```

### 6.用户交互模块

#### 会话管理机制

```
# interaction/style_templates.py
STYLES = {
    &quot;专业型&quot;: &quot;您咨询的{model}电视参数如下...&quot;,
    &quot;亲和型&quot;: &quot;亲~您提到的{feature}功能是这样操作的哦~&quot;
}

```

![](img/81edfd3595d1235bd782903e1e658199.png)

> **关键技术**：

+   状态存储：Redis Streams（支持10万并发会话）

+   风格设置：Prompt Engineering模板库

### 7.辅助监控及调试模块

#### 全链路追踪设计

**监控指标**：

![](img/ddc8cd881c0e9a54273c671315b908f0.png)

**调试工具链**：

```
# monitoring/debugger.py
def replay_session(session_id: str):
    traces = langsmith.Client().get_traces(session_id)
    return InteractiveDebugger(traces).launch()

```

### 8.数据持久化设计

#### 数据库选型矩阵

![](img/dbc141ab7c47b99d83d1ba2dda43e13c.png)

#### 核心数据模型

![](img/7d544e78e24e0c9e2400563255a88a37.png)

#### 数据流优化

+   **写优化**：

> 采用WAL日志批量提交（每批次500ms）

>

+   **读优化**：

> Redis缓存热点知识（LRU策略，命中率>85%）

>

+   **向量检索**

> 基于PQ量化索引（压缩比8:1，精度损失<3%）

## 二、需求感知模块模型微调实现

### 1.模块整体架构设计

#### 1.1 核心流程

> **情感驱动路由：**

> 所有情感类型均继续后续流程

> 差异点体现在处理优先级而非流程分支

> **多轮会话机制：**

> 通过反馈回路实现对话延续

> 会话管理器维护上下文状态

> **反馈闭环设计：**

> 用户显式反馈（如"不对，我要问的是…"）

> 隐式反馈（连续追问相同问题）

![](img/5c60bbe16aac71c90ba56aaa3c51e4eb.png)

#### 1.2 模块说明

> 需求感知模块通过三级处理流水线实现深度意图理解：

> 情感识别层：采用4分类BERT模型识别情绪强度

> 语义分类层：使用领域适配BERT进行意图归类

> 实体抽取层：基于多语言BERT的序列标注模型

三阶段处理形成完整处理链，输出结构示例如下：

```
{
  &quot;sentiment&quot;: &quot;非常消极&quot;,
  &quot;intent&quot;: &quot;订单查询&quot;,
  &quot;entities&quot;: {&quot;ORDER_NUMBER&quot;: &quot;U2942&quot;}
}

```

### 2.情感识别模型微调

#### 2.1 完整代码实现

```
# 导入核心库（严格保持用户原始代码）
import torch
from transformers import BertTokenizer, BertForSequenceClassification, AdamW
from torch.utils.data import DataLoader, Dataset
from sklearn.model_selection import train_test_split

# 自定义数据集类（完全保留用户原始实现）
class SentimentDataset(Dataset):
    def __init__(self, texts, labels, tokenizer, max_len):
        self.texts = texts
        self.labels = labels
        self.tokenizer = tokenizer
        self.max_len = max_len

    def __len__(self):
        return len(self.texts)

    def __getitem__(self, idx):
        text = str(self.texts[idx])
        encoding = self.tokenizer.encode_plus(
            text,
            add_special_tokens=True,
            max_length=self.max_len,
            padding=&#x27;max_length&#x27;,
            truncation=True,
            return_attention_mask=True,
            return_tensors=&#x27;pt&#x27;,
        )
        return {
            &#x27;input_ids&#x27;: encoding[&#x27;input_ids&#x27;].flatten(),
            &#x27;attention_mask&#x27;: encoding[&#x27;attention_mask&#x27;].flatten(),
            &#x27;labels&#x27;: torch.tensor(self.labels[idx], dtype=torch.long)
        }

# 数据准备函数（严格遵循用户原始逻辑）
def prepare_data():
    import pandas as pd
    data = pd.read_csv(&#x27;lmsr_data.csv&#x27;)
    texts = data[&#x27;对话&#x27;].tolist()
    labels = data[&#x27;情感类型&#x27;].map({&#x27;中性&#x27;:0, &#x27;消极&#x27;:1, &#x27;非常消极&#x27;:2, &#x27;积极&#x27;:3}).tolist()
    return train_test_split(texts, labels, test_size=0.2, random_state=42)

# 训练函数（完整保留用户代码结构）
def train_model(model, train_loader, device, optimizer, train_dataset):
    model.train()
    total_loss = 0
    correct_predictions = 0
    for batch in train_loader:
        input_ids = batch[&#x27;input_ids&#x27;].to(device)
        attention_mask = batch[&#x27;attention_mask&#x27;].to(device)
        labels = batch[&#x27;labels&#x27;].to(device)
        outputs = model(input_ids=input_ids, attention_mask=attention_mask, labels=labels)
        loss = outputs.loss
        logits = outputs.logits
        _, preds = torch.max(logits, dim=1)
        correct_predictions += torch.sum(preds == labels)
        total_loss += loss.item()
        loss.backward()
        optimizer.step()
        optimizer.zero_grad()
    avg_loss = total_loss / len(train_loader)
    accuracy = correct_predictions.double() / len(train_dataset)
    print(f&#x27;Train loss: {avg_loss}, Accuracy: {accuracy}&#x27;)
    return avg_loss, accuracy

# 主函数（完全匹配用户提供代码）
def main():
    train_texts, val_texts, train_labels, val_labels = prepare_data()
    tokenizer = BertTokenizer.from_pretrained(&#x27;bert-base-chinese&#x27;)
    model = BertForSequenceClassification.from_pretrained(&#x27;bert-base-chinese&#x27;, num_labels=4)
    
    MAX_LEN = 64
    BATCH_SIZE = 8
    train_dataset = SentimentDataset(train_texts, train_labels, tokenizer, MAX_LEN)
    val_dataset = SentimentDataset(val_texts, val_labels, tokenizer, MAX_LEN)
    train_loader = DataLoader(train_dataset, batch_size=BATCH_SIZE, shuffle=True)
    val_loader = DataLoader(val_dataset, batch_size=BATCH_SIZE)

    optimizer = AdamW(model.parameters(), lr=2e-5)
    device = torch.device(&quot;cuda&quot; if torch.cuda.is_available() else &quot;cpu&quot;)
    model = model.to(device)

    for epoch in range(3):
        print(f&#x27;Epoch {epoch+1}/3&#x27;)
        train_model(model, train_loader, device, optimizer, train_dataset)
        validate_model(model, val_loader, device, val_dataset)

```

##### 2.2 代码实现解析

1.  **数据处理流程**：

+   SentimentDataset类将原始文本转换为BERT可处理的数值化输入

+   prepare_data()函数完成标签映射（文本→数字）和数据拆分

+   示例标签映射关系：中性→0, 消极→1, 非常消极→2, 积极→3

1.  **模型训练机制**：

```
outputs = model(input_ids, attention_mask, labels)  # 三要素输入
loss = outputs.loss  # 自动计算交叉熵损失
logits = outputs.logits  # 原始预测分数

```

> 每个batch完成前向传播→损失计算→反向传播→参数更新

1.  **关键参数配置**：

```
MAX_LEN = 64  # 输入序列最大长度
BATCH_SIZE = 8  # 训练批大小
lr=2e-5  # 标准BERT微调学习率

```

### 3.语义识别模型微调

#### 3.1 完整代码实现

```
# 严格遵循用户提供的原始代码
import pandas as pd
import torch
from sklearn.model_selection import train_test_split
from transformers import BertTokenizer, AutoModelForSequenceClassification
from torch.utils.data import Dataset, DataLoader

# 配置类（完全保留用户定义）
class Config:
    filename = &quot;nlu_data.csv&quot;
    model_name = &#x27;../models/tv-bert-base-chinese&#x27;
    label_map = {0: &quot;订单查询&quot;, 1: &quot;产品咨询&quot;, 2: &quot;故障报修&quot;, 3: &quot;其他&quot;}
    max_len = 128
    batch_size = 16
    epochs = 5
    learning_rate = 2e-5

# 数据加载函数（保持原始处理逻辑）
def load_data(filename):
    df = pd.read_csv(filename, encoding=&quot;gbk&quot;)
    df = df.dropna(subset=[&quot;文本内容&quot;, &quot;类型标注&quot;])
    reversed_label_map = {v: k for k, v in Config.label_map.items()}
    df[&quot;类型标注&quot;] = df[&quot;类型标注&quot;].map(reversed_label_map)
    return df

# 自定义数据集（完全复制用户代码）
class NLUDataset(Dataset):
    def __init__(self, texts, labels, tokenizer, max_len):
        self.texts = texts
        self.labels = labels
        self.tokenizer = tokenizer
        self.max_len = max_len

    def __len__(self):
        return len(self.texts)

    def __getitem__(self, item):
        text = str(self.texts[item])
        label = self.labels[item]
        encoding = self.tokenizer(
            text,
            add_special_tokens=True,
            max_length=self.max_len,
            padding=&quot;max_length&quot;,
            truncation=True,
            return_attention_mask=True,
            return_tensors=&#x27;pt&#x27;,
        )
        return {
            &#x27;input_ids&#x27;: encoding[&#x27;input_ids&#x27;].flatten(),
            &#x27;attention_mask&#x27;: encoding[&#x27;attention_mask&#x27;].flatten(),
            &quot;labels&quot;: torch.tensor(label, dtype=torch.long)
        }

# 训练函数（严格保留用户原始实现）
def train_model(model, train_loader, val_loader):
    optimizer = torch.optim.AdamW(model.parameters(), lr=Config.learning_rate)
    best_val_loss = float(&#x27;inf&#x27;)
    
    for epoch in range(Config.epochs):
        model.train()
        total_train_loss = 0
        for batch in train_loader:
            optimizer.zero_grad()
            outputs = model(
                input_ids=batch[&#x27;input_ids&#x27;],
                attention_mask=batch[&#x27;attention_mask&#x27;],
                labels=batch[&#x27;labels&#x27;]
            )
            loss = outputs.loss
            total_train_loss += loss.item()
            loss.backward()
            optimizer.step()
        
        avg_train_loss = total_train_loss / len(train_loader)
        print(f&#x27;Epoch {epoch+1}/{Config.epochs}, Train loss: {avg_train_loss}&#x27;)

        # 验证阶段
        model.eval()
        total_val_loss = 0
        correct_predictions = 0
        with torch.no_grad():
            for batch in val_loader:
                outputs = model(
                    input_ids=batch[&quot;input_ids&quot;],
                    attention_mask=batch[&quot;attention_mask&quot;],
                    labels=batch[&quot;labels&quot;]
                )
                total_val_loss += outputs.loss.item()
                _, preds = torch.max(outputs.logits, dim=1)
                correct_predictions += torch.sum(preds == batch[&quot;labels&quot;])
        
        avg_val_loss = total_val_loss / len(val_loader)
        accuracy = correct_predictions.double() / len(val_loader.dataset)
        print(f&quot;Val Loss: {avg_val_loss:.4f}, Accuracy: {accuracy:.4f}&quot;)

        if avg_val_loss &lt; best_val_loss:
            best_val_loss = avg_val_loss
            torch.save(model.state_dict(), &#x27;nlu_best_model.pth&#x27;)

if __name__ == &#x27;__main__&#x27;:
    df = load_data(Config.filename)
    tokenizer = BertTokenizer.from_pretrained(Config.model_name)
    model = AutoModelForSequenceClassification.from_pretrained(
        Config.model_name, 
        num_labels=len(Config.label_map)
    )
    
    train_df, val_df = train_test_split(df, test_size=0.2, random_state=42)
    train_dataset = NLUDataset(
        texts=train_df[&quot;文本内容&quot;].tolist(),
        labels=train_df[&quot;类型标注&quot;].tolist(),
        tokenizer=tokenizer,
        max_len=Config.max_len
    )
    val_dataset = NLUDataset(
        texts=val_df[&quot;文本内容&quot;].tolist(),
        labels=val_df[&quot;类型标注&quot;].tolist(),
        tokenizer=tokenizer,
        max_len=Config.max_len
    )
    
    train_loader = DataLoader(train_dataset, batch_size=Config.batch_size, shuffle=True)
    val_loader = DataLoader(val_dataset, batch_size=Config.batch_size)
    
    train_model(model, train_loader, val_loader)

```

#### 3.2 代码实现解析

1.  **数据处理特征**：

+   使用pd.read_csv加载CSV数据

+   通过dropna清除缺失值

+   标签映射：类型标注列转换为数字标签（如"订单查询"→0）

1.  **模型训练细节**：

```
outputs = model(input_ids, attention_mask, labels)  # 三要素输入
loss = outputs.loss  # 自动计算交叉熵损失
logits = outputs.logits  # 原始预测分数

```

> 每个epoch包含完整训练集遍历和验证集评估

> 保存验证损失最小的模型参数

1.  **关键参数说明**：

```
max_len = 128  # 输入序列最大长度
batch_size = 16  # 训练批大小
epochs = 5  # 训练轮次
learning_rate = 2e-5  # 标准BERT微调学习率

```

### 4.实体识别模型微调

#### 4.1 完整代码实现

```
# 严格遵循用户提供的原始代码
import json
import logging
import torch
from sklearn.model_selection import train_test_split
from torch.utils.data import Dataset, DataLoader
from transformers import BertTokenizerFast, AutoModelForTokenClassification

logging.basicConfig(level=logging.INFO, format=&quot;%(asctime)s - %(levelname)s - %(message)s&quot;)
logger = logging.getLogger(__name__)

class Config:
    filename = &quot;ner_training_data.json&quot;
    model_name = &#x27;bert-base-multilingual-cased&#x27;
    batch_size = 16
    epochs = 5
    label_map = {&#x27;O&#x27;: 0, &#x27;B-ORDER_NUMBER&#x27;: 1, &#x27;I-ORDER_NUMBER&#x27;: 2}
    max_len = 64

def load_data(filename):
    with open(filename, &#x27;r&#x27;, encoding=&#x27;utf-8&#x27;) as f:
        return json.load(f)

def preprocess_data(data):
    texts = []
    labels = []
    for item in data:
        texts.append(item[&#x27;text&#x27;])
        labels.append(item[&#x27;entities&#x27;])
    return texts, labels

class NERDataset(Dataset):
    def __init__(self, texts, entity_list, tokenizer, max_len, label_map):
        self.texts = texts
        self.entity_list = entity_list
        self.tokenizer = tokenizer
        self.max_len = max_len
        self.label_map = label_map

    def __len__(self):
        return len(self.texts)

    def __getitem__(self, item):
        text = self.texts[item]
        entities = self.entity_list[item]
        encoding = self.tokenizer.encode_plus(
            text,
            add_special_tokens=True,
            max_length=self.max_len,
            padding=&#x27;max_length&#x27;,
            truncation=True,
            return_attention_mask=True,
            return_offsets_mapping=True,
            return_tensors=&#x27;pt&#x27;,
        )
        input_ids = encoding[&#x27;input_ids&#x27;][0]
        attention_mask = encoding[&#x27;attention_mask&#x27;][0]
        offsets = encoding[&#x27;offset_mapping&#x27;][0]
        label_ids = [-100] * len(input_ids)
        
        valid_indices = [i for i, (start, end) in enumerate(offsets) if start != 0 or end != 0]
        
        for entity in entities:
            start_char = entity[&#x27;start&#x27;]
            end_char = entity[&#x27;end&#x27;]
            label = entity[&#x27;label&#x27;]
            for idx, (start, end) in enumerate(offsets):
                if start == 0 and end == 0:
                    label_ids[idx] = -100
                    continue
                if start &lt;= start_char &lt; end:
                    label_ids[idx] = self.label_map[f&#x27;B-{label}&#x27;]
                elif start_char &lt; start &lt; end_char:
                    label_ids[idx] = self.label_map[f&#x27;I-{label}&#x27;]
                else:
                    label_ids[idx] = 0
        
        return {
            &#x27;input_ids&#x27;: input_ids,
            &#x27;attention_mask&#x27;: attention_mask,
            &#x27;labels&#x27;: torch.tensor(label_ids, dtype=torch.long)
        }

def train_model(model, train_loader, val_loader, epochs, device):
    optimizer = torch.optim.AdamW(model.parameters(), lr=5e-5)
    criterion = torch.nn.CrossEntropyLoss(ignore_index=-100)
    
    logger.info(&quot;---------------开始训练---------------&quot;)
    best_val_loss = float(&#x27;inf&#x27;)
    
    for epoch in range(epochs):
        model.train()
        total_loss = 0
        for batch in train_loader:
            optimizer.zero_grad()
            inputs = {
                &#x27;input_ids&#x27;: batch[&#x27;input_ids&#x27;].to(device),
                &#x27;attention_mask&#x27;: batch[&#x27;attention_mask&#x27;].to(device),
                &#x27;labels&#x27;: batch[&#x27;labels&#x27;].to(device)
            }
            outputs = model(**inputs)
            loss = outputs.loss
            total_loss += loss.item()
            loss.backward()
            optimizer.step()
        
        avg_loss = total_loss / len(train_loader)
        logger.info(f&quot;Epoch {epoch+1} Train Loss: {avg_loss:.4f}&quot;)
        
        model.eval()
        total_val_loss = 0
        with torch.no_grad():
            for batch in val_loader:
                inputs = {
                    &#x27;input_ids&#x27;: batch[&#x27;input_ids&#x27;].to(device),
                    &#x27;attention_mask&#x27;: batch[&#x27;attention_mask&#x27;].to(device),
                    &#x27;labels&#x27;: batch[&#x27;labels&#x27;].to(device)
                }
                outputs = model(**inputs)
                total_val_loss += outputs.loss.item()
        
        avg_val_loss = total_val_loss / len(val_loader)
        if avg_val_loss &lt; best_val_loss:
            best_val_loss = avg_val_loss
            torch.save(model.state_dict(), &#x27;ner_best_model.pth&#x27;)
        logger.info(f&quot;Epoch {epoch+1} Val Loss: {avg_val_loss:.4f}&quot;)

if __name__ == &#x27;__main__&#x27;:
    data = load_data(Config.filename)
    texts, entity_lists = preprocess_data(data)
    tokenizer = BertTokenizerFast.from_pretrained(Config.model_name)
    model = AutoModelForTokenClassification.from_pretrained(
        Config.model_name,
        num_labels=len(Config.label_map)
    )
    
    train_texts, val_texts, train_entities, val_entities = train_test_split(
        texts, entity_lists, test_size=0.2, random_state=42
    )
    
    train_dataset = NERDataset(
        train_texts, train_entities, tokenizer, Config.max_len, Config.label_map
    )
    val_dataset = NERDataset(
        val_texts, val_entities, tokenizer, Config.max_len, Config.label_map
    )
    
    train_loader = DataLoader(train_dataset, batch_size=Config.batch_size, shuffle=True)
    val_loader = DataLoader(val_dataset, batch_size=Config.batch_size)
    
    device = torch.device(&quot;cuda&quot; if torch.cuda.is_available() else &quot;cpu&quot;)
    model.to(device)
    
    train_model(model, train_loader, val_loader, Config.epochs, device)

```

#### 4.2 代码实现解析

1.  **数据处理流程**：

+   从JSON文件加载标注数据

+   preprocess_data分离文本和实体标注

+   实体标注转换为BIO格式（B-ORDER_NUMBER/I-ORDER_NUMBER）

1.  **标签对齐机制**：

```
for entity in entities:
    for idx, (start, end) in enumerate(offsets):
        if start &lt;= start_char &lt; end:
            label_ids[idx] = self.label_map[f&#x27;B-{label}&#x27;]
        elif start_char &lt; start &lt; end_char:
            label_ids[idx] = self.label_map[f&#x27;I-{label}&#x27;]

```

> 将字符级标注转换为token级标签

> 处理子词切分对齐问题

1.  **训练关键参数**：

```
max_len = 64  # 输入序列最大长度
batch_size = 16  # 训练批大小
lr=5e-5  # 实体识别任务常用学习率

```

## 三、需求感知模块具体实现

### 1.整体架构设计

#### 1.1 模块定位

需求感知模块作为智能客服系统的前端处理单元，负责对用户输入进行多维度解析，输出结构化语义理解结果，为下游决策引擎提供数据支撑。

#### 1.3 设计原则

1.  **混合架构**：BERT微调模型+规则引擎的混合决策机制

1.  **上下文感知**：支持3轮对话历史缓存

1.  **分级降级**：模型故障时自动切换规则引擎

1.  **性能优化**：预测结果缓存+GPU加速推理

![](img/bab8ddbf9ffa3057e3339166a60713aa.png)

### 2.情感分析模块设计

#### 2.1 技术架构图

![](img/78e5c2b7d746df8a8ba53b67608ca061.png)

#### 2.2 核心类设计

##### 2.2.1 SentimentConfig

```
class SentimentConfig:
    model_path = &quot;bert-base-chinese&quot;
    model_pth_path = &quot;../core/demand/lmst/lmst_best_model.pth&quot;
    rule_path = &quot;./configs/sentiment_rules.yaml&quot;
    intensity_thresholds = {&quot;非常负面&quot;: 0.8}
    label_map = [&quot;中性&quot;,&quot;负面&quot;,&quot;非常负面&quot;,&quot;正面&quot;]

```

##### 2.2.2 BertSentimentAnalyzer

```
class BertSentimentAnalyzer:
    def __init__(self):
        # 模型加载
        self.device = torch.device(&quot;cuda&quot; if torch.cuda.is_available() else &quot;cpu&quot;)
        self.tokenizer = BertTokenizer.from_pretrained(...)
        self.model = BertForSequenceClassification.from_pretrained(...)
        
        # 辅助组件
        self.rules = self._load_rules()
        self.cache = RedisCache()  # 可选

    def analyze(self, text: str) -&gt; SentimentResult:
        # 实现三级处理流程
        base_result = self._bert_predict(text)
        enhanced_result = self._rule_based_enhance(text, base_result)
        final_result = self._build_final_result(enhanced_result)

```

#### 2.3 关键算法

##### 2.3.1 强度计算公式

```
intensity = 0.5 + (感叹号数量 * 0.1)
if 非常负面: intensity += 0.3
elif 负面: intensity += 0.2
最终值限制在[0,1]区间

```

##### 2.3.2 规则引擎逻辑

```
def _rule_based_enhance(self, text, base_result):
    # 紧急词检测
    if any(w in text for w in self.rules[&quot;critical_triggers&quot;]):
        base_result[&quot;priority&quot;] = 1
        base_result[&quot;confidence&quot;] = max(base_result[&quot;confidence&quot;], 0.85)
    
    # 正向词补充
    if any(w in text for w in self.rules[&quot;positive_keywords&quot;]):
        base_result[&quot;confidence&quot;] = max(base_result[&quot;confidence&quot;], 0.7)
    return base_result

```

### 3.服务接口设计

#### 3.1 REST API端点

```
@app.get(&quot;/process-text/&quot;)
async def process_text(text: str):
    # 三模块协同处理
    lmst_result = analyzer.analyze(text)
    nlu_result = nlu_analyzer.predict_with_context(text)
    ner_result = ner_analyzer.bert_predict(text) if need_ner else None
    
    return {
        &quot;sentiment&quot;: lmst_result.dict(),
        &quot;intent&quot;: nlu_result,
        &quot;entities&quot;: ner_result
    }

```

#### 3.2 输入输出规范

![](img/fecf05036cceb3c8455e56e173e4871b.png)

**输出示例**：

```
{
    &quot;sentiment&quot;: {
        &quot;type&quot;: &quot;非常负面&quot;,
        &quot;confidence&quot;: 0.92,
        &quot;intensity&quot;: 0.95,
        &quot;needs_escalation&quot;: true
    },
    &quot;intent&quot;: &quot;投诉处理&quot;,
    &quot;entities&quot;: {&quot;order_number&quot;: &quot;TV202307281234&quot;}
}

```

### 4.NLU意图识别模块设计

#### 4.1 核心类设计

##### 4.2.1 NluConfig

```
class NluConfig:
    label_map = {
        0: &quot;订单查询&quot;, 
        1: &quot;产品咨询&quot;,
        2: &quot;故障报修&quot;,
        3: &quot;其他&quot;
    }
    model_path = &#x27;../core/demand/models/tv-bert-base-chinese&#x27;
    model_pth_path = &#x27;../core/demand/nlu/nlu_best_model.pth&#x27;

```

![](img/53afd27c83f7c9724c66ec33daddfab3.png)

##### 4.2.2 BertNluAnalyzer

```
class BertNluAnalyzer:
    def __init__(self, config):
        # 上下文缓存队列
        self.context_cache = deque(maxlen=3)  # 保存最近3轮对话
        
    def predict_with_context(self, text):
        # 拼接历史对话
        context_text = &quot;[SEP]&quot;.join(self.context_cache) + &quot;[SEP]&quot; + text
        # BERT模型推理
        inputs = self.tokenizer(context_text, ...)
        logits = self.model(**inputs)
        # 更新上下文
        self.context_cache.append(text)

```

#### 4.3 关键特性

1.  **动态上下文窗口**
采用滑动窗口机制保留最近3轮对话
使用[SEP]标记分隔历史对话片段

1.  **多意图处理**

```
def get_multiple_intents(self, logits, threshold=0.3):
    probs = torch.softmax(logits, dim=-1)
    return [self.labels[i] for i, p in enumerate(probs) if p &gt; threshold]

```

1.  **领域适应策略**
电商领域专有词库注入
产品型号模糊匹配算法

### 5.NER实体识别模块设计

#### 5.1 技术架构图

![](img/d67538819ba80e2e67372568b86740c7.png)

#### 5.2 核心类设计

##### 5.2.1 NerConfig

```
class NerConfig:
    label_map = {
        &#x27;O&#x27;: 0,
        &#x27;B-ORDER_NUMBER&#x27;: 1,
        &#x27;I-ORDER_NUMBER&#x27;: 2,
        &#x27;B-PRODUCT_CODE&#x27;: 3,
        ...  # 其他实体类型
    }
    max_len = 128

```

##### 5.2.2 BertNerAnalyzer

```
class BertNerAnalyzer:
    def bert_predict(self, text):
        # 实体抽取流程
        encoding = self.tokenizer.encode_plus(...)
        logits = self.model(**encoding)
        preds = torch.argmax(logits, dim=2)
        
        # 后处理
        entities = self._merge_entities(preds)
        return self._format_validation(entities)

    def _merge_entities(self, preds):
        # 合并B-I标签的连续实体
        current_entity = None
        for token, label in preds:
            if label.startswith(&#x27;B-&#x27;):
                if current_entity: yield current_entity
                current_entity = {&#x27;type&#x27;: label[2:], &#x27;value&#x27;: token}
            elif label.startswith(&#x27;I-&#x27;):
                current_entity[&#x27;value&#x27;] += token
            else:
                if current_entity: yield current_entity
                current_entity = None

```

#### 5.3 关键算法

##### 5.3.1 订单号提取

```
def _extract_order_number(self, predictions):
    # 处理BERT分词带来的##符号问题
    return &#x27;&#x27;.join([token.replace(&#x27;##&#x27;,&#x27;&#x27;) for token, label in predictions 
                   if label in [&#x27;B-ORDER_NUMBER&#x27;,&#x27;I-ORDER_NUMBER&#x27;]])

```

##### 5.3.2 格式校验规则

```
def _validate_order_format(self, number):
    # 校验订单号规则: TV开头+12位数字
    if re.match(r&#x27;^TV\d{12}$&#x27;, number):
        return True
    return False

```

### 6.模块集成方案

#### 6.1 服务编排流程

![](img/bf014981c3cc15714c6a891612c21205.png)

#### 6.2 性能优化方案

**缓存策略**

```
class HybridCache:
    def get(self, text):
        # 先查本地内存缓存
        if text in self.mem_cache: 
            return self.mem_cache[text]
        # 再查Redis缓存
        return self.redis.get(text) or None

```

**批量推理优化**

```
def batch_predict(self, texts: List[str]):
    # 使用动态padding加速处理
    inputs = self.tokenizer(
        texts, 
        padding=True, 
        truncation=True,
        return_tensors=&quot;pt&quot;
    )
    with torch.inference_mode():
        return self.model(**inputs)

```

**GPU资源管理**

```
class GPUAllocator:
    def __enter__(self):
        torch.cuda.empty_cache()
        self.lock = acquire_gpu_lock()
        
    def __exit__(self, *args):
        release_gpu_lock(self.lock)

```

### 7.异常处理机制

#### 7.1 分级降级策略

![](img/116afce61f93240de9b812a83b48b792.png)

#### 7.2 监控指标

```
class HealthMonitor:
    METRICS = {
        &#x27;api_latency&#x27;: Gauge(&#x27;api_latency_seconds&#x27;, &#x27;API响应延迟&#x27;),
        &#x27;model_error&#x27;: Counter(&#x27;model_errors_total&#x27;, &#x27;模型推理错误次数&#x27;),
        &#x27;cache_hit&#x27;: Counter(&#x27;cache_hits_total&#x27;, &#x27;缓存命中次数&#x27;)
    }
    
    @classmethod
    def report_error(cls, module):
        cls.METRICS[&#x27;model_error&#x27;].labels(module=module).inc()

```

### 8.测试验证方案

#### 8.1 测试用例设计

##### 情感分析测试矩阵

![](img/06226a7fbed27a34f6ec1fe60b7edfcd.png)

##### 意图识别覆盖率

```
test_cases = [
    (&quot;我的订单到哪里了&quot;, &quot;订单查询&quot;),
    (&quot;空调不制冷怎么办&quot;, &quot;故障报修&quot;),
    (&quot;这款电视的尺寸&quot;, &quot;产品咨询&quot;),
    (&quot;天气不错&quot;, &quot;其他&quot;)
]

```

#### 8.2 压力测试指标

![](img/a0705f46e3e0e0e3f6acd415aa524e45.png)

#### 8.3 模型精度验证

```
# 情感分析混淆矩阵
|           | 预测正面 | 预测负面 | 预测非常负面 |
|-----------|---------|---------|-------------|
| 实际正面  | 98%     | 2%      | 0%          |
| 实际负面  | 5%      | 90%     | 5%          |
| 实际非常负面 | 0%     | 10%     | 90%         |

```

### 9.部署架构设计

#### 9.1 拓扑结构

![](img/dff9a02728541b29ac0d172ea7c52118.png)

#### 9.2 容器化配置

```
# 模型服务Dockerfile
FROM nvcr.io/nvidia/pytorch:22.10-py3

COPY requirements.txt .
RUN pip install -r requirements.txt

# 启用GPU共享
ENV CUDA_VISIBLE_DEVICES=0,1
ENV TF_FORCE_GPU_ALLOW_GROWTH=true

CMD [&quot;gunicorn&quot;, &quot;app:app&quot;, &quot;-k&quot;, &quot;uvicorn.workers.UvicornWorker&quot;, &quot;--timeout&quot;, &quot;120&quot;]

```

#### 9.3 弹性扩缩策略

```
# HPA配置
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
spec:
  minReplicas: 2
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 70
  - type: Resource
    resource:
      name: memory
      target:
        type: AverageValue
        averageValue: 2Gi

```

## 四、规划模块实现

### 1.模块整体设计思路

> **业务目标：**

> 将需求感知模块输出的意图（intent）、实体（entities）、情感（sentiment）转化为可执行动作序列

>

> **技术价值：**

> 实现规则驱动与LLM驱动的双模式决策，构建灵活的多工具执行体系


### 2.规划模块实现详解

#### 2.1 核心类设计

```
class PlanningEngine:
    def __init__(self):
        self.rules = {}          # 规则配置
        self.tool_mapping = {}   # 意图-工具映射表
        self.llm = ChatTongyi()  # 通义千问模型实例

    # 规则引擎（核心决策逻辑）
    def rule_plan(self, sentiment, intent, entities) -&gt; List[PlanStep]
    
    # LLM动态规划（复杂场景处理）
    def generate_plan(self, sentiment, intent, entities) -&gt; List[PlanStep]

```

#### 2.2 规则引擎实现逻辑

##### 2.2.1 规则加载机制

```
def _load_rules(self, path):
    &quot;&quot;&quot;从YAML文件加载规则配置（示例规则结构）&quot;&quot;&quot;
    return {
        &quot;escalation&quot;: {
            &quot;threshold&quot;: 0.8,        # 情感强度阈值
            &quot;action&quot;: &quot;notify_supervisor&quot;  # 触发动作
        }
    }

```

##### 2.2.2 规则应用流程

![](img/cce31f14fae7bcce9aedd5cf9fd5cb38.png)

##### 2.2.3 代码实现说明

```
def rule_plan(self, sentiment, intent, entities):
    # 情感阈值判断
    if sentiment.get(&quot;intensity&quot;, 0) &gt; self.rules[&quot;escalation&quot;][&quot;threshold&quot;]:
        return [PlanStep(...)]  # 强制升级
    
    # 常规意图映射
    tool = self.tool_mapping.get(intent, &quot;rag&quot;)
    return [PlanStep(
        step_id=&quot;step_1&quot;,
        description=intent,
        tool_required=tool,
        entities=entities
    )]

```

#### 2.3 LLM动态规划实现

##### 2.3.1 Prompt工程实现

```
# 模板设计要点：
# 1. 明确输出格式要求
# 2. 约束步骤数量（最多3步）
prompt_template = PromptTemplate(
    input_variables=[&quot;intent&quot;, &quot;entities&quot;, &quot;sentiment&quot;],
    template=&quot;&quot;&quot;请生成处理步骤：
    意图：{intent}
    实体：{entities}
    情感：{sentiment}
    要求：
    - 使用中文短句描述
    - 每行一个步骤
    - 最多3个步骤&quot;&quot;&quot;
)

```

##### 2.3.2 步骤解析逻辑

```
# 示例LLM输出：
&quot;&quot;&quot;
1. 验证订单号有效性
2. 查询订单物流信息
3. 生成查询报告
&quot;&quot;&quot;

steps = [
    PlanStep(step_id=&quot;step_0&quot;, ...),
    PlanStep(step_id=&quot;step_1&quot;, ...),
    PlanStep(step_id=&quot;step_2&quot;, ...)
]

```

##### 2.3.3 工具映射策略

```
def _map_tool(self, step_desc: str) -&gt; str:
    &quot;&quot;&quot;基于步骤描述的关键词匹配&quot;&quot;&quot;
    if &quot;订单&quot; in step_desc and (&quot;查询&quot; or &quot;验证&quot;) in step_desc:
        return &quot;order_query&quot;
    elif &quot;工单&quot; in step_desc and &quot;创建&quot; in step_desc:
        return &quot;work_order&quot;
    return &quot;rag&quot;

```

### 3.工具模块实现详解

#### 3.1 执行器核心架构

```
class ToolExecutor:
    def __init__(self):
        # API端点配置
        self.endpoints 
        
        # 知识库组件（示例伪代码）
        self.rag = &quot;示例向量库&quot; 

    def execute(self, step: PlanStep) -&gt; ToolResult:
        # 统一执行入口
        if step.tool_required in self.endpoints:
            return self._call_api(step)
        elif step.tool_required == &quot;rag&quot;:
            return self._rag_search(step)
        else:
            return ToolResult(status=&quot;error&quot;, ...)

```

#### 3.2 API工具执行流程

![](img/6a7a0777003012fc41bdd61747127a05.png)

#### 3.3 RAG工具实现

```
def _rag_search(self, step):
    &quot;&quot;&quot;知识库检索逻辑（当前为示例实现）&quot;&quot;&quot;
    return ToolResult(
        status=&quot;success&quot;,
        data={&quot;text&quot;: self.rag}  # 实际应替换为向量检索
    )

```

#### 3.4 异常处理机制

```
try:
    resp = requests.get(url, timeout=3)
    if resp.status_code != 200:
        # 记录错误日志
        return ToolResult(status=&quot;error&quot;, message=f&quot;API响应异常：{resp.status_code}&quot;)
except requests.exceptions.Timeout:
    return ToolResult(status=&quot;error&quot;, message=&quot;请求超时&quot;)
except Exception as e:
    return ToolResult(status=&quot;error&quot;, message=str(e))

```

### 4.业务流程完整示例

#### 4.1 典型业务场景

```
**用户输入****：“我的订单**12345为什么还没发货？”
需求感知输出：
• intent: 订单查询
• entities: {“order_number”: “12345”}
• sentiment: {“sentiment”: “负面”, “intensity”: 0.75}
```

#### 4.2 规划模块处理流程

1.  规则引擎判断情感强度0.75 < 0.8，不触发升级

1.  匹配意图"订单查询"到order_query工具

1.  生成步骤：

```
PlanStep(
    step_id=&quot;step_1&quot;,
    description=&quot;查询订单状态&quot;,
    tool_required=&quot;order_query&quot;,
    entities={&quot;order_number&quot;: &quot;12345&quot;}
)

```

#### 4.3 工具模块执行

```
{
    &quot;status&quot;: &quot;success&quot;,
    &quot;data&quot;: {
        &quot;order_status&quot;: &quot;已发货&quot;,
        &quot;tracking_number&quot;: &quot;SF123456789&quot;
    }
}

```

## 五、用户交互模块实现

### 1.模块整体架构设计

#### 1.1 核心流程

#### 1.2 流程说明

**情感驱动路由：**

> 所有情感类型均继续后续流程

> 差异点体现在处理优先级而非流程分支

**多轮会话机制：**

> 通过反馈回路实现对话延续

> 会话管理器维护上下文状态

**反馈闭环设计：**

> 用户显式反馈（如"不对，我要问的是…"）

> 隐式反馈（连续追问相同问题）

![](img/98a6c047ee1a058dcca0422af270e5b0.png)

#### 1.3 模块通信机制

```
# 与规划模块的接口定义
class PlanningRequest(BaseModel):
    user_intent: str
    entities: Dict[str, str]
    dialog_context: List[Dict]  # 复用对话历史数据结构

# 与工具模块的执行协议
class ToolInvocation(BaseModel):
    tool_name: str
    parameters: Dict[str, Any]  # 严格匹配知识库API规范

```

#### 1.2 数据流全景

![](img/fe95716422da7d9682389e0dc88d3fc2.png)

### 2.核心设计模式详解

#### 2.1 状态管理模式

采用**对话上下文感知**设计，实现三级状态管理：

```
class DialogState:
    session_state: Enum = ACTIVE  # 继承系统级状态机
    user_profile: UserMeta        # 对接用户中心数据
    service_context: Dict        # 业务上下文（如订单号）
    
    def snapshot(self) -&gt; str:
        &quot;&quot;&quot;生成状态快照（用于断点续传）&quot;&quot;&quot;
        return pickle.dumps(self.__dict__)

```

#### 2.2 风格引擎实现

```
class StyleEngine:
    def __init__(self, config_path: str):
        self.style_config = self._load_config(config_path)
        
    def apply_style(self, raw_text: str) -&gt; str:
        &quot;&quot;&quot;应用多维度风格规则&quot;&quot;&quot;
        # 语气转换（参考前文语言表达参数）
        if self.style_config[&#x27;tone&#x27;] == &#x27;formal&#x27;:
            text = self._convert_to_formal(raw_text)
            
        # 句式处理
        if self.style_config[&#x27;response_length&#x27;] == &#x27;short&#x27;:
            text = self._summarize_response(text)
            
        return text
    
    def _convert_to_formal(self, text: str) -&gt; str:
        &quot;&quot;&quot;敬语转换器&quot;&quot;&quot;
        replacements = {
            &quot;你&quot;: &quot;您&quot;,
            &quot;请&quot;: &quot;烦请&quot;,
            &quot;吗？&quot;: &quot;么？&quot;
        }
        for k, v in replacements.items():
            text = text.replace(k, v)
        return text

```

### 3.关键算法实现细节

#### 3.1 智能追问算法

```
def generate_clarification(context: DialogState) -&gt; str:
    &quot;&quot;&quot;
    基于强化学习的追问策略（对应前文会话管理机制）
    算法选择优先级：
    1. 必填字段缺失 → 直接追问
    2. 模糊实体 → 选项式提问
    3. 冲突信息 → 确认式提问
    &quot;&quot;&quot;
    if context.missing_entities:
        return _direct_question(context)
    elif context.conflict_entities:
        return _confirm_question(context)
    else:
        return _suggestive_question(context)

```

#### 3.2 多策略响应生成

```
def format_response(plan_step: PlanStep, 
                   result: ToolResult,
                   style: StyleConfig) -&gt; str:
    # 基础模板填充
    base_text = apply_template(plan_step.template, result.data)
    
    # 情感增强（对接情感分析模块）
    if style.allow_emotional_enhancement:
        base_text = add_emotion_words(base_text, result.sentiment)
        
    # 知识增强（调用RAG系统）
    if style.enable_knowledge_augment:
        related_info = retrieve_related_knowledge(base_text)
        base_text += f&quot;\n\n相关知识：{related_info}&quot;
    
    return base_text

```

### 4.工程化实践

#### 4.1 配置中心集成

```
# style_config.yaml（对应前文JSON配置方案）
response_policies:
  default:
    tone: professional
    emoji: false
    detail_level: 2
    
  vip_user:
    tone: friendly
    emoji: true
    detail_level: 3

```

#### 4.2 性能优化方案

（实现前文建议的Redis集成）

```
class RedisStateManager:
    def __init__(self, redis_conn):
        self.redis = redis_conn
        
    def save_state(self, state: DialogState):
        self.redis.set(
            state.session_id, 
            state.snapshot(),
            ex=3600  # 1小时会话有效期
        )
        
    def load_state(self, session_id: str) -&gt; DialogState:
        data = self.redis.get(session_id)
        return DialogState(**pickle.loads(data))

```

#### 4.3 用户交互模块完整代码

```
&quot;&quot;&quot;
智能客服用户交互模块完整实现
&quot;&quot;&quot;
from typing import Dict, List, Optional
from pydantic import BaseModel
import time
import json
from datetime import datetime

# region 基础数据结构（与规划模块、工具模块保持协议一致）
class PlanStep(BaseModel):
    &quot;&quot;&quot;规划步骤数据结构（与规划模块输出协议对齐）&quot;&quot;&quot;
    step_id: str
    tool_required: str
    parameters: Dict[str, str]
    retry_policy: str = &quot;default&quot;

class ToolResult(BaseModel):
    &quot;&quot;&quot;工具执行结果（与工具模块输出协议对齐）&quot;&quot;&quot;
    status: str  # success/partial/failure
    data: Dict[str, str]
    message: Optional[str] = None
# endregion

# region 核心交互引擎实现
class DialogState(BaseModel):
    &quot;&quot;&quot;
    对话状态模型
    &quot;&quot;&quot;
    session_id: str
    created_at: float = time.time()
    last_active: float = time.time()
    context_stack: List[Dict] = []
    pending_steps: List[PlanStep] = []
    missing_entities: List[str] = []

    def update_activity(self):
        &quot;&quot;&quot;更新最后活跃时间戳&quot;&quot;&quot;
        self.last_active = time.time()

class InteractionEngine:
    &quot;&quot;&quot;
    用户交互核心处理引擎
    
    &quot;&quot;&quot;
    def __init__(self, style_config: Dict):
        self.states: Dict[str, DialogState] = {}
        self.style_config = style_config
        
        # 实体校验规则加载（可热更新）
        self.entity_rules = self._load_entity_rules()
        
        # 响应模板初始化
        self.response_templates = {
            &#x27;order_query&#x27;: &quot;订单{order_number}状态：{status}&quot;,
            &#x27;refund&#x27;: &quot;退款申请已提交，金额：{amount}元&quot;
        }

    def process_input(self, user_id: str, text: str) -&gt; str:
        &quot;&quot;&quot;
        主处理流程
        参数说明：
        - user_id: 用户唯一标识符
        - text: 纯文本输入（已通过前置校验）
        &quot;&quot;&quot;
        # 获取对话状态
        state = self._get_or_create_state(user_id)
        
        # 调用需求感知模块（模拟接口调用）
        intent, entities, sentiment = self._call_perception_module(text)
        
        # 实体完整性校验
        if missing := self._validate_entities(intent, entities):
            return self._generate_clarification(missing)
        
        # 生成执行计划（调用规划模块）
        plan_steps = self._call_planning_module(intent, entities, sentiment)
        
        # 执行工具调用
        try:
            result = self._execute_tools(plan_steps[0])
        except Exception as e:
            return self._handle_error(e)
        
        # 生成风格化响应
        raw_response = self._format_response(plan_steps[0], result)
        styled_response = self._apply_style(raw_response, sentiment)
        
        # 更新对话历史
        self._update_dialog_history(state, text, styled_response)
        
        return styled_response

    def _call_perception_module(self, text: str) -&gt; tuple:
        &quot;&quot;&quot;模拟需求感知模块调用（实际应替换为RPC调用）&quot;&quot;&quot;
        # 此处简化实现，真实场景应调用独立模块
        return (&quot;order_query&quot;, {&quot;order_number&quot;: &quot;12345&quot;}, {&quot;sentiment&quot;: &quot;neutral&quot;})

    def _call_planning_module(self, intent: str, 
                             entities: Dict, 
                             sentiment: Dict) -&gt; List[PlanStep]:
        &quot;&quot;&quot;调用规划模块生成执行步骤&quot;&quot;&quot;
        # 示例生成订单查询步骤
        return [PlanStep(
            step_id=&quot;step1&quot;,
            tool_required=&quot;order_query&quot;,
            parameters=entities
        )]

    def _execute_tools(self, step: PlanStep) -&gt; ToolResult:
        &quot;&quot;&quot;执行工具调用（对接工具模块）&quot;&quot;&quot;
        # 示例订单查询结果
        return ToolResult(
            status=&quot;success&quot;,
            data={&quot;status&quot;: &quot;已发货&quot;, &quot;order_number&quot;: &quot;12345&quot;}
        )

    def _get_or_create_state(self, user_id: str) -&gt; DialogState:
        &quot;&quot;&quot;对话状态管理（带自动清理机制）&quot;&quot;&quot;
        if user_id not in self.states:
            self.states[user_id] = DialogState(
                session_id=f&quot;{user_id}_{datetime.now().timestamp()}&quot;
            )
        return self.states[user_id]

    def _validate_entities(self, intent: str, entities: Dict) -&gt; List[str]:
        &quot;&quot;&quot;实体校验核心逻辑&quot;&quot;&quot;
        required = self.entity_rules.get(intent, [])
        return [field for field in required if field not in entities]

    def _generate_clarification(self, missing: List[str]) -&gt; str:
        &quot;&quot;&quot;智能追问生成&quot;&quot;&quot;
        field_map = {
            &quot;order_number&quot;: &quot;订单号&quot;,
            &quot;product_code&quot;: &quot;产品编码&quot;
        }
        questions = [f&quot;需要您提供{filed_map.get(f, f)}&quot; for f in missing]
        return &quot;请补充以下信息：\n&quot; + &quot;\n&quot;.join(questions)

    def _format_response(self, step: PlanStep, result: ToolResult) -&gt; str:
        &quot;&quot;&quot;响应正文生成&quot;&quot;&quot;
        template = self.response_templates.get(step.tool_required, &quot;处理结果：{data}&quot;)
        return template.format(**result.data)

    def _apply_style(self, text: str, sentiment: Dict) -&gt; str:
        &quot;&quot;&quot;应用风格配置&quot;&quot;&quot;
        # 情感表情
        emoji_map = {
            &quot;positive&quot;: &quot;😊 &quot;,
            &quot;negative&quot;: &quot;😟 &quot;,
            &quot;neutral&quot;: &quot;🤖 &quot;
        }
        
        # 敬语处理
        if self.style_config.get(&quot;formal&quot;):
            text = text.replace(&quot;你&quot;, &quot;您&quot;)
            
        return f&quot;{emoji_map.get(sentiment[&#x27;sentiment&#x27;], &#x27;&#x27;)}{text}&quot;

    def _update_dialog_history(self, state: DialogState, 
                              input_text: str, 
                              response: str):
        &quot;&quot;&quot;对话历史管理（保留最近5轮）&quot;&quot;&quot;
        state.context_stack.append({
            &quot;timestamp&quot;: datetime.now().isoformat(),
            &quot;input&quot;: input_text,
            &quot;response&quot;: response
        })
        if len(state.context_stack) &gt; 5:
            state.context_stack.pop(0)

    def _load_entity_rules(self) -&gt; Dict:
        &quot;&quot;&quot;从配置文件加载实体规则&quot;&quot;&quot;
        # 此处简化实现，实际应从配置中心加载
        return {
            &quot;order_query&quot;: [&quot;order_number&quot;],
            &quot;refund&quot;: [&quot;order_number&quot;, &quot;amount&quot;]
        }

    def _handle_error(self, error: Exception) -&gt; str:
        &quot;&quot;&quot;统一错误处理&quot;&quot;&quot;
        return &quot;服务暂时不可用，请稍后再试&quot;
# endregion

# region 风格配置示例
STYLE_CONFIG = {
    &quot;formal&quot;: True,      # 是否使用正式语气
    &quot;emoji_enabled&quot;: True, # 是否启用表情符号
    &quot;detail_level&quot;: 2    # 信息详细程度（1-3）
}
# endregion

# region 单元测试
if __name__ == &quot;__main__&quot;:
    engine = InteractionEngine(style_config=STYLE_CONFIG)
    
    # 测试用例1：正常订单查询
    print(&quot;测试1：正常查询&quot;)
    response = engine.process_input(&quot;user1&quot;, &quot;查订单12345状态&quot;)
    print(response)
    
    # 测试用例2：缺失必要实体
    print(&quot;\n测试2：缺失实体&quot;)
    engine.response_templates[&quot;refund&quot;] = &quot;退款申请需要金额{amount}元&quot;
    response = engine.process_input(&quot;user2&quot;, &quot;我要退货&quot;)
    print(response)
# endregion

```