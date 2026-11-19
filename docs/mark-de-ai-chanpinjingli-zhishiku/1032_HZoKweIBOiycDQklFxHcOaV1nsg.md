
# Dify中使用智谱扣费较快问题

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/NeRDdvJ8eoWNqixRUWjc3cuinuc](https://qqs7y1hozd1.feishu.cn/docx/NeRDdvJ8eoWNqixRUWjc3cuinuc)

工作流中只使用到了`glm-4`和`embedding-3`模型，在智谱后台查看费用详情，在调用`glm-4`和`embedding-3`模型的同时还调用了`glm-4-plus`和`text-embedding`模型。导致扣费较快。

## 解决方法

在系统模型中不要设置默认的推理和`embedding`为`glm-4-plus`和`text-embedding`。

![](img/ac6338c93f94f68571e461a32f801b18.png)
