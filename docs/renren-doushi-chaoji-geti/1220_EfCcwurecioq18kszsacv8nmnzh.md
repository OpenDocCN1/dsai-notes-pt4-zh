
# 2023-10-12 Dify.AI 上线数据集 API 功能：更高效的数据管理

> 来源：[https://xuqiwei1986.feishu.cn/docx/LUledCD7gongMNx71MLcvK8unab](https://xuqiwei1986.feishu.cn/docx/LUledCD7gongMNx71MLcvK8unab)

> 🔗 原文链接：https://mp.weixin.qq.com/s/xl9NABInnNY-wviTsMKZBw

原创 Dify.AI Dify

2023-10-12 11:14:31

精确发文时间由壹伴提供

2023-10-12 11:14 发表于 江苏

手机阅读

![](img/1432ff245be0fe5cff257d5a2b5dd47a.png)

Dify 的朋友们，

我们很高兴地向大家宣布，Dify.AI 现已推出全新的数据集 API 功能！

数据集 API 服务是一个可以高效管理和利用数据文档的工具。通过 Dify 数据集 API 功能，您可以轻松上传、实时更新和有效管理数据集。并与大型模型紧密集成，进一步增强了使用体验，大幅提高使用效率。同时，我们还提供了示例来帮助大家快速了解并上手实践。

## 如何使用数据集 API 功能？

进入数据集页面，你可以在左侧的导航中切换至 API 页面。在该页面中你可以查看 Dify 提供的数据集 API 文档，并可以在 API 密钥中管理可访问数据集 API 的凭据。

![](img/ac956cf915ed8453392c669d92f25c53.png)

## 数据集 API 调用示例

### 创建空数据集

该方法可供创建一个空数据集。

```
curl --location --request POST 'https://api.dify.ai/v1/datasets' \
--header 'Authorization: Bearer {api_key}' \
--header 'Content-Type: application/json' \
--data-raw '{"name": "name"}'
```

### 数据集列表

通过指定页码和返回条数查询数据集列表，进行数据集的管理和选择。

```
curl --location --request GET 'https://api.dify.ai/v1/datasets?page=1&limit=20' \
--header 'Authorization: Bearer {api_key}'
```

### 通过文本创建文档

通过简单的文本上传接口，轻松导入现有的文本 txt 数据。

```
curl --location --request POST '<https://api.dify.ai/v1/datasets/<uuid:dataset_id>/document/create_by_text>' \
--header 'Authorization: Bearer {api_key}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name": "Dify",
    "text": "Dify means Do it for you...",
    "indexing_technique": "high_quality",
    "process_rule": {
        "rules": {
            "pre_processing_rules": [
                {
                    "id": "remove_extra_spaces",
                    "enabled": true
                },
                {
                    "id": "remove_urls_emails",
                    "enabled": true
                }
            ],
            "segmentation": {
                "separator": "###",
                "max_tokens": 500
            }
        },
        "mode": "custom"
    }
}'
```

### 通过文件创建文档

文件上传功能还提供了别的文件格式如：markdown, md, pdf, html, htm, xlsx, docx, csv，大大丰富了选择性。

```
curl --location POST 'https://api.dify.ai/v1/datasets/{dataset_id}/document/create_by_file' \
--header 'Authorization: Bearer {api_key}' \
--form 'data="{
        "name": "Dify",
        "indexing_technique": "high_quality",
        "process_rule": {
            "rules": {
                "pre_processing_rules": [
                    {
                        "id": "remove_extra_spaces",
                        "enabled": true
                    },
                    {
                        "id": "remove_urls_emails",
                        "enabled": true
                    }
                ],
                "segmentation": {
                    "separator": "###",
                    "max_tokens": 500
                }
            },
            "mode": "custom"
        }
    }";    type=text/plain' \
--form 'file="@/path/to/file"'
```

### 获取文档嵌入状态（进度）

查看数据处理的实时情况，保证数据准备的及时性和准确性。

```
curl --location --request GET 'https://api.dify.ai/v1/datasets/{dataset_id}/documents/{batch}/indexing-status' \
--header 'Authorization: Bearer {api_key}'
```

### 删除文档

提供了便捷的文档管理功能，可以按需删除不需要的文档，保持数据集的整洁和有效。

```
curl --location --request DELETE 'https://api.dify.ai/v1/datasets/{dataset_id}/documents/{document_id}' \
--header 'Authorization: Bearer {api_key}'
```

### 数据集文档列表

提供了方便快捷的查询接口，轻松掌握数据集中所有文档的基本信息。

```
curl --location --request GET 'https://api.dify.ai/v1/datasets/{dataset_id}/documents' \
--header 'Authorization: Bearer {api_key}'
```

### 新增文档分段

分段功能提供了灵活的文档结构调整方式，有助于更好地组织和理解文档内容，提高数据的可用性和价值。

```
curl 'https://api.dify.ai/v1/datasets/aac47674-31a8-4f12-aab2-9603964c4789/documents/2034e0c1-1b75-4532-849e-24e72666595b/segment' \
  --header 'Authorization: Bearer {api_key}' \
  --header 'Content-Type: application/json' \
  --data-raw '{"segments":[{"content":"Dify means Do it for you","keywords":["Dify","Do"]}]' \
  --compressed
```

## 其他特性

同时，在这次更新中，我们还优化了一些其他小特性，为大家带来了更流畅的用户体验，一起来看看吧！

### 更多可供选择的 Embedding 模型

Dify 云端版本现已支持接入托管在 Hugging Face 上的开源 Embedding 模型，为大家提供了更广泛的选择。通过切换和测试不同模型性能，你可以找到与特定应用场景下契合度最高，效果最好的 Embedding 模型。

点击「设置」-->「模型供应商」进入配置页面后，在「Hugging Face」中选择添加 Embedding 模型：

![](img/d3ecee3bbaf937c0fee1c95a52b9fab7.png)

在输入 API key 和模型名称后即可成功部署并选择相应的 Embedding 模型：

![](img/a606e24c57698c43beb8b15e902b072a.png)

### 接入 GPT-3.5-turbo-instruct

Dify 集成了 OpenAI 新推出的 GPT-3.5-turbo-instruct 模型，该模型是改善用户与模型交互的一大飞跃。它经过训练解决了旧模型存在的问题，能深入理解并执行用户指令并给出更清晰、更切题的答案，因此用途更加广泛。

在「提示词编排」页面即可轻松选择调用。

![](img/9769b76294ef4c1df61312142c6950f8.png)

### 上下文支持绑定「查询变量」

上下文支持绑定「查询变量」，在输出时将根据变量绑定的上下文信息生成回答。

![](img/ebfe7b61bea6c75fb9795b9ad08b806e.png)

### 文本应用批量处理增加重试处理错误信息

文本应用批量处理增加了重试处理错误信息，可以更清晰的看到错误提示。

更多特性详见：

https://github.com/langgenius/dify/releases

## 往期产品更新 & 最佳实践

[Dify 全面接入智谱模型，免费获取 ChatGLM 1000万 tokens 额度](http://mp.weixin.qq.com/s?__biz=Mzg5MDkyOTY3NA==&mid=2247485079&idx=1&sn=59c7dc5212f57f0dd8191482a5711502&chksm=cfd451f2f8a3d8e4e508fcdf056d75fafbdb5fbf9a9510171bd9b86ea1771bfef9e0d873145c&scene=21#wechat_redirect)

Dify.AI × 星火 2.0，助力 LLM 的业务性应用开发

如何在几分钟内创建一个带有业务数据的官网 AI 智能客服？

## 如果你喜欢 Dify，欢迎：

- 在 GitHub 上贡献代码，与我们一同共建更好的 Dify；
- 通过线上线下活动、社交媒体向你的朋友们分享 Dify，以及你的使用经验；
- 在 GitHub 上给我们点亮⭐️

你也可以联系 Dify 小助手，加入我们的好友群聊，相互分享使用经验：

![](img/9d8ddce7049edb5443e272a5d9584a40.png)
