
# 加密传输要求

> 来源：[https://docs.feishu.cn/docx/O3eqdcQxso0xIaxC1x7cJ1mendh](https://docs.feishu.cn/docx/O3eqdcQxso0xIaxC1x7cJ1mendh)

**-问：**SHWXB对数据加密传输的要求是什么程度的，`https`够不够？

**-答1：**`https`传输是可以的。

**-答2：`TLS1.2`**。

**-答3：**`tls 1.0`--高危；`tls 1.1`--中危。但凡做外部扫描检测都能查出来。下面就看爸爸会不会下整改通知了。

**-答4：**哈哈，现在`body`对敏感数据加密？在做`https`传输？

**-答5：**敏感信息一般建议前端加密。

**-答6：**不同场景不一样，还有个点，大部分企业内部传输都是明文。

**-答7：**这是数据加密，加密的数据 加密的传输 加密的存储。

**-答8：**对的，我的意思是都要加密，`TLS1.2`是加密协议的强度。

**-答9：**上海公安网警对`tls1.2`以下协议都会开整改通报。

**总结：`TSL1.2`才行。**

[加密/算法](https://cbe8t1r4a5.feishu.cn/wiki/NXqDwGrCvil4g6kux7LcsLbCnMb)
