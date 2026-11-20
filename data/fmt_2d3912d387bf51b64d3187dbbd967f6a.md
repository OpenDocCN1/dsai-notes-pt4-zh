
# 常见问题

> 来源：https://ai.feishu.cn/docx/OscId5YzDosr4Xxdaf0cjxQrn1c

## 参数问题

1. 为什么`top_p`不能调整为 1

答：模型侧未开放1的能力

1. `temperture`不能是 0

答：模型侧未开放0的能力，`do_sample`如为`False`的时候，亦可达到模型结果稳定输出的目的（参数`top_p` `temperture`不生效）

1. 使用`CogView`的时候，为什么`n`、`size`、`quality`等参数。

答：`size` `n` `quality` 这个几个参数目前没有开放（模型侧在迭代完善中）

近期会升级到8k

1. 免费额度和在线额度的并发没有说明

在迭代中

1. 模型定价文档缺，画图，`embedding`都没有在最新的`API`中显示

在迭代中

## 计费问题

1. `Token` 和文字的比例是？

答：纯文字计算方式中，英文: 约 1 : 1.6，中文：1 : 1

2. `Function Call` 怎么叠加的计费

答：不单独收费、仅根据实际输入+实际输出到`token`计费

1. 历史记录怎么计算的

答：加入输入中一起计算

1. 多模态`Token`怎么计算的，文生图`token`怎么计算的

待产品确认公布

## 代码问题

1. 在哪里可以尝试一些基础的代码调用

答：非官方代码

```
https://github.com/zRzRzRzRzRzRzR/glm-cookbook
```

1. 觉得模型的回答有问题，在哪里可以

## 提出问题

1. 哪里可以提出问题

1. 在非官方代码提出建议和改进意见

```
https://github.com/zRzRzRzRzRzRzR/glm-cookbook
```

1. 觉得模型的回答有问题，怎么请求支持

```
在模型回答中，会有这么一组数据
Completion(model=&#x27;glm-4&#x27;, created=1705934286, choices=[CompletionChoice(index=0, finish_reason=&#x27;stop&#x27;, message=CompletionMessage(content=&quot;Why don&#x27;t scientists trust atoms?\n\nBecause they make up everything!&quot;, role=&#x27;assistant&#x27;, tool_calls=None))], request_id=&#x27;8313808155312936357&#x27;, id=&#x27;8313808155312936357&#x27;, usage=CompletionUsage(prompt_tokens=9, completion_tokens=15, total_tokens=24))
```

其中，`id`字段是反馈的途径，目前没有工单反馈（即将推出），您可以在这里先暂时提出问题，并在之后工单系统完成之后提交到工单。

```
https://github.com/zRzRzRzRzRzRzR/glm-cookbook
```
