
```python
import gradio as gr
from PIL import Image
import io
import appbuilder  # 假设appbuilder是您使用的OCR库

def ocr_function(image_upload):
    # 将上传的图片数据转换为 PIL Image 对象
    image = Image.open(io.BytesIO(image_upload))
    
    # 使用 appbuilder 的 GeneralOCR 进行 OCR
    general_ocr = appbuilder.GeneralOCR()
    message = appbuilder.Message(content={&quot;raw_image&quot;: image_upload})
    out = general_ocr.run(message)
    
    # 假设out.content包含识别的文字
    return out.content

# 创建 Gradio 接口
with gr.Blocks() as demo:
    image_upload = gr.Image(label=&quot;上传图片&quot;)
    text_output = gr.Textbox(label=&quot;识别结果&quot;)  # 设置为disabled，因为结果由OCR函数填充
    submit = gr.Button(&quot;识别&quot;)
    
    # 当用户点击&quot;识别&quot;按钮时，触发ocr_function
    submit.click(fn=ocr_function, inputs=[image_upload], outputs=[text_output])

# 启动 Gradio 应用
demo.launch()
```

+ 去官方文档查看图片组件应该如何使用

> 被其他的内容迷花了眼，[官方文档](https://www.gradio.app/guides/quickstart)全英文且内容比较丰富，建议直接搜索，而不是按照目录来找

![](img/285d4390b0f72bad4e68304d755d4490.png)

+ 发现作为输入组件，获得的是图片的路径

![](img/9da78d8aa91997cdcca9cf3195c7caea.png)

+ 于是进行对应的代码修改，上传对应路径里的图片文件

> PS：这里在前面跑通了调用应用进行OCR并出题

+ 具体代码及其效果

```python
import os
import appbuilder
# 注意需要设置os.environ[&quot;APPBUILDER_TOKEN&quot;]和app_id
# 初始化Agent实例
builder = appbuilder.AgentBuilder(app_id)

# 创建会话ID
conversation_id = builder.create_conversation()

# 上传植物图片
file_id = builder.upload_local_file(conversation_id, &quot;./自我效能感1.png&quot; )
file_id2 = builder.upload_local_file(conversation_id, &quot;./自我效能感2.png&quot;)

# 植物识别
msg = builder.run(conversation_id, &quot;请基于图片文本总结内容，然后出题&quot;, file_ids=[file_id, file_id2])
print(&quot;出题结果：&quot;, msg.content.answer)
```

![](img/b4f8895f2350cb22c795c7fc28d5d56e.png)

+ 发现仍然报类型错误

![](img/bc131f27c6c8d29934be7ad2e66ce8ad.png)

+ 需要设置`Image`组件上传的类型

![](img/9207485d6a11b162a22d7206b7bad126.png)

+ 最后成功了~

> 注意：下面的代码都省略了`token`和`app_id`的设置，其中调用应用需要`app_id`，调用组件不需要

### 调用可OCR的应用

![](img/9d3fd53898ee20827e0f5dcd66947df5.png)

```python
import gradio as gr
from PIL import Image
import appbuilder

def ocr_function(image_upload):
    # 初始化Agent实例
    builder = appbuilder.AgentBuilder(app_id)

    # 创建会话ID
    conversation_id = builder.create_conversation()

    # 上传植物图片
    file_id = builder.upload_local_file(conversation_id, image_upload )
    # file_id2 = builder.upload_local_file(conversation_id, &quot;./自我效能感2.png&quot;)

    # 植物识别
    msg = builder.run(conversation_id, &quot;请基于图片文本总结内容，然后出题&quot;, file_ids=[file_id])
    return &quot;出题结果：&quot; + msg.content.answer

# 创建 Gradio 接口
with gr.Blocks() as demo:
    image_upload = gr.Image(label=&quot;上传图片&quot;, type=&#x27;filepath&#x27;)
    text_output = gr.Textbox(label=&quot;识别结果&quot;) 
    submit = gr.Button(&quot;识别&quot;)
    
    # 当用户点击&quot;识别&quot;按钮时，触发ocr_function
    submit.click(fn=ocr_function, inputs=[image_upload], outputs=[text_output])

# 启动 Gradio 应用
demo.launch()
```

### 调用OCR组件

```python
import gradio as gr
import appbuilder

def ocr_function(image_upload):
    with open(image_upload, &#x27;rb&#x27;) as image_file:
        image = image_file.read()
    
    # 使用 appbuilder 的 GeneralOCR 进行 OCR
    general_ocr = appbuilder.GeneralOCR()
    message = appbuilder.Message(content={&quot;raw_image&quot;: image})
    out = general_ocr.run(message)
    
    return out.content

# 创建 Gradio 接口
with gr.Blocks() as demo:
    image_upload = gr.Image(label=&quot;上传图片&quot;, type=&#x27;filepath&#x27;)
    text_output = gr.Textbox(label=&quot;识别结果&quot;) 
    submit = gr.Button(&quot;识别&quot;)
    
    # 当用户点击&quot;识别&quot;按钮时，触发ocr_function
    submit.click(fn=ocr_function, inputs=[image_upload], outputs=[text_output])

# 启动 Gradio 应用
demo.launch()
```

![](img/2c4a27d676874a111a7f202e17ea77d1.png)

> 可以发现上面的步骤并不是那么一帆风顺，笔者花了大约2天的碎片时间，可能专注时间共用时 5h

> 大家自己在实践的过程中，可以参考如下顺序：

1. 先仔细搜索是否有人完成过类似的功能代码，然后直接复用
2. 如果复用报错，可以先在AI的帮助下分析报错信息和理解该段代码的逻辑
3. 如果仍然报错，考虑换一个代码参考，或者直接开始官方文档，分析具体的函数接口模式
4. 不断试错、不断分析、控制变量进行调试，直到成功

> PS：记得不要卡住自己太久，及时求助

# 整合：将功能加入已有项目

> 然后分析`ChatTests`代码，将上面已经跑通的代码整合上去

## 分析并添加UI

![](img/a99f8f23520384ca4ff5700387cd18cf.png)

+ 可以考虑将`Image`放在输入文本的左边，修改对应代码后的效果如下：

![](img/2964ff8e2aeec0f1426055936cbe345e.png)

![](img/fcaf17fb2bd433c91aa4a1ddcbdf4da6.png)

+ 在这里尝试使用可OCR的应用进行对应的出题，其实最方便的方式是复制【生成新题目】对应的函数，新建一个按钮进行对应的调用，但那样【更新题目】也同时需要进行对应的更新

+ 因此选择直接复用【生成新题目】的按钮和函数代码，需要修改其中调用应用的逻辑

> 在思考实现方案的过程中还有新的发现和思考：

+ 在上传完图片后，直接调用应用识别且生成题目，可能导致后续纯文本生成题目有问题，需要添加一个“清除输入组件”的功能

+ 后续可以考虑将OCR识别文本、语音识别文本做成和生成题目相独立的板块，但目前不清楚`OCR组件`的额度，在找到替代方案之前，考虑直接调用对应的组件

+ 目前只支持调用对应组件的话，可以考虑直接做成`tab`，或者先选择输入方式：图片/纯文本，再显示对应的图片/文本输入组件

+ 或者直接做一个只做OCR功能、不出题的应用

### 将函数代码合并到对应的位置

+ 先将`button`的输入输出修改一下：

修改前的代码：

```python
generate_button.click(chattests.generate_quesiton, 
        inputs = [context_input, advice_input], 
        outputs = [gen_output, question_markdown, 
                   question_gen, choicesA_gen, choicesB_gen, choicesC_gen, choicesD_gen,
                   answer_gen, analysis_gen, difficulty_gen, comment_gen, subject_gen, tags_gen,
                   return_log, whale_format, choices_radio, analysis, op_log, subject, difficulty]
    )
```

修改后的代码：

```python
image_input = gr.Image(type="filepath", label="文本图片", height=400, width=50)

generate_button.click(chattests.generate_quesiton, 
        inputs = [context_input, advice_input, image_input], 
        outputs = [gen_output, question_markdown, 
                   question_gen, choicesA_gen, choicesB_gen, choicesC_gen, choicesD_gen,
                   answer_gen, analysis_gen, difficulty_gen, comment_gen, subject_gen, tags_gen,
                   return_log, whale_format, choices_radio, analysis, op_log, subject, difficulty]
    )
```

+ 在发现`图片组件`内有图片后，调用`OCR组件`：

修改前的代码：

```python
# 初始化Agent
app_id = os.environ.get(&quot;CHATTEST_ID&quot;)

agent_json = appbuilder.AgentBuilder(app_id)



def generate_quesiton(self, context, advice):
        context = &quot;```\n&quot; + context + &quot;```\n&quot; + &quot;请基于以上文本出单选题，正确选项必须唯一，其他选项必须是错误的，不同选项内容必须有差异，选项中不要包含选项字母，答案是正确选项的字母。&quot;

        if advice != &quot;&quot;:

            context = context + &quot;\n 另外：&quot; + advice

        conversation_new = agent_json.create_conversation()

        msg = agent_json.run(conversation_new, context)

        # 提取 JSON 数据，最后生成的类型是‘dict’，可以放入JSON框(否则会报错)

        markdown_text = msg.content.answer

        # print(markdown_text)

        json_start_index = markdown_text.find(&quot;```json&quot;) + 8
        json_end_index = markdown_text.find(&quot;```&quot;, json_start_index) - 1

        json_data = &quot;&quot;

        # print(json_data + &quot;\n json_start_index=&quot; + str(json_start_index))

        if json_start_index != 7:

            json_data = markdown_text[json_start_index:json_end_index].strip()

            self.question_json = json.loads(json_data)

            self.update_self()

            self.addLog(&quot;生成题目成功，题干为：&quot; + self.question)

            self.conversation_current = conversation_new

        else:

            self.addLog(&quot;ERROR：生成题目失败，详见【返回内容】&quot;)

            gr.Warning(&quot;ERROR: 生成题目失败，详见【返回内容】，请重试&quot;)      

        options = [&quot;A.&quot;+self.choices[0]+&quot;\n&quot;,&quot;B.&quot;+self.choices[1]+&quot;\n&quot;, 

                   &quot;C.&quot;+self.choices[2]+&quot;\n&quot;, &quot;D.&quot;+self.choices[3]+&quot;\n&quot;]

        question_markdown = &quot;## &quot;+ self.question

        formatted_whale = self.format_whale_editor()

        radio = gr.Radio(options,label=&quot;选择你认为最正确的一项&quot;)

        return (self.question_json, question_markdown, 

                self.question, self.choices[0], self.choices[1], self.choices[2],

                self.choices[3], self.answer, self.analysis, 

                self.difficulty, self.comment, self.subject, self.tags,

                markdown_text, formatted_whale, radio, self.analysis, self.log,

                self.subject, self.difficulty)

```

修改后的代码：

```python
# 初始化Agent
```
