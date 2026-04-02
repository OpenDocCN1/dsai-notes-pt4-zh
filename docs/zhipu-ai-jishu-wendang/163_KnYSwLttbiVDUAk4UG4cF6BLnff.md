# 计费说明和使用限制

> 来源：[https://ai.feishu.cn/docx/DQf7dm3mLompe6xL3pbcGxaVnuh](https://ai.feishu.cn/docx/DQf7dm3mLompe6xL3pbcGxaVnuh)

## 计费单元

一般情况下ChatGLM模型中token和字数的换算比例约为1:1.8，但因为不同模型的分词不同，所以换算比例也存在差异，每一次实际处理token数量以模型返回为准，您可以从返回结果的usage中查看。

## 计费范围

我们将根据模型输入和输出的总token数进行计量计费（向量大模型embedding-2仅按照输入token量计费），其中如果您开启搜索服务，搜索结果作为输入也会进行计量计费。

搜索可以为模型引入更多相关信息，有助于提高输出的质量、及时性等；实际执行时我们会依据策略决定当前请求是否执行搜索，且执行后只选择搜索结果中最相关的部分输入模型，在提高效果的同时避免给您造成浪费；如果您确实不需要引入搜索结果，可以通过传参关闭。

## 扣减方式

我们支持下列2种方式扣减：

具体以资源包购买页的购买须知为准

## 产品价格

本页面仅展示 API 中模型的定价，如果您使用[的模](https://open.bigmodel.cn/pricing/?utm_campaign=open&_channel_track_key=OWTVNma9)型不在 API 模型列表中，请查看 这里

## 查看账单

有关费用账单请前往[开发工作台](https://open.bigmodel.cn/overviewhttps://open.bigmodel.cn/pricing/?utm_campaign=open&_channel_track_key=OWTVNma9)查询，资源包使用情况前往[资源包管理](https://open.bigmodel.cn/usercenter/resourcepackhttps://open.bigmodel.cn/pricing/?utm_campaign=open&_channel_track_key=OWTVNma9)查看

## 扣费示例

暂未提供