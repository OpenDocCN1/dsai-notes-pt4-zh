# OpenAI研究人员代理

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/CBotdNj4AouvtMxKHyIc9Lp3n6c](https://qqs7y1hozd1.feishu.cn/docx/CBotdNj4AouvtMxKHyIc9Lp3n6c)

# OpenAI Researcher Agent

A multi-agent research application built with OpenAI's Agents SDK and Streamlit. This application enables users to conduct comprehensive research on any topic by leveraging multiple specialized AI agents.

### Features

+   Multi-Agent Architecture:

+   Triage Agent: Plans the research approach and coordinates the workflow

+   Research Agent: Searches the web and gathers relevant information

+   Editor Agent: Compiles collected facts into a comprehensive report

+   Automatic Fact Collection: Captures important facts from research with source attribution

+   Structured Report Generation: Creates well-organized reports with titles, outlines, and source citations

+   Interactive UI: Built with Streamlit for easy research topic input and results viewing

+   Tracing and Monitoring: Integrated tracing for the entire research workflow

### How to get Started?

1.  Clone the GitHub repository

git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git

cd awesome-llm-apps/ai_agent_tutorials/openai_researcher_agent

1.  Install the required dependencies:

cd awesome-llm-apps/ai_agent_tutorials/openai_researcher_agent

pip install -r requirements.txt

1.  Get your OpenAI API Key

+   Sign up for an [OpenAI account](https://platform.openai.com/) and obtain your API key.

+   Set your OPENAI_API_KEY environment variable.

export OPENAI_API_KEY='your-api-key-here'

1.  Run the team of AI Agents

streamlit run openai_researcher_agent.py

Then open your browser and navigate to the URL shown in the terminal (typically http://localhost:8501).

### Research Process:

+   Enter a research topic in the sidebar or select one of the provided examples

+   Click "Start Research" to begin the process

+   View the research process in real-time on the "Research Process" tab

+   Once complete, switch to the "Report" tab to view and download the generated report

#

# OpenAI研究人员代理

一款使用OpenAI的Agents SDK和Streamlit构建的多智能体研究应用程序。该应用程序使用户能够通过利用多个专业AI智能体对任何主题进行全面研究。

### 特性

+   多智能体架构：

+   分诊代理：规划研究方法并协调工作流程

+   研究代理：在网络上搜索并收集相关信息

+   编辑代理：将收集到的事实整理成一份全面的报告

+   自动事实收集：从研究中捕获重要事实并注明来源

+   结构化报告生成：创建包含标题、大纲和来源引用的条理清晰的报告

+   交互式 UI：使用 Streamlit 构建，便于输入研究主题和查看结果

+   追踪与监控：整个研究工作流程的集成追踪

### 如何开始？

1.  克隆GitHub仓库

git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git

cd awesome-llm-apps/ai_agent_tutorials/openai_researcher_agent

1.  安装所需的依赖项：

cd awesome-llm-apps/ai_agent_tutorials/openai_researcher_agent

pip install -r requirements.txt

1.  获取你的OpenAI API密钥

+   注册一个[OpenAI账户](https://platform.openai.com/)并获取你的API密钥。

+   设置你的OPENAI_API_KEY环境变量。

export OPENAI_API_KEY='your-api-key-here'

1.  管理AI智能体团队

streamlit run openai_researcher_agent.py

然后打开浏览器，导航到终端中显示的 URL（通常为 http://localhost:8501）。

### 研究过程：

+   在侧边栏中输入一个研究主题，或选择提供的示例之一

+   点击“开始研究”启动流程

+   在“研究过程”标签页中实时查看研究过程

+   完成后，切换到“报告”标签页查看并下载生成的报告