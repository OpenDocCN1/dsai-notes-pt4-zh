# 调用库（Libraries）

> 来源：[https://waytoagi.feishu.cn/docx/PzwVdQC31oa6AaxiYVhco8uLn3f](https://waytoagi.feishu.cn/docx/PzwVdQC31oa6AaxiYVhco8uLn3f)

## Python 库

我们提供了一个 Python 库，你可以按如下方式安装：

$ pip install openai

安装后，可以使用绑定（the bindings）和 API Key 运行以下命令：

import os

import openai

# Load your API key from an environment variable or secret management service

openai.api_key = os.getenv("OPENAI_API_KEY")

response = openai.Completion.create(model="text-davinci-003", prompt="Say this is a test", temperature=0, max_tokens=7)

绑定（the bindings）还将安装一个命令行实用程序，你可以按如下方式使用：

$ openai api completions.create -m text-davinci-003 -p "Say this is a test" -t 0 -M 7 --stream

## Node.js 库

我们还有一个 Node.js 库，你可以通过在 Node.js 项目目录中运行以下命令来安装它：

$ npm install openai

安装后，您可以使用该库和您的 API Key 运行以下命令：

const { Configuration, OpenAIApi } = require('openai')

const configuration = new Configuration({

apiKey: process.env.OPENAI_API_KEY

})

const openai = new OpenAIApi(configuration)

const response = await openai.createCompletion({

model: 'text-davinci-003',

prompt: 'Say this is a test',

temperature: 0,

max_tokens: 7

})

## 社区库

下面这些库由更广泛的开发人员社区构建[和维护。如果你](https://help.openai.com/en/articles/6684216-adding-your-api-client-to-the-community-libraries-page)想在此处添加新库，请按照 帮助中心中文章 中有关添加社区库的说明进行操作。

请注意，OpenAI 不会验证这些项目的正确性或安全性。

### C# / .NET

+   Beta[lgo.Ope](https://github.com/betalgo)nAI.GPT3 by Betalgo

### Crystal

+   [Openai-Crystal](https://github.com/sferik/openai-crystal) by [Sferik](https://github.com/sferik)

### Go

+   [Go-GPT3](https://github.com/sashabaranov/go-gpt3) by [sashabaranov](https://github.com/sashabaranov)

[

### Java

+   [openai-java](https://github.com/TheoKanning/openai-java) by [Theo Kanning](https://github.com/TheoKanning)

### Kotlin

+   [openai-kotlin](https://github.com/Aallam/openai-kotlin) by [Mouaad Aallam](https://github.com/Aallam)

### Node.js

+   [openai-api](https://www.npmjs.com/package/openai-api) by [Njerschow](https://github.com/Njerschow)

+   [OpenAI-API-Node](https://www.npmjs.com/package/openai-api-node) by [Erlapso](https://github.com/erlapso)

+   [GPT-X](https://www.npmjs.com/package/gpt-x) by [CEIFA](https://github.com/ceifa)

+   [GPT3](https://www.npmjs.com/package/gpt3) by [Poteat](https://github.com/poteat)

+   [GPTS](https://www.npmjs.com/package/gpts) by [thencc](https://github.com/thencc)

+   @dal[enguyen/Op](https://github.com/dalenguyen)enAI by Dalenguyen

+   [Tectalic/OpenAI](https://github.com/tectalichq/public-openai-client-js) by [tectalic](https://tectalic.com/)

### PHP

+   Orha[nerday/Ope](https://github.com/orhanerday)n-AI by Orhanerday

+   [Tectalic/OpenAI](https://github.com/tectalichq/public-openai-client-php) by [tectalic](https://tectalic.com/)

### Python

+   [OthersideAI](https://www.othersideai.com/) by [OthersideAI](https://github.com/OthersideAI/chronology)

### R

+   [RGPT3](https://github.com/ben-aaron188/rgpt3) by [Ben-Aaron188](https://github.com/ben-aaron188)

### Ruby

+   [Nileshtrivedi](https://github.com/nileshtrivedi) by [OpenAI](https://github.com/nileshtrivedi/openai/)

+   [Alexrudall](https://github.com/alexrudall) by [Ruby-Openai](https://github.com/alexrudall/ruby-openai)

### Scala

+   Open[AI-Scala-Cl](https://github.com/cequence-io)ient by Cequence-IO

### Swift

+   [OpenAIKit](https://github.com/dylanshine/openai-kit) by [dylanshine](https://github.com/dylanshine)

### Unity

+   Open[Ai-Api-Un](https://github.com/hexthedev)ity by hexthedev

### Unreal Engine

+   Open[AI-API-](https://github.com/KellanM)Unreal by KellanM