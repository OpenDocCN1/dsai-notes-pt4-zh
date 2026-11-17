# 微学习19 | coze模板复现：表格智能分析及优化

> 来源：[https://datawhaler.feishu.cn/docx/OeC3dnjHpovHaQx9MDkcVGH8npe](https://datawhaler.feishu.cn/docx/OeC3dnjHpovHaQx9MDkcVGH8npe)

## 简单介绍

参考项目：Excel智能分析及优化（付费模板）

对话流名称：Excel_Smart_Processing

对话流描述：表格智能分析及优化

![](img/dac6a70793c06b5edb6756dda3991229.png)

我们可以把整体看为有五个输出（展开查看）

![](img/9badef04ffff15dde0ee0b33f72e1821.png)

# **设计目标：**

# **制作流程：**

我们一个输出做一个整体来说，共四个输出整体

## 新建一个**对话流，并打开**

对话流名称：Excel_Smart_Processing

对话流描述：表格智能分析及优化

### 输出一：接受数据表，减少用户使用时间

1.  添加输出节点

重命名：消息_文件接受

输出变量：无

输出内容：

```
**已接受到数据表，正在进行剖析**
```

### 输出二：数据快速描述性统计

1.  **代码块**

节点类型名称：文本处理

选择应用：字符串拼接

输入 String1：开始-BOT_USER_INPUT

**字符串拼接：**

```
import requests
import pandas as pd
import io
import numpy as np

def read_excel_from_url(excel_url):
    &quot;&quot;&quot;从URL读取Excel文件并返回DataFrame&quot;&quot;&quot;
    try:
        # 下载Excel文件
        response = requests.get(excel_url)
        if response.status_code == 200:
            # 将内容读入BytesIO对象
            excel_content = io.BytesIO(response.content)
            # 读取Excel文件返回DataFrame
            try:
                # 尝试读取Excel文件
                df = pd.read_excel(excel_content)
            except:
                # 如果失败,尝试读取CSV文件
                excel_content.seek(0)  # 重置指针到开头
                df = pd.read_csv(excel_content)
            return df
        else:
            raise Exception(f&quot;无法下载Excel文件，HTTP状态码：{response.status_code}&quot;)
    except Exception as e:
        raise Exception(f&quot;处理Excel文件失败：{str(e)}&quot;)
df = read_excel_from_url(&#x27;{{String1}}&#x27;)

def quick_describe(df):
    &quot;&quot;&quot;
    对数据框进行快速描述性分析
    参数:
        df: pandas DataFrame
    &quot;&quot;&quot;
    # 基本信息
    print(&quot;=&quot;*50)
    print(&quot;基本信息:&quot;)
    print(f&quot;行数: {df.shape[0]}, 列数: {df.shape[1]}&quot;)
    print(&quot;\n列名及数据类型:&quot;)
    print(df.dtypes)
    
    # 数值型变量分析
    numeric_cols = df.select_dtypes(include=[np.number]).columns
    if len(numeric_cols) &gt; 0:
        print(&quot;\n&quot; + &quot;=&quot;*50)
        print(&quot;数值型变量统计描述:&quot;)
        print(df[numeric_cols].describe())
    
    # 分类变量分析
    categorical_cols = df.select_dtypes(include=[&#x27;object&#x27;, &#x27;category&#x27;]).columns
    if len(categorical_cols) &gt; 0:
        print(&quot;\n&quot; + &quot;=&quot;*50)
        print(&quot;分类变量分析:&quot;)
        for col in categorical_cols:
            print(f&quot;\n{col}:&quot;)
            print(f&quot;唯一值数量: {df[col].nunique()}&quot;)
            print(&quot;前5个最常见值及其频数:&quot;)
            print(df[col].value_counts().head())
    
    # 时间序列分析
    datetime_cols = df.select_dtypes(include=[&#x27;datetime64&#x27;]).columns
    if len(datetime_cols) &gt; 0:
        print(&quot;\n&quot; + &quot;=&quot;*50)
        print(&quot;时间序列分析:&quot;)
        for col in datetime_cols:
            print(f&quot;\n{col}:&quot;)
            print(f&quot;时间范围: {df[col].min()} 至 {df[col].max()}&quot;)
            print(f&quot;时间跨度: {df[col].max() - df[col].min()}&quot;)
quick_describe(df)

```

1.  **数据的快速描述性统计**

节点类型名称：插件-代码执行器

**输入：**

code —— 代码块-output

1.  **数据分析**

节点类型名称：大模型

模型选择：豆包·1.5·Pro·32k

**输入：**

input —— str. 数据的快速描述性统计 result中的FinalOutput

![](img/fb490a7fe72921ec100afbd391ceba37.png)

**系统提示词**

```
## Role: 通用数据分析专家

## Profile:
- Name: DataInsight
- Version: 2.0
- Language: 中文
- Description: 你是一位专业的数据分析AI助手，擅长从各类数据中提炼关键发现并提供有价值的建议。

## Goals:
- 基于数据洞察，按照指定格式回答用户问题

## Constraints:
1. 始终聚焦于用户的核心问题
2. 保持分析的客观性和全面性
3. 避免过度解读数据
4. 使用简洁、专业的语言
5. 输出内容应简明扼要，突出重点

## Skills:
1. 趋势识别：从数据中识别关键趋势和异常
2. 洞察提炼：从复杂数据中提炼关键发现
3. 公正分析：避免偏颇，全面呈现数据情况
4. 建议形成：基于数据分析提出可行建议
5. 数据清理：处理可能存在的数据质量问题，如重复项、异常值等

## Workflows:
1. 统计理解：仔细阅读并理解描述性统计和数据
2. 数据处理：清理和整理数据，确保数据质量
3. 趋势分析：识别数据中的主要趋势和异常情况
4. 洞察总结：基于数据提炼关键发现
5. 建议形成：根据分析结果提出可行建议

## OutputFormat:
1. 数据概览
   - 数据范围：[简要描述数据的时间跨度、样本量等]
   - 主要指标：[列出分析涉及的主要指标]

2. 主要发现
   1. **[发现1]**：[简要解释]
   2. **[发现2]**：[简要解释]
   3. **[发现3]**：[简要解释]（如果必要）

3. 建议
   1. [建议1]：[简要说明]
   2. [建议2]：[简要说明]
   3. [建议3]：[简要说明]（如果必要）

4. 局限性
   - [说明分析的局限性，如数据质量问题、样本代表性等]

```

1.  **信息**

节点类型名称：输出

输出变量：output ——str. 数据分析 output

---

第二个输出的整体样子如下

![](img/4ad13c4ae84687d41da99ba9673daff1.png)

### 输出三：生成特征工程代码

1.  **代码拼接**

节点类型名称：文本处理

选择应用：字符串拼接

输入 String1：开始-BOT_USER_INPUT

**字符串拼接**

```
import requests
import pandas as pd
import io

def read_excel_from_url(excel_url):
    &quot;&quot;&quot;从URL读取Excel文件并返回DataFrame&quot;&quot;&quot;
    try:
        # 下载Excel文件
        response = requests.get(excel_url)
        if response.status_code == 200:
            # 将内容读入BytesIO对象
            excel_content = io.BytesIO(response.content)
            # 读取Excel文件返回DataFrame
            try:
                # 尝试读取Excel文件
                df = pd.read_excel(excel_content)
            except:
                # 如果失败,尝试读取CSV文件
                excel_content.seek(0)  # 重置指针到开头
                df = pd.read_csv(excel_content)
            return df
        else:
            raise Exception(f&quot;无法下载Excel文件，HTTP状态码：{response.status_code}&quot;)
    except Exception as e:
        raise Exception(f&quot;处理Excel文件失败：{str(e)}&quot;)
df = read_excel_from_url(&#x27;{{String1}}&#x27;)

# 输出字段类型
print(&quot;数据信息：&quot;)
print(df.info())

# 输出前五行数据
print(&quot;\n前五行数据：&quot;)
print(df.head())
```

1.  **读取excel字段与前五行**

节点类型名称：插件-代码执行器

**输入：**

code —— 代码拼接-output

1.  **数据分析**

节点类型名称：大模型

模型选择：豆包·工具调用

**输入：**

input —— str. 数据的快速描述性统计 result-Object中的FinalOutput

系统提示词：

```
# Role: Python数据分析与特征工程专家

## Profile:

- Description: 你是一位精通Python数据分析和特征工程的专家，能够根据给定的数据表信息进行特征工程，衍生多个有意义的新字段，并美化表格。

## Goals:
1. 理解用户提供的数据表信息概览和前五行数据{{input1}}
2. 进行特征工程，衍生多个有意义的新字段(中文命名)
3. 美化表格，使简单表格变成详细的表格
4. 生成清晰、易读、高效的Python代码

## Constraints:
- 生成的代码必须是对给定代码段的续写
- 使用Python标准库和pandas进行数据处理
- 只输出代码，不输出其他内容
- 代码中需要包含注释，解释每个步骤的作用

## Skills:
- 精通Python数据分析库（如pandas, numpy）
- 擅长数据预处理和特征工程
- 能够创造性地设计新特征
- 熟悉各种数据类型的处理方法
- 了解常见的数据分析场景和需求

## Workflow:
1. 分析用户提供的数据表信息，理解字段类型和数据特征
2. 根据现有字段，设计并创建新的有意义的特征
3. 对数据进行必要的预处理和转换
4 生成完整的Python代码，包含注释

## Examples:
Example 1:
&quot;&quot;&quot;
Input:
信息概览：
&lt;class &#x27;pandas.core.frame.DataFrame&#x27;&gt;
RangeIndex: 5 entries, 0 to 4
Data columns (total 4 columns):
 #   Column    Non-Null Count  Dtype  
---  ------    --------------  -----  
 0   date      5 non-null       datetime64[ns]
 1   sales     5 non-null       float64
 2   product   5 non-null       object 
 3   region    5 non-null       object 
dtypes: datetime64[ns](1), float64(1), object(2)
memory usage: 336.0+ bytes

前五行数据：
   date        sales  product  region
0  2023-01-01  1000.0  A       North
1  2023-01-02  1200.5  B       South
2  2023-01-03  950.75  A       East
3  2023-01-04  1100.0  C       West
4  2023-01-05  1300.0  B       North

Output:
import pandas as pd
import numpy as np

# 读取数据
df = pd.read_excel(&#x27;data.xlsx&#x27;)

# 特征工程
# 1. 从日期中提取更多信息
df[&#x27;year&#x27;] = df[&#x27;date&#x27;].dt.year
df[&#x27;month&#x27;] = df[&#x27;date&#x27;].dt.month
df[&#x27;day&#x27;] = df[&#x27;date&#x27;].dt.day
df[&#x27;day_of_week&#x27;] = df[&#x27;date&#x27;].dt.dayofweek
df[&#x27;is_weekend&#x27;] = df[&#x27;day_of_week&#x27;].isin([5, 6]).astype(int)
df[&#x27;quarter&#x27;] = df[&#x27;date&#x27;].dt.quarter

# 2. 销售相关特征
df[&#x27;sales_category&#x27;] = pd.cut(df[&#x27;sales&#x27;], bins=[0, 1000, 1200, float(&#x27;inf&#x27;)], labels=[&#x27;低&#x27;, &#x27;中&#x27;, &#x27;高&#x27;])
df[&#x27;cumulative_sales&#x27;] = df.groupby(&#x27;product&#x27;)[&#x27;sales&#x27;].cumsum()

# 3. 产品相关特征
df[&#x27;is_product_A&#x27;] = (df[&#x27;product&#x27;] == &#x27;A&#x27;).astype(int)
df[&#x27;is_product_B&#x27;] = (df[&#x27;product&#x27;] == &#x27;B&#x27;).astype(int)
df[&#x27;is_product_C&#x27;] = (df[&#x27;product&#x27;] == &#x27;C&#x27;).astype(int)

# 4. 地区相关特征
df[&#x27;is_north&#x27;] = (df[&#x27;region&#x27;] == &#x27;North&#x27;).astype(int)
df[&#x27;region_encoded&#x27;] = df[&#x27;region&#x27;].map({&#x27;North&#x27;: 0, &#x27;South&#x27;: 1, &#x27;East&#x27;: 2, &#x27;West&#x27;: 3})

# 5. 时间窗口特征
df[&#x27;sales_7d_avg&#x27;] = df.groupby(&#x27;product&#x27;)[&#x27;sales&#x27;].rolling(window=7, min_periods=1).mean().reset_index(0, drop=True)

# 6. 交叉特征
df[&#x27;product_region&#x27;] = df[&#x27;product&#x27;] + &#x27;_&#x27; + df[&#x27;region&#x27;]

# 保存结果
df.to_csv(&#x27;Data.csv&#x27;, index = True)
&quot;&quot;&quot;


## OutputFormat:
[Python代码]


```

用户提示词：

```
默认数据已经读入df，请对其续写特征工程和数据处理代码部分，并将文件保存df.to_csv(&#x27;Data.csv&#x27;, index = True)，直接输出代码，不用写```python```
```

1.  **输出代码1**

重命名：消息_文件接受

输出变量：output —— str. 生成特征工程代码-output

![](img/da856fee3683b8cef083bbe6a323fe15.png)

输出内容：

```
## 开始进行特征工程🔧，相关代码如下
```python
{{output}}
```
```

---

输出三的整体样子如下

![](img/fbf4bc68d680f627a649a154ca4a5281.png)

---

### 输出四：生成可视化图片

这个输出我们将从输出三的第二节点开始，如下图所示

![](img/4a968c5d34143e7727d701c9183bf66d.png)

1.  **生成可视化代码**

节点类型名称：大模型

模型选择：豆包·1.5·Pro·32k

**输入：**

input —— str. 数据的快速描述性统计 result中的FinalOutput

![](img/7e52ba46b875a73b73f874ebb66a99fc.png)

系统提示词：

```
# Role: Python数据分析与可视化专家


## Goals:
1. 理解给定的数据表信息概览以及前五行数据{{input1}}
2. 生成适当的数据处理代码
3. 在一个画布上创建多个相关的可视化图表

## Constraints:
- **生成的代码必须是对给定代码段的续写**
- 使用Python标准库、pandas、matplotlib、seaborn等常用数据分析和可视化库
- 智能分析绘图，必须在一个画布上绘制至少两个及以上图表
- 绘制多个图表、全面展示数据
- 只输出代码，不准输出其他的

## Skills:
- 精通Python数据分析库（如pandas, numpy）
- 精通matplotlib，不准用seaborn绘图
- 擅长数据预处理和统计分析
- 能够设计和实现多子图布局，能设计画布大小、整体标题、解决X轴堆叠问题
- 标题、抬头使用中文显示
- 精通多种图表，选择适当的图表类型进行可视化，并正确展示数据

## 给定代码段
import pandas as pd
df = pd.read_excel(&#x27;data.xlsx&#x27;)

#### OutputFormat【包含数据处理、可视化和分析的Python代码，这些代码应该续写在用户提供的已有代码之后，】:
[编写代码]
plt.tight_layout()
plt.savefig(&#x27;analysis_result.png&#x27;, dpi=300, bbox_inches=&#x27;tight&#x27;)
plt.show()
```

用户提示词：

```
默认数据已经读入df，请对其续写可视化绘图部分，并将图片保存plt.savefig(&#x27;analysis_result.png&#x27;, dpi=300, bbox_inches=&#x27;tight&#x27;)，直接输出代码，不用写```python```
```

1.  **代码拼接1**

节点类型名称：文本处理

选择应用：字符串拼接

输入 String1：开始-BOT_USER_INPUT

输入 String2：生成可视化代码-python_code

**字符串拼接**

```
import requests
import pandas as pd
import io




import matplotlib.pyplot as plt
import pandas as pd
def read_excel_from_url(excel_url):
    &quot;&quot;&quot;从URL读取Excel文件并返回DataFrame&quot;&quot;&quot;
    try:
        # 下载Excel文件
        response = requests.get(excel_url)
        if response.status_code == 200:
            # 将内容读入BytesIO对象
            excel_content = io.BytesIO(response.content)
            # 读取Excel文件返回DataFrame
            try:
                # 尝试读取Excel文件
                df = pd.read_excel(excel_content)
            except:
                # 如果失败,尝试读取CSV文件
                excel_content.seek(0)  # 重置指针到开头
                df = pd.read_csv(excel_content)
            return df
        else:
            raise Exception(f&quot;无法下载Excel文件，HTTP状态码：{response.status_code}&quot;)
    except Exception as e:
        raise Exception(f&quot;处理Excel文件失败：{str(e)}&quot;)
df = read_excel_from_url(&#x27;{{String1}}&#x27;)

{{String2}}

```

1.  **数据可视化**

节点类型名称：插件-代码执行器

**输入：**

code —— 代码块-output

1.  **判断图片是否生成成功**

条件分支

如果不为空

![](img/a150a87fa96d3cab562b9128e3c8238c.png)

![](img/e6916cdb5248827d5e7d00d6b82b7900.png)

1.  **图片生成成功**

节点类型名称：文本处理

选择应用：字符串拼接

输入 String1：数据可视化-PicList

![](img/742af45171e16fb3c7f3081e90f5f58a.png)

**字符串拼接**

![生成图片]({{String1[0]}})

```
## 数据可视化结果📊
```

1.  **图片生成失败**

节点类型名称：文本处理

选择应用：字符串拼接

输入 String1：数据可视化-PicList

![](img/8c469dd28fe3850837f63913a849e4e3.png)

**字符串拼接**

![图片生成失败](img/cd026c59db59ddbb22a09de7af99fd54.png)

```
## 生成图片失败/(ㄒoㄒ)/~~
**注意**：报错原因为{{String1}}，小概率会出现环境中没有相关字体的情况，建议重新运行一下
```

---

输出三的整体样子如下

![](img/6c9952a2f1b517488972f579d8620c65.png)

### 输出五：特征工程执行展示

从第三个输出的结束开始

![](img/635fc5a172a0e8b1e1e806e7ca6b8214.png)

1.  **代码拼接2**

节点类型名称：文本处理

选择应用：字符串拼接

输入 String1：开始-BOT_USER_INPUT

输入 String2：生成特征工程代码 - output

字符串拼接

```
import requests
import pandas as pd
import io
import numpy as np


def read_excel_from_url(excel_url):
    &quot;&quot;&quot;从URL读取Excel文件并返回DataFrame&quot;&quot;&quot;
    try:
        # 下载Excel文件
        response = requests.get(excel_url)
        if response.status_code == 200:
            # 将内容读入BytesIO对象
            excel_content = io.BytesIO(response.content)
            # 读取Excel文件返回DataFrame
            try:
                # 尝试读取Excel文件
                df = pd.read_excel(excel_content)
            except:
                # 如果失败,尝试读取CSV文件
                excel_content.seek(0)  # 重置指针到开头
                df = pd.read_csv(excel_content)
            return df
        else:
            raise Exception(f&quot;无法下载Excel文件，HTTP状态码：{response.status_code}&quot;)
    except Exception as e:
        raise Exception(f&quot;处理Excel文件失败：{str(e)}&quot;)
df = read_excel_from_url(&#x27;{{String1}}&#x27;)



{{String2}}

# 将DataFrame转换为CSV格式的字符串
csv_string = df.to_csv(index=False)

# 打印CSV格式的字符串
print(csv_string)

```

1.  **特征工程**

节点类型名称：插件-代码执行器

**输入：**

code —— 代码拼接2-output

1.  **判断执行是否成功**

条件分支 大于等于

如果：

![](img/fd9b9766f64cd6234052b08d0750d531.png)

![](img/26496a2fbcae028702d3e3b14b530c15.png)

1.  **输出前五行代码**

节点类型名称：文本处理

选择应用：字符串拼接

输入 String1：开始-BOT_USER_INPUT

输入 String2：生成特征工程代码 - output

字符串拼接

```
import requests
import pandas as pd
import io
import numpy as np


def read_excel_from_url(excel_url):
    &quot;&quot;&quot;从URL读取Excel文件并返回DataFrame&quot;&quot;&quot;
    try:
        # 下载Excel文件
        response = requests.get(excel_url)
        if response.status_code == 200:
            # 将内容读入BytesIO对象
            excel_content = io.BytesIO(response.content)
            # 读取Excel文件返回DataFrame
            try:
                # 尝试读取Excel文件
                df = pd.read_excel(excel_content)
            except:
                # 如果失败,尝试读取CSV文件
                excel_content.seek(0)  # 重置指针到开头
                df = pd.read_csv(excel_content)
            return df
        else:
            raise Exception(f&quot;无法下载Excel文件，HTTP状态码：{response.status_code}&quot;)
    except Exception as e:
        raise Exception(f&quot;处理Excel文件失败：{str(e)}&quot;)
df = read_excel_from_url(&#x27;{{String1}}&#x27;)


{{String2}}

try:
    from tabulate import tabulate
    print(tabulate(df.head(), headers=&#x27;keys&#x27;, tablefmt=&#x27;pipe&#x27;, showindex=False))
except ImportError:
      print(df.head().to_string())
  # 备选方案:使用DataFrame的to_string()方法


```

1.  **输出前五行**

节点类型名称：插件-代码执行器

**输入：**

code —— 输出前五行代码-output

1.  **特征工程失败**

节点类型名称：文本处理

选择应用：字符串拼接

字符串拼接

```
## 数据特征工程📄
**部分数据展示**：

{{String3}}
```

![](img/cd5bb1dc23420ccd13f03c3d0f20050d.png)

1.  **特征工程失败**

节点类型名称：文本处理

选择应用：字符串拼接

字符串拼接

```
## 数据特征工程
特征工程失败，换个Excel试试吧！
```

![](img/9e465ef95adcc81bc4a378be5ba76147.png)

好的，现在让我们看看整体的流程：

![](img/d8df9df01d3bbf294eb5850b1cc32fb3.png)

#