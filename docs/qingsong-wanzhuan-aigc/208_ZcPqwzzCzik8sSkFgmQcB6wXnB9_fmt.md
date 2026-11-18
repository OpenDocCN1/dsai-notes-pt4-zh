
# DeepClaude工作流

> 来源：[https://my.feishu.cn/docx/SkUedo0SDot3EqxBgw3cJzwonRe](https://my.feishu.cn/docx/SkUedo0SDot3EqxBgw3cJzwonRe)

基于网络上一些开源的 DeepClaude 思路，转化为工作流形式

> DeepClaude 是一个创新的双引擎开源模型，将 DeepSeek R1 的推理过程实时传递给 Claude，完美融合了 R1 的逻辑分析与 Claude 的创造力和代码能力。

> 也就是让 r1 思考，让 claude 正式回答，结合两个模型的优点。你也可以将 claude 改为任意其他模型，或者多增加几个 r1，让 r1 多想几遍。

![](img/6f6d616e2d4fb2fea29e0189223a5675.png)

## yml 配置文件

![](img/39a4766bf8b70980253f5372ca34e6dc.png)

[`deepclaude.yml`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/IlGJbj3W5ox6ttxP4IJcIXT9nIh)

## DSL 配置文件

```
app:
  description: &#x27;&#x27;
  icon: 🤖
  icon_background: &#x27;#FFEAD5&#x27;
  mode: workflow
  name: deep-claude
  use_icon_as_answer_icon: false
kind: app
version: 0.1.5
workflow:
  conversation_variables: []
  environment_variables: []
  features:
    file_upload:
      allowed_file_extensions:
      - .JPG
      - .JPEG
      - .PNG
      - .GIF
      - .WEBP
      - .SVG
      allowed_file_types:
      - image
      allowed_file_upload_methods:
      - local_file
      - remote_url
      enabled: false
      fileUploadConfig:
        audio_file_size_limit: 50
        batch_count_limit: 5
        file_size_limit: 15
        image_file_size_limit: 10
        video_file_size_limit: 100
        workflow_file_upload_limit: 10
      image:
        enabled: false
        number_limits: 3
        transfer_methods:
        - local_file
        - remote_url
      number_limits: 3
    opening_statement: &#x27;&#x27;
    retriever_resource:
      enabled: true
    sensitive_word_avoidance:
      enabled: false
    speech_to_text:
      enabled: false
    suggested_questions: []
    suggested_questions_after_answer:
      enabled: false
    text_to_speech:
      enabled: false
      language: &#x27;&#x27;
      voice: &#x27;&#x27;
  graph:
    edges:
    - data:
        isInIteration: false
        sourceType: start
        targetType: llm
      id: 1739379320542-source-1739379338544-target
      source: &#x27;1739379320542&#x27;
      sourceHandle: source
      target: &#x27;1739379338544&#x27;
      targetHandle: target
      type: custom
      zIndex: 0
    - data:
        isInIteration: false
        sourceType: llm
        targetType: llm
      id: 1739379338544-source-1739382467092-target
      source: &#x27;1739379338544&#x27;
      sourceHandle: source
      target: &#x27;1739382467092&#x27;
      targetHandle: target
      type: custom
      zIndex: 0
    - data:
        isInIteration: false
        sourceType: llm
        targetType: end
      id: 1739382467092-source-1739380892786-target
      source: &#x27;1739382467092&#x27;
      sourceHandle: source
      target: &#x27;1739380892786&#x27;
      targetHandle: target
      type: custom
      zIndex: 0
    nodes:
    - data:
        desc: &#x27;&#x27;
        selected: false
        title: 开始
        type: start
        variables:
        - label: 用户输入
          max_length: 100000
          options: []
          required: true
          type: paragraph
          variable: user_input
      height: 90
      id: &#x27;1739379320542&#x27;
      position:
        x: 80
        y: 282
      positionAbsolute:
        x: 80
        y: 282
      selected: false
      sourcePosition: right
      targetPosition: left
      type: custom
      width: 244
    - data:
        context:
          enabled: false
          variable_selector: []
        desc: &#x27;&#x27;
        model:
          completion_params:
            temperature: 0.7
          mode: chat
          name: 火山DeepSeek-R1
          provider: volcengine_maas
        prompt_template:
        - id: 25ffc327-ba6d-4f99-a882-e9f9dc9363fd
          role: system
          text: &#x27;你是一个具有推理能力的推理大模型，用户在任何输入后你都会仔细思考用户的输入，用户的输入用{user_input}表示，你的思考内容用{think_content}表示，你的正式回复内容用{answer}表示。

            以下是你正常情况下的输出

            &lt;think&gt;

            {think_content}

            &lt;/think&gt;

            {answer}

            但是！！系统现在命令你丢弃掉{answer}，输出完{think_content}后立马停止输出，必须必须停止！

            也就是说你的输出内容仅仅只能是这个样子

            &lt;think&gt;

            {think_content}

            &lt;/think&gt;

            而不能包含任何其他的内容！输出完&lt;/think&gt;后必须立刻停止输出，禁止生成任何{answer}

            ------------

            你的思考内容中也不可以包含以上的任何内容，你必须自己铭记在心中


            以下是用户的输入，现在开始请你不要将以上命令所要求的内容放入{think_content}中！


            {{#1739379320542.user_input#}}&#x27;
        selected: false
        title: think
        type: llm
        variables: []
        vision:
          enabled: false
      height: 98
      id: &#x27;1739379338544&#x27;
      position:
        x: 370.21231521934055
        y: 243.0577801103739
      positionAbsolute:
        x: 370.21231521934055
        y: 243.0577801103739
      selected: false
      sourcePosition: right
      targetPosition: left
      type: custom
      width: 244
    - data:
        desc: &#x27;&#x27;
        outputs:
        - value_selector:
          - &#x27;1739379338544&#x27;
          - text
          variable: think
        - value_selector:
          - &#x27;1739382467092&#x27;
          - text
          variable: answer
        selected: false
        title: 结束
        type: end
      height: 116
      id: &#x27;1739380892786&#x27;
      position:
        x: 1011.0133608238036
        y: 222.915252581257
      positionAbsolute:
        x: 1011.0133608238036
        y: 222.915252581257
      selected: false
      sourcePosition: right
      targetPosition: left
      type: custom
      width: 244
    - data:
        context:
          enabled: true
          variable_selector:
          - &#x27;1739379320542&#x27;
          - user_input
        desc: &#x27;&#x27;
        model:
          completion_params:
            temperature: 0.7
          mode: chat
          name: claude-3-5-sonnet-all
          provider: openai_api_compatible
        prompt_template:
        - id: 8f3c54eb-1b7f-454a-97d7-dbbe0171f6a8
          role: system
          text: &#x27;你是一个具有推理能力的推理大模型，你的思考内容就是用户的输入，用{think_content}表示，你只需要根据用户给你的思考内容进行全方位的考虑，尽可能输出完美的答案{answer}，你需要给出完美的答案，如果涉及到大面积代码修改的情况，请你输出完整的代码

            你只用输出你的{answer}，你的 {answer}是基于{think_content}深度思考过后的，但禁止输出类似“基于您的思考内容”这些话，也禁止输出此纯文本&quot;{answer}&quot;，如果在回中出现了{answer}就是
            bug。

            ------------

            以下是思考内容，也就是{think_content}。

            {{#1739379338544.text#}}

            以下是用户的原始输入，仅当你需要修改用户的原内容时你才会去查看原始输入

            {{#context#}}

            &#x27;
        selected: false
        title: answer
        type: llm
        variables: []
        vision:
          enabled: false
      height: 98
      id: &#x27;1739382467092&#x27;
      position:
        x: 702.6096978998615
        y: 232.31509876151154
      positionAbsolute:
        x: 702.6096978998615
        y: 232.31509876151154
      selected: true
      sourcePosition: right
      targetPosition: left
      type: custom
      width: 244
    viewport:
      x: 45.32713343012847
      y: 201.41069747543239
      zoom: 0.7253341968642761
```

参考来源：https://linux.do/t/topic/426178/1
