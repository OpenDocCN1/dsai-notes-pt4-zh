
# 花店数据库操作与查询

## 使用 sqlite3 操作 SQLite 数据库

### 导入 sqlite3 库

```python
import sqlite3
```

### 连接到数据库

```python
conn = sqlite3.connect('FlowerShop.db')
cursor = conn.cursor()
```

### 创建 Flowers 表

```python
cursor.execute('''
        CREATE TABLE Flowers (
            ID INTEGER PRIMARY KEY, 
            Name TEXT NOT NULL, 
            Type TEXT NOT NULL, 
            Source TEXT NOT NULL, 
            PurchasePrice REAL, 
            SalePrice REAL,
            StockQuantity INTEGER, 
            SoldQuantity INTEGER, 
            ExpiryDate DATE,  
            Description TEXT, 
            EntryDate DATE DEFAULT CURRENT_DATE 
        );
    ''')
```

### 插入鲜花数据

```python
flowers = [
    ('Rose', 'Flower', 'France', 1.2, 2.5, 100, 10, '2023-12-31', 'A beautiful red rose'),
    ('Tulip', 'Flower', 'Netherlands', 0.8, 2.0, 150, 25, '2023-12-31', 'A colorful tulip'),
    ('Lily', 'Flower', 'China', 1.5, 3.0, 80, 5, '2023-12-31', 'An elegant white lily'),
    ('Daisy', 'Flower', 'USA', 0.7, 1.8, 120, 15, '2023-12-31', 'A cheerful daisy flower'),
    ('Orchid', 'Flower', 'Brazil', 2.0, 4.0, 50, 2, '2023-12-31', 'A delicate purple orchid')
]

for flower in flowers:
    cursor.execute('''
        INSERT INTO Flowers (Name, Type, Source, PurchasePrice, SalePrice, StockQuantity, SoldQuantity, ExpiryDate, Description) 
        VALUES (?, ?, ?, ?, ?, ?, ?, ?, ?);
    ''', flower)
```

### 提交更改

```python
conn.commit()
```

### 关闭数据库连接

```python
conn.close()
```

> 这段Python代码使用了`sqlite3`库来创建和操作一个SQLite数据库。以下是每一部分的详细解释：

> + 导入`sqlite3`库：这是Python内置的轻量级数据库库。
> + 连接到数据库：`sqlite3.connect('FlowerShop.db')`创建了一个连接到名为`'FlowerShop.db'`的SQLite数据库的连接对象。如果这个数据库不存在，那么它将被创建。
> + 创建游标：`conn.cursor()`创建了一个游标对象，你可以使用它来执行SQL命令。
> + 创建表：`cursor.execute()`执行了一个SQL命令，创建了一个名为`'Flowers'`的表，这个表有多个字段，如`ID`、`Name`、`Type`等。
> + 插入数据：首先，创建了一个包含5种鲜花数据的列表。然后，对这个列表进行迭代，对于每种鲜花，都执行一个SQL `INSERT`命令，将鲜花的数据插入到`'Flowers'`表中。
> + 提交更改：`conn.commit()`提交了对数据库的更改。在这个点上，所有的更改都会被永久保存到数据库中。
> + 关闭数据库连接：`conn.close()`关闭了到数据库的连接。这是一个好的做法，因为它可以释放系统资源。

## 使用 Chain 查询数据库

```python
# 导入langchain的实用工具和相关的模块
from langchain.utilities import SQLDatabase
from langchain.llms import OpenAI
from langchain_experimental.sql import SQLDatabaseChain

# 连接到FlowerShop数据库（之前我们使用的是Chinook.db）
db = SQLDatabase.from_uri("sqlite:///FlowerShop.db")

# 创建OpenAI的低级语言模型（LLM）实例，这里我们设置温度为0，意味着模型输出会更加确定性
llm = OpenAI(temperature=0, verbose=True)

# 创建SQL数据库链实例，它允许我们使用LLM来查询SQL数据库
db_chain = SQLDatabaseChain.from_llm(llm, db, verbose=True)

# 运行与鲜花运营相关的问题
response = db_chain.run("有多少种不同的鲜花？")
print(response)

response = db_chain.run("哪种鲜花的存货数量最少？")
print(response)

response = db_chain.run("平均销售价格是多少？")
print(response)

response = db_chain.run("从法国进口的鲜花有多少种？")
print(response)

response = db_chain.run("哪种鲜花的销售量最高？")
print(response)
```

> 这段Python代码使用了`langchain`库来查询一个名为"FlowerShop.db"的SQLite数据库。以下是每一部分的详细解释：

> + 导入库：这部分代码导入了`langchain`库的一些模块。
> + 连接到数据库：使用`SQLDatabase.from_uri`方法创建了一个连接到名为`'FlowerShop.db'`的SQLite数据库的`SQLDatabase`对象。
> + 创建模型实例：使用`OpenAI`类创建了一个低级语言模型（LLM）实例，设置了温度为0，这意味着模型的输出会更加确定性。
> + 创建数据库链实例：使用`SQLDatabaseChain.from_llm`方法创建了一个`SQLDatabaseChain`实例，它允许我们使用LLM来查询SQL数据库。
> + 运行查询：使用`db_chain.run`方法运行了一系列与鲜花运营相关的问题，例如"有多少种不同的鲜花？"、"哪种鲜花的存货数量最少？"等，并打印出了每个问题的答案。
> + 输出：每个问题的答案都被打印出来。

## 使用 Agent 查询数据库

```python
import os

# os.environ["OPENAI_API_KEY"] = 'Your OpenAI Key'

from langchain.utilities import SQLDatabase
from langchain.llms import OpenAI
from langchain.agents import create_sql_agent
from langchain.agents.agent_toolkits import SQLDatabaseToolkit
from langchain.agents.agent_types import AgentType

# 连接到FlowerShop数据库
db = SQLDatabase.from_uri("sqlite:///FlowerShop.db")
llm = OpenAI(temperature=0, verbose=True)

# 创建SQL Agent
agent_executor = create_sql_agent(
    llm=llm,
    toolkit=SQLDatabaseToolkit(db=db, llm=llm),
    verbose=True,
    agent_type=AgentType.ZERO_SHOT_REACT_DESCRIPTION,
)

# 使用Agent执行SQL查询
questions = [
    "哪种鲜花的存货数量最少？",
    "平均销售价格是多少？",
]

for question in questions:
    response = agent_executor.run(question)
    print(response)
```

> 这段Python代码使用了`langchain`库来查询一个名为"FlowerShop.db"的SQLite数据库。以下是每一部分的详细解释：

> + 导入库：这部分代码导入了`langchain`库的一些模块。
> + 连接到数据库：使用`SQLDatabase.from_uri`方法创建了一个连接到名为`'FlowerShop.db'`的SQLite数据库的`SQLDatabase`对象。
> + 创建模型实例：使用`OpenAI`类创建了一个低级语言模型（LLM）实例，设置了温度为0，这意味着模型的输出会更加确定性。
> + 创建SQL Agent：使用`create_sql_agent`函数创建了一个SQL Agent，它使用了之前创建的LLM和数据库连接，并设置了类型为`ZERO_SHOT_REACT_DESCRIPTION`。
> + 运行查询：使用`agent_executor.run`方法运行了一系列与鲜花运营相关的问题，例如"哪种鲜花的存货数量最少？"、"平均销售价格是多少？"等，并打印出了每个问题的答案。

## Callback：引入异步通信机制

### 回调函数范例

```python
def compute(x, y, callback):
    result = x + y
    callback(result)

def print_result(value):
    print(f"The result is: {value}")

def square_result(value):
    print(f"The squared result is: {value**2}")

# 使用print_result作为回调
compute(3, 4, print_result)  # 输出: The result is: 7

# 使用square_result作为回调
compute(3, 4, square_result)  # 输出: The squared result is: 49
```

> 这段Python代码定义了一个名为`compute`的函数，它接受两个数字和一个回调函数作为参数。`compute`函数将两个数字相加，然后将结果传递给回调函数。

> 此外，定义了两个可以作为回调函数的函数：`print_result`和`square_result`。`print_result`函数接受一个值并打印出来，而`square_result`函数接受一个值，将其平方后再打印出来。

> 在代码的最后两行，`compute`函数被调用了两次，每次都传入了不同的回调函数。第一次调用`compute`时，传入了`print_result`作为回调函数，所以它将打印出"3 + 4 = 7"的结果。第二次调用`compute`时，传入了`square_result`作为回调函数，所以它将打印出"(3 + 4)² = 49"的结果。

```python
%autoawait

import asyncio

async def compute(x, y, callback):
    print("Starting compute...")
    await asyncio.sleep(0.5)  # 模拟异步操作
    result = x + y
    # callback(result)
    print("Finished compute...")

def print_result(value):
    print(f"The result is: {value}")    

async def another_task():
    print("Starting another task...")
    await asyncio.sleep(1)
    print("Finished another task...")

async def main():
    print("Main starts...")
    task1 = asyncio.create_task(compute(3, 4, print_result))
    task2 = asyncio.create_task(another_task())
    await task1
    await task2
    print("Main ends...")

await main()
```

> 这段Python代码是一个简单的异步编程示例，使用了Python的`asyncio`库。以下是每部分的详细解释：

> + `%autoawait`：这是一个IPython魔法命令，它允许你在cell中直接使用`await`关键字，而不需要将它包含在`async def`函数中。
> + `import asyncio`：这行代码导入了Python的`asyncio`库，这是Python的内置库，用于处理异步I/O操作。
> + `async def compute(x, y, callback):`：这是一个异步函数，它接收两个数字和一个回调函数作为参数，然后模拟异步操作（通过`await asyncio.sleep(0.5)`暂停0.5秒），然后计算两个数字的和，并打印出"Finished compute..."。
> + `def print_result(value):`：这是一个普通的同步函数，它接收一个值作为参数，并打印出这个值。
> + `async def another_task():`：这是另一个异步函数，它模拟异步操作（通过`await asyncio.sleep(1)`暂停1秒），然后打印出"Finished another task..."。
> + `async def main():`：这是主异步函数，它首先打印出"Main starts..."，然后创建两个异步任务（`task1`和`task2`），并等待这两个任务完成，最后打印出"Main ends..."。
> + `await main()`：这行代码运行主异步函数。因为我们在cell的开始使用了`%autoawait`魔法命令，所以我们可以直接在cell中使用`await`关键字。

### 在组件中使用 Callback 处理器

安装包

```bash
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple loguru==0.7.2
```

```python
from loguru import logger
from langchain.callbacks import FileCallbackHandler
from langchain.chains import LLMChain
from langchain.llms import OpenAI
from langchain.prompts import PromptTemplate

logfile = "output.log"
logger.add(logfile, colorize=True, enqueue=True)

handler = FileCallbackHandler(logfile)

llm = OpenAI()
prompt = PromptTemplate.from_template("1 + {number} = ")

# this chain will both print to stdout (because verbose=True) and write to 'output.log'
# if verbose=False, the FileCallbackHandler will still write to 'output.log'

chain = LLMChain(llm=llm, prompt=prompt, callbacks=[handler], verbose=True)

answer = chain.run(number=2)

logger.info(answer)
```
