
# 📊 AI数据分析代理

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/IkfGdXdxAoaro1xpG8Ic7IhGnZg](https://qqs7y1hozd1.feishu.cn/docx/IkfGdXdxAoaro1xpG8Ic7IhGnZg)

## 📊 AI Data Analysis Agent

An AI data analysis Agent built using the Agno Agent framework and Openai's gpt-4o model. This agent helps users analyze their data - csv, excel files through natural language queries, powered by OpenAI's language models and DuckDB for efficient data processing - making data analysis accessible to users regardless of their SQL expertise.

### Features

#### 📤 File Upload Support

- Upload CSV and Excel files
- Automatic data type detection and schema inference
- Support for multiple file formats

#### 💬 Natural Language Queries

- Convert natural language questions into SQL queries
- Get instant answers about your data
- No SQL knowledge required

#### 🔍 Advanced Analysis

- Perform complex data aggregations
- Filter and sort data
- Generate statistical summaries
- Create data visualizations

#### 🎯 Interactive UI

- User-friendly Streamlit interface
- Real-time query processing
- Clear result presentation

### How to Run

#### 1. Setup Environment

Clone the repository and install dependencies:

```
# Clone the repository
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/starter_ai_agents/ai_data_analysis_agent

# Install dependencies
pip install -r requirements.txt
```

#### 2. Configure API Keys

Get your OpenAI API key from [OpenAI Platform](https://platform.openai.com/).

#### 3. Run the Application

```
streamlit run ai_data_analyst.py
```

### Usage

1. Launch the application using the command above.
2. Provide your OpenAI API key in the sidebar of Streamlit.
3. Upload your CSV or Excel file through the Streamlit interface.
4. Ask questions about your data in natural language.
5. View the results and generated visualizations.

## 📊 AI数据分析代理

一个使用Agno Agent框架和OpenAI的gpt-4o模型构建的AI数据分析代理。该代理借助OpenAI的语言模型和DuckDB进行高效数据处理，通过自然语言查询帮助用户分析其数据（CSV、Excel文件），使数据分析对用户来说触手可及，无论其SQL专业知识如何。

### 功能

#### 📤 文件上传支持

- 上传 CSV 和 Excel 文件
- 自动数据类型检测和模式推断
- 支持多种文件格式

#### 💬 自然语言查询

- 将自然语言问题转换为 SQL 查询
- 获取关于您数据的即时答案
- 无需SQL知识

#### 🔍 高级分析

- 执行复杂的数据聚合
- 筛选和排序数据
- 生成统计摘要
- 创建数据可视化

#### 🎯 交互式 UI

- 用户友好的Streamlit界面
- 实时查询处理
- 清晰的结果展示

### 运行方法

#### 1. 设置环境

```
# 克隆仓库
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/starter_ai_agents/ai_data_analysis_agent

# 安装依赖项
pip install -r requirements.txt
```

#### 2. 配置 API 密钥

获取OpenAIAPI密钥从[OpenAI平台](https://platform.openai.com/)

#### 3. 运行应用程序

```
streamlit run ai_data_analyst.py
```

### 用法

1. 使用上述命令启动应用程序
2. 在Streamlit侧边栏中提供你的OpenAI API密钥
3. 通过 Streamlit 界面上传您的 CSV 或 Excel 文件
4. 用自然语言询问有关您数据的问题
5. 查看结果和生成的可视化内容
