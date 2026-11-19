
# coze智能体：Excel的智能分析与优化BOT

> 来源：[https://v11enp9ok1h.feishu.cn/docx/Ugxsd9T67o4X7WxR3Bvc0oAEn9Q](https://v11enp9ok1h.feishu.cn/docx/Ugxsd9T67o4X7WxR3Bvc0oAEn9Q)

| 智能体名称 | Excel的智能分析与优化BOT |
| --- | --- |
| 智能体简介 | 这是一款智能数据分析助手,能够自动处理Excel数据并输出洞察。用户只需上传Excel文件,系统就能自动完成数据分析、可视化呈现、特征优化等工作,大幅提升数据分析效率,降低使用门槛。 |
| 应用场景 | 应用场景：信息处理 Excel小白、有数据分析需求的场景。 |
| 智能体主要功能 | 上传 Excel 自动完成数据分析、数据可视化、特征工程 |

这里提供两个数据集，供你们测试玩耍：

![](img/9c1f948d417cb19b26196092c14c6ab6.png)

[睡眠质量数据.xlsx](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/UMXYbux0UoV7nbxIWXCc3YTmnsh)

![](img/b5833c1e7bf8cc37ce7d2bd7bff81761.png)

[学生成绩表.xlsx](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/NhV0biFlLobwsOxSMWIcrOx6nOK)

## 工作流展示

![](img/fb6648e25de490c64804df8a2305f2b9.png)

我们可以把整体看为有三个输出

![](img/0eafd59c840b475f73917036d3f9de0a.png)

![](img/477c43ed22f31c04e683e521b7e31c36.png)

![](img/84d72497626be31bdadc617097c9acdf.png)

## 制作流程：

我们一个输出做一个整体来说，共三个输出整体，加上准备共四个，如下：

### 准备

#### 新建一个工作流

+   工作流名称：`Excel_Smart_Processing`

+   工作流描述：表格智能分析及优化

![](img/be1260a57e1a69dc0aa1b894d5d328dc.png)

#### 修改输入节点

+   改为“`File`”

![](img/a8067366c723c8177f70532a05edb708.png)

#### 添加输出节点

![](img/aeadc65f1f0f1c2789099a937bc191e3.png)

#### 修改输出节点

+   删除输出变量

+   添加“输出内容”

```
**已接受到数据表，正在进行剖析**
```

### 恭喜！

> 你完成了第一个部分，还有三个~

### 数据分析的输出

我们要做的输出，如下图：

![](img/76f179858e6f3ef86c32bf42d5c4ae69.png)

#### 添加文本处理节点

#### 修改文本处理节点

+   输入引用开始节点

+   字符串拼接为

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

+   重新命名为代码块

![](img/7f40abaa3d57da13414af1d588c53672.png)

#### 添加插件（代码执行器-`CodeRunner`）节点

![](img/6b60bc9c57f851df992e58365a7d3603.png)

![](img/1e367973219d8ee59539c2c54ee52ef7.png)

#### 修改插件（代码执行器-`CodeRunner`）节点

+   `code`引用代码块

+   修改名称为“数据的快速描述性统计”

![](img/9a9d9e810610bb7731761dea0bca834e.png)

#### 添加插件（大模型）节点

![](img/97eb66aaaf49400f0cddf4685aa405a2.png)

#### 修改插件（大模型）节点

+   输入引用上一个节点的`FinalOutput`

+   系统提示词

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

+   用户提示词

```
数据的快速描述性统计{{input}}
```

+   修改名称为数据分析

![](img/7f26f7fb6b10a6c8207243fec296dcad.png)

![](img/1da5c029cd044e7a3462a5c0312871ca.png)

#### 添加输出节点

![](img/33a4863fad6569b33333ca1bd91120df.png)

#### 修改输出节点

+   输入引用上一个节点的`output`

+   输出内容

```
## 数据分析🔍
{{output}}
```

+

![](img/161450ecba7f7b0bee6af286455d7f51.png)

#### 连接（输出）节点到结束节点

### 恭喜！

> 你完成了第二个部分，下面让我们去看看可以运行吗

点击试运行，上传文件，再点击试运行

我们可以看到是可以正常输出的

![](img/ba822fd598ea7d3e5c7af75e4e8a58c6.png)

### 合并

> 因为本次节点较多，让我们合并第二个部分的节点，也可以不合并哈

![](img/6b85d4fc90ce06b794637d94de741bca.png)

[PixPin_2025-06-08_18-33-12.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/UMWdbb4scoJ3mZxJrOAcoxqbnpb)

### 数据特证工程

我们要做的输出和工作流，如下图：

![](img/e6284ce42c111c16f5673fb34064476e.png)

![](img/afee4429766f430029dd860d551f75f5.png)

#### 添加文本处理节点

#### 修改文本处理节点

+   命名为代码拼接

+   输入引用开始节点

+   输出为

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

![](img/2ee169e88548d5c950e2a0d5afc1577a.png)

#### 添加插件（代码执行器-`CodeRunner`）节点

![](img/3cbba48aa7c3ba746e5206d179696a88.png)

![](img/61ccc49dd379eb4ac7cd58595fa9eee3.png)

#### 修改插件（代码执行器-`CodeRunner`）节点

+   `code`引用代码拼接

+   修改名称为“读取excel字段与前五行”

![](img/bf089b504107d575a2eac843dca9e111.png)

#### 添加大模型节点

![](img/967e3d5c83e3b6dcff9b050abb24a1b7.png)

#### 修改大模型节点

+   输入引用上一个节点的`FinalOutput`

+   系统提示词
