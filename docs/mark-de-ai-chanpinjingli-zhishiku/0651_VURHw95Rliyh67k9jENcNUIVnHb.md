# 0-1基于dify搭建智能体（Agent）助手

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/Lhf7dRzg4o6Lrhxyo6ScCWRgn0g](https://qqs7y1hozd1.feishu.cn/docx/Lhf7dRzg4o6Lrhxyo6ScCWRgn0g)

本方案旨在利用 Dify 平台的智能体功能，为电商企业打造一个高度智能化的 AI 客服助手。

> 通过采用先进的智能体技术，能够**自主理解和处理复杂客户询问任务****，**提供个性化和精准的服务响应。

> 本方案适合各种规模和复杂度的电商平台，帮助企业显著提升客服效率和响应速度，减少人工参与，降低运营成本。

> 通过这套方案，企业可以**实现客服流程的全面智能化**，提升市场竞争力和客户满意度。

# 一、构建智能体（Agent）AI 电商客服助手

## 1.搭建 AI 应用

#####

创建一个智能体（Agent）AI 电商客服助手。

> 其不仅可以检索和利用知识库中的信息，还能够进行自主决策和处理复杂任务，这为用户带来了更优质的体验。

#####

1.  **创建知识库。**

> 使大模型能够获取专属业务知识，从而提供更精准的回答。若知识库已创建则可跳过本步骤。

1.  单击 Dify 平台顶部**知识库**，接着单击下方的**创建知识库**，

1.  选择数据源为**导入已有文本**，随后选择本地文件，最后单击**下一步**。

1.  **设置文本分**段与清洗。按如下配置进行设置后，单击保存并处理。未提及配置使用默认值即可。

1.  在**处理并完成**页面，单击下方的**前往文档**，在文档列表页面，待状态显示为**可用**后，即表示当前文档已处理完成。

>

1.  创建"查询订单信息"自定义工具。

> 使大模型能够通过工具调用获取实时订单数据。

1.  打开浏览器，进入 Dify 平台。

1.  单击Dify 系统页面顶部**工具**，接着单击下方的**自定义 > 创建自定义工具**。

1.  在弹出的**创建自定义工具**面板中，填写如下信息，然后单击**保存**。未提及配置使用默认值即可。

```
openapi: &quot;3.0.0&quot;
info:
  version: 1.0.0
  title: 订单API
servers:
  - url: http://&lt;ECS 公网 IP&gt;
paths:
  /order:
    get:
      summary: 获取订单详情信息
      operationId: 查询订单信息
      parameters:
        - name: orderid
          in: query
          required: true
          schema:
            type: string
          description: 订单唯一标识
      responses:
        &#x27;200&#x27;:
          description: 获取成功
          content:
            application/json:
              schema:
                type: object
                properties:
                  code:
                    type: integer
                  data:
                    type: object
      security:
        - bearerAuth: []
components:
  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
```

1.  单击顶部**工作室**，然后单击**创建空白应用**。

1.  在弹出的**创建空白应用**面板中，选择应用类型为 **Agent**，并设置应用名称，最后单击**创建**，进入应用**编排**页面。

1.  设置对话使用的模型。在右侧**模型**选择下拉框中，选择模型为qwen-max。模型参数保持默认即可，您也可以按需进行调整。

![](img/e1bf895b9bbfdd0b736c95112612b607.png)

1.  **设置提示词。**在提示词输入框中填写以下提示词。

此处的提示词：

```
7. 如果当前需要查询订单信息，则从当前的输入中分析出订单ID（订单ID为一串连续的12位数字，例如：202501011001），然后调用工具“查询订单信息”进行查询，其有一个输入参数为订单ID（orderid）
```

增加的内容是为了提示大模型在需要查询订单时，调用（Function Call）工具“查询订单信息”，以获取订单数据。

```
你是一名资深电商客服专家，拥有十年以上的电商客服经验。你的任务是通过友好、专业的交流，帮助用户解决他们在购物过程中遇到的问题，提供满意的售后服务。在回答问题时，请遵循以下原则：

1. 亲切的口语化表达：使用自然、亲切的语言，让用户感受到温暖和被重视。例如：
- &quot;您好！感谢您的咨询，很高兴为您服务。&quot;
- &quot;没问题，我来帮您看看具体情况。&quot;

2. 关键数据精确：在涉及金额、时间、订单号等关键信息时，务必准确无误。例如：
- &quot;您的订单号是123456789，总金额为588元。&quot;
- &quot;预计送达时间为2天后，您可以放心等待。&quot;

3. 引导用户描述详情：当遇到不确定或复杂的问题时，耐心引导用户提供更多信息，以便更好地解决问题。例如：
- &quot;请您提供一下订单号，方便我查询具体情况。&quot;
- &quot;您能具体描述一下问题吗？我会尽力帮您解决。&quot;

4. 专业性与耐心：始终保持专业和耐心，即使面对复杂或重复的问题，也要保持冷静，避免让用户感到被忽视或不被重视。例如：
- &quot;这个问题我了解，让我为您详细解答。&quot;
- &quot;如果您有任何疑问，随时欢迎咨询，我们会一直陪着您解决问题。&quot;

5. 结束语：在解决问题后，以友好和感谢的态度结束对话，让用户体验到愉快的售后服务。例如：
- &quot;希望我的解答对您有帮助，祝您购物愉快！&quot;
- &quot;感谢您的耐心配合，祝您生活愉快！&quot;

6. 合规性与隐私保护：在回答问题时，确保不泄露用户的个人信息，同时遵守公司的相关规则和政策。

7. 如果当前需要查询订单信息，则从当前的输入中分析出订单ID（订单ID为一串连续的12位数字，例如：202501011001），然后调用工具“查询订单信息”进行查询，其有一个输入参数为订单ID（orderid）。

请根据以上原则，为用户提供专业、亲切、高效的客服支持！
```

1.  添加知识库。单击**知识库**区域右侧的**“+添加**”**，然**后选择前面步骤创建的知识库，最后单击添加。

1.  添加自定义工具。单击**工具**区域右侧的“**+添加**”按钮。在弹出面板中选择自定义 > 查询订单信息 > 查询订单信息。

1.  调试应用。在右侧**调试与预览**下的对话框中输入问题，然后单击，即可调试对话效果。

1.  发布应用。单击右上方的**发布 > 发布更新**，然后单击下方的**运行**。系统将打开一个新页面，该页面的地址即为应用发布后的访问地址。

1.  测试对话效果。按照页面提示开始对话，体验与 AI 应用对话功能。

![](img/d43ebcf7f1cc4e1dcaca961a34bd5887.png)

## **2.集成 AI 应用**

### 2.1网页嵌入

通过在电商系统的网页中嵌入 Dify 提供的代码，可以实现快速集成，无需额外的开发工作。

1.  **单击 **Dify 平台页面顶部中间的工作室，单击需要集成到电商系统中的目标应用，进入应用编排页面。

1.  单击右侧**发布 > 嵌入网站**。在弹出的**“嵌入到网站中”**的面板中选择第 2 种方式，然后复制代码。

![](img/3b1066fec3beb7c2290dad89e54064ab.png)

1.  找到在“部署 Dify 平台”方案中已部署电商系统的目标云服务器ECS，其规格为ecs.e-c1m2.large（2 vCPU 4 GiB）的 ECS。

1.  **若通过手**动部署 Dify 平台：在部署资源阶段所创建的 ECS，即为目标云服务器 ECS。

1.  **若通过一****键部**署 Dif**y 平台**：在一键部署阶段完成后，单击资**源栈详情页**面的资源页**签，找到资**源名称为 EcsInstance 的资源 ID，使用这个资源 ID 作为 ECS 的实例 ID，可以在 ECS 控制台的实例列表中快速定位到目标云服务器 ECS。

1.  **登录目标云服务器ECS。**

1.  登录ECS管理控制台，在**实例**页面，找到前面步骤所创建的 ECS 实例。

1.  在右侧**操作**列，单击**远程连接**，并选择**通过Workbench远程连接**。根据页面提示登录，进入终端页面。具体操作请参见：使用Workbench登录ECS实例。

1.  执行脚本。将“<上一步骤复制的代码>”替换为前面第一步骤复制的代码，然后在终端页面中执行如下脚本。

```
sed -i &#x27;/&lt;\/html&gt;/{n;:a;N;$!ba;d}&#x27; templates/index.html

echo &quot;&quot; &gt;&gt; templates/index.html

cat &lt;&lt; EOF &gt;&gt; templates/index.html
&lt;上一步骤复制的代码&gt;
EOF
```

打开浏览器，单击页面右下角即可开始进行对话。

![](img/f391da9cd0aa4831023d581a2dc13c27.png)

### 2.2基于 APIs 开发

> Dify 基于“后端即服务”理念为所有应用提供了 API。

> 为 AI 应用开发者带来了诸多便利。通过这一理念，开发者可以直接在前端应用中获取大型语言模型的强大能力，而无需关注复杂的后端架构和部署过程。

#### 2.2.1如何使用

**选择一个应用，在应用（Apps****）**左侧导航中可以找到访问 API（API Access）。

> 在该页面中你可以查看 Dify 提供的 API 文档，并管理可访问 API 的凭据。

![](img/e524b1d701138adba818a46f4c557127.png)

> 例如，你是一个咨询公司的开发部分。

> 你可以基于公司的私有数据库提供 AI 能力给终端用户或开发者，但开发者无法掌握你的数据和 AI 逻辑设计，从而使得服务可以安全、可持续的交付并满足商业目的。

在最佳实践中，API 密钥应通过后端调用。

> 而不是直接以明文暴露在前端代码或请求中，这样可以防止你的应用被滥用或攻击。

> 可以为一个应用创建多个访问凭据，以实现交付给不同的用户或开发者。

> 这意味着 API 的使用者虽然使用了应用开发者提供的 AI 能力，但背后的 Promp 工程、知识库和工具能力是经封装的。

#### 2.2.2文本生成型应用

> **可用于生成高质量文本的应用。**

> 例如生成文章、摘要、翻译等，通过调用 completion-messages 接口，发送用户输入得到生成文本结果。

用于生成文本的模型参数和提示词模板取决于开发者在 Dify 提示词编排页的设置。

可以在应用 -> 访问 API 中找到该应用的 API 文档与范例请求。

例如，创建文本补全信息的 API 的调用示例：

```
curl --location --request POST &#x27;https://api.dify.ai/v1/completion-messages&#x27; \
--header &#x27;Authorization: Bearer ENTER-YOUR-SECRET-KEY&#x27; \
--header &#x27;Content-Type: application/json&#x27; \
--data-raw &#x27;{
    &quot;inputs&quot;: {},
    &quot;response_mode&quot;: &quot;streaming&quot;,
    &quot;user&quot;: &quot;abc-123&quot;
}&#x27;
```

```
import requests
import json

url = 

headers = {
    &#x27;Authorization&#x27;: &#x27;Bearer ENTER-YOUR-SECRET-KEY&#x27;,
    &#x27;Content-Type&#x27;: &#x27;application/json&#x27;,
}

data = {
    &quot;inputs&quot;: {&quot;text&quot;: &#x27;Hello, how are you?&#x27;},
    &quot;response_mode&quot;: &quot;streaming&quot;,
    &quot;user&quot;: &quot;abc-123&quot;
}

response = requests.post(url, headers=headers, data=json.dumps(data))

print(response.text)
```

#### 2.2.3对话型应用

> 可用于大部分场景的对话型应用，采用一问一答模式与用户持续对话。

> 要开始一个对话请调用 chat-messages 接口，每次对话开始都会产生出新的 conversation\_id，通过该conversation\_id 传回接口就可继续保持该会话。

> 重要说明: Service API 不共享 WebApp 创建的对话。通过 API 创建的对话与 WebApp 界面中创建的对话是相互隔离的。

#### 2.2.4conversation_id 的注意事项：

+   **生成 ****conversation_id****：**

> 开始新对话时，请将 conversation_id 字段留空。系统将生成并返回一个新的 conversation_id，未来的交互中会使用该 conversation_id 继续对话。

>

+   **处理现有会话中的 ****conversation_id****： **

> 生成 conversation_id 后，对 API 的未来调用应包含此 conversation_id，以确保与 Dify 机器人的对话连续性。传递上一个 conversation_id 时，将忽略任何新的 inputs，仅处理正在进行的对话的 query。

>

+   **管理动态变量： **

> 如果在会话期间需要修改逻辑或变量，你可以使用会话变量（特定于会话的变量）来调整 bot 的行为或回应。

**发送对话信息的 ****chat-messages**** API的调用：**

> 可以在应用 -> 访问 API 中找到该应用的 API 文档与范例请求。

```
curl --location --request POST &#x27;https://api.dify.ai/v1/chat-messages&#x27; \
--header &#x27;Authorization: Bearer ENTER-YOUR-SECRET-KEY&#x27; \
--header &#x27;Content-Type: application/json&#x27; \
--data-raw &#x27;{
    &quot;inputs&quot;: {},
    &quot;query&quot;: &quot;eh&quot;,
    &quot;response_mode&quot;: &quot;streaming&quot;,
    &quot;conversation_id&quot;: &quot;1c7e55fb-1ba2-4e10-81b5-30addcea2276&quot;,
    &quot;user&quot;: &quot;abc-123&quot;
}&#x27;
```

```
import requests
import json

url = &#x27;https://api.dify.ai/v1/chat-messages&#x27;
headers = {
    &#x27;Authorization&#x27;: &#x27;Bearer ENTER-YOUR-SECRET-KEY&#x27;,
    &#x27;Content-Type&#x27;: &#x27;application/json&#x27;,
}
data = {
    &quot;inputs&quot;: {},
    &quot;query&quot;: &quot;eh&quot;,
    &quot;response_mode&quot;: &quot;streaming&quot;,
    &quot;conversation_id&quot;: &quot;1c7e55fb-1ba2-4e10-81b5-30addcea2276&quot;,
    &quot;user&quot;: &quot;abc-123&quot;
}

response = requests.post(url, headers=headers, data=json.dumps(data))

print(response.text)
```

# 二、搭建复杂业务流程管理的对话流（Chatflow）

## 1.搭建 AI 应用

> 创建一个对话流（Chatflow）AI 电商客服助手。

> Chatflow 专注于流程的结构化和引导性，具备强大的流程编排能力，非常适合处理复杂的业务流程和多步骤任务。

1.  **创建知识库。**

> 使大模型能够获取专属业务知识，从而提供更精准的回答。若知识库已创建则可跳过本步骤。

1.  打开浏览器，进入 Dify 平台。

1.  单击 Dify 平台顶部**知识库**，接着单击下方的**创建知识库**，

1.  选择数据源为**导入已有文本**，随后选择本地文件，最后单击**下一步**。

1.  **设置文本分**段与清洗。按如下配置进行设置后，单击保存并处理。未提及配置使用默认值即可。

1.  创建"查询订单信息"自定义工具。

> 使大模型能够通过工具调用获取实时订单数据。若"查询订单信息"已创建则可跳过本步骤。

1.  打开浏览器，进入 Dify 平台。

1.  单击Dify 系统页面顶部**工具**，接着单击下方的**自定义 > 创建自定义工具**。

1.  在弹出的**创建自定义工具**面板中，填写如下信息，然后单击**保存**。未提及配置使用默认值即可。

```
openapi: &quot;3.0.0&quot;
info:
  version: 1.0.0
  title: 订单API
servers:
  - url: http://&lt;ECS 公网 IP&gt;
paths:
  /order:
    get:
      summary: 获取订单详情信息
      operationId: 查询订单信息
      parameters:
        - name: orderid
          in: query
          required: true
          schema:
            type: string
          description: 订单唯一标识
      responses:
        &#x27;200&#x27;:
          description: 获取成功
          content:
            application/json:
              schema:
                type: object
                properties:
                  code:
                    type: integer
                  data:
                    type: object
      security:
        - bearerAuth: []
components:
  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
```

1.  创建"创建退款申请"的自定义工具。

> 使大模型能够通过工具调用来创建退款申请。

1.  单击Dify 系统页面顶部**工具**，接着单击下方的**自定义 > 创建自定义工具**。

1.  在弹出的**创建自定义工具**面板中，填写如下信息，然后单击**保存**。未提及配置使用默认值即可。

```
openapi: &quot;3.0.0&quot;
info:
  version: 1.0.0
  title: 退款API
servers:
  - url: http://&lt;ECS 公网 IP&gt;
paths:
  /refund:
    post:
      summary: 创建退款申请
      operationId: 创建退款申请
      requestBody:
        required: true
        content:
          application/x-www-form-urlencoded:
            schema:
              type: object
              properties:
                orderid:
                  type: string
                  description: 订单唯一标识
                reason:
                  type: string
                  description: 退款原因
      responses:
        &#x27;200&#x27;:
          description: 创建成功
          content:
            application/json:
              schema:
                type: object
                properties:
                  code:
                    type: integer
                  refund:
                    type: object
      security:
        - bearerAuth: []
components:
  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
```

## 2.导入 DSL 创建

1.  单击 Dify 系统页面顶部工作室。

> 然后单击**导入 DSL 文件****，在**弹出框中选择 URL，并单击创建。

![](img/645acb1469c51f1c2027ae56aa1786f5.png)

1.  在编排页面，按以下步骤操作修改节点并为节点配置自定义工具。

1.  **设置完成后，页面显示如下：**

![](img/becc582c393046db3151c1223d007c3d.png)

1.  **调试应用。**

> 单击右上方的**预览** ，在**预览**面板下的对话框中输入问题，然后单击，即可调试对话效果。

1.  **发布应用。**

> 单击右上方的**发布 > 发布更新**，然后单击**运行**。系统将打开一个新页面，该页面的地址即为应用发布后的访问地址。

1.  **测试对话效果。**

> 按照页面提示开始对话，体验与 AI 应用对话功能。

+   以下问题在通过知识库检索到专属内容后，由大模型进行了推理并作出回复。

![](img/7c8c6702ba68b8370406dc7213fe9d56.png)

+   以下问题在调用“查询订单信息”工具后，由大模型进行了推理并作出回复。

![](img/a0d14fdc9072fc02b2add0e1bbf21845.png)

+   以下问题在调用“查询订单信息”和“创建退款申请”工具后，由大模型进行了推理并作出回复。

![](img/5f96c6daaff345a8f4c1ab7f859d92ac.png)