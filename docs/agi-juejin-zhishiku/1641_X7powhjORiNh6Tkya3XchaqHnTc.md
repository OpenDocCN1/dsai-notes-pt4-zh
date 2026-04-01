# 9个范例带你入门LangChain

> 来源：[https://agijuejin.feishu.cn/docx/PdgJdEYSFoXhSsxr49Jc6709nzf](https://agijuejin.feishu.cn/docx/PdgJdEYSFoXhSsxr49Jc6709nzf)

> 作者：明训 | 算法美食屋

> 原文：https://mp.weixin.qq.com/s/qHUxO6Ml-O1PCK1bc9uD7g

**前方干货预警：这可**能是**你心心念念想找的 最好懂最**具实操性 的 langchain教程。 本文通过演示9个具有代表性的应用范例，带你零基础入门langchain。

**公众号算法美食屋后台**回复关键词： langchain ，获取本文notebook源代码。

9个范例功能列表如下：

1， **文本总结 **(Summarization): 对文本/聊天内容的重点内容总结。

2， **文档问答 **(Question and Answering Over Documents): 使用文档作为上下文信息，基于文档内容进行问答。

3， **信息抽取 **(Extraction): 从文本内容中抽取结构化的内容。

4， **结果评估 **(Evaluation): 分析并评估LLM输出的结果的好坏。

5， **数据库问答 **(Querying Tabular Data): 从数据库/类数据库内容中抽取数据信息。

6， **代码理解 **(Code Understanding): 分析代码，并从代码中获取逻辑，同时也支持QA。

7， **API交互 **(Interacting with APIs): 通过对API文档的阅读，理解API文档并向真实世界调用API获取真实数据。

8， **聊天机器人 **(Chatbots): 具备记忆能力的聊天机器人框架（有UI交互能力)。

9， **智能体 **(Agents): 使用LLMs进行任务分析和决策，并调用工具执行决策。

```
# 在我们开始前，安装需要的依赖
!pip install langchain
!pip install openai
!pip install tiktoken 
!pip install faiss-cpu 
```

```
openai_api_key=&#x27;YOUR_API_KEY&#x27;
# 使用你自己的OpenAI API key
```

## 一， 文本总结(Summarization)

扔给LLM一段文本，让它给你生成总结可以说是最常见的场景之一了。

目前最火的应用应该是 chatPDF，就是这种功能。

### **1，短文本总结**

```
**
# S****ummari****es O**f Short Tex**t

fro**m langchain.llms import OpenAI
from langchain import PromptTemplate

llm = OpenAI(temperature=0, model_name = &#x27;gpt-3.5-turbo&#x27;, openai_api_key=openai_api_key) # 初始化LLM模型

# 创建模板
template = &quot;&quot;&quot;
%INSTRUCTIONS:
Please summarize the following piece of text.
Respond in a manner that a 5 year old would understand.

%TEXT:
{text}
&quot;&quot;&quot;

# 创建一个 Lang Chain Prompt 模板，稍后可以插入值
prompt = PromptTemplate(
    input_variables=[&quot;text&quot;],
    template=template,
)
```

```
confusing_text = &quot;&quot;&quot;
For the next 130 years, debate raged.
Some scientists called Prototaxites a lichen, others a fungus, and still others clung to the notion that it was some kind of tree.
“The problem is that when you look up close at the anatomy, it’s evocative of a lot of different things, but it’s diagnostic of nothing,” says Boyce, an associate professor in geophysical sciences and the Committee on Evolutionary Biology.
“And it’s so damn big that when whenever someone says it’s something, everyone else’s hackles get up: ‘How could you have a lichen 20 feet tall?’”
&quot;&quot;&quot;
```

```
**print** (&quot;-------** Prom**pt Begin -------&quot;)
# 打印模板内容
final_prompt = prompt.format(text=confusing_text)
print(final_prompt)

print (&quot;------- Prompt End -------&quot;)
```

```
------- Prompt Begin -------

%INSTRUCTIONS:
Please summarize the following piece of text.
Respond in a manner that a 5 year old would understand.

%TEXT:

For the next 130 years, debate raged.
Some scientists called Prototaxites a lichen, others a fungus, and still others clung to the notion that it was some kind of tree.
“The problem is that when you look up close at the anatomy, it’s evocative of a lot of different things, but it’s diagnostic of nothing,” says Boyce, an associate professor in geophysical sciences and the Committee on Evolutionary Biology.
“And it’s so damn big that when whenever someone says it’s something, everyone else’s hackles get up: ‘How could you have a lichen 20 feet tall?’”


------- Prompt End -------
```

```
**outpu**t = llm(final_prompt)
print (output)
```

```
People argued for a long time about what Prototaxites was. Some thought it was a lichen, some thought it was a fungus, and some thought it was a tree. But it was hard to tell for sure because it looked like different things up close and it was really, really big.
```

### **2，长文本总结**

对于文本长度较短的文本我们可以直接这样执行summary操作

但是对于文本长度超过lLM支持的max token size 时将会遇到困难

Lang Chain 提供了开箱即用的工具解决长文本的问题：load_summarize_chain

```
**# Su****mmarie****s Of**** Longe****r Te****xt

fr**om langchain.llms import OpenAI
from langchain.chains.summarize import load_summarize_chain
from langchain.text_splitter import RecursiveCharacterTextSplitter

llm = OpenAI(temperature=0, openai_api_key=openai_api_key)
```

```
**with** open(&#x27;wonder**la**nd.txt&#x27;, &#x27;r&#x27;) as file:
    t**ext =** file.read() # 文章本身是爱丽丝梦游仙境

# 打印小说的前285个字符
print (text[:285])
```

```
The Project Gutenberg eBook of Alice’s Adventures in Wonderland, by Lewis Carroll

This eBook is for the use of anyone anywhere in the United States and
most other parts of the world at no cost and with almost no restrictions
whatsoever. You may copy it, give it away or re-use it unde
```

```
**num_t**okens = llm.get_num_tokens(text)

print (f&quot;There are {num_tokens} tokens in your file&quot;) 
# 全文一共4w8词
# 很明显这样的文本量是无法直接送进LLM进行处理和生成的
```

There are 48613 tokens in your file

解决长文本的方式无非是'chunking','splitting' 原文本为小的段落/分割部分

```
text_splitter = RecursiveCharacterT**extSp**litter(separators=[&quot;\n\n&quot;, &quot;\n&quot;], chunk_size=5000, chunk_overlap=350)
# 虽然我使用的是 RecursiveCharacterTextSplitter，但是你也可以使用其他工具
docs = text_splitter.create_documents([text])

print (f&quot;You now have {len(docs)} docs intead of 1 piece of text&quot;)
```

You now have 36 docs intead of 1 piece of text

现在就需要一个 Lang Chain 工具，将分段文本送入LLM进行summary

```
# 设置 lang chain
# 使用 map_reduce的chain_type，这样可以将多个文档合并成一个
chain = load_summarize_chain(llm=llm, chain_type=&#x27;map_reduce&#x27;) # verbose=True 展示运行日志
```

```
# Use it. This will run through the 36 documents**, sum**marize the chunks, then get a summary of the summary.
# 典型的map reduce的思路去解决问题，将文章拆分成多个部分，再将多个部分分别进行 summarize，最后再进行 合并，对 summarys 进行 summary
output = chain.run(docs)
print (output)
# Try yourself
```

```
 Alice follows a white rabbit down a rabbit hole and finds herself in a strange world full of peculiar characters. She experiences many strange adventures and is asked to settle disputes between the characters. In the end, she is in a court of justice with the King and Queen of Hearts and is questioned by the King. Alice reads a set of verses and has a dream in which she remembers a secret. Project Gutenberg is a library of electronic works founded by Professor Michael S. Hart and run by volunteers.
```

## 二，文档问答(QA based Documents)

为了确保LLM能够执行QA任务

1.  需要向LLM传递能够让他参考的上下文信息

1.  需要向LLM准确地传达我们的问题

### **1，短文本问答**

```
# 概括来说，使用文档作为上下文进行QA系统的构建过程类似于 llm(you**r co****ntext **+ your question) = your answer
# Simple Q&amp;A Example

from langchain.llms import OpenAI

llm = OpenAI(temperature=0, openai_api_key=openai_api_key)
```

```
context = &quot;&quot;&quot;
Rachel is 30 years old
Bob is 45 years old
Kevin is 65 years old
&quot;&quot;&quot;

question = &quot;Who is under 40 years old?&quot;
```

```
**outpu**t = llm(context + question)

print (output.strip())
```

Rachel is under 40 years old.

### **2，长文本问答**

对于更长的文本，可以文本进行分块，对分块的内容进行 embedding，将 embedding 存储到数据库中，然后进行查询。

目标是选择相关的文本块，但是我们应该选择哪些文本块呢？目前最流行的方法是基于比较向量嵌入来选择相似的文本。

```
**from** langchain **import**** Ope****nAI
fr****om l****angcha****in.v****ectors****tore****s impo**rt FAISS
from langchain.chains import RetrievalQA
from langchain.document_loaders import TextLoader
from langchain.embeddings.openai import OpenAIEmbeddings
llm = OpenAI(temperature=0, openai_api_key=openai_api_key)
```

```
lo**ader **= **TextL**oader(&#x27;wonderland.txt&#x27;) # 载入一个长文本，我们还是使用爱丽丝漫游仙境这篇小说作为输入
doc = loader.load()
print (f&quot;You have {len(doc)} document&quot;)
print (f&quot;You have {len(doc[0].page_content)} characters in that document&quot;)
```

You have 1 document

You have 164014 characters in that document

```
# 将小说分割成多个部分
text_splitter = RecursiveCharacterTextSplitter(chunk_size=3000, chunk_overlap=400)
docs = text_splitter.split_documents(doc)
```

```
# 获取字符的总数，以便可以计算平均值
num_tota**l_c**har**ac****ters **= sum([len(x.page_content) for x in docs])

print (f&quot;Now you have {len(docs)} documents that have an average of {num_total_characters / len(docs):,.0f} characters (smaller pieces)&quot;)
```

```
Now you have 62 documents that have an average of 2,846 characters (smaller pieces)
```

```
# 设置 embedding 引擎
embeddings = OpenAIEmbeddings(openai_api_key=openai_api_key)

# Embed 文档，然后使用伪数据库将文档和原始文本结合起来
# 这一步会向 OpenAI 发起 API 请求
docsearch = FAISS.from_documents(docs, embeddings)
```

```
# 创建QA-retrieval chain
qa = RetrievalQA.from_chain_type(llm=llm, chain_type=&quot;stuff&quot;, retriever=docsearch.as_retriever())
```

```
query = &quot;What does the author describe the Alice following with?&quot;
qa.run(query)
# 这个过程中，检索器会去获取类似的文件部分，并结合你的问题让 LLM 进行推理，最后得到答案
# 这一步还有很多可以细究的步骤，比如如何选择最佳的分割大小，如何选择最佳的 embedding 引擎，如何选择最佳的检索器等等
# 同时也可以选择云端向量存储
```

```
&#x27; The author describes Alice following a White Rabbit with pink eyes.&#x27;
```

## 三，信息抽取(Extraction)

Extraction是从一段文本中解析结构化数据的过程.

通常与Extraction parser一起使用，以构建数据，以下是一些使用范例。

1.  从句子中提取结构化行以插入数据库

1.  从长文档中提取多行以插入数据库

1.  从用户查询中提取参数以进行 API 调用

1.  最近最火的 Extraction 库是 KOR

### **1，手动格式转换**

```
**from**** langc****hain****.schem****a im****port H**umanMessage
from langchain.prompts import PromptTemplate, ChatPromptTemplate, HumanMessagePromptTemplate

from langchain.chat_models import ChatOpenAI


chat_model = ChatOpenAI(temperature=0, model=&#x27;gpt-3.5-turbo&#x27;, openai_api_key=openai_api_key)
```

```
# Vanilla Extraction
instructions = &quot;&quot;&quot;
You will be given a sentence with fruit names, extract those fruit names and assign an emoji to them
Return the fruit name and emojis in a python dictionary
&quot;&quot;&quot;

fruit_names = &quot;&quot;&quot;
Apple, Pear, this is an kiwi
&quot;&quot;&quot;
```

```
# Make your pro**mpt w****hich **combines the instructions w/ the fruit names
prompt = (instructions + fruit_names)

# Call the LLM
output = chat_model([HumanMessage(content=prompt)])

print (output.content)
print (type(output.content))
```

```
{&#x27;Apple&#x27;: &#x27;🍎&#x27;, &#x27;Pear&#x27;: &#x27;🍐&#x27;, &#x27;kiwi&#x27;: &#x27;🥝&#x27;}
&lt;class &#x27;str&#x27;&gt;
```

```
**outpu****t_dic**t = eval(output.content) #利用python中的eval函数手动转换格式

print (output_dict)
print (type(output_dict))
```

### **2，自动格式转换**

使用langchain.output_parsers.StructuredOutputParser可以自动生成一个带有格式说明的提示。

这样就不需要担心提示工程输出格式的问题了，将这部分完全交给 Lang Chain 来执行，将LLM的输出转化为 python 对象。

```
# 解析输出并获取结构化的数据**
fro****m lang**chain.output_parsers import StructuredOutputParser, ResponseSchema

response_schemas = [
    ResponseSchema(name=&quot;artist&quot;, description=&quot;The name of the musical artist&quot;),
    ResponseSchema(name=&quot;song&quot;, description=&quot;The name of the song that the artist plays&quot;)
]

# 解析器将会把LLM的输出使用我定义的schema进行解析并返回期待的结构数据给我
output_parser = StructuredOutputParser.from_response_schemas(response_schemas)

```

```
format_instructions = output_parser.get_format_instructions()
print(format_instructions)
```

```
The output should be a markdown code snippet formatted in the following schema, including the leading and trailing &quot;\`\`\`json&quot; and &quot;\`\`\`&quot;:

```json
{
        &quot;artist&quot;: string  // The name of the musical artist
        &quot;song&quot;: string  // The name of the song that the artist plays
}
```
```

```
# 这个 Prompt 与之前我们构建 Chat Model 时 Prompt 不同
# 这个 Prompt 是一个 ChatPromptTemplate，它会自动将我们的输出转化为 python 对象
prompt = ChatPromptTemplate(
    messages=[
        HumanMessagePromptTemplate.from_template(&quot;Given a command from the user, extract the artist and song names \n \
                                                    {format_instructions}\n{user_prompt}&quot;)  
    ],
    input_variables=[&quot;user_prompt&quot;],
    partial_variables={&quot;format_instructions&quot;: format_instructions}
)
```

```
artist_query = prompt.format_prompt(user_prompt=&quot;I really like So Young by Portugal. The Man&quot;)
print(artist_query.messages[0].content)
```

```
Given a command from the user, extract the artist and song names 
                                                     The output should be a markdown code snippet formatted in the following schema, including the leading and trailing &quot;\`\`\`json&quot; and &quot;\`\`\`&quot;:

```json
{
        &quot;artist&quot;: string  // The name of the musical artist
        &quot;song&quot;: string  // The name of the song that the artist plays
}
```
I really like So Young by Portugal. The Man
```

```
**artis****t_out**put = chat_model(artist_query.to_messages())
output = output_parser.parse(artist_output.content)

print (output)
print (type(output))
# 这里要注意的是，因为我们使用的 turbo 模型，生成的结果并不一定是每次都一致的
# 替换成gpt4模型可能是更好的选择
```

```
{&#x27;artist&#x27;: &#x27;Portugal. The Man&#x27;, &#x27;song&#x27;: &#x27;So Young&#x27;}
&lt;class &#x27;dict&#x27;&gt;
```

## 四，结果评估(Evaluation)

由于自然语言的不可预测性和可变性，评估LLM的输出是否正确有些困难，langchain 提供了一种方式帮助我们去解决这一难题。

```
**# Em****beddin****gs, ****store,**** and**** retri****eval**
from langc**hain.e****mbed****dings.**openai** imp****ort Op**enAIEmbeddings
from langchain.vectorstores import FAISS
from langchain.chains import RetrievalQA

# Model and doc loader
from langchain import OpenAI
from langchain.document_loaders import TextLoader

# Eval
from langchain.evaluation.qa import QAEvalChain

llm = OpenAI(temperature=0, openai_api_key=openai_api_key)
```

```
**# 还是使**用爱**丽丝漫游仙**境作为文本输入
loader = TextLoader(&#x27;wonderland.txt&#x27;)
doc = loader.load()

print (f&quot;You have {len(doc)} document&quot;)
print (f&quot;You have {len(doc[0].page_content)} characters in that document&quot;)
```

```
You have 1 document
You have 164014 characters in that document
```

```
text_splitter = RecursiveCharacterTex**tSp**lit**te****r(chu**nk_size=3000, chunk_overlap=400)
docs = text_splitter.split_documents(doc)

# Get the total number of characters so we can see the average later
num_total_characters = sum([len(x.page_content) for x in docs])

print (f&quot;Now you have {len(docs)} documents that have an average of {num_total_characters / len(docs):,.0f} characters (smaller pieces)&quot;)
```

```
Now you have 62 documents that have an average of 2,846 characters (smaller pieces)
```

```
# Embeddings and docstore
embeddings = OpenAIEmbeddings(openai_api_key=openai_api_key)
docsearch = FAISS.from_documents(docs, embeddings)
```

```
chain = RetrievalQA.from_chain_type(llm=llm, chain_type=&quot;stuff&quot;, retriever=docsearch.as_retriever(), input_key=&quot;question&quot;)
# 注意这里的 input_key 参数，这个参数告诉了 chain 我的问题在字典中的哪个 key 里
# 这样 chain 就会自动去找到问题并将其传递给 LLM
```

```
question_answers = [
    {&#x27;question&#x27; : &quot;Which animal give alice a instruction?&quot;, &#x27;answer&#x27; : &#x27;rabbit&#x27;},
    {&#x27;question&#x27; : &quot;What is the author of the book&quot;, &#x27;answer&#x27; : &#x27;Elon Mask&#x27;}
]
```

```
predictions = chain.apply(question_answers)
predictions
# 使用LLM模型进行预测，并将答案与我提供的答案进行比较，这里信任我自己提供的人工答案是正确的
```

```
[{&#x27;question&#x27;: &#x27;Which animal give alice a instruction?&#x27;,
  &#x27;answer&#x27;: &#x27;rabbit&#x27;,
  &#x27;result&#x27;: &#x27; The Caterpillar gave Alice instructions.&#x27;},
 {&#x27;question&#x27;: &#x27;What is the author of the book&#x27;,
  &#x27;answer&#x27;: &#x27;Elon Mask&#x27;,
  &#x27;result&#x27;: &#x27; The author of the book is Lewis Carroll.&#x27;}]
```

```
# Start your eval chain
eval_chain = QAEvalChain.from_llm(llm)

graded_outputs = eval_chain.evaluate(question_answers,
                                     predictions,
                                     question_key=&quot;question&quot;,
                                     prediction_key=&quot;result&quot;,
                                     answer_key=&#x27;answer&#x27;)
```

```
graded_outputs
```

```
[{&#x27;text&#x27;: &#x27; INCORRECT&#x27;}, {&#x27;text&#x27;: &#x27; INCORRECT&#x27;}]
```

## 五，数据库问答(Querying Tabular Data)

```
# 使用自然语言查询一个 SQLit**e 数据**库，我们将使用旧金山树**木数据集
#** Don&#x27;t run following code if you don&#x27;t run sqlite and follow db
from langchain import OpenAI, SQLDatabase, SQLDatabaseChain

llm = OpenAI(temperature=0, openai_api_key=openai_api_key)
```

```
sqlite_db_path = &#x27;data/San_Francisco_Trees.db&#x27;
db = SQLDatabase.from_uri(f&quot;sqlite:///{sqlite_db_path}&quot;)
```

```
db_chain = SQLDatabaseChain(llm=llm, database=db, verbose=True)
```

```
db_chain.run(&quot;How many Species of trees are there in San Francisco?&quot;)
```

1.  Find which table to use

1.  Find which column to use

1.  Construct the correct sql query

1.  Execute that query

1.  Get the result

1.  Return a natural language reponse back

confirm LLM result via pandas

```
**import**** sqlit**e3
impor**t **pandas as pd

# Connect to the SQLite database
connection = sqlite3.connect(sqlite_db_path)

# Define your SQL query
query = &quot;SELECT count(distinct qSpecies) FROM SFTrees&quot;

# Read the SQL query into a Pandas DataFrame
df = pd.read_sql_query(query, connection)

# Close the connection
connection.close()
```

```
# Display the result in the first column first cell
print(df.iloc[0,0])
```

## 六，代码理解(Code Understanding)

代码理解用到的工具和文档问答差不多，不过我们的输入是一个项目的代码。

```
**# Help****er t****o read**** loc****al fil****es
i****mport ****os

****# Vect****or S****upport**
from langchain.vectorstores import FAISS
from langchain.embeddings.openai import OpenAIEmbeddings

# Model and chain
from langchain.chat_models import ChatOpenAI

# Text splitters
from langchain.text_splitter import CharacterTextSplitter
from langchain.document_loaders import TextLoader

llm = ChatOpenAI(model=&#x27;gpt-3.5-turbo&#x27;, openai_api_key=openai_api_key)
```

```
embeddings = OpenAIEmbeddings(disallowed_special=(), openai_api_key=openai_api_key)
```

```
root_dir = &#x27;/content/drive/MyD**riv****e/**thefuzz-**mas**ter&#x27;
d**oc**s = []

**# G**o through each folder
for dirpath, dirnames, **filena**mes in os.w**al**k(root_dir):**
   ** 
    # Go through each file
    for file in filenames:
        try: 
            # Load up the file as a doc and split
            loader = TextLoader(os.path.join(dirpath, file), encoding=&#x27;utf-8&#x27;)
            docs.extend(loader.load_and_split())
        except Exception as e: 
            pass
```

```
**print** (**f&quot;You** h**ave {**len(docs)} documents\n&quot;)
print (&quot;------ Start Document ------&quot;)
print (docs[0].page_content[:300])

```

```
You have 175 documents

------ Start Document ------
from timeit import timeit
import math
import csv

iterations = 100000


reader = csv.DictReader(open(&#x27;data/titledata.csv&#x27;), delimiter=&#x27;|&#x27;)
titles = [i[&#x27;custom_title&#x27;] for i in reader]
title_blob = &#x27;\n&#x27;.join(titles)


cirque_strings = [
    &quot;cirque du soleil - zarkana - las vegas&quot;,
    &quot;cirque du sol
```

```
docsearch = FAISS.from_documents(docs, embeddings)
```

```
# Get our retriever ready
qa = RetrievalQA.from_chain_type(llm=llm, chain_type=&quot;stuff&quot;, retriever=docsearch.as_retriever())
```

```
query = &quot;What function do I use if I want to find the most similar item in a list of items?&quot;
output = qa.run(query)
```

```
**print** (output)
```

```
You can use the `process.extractOne()` function from `thefuzz` package to find the most similar item in a list of items. For example:

```
from thefuzz import process

choices = [&quot;New York Yankees&quot;, &quot;Boston Red Sox&quot;, &quot;Chicago Cubs&quot;, &quot;Los Angeles Dodgers&quot;]
query = &quot;new york mets vs atlanta braves&quot;

best_match = process.extractOne(query, choices)
print(best_match)
```

This will output:

```
(&#x27;New York Yankees&#x27;, 50)
```

Where `(&#x27;New York Yankees&#x27;, 50)` means that the closest match found was &quot;New York Yankees&quot; with a score of 50 (out of 100).
```

```
query = &quot;Can you write the code to use the process.extractOne() function? Only respond with code. No other text or explanation&quot;
output = qa.run(query)
print(output)
```

```
process.extractOne(query, choices)
```

## 七，API交互(Interacting with APIs)

如果你需要的数据或操作在 API 之后，就需要LLM能够和API进行交互。

到这个环节，就与 Agents 和 Plugins 息息相关了。

Demo可能很简单，但是功能可以很复杂。

```
**from**** langc****hain****.chain**s import APIChain
from langchain.llms import OpenAI

llm = OpenAI(temperature=0, openai_api_key=openai_api_key)
```

```
api_docs = &quot;&quot;&quot;

BASE URL: https://restcountries.com/

API Documentation:

The API endpoint /v3.1/name/{name} Used to find informatin about a country. All URL parameters are listed below:
    - name: Name of country - Ex: italy, france
    
The API endpoint /v3.1/currency/{currency} Uesd to find information about a region. All URL parameters are listed below:
    - currency: 3 letter currency. Example: USD, COP
    
Woo! This is my documentation
&quot;&quot;&quot;

chain_new = APIChain.from_llm_and_api_docs(llm, api_docs, verbose=True)
```

```
chain_new.run(&#x27;Can you tell me information about france?&#x27;)
```

![](img/08c34bd824b471da21e8b27938232943.png)

```
&#x27; France is an officially-assigned, independent country located in Western Europe. Its capital is Paris and its official language is French. Its currency is the Euro (€). It has a population of 67,391,582 and its borders are with Andorra, Belgium, Germany, Italy, Luxembourg, Monaco, Spain, and Switzerland.&#x27;
```

```
chain_new.run(&#x27;Can you tell me about the currency COP?&#x27;)
```

![](img/c5c8aac2c2567c016bad24de7b0e1420.png)

```
&#x27; The currency of Colombia is the Colombian peso (COP), symbolized by the &quot;$&quot; sign.&#x27;
```

## 八，聊天机器人(Chatbots)

聊天机器人使用了之前提及过的很多工具，且最重要的是增加了一个重要的工具：记忆力。

与用户进行实时交互，为用户提供自然语言问题的平易近人的 UI，

```
**from**** langc****hain**.llms impor**t Open****AI
f****rom la****ngch****ain im**port LLMChain
from langchain.prompts.prompt import PromptTemplate

# Chat specific components
from langchain.memory import ConversationBufferMemory
```

```
template = &quot;&quot;&quot;
You are a chatbot that is unhelpful.
Your goal is to not help the user but only make jokes.
Take what the user is saying and make a joke out of it

{chat_history}
Human: {human_input}
Chatbot:&quot;&quot;&quot;

prompt = PromptTemplate(
    input_variables=[&quot;chat_history&quot;, &quot;human_input&quot;], 
    template=template
)
memory = ConversationBufferMemory(memory_key=&quot;chat_history&quot;)
```

```
llm_chain = LLMChain(
    llm=OpenAI(openai_api_key=openai_api_key), 
    prompt=prompt, 
    verbose=True, 
    memory=memory
)
```

```

llm_chain.predict(human_input=&quot;Is an pear a fruit or vegetable?&quot;)
```

![](img/264687d13a4e49ae55def17c174ae40f.png)

```
&#x27; An pear is a fruit, but a vegetable-pear is a pun-ishable offense!&#x27;
```

```
llm_chain.predict(human_input=&quot;What was one of the fruits I first asked you about?&quot;)
# 这里第二个问题的答案是来自于第一个答案本身的，因此我们使用到了 memory
```

![](img/8b6e3fe0158a2d6540fb106776b2b9d8.png)

```
&quot; An pear - but don&#x27;t let it get to your core!&quot;
```

## **九，智能体(Agents) **

Agents是 LLM 中最热门的 🔥 主题之一。

Agents可以查看数据、推断下一步应该采取什么行动，并通过工具为您执行该行动, 是一个具备AI智能的决策者。

温馨提示：小心使用 Auto GPT, 会迅速消耗掉你大量的token。

```
# Helpers**
impor****t os
i****mpor****t json**

from langchai**n.ll****ms imp****ort ****OpenAI**

# Agent impo**rts
****from l****angc****hain.a****gent****s impo**rt load_tools
from langchain.agents import initialize_agent

# Tool imports
from langchain.agents import Tool
from langchain.utilities import GoogleSearchAPIWrapper
from langchain.utilities import TextRequestsWrapper
```

```
os.environ[&quot;GOOGLE_CSE_ID&quot;] = &quot;YOUR_GOOGLE_CSE_ID&quot;
os.environ[&quot;GOOGLE_API_KEY&quot;] = &quot;YOUR_GOOGLE_API_KEY&quot;
```

```
llm = OpenAI(temperature=0, openai_api_key=openai_api_key)
```

```
search = GoogleSearchAPIWrapper()

requests = TextRequestsWrapper()
```

```
toolkit = [
    Tool(
        name = &quot;Search&quot;,
        func=search.run,
        description=&quot;useful for when you need to search google to answer questions about current events&quot;
    ),
    Tool(
        name = &quot;Requests&quot;,
        func=requests.get,
        description=&quot;Useful for when you to make a request to a URL&quot;
    ),
]
```

```
agent = initialize_agent(toolkit, llm, agent=&quot;zero-shot-react-description&quot;, verbose=True, return_intermediate_steps=True)
```

```
response = agent({&quot;input&quot;:&quot;What is the capital of canada?&quot;})
response[&#x27;output&#x27;]
```

![](img/035cc30458618f688ce9b663074e7375.png)

```
&#x27;Ottawa is the capital of Canada.&#x27;
```

```
response = agent({&quot;input&quot;:&quot;Tell me what the comments are about on this webpage https://news.ycombinator.com/item?id=34425779&quot;})
response[&#x27;output&#x27;]
```

![](img/2e6fbdd38d3cf64a6f7b9c88f898e9ba.png)

```
&#x27;The comments on the webpage are about the history of Y Combinator.&#x27;
```

**感谢明训同学供稿！ **

**公众号算法美食屋后台**回复关键词： langchain ，获取本文notebook源代码。