
# 【LLM & KBQA】FlexKBQA：一种结合LLM的KBQA框架

> 来源：https://geekbang.feishu.cn/docx/B5mrdbr9poBsrMxWnwec4orCnMf

> 👨🏻 作者：余俊晖

> 📃 作者介绍：一名NLP炼丹师，NLP算法竞赛TOP选手

> 🛖 👸号：大模型自然语言处理

> ⏰ 收录时间：2024-05-28 17:09:42

> 🔗 原文链接：https://mp.weixin.qq.com/s?__biz=Mzg4NjI0NDg0Ng==&mid=2247484609&idx=1&sn=5640f7675c704c02182a06073cda430e&chksm=cf9dd040f8ea5956100fdb828e30030d63ab7e4203f57b142d4ff2c033acbddab0a5957853ff#rd

![](img/115228c937d56ef074e9a08fa8a88f25.png)

关注作者👸号，获取更多AI知识

## 前言

大语言模型（LLMs）在知识库问答（KBQA）领域的应用主要集中在包括但不限于以下几个方面：

1. 直接生成答案：一些方法直接利用LLMs生成答案，而不是生成中间的程序（如`SPARQL`查询）。这种方法通常依赖于模型的上下文学习能力，通过提供少量的示例（`in-context learning`）来引导模型理解问题并生成答案。

1. 程序生成：在某些情况下，LLMs被用来直接生成执行查询的程序，例如`SPARQL`查询。这种方法需要模型理解问题的结构，并能够将自然语言问题转换为有效的查询语言。

1. 语义解析：LLMs也被用于将自然语言问题映射到结构化的查询表示，如`SPARQL`。这种方法通常涉及将问题分解为实体和关系，然后构造查询以从知识库中检索答案。

1. 模型微调：在某些情况下，LLMs会在特定的KBQA任务上进行微调，以提高其在特定领域或数据集上的性能。

这些方法通常面临一些挑战，如上下文窗口大小的限制、推理成本高、模型部署困难以及在处理特定领域知识库时的准确性问题。本文介绍的方法：`FlexKBQA`，通过结合LLMs的生成能力和轻量级模型的推理效率，提供更灵活、高效且可扩展的KBQA解决方案。

## 方法

### 1. Automatic Program Sampling：自动程序采样

FlexKBQA框架中的一个关键组件，负责从知识库（`KB`）中自动生成多样化的程序（如`SPARQL`查询），这些程序随后可以被转换为自然语言问题。这个过程分为两个主要步骤：模板收集（`Template Collection`）和逐步`Grounding`（`Step-wise Grounding`）：

+   模板收集（`Template Collection`）：首先，从知识库中收集结构化查询（如`SPARQL`）的模板，这些模板包含了变量（例如，`ent0`、`rel0`、`ent1`），用于表示实体和关系。这些模板的设计旨在捕捉不同类型问题的基本逻辑。例如，一个`SPARQL`查询模板可能看起来像这样：

```
SELECT ( ?x0 AS ?value ) WHERE
{ ?x0 :type.object.type ?ent1 .
VALUES ?x1 { ?ent0 }
?x0 ?rel0 ?x1 .
FILTER ( ?x0 ! = ?x1 )
}
```

+   在这个例子中，`?type` 是一个占位符，用于在后续步骤中填充具体的实体和关系。模板的多样性对于生成覆盖用户可能遇到的各种问题类型至关重要。

+   逐步`Grounding`（`Step-wise Grounding`）：在收集了多样化的模板之后，下一步是将模板中的变量逐步替换为具体的实体和关系值。这个过程称为“`Step-wise Grounding`”，通过迭代地确定模板中变量的值，将这些模板转换为可执行的程序。这个过程通过直接将模板中的变量视为查询对象，利用`SPARQL`的查询机制来实现。例如，对于上述`SPARQL`模板，可能的接地顺序是首先确定 `ent0` ，然后是 `rel0` ，最后是 `ent1` 。一旦这些变量被赋予了具体的值，就可以将它们填充到模板中，形成一个可执行的程序。

```
SELECT ?rel0 ?ent0 ?ent1 WHERE
{ ?x0 :type.object.type ?ent1 .
VALUES ?x1 { ?ent0 }
?x0 ?rel0 ?x1 .
FILTER ( ?x0 ! = ?x1 ) }
```

+   逐步接地的目的是有效地缩小搜索空间，同时保持生成程序的多样性。这个过程可以通过自动化算法实现，例如随机采样或基于某些策略的采样。在实际应用中，人们可以根据特定的业务场景自定义和设计自己的模板。

**小结**：通过这两个步骤，能够生成大量的有效程序，这些程序随后可以被用于生成自然语言问题，为后续的模型训练提供数据支持。这种方法有助于减轻手动标注数据的负担，特别是在数据稀缺的情况下。

### 2. 低资源程序翻译（Low-Resource Program Translation）

![](img/84507f82545adba2faf1fa24f66d6b2d.png)

+   传统的（a）LLM直接在自然文本上进行训练，缺乏对程序的训练。该文认为将程序转换为自然语言更加高效，特别是资源有限的情况下。

+   使用大语言模型（LLMs）将上述生成的程序（S-expressions或`SPARQL`查询）转换为自然语言问题。这个过程涉及到构建一个提示（`prompt`），其中包括一个指令（`Inst`）来指导模型将程序翻译成问题，以及一些示例程序和问题对作为示范。这个翻译过程可以表示为：

+   下面给出了GrailQA、WebQSP和KQA Pro数据集的`prompt`，设计了不同的`prompt`，这些`prompt`直接从数据集中的问题和答案对构建。这些`prompt`帮助模型理解如何将结构化查询转换为自然语言，同时保持问题的准确性和流畅性。

+   `prompt`参考：

+   Listing 1. Example Prompt for GrailQA

清单 1.GrailQA 的示例提示

```
### Convert the s-expressions to natural language questions.
# s-expression:(AND medicine.routed_drug (JOIN medicine.routed_drug.
marketed_formulations Oxybutynin chloride 5 extended release film coated tablet))
# question:oxybutynin chloride 5 extended release film coated tablet is the
ingredients of what routed drug?
# s-expression:(ARGMAX food.food food.food.energy)
# question:when it comes to the food that has the most energy per 100g what is the
name of it?
# s-expression:(AND music.genre (JOIN (R music.genre.parent_genre) (JOIN music.genre.
albums confessions tour)))
# question:the albums confessions tour is part of what parent genre of a musical genre
?
# s-expression:(AND architecture.architect (JOIN architecture.architect.
architectural_style (JOIN (R architecture.architect.architectural_style) Josef
Fanta)))
# question:which architect has a similar architectural style to josef fanta?
# s-expression:(AND architecture.building (lt architecture.building.floors 9ˆˆhttp://
www.w3.org/2001/XMLSchema#integer))
# question:which building has less than 9 floors?
# s-expression:(AND comic_books.comic_book_series (JOIN (R comic_books.
comic_book_genre.comic_book_series_in_this_genre) (JOIN (R comic_books.
comic_book_story.genre) Case of the Namesake Murders)))
# question:case of the namesake murders is the same genre as what comic book series?
# s-expression:(AND film.director (JOIN (R media_common.quotation.author) It is an
open question whether any behavior based on fear of eternal punishment can be
regarded as ethical or should be regarded as merely cowardly.))
# question:what is the name of the author who wrote it is an open question whether any
behavior based on fear of eternal punishment can be regarded as ethical or should
be regarded as merely cowardly.?
# s-expression:(AND meteorology.beaufort_wind_force (ge meteorology.
beaufort_wind_force.wave_height 7.0ˆˆhttp://www.w3.org/2001/XMLSchema#float))
# question:for waves higher than 7.0 what is the beaufort window force?
# s-expression:(AND book.short_story (JOIN book.short_story.characters (JOIN book.
book_character.appears_in_stories Doing Clarence a Bit of Good)))
# question:what short story has a character who also is in doing clarence a bit of
good?
# s-expression:(AND education.school_category (AND (JOIN (R education.
educational_institution.school_type) Chiang Kai Shek College) (JOIN education.
school_category.schools_of_this_kind Sacred Heart High School (Roseville, Michigan
))))
# question:chiang kai shek college and sacred heart high school (roseville, michigan)
are in what category of school?
# s-expression:(ARGMIN food.bottled_water food.bottled_water.nitrate_content)
# question:the bottled water that has the least amount of nitrates?
......
# s-expression:
```

+   Listing 2. Example Prompt for WebQSP

清单 2.WebQSP 示例提示
