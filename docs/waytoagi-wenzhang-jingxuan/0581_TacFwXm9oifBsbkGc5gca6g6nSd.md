# GPT4作为考官也很靠谱

> 来源：[https://waytoagi.feishu.cn/docx/LAPJd3OkVownewxBlAScG9n6n3c](https://waytoagi.feishu.cn/docx/LAPJd3OkVownewxBlAScG9n6n3c)

除了答题，GPT4作为考官也很靠谱。以下为一个群友的贡献

来源于Andrej Karpathy  https://twitter.com/karpathy

> **贡献者**：醉猫

> **思路**：讲解概念→出题→解题说明→anki复习卡片

> **拓展**：按照这个思路可以做一个流程，是一个老师出题利器，也可以针对自己的学习有更好复盘

> **例如**：https://chat.openai.com/share/a54de047-8796-47b4-937d-5b7dc70bc16e

## 以下是训练展示：

> 原文是英文

> 加了翻译是绿色

> 问题：

> I am a teacher at a University and I would like to create multiple choice questions for my students based on some seed material. Can you help me generate these questions if I give you the material? Here is my desired format for each multiple choice question:

> 

> {QUESTION}

> A: {CANDIDATE ANSWER 1}

> B: {CANDIDATE ANSWER 2}

> C: {CANDIDATE ANSWER 3}

> D: {CANDIDATE ANSWER 4}

> Answer: {LETTER}

> 

> For example:

> 

> What is the most populous state of the United States?

> A: Florida

> B: Texas

> C: California

> D: New York

> Answer: C

> 

> You'll notice that the Multiple Choice options are designed to be somewhat hard, with distractor answers that are plausible (e.g. Texas, Florida and New York are quite populous but not the most populous).

> 

> Sounds good? If so, please respond with only  "I understand.", then I will give you the seed material I would like questions about.

> 中文翻译：

> 我是一所大学的老师，我想根据一些种子材料为我的学生创造多项选择题。如果我给予你材料，你能帮我生成这些问题吗？以下是我想要的每个选择题的格式：

> 

> {问题}

> 答：{候选答案1}

> B：{候选答案2}

> C：{候选答案3}

> D：{候选答案4}

> 回答：{LETTER}

> 

> 例如：

> 

> 美国人口最多的州是哪个？

> 答：佛罗里达

> B：得克萨斯

> C：加州

> D：纽约

> 答案：C

> 

> 您会注意到，多项选择选项的设计有些困难，干扰因素的答案是合理的（例如：德克萨斯州、佛罗里达州和纽约州人口相当多，但不是人口最多的）。

> 

> 听起来不错吧？如果是这样，请只回答“我明白。”，然后我会给予你我想要的种子材料的问题。

> Here is the seed material, in triple quotes just for clarity:

> 

> """

> Carbon (from Latin carbo 'coal') is a chemical element with the symbol C and atomic number 6. It is nonmetallic and tetravalent—its atom making four electrons available to form covalent chemical bonds. It belongs to group 14 of the periodic table.[14] Carbon makes up about 0.025 percent of Earth's crust.[15] Three isotopes occur naturally, 12C and 13C being stable, while 14C is a radionuclide, decaying with a half-life of about 5,730 years.[16] Carbon is one of the few elements known since antiquity.[17]

> """

>

> 以下是种子材料，为了清楚起见，在三重引号中：

> 

> ““”

> 碳（来自拉丁语carbo“coal”）是一种化学元素，符号为C，原子序数为6。它是非金属的四价原子，它的四个电子可形成共价化学键。它属于元素周期表的第14族。[14]碳约占地壳的0.025%。[15]自然界中有三种同位素，12C和13C是稳定的，而14C是一种放射性核素，半衰期约为5，730年。[16]碳是自古以来已知的少数元素之一。[17]

> ““”

>

> Now please generate one Multiple Choice question. Make the correct answer be A.

> 现在请生成一个多项选择题。正确答案为A。

> What is the atomic number of Carbon?

> A: 6

> B: 8

> C: 7

> D: 12

> Answer：A

> 碳的原子序数是多少？

> A：6

> B：8

> C：7

> D：12

> 答案：A

> One more please, make the correct answer be C.请再答一个问题，使正确答案为C。

> Which group of the periodic table does Carbon belong to?

> A: Group 12

> B: Group 15

> C: Group 14

> D: Group 18

> Answer：C

> 碳属于元素周期表的哪一组？

> A：第12组

> B：第15组

> C：第14组

> D：第18组

>

> 答案：C

> One more please, make the correct answer be D.请再来一个，使正确答案为D。

> Approximately how much of Earth's crust does Carbon make up?

> A: 25%

> B: 2.5%

> C: 0.0025%

> D: 0.025%

> Answer：D

> 碳元素在地壳中所占的比例大约是多少？

> A：25%

> B：2.5%

> C：0.0025%

> D：0.025%

> 答案：D

后面不展示了，可以这样一直问下去，你学会了么