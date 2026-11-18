
# SDK公示的完整性

> 来源：[https://docs.feishu.cn/docx/X7PLdGVO9oFWyux769XcSy82nGc](https://docs.feishu.cn/docx/X7PLdGVO9oFWyux769XcSy82nGc)

- **问**：APP公示SDK，如何保障公示的完整性，比如APP自身集成的SDK，SDK嵌套的SDK，以及H5页面出现的SDK？

- **答1：**只公布自己**集成**的就好，其他没管了。

- **答2：**SDK嵌套的SDK有**接口调用**就有点尴尬，如果还超频就更尴尬。

- **答3：****最终责任都在APP**。所以现在做法是**集成一个新的SDK**，通过技术手段，管控所有SDK的行为。

- **追问：**请问现在业内有类似的实践吗？

- **答4：**这个就比较技术了。最笨的就是`hook`手段。实践也是有的，一些大点的公司自己做了，大概手段就是**管控SDK行为**，允许的做白名单放行，不允许的，如果偷偷拿用户设备信息，直接拦截或者返回一个假的。外面评测机构也有自己开发的。我理解梆梆，爱加密这种应该都有。

[SDK合规](https://cbe8t1r4a5.feishu.cn/wiki/wikcnBRo8jxV2MQwoTa63T6qfce)
