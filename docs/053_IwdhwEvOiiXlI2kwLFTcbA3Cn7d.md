# API接口开发注意事项

> 来源：[https://hyperspace.feishu.cn/docx/TGZGdGRqwoOgnYxovB9cpDNvnEb](https://hyperspace.feishu.cn/docx/TGZGdGRqwoOgnYxovB9cpDNvnEb)

+   接口支持 8 种 HTTP 协议，支持 restFul 风格的 HTTP 接口；

+   入参支持 body 传参，path 传参，header 传参（同名参数将不做位置区分），当前不支持无参接口触发；

+   当前api接口支持如下三种媒体类型；

+   application/json json类型

+   multipart/form-data 表单类型

+   application/x-www-form-urlencoded 媒体表单

+   API 接口返回仅支持在body中返回 json 数据，支持 sse 流式返回内容，API 流式返回时，模型只认为最后一条结果为最终结果前面返回的数据将被忽略，强烈建议将 json 格式化为一行返回，多行返回将会被解释为流式返回，导致内容不全；

+   prompt、errCode、errMsg、actionName、actionContent 为保留字段；

+   prompt：prompt 字段将会作为模型的回复规范进入大模型

+   errCode：返回此字段不为0时，系统将认为 API 调用失败

+   errMsg：暂无

+   actionName：此字段将在前端界面显示前部分内容

+   actionContent：此字段将在前端界面显示后部分内容