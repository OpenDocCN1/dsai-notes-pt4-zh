
# 05.创建高级提示

> 来源：[https://gotoaiworld.feishu.cn/docx/UcRYdQOstokdZnx4WWPctCO6nUe](https://gotoaiworld.feishu.cn/docx/UcRYdQOstokdZnx4WWPctCO6nUe)

## 生成 Python Web API 的代码

以下是生成一个简单 Python Web API 的示例代码，使用 `Flask` 框架。

```
from flask import Flask, jsonify, request
app = Flask(name)

# 示例数据
items = [
{"id": 1, "name": "Item One"},
{"id": 2, "name": "Item Two"}
]

# 获取所有项目
@app.route('/items', methods=['GET'])
def get_items():
return jsonify(items)

# 根据 ID 获取单个项目
@app.route('/items/<int:item_id>', methods=['GET'])
def get_item(item_id):
item = next((item for item in items if item["id"] == item_id), None)
if item:
return jsonify(item)
else:
return jsonify({"error": "Item not found"}), 404

# 添加新项目
@app.route('/items', methods=['POST'])
def add_item():
new_item = request.get_json()
items.append(new_item)
return jsonify(new_item), 201

if name == '__main__':
app.run(debug=True)
```

此代码创建了一个简单的 API，支持获取所有项目、根据 ID 获取单个项目以及添加新项目。

```
#import necessary libraries
import flask
from flask import request, jsonify
#create an instance of the Flask class
app = flask.Flask(name)

#create an endpoint for the API
@app.route('/api/v1/endpoint', methods=['GET'])
def api_endpoint():
#get the request data
data = request.get_json()

#process the data
result = process_data(data)

#return the result
return jsonify(result)

#function to process the data
def process_data(data):
#process the data
result = {'result': 'success'}

#return the result
return result
#run the Flask app
if name == '__main__':
app.run()
```

再次运行提示，得到以下结果：

```
#import necessary libraries
import flask
from flask import request, jsonify
#create a Flask app
app = flask.Flask(name)

#create a route for the API
@app.route('/api', methods=['GET'])
def api_call():
#get the data from the request
data = request.get_json()

#process the data
result = process_data(data)

#return the result
return jsonify(result)

#define the process_data function
def process_data(data):
#do something with the data
result = data + 1
#return the result
return result
#run the app
if name == '__main__':
app.run()
```

这两个输出之间只有细微的差别。这次我们反过来，把 `temperature` 设置为 0.9：

```
# Import necessary libraries
import flask
from flask import request, jsonify
# Create a Flask app
app = flask.Flask(name)  # Create a route for the API
@app.route('/api', methods=['GET'])
def api_call():
# Get the data from the request
data = request.args
# Process the data
result = process_data(data) # Return the result
return jsonify(result)  # Function to process the data
def process_data(data):
# Do something with the data
result = data + 1
# Return the result
return result
# Run the app
if name == '__main__':
app.run()
```

第二次尝试，`temperature` 仍为 0.9：

```
import flask
from flask import request, jsonify
# create the Flask app
app = flask.Flask(name)

app.config['DEBUG'] = True
# create some test data
books = [
{'id': 0, 'title': 'A Fire Upon The Deep', 'author': 'Vernor Vinge', 'first_sentence': 'The coldsleep itself was dreamless.', 'year_published': '1992'},
{'id': 1, 'title': 'The Ones Who Walk Away From Omelas', 'author': 'Ursula K. Le Guin', 'first_sentence': 'With a clamor of bells that set the swallows soaring, the Festival of Summer came to the city Omelas, bright-towered by the sea.', 'published': '1973'},
{'id': 2, 'title': 'Dhalgren', 'author': 'Samuel R. Delany', 'first_sentence': 'to wound the autumnal city.', 'published': '1975'}
]

# create an endpoint
@app.route('/', methods=['GET'])
def home():
return '''<h1>Welcome to our book API!</h1>'''
@app.route('/api/v1/resources/books
```

如你所见，结果差异非常大。

> Note，除了 `temperature`，还有更多参数可以调整以改变输出，比如 `top-k`、`top-p`、重复惩罚、长度惩罚和多样性惩罚，但这些内容超出了本课程的范围。

## 好的实践

有许多方法可以帮助你获得想要的结果。随着你越来越多地使用提示，你会找到自己的风格。

除了我们已经介绍的技巧外，提示大型语言模型时还有一些值得注意的好习惯。

以下是一些值得考虑的好习惯：

+ 指定上下文。上下文很重要，越具体越好，比如领域、主题等。
+ 限制输出。如果你想要特定数量的条目或特定长度，请明确说明。
+ 明确“做什么”和“怎么做”。记得同时说明你想要什么以及如何实现，比如“创建一个包含 `products` 和 `customers` 路由的 Python Web API，分成 3 个文件”。
+ 使用模板。通常你会想用公司数据丰富提示，使用模板来实现。模板中可以包含变量，替换成实际数据。
+ 拼写正确。虽然大型语言模型可能会给出正确答案，但拼写正确会得到更好的回应。

## 练习

下面是用 Python 和 Flask 构建简单 API 的示例代码：

```
from flask import Flask, request
app = Flask(name)

@app.route('/')
def hello():
name = request.args.get('name', 'World')
return f'Hello, {name}!'
if name == '__main__':
app.run()
```

使用 GitHub Copilot 或 ChatGPT 等 AI 助手，应用“自我优化”技术来改进代码。

## 解决方案

请尝试通过为代码添加合适的提示来完成练习。

Tip

设计提示时，可以要求改进，并限制改进次数。你也可以指定改进方向，比如架构、性能、安全性等。

[Solution](https://github.com/microsoft/generative-ai-for-beginners/blob/main/05-advanced-prompts/python/aoai-solution.py)

## 知识检测

为什么要使用 chain-of-thought 提示？请给出 1 个正确回答和 2 个错误回答。

1. 教大型语言模型如何解决问题。
2. B，教大型语言模型发现代码中的错误。
3. C，指导大型语言模型提出不同的解决方案。

答：1，因为 chain-of-thought 是通过给模型提供一系列步骤，以及类似问题及其解决方法，来教它如何解决问题。

## 🚀 挑战

你刚刚在练习中使用了自我优化技术。选取你写过的任意程序，考虑你想对它做哪些改进。然后用自我优化技术应用这些改进。你觉得结果是更好还是更差？

## 干得好！继续学习

完成本课后，查看我们的[生成式 AI 学习合集](https://aka.ms/genai-collection?WT.mc_id=academic-105485-koreyst)，继续提升你的生成式 AI 知识！

[接下来进入第 6](https://github.com/microsoft/generative-ai-for-beginners/blob/main/translations/zh/06-text-generation-apps/README.md?WT.mc_id=academic-105485-koreyst) 课，我们将通过构建文本生成应用来应用提示工程的知识。
