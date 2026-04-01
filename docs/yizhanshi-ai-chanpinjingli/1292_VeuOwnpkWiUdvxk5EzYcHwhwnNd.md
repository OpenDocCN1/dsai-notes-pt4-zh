# 基于 MCP 的合成数据生成器技术文档

> 来源：[https://v11enp9ok1h.feishu.cn/docx/S0nRd2tkdoVfXaxezVHc6cxEnxe](https://v11enp9ok1h.feishu.cn/docx/S0nRd2tkdoVfXaxezVHc6cxEnxe)

了解如何构建一个能够生成任何类型合成数据集的 MCP 服务器。它使用 Cursor 作为 MCP 主机，并使用 SDV 生成逼真的表格合成数据。

![](img/4e3645156fdc02d985e18d699ead5dcd.png)

### 技术栈

+   Cursor 作为 MCP 主机

+   Datacebo 的 SDV 用于生成逼真的表格合成数据

### 工作流程

+   用户提交查询

+   智能体连接到 MCP 服务器以查找工具

+   智能体根据查询使用适当的工具

+   返回关于合成数据创建、评估或可视化的响应

## MCP 服务器概述

我们的 MCP 服务器包括三个工具：

+   SDV 生成

+   SDV 评估

+   SDV 可视化

我们使用 SDV SDK 对这些工具的实际实现保存在单独的文件 tools.py 中，这里将其导入。

![](img/629b4792fb1eaaae970ba218549875c8.png)

```
from mcp.server.fastmcp import FastMCP
from tools import generate, evaluate, visualize  # 所有三个SDV函数

# 创建FastMCP实例
mcp = FastMCP(&quot;sdv_mcp&quot;)

@mcp.tool()
def sdv_generate(folder_name: str) -&gt; str:
    &quot;&quot;&quot;使用SDV合成器基于真实数据生成合成数据。
    
    参数:
        folder_name (str): 包含CSV数据文件和metadata.json的文件夹路径
        
    返回:
        str: 包含生成表信息的成功消息
    &quot;&quot;&quot;
    return generate(folder_name)

@mcp.tool()
def sdv_evaluate(folder_name: str) -&gt; dict:
    &quot;&quot;&quot;评估合成数据与真实数据的质量比较。
    
    参数:
        folder_name (str): 包含原始CSV数据文件和metadata.json的文件夹路径
        
    返回:
        dict: 评估结果，包括总体得分和详细属性
    &quot;&quot;&quot;
    return evaluate(folder_name)

@mcp.tool()
def sdv_visualize(folder_name: str, table_name: str, column_name: str) -&gt; str:
    &quot;&quot;&quot;生成特定列的真实数据与合成数据比较可视化。
    
    参数:
        folder_name (str): 包含原始CSV数据文件和metadata.json的文件夹路径
        table_name (str): 要可视化的表名（必须存在于元数据中）
        column_name (str): 要可视化的列名
        
    返回:
        str: 包含保存的可视化路径的成功消息或错误消息
    &quot;&quot;&quot;
    return visualize(folder_name, table_name, column_name)

# 运行服务器
if __name__ == &quot;__main__&quot;:
    mcp.run(transport=&quot;stdio&quot;)  # Model Context Protocol
```

## SDV 生成工具

此工具使用 SDV 合成器从真实数据创建合成数据。SDV 提供了多种合成器，每种都利用不同的算法来生成合成数据。

![](img/13517c6caeb39d64a11269f3020c613e.png)

```
import os
from sdv.io.local import CSVHandler
from sdv.metadata import Metadata
from sdv.multi_table import HMASynthesizer  # 数据建模

def generate(folder_name: str):
    &quot;&quot;&quot;使用SDV合成器基于真实数据生成合成数据。&quot;&quot;&quot;
    # 从指定文件夹加载CSV文件
    connector = CSVHandler()
    data = connector.read(folder_name=folder_name)
    
    metadata_file = os.path.join(folder_name, &quot;metadata.json&quot;)  # 加载元数据
    metadata = Metadata.load_from_json(metadata_file)
    
    synthesizer = HMASynthesizer(metadata)  # 训练合成器
    synthesizer.fit(data)
    
    synthetic_data = synthesizer.sample(scale=1)  # 采样数据
    
    # 将合成数据保存为CSV文件
    os.makedirs(&quot;synthetic_data&quot;, exist_ok=True)
    for table_name, df in synthetic_data.items():
        output_file = os.path.join(&quot;synthetic_data&quot;, f&quot;{table_name}.csv&quot;)
        df.to_csv(output_file, index=False)
        
    return f&quot;数据生成成功并保存在&#x27;synthetic_data&#x27;文件夹中。&quot;
```

## SDV 评估工具

此工具评估合成数据与真实数据的质量比较。我们将评估统计相似性，以确定合成数据捕获了哪些真实数据模式。

![](img/4515ebef8942b241c521d26c4c8802eb.png)

```
from sdv.evaluation.multi_table import evaluate_quality  # 质量评估

def evaluate(folder_name: str):
    &quot;&quot;&quot;评估合成数据与真实数据的比较。&quot;&quot;&quot;
    # 加载元数据
    metadata_file = os.path.join(folder_name, &quot;metadata.json&quot;)
    metadata = Metadata.load_from_json(metadata_file)
    
    # 从元数据获取表列表
    table_names = metadata.tables
    
    # 创建数据字典
    real_data_dict, synthetic_data_dict = {}, {}
    
    # 从CSV文件加载每个表
    for table_name in table_names:
        real_path = os.path.join(folder_name, f&quot;{table_name}.csv&quot;)
        synthetic_path = os.path.join(&quot;synthetic_data&quot;, f&quot;{table_name}.csv&quot;)
        
        # 读取当前表的真实数据和合成数据文件
        real_data_dict[table_name] = pd.read_csv(real_path)
        synthetic_data_dict[table_name] = pd.read_csv(synthetic_path)
    
    quality_report = evaluate_quality(  # 运行评估
        real_data=real_data_dict,
        synthetic_data=synthetic_data_dict,
        metadata=metadata,
        verbose=False,
    )
    
    overall_score = quality_report.get_score()  # 获取指标
    properties_df = quality_report.get_properties()
    properties = properties_df.to_dict(orient=&quot;records&quot;)
    
    # 返回指标
    return {&quot;Overall Score&quot;: overall_score, &quot;Properties&quot;: properties}
```

## SDV 可视化工具

此工具生成特定列的真实数据与合成数据比较可视化。使用此函数可视化真实列及其对应的合成列。

![](img/67e10e6ce4a92ddabdd2c240d8008950.png)

```
from sdv.evaluation.multi_table import get_column_plot  # 数据绘图

def visualize(folder_name: str, table_name: str, column_name: str, visualization_folder: str = &quot;plots&quot;):
    &quot;&quot;&quot;生成特定列的真实数据与合成数据比较可视化。&quot;&quot;&quot;
    # 加载元数据
    metadata_file = os.path.join(folder_name, &quot;metadata.json&quot;)
    metadata = Metadata.load_from_json(metadata_file)
    
    # 加载指定表的真实数据和合成数据
    real_path = os.path.join(folder_name, f&quot;{table_name}.csv&quot;)
    synthetic_path = os.path.join(&quot;synthetic_data&quot;, f&quot;{table_name}.csv&quot;)
    real_data = pd.read_csv(real_path)
    synthetic_data = pd.read_csv(synthetic_path)
    
    # 创建get_column_plot所需的数据字典
    real_data_dict = {table_name: real_data}
    synthetic_data_dict = {table_name: synthetic_data}
    
    # 如果可视化文件夹不存在则创建
    os.makedirs(visualization_folder, exist_ok=True)
    
    fig = get_column_plot(  # 生成列图
        real_data=real_data_dict,
        synthetic_data=synthetic_data_dict,
        metadata=metadata,
        table_name=table_name,
        column_name=column_name,
    )
    
    safe_column_name = column_name.replace(&quot; &quot;, &quot;_&quot;).replace(&quot;/&quot;, &quot;-&quot;)
    filename = f&quot;{table_name}.{safe_column_name}.png&quot;  # 创建并保存文件
    filepath = os.path.join(visualization_folder, filename)
    fig.write_image(filepath)
    
    return f&quot;成功保存{table_name}.{column_name}的可视化。&quot;
```

工具和服务器准备就绪后，让我们将其与 Cursor IDE 集成！请按照以下步骤操作：文件 → 首选项 → Cursor 设置 → MCP → 添加新的全局 MCP 服务器。在 JSON 文件中，添加如下所示内容：

![](img/17e2b96515919fac7e75dd14b9c29c50.png)

```
{
  &quot;mcpServers&quot;: {
    &quot;sdv_mcp&quot;: {  // 服务器名称
      &quot;command&quot;: [&quot;uv&quot;,
        &quot;--directory&quot;,
        &quot;absolute/path/to/project_root&quot;,  // 服务器路径
        &quot;run&quot;,
        &quot;--with&quot;,
        &quot;mcp&quot;,
        &quot;server.py&quot;
      ]
    }
  }
}
```

完成！您的合成数据生成器 MCP 服务器已启动并连接到 Cursor。

![](img/58d59349bf9f3c736eaeeea397286d6b.png)