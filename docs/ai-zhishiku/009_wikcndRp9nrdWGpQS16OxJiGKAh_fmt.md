
# 6、OpenAI 相关问题

> 来源：[https://my.feishu.cn/docx/PJKZdu9fjosL81xeSuzcgCjGnwc](https://my.feishu.cn/docx/PJKZdu9fjosL81xeSuzcgCjGnwc)

## 如何注册 OpenAI 账号？

去 chat.openai.com 注册。你需要：

+   一个良好的🪜（OpenAI 支持地区原生 IP 地址）
+   一个支持的邮箱（例如 Gmail 或者公司/学校邮箱，非 Outlook 或 QQ 邮箱）
+   一个国外手机号（中国地区手机号不支持）

## 怎么开通 OpenAI API？怎么查询 API 余额？怎么看 API 是否被封

一键查询 OpenAI API 余额状态：https://openai.aigc369.com

## 我新注册的 OpenAI 账号怎么没有 API 余额？

（4 月 6 日更新）新注册账号通常会在 24 小时后显示 API 余额。当前新注册账号赠送 5 美元余额。

## 如何给 OpenAI API 充值？

OpenAI 只接受指定地区的信用卡（中国信用卡无法使用）。一些途径举例：

1. Depay 虚拟信用卡
2. 申请国外信用卡
3. 网上找人代充

## 如何使用 GPT-4 的 API 访问？

+   GPT-4 的 API 访问需要单独申请。到以下地址填写你的信息进入申请队列 waitlist（[准备好你的 OpenAI 组织 ID](https://openai.com/waitlist/gpt-4-api)）：https://openai.com/waitlist/gpt-4-api 之后等待邮件消息。
+   开通 ChatGPT Plus 不代表有 GPT-4 权限，两者毫无关系。

## 为什么我的 Token 消耗得这么快？

+   如果你有 GPT-4 的权限，并且日常在使用 GPT-4 API，那么由于 GPT-4 价格是 GPT-3.5 的 15 倍左右，你的账单金额会急速膨胀；
+   如果你在使用 GPT-3.5，并且使用频率并不高，仍然发现自己的账单金额在飞快增加，那么请马上按照以下步骤排查：
    - 去 OpenAI 官网查看你的 API key 消费记录。如果你的 token 每小时都有消费，并且每次都消耗了上万 token，那你的 key 一定是泄露了，请立即删除重新生成。不要在乱七八糟的网站上查余额。
    - 如果你的密码设置很短，比如 5 位以内的字母，那么爆破成本是非常低的，建议你搜索一下 Docker 的日志记录，确认是否有人大量尝试了密码组合，关键字：`got access code`
+   通过上述两个方法就可以定位到你的 token 被快速消耗的原因：
    - 如果 OpenAI 消费记录异常，但是 Docker 日志没有问题，那么说明是 API key 泄露；
    - 如果 Docker 日志发现大量 `got access code` 爆破记录，那么就是密码被爆破了。

## API 是怎么计费的？

OpenAI 网站计费说明：[https://openai.com/pricing#language-models](https://openai.com/pricing#language-models)

OpenAI 根据 token 数收费，1000 个 token 通常可代表 750 个英文单词，或 500 个汉字。输入（Prompt）和输出（Completion）分别统计费用。

## `gpt-3.5-turbo` 和 `gpt-3.5-turbo-0301`（或者 `gpt-3.5-turbo-mmdd`）模型有什么区别？

官方文档说明：[https://platform.openai.com/docs/models/gpt-3-5](https://platform.openai.com/docs/models/gpt-3-5)

+   `gpt-3.5-turbo` 是最新的模型，会不断得到更新。
+   `gpt-3.5-turbo-0301` 是 3 月 1 日定格的模型快照，不会变化，预期 3 个月后被新快照替代。
