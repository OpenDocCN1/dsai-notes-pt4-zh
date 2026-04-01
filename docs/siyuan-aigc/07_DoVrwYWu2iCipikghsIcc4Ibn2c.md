# 进阶：API与速率限制

> 来源：[https://my.feishu.cn/docx/Q7s8d6ejToSbemxkeSqckSmlnbe](https://my.feishu.cn/docx/Q7s8d6ejToSbemxkeSqckSmlnbe)

# API与速率限制

# API

API（Application Programming Interface，应用程序编程接口）是一些预先定义的函数，目的是提供应用程序与开发人员基于某软件或硬件得以访问一组例程的能力。

# API 接入指南

ChatGPT API 接入需要如下条件。

1.

[已成功注册 OpenAI 的账号即可访问网站：h](https://platform.openai.com/account/api-keys)ttps://platform.openai.com/account/api-keys

2.

创建 API KEY，这个 API KEY 是用于 HTTP 请求身份验证的，可以创建多个。如下图点击“Create new secret key“，注意这个创建之后需要马上复制好保存，关闭弹框之后就看不到了。

1.官方 API 文档链接[https://platform.openai.com](https://platform.openai.com/docs/api-reference/)/docs/api-reference/。

2.注意 API 调用是收费的，但是 OpenAI 已经为我们免费提供了5美元的用量。

# 使用API的注意事项

为了避免速率限制对用户造成不必要的影响，建议用户在使用OpenAI的API时遵循以下几点：

1.遵守OpenAI的API使用政策和服务条款。

2.根据自身需求选择合适的API服务计划，以获得更高的速率限制。

3.通过优化请求参数和减少重复请求等方式，尽可能减少API的使用次数。

4.在进行大规模或高频次的API请求时，采用分布式或异步处理方式，以提高效率和并发性。

5.监控API使用情况和响应状态，及时调整请求速率和处理策略，避免出现错误和超时等问题。