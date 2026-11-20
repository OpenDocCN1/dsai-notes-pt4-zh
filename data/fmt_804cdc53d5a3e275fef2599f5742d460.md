
# 🧠数学辅导Agent——带有反馈循环的智能RAG

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/OEtcdDTXloYbRPxs4T6cxR7SnIe](https://qqs7y1hozd1.feishu.cn/docx/OEtcdDTXloYbRPxs4T6cxR7SnIe)

> 本项目实现智能体增强检索生成（Agentic-RAG）架构。

> 在向量数据库和网络搜索之间路由查询，应用输入/输出护栏，并纳入人类反馈以进行持续学习。

### [Code](https://qqs7y1hozd1.feishu.cn/wiki/TRITwz3YciUXA0ksiqPcOso4nEf)

### [requirements.txt](https://qqs7y1hozd1.feishu.cn/wiki/UOHaw0ANaixLglkZZXicDcBRnSb)

![](img/cb3d75554f9f214ac2a71e17f2a55071.png)

[Code.docx](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/DiQ1baIjnoX6GExXNftcyusLnmh)

![](img/a5dbf41315e1f2193f654c83f3bd6a94.png)

[requirements.txt.docx](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/C6rSbE1Khoy56jxJT5Uc3zMtnue)

## 📌 功能

+ ✅ 输入护栏（`DSPy`）：仅接受学术数学问题。
+ 📚 知识库搜索：使用 `Qdrant` 向量数据库和 `OpenAI` 嵌入来匹配已知问题。
+ 🌐 Web 回退：在未找到良好匹配时集成 `Tavily API`。
+ ✍️ `GPT-4.1` 解释：生成逐步的数学解题步骤。
+ 🛡️ 输出护栏：用于确保正确性和安全性的过滤器。
+ 👍 人工反馈：用户对答案进行评分（是/否），记录下来以供未来学习。
+ 📊 基准测试：在 `JEEBench` 数据集上进行评估，问题数量可调整。
+ 💻 `Streamlit` UI：带有多个标签页的交互式仪表盘。

## 📚 知识库

+ **数据集：**`JEEBench` (`HuggingFace`)
+ **向量数据库：**`Qdrant`（使用 `OpenAI` 嵌入）
+ **存储：**使用 `llama-index` 构建，用于持久化嵌入并执行前 1 相似性搜索

## 🌐 网络搜索

+ 当知识库（`KB`）中没有合适匹配时，使用 `Tavily API` 进行备用搜索
+ 获取的内容被输送到 `GPT-4o` 中进行清晰解释

## 🔐 护栏

+ 输入护栏（`DSPy`）：仅接受与数学相关的学术问题
+ 输出护栏（`DSPy`）：阻止幻觉或离题内容

## 👨🏫 人工反馈

+ `Streamlit` UI允许学生在看到答案后给出👍 / 👎
+ 反馈信息会记录到本地的 `JSON` 文件中，以便未来改进

## 📊 基准测试

+ 在 50 道随机的 `JEEBench` 数学问题上进行评估
+ 当前准确率：66%
+ 基准测试结果已保存至：`benchmark/results.csv`

## 🚀 演示

要使用 `Streamlit` 运行应用程序：

```
streamlit run app/streamlit.py
```

# 🧠 Math Tutor Agent – Agentic RAG with Feedback Loop

> This project implements an Agentic-RAG architecture to simulate a math professor that solves JEE-level math questions with step-by-step explanations. The system smartly routes queries between a vector database and web search, applies input/output guardrails, and incorporates human feedback for continuous learning.

## 📌 Features

+ ✅ Input Guardrails (`DSPy`): Accepts only academic math questions.
+ 📚 Knowledge Base Search: Uses `Qdrant` Vector DB with `OpenAI` Embeddings to match known questions.
+ 🌐 Web Fallback: Integrates `Tavily API` when no good match is found.
+ ✍️ `GPT-4.1` Explanations: Generates step-by-step math solutions.
+ 🛡️ Output Guardrails: Filters for correctness and safety.
+ 👍 Human-in-the-Loop Feedback: Users rate answers (Yes/No), logged for future learning.
+ 📊 Benchmarking: Evaluated on `JEEBench` dataset with adjustable question limits.
+ 💻 `Streamlit` UI: Interactive dashboard with multiple tabs.

## 🚀 Architecture Flow

![](img/4cad08c68e12e3e8512e6561db7e47e5.png)

## 📚 Knowledge Base

+ **Dataset:** `JEEBench` (`HuggingFace`)
+ **Vector DB:** `Qdrant` (with `OpenAI` embeddings)
+ **Storage:** Built with `llama-index` to persist embeddings and perform top-1 similarity search

## 🌐 Web Search

+ Uses `Tavily API` for fallback search when the `KB` doesn't contain a good match
+ Fetched content is piped into `GPT-4o` for clean explanation

## 🔐 Guardrails

+ Input Guardrail (`DSPy`): Accepts only math-related academic questions
+ Output Guardrail (`DSPy`): Blocks hallucinated or off-topic content

## 👨‍🏫 Human-in-the-Loop Feedback

+ `Streamlit` UI allows students to give 👍 / 👎 after seeing the answer
+ Feedback is logged to a local `JSON` file for future improvement

## 📊 Benchmarking

+ Evaluated on 50 random `JEEBench` math questions
+ Current Accuracy: 66%
+ Benchmark results saved to: `benchmark/results.csv`

## 🚀 Demo

To run the app with `Streamlit`:

```
streamlit run app/streamlit.py
```
