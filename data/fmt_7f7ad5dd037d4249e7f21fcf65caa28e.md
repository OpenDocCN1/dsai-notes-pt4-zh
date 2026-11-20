
# 企业级稳定可部署的AI agent

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/VzxxdKUB4ouspHxKZ5dc2Htdnyb](https://qqs7y1hozd1.feishu.cn/docx/VzxxdKUB4ouspHxKZ5dc2Htdnyb)

## 前言

今天，我们将重点关注AI在实际应用中的落地表现，特别是Spring AI如何能够帮助企业实现功能优化以及推动AI与业务的深度融合。我们将以当前大厂广泛追逐的智能体赛道为切入点，探讨其在实际场景中的应用。考虑到许多同学可能已经接触过智能体，以这一主题作为讨论的基础，能够更有效地帮助大家理解相关概念和技术的实际操作与效果

因此，在本章节中，我们将以智能体为出发点，带领大家轻松实现一个本地稳定且可部署的智能体解决方案。在这一过程中，我将详细介绍每一个步骤，确保大家能够顺利跟上。此外，在章节的最后，我会根据我的理解，分析这一方案与现有智能体的优缺点，以帮助大家全面了解不同选择的利弊

## 准备工作

当然，Spring AI集成了许多知名公司的接口实现。如果你真的想使用OpenAI的接口，可以考虑国内的混元API。混元API兼容OpenAI的接口规范，这意味着你可以直接使用OpenAI官方提供的SDK来调用混元的大模型。这一设计大大简化了迁移过程，你只需将`base_url`和`api_key`替换为混元相关的配置，而无需对现有应用进行额外修改。这样，你就能够无缝地将您的应用切换到混元大模型，享受到强大的AI功能和支持

### 申请API KEY

大家完全不必担心，经过我亲自测试，目前所有接口都能够正常兼容，并且没有发现任何异常或问题

可以通过以下链接申请：[混元API申请地址](https://console.cloud.tencent.com/hunyuan/start?from_column=20421&from=20421)

![](img/81d779a4a7ffeae1260760db4c2790b3.png)

+ 请确保在您个人的账户下申请相关的API KEY

![](img/d27cbc06ec65cefb756d359fe08fa46e.png)

请务必妥善保存您的API KEY信息，因为在后续使用过程中，这一信息将会变得非常重要

### 对接文档

在这里，了解一些注意事项并不是强制性的，因为我们并不需要直接对接混元（Hunyuan）的接口。实际上，我们可以在Spring AI中直接使用兼容OpenAI的接口，这样能够大大简化我们的操作流程

> 如果您有兴趣深入了解相关的API文档，可以自行查找接口文档地址，里面有详尽的说明和指导：API接口文档

请大家特别注意：由于智能体在运行时需要调用相关的插件或工作流，因此支持函数回调的模型仅限于以下三个

这意味着，除了这三个模型之外，其他模型都不具备这一支持功嘛，请确保在选择模型时考虑这一点

![](img/79e75ad4ea54b242c9b324137f0bbc00.png)

请大家留意，目前混元尚未推出预付费的大模型资源包，用户只能进行并发包的预购

有关计费详情，请参见下方图示：

![](img/30acc995f344bdb511808a5e2f441632.png)

### 项目配置

接下来，我们将继续使用之前的Spring AI演示项目，并对其进行必要的修改。具体需要调整的 Maven POM 依赖项如下所示：

```
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-openai-spring-boot-starter</artifactId>
</dependency>
```

如图所示，在我第一次配置文件时选择了使用 functioncall 模型，因为它的成本相对较低。然而，后来我发现该模型在对系统提示词的识别上表现并不理想，后面我都换成了pro模型，大家可以根据自己的具体需求和预算做出相应的选择

> functioncall对提示词不敏感但是对函数回调的结果可以很好的解析，pro对提示词敏感但是函数回调的结果他不直接回答，一直输出planner内容但就是不回复用户。后面会有详细说明

![](img/3deb6522cff6a77066b93aedc9f4c1e5.png)

`application.properties` 文件用于全局配置，所有的 `ChatClient` 都会遵循这一设置。这样做的一个显著好处是，开发人员在代码层面无需进行任何修改，只需在 Maven 的 POM 文件中更改相应的依赖项，即可轻松切换到不同的 AI 大模型厂商。这种灵活性不仅提高了项目的可维护性，还方便了模型的替换与升级

## Spring AI 智能体构建

现在，假设你已经完成了所有的准备工作，我们可以开始构建属于自己的智能体。首先，我们将专注于单独定制配置参数。之前提到过，`application.properties` 文件是全局设置，适用于所有的 `ChatClient`，但每个模型实际上都有自己特定的领域和应用场景。因此，我们首先需要配置如何为每个接口进行个性化定制，以确保模型的表现更加贴合实际的业务需求

### 个性化配置模型

#### 普通调用

首先，让我们来观察在正常情况下代码应该如何编写：

```
@PostMapping("/ai-function")
ChatDataPO functionGenerationByText(@RequestParam("userInput") String userInput) {
    String content = this.myChatClientWithSystem
            .prompt()
            .system("你是努力的小雨，一名 Java 服务端码农，潜心研究着 AI 技术的奥秘。热爱技术交流与分享，对开源社区充满热情。")
            .user(userInput)
            .advisors(messageChatMemoryAdvisor)
            .functions("CurrentWeather")
            .call()
            .content();
    log.info("content: {}", content);
    ChatDataPO chatDataPO = ChatDataPO.builder().code("text").data(ChildData.builder().text(content).build()).build();;
    return chatDataPO;
}
```

如图所示，在我们发起请求之前，如果提前设置一个断点，我们就能够在这一时刻查看到 `chatOptions` 参数，这个参数代表了我们默认的配置设置。因此，我们的主要目标就是在发送请求之前，探讨如何对 `chatOptions` 参数进行有效的修改

![](img/c4f0af34cf9a8b2188fc41ee22d3d63d.png)

在对提示词进行测试的过程中，我们发现 functioncall 模型对于 system 提示词的响应效果并不显著，似乎没有发挥出预期的作用。然而，这个模型的一个显著优点是它支持函数回调功能（在前面的章节中已经详细讲解过），此外，与pro 模型相比，functioncall 模型的使用费用也相对较低，这使得它在某些情况下成为一个更具成本效益的选择

![](img/1e7b68982bbe1d26c3f8cd49d6e282ff.png)

#### 特殊调用

为了使模型的回复更加贴合提示词的要求，我们可以对模型进行单独配置。如果你希望对某一个特定方法进行调整，而不是采用像 `application.properties` 中的全局设置，那么可以通过自行修改相应的参数来实现。具体的配置方法如下所示：

```
//省略重复代码
OpenAiChatOptions openAiChatOptions = OpenAiChatOptions.builder()
        .withModel("hunyuan-pro").withTemperature(0.5f).build();
String content = this.myChatClientWithSystem
        .prompt()
        .system("你是努力的小雨，一名 Java 服务端码农，潜心研究着 AI 技术的奥秘。热爱技术交流与分享，对开源社区充满热情。")
        .user(userInput)
        .options(openAiChatOptions)
        .advisors(messageChatMemoryAdvisor)
//省略重复代码
}
```

+ 在此，我们只需简单地配置相关的选项即可完成设置。接下来，我们可以在断点的部分检查相关的配置，以确保这些设置已经生效并正常运行

![](img/67de245bcd05ebe5fce54ead3e8938f6.png)

+ 同样的写法，例如，我们之前设置的 pro 模型相比于 function-call 模型在处理系统提示词时显得更加友好

![](img/049c90681c45f9f88b6d400a30cf1dee.png)

### 思考路径

实际上，在绝大多数智能体中，这些思考路径并不会被显示出来，只有百度那边的智能体系统会将其呈现给用户。这些思考路径都是由大模型生成并返回的，因此我并没有在这里进行额外的配置。实际上，我们也可以选择返回这些路径，相关的源代码也在此处：

```
private void writeWithMessageConverters(Object body, Type bodyType, ClientHttpRequest clientRequest)
        throws IOException {

//省略代码
    for (HttpMessageConverter messageConverter : DefaultRestClient.this.messageConverters) {
        if (messageConverter instanceof GenericHttpMessageConverter genericMessageConverter) {
            if (genericMessageConverter.canWrite(bodyType, bodyClass, contentType)) {
                logBody(body, contentType, genericMessageConverter);
                genericMessageConverter.write(body, bodyType, contentType, clientRequest);
                return;
            }
        }
        if (messageConverter.canWrite(bodyClass, contentType)) {
            logBody(body, contentType, messageConverter);
            messageConverter.write(body, contentType, clientRequest);
            return;
        }
    }
//省略代码
}
```

如图所示，目前我们仅仅进行了简单的打印操作，并未实现消息转换器（message converter）

考虑到我们的业务系统并不需要将这些信息展示给客户，因此我们认为当前的实现方式已足够满足需求

![](img/b4ecb4eb0754e6c2fe56ad684921d67f.png)

### 配置插件

我之前在视频中详细讲解了智能体如何创建自定义插件。在这次的实践中，我们将继续利用百度天气插件来获取实时的天气信息。不过，与之前不同的是，这一次我们将把这一功能集成到Spring AI项目中

#### 数据库配置

每个业务系统通常都会配备自有数据库，以便更好地服务用户。为了演示这一点，我们将创建一个MySQL示例，具体内容是获取地区编码值，并将其传递给API进行调用。在这个过程中，你可以通过插件对数据库进行各种操作，但在此我们主要专注于查询的演示

本次示例中，我将继续使用腾讯云轻量应用服务器来搭建一个MySQL单机环境。在成功搭建环境后，我们将继续进行后续操作。请确保在开始之前，所有必要的配置和设置都已完成，以便顺利进行数据库的查询和API的调用

![](img/e7e07151982b187775053793d27bc29b.png)

以下是与相关配置有关的POM文件依赖项：

```
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.49</version>
</dependency>
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-spring-boot3-starter</artifactId>
    <version>3.5.7</version>
</dependency>
```

数据库连接配置信息如下：

```
spring.datasource.url=jdbc:mysql://ip:3306/agent?useSSL=false&serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
spring.datasource.username=agent
spring.datasource.password=password
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
```

#### 导入数据

我已经成功完成了百度地图提供的数据导入工作，具体情况请参见下图所示：

![](img/7932b6ead8d5b0169d9879623e5c4ce6.png)

#### 操作数据库

接下来，您只需在插件类内部直接进行数据库操作即可。关于 `SearchHttpAK` 实体类，您可以直接从百度地图提供的 Java SDK 中复制，无需额外说明。同时，请注意，`areaInfoPOMapper` 需要您在配置类中自行进行 Bean 注入，以确保其正常使用
