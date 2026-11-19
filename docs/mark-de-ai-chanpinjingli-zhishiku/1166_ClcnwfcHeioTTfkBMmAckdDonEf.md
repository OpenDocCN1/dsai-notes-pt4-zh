
# 🛫 AI旅行代理

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/JsPHddyASoA2ZNxGuc2cyOenncc](https://qqs7y1hozd1.feishu.cn/docx/JsPHddyASoA2ZNxGuc2cyOenncc)

## 🛫 AI Travel Agent

This Streamlit app is an AI-powered travel Agent that generates personalized travel itineraries using OpenAI GPT-4o. It automates the process of researching, planning, and organizing your dream vacation, allowing you to explore exciting destinations with ease.

### Features

+ Research and discover exciting travel destinations, activities, and accommodations

+ Customize your itinerary based on the number of days you want to travel

+ Utilize the power of GPT-4o to generate intelligent and personalized travel plans

+ Download your itinerary as a calendar (.ics) file to import into Google Calendar, Apple Calendar, or other calendar apps

### How to get Started?

1. Clone the GitHub repository

```
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/ai_agent_tutorials/ai_travel_agent
```

1. Install the required dependencies:

```
pip install -r requirements.txt
```

1. Get your OpenAI API Key

+ Sign up for an [OpenAI account](https://platform.openai.com/) (or the LLM provider of your choice) and obtain your API key.

1. Get your SerpAPI Key

+ Sign up for an [SerpAPI account](https://serpapi.com/) and obtain your API key.

1. Run the Streamlit App

```
streamlit run travel_agent.py
```

For local LLM usage (with Ollama):

```
streamlit run local_travel_agent.py
```

### How it Works?

The AI Travel Agent has two main components:

+ Researcher: Responsible for generating search terms based on the user's destination and travel duration, and searching the web for relevant activities and accommodations using SerpAPI.

+ Planner: Takes the research results and user preferences to generate a personalized draft itinerary that includes suggested activities, dining options, and accommodations.

### Using the Calendar Download Feature

After generating your travel itinerary:

1. Click the "Download Itinerary as Calendar (.ics)" button that appears next to the "Generate Itinerary" button

1. Save the .ics file to your computer

1. Import the file into your preferred calendar application (Google Calendar, Apple Calendar, Outlook, etc.)

1. Each day of your itinerary will appear as an all-day event in your calendar

1. The complete details for each day's activities are included in the event description

This feature makes it easy to keep track of your travel plans and have your itinerary available on all your devices, even offline.

### Local vs Cloud Version

+ `travel_agent.py`: Uses OpenAI's GPT-4o for high-quality itineraries (requires OpenAI API key)

+ `local_travel_agent.py`: Uses Ollama for local LLM inference without sending data to external APIs (requires Ollama to be installed and running)

## 🛫 AI旅行代理

这个Streamlit应用程序是一个由AI驱动的旅行代理，它使用OpenAI GPT-4o生成个性化的旅行行程。它能自动完成研究、规划和组织梦想假期的过程，让您轻松探索令人兴奋的目的地。

### 特性

+ 研究并发现令人兴奋的旅游目的地、活动和住宿

+ 根据您想要旅行的天数定制您的行程

+ 利用GPT-4o的强大功能，生成智能且个性化的旅行计划

+ 将您的行程下载为日历（.ics）文件，以便导入到谷歌日历、苹果日历或其他日历应用程序中

### 如何开始？

1. 克隆GitHub仓库

```
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/ai_agent_tutorials/ai_travel_agent
```

1. 安装所需依赖项：

```
pip install -r requirements.txt
```

1. 获取你的OpenAI API密钥

+ 注册一个[OpenAI账户](https://platform.openai.com/)（或您选择的大语言模型提供商账户）并获取您的API密钥。

1. 获取您的SerpAPI密钥

+ 注册一个[SerpAPI账户](https://serpapi.com/)，并获取你的API密钥。

1. 运行Streamlit应用程序

```
streamlit run travel_agent.py
```

对于本地大语言模型（LLM）的使用（搭配Ollama）：

```
streamlit run local_travel_agent.py
```

### 它是如何工作的？

AI旅行代理有两个主要组件：

+ 研究员：负责根据用户的目的地和旅行时长生成搜索词，并使用SerpAPI在网络上搜索相关活动和住宿。

+ 行程规划师：根据研究结果和用户偏好生成个性化的行程草案，其中包括推荐的活动、餐饮选择和住宿安排。

### 使用日历下载功能

生成旅行行程后：

1. 点击出现在“生成行程”按钮旁边的“下载行程为日历文件（.ics）”按钮

1. 将.ics 文件保存到您的计算机

1. 将文件导入您首选的日历应用程序（谷歌日历、苹果日历、Outlook等）

1. 您行程中的每一天都会在您的日历中显示为全天活动

1. 每天活动的完整细节都包含在活动描述中

此功能让您轻松跟踪旅行计划，并确保您的行程在所有设备上均可使用，甚至离线状态下也能查看。

### 本地版本与云端版本

+ `travel_agent.py`: 使用OpenAI的GPT-4o生成高质量行程（需要OpenAI API密钥）

+ `local_travel_agent.py`: 使用Ollama进行本地大语言模型推理，无需将数据发送到外部API（需要安装并运行Ollama）
