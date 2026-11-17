
```
from hello_agents.protocols import A2AServer, A2AClient
import threading
import time

# 1. 创建多个Agent服务
researcher = A2AServer(
    name=&quot;researcher&quot;,
    description=&quot;研究员&quot;)@researcher.skill(&quot;research&quot;)def do_research(text: str) -&gt; str:import re
    match = re.search(r&#x27;research\s+(.+)&#x27;, text, re.IGNORECASE)
    topic = match.group(1).strip() if match else text
    return str({&quot;topic&quot;: topic, &quot;findings&quot;: f&quot;{topic}的研究结果&quot;})

writer = A2AServer(
    name=&quot;writer&quot;,
    description=&quot;撰写员&quot;)@writer.skill(&quot;write&quot;)def write_article(text: str) -&gt; str:import re
    match = re.search(r&#x27;write\s+(.+)&#x27;, text, re.IGNORECASE)
    content = match.group(1).strip() if match else text
    
    # 尝试解析研究数据try:
        data = eval(content)
        topic = data.get(&quot;topic&quot;, &quot;未知主题&quot;)
        findings = data.get(&quot;findings&quot;, &quot;无研究结果&quot;)except:
        topic = &quot;未知主题&quot;
        findings = content
    
    return f&quot;# {topic}\n\n基于研究：{findings}\n\n文章内容...&quot;

editor = A2AServer(
    name=&quot;editor&quot;,
    description=&quot;编辑&quot;)@editor.skill(&quot;edit&quot;)def edit_article(text: str) -&gt; str:import re
    match = re.search(r&#x27;edit\s+(.+)&#x27;, text, re.IGNORECASE)
    article = match.group(1).strip() if match else text
    
    result = {&quot;article&quot;: article + &quot;\n\n[已编辑优化]&quot;,&quot;feedback&quot;: &quot;文章质量良好&quot;,&quot;approved&quot;: True}return str(result)# 2. 启动所有服务
threading.Thread(target=lambda: researcher.run(port=5000), daemon=True).start()
threading.Thread(target=lambda: writer.run(port=5001), daemon=True).start()
threading.Thread(target=lambda: editor.run(port=5002), daemon=True).start()
time.sleep(2)  # 等待服务启动# 3. 创建客户端连接到各个Agent
researcher_client = A2AClient(&quot;http://localhost:5000&quot;)
writer_client = A2AClient(&quot;http://localhost:5001&quot;)
editor_client = A2AClient(&quot;http://localhost:5002&quot;)# 4. 协作流程def create_content(topic):# 步骤1：研究
    research = researcher_client.execute_skill(&quot;research&quot;, f&quot;research {topic}&quot;)
    research_data = research.get(&#x27;result&#x27;, &#x27;&#x27;)# 步骤2：撰写
    article = writer_client.execute_skill(&quot;write&quot;, f&quot;write {research_data}&quot;)
    article_content = article.get(&#x27;result&#x27;, &#x27;&#x27;)# 步骤3：编辑
    final = editor_client.execute_skill(&quot;edit&quot;, f&quot;edit {article_content}&quot;)return final.get(&#x27;result&#x27;, &#x27;&#x27;)# 使用
result = create_content(&quot;AI在医疗领域的应用&quot;)print(f&quot;\n最终结果：\n{result}&quot;)
```

### 10.3.4 在智能体中使用 A2A 工具

现在让我们看看如何将 A2A 集成到 HelloAgents 的智能体中。

（1）使用 `A2ATool` 包装器

```
from hello_agents import SimpleAgent, HelloAgentsLLM
from hello_agents.tools import A2ATool
from dotenv import load_dotenv

load_dotenv()
llm = HelloAgentsLLM()# 假设已经有一个研究员Agent服务运行在 http://localhost:5000# 创建协调者Agent
coordinator = SimpleAgent(name=&quot;协调者&quot;, llm=llm)# 添加A2A工具，连接到研究员Agent
researcher_tool = A2ATool(
    name=&quot;researcher&quot;,
    description=&quot;研究员Agent，可以搜索和分析资料&quot;,
    agent_url=&quot;http://localhost:5000&quot;)
coordinator.add_tool(researcher_tool)# 协调者可以调用研究员Agent
response = coordinator.run(&quot;请让研究员帮我研究AI在教育领域的应用&quot;)print(response)
```

（2）实战案例：智能客服系统

让我们构建一个完整的智能客服系统，包含三个 Agent：

+ 接待员：分析客户问题类型
+ 技术专家：回答技术问题
+ 销售顾问：回答销售问题

```
from hello_agents import SimpleAgent, HelloAgentsLLM
from hello_agents.tools import A2ATool
from hello_agents.protocols import A2AServer
import threading
import time
from dotenv import load_dotenv

load_dotenv()
llm = HelloAgentsLLM()# 1. 创建技术专家Agent服务
tech_expert = A2AServer(
    name=&quot;tech_expert&quot;,
    description=&quot;技术专家，回答技术问题&quot;)@tech_expert.skill(&quot;answer&quot;)def answer_tech_question(text: str) -&gt; str:import re
    match = re.search(r&#x27;answer\s+(.+)&#x27;, text, re.IGNORECASE)
    question = match.group(1).strip() if match else text
    # 实际应用中，这里会调用LLM或知识库return f&quot;技术回答：关于&#x27;{question}&#x27;，我建议您查看我们的技术文档...&quot;# 2. 创建销售顾问Agent服务
sales_advisor = A2AServer(
    name=&quot;sales_advisor&quot;,
    description=&quot;销售顾问，回答销售问题&quot;)@sales_advisor.skill(&quot;answer&quot;)def answer_sales_question(text: str) -&gt; str:import re
    match = re.search(r&#x27;answer\s+(.+)&#x27;, text, re.IGNORECASE)
    question = match.group(1).strip() if match else text
    return f&quot;销售回答：关于&#x27;{question}&#x27;，我们有特别优惠...&quot;# 3. 启动服务
threading.Thread(target=lambda: tech_expert.run(port=6000), daemon=True).start()
threading.Thread(target=lambda: sales_advisor.run(port=6001), daemon=True).start()
time.sleep(2)# 4. 创建接待员Agent（使用HelloAgents的SimpleAgent）
receptionist = SimpleAgent(
    name=&quot;接待员&quot;,
    llm=llm,
    system_prompt=&quot;&quot;&quot;你是客服接待员，负责：
1. 分析客户问题类型（技术问题 or 销售问题）
2. 将问题转发给相应的专家
3. 整理专家的回答并返回给客户

请保持礼貌和专业。&quot;&quot;&quot;)# 添加技术专家工具
tech_tool = A2ATool(
    agent_url=&quot;http://localhost:6000&quot;,
    name=&quot;tech_expert&quot;,
    description=&quot;技术专家，回答技术相关问题&quot;)
receptionist.add_tool(tech_tool)# 添加销售顾问工具
sales_tool = A2ATool(
    agent_url=&quot;http://localhost:6001&quot;,
    name=&quot;sales_advisor&quot;,
    description=&quot;销售顾问，回答价格、购买相关问题&quot;)
receptionist.add_tool(sales_tool)# 5. 处理客户咨询def handle_customer_query(query):print(f&quot;\n客户咨询：{query}&quot;)print(&quot;=&quot; * 50)
    response = receptionist.run(query)print(f&quot;\n客服回复：{response}&quot;)print(&quot;=&quot; * 50)# 测试不同类型的问题if __name__ == &quot;__main__&quot;:
    handle_customer_query(&quot;你们的API如何调用？&quot;)
    handle_customer_query(&quot;企业版的价格是多少？&quot;)
    handle_customer_query(&quot;如何集成到我的Python项目中？&quot;)
```

（3）高级用法：Agent 间协商

A2A 协议还支持 Agent 间的协商机制：

```
from hello_agents.protocols import A2AServer, A2AClient
import threading
import time

# 创建两个需要协商的Agent
agent1 = A2AServer(
    name=&quot;agent1&quot;,
    description=&quot;Agent 1&quot;)@agent1.skill(&quot;propose&quot;)def handle_proposal(text: str) -&gt; str;&quot;&quot;&quot;处理协商提案&quot;&quot;&quot;import re
    
    # 解析提案match = re.search(r&#x27;propose\s+(.+)&#x27;, text, re.IGNORECASE)
    proposal_str = match.group(1).strip() if match else text
    
    try:
        proposal = eval(proposal_str)
        task = proposal.get(&quot;task&quot;)
        deadline = proposal.get(&quot;deadline&quot;)# 评估提案if deadline &gt;= 7:  # 至少需要7天
            result = {&quot;accepted&quot;: True, &quot;message&quot;: &quot;接受提案&quot;}else:
            result = {&quot;accepted&quot;: False,&quot;message&quot;: &quot;时间太紧&quot;,&quot;counter_proposal&quot;: {&quot;deadline&quot;: 7}}return str(result)except:return str({&quot;accepted&quot;: False, &quot;message&quot;: &quot;无效的提案格式&quot;})

agent2 = A2AServer(
    name=&quot;agent2&quot;,
    description=&quot;Agent 2&quot;)@agent2.skill(&quot;negotiate&quot;)def negotiate_task(text: str) -&gt; str;&quot;&quot;&quot;发起协商&quot;&quot;&quot;import re
    
    # 解析任务和截止日期match = re.search(r&#x27;negotiate\s+task:(.+?)\s+deadline:(\d+)&#x27;, text, re.IGNORECASE)if match:
        task = match.group(1).strip()
        deadline = int(match.group(2))# 向agent1发送提案
        proposal = {&quot;task&quot;: task, &quot;deadline&quot;: deadline}return str({&quot;status&quot;: &quot;negotiating&quot;, &quot;proposal&quot;: proposal})else:return str({&quot;status&quot;: &quot;error&quot;, &quot;message&quot;: &quot;无效的协商请求&quot;})# 启动服务
threading.Thread(target=lambda: agent1.run(port=7000), daemon=True).start()
threading.Thread(target=lambda: agent2.run(port=7001), daemon=True).start()
```

## [10.4 ANP 协议实战](https://datawhalechina.github.io/hello-agents/#/./chapter10/%E7%AC%AC%E5%8D%81%E7%AB%A0%20%E6%99%BA%E8%83%BD%E4%BD%93%E9%80%9A%E4%BF%A1%E5%8D%8F%E8%AE%AE?id=_104-anp-%e5%8d%8f%e8%ae%ae%e5%ae%9e%e6%88%98)
