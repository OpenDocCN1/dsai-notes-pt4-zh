
# Agent实战-JSON结构化智能

> 来源：[https://agijuejin.feishu.cn/docx/SBAzda2aeoCnE9x5huccirvUn9g](https://agijuejin.feishu.cn/docx/SBAzda2aeoCnE9x5huccirvUn9g)

> 作者：AI小智

> 原文：`https://mp.weixin.qq.com/s/THXCYn23YLpZw2SY3Jh4Vw`

> ❝
> 本文[译自JSON agen ts with Ollama & LangChain](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=Mzg2ODA5NTM1OA==&action=getalbum&album_id=3115055723522015235&token=162966106&lang=zh_CN#wechat_redirect)一文，以电影推荐助手为实践案例，讲解了博主在工程实践中，如何基于LangChain框架和本地LLM优雅实现了Json结构化的智能体。系列合集，点击链接查看

随着AI应用工程的飞速发展，我们不难发现为大语言模型（LLMs）提供额外工具能大大增强其功能。

举例来说，GPT3.5版本通过集成Bing搜索和Python解释器实现了能力的跃迁。GPTs则直接将api调用作为工具进行了集成，LLM会决定是直接作出回应，还是先调用它提供的工具。这些工具不仅限于获取额外信息，它们还能发挥其他功能，比如帮用户订餐。

智能代理LLM与图数据库的交互示意图

![](img/4f50acd9ecc209d5e04ccaf0775cdd2e.png)

## 语义层的工具

LangChain文档中的示例（JSON代理，HuggingFace示例）使用单字符串输入的工具。但因为语义层的工具需要稍微复杂一些的输入，我需要进行一些深入研究。下面是推荐工具的示例输入：

```
all_genres = [
    "Action",
    "Adventure",
    "Animation",
    "Children",
    "Comedy",
    "Crime",
    "Documentary",
    "Drama",
    "Fantasy",
    "Film-Noir",
    "Horror",
    "IMAX",
    "Musical",
    "Mystery",
    "Romance",
    "Sci-Fi",
    "Thriller",
    "War",
    "Western",
]

class RecommenderInput(BaseModel):
     movie: Optional[str] = Field(description="用来推荐的电影")
     genre: Optional[str] = Field(
         description=("用于推荐的电影类型。可选项有：" f"{all_genres}")
     )
```

推荐工具有两个可选的输入项：电影和类型，并且我们为类型提供了一系列可选的值。虽然这些输入项并不特别复杂，但比单一字符串输入要高级一些，因此实现起来也略有不同。

## 基于JSON的LLM智能代理提示

在我的实现中，我深受现有的 `hwchase17/react-json` 提示的启发，这一提示可以在LangChain hub中找到。提示使用以下系统消息：

```
尽你所能回答下面的问题。你可以使用以下工具：

{tools}

你可以通过指定一个JSON块来使用工具。
具体而言，这个JSON应该包含一个`action`键（用来指定要使用的工具名称）和一个`action_input`键（工具的输入在这里）。

"action"键里的值应当仅为：{tool_names}

$JSON_BLOB应该只包含单一的动作，请不要返回一个列表包含多个动作。以下是一个有效$JSON_BLOB的示例：

{

   "action": $TOOL_NAME,

   "action_input": $INPUT

}

每次回答都要遵循以下格式：

Question: 你需要回答的问题
Thought: 你应该在思考要做什么
Action:

$JSON_BLOB

Observation: 动作的结果
...（这种思考/动作/观察的过程可以重复N次）
Thought: 我现在知道最终答案了
Final Answer: 对原本提问的最终回答

开始！请记住每次回答时都要精确使用`Final Answer`这个词。
```

提示的开始部分通过定义可用的工具来设定，后面我们将深入讨论。提示中最关键的部分是对LLM输出预期的指示。当LLM需要使用工具时，它应该使用以下JSON结构：

```
{
   "action": $TOOL_NAME,
   "action_input": $INPUT
}
```

这就是为什么它被称作基于JSON的代理：我们指导LLM在希望使用任何可用工具时生成一个JSON。然而，这只是输出定义的一小部分。完整的输出应遵循以下结构：

```
Thought: 你应该在思考要做什么
Action:

$JSON_BLOB

Observation: 动作的结果
...（这可以重复N次）
Final Answer: 对原本提问的最终回答
```

LLM在输出中总是需要解释它正在做什么，即`Thought`部分。当它想要使用任何可用的工具时，它应以JSON块的形式提供动作输入。`Observation`部分留给工具的输出，而当代理决定可以回答用户提出的问题时，它应使用`Final Answer`关键词。以下是电影智能代理使用此结构的一个实例。

![](img/5f6ee0b8fbd6de3bdb58b19215884fce.png)

在这个例子中，我们让代理推荐一部喜剧片。由于代理的一个可用工具是推荐工具，它决定利用推荐工具，并提供了用JSON写的输入语法。幸运的是，LangChain有一个内置的JSON智能代理输出解析器，我们无需操心其实现细节。然后，LLM从工具得到回应，并在提示语中作为观察结果使用。由于工具提供了所有必要的信息，LLM认为已经有了足够的信息来构建可以交给用户的最终答案。

我注意到对Mixtral的提示工程经常失败，它不总是只在需要工具时使用JSON语法。在我的测试中，当它不想使用任何工具时，有时它会使用如下的JSON动作输入：

```
{
  "action": Null,
  "action_input": ""
}
```

如果动作为`null`或类似的，LangChain的输出解析函数并不会忽视这个动作，而是会报错说没有定义`null`这个工具。我尝试对此进行提示修改，但没能一直做到。因此，我决定增加一个假设性的闲聊工具，以便用户想要进行闲聊时代理可以调用。

```
response = (
    "创建一个最终回答它们是否有任何关于电影或演员的问题"
)


class SmalltalkInput(BaseModel):
    query: Optional[str] = Field(description="用户提问")

class SmalltalkTool(BaseTool):
    name = "Smalltalk"
    description = "当用户打招呼或想要闲聊时适用"
    args_schema: Type[BaseModel] = SmalltalkInput

    def _run(
        self,
        query: Optional[str] = None,
        run_manager: Optional[CallbackManagerForToolRun] = None,
    ) -> str:
        """使用该工具。"""
        return response
```

如此，代理在用户打招呼时可以决定使用一个假的`Smalltalk`工具，我们再也不会因为解析`null`或者缺失工具名而遇到问题了。

![](img/f19bcad95f23c20a5fb1a833676c5350.png)

这样的临时弥补方法很管用，所以我选择留用它。像之前说的，大多数模型并未被训练以产生操作输入或者在不需要动作时生成文本，因此我们必须利用现有资源。至于操控模型以便它只在有必要时产生JSON动作输入，有时是成功的，有时则依赖情况而定。但像`smalltalk`工具这样给它提供一个备选项，可以避免出现异常。

## 在系统提示中定义工具输入

如前所述，我需要弄清楚如何定义略微复杂的工具输入，这样LLM才能正确解释它们。好笑的是，在我实现了一个自定义功能后，我找到了一个现成的LangChain功能，这个功能可以将自定义的Pydantic工具输入定义转换成Mixtral能识别的JSON对象。

```
from langchain.tools.render import render_text_description_and_args

tools = [RecommenderTool(), InformationTool(), Smalltalk()]

tool_input = render_text_description_and_args(tools)
print(tool_input)
```

它产生了以下的字符串描述：

```
"Recommender":"当你需要推荐一部电影时使用",
"args":{
   {
      "movie":{
         {
            "title":"Movie",
            "description":"用于推荐的电影",
            "type":"string"
         }
      },
      "genre":{
         {
            "title":"Genre",
            "description":"用于推荐的电影类型。可选项有：['Action', 'Adventure', 'Animation', 'Children', 'Comedy', 'Crime', 'Documentary', 'Drama', 'Fantasy', 'Film-Noir', 'Horror', 'IMAX', 'Musical', 'Mystery', 'Romance', 'Sci-Fi', 'Thriller', 'War', 'Western']",
            "type":"string"
         }
      }
   }
},
"Information":"当你需要回答关于各种演员或电影问题时使用",
"args":{
   {
      "entity":{
         {
            "title":"Entity",
            "description":"问题中提到的电影或人名",
            "type":"string"
         }
      },
      "entity_type":{
         {
            "title":"Entity Type",
            "description":"实体的类型。可选项为'movie'或'person'",
            "type":"string"
         }
      }
   }
},
"Smalltalk":"当用户打招呼或想要闲聊时使用",
"args":{
   {
      "query":{
         {
            "title":"Query",
            "description":"用户提问",
            "type":"string"
         }
      }
   }
}
```

我们只需将这些工具描述复制粘贴到系统提示中，Mixtral就能正确使用这些提前定义的工具，这非常方便。

## 结论

为实现这个基于JSON的智能代理，Harrison Chase和LangChain团队已经完成了大部分工作，我对此表示由衷的感谢。我只需要把碎片拼凑起来即可。正如所说，不要期待与GPT-4同等水平的性能。然而，我相信像Mixtral这样更强大的开源LLMs可以立即当做智能代理使用（比起GPT-4来可能需要更多的异常处理）。我期待未来会有更多开源LLMs被优化以作为智能代理使用。

## References

+   Langchain模板：`https://github.com/langchain-ai/langchain/tree/master/templates/neo4j-semantic-ollama?ref=blog.langchain.dev`

+   Jupyter笔记本版本：`https://github.com/tomasonjo/blogs/blob/master/llm/ollama_semantic_layer.ipynb?ref=blog.langchain.dev`
