
# 💻 网页抓取 AI 智能体

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/MQ4ddlPeHone4lxCDEMcNtqsn6f](https://qqs7y1hozd1.feishu.cn/docx/MQ4ddlPeHone4lxCDEMcNtqsn6f)

> 这个 Streamlit 应用程序允许使用 OpenAI API 和 `scrapegraphai` 库来抓取网站。

> 只需提供你的 OpenAI API 密钥，输入你想要抓取的网站的 URL，并指定你希望 AI 代理从网站中提取的内容。

[Code](https://qqs7y1hozd1.feishu.cn/wiki/Eu0gwEy4Wi8DG8k79xacrQ9An7c)

[requirements.txt](https://qqs7y1hozd1.feishu.cn/wiki/KUIuwcdudiy2xNkGqUIcW2w1nNe)

![](img/45a75f809339353deeae8fdcd8d7f394.png)

[Code (10).docx](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/BCFAbf1tgoSKD4xV2YIcsJJlnpb)

![](img/2872a44b690b0ce99cca10d87afb2565.png)

[requirements.txt (10).docx](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/XFHXbLBRgofUWgxllGncSlKlnXh)

## 特性

+ 通过提供 URL 抓取任何网站

+ 利用 OpenAI 的大语言模型（GPT-3.5-turbo 或 GPT-4）进行智能抓取

+ 通过指定你希望 AI 代理提取的内容来定制抓取任务

## 如何开始？

### 克隆 GitHub 仓库

`git clone` [仓库](https://github.com/Shubhamsaboo/awesome-llm-apps.git)

```
cd awesome-llm-apps/advanced_tools_frameworks/web_scrapping_ai_agent
```

### 安装所需的依赖项

`pip install -r requirements.txt`

### 获取你的 OpenAI API 密钥

+ 注册一个 [OpenAI 账户](https://platform.openai.com/)（或你选择的大语言模型提供商账户）并获取你的 API 密钥。

### 运行 Streamlit 应用

`streamlit run ai_scrapper.py`

## 它是如何工作的？

+ 应用程序会提示您输入 OpenAI API 密钥，该密钥用于验证和访问 OpenAI 语言模型。

+ 您可以为抓取任务选择所需的语言模型（GPT-3.5-turbo 或 GPT-4）。

+ 在提供的文本输入字段中输入你想要抓取的网站的 URL。

+ 通过输入用户提示，指定你希望 AI 代理从网站中提取的内容。

+ 应用程序使用提供的 URL、用户提示和 OpenAI 配置创建一个 `SmartScraperGraph` 对象。

+ `SmartScraperGraph` 对象会抓取网站，并使用指定的语言模型提取所需信息。

+ 抓取的结果会显示在应用程序中供您查看

## 💻 Web Scrapping AI Agent

This Streamlit app allows you to scrape a website using OpenAI API and the `scrapegraphai` library. Simply provide your OpenAI API key, enter the URL of the website you want to scrape, and specify what you want the AI agent to extract from the website.

## Features

+ Scrape any website by providing the URL

+ Utilize OpenAI's LLMs (GPT-3.5-turbo or GPT-4) for intelligent scraping

+ Customize the scraping task by specifying what you want the AI agent to extract

## How to get Started?

1. Clone the GitHub repository

```
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/advanced_tools_frameworks/web_scrapping_ai_agent
```

1. Install the required dependencies:

`pip install -r requirements.txt`

1. Get your OpenAI API Key

+ Sign up for an [OpenAI account](https://platform.openai.com/) (or the LLM provider of your choice) and obtain your API key.

1. Run the Streamlit App

`streamlit run ai_scrapper.py`

## How it Works?

+ The app prompts you to enter your OpenAI API key, which is used to authenticate and access the OpenAI language models.

+ You can select the desired language model (GPT-3.5-turbo or GPT-4) for the scraping task.

+ Enter the URL of the website you want to scrape in the provided text input field.

+ Specify what you want the AI agent to extract from the website by entering a user prompt.

+ The app creates a `SmartScraperGraph` object using the provided URL, user prompt, and OpenAI configuration.

+ The `SmartScraperGraph` object scrapes the website and extracts the requested information using the specified language model.

+ The scraped results are displayed in the app for you to view
