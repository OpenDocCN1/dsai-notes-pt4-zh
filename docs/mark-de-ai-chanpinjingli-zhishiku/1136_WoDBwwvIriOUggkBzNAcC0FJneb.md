# 使用 Cohere Embed-4 的视觉 RAG 🖼️

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/Y2aYdEifkoCGYFxtFNYcrNYTnse](https://qqs7y1hozd1.feishu.cn/docx/Y2aYdEifkoCGYFxtFNYcrNYTnse)

# Vision RAG with Cohere Embed-4 🖼️

A powerful visual Retrieval-Augmented Generation (RAG) system that utilizes Cohere's state-of-the-art Embed-4 model for multimodal embedding and Google's efficient Gemini 2.5 Flash model for answering questions about images and PDF pages.

## Features

+   Multimodal Search: Leverages Cohere Embed-4 to find the most semantically relevant image (or PDF page image) for a given text question.

+   Visual Question Answering: Employs Google Gemini 2.5 Flash to analyze the content of the retrieved image/page and generate accurate, context-aware answers.

+   Flexible Content Sources:

+   Use pre-loaded sample financial charts and infographics.

+   Upload your own custom images (PNG, JPG, JPEG).

+   Upload PDF documents: Automatically extracts pages as images for analysis.

+   No OCR Required: Directly processes complex images and visual elements within PDF pages without needing separate text extraction steps.

+   Interactive UI: Built with Streamlit for easy interaction, including content loading, question input, and result display.

+   Session Management: Remembers loaded/uploaded content (images and processed PDF pages) within a session.

## Requirements

+   Python 3.8+

+   Cohere API key

+   Google Gemini API key

## How to Run

Follow these steps to set up and run the application:

1.  Clone and Navigate to Directory :

1.  git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/rag_tutorials/vision_rag

1.  Install Dependencies:

1.  pip install -r requirements.txt

1.  (Ensure you have the latest PyMuPDF installed along with other requirements)

1.  Set up your API keys:

+   [Get a Cohere API ](https://dashboard.cohere.com/api-keys)key from: https://dashboard.cohere.com/api-keys

+   [Get a Google API k](https://aistudio.google.com/app/apikey)ey from: https://aistudio.google.com/app/apikey

1.  Run the Streamlit app:

1.  streamlit run vision_rag.py

1.  Access the Web Interface:

+   Streamlit will provide a local URL (usually http://localhost:8501) in your terminal.

+   Open this URL in your web browser.

## How It Works

The application follows a two-stage RAG process:

1.  Retrieval:

+   When you load sample images or upload your own images/PDFs:

+   Regular images are converted to base64 strings.

+   PDFs are processed page by page: Each page is rendered as an image, saved temporarily, and converted to a base64 string.

+   Cohere's embed-v4.0 model (with input_type="search_document") is used to generate a dense vector embedding for each image or PDF page image.

+   When you ask a question, the text query is embedded using the same embed-v4.0 model (with input_type="search_query").

+   Cosine similarity is calculated between the question embedding and all image embeddings.

+   The image with the highest similarity score (which could be a regular image or a specific PDF page image) is retrieved as the most relevant context.

1.  Generation:

+   The original text question and the retrieved image/page image are passed as input to the Google gemini-2.5-flash-preview-04-17 model.

+   Gemini analyzes the image content in the context of the question and generates a textual answer.

## Usage

1.  Enter your Cohere and Google API keys in the sidebar.

1.  Load content:

+   Click "Load Sample Images" to download and process the built-in examples.

+   OR/AND Use the "Upload Your Images or PDFs" section to upload your own image or PDF files.

1.  Once content is loaded and processed (embeddings generated), the "Ask a Question" section will be enabled.

1.  Optionally, expand "View Loaded Images" to see thumbnails of all images and processed PDF pages currently in the session.

1.  Type your question about the loaded content into the text input field.

1.  Click "Run Vision RAG".

1.  View the results:

+   The Retrieved Image/Page deemed most relevant to your question (caption indicates source PDF and page number if applicable).

+   The Generated Answer from Gemini based on the image and question.

## Use Cases

+   Analyze financial charts and extract key figures or trends.

+   Answer specific questions about diagrams, flowcharts, or infographics within images or PDFs.

+   Extract information from tables or text within screenshots or PDF pages without explicit OCR.

+   Build and query visual knowledge bases (from images and PDFs) using natural language.

+   Understand the content of various complex visual documents, including multi-page reports.

## Note

+   Image and PDF processing (page rendering + embedding) can take time, especially for many items or large files. Sample images are cached after the first load; PDF processing currently happens on each upload within a session.

+   Ensure your API keys have the necessary permissions and quotas for the Cohere and Gemini models used.

+   The quality of the answer depends on both the relevance of the retrieved image and the capability of the Gemini model to interpret the image based on the question.

#

# 使用 Cohere Embed-4 的视觉 RAG 🖼️

一个强大的视觉检索增强生成（RAG）系统，该系统利用Cohere的最先进的Embed-4模型进行多模态嵌入，并使用谷歌高效的Gemini 2.5 Flash模型来回答有关图像和PDF页面的问题。

## 功能

+   多模态搜索：利用Cohere Embed-4为给定的文本问题找到语义上最相关的图像（或PDF页面图像）。

+   视觉问答：采用谷歌Gemini 2.5 Flash分析检索到的图像/页面内容，并生成准确、有上下文感知的答案。

+   灵活的内容来源：

+   使用预加载的示例财务图表和信息图。

+   上传您自己的自定义图像（PNG、JPG、JPEG）。

+   上传PDF文档：自动提取页面作为图像进行分析。

+   无需OCR：直接处理PDF页面内的复杂图像和视觉元素，无需单独的文本提取步骤。

+   交互式 UI：采用 Streamlit 构建，便于交互，包括内容加载、问题输入和结果显示。

+   会话管理：记住会话中加载/上传的内容（图像和已处理的PDF页面）。

## 要求

+   Python 3.8+

+   Cohere API密钥

+   谷歌双子星API密钥

## 如何运行

按照以下步骤设置并运行应用程序：

1.  克隆并导航到目录：

1.  git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/rag_tutorials/vision_rag

1.  安装依赖项：

1.  pip install -r requirements.txt

1.  (确保你已经安装了最新的PyMuPDF以及其他所需依赖)

1.  设置您的 API 密钥：

+   [从以下位置获取Cohere API](https://dashboard.cohere.com/api-keys)密钥：https://dashboard.cohere.com/api-keys

+   [从以下链接获取 Google API](https://aistudio.google.com/app/apikey) 密钥：https://aistudio.google.com/app/apikey

1.  运行Streamlit应用程序：

1.  streamlit run vision_rag.py

1.  访问Web界面：

+   Streamlit将在您的终端中提供一个本地URL（通常为http://localhost:8501）。

+   在您的网络浏览器中打开此URL。

## 工作原理

该应用程序遵循两阶段的RAG流程：

1.  检索：

+   当您加载示例图像或上传自己的图像/PDF时：

+   常规图像被转换为Base64字符串。

+   PDF文件按页处理：每页都被渲染成图像，临时保存，然后转换为Base64字符串。

+   Cohere的embed-v4.0模型（使用input_type="search_document"）用于为每个图像或PDF页面图像生成密集矢量嵌入。

+   当你提问时，文本查询会使用相同的embed-v4.0模型（input_type="search_query"）进行嵌入。

+   在问题嵌入和所有图像嵌入之间计算余弦相似度。

+   相似度得分最高的图像（可能是普通图像或特定的PDF页面图像）被检索出来作为最相关的上下文。

1.  生成：

+   原始文本问题和检索到的图像/页面图像作为输入传递给谷歌gemini-2.5-flash-preview-04-17模型。

+   双子星在问题的语境中分析图像内容，并生成文本答案。

## 用法

1.  在侧边栏中输入您的Cohere和Google API密钥。

1.  加载内容：

+   点击“加载示例图像”以下载并处理内置示例。

+   或/与使用“上传您的图像或PDF”部分上传您自己的图像或PDF文件。

1.  一旦内容加载并处理完毕（生成嵌入向量），“提问”部分将被启用。

1.  您可以选择展开“查看已加载图像”，以查看当前会话中所有图像和已处理PDF页面的缩略图。

1.  在文本输入框中输入你关于已加载内容的问题。

1.  点击“运行视觉RAG”。

1.  查看结果：

+   检索到的图像/页面被认为与您的问题最相关（标题中会注明来源PDF和页码，如果适用）。

+   基于图像和问题由Gemini生成的答案。

## 使用案例

+   分析财务图表并提取关键数据或趋势。

+   回答有关图像或PDF中图表、流程图或信息图的具体问题。

+   无需显式OCR即可从屏幕截图或PDF页面中的表格或文本中提取信息。

+   使用自然语言构建和查询可视化知识库（来自图像和PDF）。

+   理解各种复杂视觉文档的内容，包括多页报告。

## 注意

+   图像和PDF处理（页面渲染 + 嵌入）可能需要一些时间，特别是对于大量项目或大文件而言。示例图像在首次加载后会被缓存；PDF处理目前在会话内每次上传时都会进行。

+   确保你的 API 密钥具有所使用的 Cohere 和 Gemini 模型所需的权限和配额。

+   答案的质量既取决于检索到的图像的相关性，也取决于Gemini模型根据问题解读图像的能力。