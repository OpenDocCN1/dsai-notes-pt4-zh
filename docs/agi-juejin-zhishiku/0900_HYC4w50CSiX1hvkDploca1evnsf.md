
# LLM应用：利用子知识结构实现多级RAG

> 来源：[https://agijuejin.feishu.cn/docx/LH5HdUFM2oTgAfx1F7JcTeOUnEh](https://agijuejin.feishu.cn/docx/LH5HdUFM2oTgAfx1F7JcTeOUnEh)

> 作者：[hadiii](https://www.zhihu.com/people/hadiiii)

> 原文：https://zhuanlan.zhihu.com/p/671429602

> 本文是一个个人探索笔记，本来是近期项目LLM+解密游戏里面的RAG部分。后来抽象了一下其中的想法，概括为利用子知识结构实现多级RAG的功能，似乎勉强称得上有一些通用思路。笔者才学疏浅，本文不使用向量数据库，也没有实际落地，只是在此做一个Toy project的思路和实践记录。

我是 [@hadiii](https://www.zhihu.com/people/c97e9fb7ac40f4bc62f79de0ab745739)，一名爱打游戏的普通人类，AI Agent爱好者。日常更新游戏和AI的相关内容，也想和大家讨论相关话题，和大家一起进步！欢迎点赞收藏关注~

## 动机

在传统的检索式问答系统中，RAG通过对文本的切分来检索相关信息，但这种方法常常导致抽取的信息片段缺乏对其所属知识体系的宏观认识，因为只有n个被切分的chunk被导入了上下文。这一局限性在处理需要对大量知识有全面理解的复杂查询时尤为明显。此外，若对整个知识库进行检索，往往会导致大量的计算资源被无效消耗在不相关的信息上。

为了改善这些问题，本文尝试通过引入多级RAG架构来优化知识检索过程。通过将知识库分层，首先在较高层次上对用户的查询进行理解和定位，然后再在更具体的层次上检索细节信息，这样的分层检索策略不仅可以减少无效的资源消耗，还能提供更准确、更具上下文相关性的回答。

## 实现

> 整个项目都不使用LangChain（也不是很看好LangChain），也不使用向量数据库，只是把其思路用原生JSON格式实现。Embedding和生成模型都是基于OpenAI的API。下面介绍的两个成员函数都是来自于一个自定义的LLM based `agent`类，下文摘取了和本文相关的部分。

![](img/63b9a083579ad8c758f534f6e9d1bea4.png)

### 添加顶层知识到`agent`类的知识库：

```
def add_knowledge_from_text(self, text, sub_knowledge_file_path=None):
  """
  根据提供的文本添加一个知识对象到知识库中，并记录日志。可选地，可以指定一个子知识的引用。

  参数:
  text (str): 要添加到知识库的知识文本。
  sub_knowledge (str, optional): 子知识的引用路径或标识符，默认为None。

  返回:
  无
  """
  if text:
      embedding_list = apis.request_embedding(text)
      knowledge = {
          "text": text,
          "embedding": embedding_list[0],
          "sub_knowledge": sub_knowledge_file_path
      }
      self.basic_knowledge.append(knowledge)
      logger.info(f"添加了知识：{text}")
  else:
      logger.info(f"要添加的知识为空")
```

### 添加有子知识引用的顶层知识到`agent`类的知识库：

```
def add_knowledge_from_sub_knowledge_list(self, summary_text, sub_knowledge_list):
  """
  为具有子知识列表的知识总结创建一个JSON文件来存储子知识，并将知识总结添加到知识库中。

  参数:
  summary_text (str): 知识总结的文本。
  sub_knowledge_list (list): 子知识的列表，每个子知识通常包含文本描述等信息。

  返回:
  子知识文件的路径。异常时返回None。
  """
  # 检查输入参数
  if not summary_text or not isinstance(summary_text, str):
      logger.error("summary_text 必须是非空字符串。")
      return None
  if not isinstance(sub_knowledge_list, list):
      logger.error("sub_knowledge_list 必须是列表。")
      return None

  sub_knowledge_file = f"../resource/knowledge/{hash(summary_text)}_sub_knowledge.json"
  sub_knowledge_content = {
      "summary_text": summary_text,
      "sub_knowledge_list": sub_knowledge_list
  }

  try:
      with open(sub_knowledge_file, 'w', encoding="utf-8") as file:
          json.dump(sub_knowledge_content, file, ensure_ascii=False, indent=4)
  except IOError as e:
      logger.error(f"写入子知识文件时发生错误: {e}")
      return None

  logger.info(f"为知识总结：{summary_text} 添加了子知识文件：{sub_knowledge_file}")
  self.add_knowledge_from_text(summary_text, sub_knowledge_file)
  return sub_knowledge_file
```

> 还有其他的子知识管理功能实现，此处不做展开。

> 例如：要么是直接管理子知识，要么是删除顶层知识时递归删除其子知识之类的方法。

![](img/660f070af8eaf6c6f4b24a1e3286bde8.png)

## 实验

> 验证在个人的Gradio demo上进行。

### 注入有子知识的顶层知识：

这一步大概是丢进去一个文档，或者PDF什么的，反正能转成文字的，和直接RAG没有区别。这里用“宵宫”的角色文案做示范。丢进去的时候会实现文本切分，切分的文本作为子知识列表准备被导入。切分的实现可见下文：

[LLM知识库导入的细节：](https://zhuanlan.zhihu.com/p/671295504)[平滑](https://zhuanlan.zhihu.com/p/671295504)的文本分割26 赞同 · 4 评论文章

然后需要手动写一个总结性的知识文本作为其顶层知识，随后链接到子知识文件。当然，这个总结文本生成也可以用GPT跑一遍总结知识的Prompt模板来实现。

> 手动管理在游戏世界观场景更有用。

![](img/f6f540819b920f42eaadc613144b6685.png)

### 对话，使用最后一段`chunk`的文本作为`query`来触发RAG：

实验中的对话系统可以参考下文，主要使用了知识库和记忆流检索，以及基于CoT思路使用内心独白的部分：

[LLM风格化对话中的Pr](https://zhuanlan.zhihu.com/p/671966233)[om](https://zhuanlan.zhihu.com/p/671966233)pt工程16 赞同 · 3 评论文章

项目的对话系统实现了RAG功能，我们使用最后一个`chunk`的文本作为`query`，验证能不能检索到两层知识：

![](img/1c666a487996ba6c85b759beeee98306.png)

以下是`brain`类原始的log文件。可以看到`brain`模块进行了两次RAG，并且首先包含了“宵宫”的总体信息，然后又包括了最匹配用户`query`的子知识`chunk`内容。

如下所示：

> 2023-12-23 13:40:11,248 - `brain` - INFO - 找到了最相似的知识：

> 宵宫的名字在花见坂可谓是家喻户晓。她是稻妻技艺最精湛的烟花专家，被誉为“夏祭的女王”。继承了“长野原”衣钵的宵宫，能为烟花赋予变幻莫测的姿态，帯给人们无与伦比的体验。

> 2023-12-23 13:40:11,249 - `brain` - INFO - 查询到子知识路径，开始尝试查找最相似的子知识。

> 2023-12-23 13:40:11,252 - `brain` - INFO - 找到了最相似的子知识：

> 她所享受的是和別人交流的过程——即便是同样的事，不同的人也会有完全不同的看法。接管烟花店后，宵宫跟街坊的关系愈发融洽。不少老人视宵宫为忘年之交，常邀请宵宫来家中坐坐，聊起多年前的庆典盛况。已至中年的父母辈会拜托宵宫调解自己与孩子的关系，和宵宫唠唠柴米油盐的家常。青年们则与宵宫称兄道弟，自发帮宵宫搬运沉重的烟花原材料，希望宵宫能提前下班，早点带街上的孩子们出去玩。“我相信语言有它独特的力量，没有什么是说出来还解决不了的！”宵宫如是说。

## 结果分析

实验结果显示，RAG系统从顶层知识“宵宫的角色文案”中检索到了相关的总体信息，并能够进一步从子知识中精准地提取出与用户查询最匹配的细节信息。这证明了系统在处理具有层次结构的知识时的有效性，并展现了其在理解和响应用户需求方面的能力。

特别是在游戏环境中，能够让LLM首先获得整体世界观，再获得相对于查询最相似的信息，这对角色扮演类对话，或是LLM驱动角色很有帮助。其他应用中，这种思路也可能会改善`chunk`语义的孤立性问题。

## 总结

### 优势

1. 分层知识管理：通过顶层知识提供了相关话题的宏观框架，可以帮助LLM构建整体认识。每次RAG时会拿到多级的语义，既有宏观认识也有最匹配用户`query`的内容，或许能改善RAG效果。尤其是在游戏场景大量用到架空世界观的设定时，更需要宏观的相关认识。

2. 资源优化：因为引入了类似于分块检索的机制，可以减少在大型知识库中无目的搜索。以相似度计算为基准而言，可以节省计算资源。

### 展望

检索效率：未使用向量数据库可能导致处理数据时效率低下。不清楚这种思路能不能结合向量数据库，但在简单的项目中，或者独立游戏项目中，这种思路是有效的，其依赖的库很少。
