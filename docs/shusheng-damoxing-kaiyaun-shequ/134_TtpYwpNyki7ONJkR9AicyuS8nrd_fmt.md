
# 论文分类微调数据集清洗教程

> 来源：[https://aicarrier.feishu.cn/docx/V03sdh05cojsAPxo8G7cR6zlnvb](https://aicarrier.feishu.cn/docx/V03sdh05cojsAPxo8G7cR6zlnvb)

数据集是模型微调中至关重要的一部分，本文教程主要给大家自己清洗数据集提供一个思路。

首先，贴出原数据集源链接 [arxiv 数据集](https://www.kaggle.com/datasets/Cornell-University/arxiv)。

原始数据格式：

```
{&quot;id&quot;: &quot;0711.3691&quot;,
 &quot;submitter&quot;: &quot;Eric Laporte&quot;,
 &quot;authors&quot;: &quot;Olivier Blanc (IGM-LabInfo), Matthieu Constant (IGM-LabInfo), Eric Laporte (IGM-LabInfo)&quot;,
 &quot;title&quot;: &quot;Outilex, plate-forme logicielle de traitement de textes \&#x27;ecrits&quot;,
 &quot;comments&quot;: null,
 &quot;journal-ref&quot;: &quot;Dans Verbum ex machina. Proceedings of TALN - Outilex, plate-forme logicielle de traitement de textes \&#x27;ecrits, Louvain : Belgique (2006)&quot;,
 &quot;doi&quot;: null,
 &quot;report-no&quot;: null,
 &quot;categories&quot;: &quot;cs.CL&quot;,
 &quot;license&quot;: null,
 &quot;abstract&quot;: &quot;The Outilex software platform, which will be made available to research, development and industry, comprises software components implementing all the fundamental operations of written text processing: processing without lexicons, exploitation of lexicons and grammars, language resource management. All data are structured in XML formats, and also in more compact formats, either readable or binary, whenever necessary; the required format converters are included in the platform; the grammar formats allow for combining statistical approaches with resource-based approaches. Manually constructed lexicons for French and English, originating from the LADL, and of substantial coverage, will be distributed with the platform under LGPL-LR license.&quot;,
 &quot;versions&quot;: [
 {&quot;version&quot;: &quot;v1&quot;, &quot;created&quot;: &quot;Fri, 23 Nov 2007 09:45:13 GMT&quot;},
 {&quot;version&quot;: &quot;v2&quot;, &quot;created&quot;: &quot;Tue, 27 Nov 2007 10:22:14 GMT&quot;}
 ],
 &quot;update_date&quot;: &quot;2007-11-27&quot;,
 &quot;authors_parsed&quot;: [
 [&quot;Blanc&quot;, &quot;Olivier&quot;, &quot;&quot;, &quot;IGM-LabInfo&quot;],
 [&quot;Constant&quot;, &quot;Matthieu&quot;, &quot;&quot;, &quot;IGM-LabInfo&quot;],
 [&quot;Laporte&quot;, &quot;Eric&quot;, &quot;&quot;, &quot;IGM-LabInfo&quot;]
 ]}
```

## 1.筛选出指定类型的论文

原数据集包含170w+的论文数据，类型较多，而此次赛事只挑选了其中10个类别，我们需要筛选出需要的数据类型，多类别的论文以最后一个论文为准：

```
import json

# 要保留的类别关键词
target_categories = {
    &quot;astro-ph&quot;, &quot;cond-mat.mes-hall&quot;, &quot;cond-mat.mtrl-sci&quot;,
    &quot;cs.CL&quot;, &quot;cs.CV&quot;, &quot;cs.LG&quot;,
    &quot;gr-qc&quot;, &quot;hep-ph&quot;, &quot;hep-th&quot;, &quot;quant-ph&quot;
}

input_path = &quot;----&quot;#原数据路径
output_path = &quot;----&quot;  # 使用 JSON Lines 格式输出路径

count = 0

with open(input_path, &#x27;r&#x27;) as infile, open(output_path, &#x27;w&#x27;) as outfile:
    for line in infile:
        try:
            record = json.loads(line)
            record_cats = record.get(&quot;categories&quot;, &quot;&quot;).split()
            if record_cats:
                last_cat = record_cats[-1]
                if last_cat in target_categories:
                    outfile.write(json.dumps(record) + &#x27;\n&#x27;)
                    count += 1
        except json.JSONDecodeError:
            continue  # 忽略格式错误的行

print(f&quot;筛选完成，共保存了 {count} 条记录到 {output_path}&quot;)
```

保存为`pre.py`，运行：

```
python pre.py
```

## 2.提高数据质量

筛选出来的数据条数依然太多，且质量不一，我们可以尝试提高数据质量。笔者这里关注的主要内容是摘要的长度和发布日期，将发布日期较早以及内容缺失的数据剔除，并筛选长度合适的摘要。摘要太短包含的有效分类信息越少，太长则容易造成信息冗余，误导模型。

```
import json

input_path = &quot;----&quot;          # 上一步筛选后的数据
output_path = &quot;----&quot;  # 输出高质量数据

count = 0

with open(input_path, &#x27;r&#x27;) as infile, open(output_path, &#x27;w&#x27;) as outfile:
    for line in infile:
        try:
            record = json.loads(line)

            # 获取更新日期和摘要
            update_date = record.get(&quot;update_date&quot;, &quot;&quot;)
            abstract = record.get(&quot;abstract&quot;, &quot;&quot;)

            # 过滤条件，这里根据自己的模型参数修改
            if len(abstract) &gt;= 300 and len(abstract)&lt;=4096:
                if update_date and int(update_date[:4]) &gt;= 2020:
                    outfile.write(json.dumps(record) + &#x27;\n&#x27;)
                    count += 1

        except json.JSONDecodeError:
            continue  # 跳过格式错误的行

print(f&quot;高质量筛选完成，共保留 {count} 条记录到 {output_path}&quot;)
```

仿照上文保存为py文件执行。

## 3.随机抽样

筛选出来的数据依然较多，笔者试了下，直接用于训练的话仅凭50%的A100要好几十天。故我们还需要对数据进行随机抽样，只取一小部分数据用于训练就行。

```
import json
import random

input_path = &quot;----------&quot;
output_path = &quot;----------&quot;
sample_size = 20000  # 你可以改成 10000 等其他数字

# 先将所有数据加载到内存中（30万条可以接受）
with open(input_path, &#x27;r&#x27;) as infile:
    data = [json.loads(line) for line in infile]

print(f&quot;原始数据量：{len(data)} 条&quot;)
random.seed(42) #随机数种子，可以自己随便调
# 随机抽样
sampled_data = random.sample(data, sample_size)

# 保存结果
with open(output_path, &#x27;w&#x27;) as outfile:
    for record in sampled_data:
        outfile.write(json.dumps(record) + &#x27;\n&#x27;)

print(f&quot;已随机抽取 {sample_size} 条数据保存到 {output_path}&quot;)
```

仿照上文保存为py文件执行。

## 4.准备预训练数据

预训练的数据格式为

```
{&quot;messages&quot;: [
 {
 &quot;role&quot;: &quot;assistant&quot;,
 &quot;content&quot;: &quot;This is a paper with ID 0711.3691, titled &quot;Outilex, plate-forme logicielle de traitement de textes \&#x27;ecrits&quot;, submitted by Eric Laporte. The authors are Olivier Blanc (IGM-LabInfo), Matthieu Constant (IGM-LabInfo), Eric Laporte (IGM-LabInfo).\nThe paper belongs to the cs.CL category and is published in Dans Verbum ex machina. Proceedings of TALN - Outilex, plate-forme\n  logicielle de traitement de textes \&#x27;ecrits, Louvain : Belgique (2006). The latest version is v2, created on November 27, 2007. No DOI information available. No license information available.\n\nAbstract:\nThe Outilex software platform, which will be made available to research, development and industry, comprises software components implementing all the fundamental operations of written text processing: processing without lexicons, exploitation of lexicons and grammars, language resource management. All data are structured in XML formats, and also in more compact formats, either readable or binary, whenever necessary; the required format converters are included in the platform; the grammar formats allow for combining statistical approaches with resource-based approaches. Manually constructed lexicons for French and English, originating from the LADL, and of substantial coverage, will be distributed with the platform under LGPL-LR license.&quot;
 }
 ]}
```

通过以下代码进行转换：



```python
import json

input_path = "--------"  # 刚刚随机抽样的数据
output_path = "--------"

count = 0
skipped = 0

with open(input_path, 'r') as infile, open(output_path, 'w') as outfile:
    for line in infile:
        try:
            record = json.loads(line)

            paper_id = record.get("id")
            title = record.get("title", "").replace("\n", " ").strip()
            submitter = record.get("submitter")
            authors = record.get("authors", "").replace("\n", " ").strip()
            categories = record.get("categories", "").split()
            abstract = record.get("abstract", "").strip()

            # 必要字段缺失，跳过
            if not all([paper_id, title, submitter, authors, categories, abstract]):
                skipped += 1
                continue

            category = categories[-1]
            journal = record.get("journal-ref", "No journal information available.")
            doi = record.get("doi", "No DOI information available.")
            license_url = record.get("license", "No license information available.")

            versions = record.get("versions", [])
            if not versions or "created" not in versions[-1]:
                skipped += 1
                continue
            version_str = versions[-1].get("version", "unknown")
            update_date = versions[-1]["created"]

            content = f"""This is a paper with ID {paper_id}, titled "{title}", submitted by {submitter}. The authors are {authors}.
The paper belongs to the {category} category and is published in {journal}. The latest version is {version_str}, created on {update_date}. DOI: {doi}. License: {license_url}.

Abstract:
{abstract}
"""

            result = {
                "messages": [
                    {
                        "role": "assistant",
                        "content": content
                    }
                ]
            }

            outfile.write(json.dumps(result) + "\n")
            count += 1

        except json.JSONDecodeError:
            skipped += 1
            continue

print(f"转换完成，共保存 {count} 条数据到 {output_path}，跳过 {skipped} 条不完整数据")
```

仿照上文保存为 `py` 文件执行。

## 5. SFT微调数据集准备

微调数据集格式为：

```json
{
  "system": "你是个优秀的论文分类师",
  "conversation": [
    {
      "human": "Based on the title 'Outilex, plate-forme logicielle de traitement de textes \'ecrits\'', authors 'Olivier Blanc (IGM-LabInfo), Matthieu Constant (IGM-LabInfo), Eric\n  Laporte (IGM-LabInfo)', and abstract 'The Outilex software platform, which will be made available to research, development and industry, comprises software components implementing all the fundamental operations of written text processing: processing without lexicons, exploitation of lexicons and grammars, language resource management. All data are structured in XML formats, and also in more compact formats, either readable or binary, whenever necessary; the required format converters are included in the platform; the grammar formats allow for combining statistical approaches with resource-based approaches. Manually constructed lexicons for French and English, originating from the LADL, and of substantial coverage, will be distributed with the platform under LGPL-LR license.', please determine the scientific category of this paper.  \n\nA. astro-ph\nB. cond-mat.mes-hall\nC. cond-mat.mtrl-sci\nD. cs.CL\nE. cs.CV\nF. cs.LG\nG. gr-qc\nH. hep-ph\nI. hep-th\nJ. quant-ph",
      "assistant": "D"
    }
  ]
}
```

将随机抽样的数据集转换为如上格式：

```python
import json
import random

input_file = "---------"   # 20000条原始数据文件路径
output_file = "----------"

# 类别对应选项映射
label_map = {
    "astro-ph": "A",
    "cond-mat.mes-hall": "B",
    "cond-mat.mtrl-sci": "C",
    "cs.CL": "D",
    "cs.CV": "E",
    "cs.LG": "F",
    "gr-qc": "G",
    "hep-ph": "H",
    "hep-th": "I",
    "quant-ph": "J"
}

options_text = (
    "\n\nA. astro-ph\nB. cond-mat.mes-hall\nC. cond-mat.mtrl-sci\nD. cs.CL\n"
    "E. cs.CV\nF. cs.LG\nG. gr-qc\nH. hep-ph\nI. hep-th\nJ. quant-ph"
)

# 读取所有数据
with open(input_file, 'r', encoding='utf-8') as f:
    data = [json.loads(line) for line in f]

# 随机抽样1000条
random.seed(42)
sampled = random.sample(data, 1000)

with open(output_file, 'w', encoding='utf-8') as f_out:
    count = 0
    for item in sampled:
        # 多类别时取最后一个类别（通常以空格分割）
        categories_str = item.get("categories", "").strip()
        if not categories_str:
            continue
        last_category = categories_str.split()[-1]

        if last_category not in label_map:
            continue

        title = item.get("title", "").replace("\n", " ").strip()
        authors = item.get("authors", "").replace("\n", " ").strip()
        abstract = item.get("abstract", "").replace("\n", " ").strip()
        if not title or not authors or not abstract:
            continue

        human_text = (
            f"Based on the title '{title}', authors '{authors}', and abstract '{abstract}', "
            f"please determine the scientific category of this paper.{options_text}"
        )

        finetune_sample = {
            "system": "你是个优秀的论文分类师",
            "conversation": [
                {
                    "human": human_text,
                    "assistant": label_map[last_category]
                }
            ]
        }

        f_out.write(json.dumps(finetune_sample, ensure_ascii=False) + "\n")
        count += 1

print(f"转换完成，共生成{count}条微调数据，保存到 {output_file}")
```

仿照上文保存为 `py` 文件执行。

ok,进行到这，我们就已经得到了一个自己洗的数据集，可以尝试微调训练了，后续可以根据微调结果进一步修改参数。
