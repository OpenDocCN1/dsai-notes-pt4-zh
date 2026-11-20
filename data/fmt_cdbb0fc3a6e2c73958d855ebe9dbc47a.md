
# 微学习19 | coze模板复现：表格智能分析及优化

> 来源：[https://datawhaler.feishu.cn/docx/OeC3dnjHpovHaQx9MDkcVGH8npe](https://datawhaler.feishu.cn/docx/OeC3dnjHpovHaQx9MDkcVGH8npe)

## 简单介绍

参考项目：Excel智能分析及优化（付费模板）

对话流名称：`Excel_Smart_Processing`

对话流描述：表格智能分析及优化

![](img/dac6a70793c06b5edb6756dda3991229.png)

我们可以把整体看为有五个输出（展开查看）

![](img/9badef04ffff15dde0ee0b33f72e1821.png)

## 设计目标

## 制作流程

我们一个输出做一个整体来说，共四个输出整体

### 新建对话流并打开

对话流名称：`Excel_Smart_Processing`

对话流描述：表格智能分析及优化

#### 输出一：接受数据表，减少用户使用时间

1. 添加输出节点

重命名：`消息_文件接受`

输出变量：无

输出内容：

```
**已接受到数据表，正在进行剖析**
```

#### 输出二：数据快速描述性统计

1. `代码块`

节点类型名称：`文本处理`

选择应用：`字符串拼接`

输入 `String1`：`开始-BOT_USER_INPUT`

字符串拼接：

```
import requests
import pandas as pd
import io
import numpy as np

def read_excel_from_url(excel_url):
    """从URL读取Excel文件并返回DataFrame"""
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
            raise Exception(f"无法下载Excel文件，HTTP状态码：{response.status_code}")
    except Exception as e:
        raise Exception(f"处理Excel文件失败：{str(e)}")
df = read_excel_from_url('{{String1}}')

def quick_describe(df):
    """
    对数据框进行快速描述性分析
    参数:
        df: pandas DataFrame
    """
    # 基本信息
    print("="*50)
    print("基本信息:")
    print(f"行数: {df.shape[0]}, 列数: {df.shape[1]}")
    print("\n列名及数据类型:")
    print(df.dtypes)
    
    # 数值型变量分析
    numeric_cols = df.select_dtypes(include=[np.number]).columns
    if len(numeric_cols) > 0:
        print("\n" + "="*50)
        print("数值型变量统计描述:")
        print(df[numeric_cols].describe())
    
    # 分类变量分析
    categorical_cols = df.select_dtypes(include=['object', 'category']).columns
    if len(categorical_cols) > 0:
        print("\n" + "="*50)
        print("分类变量分析:")
        for col in categorical_cols:
            print(f"\n{col}:")
            print(f"唯一值数量: {df[col].nunique()}")
            print("前5个最常见值及其频数:")
            print(df[col].value_counts().head())
    
    # 时间序列分析
    datetime_cols = df.select_dtypes(include=['datetime64']).columns
    if len(datetime_cols) > 0:
        print("\n" + "="*50)
        print("时间序列分析:")
        for col in datetime_cols:
            print(f"\n{col}:")
            print(f"时间范围: {df[col].min()} 至 {df[col].max()}")
            print(f"时间跨度: {df[col].max() - df[col].min()}")
quick_describe(df)
```

1. `数据的快速描述性统计`

节点类型名称：`插件-代码执行器`

**输入：**

`code` —— `代码块-output`

1. `数据分析`

节点类型名称：`大模型`

模型选择：`豆包·1.5·Pro·32k`

**输入：**

`input` —— str. 数据的快速描述性统计 result中的`FinalOutput`

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

1. `信息`

节点类型名称：`输出`

输出变量：`output` ——str. 数据分析 `output`

---

第二个输出的整体样子如下

![](img/4ad13c4ae84687d41da99ba9673daff1.png)

#### 输出三：生成特征工程代码

1. `代码拼接`

节点类型名称：`文本处理`

选择应用：`字符串拼接`

输入 `String1`：`开始-BOT_USER_INPUT`

字符串拼接

```
import requests
import pandas as pd
import io

def read_excel_from_url(excel_url):
    """从URL读取Excel文件并返回DataFrame"""
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
            raise Exception(f"无法下载Excel文件，HTTP状态码：{response.status_code}")
    except Exception as e:
        raise Exception(f"处理Excel文件失败：{str(e)}")
df = read_excel_from_url('{{String1}}')

# 输出字段类型
print("数据信息：")
print(df.info())

# 输出前五行数据
print("\n前五行数据：")
print(df.head())
```

1. `读取excel字段与前五行`

节点类型名称：`插件-代码执行器`

**输入：**

`code` —— `代码拼接-output`

1. `数据分析`

节点类型名称：`大模型`

模型选择：`豆包·工具调用`

**输入：**

`input` —— str. 数据的快速描述性统计 result-Object中的`FinalOutput`

系统提示词：
