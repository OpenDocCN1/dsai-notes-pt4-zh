
```
Sure, here are three suggestions to improve the code:

Import the correct module: The first line should import Flask instead of flask. The correct import statement should be from flask import Flask.

Add a main function: Add a if __name__ == '__main__': block to the end of the file to run the application when the file is executed.

Use f-strings: Use f-strings to format the return statements in the route functions. For example, return 'Products' can be changed to return f'This is the {request.path} page'.

from flask import Flask, request

app = Flask(__name__)

@app.route('/products')
def products():
    return f'This is the {request.path} page'

@app.route('/customers')
def customers():
    return f'This is the {request.path} page'

if __name__ == '__main__':
    app.run()
```

正如您所看到的，由于对第一个答案的批评，上述人工智能响应正在改进第一个建议的代码。

## [多维度的提示](https://microsoft.github.io/generative-ai-for-beginners/#/05-advanced-prompts/translations/cn/README?wt.mc_id=academic-105485-koreyst&id=%e5%a4%9a%e7%bb%b4%e5%ba%a6%e7%9a%84%e6%8f%90%e7%a4%ba)

多维度的提示是一种类似于自我完善的技术，但它更多的是要求 LLMs 解释自己。目标是减少 LLMs 输出不一致，以确保得出正确的答案。要遵循的工作流程是：

1. 请 LLM 回答问题
2. 对于答案的每一部分，请 LLM 更深入地解释。
3. 如果存在不一致，则丢弃不一致的部分。

重复 2 和 3，直到您完成所有部分并对答案感到满意为止。

> prompt: How can I create a crisis plan to mitigate for a pandemic in 5 steps? LLM answer:

```
1. Identify the risks
1. Identify the stakeholders
1. Identify the resources
1. Identify the actions
1. Identify the communication channels
```

它确定了 5 个步骤，但是我们可以确定这是否正确吗？让我们请 LLM 解释一下每一步：

> prompt: Explain the first step in more detail, what are the risks in detail with a pandemic?

```
The risks are the following:

- Loss of life
- Loss of business
- Loss of reputation
- Loss of customers
- Loss of employees
```

此时，我们希望确保 LLM 是正确的，因此我们要求它更详细地解释第一个风险，并希望它与上面的答案一致：

> prompt: In a Pandemic, which is the biggest risk and why?

```
The biggest risk is loss of life because it's the most important thing to protect.
```

> What are the two biggest risks in a pandemic?

```
The two biggest risks are loss of life and loss of business.
```

在这一点上，LLMs 是一致的，并提到“生活”和“商业”是两大风险。我们现在可以继续下一步并感到相当有信心。然而，我们不应该盲目相信 LLM，我们应该始终验证输出。

## [改变您的输出](https://microsoft.github.io/generative-ai-for-beginners/#/05-advanced-prompts/translations/cn/README?wt.mc_id=academic-105485-koreyst&id=%e6%94%b9%e5%8f%98%e6%82%a8%e7%9a%84%e8%be%93%e5%87%ba)

LLM 本质上是不确定的，这意味着每次运行相同的提示时都会得到不同的结果。例如尝试以下提示：

> "Generate code for a Python Web API"

```
# Import necessary modules
import flask
from flask import request, jsonify

# Create a Flask app
app = flask.Flask(__name__)

# Create a route for the API
@app.route('/api', methods=['GET'])
def api():
    # Get the data from the request
    data = request.args

    # Process the data
    result = process_data(data)

    # Return the result as JSON
    return jsonify(result)

# Function to process the data
def process_data(data):
    # Do something with the data
    result = data + 1

    # Return the result
    return result

# Run the app
if __name__ == '__main__':
    app.run()
```

再次运行相同的提示会生成略有不同的响应：

```
#import necessary packages
import flask
from flask import request, jsonify

#create the Flask application
app = flask.Flask(__name__)
app.config["DEBUG"] = True

#create a list of books
books = [
    {'id': 0,
     'title': 'A Fire Upon the Deep',
     'author': 'Vernor Vinge',
     'first_sentence': 'The coldsleep itself was dreamless.',
     'year_published': '1992'},
    {'id': 1,
     'title': 'The Ones Who Walk Away From Omelas',
     'author': 'Ursula K. Le Guin',
     'first_sentence': 'With a clamor of bells that set the swallows soaring, the Festival of Summer came to the city Omelas, bright-towered by the sea.',
     'published': '1973'},
    {'id': 2,
     'title': 'Dhalgren',
     'author': 'Samuel R. Delany',
     'first_sentence': 'to wound the autumnal city.',
     'published': '1975'}
]

#create an endpoint for the API
@app.route('/', methods=['GET'])
def home():
    return '''<h1>Books API</h1><p>A prototype API for retrieving books.</p>'''
#create an endpoint to return all books
@app.route('/api/v1/resources/books/all', methods=['GET'])
def api_all():
    return jsonify(books)
#create an endpoint to return a single book
@app.route('/api/v1/resources/books', methods=['GET'])
def api_id():
    #check if an ID was provided as part of the URL
    #if ID is provided, assign it to a variable
    #if no ID is provided, display an error
    if 'id' in request.args:
        id = int(request.args['id'])
    else:
        return "Error: No id field provided. Please specify an id."
    #create an empty list for our results
    results = []
    #loop through the data and match results that fit the requested ID
    #IDs are unique, but other fields might return many results
    for book in books:
        if book['id'] == id:
            results.append(book)
    #use the jsonify function from Flask to convert our list of
    #Python dictionaries to the JSON format
    return jsonify(results)
app.run()
```

> 多变的输出是个问题吗？

取决于你想做什么。如果你想要一个具体的回应，那就是一个问题。如果您可以接受诸如“生成任意 3 个地理问题”之类的不同输出，那么这不是问题。

### [利用温度来改变输出](https://microsoft.github.io/generative-ai-for-beginners/#/05-advanced-prompts/translations/cn/README?wt.mc_id=academic-105485-koreyst&id=%e5%88%a9%e7%94%a8%e6%b8%a9%e5%ba%a6%e6%9d%a5%e6%94%b9%e5%8f%98%e8%be%93%e5%87%ba)

好的，所以我们决定将输出限制为更可预测，即更具确定性。我们该怎么做呢？

温度是 0 到 1 之间的值，其中 0 最具确定性，1 最具变化性。默认值为 0.7。让我们看看在温度设置为 0.1 的情况下两次运行相同的提示会发生什么：

> "Generate code for a Python Web API"

```
#import necessary libraries
import flask
from flask import request, jsonify

#create an instance of the Flask class
app = flask.Flask(__name__)

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
if __name__ == '__main__':
    app.run()
```

再次运行提示符会给我们这样的结果：

```
#import necessary libraries
import flask
from flask import request, jsonify

#create a Flask app
app = flask.Flask(__name__)

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
if __name__ == '__main__':
    app.run()
```

这两个输出之间只有很小的差异。这次我们反其道而行之，将温度设置为 0.9：
