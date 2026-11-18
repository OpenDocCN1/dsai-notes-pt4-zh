
```
import openai
import pinecone
import time
from collections import deque
from typing import Dict, List

#Set API Keys
OPENAI_API_KEY = ""
PINECONE_API_KEY = ""
PINECONE_ENVIRONMENT = "us-east1-gcp" #Pinecone Environment (eg. "us-east1-gcp")

#Set Variables
YOUR_TABLE_NAME = "test-table"
OBJECTIVE = "Solve world hunger."
YOUR_FIRST_TASK = "Develop a task list."

#Print OBJECTIVE
print("\033[96m\033[1m"+"\n*****OBJECTIVE*****\n"+"\033[0m\033[0m")
print(OBJECTIVE)

# Configure OpenAI and Pinecone
openai.api_key = OPENAI_API_KEY
pinecone.init(api_key=PINECONE_API_KEY, environment=PINECONE_ENVIRONMENT)

# Create Pinecone index
table_name = YOUR_TABLE_NAME
dimension = 1536
metric = "cosine"
pod_type = "p1"
if table_name not in pinecone.list_indexes():
    pinecone.create_index(table_name, dimension=dimension, metric=metric, pod_type=pod_type)

# Connect to the index
index = pinecone.Index(table_name)

# Task list
task_list = deque([])

def add_task(task: Dict):
    task_list.append(task)

def get_ada_embedding(text):
    text = text.replace("\n", " ")
    return openai.Embedding.create(input=[text], model="text-embedding-ada-002")["data"][0]["embedding"]

def task_creation_agent(objective: str, result: Dict, task_description: str, task_list: List[str]):
    prompt = f"You are an task creation AI that uses the result of an execution agent to create new tasks with the following objective: {objective}, The last completed task has the result: {result}. This result was based on this task description: {task_description}. These are incomplete tasks: {', '.join(task_list)}. Based on the result, create new tasks to be completed by the AI system that do not overlap with incomplete tasks. Return the tasks as an array."
    response = openai.Completion.create(engine="text-davinci-003",prompt=prompt,temperature=0.5,max_tokens=100,top_p=1,frequency_penalty=0,presence_penalty=0)
    new_tasks = response.choices[0].text.strip().split('\n')
    return [{"task_name": task_name} for task_name in new_tasks]

def prioritization_agent(this_task_id:int):
    global task_list
    task_names = [t["task_name"] for t in task_list]
    next_task_id = int(this_task_id)+1
    prompt = """You are an task prioritization AI tasked with cleaning the formatting of and reprioritizing the following tasks: {task_names}. Consider the ultimate objective of your team:{OBJECTIVE}. Do not remove any tasks. Return the result as a numbered list, like:
    #. First task
    #. Second task
    Start the task list with number {next_task_id}."""
    response = openai.Completion.create(engine="text-davinci-003",prompt=prompt,temperature=0.5,max_tokens=1000,top_p=1,frequency_penalty=0,presence_penalty=0)
    new_tasks = response.choices[0].text.strip().split('\n')
    task_list = deque()
    for task_string in new_tasks:
        task_parts = task_string.strip().split(".", 1)
        if len(task_parts) == 2:
            task_id = task_parts[0].strip()
            task_name = task_parts[1].strip()
            task_list.append({"task_id": task_id, "task_name": task_name})

def execution_agent(objective:str,task: str) -> str:
    #context = context_agent(index="quickstart", query="my_search_query", n=5)
    context=context_agent(index=YOUR_TABLE_NAME, query=objective, n=5)
    #print("\n*******RELEVANT CONTEXT******\n")
    #print(context)
    response = openai.Completion.create(
        engine="text-davinci-003",
        prompt=f"You are an AI who performs one task based on the following objective: {objective}. Your task: {task}\nResponse:",
        temperature=0.7,
        max_tokens=2000,
        top_p=1,
        frequency_penalty=0,
        presence_penalty=0
    )
    return response.choices[0].text.strip()

def context_agent(query: str, index: str, n: int):
    query_embedding = get_ada_embedding(query)
    index = pinecone.Index(index_name=index)
    results = index.query(query_embedding, top_k=n,
    include_metadata=True)
    #print("***** RESULTS *****")
    #print(results)
    sorted_results = sorted(results.matches, key=lambda x: x.score, reverse=True)    
    return [(str(item.metadata['task'])) for item in sorted_results]

# Add the first task
first_task = {
    "task_id": 1,
    "task_name": YOUR_FIRST_TASK
}

add_task(first_task)
# Main loop
task_id_counter = 1
while True:
    if task_list:
        # Print the task list
        print("\033[95m\033[1m"+"\n*****TASK LIST*****\n"+"\033[0m\033[0m")
        for t in task_list:
            print(str(t['task_id'])+": "+t['task_name'])

        # Step 1: Pull the first task
        task = task_list.popleft()
        print("\033[92m\033[1m"+"\n*****NEXT TASK*****\n"+"\033[0m\033[0m")
        print(str(task['task_id'])+": "+task['task_name'])

        # Send to execution function to complete the task based on the context
        result = execution_agent(OBJECTIVE,task["task_name"])
        this_task_id = int(task["task_id"])
        print("\033[93m\033[1m"+"\n*****TASK RESULT*****\n"+"\033[0m\033[0m")
        print(result)

        # Step 2: Enrich result and store in Pinecone
        enriched_result = {'data': result}  # This is where you should enrich the result if needed
        result_id = f"result_{task['task_id']}"
        vector = enriched_result['data']  # extract the actual result from the dictionary
        index.upsert([(result_id, get_ada_embedding(vector),{"task":task['task_name'],"result":result})])

    # Step 3: Create new tasks and reprioritize task list
    new_tasks = task_creation_agent(OBJECTIVE,enriched_result, task["task_name"], [t["task_name"] for t in task_list])

    for new_task in new_tasks:
        task_id_counter += 1
        new_task.update({"task_id": task_id_counter})
        add_task(new_task)
    prioritization_agent(this_task_id)

time.sleep(1)  # Sleep before checking the task list again
```

以上是来自于 `babyagi` 仓库的源码，总行数140行涵盖 `prompt`，同时任务为三个 `agent` 进行协同组织。

这个是 `babyagi` 的 webui 演示 https://babyagi-ui.vercel.app/zh，同学们可以先体验一下了解一下 `babyagi` 的输入输出的 workflow 结合上图用 `MG` 进行重写。

其中 `MG` 已经抽象好了许多上层类，以及 react 的规划模式和 actions 列表。

思考部分：

1. 什么是 `enrich`

1. 何时应该 creat new task，何时应该排序任务优先级

1. 新的 new task 应该观察什么作为创建的依据（当前任务列表/目标/已完成的任务结果）

1. 人类是否可以介入这个流程比如新任务的合入审核，任务执行时的拆解？

你不一定要完全依据源码的逻辑进行重写，尝试找到更优秀的 SOP。

> `Chroma`/`Weaviate` 来存储和检索上下文的任务结果。该脚本根据 `TABLE_NAME` 变量中指定的表名创建 `Chroma`/`Weaviate` 集合。然后使用 `Chroma`/`Weaviate` 将任务结果以及任务名称和任何其他元数据存储在集合中。而向量数据库并不是本次学习的重点即便 `MG` 的 example 里有对向量检索进行支持，理论上 `loop` 的次数较少时是可以将上下文的任务结果完全的作为 `prompt` 的输入，所以在本次学习中可以将上下文的任务结果写入一个外部的 `txt` 或者直接在 `memory` 中调用的，学习者可以不使用向量数据库完成本次任务

# 第五章：Data Interpreter（DI）

## 什么是 Data Interpreter（DI）

数据解释器是一个通过编写代码来解决数据相关问题的智能体。它能理解用户需求，制定计划，编写执行代码，并在必要时使用工具。这些能力使它能够处理广泛的场景。

数据解释器（Data Interpreter）提出了三个关键技术：

1）基于分层图结构的动态计划，基于分层的图结构进行任务和代码规划，有效管理任务间的复杂依赖，灵活应对数据科学任务的实时数据变化；

2）工具集成与进化，通过在代码生成过程中自动集成代码片段作为工具，动态嵌入了数据科学领域所需的领域知识；

3）基于验证与经验驱动的推理，自动在反馈中增强逻辑一致性检测，通过基于置信度的验证提升执行代码的逻辑合理性，并借助经验库增强推理能力。

Data Interpreter 在数学推理任务，机器学习任务和复杂的开放性任务上达到了 sota 水平。相对其他开源基线，Data Interpreter 在 MATH 数学推理任务中准确率提升了26%，在 ML-Benchmark 机器学习任务中将得分从0.86提升至0.95，在开放式任务 Open-ended tasks 中更是实现了112%的惊人提升。

> [数据解释器](https://deepwisdom.feishu.cn/wiki/Praxw5RxEi4NgGkrofJc54QdnXg)需要了解更多 Data Interpreter，请查看

更多内容正在骑马来的路上🐎（欢迎优秀学习者参与内测）
