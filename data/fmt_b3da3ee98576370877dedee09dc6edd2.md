
The customer is happy with the product but suggests offering larger options for the same price. They were pleased with the early delivery.

### 1.2.2 侧重于价格和质量

```python
prompt = f""" Your task is to generate a short summary of a product \ review from an ecommerce site to give feedback to the \ pricing deparmtment, responsible for determining the \ price of the product.

Summarize the review below, delimited by triple backticks, in at most 30 words, and focusing on any aspects \ that are relevant to the price and perceived value.

Review: {prod_review} """

response = get_completion(prompt)
print(response)
```

The customer loves the panda plush toy for its softness and cuteness, but feels it is overpriced compared to other options available.

### 1.3 关键信息提取

```python
prompt = f""" Your task is to extract relevant information from \ a product review from an ecommerce site to give \ feedback to the Shipping department.

From the review below, delimited by triple quotes \ extract the information relevant to shipping and \ delivery. Limit to 30 words.

Review: {prod_review} """

response = get_completion(prompt)
print(response)
```

The shipping department should take note that the product arrived a day earlier than expected.

### 2.1 同时概括多条文本

```python
review_1 = prod_review

review for a standing lamp

review_2 = """ Needed a nice lamp for my bedroom, and this one \ had additional storage and not too high of a price \ point. Got it fast - arrived in 2 days. The string \ to the lamp broke during the transit and the company \ happily sent over a new one. Came within a few days \ as well. It was easy to put together. Then I had a \ missing part, so I contacted their support and they \ very quickly got me the missing piece! Seems to me \ to be a great company that cares about their customers \ and products. """

review for an electric toothbrush

review_3 = """ My dental hygienist recommended an electric toothbrush, \ which is why I got this. The battery life seems to be \ pretty impressive so far. After initial charging and \ leaving the charger plugged in for the first week to \ condition the battery, I've unplugged the charger and \ been using it for twice daily brushing for the last \ 3 weeks all on the same charge. But the toothbrush head \ is too small. I’ve seen baby toothbrushes bigger than \ this one. I wish the head was bigger with different \ length bristles to get between teeth better because \ this one doesn’t. Overall if you can get this one \ around the $50 mark, it's a good deal. The manufactuer's \ replacements heads are pretty expensive, but you can \ get generic ones that're more reasonably priced. This \ toothbrush makes me feel like I've been to the dentist \ every day. My teeth feel sparkly clean! """

review for a blender

review_4 = """ So, they still had the 17 piece system on seasonal \ sale for around $49 in the month of November, about \ half off, but for some reason (call it price gouging) \ around the second week of December the prices all went \ up to about anywhere from between $70-$89 for the same \ system. And the 11 piece system went up around $10 or \ so in price also from the earlier sale price of $29. \ So it looks okay, but if you look at the base, the part \ where the blade locks into place doesn’t look as good \ as in previous editions from a few years ago, but I \ plan to be very gentle with it (example, I crush \ very hard items like beans, ice, rice, etc. in the \ blender first then pulverize them in the serving size \ I want in the blender then switch to the whipping \ blade for a finer flour, and use the cross cutting blade \ first when making smoothies, then use the flat blade \ if I need them finer/less pulpy). Special tip when making \ smoothies, finely cut and freeze the fruits and \ vegetables (if using spinach-lightly stew soften the \ spinach then freeze until ready for use-and if making \ sorbet, use a small to medium sized food processor) \ that you plan to use that way you can avoid adding so \ much ice if at all-when making your smoothie. \ After about a year, the motor was making a funny noise. \ I called customer service but the warranty expired \ already, so I had to buy another one. FYI: The overall \ quality has gone done in these types of products, so \ they are kind of counting on brand recognition and \ consumer loyalty to maintain sales. Got it in about \ two days. """

reviews = [review_1, review_2, review_3, review_4]
```

```python
for i in range(len(reviews)):
    prompt = f""" Your task is to generate a short summary of a product \ review from an ecommerce site.

Summarize the review below, delimited by triple \
backticks in at most 20 words.

Review: ```{reviews[i]}```

"""

    response = get_completion(prompt)

    print(i, response, "\n")
```

```
0 Soft and cute panda plush toy loved by daughter, but small for the price. Arrived early.

1 Great lamp with storage, fast delivery, excellent customer service, and easy assembly. Highly recommended.

2 Impressive battery life, but toothbrush head is too small. Good deal if bought around $50.

3 The reviewer found the price increase after the sale disappointing and noticed a decrease in quality over time.
```

```python
from langchain_text_splitters import RecursiveCharacterTextSplitter

# 切分文档
text_splitter = RecursiveCharacterTextSplitter(
    chunk_size=500, chunk_overlap=50)

split_docs = text_splitter.split_documents(texts)
```

## 二、构建Chroma向量库

Langchain 集成了超过 30 个不同的向量存储库。我们选择 `Chroma` 是因为它轻量级且数据存储在内存中，这使得它非常容易启动和开始使用。

LangChain 可以直接使用 OpenAI 和百度千帆的 Embedding，同时，我们也可以针对其不支持的 Embedding API 进行自定义，例如，我们可以基于 LangChain 提供的接口，封装一个 `zhupuai_embedding`，来将智谱的 Embedding API 接入到 LangChain 中。在本章的附LangChain自定义Embedding封装讲解中，我们以智谱 Embedding API 为例，介绍了如何将其他 Embedding API 封装到 LangChain 中，欢迎感兴趣的读者阅读。

注：如果你使用智谱 API，你可以参考讲解内容实现封装代码，也可以直接使用我们已经封装好的代码`zhipuai_embedding.py`，将该代码同样下载到本 Notebook 的同级目录，就可以直接导入我们封装的函数。在下面的代码 Cell 中，我们默认使用了智谱的 Embedding，将其他两种 Embedding 使用代码以注释的方法呈现，如果你使用的是百度 API 或者 OpenAI API，可以根据情况来使用下方 Cell 中的代码。

```python
# 使用 OpenAI Embedding
# from langchain.embeddings.openai import OpenAIEmbeddings
# 使用百度千帆 Embedding
# from langchain.embeddings.baidu_qianfan_endpoint import QianfanEmbeddingsEndpoint
# 使用我们自己封装的智谱 Embedding，需要将封装代码下载到本地使用
from zhipuai_embedding import ZhipuAIEmbeddings

# 定义 Embeddings
# embedding = OpenAIEmbeddings() 
embedding = ZhipuAIEmbeddings()
# embedding = QianfanEmbeddingsEndpoint()

# 定义持久化路径
persist_directory = '../../../data_base/vector_db/chroma'
```

```python
!rm -rf '../../../data_base/vector_db/chroma'  # 删除旧的数据库文件（如果文件夹中有文件的话），windows电脑请手动删除
```

```python
from langchain_community.vectorstores import Chroma

vectordb = Chroma.from_documents(
    documents=split_docs,
    embedding=embedding,
    persist_directory=persist_directory  # 允许我们将persist_directory目录保存到磁盘上
)
```

```python
print(f"向量库中存储的数量：{vectordb._collection.count()}")
```

```
向量库中存储的数量：1004
```

## 三、向量检索

### 3.1 相似度检索

Chroma的相似度搜索使用的是余弦距离，即：

其中 、 分别是向量 、 的分量。

当你需要数据库返回严谨的按余弦相似度排序的结果时可以使用`similarity_search`函数。

```python
question="什么是大语言模型"
```

```python
sim_docs = vectordb.similarity_search(question,k=3)
print(f"检索到的内容数：{len(sim_docs)}")
```

```
检索到的内容数：3
```

```python
for i, sim_doc in enumerate(sim_docs):
    print(f"检索到的第{i}个内容: \n{sim_doc.page_content[:200]}", end="\n--------------\n")
```
