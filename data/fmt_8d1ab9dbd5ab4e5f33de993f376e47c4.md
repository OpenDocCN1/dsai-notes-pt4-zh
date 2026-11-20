
# 步骤1：使用向量数据库进行语义搜索、相似性搜索与 RAG

```
import logging
import numpy as np

# Configure logging
logging.basicConfig(level=logging.INFO, format='%(asctime)s %(levelname)s %(message)s')

# Replace infinity values with NaN and then fill NaN values
df.replace([np.inf, -np.inf], np.nan, inplace=True)
df.fillna('', inplace=True)

# Convert columns to string type
df['Title'] = df['Title'].astype(str)
df['abstractText'] = df['abstractText'].astype(str)

# Log the data types
logging.info(f"Title column type: {df['Title'].dtype}")
logging.info(f"abstractText column type: {df['abstractText'].dtype}")

with client.batch(
    batch_size=10,  # Specify batch size
    num_workers=2,   # Parallelize the process
) as batch:
    for index, row in df.iterrows():
        try:
            question_object = {
                "title": row.Title,
                "abstractText": row.abstractText,
            }
            batch.add_data_object(
                question_object,
                class_name="articles",
                uuid=generate_uuid5(question_object)
            )
        except Exception as e:
            logging.error(f"Error processing row {index}: {e}")
```

要检查数据是否已进入集群，您可以运行以下命令：

```
client.query.aggregate("articles").with_meta_count().do()
```

由于某种原因，我的行中只有 9997 行被矢量化。 ˙\_(ツ)_/˙

## 使用向量数据库进行语义搜索

当我们谈论向量数据库中的语义时，我们的意思是使用LLM API 将术语向量化到向量空间中，该 API 已接受过大量非结构化内容的训练。这意味着向量考虑了术语的上下文。例如，如果在训练数据中术语“塞缪尔·克莱门斯”附近多次提到术语“马克·吐温”，则这两个术语的向量在向量空间中应该彼此接近。同样，如果术语“口腔癌”与“口腔肿瘤”在训练数据中多次出现，我们预计一篇有关口腔癌的文章的向量会在向量空间中靠近一篇有关口腔肿瘤的文章。

您可以通过运行一个简单的查询来检查它是否有效：

```
response = (
    client.query
    .get("articles", ["title", "abstractText"])
    .with_additional(["id"])
    .with_near_text({ "concepts": ["Mouth Neoplasms"] })
    .with_limit(10)
    .do()
)

print(json.dumps(response, indent=4))
```

### 结果如下：

**第 1 条：** “牙龈转移是上皮样恶性间皮瘤多器官播散的第一个迹象。”本文是关于对患有恶性间皮瘤（肺癌的一种形式）并扩散到牙龈的患者进行的一项研究。该研究旨在测试不同治疗方法（化疗、剥脱术和放疗）对癌症的效果。这似乎是一篇适合返回的文章——它是关于牙龈肿瘤的，它是口腔肿瘤的一个子集。

**第 2 条：** “小唾液腺来源的肌上皮瘤。光和电子显微镜研究。”本文介绍的是从一名 14 岁男孩的牙龈中切除的肿瘤，该肿瘤已扩散到上颌的部分部位，并且由起源于唾液腺的细胞组成。这似乎也是一篇值得退回的文章——它是关于从男孩嘴里取出的肿瘤的。

**第 3 条：** “下颌骨转移性神经母细胞瘤。案件报告。”本文是一名下颌患有癌症的 5 岁男孩的案例研究。这是关于癌症，但从技术上讲不是口腔癌——下颌肿瘤（下颌肿瘤）不是口腔肿瘤的子集。

这就是**我们**所说的语义搜索——这些文章的标题或摘要中都没有“口”这个词。第一篇文章是关于牙龈（牙龈）肿瘤，这是口腔肿瘤的一个子集。第二篇文章是关于起源于受试者唾液腺的牙龈肿瘤，两者都是口腔肿瘤的亚型。第三篇文章是关于下颌肿瘤 - 从技术上讲，根据 MeSH 词汇表，下颌肿瘤不是口腔肿瘤的子集。尽管如此，矢量数据库知道下颌骨靠近嘴。

## 使用向量数据库进行相似性搜索

我们还可以利用向量数据库来查找类似的文章。我选择了一篇使用上面的口腔肿瘤查询返回的文章，标题为“牙龈转移是上皮样恶性间皮瘤多器官播散的第一个迹象”。使用该文章的 ID，我可以查询矢量数据库中的所有类似实体：

```
response = (
    client.query
    .get("articles", [ "title", "abstractText" ])
    .with_near_object({
        "id": "a7690f03-66b9-5d17-b765-8c6eb21f99c8" #id for a given article
    })
    .with_limit(10)
    .with_additional([ "distance" ])
    .do()
)

print(json.dumps(response, indent=2))
```

结果按相似性顺序排列。相似度计算为向量空间中的距离。正如您所看到的，顶部结果是《Gingival》文章 - 这篇文章是与其本身最相似的文章。

其他文章有：

**第四条**：“针对烟草使用者口腔恶性病变筛查的可行性研究”。这是关于口腔癌的，但关于如何让吸烟者报名参加筛查，而不是关于他们的治疗方式。

**第五条：** “扩大胸膜切除术和剥脱术治疗恶性胸膜间皮瘤是老年人有效、安全的细胞减灭术。”本文是关于通过胸膜切除术和剥脱术（去除肺部癌症的手术）治疗老年人胸膜间皮瘤（肺部癌症）的研究。所以这是相似的，因为它是关于间皮瘤的治疗，而不是关于牙龈肿瘤。

**第 3 条（上文）：** “下颌骨转移性神经母细胞瘤。案件报告。”同样，这是一篇关于下颌患有癌症的 5 岁男孩的文章。这是关于癌症，但从技术上讲不是口腔癌，而且这也不是像牙龈文章那样真正涉及治疗结果。

有人可**能会说，所**有这些文章都与我们最初的牙龈文章相似。很**难评估它们**的相似程度，从而评估相似性搜索的执行情况，因为这很大程度上取决于用户所说的相似的含义。您是否对有关间皮瘤治疗的其他文章感兴趣，而第一篇文章是关于间皮瘤如何扩散到牙龈的事实无关紧要？在这种情况下，第 5 条是最相似的。或者您是否有兴趣通过治疗或预防来减少任何类型的口腔癌？在这种情况下，第 4 条是最相似的。矢量数据库的一个缺点是它是一个黑匣子——我们不知道为什么这些文章被返回。

## 使用矢量数据库的检索增强生成 (RAG)

以下是如何使用向量数据库检索结果，然后将其发送到LLM进行汇总——RAG 的示例。

```
response = (
    client.query
    .get("articles", [ "title", "abstractText" ])
    .with_near_text({ "concepts": [ "Gingival metastasis as first sign of multiorgan dissemination of epithelioid malignant mesothelioma" ] })
    .with_generate(single_prompt="Please explain this article {title} like you would to someone without a medical degree.")
    .with_limit(1)
    .do()
)

print(json.dumps(response, indent=4))
```

您可以看到下面的回复：

> “当然！本文讨论的是一个人患有一种称为上皮样恶性间皮瘤的癌症的病例。这种癌症通常始于肺部或腹部内壁。然而，在这种情况下，癌症扩散到的第一个迹象是身体的其他部位出现在牙龈（牙龈），这称为牙龈转移。\n\n转移意味着癌细胞已从原来的肿瘤扩散到身体的其他部位。这很重要，因为它表明癌症在被发现之前就已经扩散到多个器官。\n\n总的来说，本文强调了早期检测和监测癌症的重要性，以及癌症扩散到身体意想不到部位的可能性。”

其实我对这个回应很失望。摘要清楚地解释了这是一项对 13 名转移性恶性间皮瘤患者进行的研究，这些患者接受了不同的治疗和结果。 RAG 输出将这篇文章描述为关于“一个人”，但根本没有提及该研究。

我们不只是总结一篇文章，而是尝试总结几篇文章。在下一个示例中，我们使用与上面相同的搜索词（口腔肿瘤），然后发送前三篇文章以及提示“以要点总结此处的关键信息”。让没有医学学位的人也能理解” LLM 。

```
response = (
    client.query
    .get(collection_name, [ "title", "abstractText" ])
    .with_near_text({ "concepts": [ "Mouth Neoplasms" ] })
    .with_limit(3)
    .with_generate(grouped_task="Summarize the key information here in bullet points. Make it understandable to someone without a medical degree.")
    .do()
)

print(response["data"]["Get"]["Articles"][0]["_additional"]["generate"]["groupedResult"])
```

结果如下：

```
- Metastatic malignant mesothelioma to the oral cavity is rare, with more cases in jaw bones than soft tissue
- Average survival rate for this type of cancer is 9-12 months
- Study of 13 patients who underwent neoadjuvant chemotherapy and surgery showed a median survival of 11 months
- One patient had a gingival mass as the first sign of multiorgan recurrence of mesothelioma
- Biopsy of new growing lesions, even in uncommon sites, is important for patients with a history of mesothelioma
- Myoepithelioma of minor salivary gland origin can show features indicative of malignant potential
- Metastatic neuroblastoma in the mandible is very rare and can present with osteolytic jaw defects and looseness of deciduous molars in children
```

**在我看来，**这比之前的回复更好——它提到了第 1 条中进行的研究、治疗方法和结果。倒数**第二个项目**是关于“小唾液腺起源的肌上皮瘤”。光和电子显微镜研究”文章，似乎是准确的一行描述。最后一个要点是关于上面引用的第 3 条，而且，似乎又是准确的一行描述。

# 步骤2：使用知识图谱进行数据检索

以下是我们如何使用知识图进行语义搜索、相似性搜索和 RAG 的高级概述：

![](img/3542c4739f8e715f81438b119bcf5028.png)

使用知识图检索数据的第一步是将数据转换为 RDF 格式。下面的代码为所有数据类型创建类和属性，然后用文章和 MeSH 术语的实例填充它。我还创建了发布日期和访问级别的属性，并用随机值填充它们，作为演示。
