# 如何从0到1开发插件

> 来源：[https://hyperspace.feishu.cn/docx/QVQwdvsOAoeKxaxnj3wcNFx9niB](https://hyperspace.feishu.cn/docx/QVQwdvsOAoeKxaxnj3wcNFx9niB)

开发插件需要如下步骤：

1.  构思插件清单文件（ ai-plugin.json，必选）

1.  定义插件服务描述文件（ openapi.yaml，必选）

1.  编写示例描述文件（ example.yaml，可选）

1.  编写动作消息注册文件（ msg_content.yaml，可选）

1.  启动插件服务（ openapi.yaml服务, 必选）
下文将展开介绍如何定义插件配置文件，并给出文件编写规范。

## 一、构思插件 manifest 描述文件（ai-plugin.json，必选）

一个插件需要一份 ai-plugin.json 命名的 manifest 文件，这个文件需要描述插件的基本信息。以下是简单的示例，这个示例创建了一个名为“单词本”的插件。

```
{
    &quot;schema_version&quot;: &quot;v1&quot;,
    &quot;name_for_human&quot;: &quot;单词本&quot;,
    &quot;name_for_model&quot;: &quot;wordbook&quot;,
    &quot;description_for_human&quot;: &quot;个性化的英文单词本，可以增加、删除和浏览单词本中的单词，并可以按要求从单词本中随机抽取单词生成句子或段落&quot;,
    &quot;description_for_model&quot;: &quot;帮助用户管理单词本，可以增加、删除、浏览单词本，背单词时可以指定随机抽取单词本中若干个单词，生成句子或段落&quot;,
    &quot;auth&quot;: {
        &quot;type&quot;: &quot;service_http&quot;
    },
    &quot;api&quot;: {
        &quot;type&quot;: &quot;openapi&quot;,
        &quot;url&quot;: &quot;PLUGIN_HOST/openapi.yaml&quot;
    },
    &quot;logo_url&quot;: &quot;PLUGIN_HOST/logo.png&quot;,
    &quot;contact_email&quot;: &quot;support@example.com&quot;,
    &quot;legal_info_url&quot;: &quot;http://www.example.com/legal&quot;,
    &quot;examples&quot;: {
        &quot;url&quot;: &quot;PLUGIN_HOST/example.yaml&quot;
    }
}

```

说明： PLUGIN_HOST 的位置理解为提供数据的服务地址（ IP + 端口或者服务 host），开发者需要根据实际情况替换成自己服务地址，调试环境可以是 localhost + 端口，正式上线需要换成公网地址。

ai-plugin.json 文件总长度建议不超过 1500 字符，超限可能影响插件使用效果，其中各字段的要求详见下方表格：

## 二、定义插件服务描述文件（openapi.yaml，必选）

您的 APIs 定义描述文件需要满足 OpenAPI 标准（详见https://swagger.io/specification/）

文心智能体平台只能获取到您在 API 描述中明确定义的内容。这意味着您只需要向模型暴露插件能力实现必要的 API 端点，无需暴露所有 API 端点。 一个基础的 OpenAPI 格式的 API 描述如下所示：

```
openapi: 3.0.1
info:
    title: 单词本
    description: 个性化的英文单词本，可以增加、删除和浏览单词本中的单词，背单词时从已有单词本中随机抽取单词生成句子或者段落。
    version: &quot;v1&quot;
servers:
    - url: PLUGIN_HOST    #  PLUGIN_HOST 的位置理解为提供数据的服务地址（ IP + 端口或者服务 host），开发者需要根据实际情况替换成自己服务地址，调试环境可以是 localhost + 端口，正式上线需要换成公网地址。
paths:
    /get_wordbook:
        get:
            operationId: getWordbook
            summary: 展示单词列表
            responses:
                &quot;200&quot;:
                    description: 列表展示完成
                    content:
                        application/json:
                            schema:
                                $ref: &quot;#/components/schemas/getWordbook&quot;
    /generate_sentences:
        post:
            operationId: generateSentences
            summary: 背单词，生成句子
            requestBody:
                required: true
                content:
                    application/json:
                        schema:
                            $ref: &quot;#/components/schemas/generateSentences&quot;
            responses:
                &quot;200&quot;:
                    description: 句子生成成功
                    content:
                        application/json:
                            schema:
                                $ref: &quot;#/components/schemas/responseSentences&quot;
    /add_word:
        post:
            operationId: addWord
            summary: 在单词本中添加一个单词
            requestBody:
                required: true
                content:
                    application/json:
                        schema:
                            $ref: &quot;#/components/schemas/addWord&quot;
            responses:
                &quot;200&quot;:
                    description: 单词添加成功
                    content:
                        application/json:
                            schema:
                                $ref: &quot;#/components/schemas/messageResponse&quot;
    /delete_word:
        delete:
            operationId: deleteWord
            summary: 从单词本中删除一个单词
            requestBody:
                required: true
                content:
                    application/json:
                        schema:
                            $ref: &quot;#/components/schemas/deleteWord&quot;
            responses:
                &quot;200&quot;:
                    description: 单词删除成功
                    content:
                        application/json:
                            schema:
                                $ref: &quot;#/components/schemas/messageResponse&quot;

components:
    schemas:
        getWordbook:
            type: object
            required: [wordbook]
            properties:
                wordbook:
                    type: array
                    items:
                        type: string
                    description: 单词本单词列表
        addWord:
            type: object
            required: [word]
            properties:
                word:
                    type: string
                    description: 需要添加到单词本中的一个单词
        deleteWord:
            type: object
            required: [word]
            properties:
                word:
                    type: string
                    description: 需要删除的单词
        generateSentences:
            type: object
            required: [word_number]
            properties:
                word_number:
                    type: integer
                    description: 几个单词
        responseSentences:
            type: object
            required: [words]
            properties:
                words:
                    type: array
                    items:
                        type: string
                        description: 随机抽取的英文单词
        messageResponse:
            type: object
            required: [message]
            properties:
                message:
                    type: string
                    description: 回复信息

```

## 三、编写示例描述文件（example.yaml，可选）

说明：example 机制可帮助开发者提供示例，提升插件调用的正确率。

为了保障模型触发的效果，每个 example.yaml 文件中最多提供 3 个示例。

每个 example.yaml 文件的长度建议不超过 300 个字符

```
version: 0.0.1
examples:
    - context: # 对话历史
          - role: user
            content: 请帮我添加一个单词Hello
          - role: bot
              # 触发插件
            plugin:
                # 应当触发的接口operationId
                operationId: addWord
                # 思考过程，对触发有帮助
                thoughts: 这是一个插入单词到单词本的需求
                requestArguments:
                    word: Hello
    - context: # 对话历史
          - role: user
            content: 删除单词Hello
          - role: bot
              # 触发插件
            plugin:
                # 应当触发的接口operationId
                operationId: deleteWord
                # 思考过程，对触发有帮助
                thoughts: 这是一个从单词本中删除单词的需求
                requestArguments:
                    word: Hello
    - context: # 对话历史
          - role: user
            content: 我要背两个单词
          - role: bot
              # 触发插件
            plugin:
                # 应当触发的接口operationId
                operationId: generateSentences
                # 思考过程，对触发有帮助
                thoughts: 这是一个从单词本中抽取单词生成句子的需求
                requestArguments:
                    word_number: 2
    # 反例，无需触发此插件的任一某个接口
    - context: # 对话历史
          - role: user
            content: 怎么养成记单词的习惯?
          - role: bot
              # 无需触发
            plugin:
                thoughts: 我不需要使用以上工具
```

## 四、编写动作消息注册文件（msg_content.yaml，可选）

插件动作消息注册文件，为 yaml 格式，用于描述和定义了插件动作，该动作可以显示在插件信息栏中。如果您希望将插件的执行动作，显示在用户前端，需要定义此 msg_content 文件，您需要将插件返回的错误码和提示信息注册在 msg_content 中，在您的插件返回 errCode 后，系统将会根据您的 errCode，映射响应的执行动作。

**规范要求**

1.  一组执行动作，包括 1 个动作名（ actionName ）和1 个动作内容（ actionContent ）。此执行动作将会显示给用户。

1.  每一组执行动作中，动作名可重复，动作内容不可重复；例如：

+   阅读文档｜开始阅读文档；

+   阅读文档｜完成阅读文档；

```
# yaml 文件中定义的 operationId
create:
  # 标识插件调用失败的错误码和文案
  error:
    - errCode: failed
      actionName: 生成视频
      actionContent: 画面生成失败，建议您换一个话题，更新描述后可再次尝试
    - errCode: STG.aft.person_refused
      actionName: 生成视频失败
      actionContent: 我没有学习过指定人物的视频生成能力，请更新描述后可再次尝试
  success:
  # errCode = 0 是默认成功，无需单独定义
  # 有时使用sse返回，执行过程中有很多过程，都应该定义为成功
    - errCode: doing
      actionName: 开始执行
      actionContent: 开始执行视频生成
    - errCode: complete
      actionName: 执行完成
      actionContent: 视频生成完成

# 可定义通用的错误码和文案，将为每一个 api 追加 common 中的内容
# common为保留字，如果将一个 api 的 operationId 定义为了 common，此处定义的错误码仍会生效到每一个 api 上
common:
  # 同上
  error:
    # 如果错误码与其他 operationId 中定义的相同，以其他 operationId 为准
    - errCode: -1
      actionName: 鉴权失败
      actionContent: 鉴权信息已过期，请重新登录

```

## 五、开发自己的plugin-server

注意：为了给用户提供更好的体验，插件服务有超时要求：0.5s 连接超时，3s 读超时，2s 读响应头超时，如果插件服务内部处理流程较长，建议采用 sse 流式方式及时返回。

根据信息中描述的 API 定义开发自己的服务接口，并在服务中定义自己的插件能力。如下是一个 python 的示例，您也可以使用其他开发语言进行 server 开发：

若您期望使用本地调试功能，可在代码中增加如下代码段（在示例中已包含）。

```
from flask_cors import CORS
CORS(app, resources={r&quot;/*&quot;: {&quot;origins&quot;: &quot;https://agents.baidu.com&quot;}})
```

```
#!/usr/env python3
# -*- coding: UTF-8 -*-

from flask import Flask, request, send_file, make_response
from flask_cors import CORS
import json
import random

app = Flask(__name__)
CORS(app, resources={r&quot;/*&quot;: {&quot;origins&quot;: &quot;https://agents.baidu.com&quot;}})

wordbook = []

def make_json_response(data, status_code=200):
    response = make_response(json.dumps(data), status_code)
    response.headers[&quot;Content-Type&quot;] = &quot;application/json&quot;
    return response


@app.route(&quot;/add_word&quot;, methods=[&#x27;POST&#x27;])
async def add_word():
    &quot;&quot;&quot;
        添加一个单词
    &quot;&quot;&quot;
    word = request.json.get(&#x27;word&#x27;, &quot;&quot;)
    wordbook.append(word)
    return make_json_response({&quot;message&quot;: &quot;单词添加成功&quot;})


@app.route(&quot;/delete_word&quot;, methods=[&#x27;DELETE&#x27;])
async def delete_word():
    &quot;&quot;&quot;
        删除一个单词
    &quot;&quot;&quot;
    word = request.json.get(&#x27;word&#x27;, &quot;&quot;)
    if word in wordbook:
        wordbook.remove(word)
    return make_json_response({&quot;message&quot;: &quot;单词删除成功&quot;})


@app.route(&quot;/get_wordbook&quot;)
async def get_wordbook():
    &quot;&quot;&quot;
        获得单词本
    &quot;&quot;&quot;
    return make_json_response({&quot;wordbook&quot;: wordbook})


@app.route(&quot;/generate_sentences&quot;, methods=[&#x27;POST&#x27;])
async def generate_sentences():
    &quot;&quot;&quot;
        生成句子
    &quot;&quot;&quot;
    number = request.get_json()[&#x27;word_number&#x27;]
    number = min(number, len(wordbook))
    random_words = random.sample(wordbook, number)
    prompt = &quot;利用英文单词（ words ）生成一个英文段落，要求这个段落不超过 100 个英文单词且必须全英文，并包含上述英文单词，同时是一个有逻辑的句子&quot;
    # API返回字段&quot;prompt&quot;有特殊含义：开发者可以通过调试它来调试输出效果
    return make_json_response({&quot;words&quot;: random_words, &quot;prompt&quot;: prompt})


@app.route(&quot;/logo.png&quot;)
async def plugin_logo():
    &quot;&quot;&quot;
        注册用的：返回插件的 logo，要求 48 x 48 大小的 png 文件.
        注意：API路由是固定的，事先约定的。
    &quot;&quot;&quot;
    return send_file(&#x27;logo.png&#x27;, mimetype=&#x27;image/png&#x27;)


@app.route(&quot;/.well-known/ai-plugin.json&quot;)
async def plugin_manifest():
    &quot;&quot;&quot;
        注册用的：返回插件的描述文件，描述了插件是什么等信息。
        注意：API 路由是固定的，事先约定的。
    &quot;&quot;&quot;
    host = request.host_url
    with open(&quot;.well-known/ai-plugin.json&quot;, encoding=&quot;utf-8&quot;) as f:
        text = f.read().replace(&quot;PLUGIN_HOST&quot;, host)
        return text, 200, {&quot;Content-Type&quot;: &quot;application/json&quot;}


@app.route(&quot;/.well-known/openapi.yaml&quot;)
async def openapi_spec():
    &quot;&quot;&quot;
        注册用的：返回插件所依赖的插件服务的API接口描述，参照 openapi 规范编写。
        注意：API 路由是固定的，事先约定的。
    &quot;&quot;&quot;
    with open(&quot;.well-known/openapi.yaml&quot;, encoding=&quot;utf-8&quot;) as f:
        text = f.read()
        return text, 200, {&quot;Content-Type&quot;: &quot;text/yaml&quot;}

@app.route(&quot;/example.yaml&quot;)
async def exampleSpec():
    host = request.host_url
    with open(&quot;example.yaml&quot;) as f:
        text = f.read().replace(&quot;PLUGIN_HOST&quot;, host)
        return text, 200, {&quot;Content-Type&quot;: &quot;text/yaml&quot;}



@app.route(&#x27;/&#x27;)
def index():
    return &#x27;welcome to my webpage!&#x27;

if __name__ == &#x27;__main__&#x27;:
    app.run(debug=True, host=&#x27;127.0.0.1&#x27;, port=8081)
```

## 六、接下来

您已经完成了插件基础开发，接下来您可能需要了解：

+   [如何在平台创建并发布插件](https://agents.baidu.com/docs/develop/ability-plugin/basic/create_to_publish/)

+   [API接口开发注意事项](https://agents.baidu.com/docs/develop/ability-plugin/advanced/api_notes/)

+   [如何使用插件鉴权功能](https://agents.baidu.com/docs/develop/ability-plugin/advanced/auth/)