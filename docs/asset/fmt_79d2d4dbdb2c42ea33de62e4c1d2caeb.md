
```
### Convert the s-expressions to natural language questions.
# s-expression:(JOIN (R location.location.containedby) Auburn University)
# question:where is university of auburn?
# s-expression:(JOIN (R government.political_party_tenure.politician) (JOIN (R
government.political_party.politicians_in_this_party) New Democratic Party))
# question:who founded the new democratic party?
# s-expression:(ARGMAX (JOIN (R location.location.contains) China) topic_server.
population_number)
# question:where do most chinese live?
# s-expression:(JOIN (R film.performance.actor) (AND (JOIN film.performance.character
Dorothy Gale) (JOIN (R film.film.starring) The Wizard of Oz)))
# question:who played dorothy in the film wizard of oz?
# s-expression:(AND (JOIN common.topic.notable_types College/University) (JOIN (R
education.education.institution) (JOIN (R people.person.education) Jerry Rice)))
# question:what college did jerry rice attend?
# s-expression:(AND (JOIN common.topic.notable_types City/Town/Village) (JOIN (R
location.location.contains) Oakland County))
# question:what cities are in oakland county michigan?
# s-expression:(JOIN (R people.marriage.spouse) (AND (JOIN people.marriage.time_macro
2015ˆˆhttp://www.w3.org/2001/XMLSchema#date) (AND (JOIN people.marriage.
type_of_union Marriage) (JOIN (R people.person.spouse_s) Jane Krakowski))))
# question:who is married to jane krakowski?
# s-expression:(AND (JOIN people.person.gender Female) (JOIN (R people.marriage.spouse
) (ARGMAX (JOIN (R people.person.spouse_s) Robert Downey Jr.) people.marriage.from
)))
# question:who is robert downey jr wife?
# s-expression:(JOIN (R location.religion_percentage.religion) (ARGMAX (JOIN (R
location.statistical_region.religions) United States of America) location.
religion_percentage.percentage))
# question:what is the most practiced religion in the united states?
# s-expression:(ARGMAX (AND (JOIN sports.sports_championship_event.champion Dallas
Cowboys) (JOIN (R sports.sports_team.championships) Dallas Cowboys)) time.event.
end_date)
# question:when was the last dallas cowboys super bowl win?
# s-expression:(ARGMAX (JOIN (R film.performance.film) (JOIN (R film.actor.film)
Brittany Murphy)) film.film.initial_release_date)
# question:what is the last movie brittany murphy made?
# s-expression:(AND (JOIN book.written_work.subjects Evolution) (AND (JOIN common.
topic.notable_types Book) (JOIN (R book.author.works_written) Charles Darwin)))
# question:what book did charles darwin write on evolution?
......
# s-expression:
```

Listing 3. Example Prompt for KQA Pro

清单 3.KQA Pro 的示例提示

```
### Convert the sparqls to natural language questions.
# sparql:SELECT DISTINCT ?e WHERE { ?e &lt;pred:instance_of&gt; ?c . ?c &lt;pred:name&gt; &quot;town&quot; .
?e &lt;TOID&gt; ?pv . ?pv &lt;pred:value&gt; &quot;4000000074573917&quot; . ?e &lt;OS_grid_reference&gt; ?
pv_1 . ?pv_1 &lt;pred:value&gt; &quot;SP8778&quot; . }
# question:Which town has a TOID of 4000000074573917 and has an OS grid reference of
SP8778?
# sparql:SELECT DISTINCT ?pv WHERE { ?e &lt;pred:instance_of&gt; ?c . ?c &lt;pred:name&gt; &quot;human&quot;
. ?e &lt;ISNI&gt; ?pv_1 . ?pv_1 &lt;pred:value&gt; &quot;0000 0001 2136 4821&quot; . ?e &lt;date_of_birth&gt;
?pv . }
# question:When was the person with ISNI 0000 0001 2136 4821 born?
# sparql:ASK { ?e &lt;pred:name&gt; &quot;Eve Myles&quot; . ?e &lt;official_website&gt; ?pv . ?pv &lt;pred:
value&gt; &quot;http://www.cheechandchong.com&quot; . }
# question:Is http://www.cheechandchong.com Eve Myles’s official website?
# sparql:SELECT DISTINCT ?p WHERE { ?e_1 &lt;pred:name&gt; &quot;alternative rock&quot; . ?e_2 &lt;pred:
name&gt; &quot;Greg Graffin&quot; . ?e_1 ?p ?e_2 . }
# question:What has alternative rock in common with Greg Graffin?
# sparql:ASK { ?e &lt;pred:instance_of&gt; ?c . ?c &lt;pred:name&gt; &quot;human&quot; . ?e &lt;ISNI&gt; ?pv_1 . ?
pv_1 &lt;pred:value&gt; &quot;0000 0001 0893 552X&quot; . ?e &lt;name_in_native_language&gt; ?pv . ?pv &lt;
pred:value&gt; &quot;Elias Koteas&quot; . }
# question:Is Elias Koteas the name in native language of the person with ISNI 0000
0001 0893 552X?
# sparql:SELECT DISTINCT ?qpv WHERE { ?e_1 &lt;pred:name&gt; &quot;Gregg Allman&quot; . ?e_2 &lt;pred:
name&gt; &quot;Cher&quot; . ?e_2 &lt;birth_name&gt; ?pv . ?pv &lt;pred:value&gt; &quot;Cherilyn Sarkisian&quot; . ?
e_1 &lt;spouse&gt; ?e_2 . [ &lt;pred:fact_h&gt; ?e_1 ; &lt;pred:fact_r&gt; &lt;spouse&gt; ; &lt;pred:fact_t&gt;
?e_2 ] &lt;end_time&gt; ?qpv . }
# question:When did Gregg Allman stop being the spouse of Cher (the one whose birth
name is Cherilyn Sarkisian)?
# sparql:SELECT (COUNT(DISTINCT ?e) AS ?count) WHERE { ?e &lt;pred:instance_of&gt; ?c . ?c &lt;
pred:name&gt; &quot;town&quot; . ?e &lt;postal_code&gt; ?pv . ?pv &lt;pred:value&gt; &quot;VLT&quot; . ?e &lt;area&gt; ?
pv_1 . ?pv_1 &lt;pred:unit&gt; &quot;square mile&quot; . ?pv_1 &lt;pred:value&gt; ?v . FILTER ( ?v &lt;
&quot;530&quot;ˆˆxsd:double ) . }
# question:How many towns’ postal code is VLT and area is less than 530 square miles?
# sparql:SELECT (COUNT(DISTINCT ?e) AS ?count) WHERE { ?e &lt;pred:instance_of&gt; ?c . ?c &lt;
pred:name&gt; &quot;sovereign state&quot; . ?e &lt;population&gt; ?pv . ?pv &lt;pred:unit&gt; &quot;1&quot; . ?pv &lt;
pred:value&gt; ?v . FILTER ( ?v != &quot;7600000000&quot;ˆˆxsd:double ) . }
# question:What number of sovereign states have a population not equal to 7600000000?
# sparql:SELECT DISTINCT ?pv WHERE { ?e &lt;pred:name&gt; &quot;63rd Golden Globe Awards&quot; . ?e &lt;
edition_number&gt; ?pv . }
# question:What is the edition number of the 63rd Golden Globe Awards?
# sparql:ASK { ?e &lt;pred:instance_of&gt; ?c . ?c &lt;pred:name&gt; &quot;film&quot; . ?e &lt;official_website
&gt; ?pv_1 . ?pv_1 &lt;pred:value&gt; &quot;http://www.lovelybones.com&quot; . ?e &lt;duration&gt; ?pv . ?
pv &lt;pred:unit&gt; &quot;minute&quot; . ?pv &lt;pred:value&gt; ?v . FILTER ( ?v &gt; &quot;60&quot;ˆˆxsd:double ) .
}
# question:Does the film, whose official website is http://www.lovelybones.com, have a
duration greater than 60 minutes?
......
# sparql:
```

## 3. 执行引导的自训练（Execution-Guided Self Training, EGST）

这个策略通过迭代过程来提高知识库问答（KBQA）模型的性能，尤其是在数据稀缺的情况下。这个策略的核心思想是利用模型的预测结果来指导模型的自我改进。

EGST步骤：

（1）初始化模型

首先，使用大型语言模型（LLM）生成的合成数据对（程序和对应的自然语言问题）来微调一个轻量级模型（学生模型）。这个合成数据集是通过上述的"Low-Resource Program Translation"方法生成的。

（2）生成伪标签

在每次迭代中，使用当前的学生模型（教师模型）来预测未标记的真实用户问题。这些预测的程序（伪标签）是基于模型对问题的理解和知识库中的数据生成的。

（3）执行引导过滤

对生成的伪标签进行过滤，以确保它们的质量和准确性。这个过程包括以下几个步骤：

+   错误过滤（Error Filtering）：移除那些执行错误或无法从知识库中检索答案的程序。

+   语义过滤（Semantic Filtering）：使用预训练的句子转换器（如Sentence-BERT）来计算自然语言问题和预测程序中关系之间的语义相似度，过滤掉低相似度的数据对。

+   内在推理过滤（Inherent Reasoning Filtering）：排除那些伪答案与LLM直接推理结果不一致的样本。

（4）微调学生模型

使用经过过滤的合成数据和原始的合成数据对来微调学生模型。这个过程允许模型从真实世界的数据中学习，同时保持对合成数据的泛化能力。

（5）更新教师模型

将微调后的学生模型作为下一次迭代的教师模型，模型可以在每次迭代中不断改进。

（6）迭代直至收敛

重复上述步骤，直到模型的性能不再显著提高，即达到收敛。这个过程允许模型逐渐适应真实用户问题的数据分布，从而提高其在实际应用中的性能。

小结：EGST策略的关键优势在于它能够结合合成数据和真实用户数据，通过迭代过程不断优化模型。这种方法特别适用于那些难以获得大量标注数据的场景，如零样本（Zero-shot）或少量样本（Few-shot）学习。该文通过这种方式，FlexKBQA能够在资源有限的情况下，有效地提升KBQA模型的性能。

## 4. 内在推理增强（Inherent Reasoning Augmentation）

在EGST阶段，选择那些答案与LLM生成的答案一致的样本，以减少训练数据中的噪声。此外，当语义解析方法失败时，可以使用LLM的内在推理结果作为最终答案。（主要起到补充语义的作用）

## 总结

![](img/7e39ae6268bc5ebbf10c720e2a01e3d4.png)

![](img/983aff186181093f79c0cb72c2154441.png)

![](img/89a635f9e4f5102e79d5eee2546119f1.png)

从上面结果来看还是取得了比较好的结果的，文章中对于数据合成-从知识库中自动化生成 SPARQL查询（中间结果）比较有意思的。最后在语义解析失败时，使用了LLM进行兜底。

## 参考文献

1.FlexKBQA: A Flexible LLM-Powered Framework for Few-Shot Knowledge Base Question Answering，`https://arxiv.org/abs/2308.12060v3`

1.FlexKBQA：用于少样本知识库问答的灵活LLM框架，`https://arxiv.org/abs/2308.12060v3`

2.`https://github.com/leezythu/FlexKBQA`

2.链接：`https://github.com/leezythu/FlexKBQA`
