# 通过MCP+数据库实现AI检索和分析

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/UppUddpdBoR0InxTl9gcUi1Znte](https://qqs7y1hozd1.feishu.cn/docx/UppUddpdBoR0InxTl9gcUi1Znte)

通过 MCP（Multi-Agent Collaboration Platform，多智能体协作平台）+ 数据库，实现一个AI检索和分析系统。

## 一、系统目标

实现通过 AI 多智能体对结构化（数据库）和非结构化（文档、文本）数据进行：

> ✅ 智能检索（自然语言问答、SQL 自动生成）

> ✅ 智能分析（总结、趋势识别、异常检测、风险分析等）

> ✅ 多模型协同分析 + 决策建议

> ✅ 结果推送（如 ECM、BPM 系统对接、前端仪表盘展示）

## 二、技术组成与框架结构

整体结构如下：

```
[ 用户 ] ⇄ [ MCP Server ] ⇄ [ Agent / LangChain Tool ] ⇄ [ 数据库 / 文档系统 ]
                             ⇅
                     [ 多模型分析引擎 ]
```

1.  MCP Server

+   支持用户通过自然语言输入查询意图（如：“分析近三月销售异常”）

+   调用 Agent 工厂，动态分配合适的 AI Agent 进行任务分解与协作

+   负责中控调度、缓存上下文、流程记录和结果集成

1.  数据检索模块（LangChain + 数据库工具）

+   使用 LangChain SQLDatabaseChain 或 SQLAgent 工具对接数据库

+   支持自然语言转 SQL（通过 GPT/Claude 等模型）

+   数据库返回结构化数据（DataFrame）

+   对于多表分析，使用 SQLGraph 工具做自动联表推理

1.  AI 分析 Agent

+   负责对结构化数据进行统计分析、趋势识别、聚类等

+   可通过 Python Agent（LangChain 的 Pandas DataFrame Agent）进行数据分析（用模型调用 pandas、matplotlib 等进行分析）

1.  多模型投票机制（可选）

+   通过调用 Claude/GPT/Mistral 等多个模型

+   对分析结果进行比对和投票（如多个模型都认为某条数据是风险项，则置信度更高）

1.  分析结果输出模块

+   可以将结果推送到：

+   ECM 系统（如 IBM FileNet）做文档记录

+   BPM 系统（如 IBM BPM）发起审批流程

+   前端仪表盘展示（如 React + Tailwind + Charts）

## 三、功能示例

示例 1：自然语言问数据库

> 用户输入：「帮我查一下过去3个月每个地区的销售趋势，并找出异常波动。」

执行流程：

1.  MCP 调用 SQLAgent 将用户意图翻译为 SQL

1.  查询数据库中相关销售表

1.  将数据交给 Pandas Agent 进行趋势图绘制 + 异常检测

1.  返回分析结果 + 图表

1.  可选：将异常记录推送到 BPM 发起“销售异常核查”流程

示例 2：合同数据分析（结合 ECM）

> **用户输入：「找出近30天签署的合同**中，金额超过100万的，并做风险分析。」

1.  查询数据库 + ECM 中文档（合同 PDF 或条款摘要）

1.  使用 NLP 模型（或 Agent）读取文本内容进行分析（如违约风险）

1.  多模型评估结果（Claude/GPT/Mistral 投票）

1.  推送分析结果到 ECM 系统的备注字段或 BPM 流程中

## 四、数据库接入方式（MySQL/PostgreSQL 示例）

可以使用 LangChain 中的 SQLDatabase 对象连接数据库：

```
from langchain.utilities import SQLDatabase
 
db = SQLDatabase.from_uri(&quot;mysql+pymysql://user:password@host:3306/database_name&quot;)
```

然后使用 SQLDatabaseChain 或 SQLAgent 对接：

```
from langchain.chains import SQLDatabaseChain
from langchain.chat_models import ChatOpenAI
 
llm = ChatOpenAI(model_name=&quot;gpt-4&quot;)
db_chain = SQLDatabaseChain.from_llm(llm, db)
 
response = db_chain.run(&quot;显示最近三个月销售额趋势&quot;)
```

## 五、搭建一个本地 MCP + SQLite 的 AI 数据分析原型系统，使用 LangChain + GPT（或本地模型）实现自然语言到 SQL，再通过 Pandas 分析数据。

### 第一步：系统核心结构设计

我们先构建一个最简 MVP（最小可行产品）版本，包含以下模块：

#### **系统模块划分**

### 第二步：环境准备

安装依赖（建议新建虚拟环境）

pip install langchain openai sqlite3 pandas matplotlib streamlit

**你可以用 gpt-3.5-turbo 作为 LLM**，也可以对接本地模型（如 Ollama）

###  第三步：原型代码结构

我们先建立如下文件结构：

```
mcp_sqlite_demo/
│
├── main.py                # 主入口
├── db_init.py             # 创建并填充 SQLite 示例数据
├── mcp_server.py          # 模拟 MCP 调度器
├── agent_sql.py           # SQL Agent 实现
├── agent_analysis.py      # 数据分析 Agent 实现
```

### 第四步：编写关键代码

1.  创建示例数据库（db_init.py）

```
import sqlite3
import random
from datetime import datetime, timedelta
 
conn = sqlite3.connect(&quot;sales.db&quot;)
cur = conn.cursor()
 
cur.execute(&quot;DROP TABLE IF EXISTS sales&quot;)
cur.execute(&quot;&quot;&quot;
CREATE TABLE sales (
    id INTEGER PRIMARY KEY,
    region TEXT,
    amount REAL,
    sale_date TEXT
)
&quot;&quot;&quot;)
 
regions = [&quot;华东&quot;, &quot;华南&quot;, &quot;华北&quot;, &quot;西南&quot;, &quot;东北&quot;]
start_date = datetime(2023, 12, 1)
 
for i in range(90):
    for region in regions:
        date = (start_date + timedelta(days=i)).strftime(&quot;%Y-%m-%d&quot;)
        amount = round(random.uniform(5000, 50000), 2)
        cur.execute(&quot;INSERT INTO sales (region, amount, sale_date) VALUES (?, ?, ?)&quot;,
                    (region, amount, date))
 
conn.commit()
conn.close()
print(&quot;数据库初始化完成 ✅&quot;)
```

运行后会生成一个 sales.db 的销售数据库。

1.  SQL Agent（agent_sql.py）

```
from langchain.utilities import SQLDatabase
from langchain.agents import create_sql_agent
from langchain.agents.agent_toolkits import SQLDatabaseToolkit
from langchain.chat_models import ChatOpenAI
 
def get_sql_agent():
    db = SQLDatabase.from_uri(&quot;sqlite:///sales.db&quot;)
    llm = ChatOpenAI(model=&quot;gpt-3.5-turbo&quot;, temperature=0)
    toolkit = SQLDatabaseToolkit(db=db, llm=llm)
    agent_executor = create_sql_agent(
        llm=llm,
        toolkit=toolkit,
        verbose=True,
    )
    return agent_executor
```

3. 分析 Agent（agent_analysis.py）

```
import pandas as pd
import matplotlib.pyplot as plt
 
def analyze_sales(df):
    print(&quot;\n🔍 正在分析销售趋势...&quot;)

    trend = df.groupby(&quot;sale_date&quot;)[&quot;amount&quot;].sum()
    trend.plot(kind=&quot;line&quot;, title=&quot;每日销售趋势&quot;, figsize=(10, 5))
    plt.tight_layout()
    plt.savefig(&quot;sales_trend.png&quot;)

    max_day = trend.idxmax()
    print(f&quot;\n📈 销售峰值日期：{max_day}，金额：{trend.max():,.2f}&quot;)
```

4. MCP Server 主控（mcp_server.py）

```
from agent_sql import get_sql_agent
from agent_analysis import analyze_sales
import pandas as pd
import sqlite3
 
def run_query(natural_language_query):
    print(f&quot;\n🧠 MCP 收到请求：{natural_language_query}&quot;)
 
    agent = get_sql_agent()
    sql_response = agent.run(natural_language_query)
 
    print(&quot;\n📊 SQL Agent 执行结果如下：&quot;)
    print(sql_response)
 
    # 执行 SQL 再交给分析模块
    db = sqlite3.connect(&quot;sales.db&quot;)
    df = pd.read_sql_query(&quot;SELECT * FROM sales&quot;, db)
    analyze_sales(df)
```

1.  启动主程序（main.py）

```
fro**m mc**p_server import run_query
 
if name == &quot;__main__&quot;:
    print(&quot;欢迎使用 MCP + SQLite 智能分析系统&quot;)
    while True:
        query = input(&quot;\n💬 请输入分析请求（或 q 退出）：&quot;)
        if query.lower() in [&#x27;q&#x27;, &#x27;exit&#x27;]:
            break
        run_query(query)
```

## 第五步：运行测试

先初始化数据库：

> python db_init.py

然后运行主程序：

> **python main.py**

示例输入：

> 过去30天每个地区销售趋势如何？有没有异常波动？

## 六、可运行的本地系统模板

把这个 MCP + SQLite 的 AI 分析系统打造成一个完整可运行的本地系统模板，包括：

+   ✅ 完整代码结构

+   ✅ Streamlit 前端界面

+   ✅ Docker 容器化部署支持

+   ✅ 可扩展的多 Agent 结构

#### 1.项目结构设计（最终版）

我们建立如下目录结构：

```
mcp_sqlite_ai/
│
├── app/
│   ├── __init__.py
│   ├── db_init.py               # 初始化 SQLite 示例数据
│   ├── mcp_server.py            # MCP 调度模块
│   ├── agent_sql.py             # SQL Agent
│   ├── agent_analysis.py        # 分析 Agent
│   └── config.py                # 模型配置
│
├── frontend/
│   └── dashboard.py             # Streamlit 前端交互界面
│
├── Dockerfile                   # Docker 镜像构建
├── requirements.txt             # 依赖列表
└── run.sh                       # 一键运行脚本
```

#### 2.文件内容逐个说明

✅ app/db_init.py

# 初始化数据库

```
imp**ort **sqlite3
import random
from datetime import datetime, timedelta
 
def init_db():
    conn = sqlite3.connect(&quot;sales.db&quot;)
    cur = conn.cursor()
    cur.execute(&quot;DROP TABLE IF EXISTS sales&quot;)
    cur.execute(&quot;&quot;&quot;
    CREATE TABLE sales (
        id INTEGER PRIMARY KEY,
        region TEXT,
        amount REAL,
        sale_date TEXT
    )&quot;&quot;&quot;)

    regions = [&quot;华东&quot;, &quot;华南&quot;, &quot;华北&quot;, &quot;西南&quot;, &quot;东北&quot;]
    start_date = datetime(2023, 12, 1)

    for i in range(90):
        for region in regions:
            date = (start_date + timedelta(days=i)).strftime(&quot;%Y-%m-%d&quot;)
            amount = round(random.uniform(5000, 50000), 2)
            cur.execute(&quot;INSERT INTO sales (region, amount, sale_date) VALUES (?, ?, ?)&quot;,
                        (region, amount, date))

    conn.commit()
    conn.close()
    print(&quot;✅ SQLite 数据库初始化完成&quot;)
 
if name == &quot;__main__&quot;:
    init_db()
```

✅ app/config.py

> **O**PENAI_API_KEY = "your-openai-key"  # 替换成你自己的，或使用环境变量加载

✅ app/agent_sql.py

```
from langchain.utilities import SQLDatabase
from langchain.agents import create_sql_agent
from langchain.agents.agent_toolkits import SQLDatabaseToolkit
from langchain.chat_models import ChatOpenAI
from app.config import OPENAI_API_KEY
 
def get_sql_agent():
    db = SQLDatabase.from_uri(&quot;sqlite:///sales.db&quot;)
    llm = ChatOpenAI(model=&quot;gpt-3.5-turbo&quot;, temperature=0, openai_api_key=OPENAI_API_KEY)
    toolkit = SQLDatabaseToolkit(db=db, llm=llm)
    return create_sql_agent(llm=llm, toolkit=toolkit, verbose=True)
```

✅ app/agent_analysis.py

```
import pandas as pd
import matplotlib.pyplot as plt
 
def analyze_sales(df, output_path=&quot;frontend/sales_plot.png&quot;):
    trend = df.groupby(&quot;sale_date&quot;)[&quot;amount&quot;].sum()
    trend.plot(kind=&quot;line&quot;, title=&quot;每日销售趋势&quot;, figsize=(10, 5))
    plt.tight_layout()
    plt.savefig(output_path)
    plt.close()
    return f&quot;趋势图已生成：{output_path}&quot;
```

✅ app/mcp_server.py

```
import sqlite3
import pandas as pd
from app.agent_sql import get_sql_agent
from app.agent_analysis import analyze_sales
 
def run_mcp_query(query):
    agent = get_sql_agent()
    response = agent.run(query)

    conn = sqlite3.connect(&quot;sales.db&quot;)
    df = pd.read_sql_query(&quot;SELECT * FROM sales&quot;, conn)
    plot_path = analyze_sales(df)

    return response, plot_path
```

✅ frontend/dashboard.py （Streamlit 前端）

```
import streamlit as st
from app.mcp_server import run_mcp_query
 
st.set_page_config(page_title=&quot;AI 智能销售分析&quot;, layout=&quot;centered&quot;)
 
st.title(&quot;📊 AI 销售数据分析平台&quot;)
query = st.text_input(&quot;请输入分析请求（如：过去30天销售趋势）&quot;, &quot;&quot;)
 
if st.button(&quot;提交分析&quot;) and query.strip():
    with st.spinner(&quot;正在分析，请稍候...&quot;):
        response, img_path = run_mcp_query(query)
        st.success(&quot;分析完成&quot;)
        st.image(img_path, caption=&quot;销售趋势图&quot;, use_column_width=True)
        st.markdown(&quot;### 🤖 SQL Agent 结果&quot;)
        st.markdown(response)
```

✅ requirements.txt

```
langchain&gt;=0.1.0
openai&gt;=1.0.0
pandas
matplotlib
streamlit
sqlite3
```

✅ Dockerfile

```
FROM python:3.10-slim
 
WORKDIR /app
 
COPY . /app
RUN pip install --upgrade pip &amp;&amp; \
    pip install -r requirements.txt
 
EXPOSE 8501
 
CMD [&quot;streamlit&quot;, &quot;run&quot;, &quot;frontend/dashboard.py&quot;, &quot;--server.port=8501&quot;, &quot;--server.address=0.0.0.0&quot;]
```

✅ run.sh

```
#!/bin/bash
echo &quot;初始化数据库...&quot;
python3 app/db_init.py
 
echo &quot;启动前端界面...&quot;
streamlit run frontend/dashboard.py
```

## 3.启动方式

👉 本地运行：

chmod +x run.sh

./run.sh

浏览器访问：http://localhost:8501

🐳 Docker 容器运行：

docker build -t mcp-ai-agent .