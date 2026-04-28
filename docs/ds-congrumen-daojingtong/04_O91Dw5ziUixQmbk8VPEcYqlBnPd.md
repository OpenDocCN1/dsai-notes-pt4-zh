# 编程与代码相关

> 来源：[https://yigather.feishu.cn/docx/E1JxdWsBsoLypVxl6pgclwNfnXe](https://yigather.feishu.cn/docx/E1JxdWsBsoLypVxl6pgclwNfnXe)

![](img/d76ad625bf5e10ce43634907e53e47ac.png)

以下是用 **DeepSeek 大模型** 实现代码开发与文档处理任务的实用案例教程，内容简洁、步骤清晰，适合小白直接操作。

---

### **案例 1：代码生成与补全**

**目标**：根据自然语言描述生成可用代码，并添加注释。

#### **功能 1：需求转代码**

**简介**：将功能需求直接转化为 Python/JavaScript 代码。

**步骤**：

1.  **输入指令**（明确语言和功能）：

```
用 Python 写一个爬虫：  
- 目标网站：https://example.com/news  
- 需求：抓取所有标题和发布时间，保存为 CSV  
- 要求：用 requests 和 BeautifulSoup，添加异常处理  
```

1.  **调用模型**：设置 temperature=0.3（平衡稳定性与灵活性）。

1.  **输出结果**：

```
import requests  
from bs4 import BeautifulSoup  
import csv  

url = &quot;https://example.com/news&quot;  
try:  
    response = requests.get(url, timeout=10)  
    soup = BeautifulSoup(response.text, &#x27;html.parser&#x27;)  
    # 提取所有新闻条目  
    articles = soup.select(&#x27;.news-item&#x27;)  
    with open(&#x27;news.csv&#x27;, &#x27;w&#x27;, newline=&#x27;&#x27;) as f:  
        writer = csv.writer(f)  
        writer.writerow([&#x27;标题&#x27;, &#x27;时间&#x27;])  
        for item in articles:  
            title = item.select_one(&#x27;.title&#x27;).text.strip()  
            time = item.select_one(&#x27;.time&#x27;).get(&#x27;datetime&#x27;)  
            writer.writerow([title, time])  
except Exception as e:  
    print(f&quot;抓取失败: {e}&quot;)  
```

**技巧**：

+   **细化要求**：明确指定库版本（如 BeautifulSoup4）避免兼容问题

+   **防御性指令**：添加“考虑反爬机制”或“设置随机延迟”提升代码健壮性

---

#### **功能 2：代码自动补全**

**简介**：根据已有代码片段补全后续逻辑。

**步骤**：

1.  **输入上下文**：

```
// 已有代码：实现表格按价格排序  
function sortTable(columnIndex) {  
  const table = document.getElementById(&#x27;data-table&#x27;);  
  const rows = Array.from(table.rows).slice(1);  
```

1.  **追加指令**：

```
请补全排序逻辑：  
- 支持升序/降序切换  
- 数字按值排序，文本按字母排序  
```

1.  **输出补全代码**：

```
  let isAscending = !table.rows[0].cells[columnIndex].classList.contains(&#x27;asc&#x27;);  
  rows.sort((a, b) =&gt; {  
    const x = a.cells[columnIndex].textContent;  
    const y = b.cells[columnIndex].textContent;  
    return isAscending ? (isNaN(x) ? x.localeCompare(y) : x - y) : (isNaN(y) ? y.localeCompare(x) : y - x);  
  });  
  // 更新表格显示  
  table.tBodies[0].append(...rows);  
  table.rows[0].cells[columnIndex].classList.toggle(&#x27;asc&#x27;, isAscending);  
```

**技巧**：

+   **注释引导**：在代码中插入 // TODO: 这里需要... 引导模型聚焦

+   **限制范围**：用 max_tokens=300 防止生成无关内容

---

#### **功能 3：代码解释与注释**

**简介**：为复杂代码添加中文注释。

**步骤**：

1.  **输入代码**：

```
def quicksort(arr):  
    if len(arr) &lt;= 1:  
        return arr  
    pivot = arr[len(arr)//2]  
    left = [x for x in arr if x &lt; pivot]  
    middle = [x for x in arr if x == pivot]  
    right = [x for x in arr if x &gt; pivot]  
    return quicksort(left) + middle + quicksort(right)  
```

1.  **添加指令**：

```
为这段代码添加行内中文注释，说明每步逻辑  
```

1.  **输出结果**：

```
def quicksort(arr):  
    # 基线条件：数组长度≤1时直接返回  
    if len(arr) &lt;= 1:  
        return arr  
    # 选择中间元素作为基准值  
    pivot = arr[len(arr)//2]  
    # 划分左（小于基准）、中（等于基准）、右（大于基准）三部分  
    left = [x for x in arr if x &lt; pivot]  
    middle = [x for x in arr if x == pivot]  
    right = [x for x in arr if x &gt; pivot]  
    # 递归排序左右部分并合并  
    return quicksort(left) + middle + quicksort(right)  
```

**技巧**：

+   **深度追问**：追加“请解释递归终止条件的作用”获取更多细节

+   **术语控制**：要求“避免专业术语，用小白能懂的语言”

---

### **案例 2：代码调试与优化**

**目标**：分析错误原因并提供优化方案。

#### **功能 1：错误分析定位**

**简介**：根据报错信息定位问题根源。

**步骤**：

1.  **输入报错信息**：

```
Traceback (most recent call last):  
  File &quot;main.py&quot;, line 8, in &lt;module&gt;  
    print(data[&#x27;user&#x27;][&#x27;name&#x27;])  
KeyError: &#x27;user&#x27;  
```

1.  **追加代码上下文**：

```
data = {&#x27;users&#x27;: [{&#x27;id&#x27;: 1, &#x27;name&#x27;: &#x27;Alice&#x27;}]}  
```

1.  **指令**：

```
请分析错误原因并提供两种修复方案  
```

1.  **输出结果**：

```
原因：字典键名错误，正确应为&#x27;users&#x27;而非&#x27;user&#x27;  
方案1: 修改为 data[&#x27;users&#x27;][0][&#x27;name&#x27;]  
方案2: 使用 data.get(&#x27;user&#x27;, {}) 避免直接报错  
```

**技巧**：

+   **模拟追问**：输入“如果是异步代码可能出现哪些类似错误？”扩展排查范围

+   **多方案要求**：明确“至少提供两种解决思路”

---

#### **功能 2：性能优化建议**

**简介**：识别代码瓶颈并提出优化方案。

**步骤**：

1.  **输入待优化代码**：

```
result = []  
for i in range(100000):  
    result.append(i**2)  
```

1.  **添加指令**：

```
请分析时间复杂度并提出更高效的实现方式  
```

1.  **输出结果**：

```
- 原方案：O(n) 时间复杂度，但列表追加有内存重分配开销  
- 优化方案：使用列表推导式 result = [i**2 for i in range(100000)]  
- 进阶方案：改用 NumPy 向量化操作（适合超大规模数据）  
```

**技巧**：

+   **量化要求**：添加“请给出不同数据规模下的耗时对比”

+   **场景区分**：明确“如果是实时系统需要如何优化？”

---

#### **功能 3：API 文档生成**

**简介**：根据代码自动生成接口文档。

**步骤**：

1.  **输入代码**：

```
def create_user(name: str, age: int) -&gt; dict:  
    &quot;&quot;&quot;  
    创建用户档案  
    :param name: 用户名  
    :param age: 年龄（需≥18）  
    :return: 包含用户ID和注册时间的字典  
    &quot;&quot;&quot;  
    user_id = generate_uuid()  
    return {&#x27;id&#x27;: user_id, &#x27;name&#x27;: name, &#x27;reg_time&#x27;: datetime.now()}  
```

1.  **添加指令**：

```
生成 Markdown 格式的 API 文档，包含请求示例  
```

1.  **输出结果**：

```
## create_user  
**功能**：创建用户档案  

### 参数  
| 参数名 | 类型   | 说明          |  
|--------|--------|---------------|  
| name   | string | 用户名        |  
| age    | int    | 年龄（需≥18） |  

### 返回示例  
```json  
{  
  &quot;id&quot;: &quot;550e8400-e29b-41d4-a716-446655440000&quot;,  
  &quot;name&quot;: &quot;张三&quot;,  
  &quot;reg_time&quot;: &quot;2024-06-01T12:34:56&quot;  
}  
```

```

```

**技巧**：

+   **模板约束**：提供 Swagger/OpenAPI 模板要求严格遵循格式

+   **字段映射**：要求“将 Python 类型转换为 JSON 类型（如 datetime→string）”

---

### **案例 3：代码库解释与维护**

**目标**：理解复杂项目结构并生成示例。

#### **功能 1：代码库全局解释**

**简介**：通过目录结构分析项目架构。

**步骤**：

1.  **输入目录结构**：

```
src/  
├─ api/  
│  ├─ auth.py  
│  └─ users.py  
├─ models/  
│  └─ database.py  
└─ utils/  
   └─ logger.py  
```

1.  **添加指令**：

```
推测这是一个什么类型的项目？各模块的核心职责是什么？  
```

1.  **输出结果**：

```
项目类型：Web 后端服务  
- api/: 接口层（认证/用户管理）  
- models/: 数据模型与数据库操作  
- utils/: 通用工具模块（日志处理等）  
```

**技巧**：

+   **对比分析**：追加“如果是微服务架构应该如何调整目录？”

+   **技术栈推测**：要求“根据文件命名推测可能使用的框架（如 Flask/Django）”

---

#### **功能 2：单文件功能解读**

**简介**：解析陌生代码文件的核心逻辑。

**步骤**：

1.  **输入代码片段**：

```
function debounce(fn, delay) {  
  let timer;  
  return (...args) =&gt; {  
    clearTimeout(timer);  
    timer = setTimeout(() =&gt; fn.apply(this, args), delay);  
  };  
}  
```

1.  **指令**：

```
请用通俗语言解释这个函数的作用，并给出使用示例  
```

1.  **输出结果**：

```
**作用**：防抖函数，连续调用时只在停止操作 delay 毫秒后执行一次  
**示例**：  
const searchInput = document.getElementById(&#x27;search&#x27;);  
searchInput.addEventListener(&#x27;input&#x27;, debounce(() =&gt; {  
  console.log(&#x27;实际发起搜索请求&#x27;);  
}, 500));  
```

**技巧**：

+   **场景化要求**：添加“请说明在搜索框/窗口 resize 时的典型应用场景”

+   **边界测试**：追问“如果 delay 设为 0 会有什么效果？”

---

#### **功能 3：代码示例生成**

**简介**：根据技术文档生成对应代码示例。

**步骤**：

1.  **输入文档片段**：

```
Redis 分布式锁实现要求：  
- 使用 SETNX 命令  
- 设置超时时间  
- 防止误删其他客户端的锁  
```

1.  **添加指令**：

```
用 Python 代码实现上述逻辑，要求兼容 Redis 5.0+  
```

1.  **输出代码**：

```
import redis  
import uuid  

r = redis.Redis(host=&#x27;localhost&#x27;, port=6379)  

class DistributedLock:  
    def __init__(self, key, timeout=30):  
        self.key = key  
        self.timeout = timeout  
        self.identifier = str(uuid.uuid4())  

    def acquire(self):  
        # SET key value NX EX timeout  
        return r.set(self.key, self.identifier, nx=True, ex=self.timeout)  

    def release(self):  
        lua_script = &quot;&quot;&quot;  
        if redis.call(&#x27;get&#x27;, KEYS[1]) == ARGV[1] then  
            return redis.call(&#x27;del&#x27;, KEYS[1])  
        else  
            return 0  
        end  
        &quot;&quot;&quot;  
        r.eval(lua_script, 1, self.key, self.identifier)  
```

**技巧**：

+   **版本约束**：明确“需要兼容 Python 3.8 和 Redis 6.2”

+   **安全强化**：要求“添加重试机制和异常处理”

---

### **核心技巧总结**

1.  **提示词设计公式**：

```
语言限定 + 功能描述 + 输出要求 + 避错指令  
（例：“用 JavaScript 实现 X，要求兼容 IE11，避免使用箭头函数”）  
```

1.  **参数调优指南**：

+   **精确生成**：temperature=0~0.3 + max_tokens=500

+   **探索性生成**：temperature=0.7 + top_p=0.9

1.  **结果验证方法**：

+   对生成的代码用 print/console.log 插入验证点

+   要求模型自己写测试用例：“请为这段代码编写3个单元测试”