
```
completion = openai.Completion.create(engine=deployment_name, prompt=new_prompt, max_tokens=1200)
```

运行这段代码后，我们得到了以下输出：

```
No of recipes (for example, 5): 2
List of ingredients (for example, chicken, potatoes, and carrots): apple,flour
Filter (for example, vegetarian, vegan, or gluten-free): sugar
-Apple and flour pancakes: 1 cup flour, 1/2 tsp baking powder, 1/2 tsp baking soda, 1/4 tsp salt, 1 tbsp sugar, 1 egg, 1 cup buttermilk or sour milk, 1/4 cup melted butter, 1 Granny Smith apple, peeled and grated
-Apple fritters: 1-1/2 cups flour, 1 tsp baking powder, 1/4 tsp salt, 1/4 tsp baking soda, 1/4 tsp nutmeg, 1/4 tsp cinnamon, 1/4 tsp allspice, 1/4 cup sugar, 1/4 cup vegetable shortening, 1/4 cup milk, 1 egg, 2 cups shredded, peeled apples
Shopping list:
-Flour, baking powder, baking soda, salt, sugar, egg, buttermilk, butter, apple, nutmeg, cinnamon, allspice
```

## 改进你的设置

目前为止，我们的代码是可用的，但还有一些调整可以进一步优化。我们应该做的一些事情包括：

- `将密钥与代码分离`，比如 API 密钥。密钥不应该写在代码里，应该存放在安全的位置。为了将密钥与代码分离，我们可以使用环境变量，并借助像 `python-dotenv` 这样的库从文件中加载它们。代码示例如下：

1. 创建一个 `.env` 文件，内容如下：

```bash
OPENAI_API_KEY=sk-...
```

> 注意，对于 Azure，你需要设置以下环境变量：

```bash
OPENAI_API_TYPE=azure
OPENAI_API_VERSION=2023-05-15
OPENAI_API_BASE=<replace>
```

在代码中，你可以这样加载环境变量：

```python
from dotenv import load_dotenv
load_dotenv()

openai.api_key = os.environ["OPENAI_API_KEY"]
```

- `关于令牌长度的说明`。我们需要考虑生成文本所需的令牌数量。令牌是有成本的，所以尽可能节省令牌的使用。例如，我们能否调整提示语，使得使用更少的令牌？

要更改使用的令牌数，可以使用 `max_tokens` 参数。例如，如果你想使用100个令牌，可以这样写：

```python
completion = client.chat.completions.create(model=deployment, messages=messages, max_tokens=100)
```

- 尝试调整 `temperature` 参数。`temperature` 是我们之前没提到但对程序表现很重要的参数。`temperature` 值越高，输出越随机；值越低，输出越可预测。你可以根据是否需要输出的多样性来决定。

- 要调整 `temperature`，可以使用 `temperature` 参数。例如，如果你想设置 `temperature` 为0.5，可以这样写：

```python
completion = client.chat.completions.create(model=deployment, messages=messages, temperature=0.5)
```

注意，越接近1.0，输出越多样化。

## 任务

这次任务你可以自由选择要做什么。

以下是一些建议：

- 调整配方生成器应用，进一步改进它。尝试不同的 `temperature` 值和提示语，看看能得到什么效果。

- 制作一个“学习伙伴”应用。这个应用可以回答某个主题的问题，比如 Python，你可以设计提示语如“Python 中某个主题是什么？”，或者“给我展示某个主题的代码”等。

- 历史机器人，让历史变得生动。让机器人扮演某个历史人物，向它提问关于其生平和时代的问题。

## 解决方案

### 学习伙伴

下面是一个起始提示，看看你如何使用并调整它以符合你的需求。

```
- "You're an expert on the Python language

    Suggest a beginner lesson for Python in the following format:

    Format:

    - concepts:

    - brief explanation of the lesson:

    - exercise in code with solutions";
```

### 历史机器人

以下是一些你可以使用的提示语：

```
- "You are Abe Lincoln, tell me about yourself in 3 sentences, and respond using grammar and words like Abe would have used";

- "You are Abe Lincoln, respond using grammar and words like Abe would have used:

   Tell me about your greatest accomplishments, in 300 words";
```

## 知识检测

`temperature` 参数的作用是什么？

1. 它控制输出的随机程度。

1. 它控制响应的长度。

1. 它控制使用的令牌数量。

## 🚀 挑战

在完成任务时，尝试调整 `temperature`，分别设置为0、0.5和1。记住，0表示最不多样，1表示最多样。哪个值最适合你的应用？

## 干得好！继续学习

完成本课后，查看我们的[生成式 AI 学习合集](https://aka.ms/genai-collection?WT.mc_id=academic-105485-koreyst)，继续提升你的生成式 AI 知识！

[接下来进入第 7 课](https://github.com/microsoft/generative-ai-for-beginners/blob/main/translations/zh/07-building-chat-applications/README.md?WT.mc_id=academic-105485-koreyst)，我们将学习如何构建聊天应用！
