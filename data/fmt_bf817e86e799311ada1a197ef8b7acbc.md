
# 场景七：语义相似度与 Embeddings 应用

## 小结

解析：这是对相似度计算结果的一个具体应用和验证。程序自动找到了与第一句话 "今天北京天气真不错" 语义最接近的句子是第四句 "北京的天气今天很好"，并且显示了它们之间高达 0.9658 的相似度分数。这再次印证了嵌入向量和余弦相似度在捕捉和量化语义相似性方面的有效性。

这段输出完整地展示了 "场景七" 代码的执行流程和结果：

1. 确认了输入文本。
1. 成功调用了硅基流动的 `Embeddings API`，并使用 `BAAI/bge-m3` 模型为所有输入文本生成了 1024 维的嵌入向量。
1. 报告了 API 调用的 Token 消耗。
1. 计算并展示了所有文本对之间的余弦相似度，结果符合人类对这些句子语义关系的直观判断（语义相同的句子相似度最高，语义相关的句子相似度较高，语义无关的句子相似度最低）。
1. 通过相似度矩阵和查找最相似句子的方式，进一步验证和突显了语义相似度的计算结果。

## Embeddings API 的主要应用场景

- **语义搜索：** 在大量文档中查找与用户查询语义最相关的文档（信息检索、知识库问答的基础）。
- **文本聚类：** 将相似的文本自动分组。
- **文本分类：** 基于文本的语义内容进行分类（如情感分析、主题分类）。
- **推荐系统：** 基于用户喜欢的内容的嵌入向量，推荐相似的内容。
- **异常检测：** 识别出与大多数文本语义显著不同的异常文本。
- **RAG (Retrieval-Augmented Generation)：** 作为 `RAG` 流程的第一步，将文档库和用户查询转换为向量，以便进行相关性检索。

## 注意事项

- `API Key` 安全：务必妥善保管您的硅基流动 `API Key`，不要硬编码在代码中或提交到公共仓库。使用环境变量是推荐做法。
- 成本：调用 `Embeddings API` 通常会根据处理的 Token 数量收费，请查阅硅基流动的定价策略。
- 模型选择：`BAAI/bge-m3` 是一个强大的通用嵌入模型，但根据具体任务和语言，硅基流动或其他平台可能提供更适合的模型。
- 向量维度：`bge-m3` 输出 1024 维向量。不同的嵌入模型维度不同，这会影响存储需求和计算效率。
- 下游应用：获取嵌入向量只是第一步，如何有效地存储、索引（如使用向量数据库 `Faiss`、`Milvus`、`ChromaDB` 等）和利用这些向量是构建实际应用的关键。

# 实现简单的检索增强生成 （RAG）

## 引言

大语言模型 （LLM） 虽然知识渊博，但其知识通常截止于训练时间点，并且无法访问私有或实时更新的信息。检索增强生成 （`RAG`，Retrieval-Augmented Generation） 是一种强大的技术，旨在解决这个问题。`RAG` 通过结合信息检索 和文本生成 ，让 `LLM` 能够基于从外部知识库检索到的相关信息来回答问题或生成内容。这不仅提高了回答的准确性和时效性，还能有效减少模型的“幻觉”现象，并允许模型利用特定的、非公开的数据。

在本场景中，我们将构建一个非常基础的 `RAG` 流程：

1. 创建一个小型的本地“知识库”（几段文本）。
1. 使用硅基流动 `Embeddings API` （`BAAI/bge-m3`） 将知识库内容和用户问题转换为向量。
1. 通过计算向量相似度，从知识库中“检索”出与问题最相关的信息片段。
1. 将用户问题和检索到的信息片段一起“增强”输入提示，发送给 `DeepSeek Chat API` （`deepseek-chat`）。
1. 由 `DeepSeek` 模型根据提供的信息生成最终的、有依据的回答。

## 前提条件

### `API Keys`

- 硅基流动 `API Key` （用于 `Embeddings`）: 环境变量 `SILICONFLOW_API_KEY`。
- `DeepSeek API Key` （用于 `Chat Completion`）: 环境变量 `DEEPSEEK_API_KEY`。

### Python 库

- `openai`： 用于调用硅基流动和 `DeepSeek` 的 `API`。 (`pip install openai`)
- `numpy`： 用于向量计算（余弦相似度）。 (`pip install numpy`)

## 核心 RAG 流程

1. 嵌入知识库 （`Embed Knowledge Base`）: 将知识库中的每个文本块转换为向量。
1. 嵌入查询 （`Embed Query`）: 将用户的查询文本转换为向量。
1. `Retrieve`: 计算查询向量与知识库中所有向量的相似度，找出最相似（最相关）的文本块。
1. `Augment`: 构建一个新的提示，包含原始查询和检索到的相关文本块，并指示模型基于该文本块回答。
1. `Generate`: 将增强后的提示发送给 `LLM` （`DeepSeek Chat`） 获取最终答案。

## 编程实践：简单的 `RAG` 实现

```
import os
import json
from openai import OpenAI, APIError, AuthenticationError
import numpy as np
import traceback

# --- 安全地获取 API Keys ---
siliconflow_api_key = os.getenv("SILICONFLOW_API_KEY")
deepseek_api_key = os.getenv("DEEPSEEK_API_KEY")

if not siliconflow_api_key:
    raise ValueError("请设置环境变量 SILICONFLOW_API_KEY")
if not deepseek_api_key:
    raise ValueError("请设置环境变量 DEEPSEEK_API_KEY")

# --- 初始化两个 OpenAI 客户端 ---
# 客户端 1: 指向硅基流动 Embeddings API
client_sf = OpenAI(
    api_key=siliconflow_api_key,
    base_url="https://api.siliconflow.cn/v1"
)
# 客户端 2: 指向 DeepSeek Chat API
client_ds = OpenAI(
    api_key=deepseek_api_key,
    base_url="https://api.deepseek.com/v1"
)

print("\n\n--- 场景八：实现简单的检索增强生成 (RAG) ---")

# --- 1. 准备一个简单的知识库 (几段文本) ---
knowledge_base_texts = [
    "Luminaris 水晶是一种罕见的矿石，据说蕴含着纯净的星辰能量，夜晚会发出柔和的蓝光。",
    "这种水晶主要被发现在极北之地的冰封洞穴深处，或是高耸入云的山脉之心。",
    "古代文献记载，Luminaris 水晶的能量可以被用来驱动精密的魔法装置，也可用于制作具有治疗效果的护身符。",
    "采集 Luminaris 水晶极其危险，不仅因为环境恶劣，更因为守护水晶的元素生物会攻击入侵者。"
]
print(f"知识库包含 {len(knowledge_base_texts)} 条信息。")

# --- 定义余弦相似度计算函数 (同场景七) ---
def cosine_similarity(v1, v2):
    vec1 = np.array(v1)
    vec2 = np.array(v2)
    dot_product = np.dot(vec1, vec2)
    norm_v1 = np.linalg.norm(vec1)
    norm_v2 = np.linalg.norm(vec2)
    if norm_v1 == 0 or norm_v2 == 0:
        return 0.0
    else:
        similarity = dot_product / (norm_v1 * norm_v2)
        return np.clip(similarity, -1.0, 1.0)

# --- 辅助函数：获取嵌入向量 ---
def get_embeddings(texts, client, model="BAAI/bge-m3"):
    """使用指定客户端和模型获取文本列表的嵌入向量"""
    try:
        response = client.embeddings.create(model=model, input=texts)
        # 返回一个向量列表，顺序与输入文本一致
        embeddings = [item.embedding for item in sorted(response.data, key=lambda x: x.index)]
        return embeddings
    except APIError as e:
        print(f"获取嵌入向量时出错 (API Error): {e.status_code} - {e.message}")
    except Exception as e:
        print(f"获取嵌入向量时发生意外错误: {e}")
    return None # 出错时返回 None


# --- RAG 核心逻辑 ---
def perform_rag(user_query):
    print(f"\n用户查询: {user_query}")

    # --- 步骤 1 & 2: 获取知识库和查询的嵌入向量 ---
    print("--- 正在获取知识库嵌入向量 (使用硅基流动 bge-m3)... ---")
    kb_embeddings = get_embeddings(knowledge_base_texts, client_sf)
    if not kb_embeddings:
        print("无法获取知识库嵌入，RAG 中止。")
        return

    print("--- 正在获取查询嵌入向量 (使用硅基流动 bge-m3)... ---")
    query_embedding = get_embeddings([user_query], client_sf) # 查询是单个文本，放入列表
    if not query_embedding:
        print("无法获取查询嵌入，RAG 中止。")
        return
    query_embedding = query_embedding[0] # 获取列表中的第一个（也是唯一一个）向量

    # --- 步骤 3: 检索最相关的知识片段 ---
    print("--- 正在计算相似度并检索相关信息... ---")
    similarities = [cosine_similarity(query_embedding, kb_emb) for kb_emb in kb_embeddings]

    # 找到最相似的知识片段的索引
    most_similar_index = np.argmax(similarities)
    retrieved_context_text = knowledge_base_texts[most_similar_index]
    max_similarity = similarities[most_similar_index]

    print(f"--- 检索到的最相关信息 (索引 {most_similar_index}, 相似度: {max_similarity:.4f}): ---")
    print(retrieved_context_text)

    # (可选) 可以设置一个相似度阈值，低于阈值则认为没有找到相关信息
    similarity_threshold = 0.5 # 示例阈值
    if max_similarity < similarity_threshold:
        print("--- 相似度低于阈值，可能没有找到强相关信息。将不使用 RAG，直接提问。---")
        retrieved_context_text = None # 不使用检索到的内容

    # --- 步骤 4: 构建增强提示 ---
    system_prompt = "你是一个乐于助人的AI助手。请根据下面提供的背景知识来回答用户的问题。如果背景知识与问题无关或不足以回答，请说明你依赖了背景知识但无法完全回答，或者直接回答（如果问题很通用）。"

    messages = [
        {"role": "system", "content": system_prompt}
    ]

    if retrieved_context_text:
        # 如果找到了相关上下文，则加入提示
        augmented_prompt = f"背景知识：\n\"\"\"\n{retrieved_context_text}\n\"\"\"\n\n用户问题：{user_query}"
        messages.append({"role": "user", "content": augmented_prompt})
    else:
        # 如果没有找到相关上下文或相似度过低，直接用原始问题
        messages.append({"role": "user", "content": user_query})

    print("\n--- 准备发送给 DeepSeek Chat 的最终消息 (已增强): ---")
    print(messages[-1]['content']) # 打印最后的用户消息内容

    # --- 步骤 5: 调用 DeepSeek Chat API 生成最终答案 ---
    print("\n--- 正在调用 DeepSeek Chat API 生成答案... ---")
    try:
        chat_response = client_ds.chat.completions.create(
            model="deepseek-chat",
            messages=messages,
            temperature=0.3 # RAG 通常使用较低温度以保证答案基于事实
        )
        final_answer = chat_response.choices[0].message.content
        print("\n--- DeepSeek 生成的最终答案: ---")
        print(final_answer)

    except APIError as e:
        print(f"调用 DeepSeek Chat 时出错 (API Error): {e.status_code} - {e.message}")
    except Exception as e:
        print(f"调用 DeepSeek Chat 时发生意外错误: {e}")


# --- 执行 RAG 流程 ---
try:
    # 示例问题，答案应该主要来自知识库的第三条
    question = "Luminaris 水晶有什么用途？"
    perform_rag(question)

    print("\n" + "="*50 + "\n")

    # 另一个示例问题，答案应该主要来自知识库的第一条
    question_2 = "Luminaris 水晶晚上会怎样？"
    perform_rag(question_2)

except AuthenticationError as e:
    print(f"\n认证错误: API Key 可能无效或权限不足。请检查 SILICONFLOW_API_KEY 或 DEEPSEEK_API_KEY。 {e}")
except Exception as e:
    print(f"\n发生意外错误: {type(e).__name__} - {e}")
    traceback.print_exc()
```

### 代码解释

1. `API Key` 与客户端：获取硅基流动和 `DeepSeek` 的 `API Key`，并分别初始化两个 `OpenAI` 客户端 （`client_sf` 用于 `Embeddings`，`client_ds` 用于 `Chat`）。
