
# 传输协议科普

> 来源：[https://docs.feishu.cn/docx/EVJ0dqjNXotRJmxom8GcAt7TnOh](https://docs.feishu.cn/docx/EVJ0dqjNXotRJmxom8GcAt7TnOh)

- 问：`http`传输还是`https`传输有何区别？
- 答1：这个只是个传输协议的问题吧？而且加S也不一定是合规的。很多加密方式也是不合规的。
- 答2：要求是`https`，得要`TLS 1.2`以上。
- 答3：`https`是基本。只是是通讯传输加密方式。但是用什么加密协议进行加密，就要另当别论。`SSL`协议，`TLS`协议都是基于传输的加密协议。然后这些协议本身，还有各种加密算法的支持。比如`RSA`算法甚至是国密`SM`算法。现在金融监管在推动国密改造，就是我们`https`证书里面加密算法都要`SM`算法支持。

推荐关注这个网站：`https://myssl.com`，也是运维和安全常用的网站。

`TLS 1.0 1.1`协议现在已经被公安部门禁用了。以及`SSL`协议，全系列都不安全了。`TLS1.3`太高，没`1.2`基础广。大部分还在用`1.2`。

[技术概念科普](https://cbe8t1r4a5.feishu.cn/wiki/GfWtwCXWbiBzgQk1sxUcfFgXnSP)
