# 常见问题

> 来源：[https://hyperspace.feishu.cn/docx/Sz7Sdc2yRoo5e7x5ZyacyUUMnLg](https://hyperspace.feishu.cn/docx/Sz7Sdc2yRoo5e7x5ZyacyUUMnLg)

## 一、如何判断我的插件定位是否合适？

大模型插件的概念，是给大模型提供了很多工具，使大模型有了除自然语言生成和理解以外的额外能力。

如：

+   实时信息：获取天气、股票等；

+   私域知识库：提供企业内部的规章制度、特定领域的文献和资料等；

+   记忆增强：单词本、日程管理等记忆能力；

+   复杂计算：公积金计算、科学计算器等复杂计算能力。

而模型本身就具备的能力不适合做成插件，因为随着模型能力的增强，插件的功能模型自己就可以完成。

不适合的插件如：

+   人物模拟：模拟成心理医生，帮我解决心理问题（可以通过给模型添加人设来解决）

+   文本理解与摘要：输入文章，解析文章中的内容（模型本身具备自然语言理解能力）

+   文本生成：输入一个主题，生成一个诗歌或文章（模型具备文本生成能力）

+   公域知识检索：如何养猫如何养狗等互联网上资料相对较多的领域

+   翻译等理解与生成相关的主题

## 二、ai-plugin.json 中的字段会怎么影响我的插件？

+   name_for_human：给用户看到的插件名称，必须唯一

+   name_for_model：给模型看到的唯一标识，必须唯一

+   description_for_human：给用户看到的描述信息，仅做展示

+   description_for_model：提供给模型的描述信息，会作为召回插件的重要依据

具体字段格式要求，可以参考：[构思插件 manifest 描述文件](https://agents.baidu.com/docs/develop/ability-plugin/basic/develop_from_scratch/)（ ai-plugin.json，必选）

## 三、为什么我的插件召回率低？

模型决定是否插件召回，主要参考插件的描述信息。

会产生影响的描述信息主要有两部分：

+   ai-plugin.json 中的 description_for_model，作为插件整体的描述信息决定是否使用插件；

+   openapi.yaml 定义的接口 description (或 summary，同时存在优先使用 description )，决定了模型应该调用哪个接口来解决问题。

如果召回效果较差，建议调整这两个字段的描述信息来进行效果微调。

也可以使用 example.yaml 文件添加接口的调用示例。 但 example 文件不建议有太多示例，建议每个接口定义一个示例，一个 example 配置文件不要超过 300 个字符。

## 四、如何更好的编写描述信息？

+   首先我们要理解描述插件的定位：大模型可使用的工具集合。

+   那么插件的描述信息的目标是：为了向模型清晰描述插件的能力范围和使用场景。

+   以单词本添加单词接口为例，我们不需要将执行过程告诉模型，我们仅需要将功能描述清楚即可：

+   错误情况❌：从用户的 query 中提取英文单词，保存到 mysql 存储里。

+   正确情况✅：添加单词到单词本。 或者更详细的，可以将能力边界说明。如：添加单词到单词本，包括但不限于英语单词、日语单词、法语单词等，但不要直接添加汉语单词。

## 五、模型生成参数格式不准确怎么办？

模型在生成参数的过程中同样会有一些幻觉，不能依靠参数中定义的 required 字段保证一定有参数，需要有对应的兜底逻辑。 此外，参数的拆分尽量可简单描述。 如：

想让模型获取 query 中的时间，如果直接要求可能格式模型是无法保证的，可能是 2023-12-01 可能是 2023年12月01日

可以拆分成多个参数，如 年、月、日 分别使用数字让模型填充，稳定性会高很多。

## 六、模型润色效果怎么优化？

优化结果有一种方式：

1.  无润色，直接输出结果可参考章节：如何设置大模型直接输出插件返回结果

通过在召回结果中返回 prompt 字段让模型来对结果进行润色。需要注意的是：

1.  prompt 是默认字段，无需在 openapi.yaml 中配置

1.  prompt 是对模型润色结果的要求，可以命令的口吻要求模型对内容优化

1.  可以使用 重点描述 的 markdown 语法强调要求。

使用 x-return-raw 无润色需要注意：

1.  插件调用一定要：等待接口全部返回后统一进行处理，x-return-raw 的返回方式和 sse 返回结合也不能加快直接输出文本的时间速度。

## 七、响应时间太长怎么办？

如果接口的处理时间的确太长无法优化，如生成文件、处理图片等，则需要使用 sse 的方式返回结果。

openapi.yaml 文件 我们需要在返回协议中标记 ContentType 为 text/event-stream

```
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
                        text/event-stream: # 《==== 返回协议需要是text/event-stream
                            schema:
                                $ref: &quot;#/components/schemas/messageResponse&quot;
```

sse 的返回格式示例如下：

```
data: {&quot;message&quot;: &quot;等待&quot;}

data: {&quot;message&quot;: &quot;等待&quot;}

data: {&quot;message&quot;: &quot;单词添加成功&quot;}
```

注意事项：

+   3s 内必须返回第一包

+   可以有 event: xxx 的消息类型返回，也可以省略

+   建议每 2s 返回一包，直到生成完成，不要频繁发包

+   插件只会使用最后一包结果，所以最后一包结果必须是全量数据，中间的数据包会丢弃（特殊情况 使用 msg_content 展示生成过程）

+   不能结合 x-return-raw 实现插件结果 sse 展示