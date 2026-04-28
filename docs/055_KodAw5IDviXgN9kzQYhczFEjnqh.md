# 如何设置大模型直接输出插件返回结果

> 来源：[https://hyperspace.feishu.cn/docx/Fnh1daMCxodAjQxarxpcWrBrnAb](https://hyperspace.feishu.cn/docx/Fnh1daMCxodAjQxarxpcWrBrnAb)

## 一、文档说明

本文档针对 openapi.yaml 升级相关扩展规范而制定。 依据该规范，开发者可以更灵活的设置模型对于插件 API 的 Json 输出结果的润色逻辑，以满足不同场景下对于 Json 转自然语言的差异性需求，可以支持模型返回 Json 结果中指定字符串直接输出。

## 二、规范使用说明

扩展规范写法：在 Operation 对象级增加扩展字段。

```
x-return-raw:
    description: 标记当前 API 请求的结果是否经过大模型，当 name 的值不空时，从插件结果中获取具体字段，类型必须 String，直接返回，而不经过大模型。
    mediaType: application/json
    name: this_is_a_field_name
    type: string
```

具体规范字段位置： 整体放置于接口定义下的 post 节点下，与 operationId、requestBody 等字段并列。

规范字段含义说明：

+   x-return-raw 为配置当前 API 不经过 EB 大模型的配置根节点，名字固定。下面包括 4 个成员。分别如下：

+   description: 非空的字符串 - 不可缺少，内容不空，填写本规范的相关描述信息。其中“标记当前 API 请求的结果是否经过大模型，当 name 的值不空时，从插件结果中获取具体字段，类型必须 String ，直接返回，而不经过大模型。” 为示范。

+   mediaType: application/json - 固定写法，其含义是目前的规范仅仅在 metaType 所设定的 API 接口上生效，不支持其他值。

+   type: String - 固定写法，其含义是 return-raw 所获取的字段的类型必须是 String ，不支持多级嵌套的 Json 字段结构。

+   name: 非空的字符串 - 其含义是 从 API 接口所返回 responses 字段中，根据此字段获取接口的原始信息，最终返回给用户。

+   代码示例

```
paths:
    /this/is/a/post/api
        post:
            operationId: xxxx
            summary: this is operation xxxx
            requestBody:
              content:
                &#x27;application/json&#x27;:
                  schema:
                    properties:
                      field_str1:
                        description: this is field_str1
                        type: string
                      field_int1:
                        description: this is field_int1
                        type: int
            responses:
                &quot;200&quot;:
                    description: 接口执行成功
                    content:
                        application/json:
                            schema:
                                $ref: &quot;#/components/schemas/xxxxResponseOK&quot;

            x-return-raw:
                description: 标记当前 API 请求的结果是否经过大模型，当 name 的值不空时，从插件结果中获取具体字段，类型必须 String，直接返回，而不经过大模型。
                mediaType: application/json
                name: this_is_a_field_name
                type: string
```