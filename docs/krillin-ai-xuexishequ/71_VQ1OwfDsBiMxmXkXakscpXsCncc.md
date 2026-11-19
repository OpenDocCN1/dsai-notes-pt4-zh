
# Usage  使用

> 来源：`https://kcn9ut4m7icc.feishu.cn/docx/FYY5dMWFOoPYXSxb1XRcs2fDnUf`

Learn how Cursor handles fast and slow request pools, pricing, and monthly request allocation

了解 Cursor 如何处理快速和慢速请求池、定价以及每月请求分配

## Fast and Slow Requests  快速和慢速请求

There are two types of requests in Cursor, slow and fast that has its own pool

Cursor 中有两种请求类型，慢速和快速，各自拥有自己的池子

By default, Cursor servers try to give all users fast premium model requests. However, when users run out of fast premium credits, they are moved to a slow pool. Wait times in the slow pool are calculated proportionally to how many slow requests you’ve used, so they generally remain manageable unless you’re well over your fast request limit.

默认情况下，Cursor 服务器尝试为所有用户提供快速的 premium 模型请求。然而，当用户用完快速 premium 积分时，他们会被转移到慢速池。慢速池中的等待时间按您使用的慢速请求数量成比例计算，因此除非您远远超出快速请求限制，否则通常保持可管理。

To bypass wait times entirely, you can enable usage-based pricing (you’ll only be charged for requests beyond your included fast requests)

绕过所有等待时间，您可以选择启用基于使用量的计费（您只需为超出包含的快速请求的请求付费）

See [`models`](https://docs.cursor.com/settings/models) for an overview of which models are premium and their alternatives

查看模型以了解哪些模型是 premium 及其替代方案

## Included requests  包含请求

Every subscription includes a set amount of fast requests. See plans for details on how many fast requests your subscription includes.

每个订阅都包含一定数量的快速请求。查看计划以了解您的订阅包含多少快速请求。

## Additional requests  附加请求

We offer usage-based pricing for additional requests beyond your plan’s included quota:

我们为您提供超出计划包含配额的额外请求的按使用量计费

### Usage-based Pricing  基于使用量定价

You may opt in to usage-based pricing for requests that go beyond what is included in your Pro or Business plan from your [`dashboard`](https://docs.cursor.com/account/dashboard)

您可以在 [`仪表板`](https://docs.cursor.com/account/dashboard) 上选择为超出您的专业版或商业版计划包含范围的请求使用基于使用量的定价

Usage-based pricing is only available with a paid subscription.

基于使用量的定价仅适用于付费订阅。

From the dashboard, you can toggle usage-based pricing for premium models and other models (see [`models`](https://docs.cursor.com/settings/models) to understand which model is which). You can also configure a spend limit in USD to make sure you never go over that. Once the spending limit is hit, slow requests will be used.

从仪表板，您可以切换 premium 型号和其他型号的按使用量计费（查看模型了解哪个型号是哪个）。您还可以配置美元消费限额，以确保您不会超过该限额。一旦达到消费限额，将使用慢速请求。

We will bill for additional fast requests when you’ve made requests totaling $20, or on the 2nd or 3rd day of the month, whichever comes first.

我们将对额外的快速请求进行收费，当您的请求总额达到 20 美元时，或者在每月的 2 日或 3 日，以先到者为准。

#### Single invoice  单张发票

#### Multiple invoices  多张发票

For team accounts, administrators can restrict usage-based pricing settings to admin-only access

团队账户中，管理员可以限制基于使用量的定价设置仅对管理员开放

Cost per request for each model can be found on the [`models`](https://docs.cursor.com/settings/models) page

每个模型的请求成本可在模型页面找到

### Fast requests packages (deprecated) 快速请求包（已弃用）

Fast requests packages have been deprecated. Existing users with additional packages can continue to use them and have the option to remove them, but new packages cannot be purchased. If you are at the base amount of fast requests packages, the option to modify them will no longer be available in the dashboard.

快速请求包已被弃用。拥有额外包的现有用户可以继续使用它们，并可以选择删除它们，但无法购买新包。如果您处于快速请求包的基础数量，则无法在仪表板上修改它们的选项。

## FAQ  常见问题解答

### When do my fast requests reset? 我的快速请求何时重置？

Your Fast Requests reset on a fixed monthly date based on when you first set up your plan. If you purchase additional requests (for example, upgrading from 500 to 1000 requests), the reset date remains unchanged. For instance, if your plan started on the 23rd, your requests will always reset on the 23rd of each month, regardless of when you purchase additional requests.

您的快速请求将在您首次设置计划时确定的固定每月日期重置。如果您购买额外的请求（例如，从 500 个请求升级到 1000 个请求），重置日期保持不变。例如，如果您的计划从 23 日开始，您的请求将始终在每个月的 23 日重置，无论您何时购买额外的请求。

### What does “500 premium requests” mean for teams? “500 个高级请求”对团队意味着什么？

Each user gets their own quota of 500 fast requests for premium models per month. These requests are not pooled across the team - every team member gets their own fresh 500 requests when their personal monthly cycle resets.

每个用户每月可获得 500 次快速请求的配额，适用于高级模型。这些请求不会在整个团队中汇总 - 每位团队成员在他们的个人月度周期重置时，都会获得自己的 500 次新的请求。
