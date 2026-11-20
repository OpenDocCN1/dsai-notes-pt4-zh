
# GPT-4o新版本突然上线：丝滑解决9.11和9.9谁大，更强也便宜了

> 来源：[https://agijuejin.feishu.cn/docx/PumrdfPYGordgUxkFMNcBh7eneh](https://agijuejin.feishu.cn/docx/PumrdfPYGordgUxkFMNcBh7eneh)

> 明敏 发自 凹非寺

> 量子位 | 👸号 QbitAI

`GPT-4o`新版本突然上线，更强更便宜。

能力全方位提升，ZeroEval基准测试直接跃居第一。

![](img/f8fa2383d929aa00ad9cffc8259e445b.png)

输入和输出分别节省50%、33%。

四舍五入就是API降价啊。

![](img/13a8c1babdf527862b6ef29517f596e7.png)

token输出扩展到16k，此前支持4k。

9.11和9.9谁大这个问题，也能丝滑解决。

![](img/21ea492b735afa604edf0cf8da962381.png)

这是因为OpenAI给API中引入了结构化输出。

通过JSON模式确保模型输出符合开发者定义的结构，能让模型变得更可靠安全。

最新版模型`GPT-4o-2024-08-06`在JSON模式评估中，得分100%。

相比之下，去年6月的版本得分还低于40%。

![](img/95fbb525fb3831625c5879d15cd38a57.png)

在代码编辑方面，最新版本的基准得分和此前版本相同，但是现在成本只有原来的一半。

![](img/79b431481ab888f8754fa07d913039c6.png)

还有人发现，这个新版本是OpenAI在图像输入上最便宜的型号。

![](img/d93a6a53742f6455bba4e35a1925d4e6.png)

## API支持结构化输出

去年OpenAI DevDay上，官方正式给`GPT-4`引入了JSON模式，可以帮助开发者构建应用程序。

尽管JSON模式能提高模型可靠性，但是它并不能保证模型生成结果符合特定模式。

因此在这次更新中，OpenAI在API中引入结构化输出，可以确保模型生成内容和开发者提供的JSON模式完全匹配。

主要形式有两种：

1. 函数调用：通过在函数定义中设置`strict:true`来调用结构化输出。

   此功能适用于所有模型型号，包括`gpt-4-0613`和`gpt-3.5-turbo-0613`等。当结构化输出被启用时，模型输出将匹配提供的工具定义。

2. `response_format`参数新选项。

   开发者可以通过`json_schema`（`response_format`参数的新选项）提供JSON模式。

   当模型不是调用工具，而是以结构化的方式响应用户时，这是有用的。该功能适用于`gpt-4o-2024-08-06`和`gpt-4o-mini-2024-07-18`。当`response_format`使用`strict: true`提供时，模型输出将与提供的模式匹配。

OpenAI更新了`Python`和`Node` SDK，支持本地的结构化输出功能。

此外，还强调了安全输出。新模式将遵循现有的安全规则，并引入了`refusal`字符串值来帮助开发者识别模型拒绝`unsafe`请求的情况。

结构化输出还可以用于动态生成用户界面、分离最终答案与推理过程、从非结构化数据中提取结构化数据等多种用例。

![](img/d1f790d0685c373b9d05af37cf96a72b.png)

最后在价格方面，输入价格为2.5美元/100万token，输出价格为10美元/100万token。

由于新模型在输入和输出上能更节省，四舍五入也是降价了。

![](img/95cad9396c55eccab3464684e1a0c4b8.png)

有人发现，这个新模型的输出没有上一版本那么冗长，成本上更可控。

![](img/62d837ab7061ad0bc01ae8de9aeeca5a.png)

参考链接：

`openai.com/index/intro…`
