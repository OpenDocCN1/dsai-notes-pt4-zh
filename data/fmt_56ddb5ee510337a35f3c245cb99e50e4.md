
# 🎙️ 客户支持语音座席

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/MWQPdH9njoN1pzxSR0Vc9seBnlh](https://qqs7y1hozd1.feishu.cn/docx/MWQPdH9njoN1pzxSR0Vc9seBnlh)

> 一款由OpenAI SDK驱动的客户支持代理应用程序，以语音形式回答有关知识库的问题。

> 该系统使用Firecrawl爬取留档网站，利用OpenAI的GPT-4o和TTS功能，使用Qdrant将内容处理成可搜索的知识库，并为用户查询提供文本和语音响应。

[Code](https://qqs7y1hozd1.feishu.cn/wiki/W20swnsaJiz5gVkkKLpclmqWnWg)

[`requirements.txt`](https://qqs7y1hozd1.feishu.cn/wiki/Kb18ww9mRihiZ3kZ82lctQvOnke)

![](img/89a8c3708a3e00d591701aa44c3f8ad7.png)

[`Code (12).docx`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/MNUpbXv0voGHnJx5SupcFqponsd)

![](img/0aa8e0fda533d16947eae9301e3b8c2d.png)

[`requirements.txt (12).docx`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Jld6bYkO0ozBq1xdyRGcPRidnDg)

## 功能

### 知识库创建

- 使用Firecrawl爬取留档网站
- 使用Qdrant向量数据库存储和索引内容
- 使用FastEmbed生成用于语义搜索功能的嵌入向量

### AI智能体团队

- 留档处理器：分析留档内容并生成清晰、简洁的用户查询响应
- TTS代理：将文本响应转换为具有适当节奏和强调的自然语音
- 语音定制：支持多种OpenAI TTS语音：
  - 合金、灰烬、民谣、珊瑚、回声、寓言、玛瑙、新星、圣人、微光、诗句

### 交互式界面

- 带有侧边栏配置的简洁 Streamlit UI
- 实时留档搜索和响应生成
- 内置音频播放器，具备下载功能
- 系统初始化和查询处理的进度指示器

## 如何运行

### 克隆仓库

```
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/ai_agent_tutorials/ai_voice_agent_openaisdk
```

### 安装依赖项

```
pip install -r requirements.txt
```

### 配置 API 密钥

- 获取OpenAI API密钥从[OpenAI平台](https://platform.openai.com/)
- 获取Qdrant API密钥和URL从[Qdrant Cloud](https://cloud.qdrant.io/)
- 获取用于留档爬取的Firecrawl API密钥

### 运行应用程序

```
streamlit run ai_voice_agent_docs.py
```

### 使用界面

- 在侧边栏中输入 API 凭证
- 输入你想了解的留档URL
- 从下拉菜单中选择您喜欢的语音
- 点击“初始化系统”来处理留档
- 提问并接收文本和语音回复

## 功能详情

### 知识库创建

- 从您的留档中构建可搜索的知识库
- 保留文档结构和元数据
- 支持多页面抓取（默认配置下限制为5页）

### 向量搜索

- 使用FastEmbed生成嵌入向量
- 用于查找相关内容的语义搜索功能
- 使用Qdrant进行高效文档检索

### 语音生成

- 使用OpenAI的TTS模型进行高质量语音合成
- 多种语音选项可供定制
- 具有适当节奏和强调的自然语音模式

# 🎙️ Customer Support Voice Agent

An OpenAI SDK powered customer support agent application that delivers voice-powered responses to questions about your knowledge base using OpenAI's GPT-4o and TTS capabilities. The system crawls through documentation websites with Firecrawl, processes the content into a searchable knowledge base with Qdrant, and provides both text and voice responses to user queries.

## Features

### Knowledge Base Creation

- Crawls documentation websites using Firecrawl
- Stores and indexes content using Qdrant vector database
- Generates embeddings for semantic search capabilities using FastEmbed

### AI Agent Team

- Documentation Processor: Analyzes documentation content and generates clear, concise responses to user queries
- TTS Agent: Converts text responses into natural-sounding speech with appropriate pacing and emphasis
- Voice Customization: Supports multiple OpenAI TTS voices:
  - alloy, ash, ballad, coral, echo, fable, onyx, nova, sage, shimmer, verse

### Interactive Interface

- Clean Streamlit UI with sidebar configuration
- Real-time documentation search and response generation
- Built-in audio player with download capability
- Progress indicators for system initialization and query processing

## How to Run

### Clone the repository

```
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/ai_agent_tutorials/ai_voice_agent_openaisdk
```

### Install dependencies

```
pip install -r requirements.txt
```

### Configure API Keys

- Get OpenAI API key from [OpenAI Platform](https://platform.openai.com/)
- Get Qdrant API key and URL from [Qdrant Cloud](https://cloud.qdrant.io/)
- Get Firecrawl API key for documentation crawling

### Run the Application

```
streamlit run ai_voice_agent_docs.py
```

### Use the Interface

- Enter API credentials in the sidebar
- Input the documentation URL you want to learn about
- Select your preferred voice from the dropdown
- Click "Initialize System" to process the documentation
- Ask questions and receive both text and voice responses

## Features in Detail

### Knowledge Base Creation

- Builds a searchable knowledge base from your documentation
- Preserves document structure and metadata
- Supports multiple page crawling (limited to 5 pages per default configuration)

### Vector Search

- Uses FastEmbed for generating embeddings
- Semantic search capabilities for finding relevant content
- Efficient document retrieval using Qdrant

### Voice Generation

- High-quality text-to-speech using OpenAI's TTS models
- Multiple voice options for customization
- Natural speech patterns with proper pacing and emphasis
