# 传输协议科普

> 来源：[https://docs.feishu.cn/docx/EVJ0dqjNXotRJmxom8GcAt7TnOh](https://docs.feishu.cn/docx/EVJ0dqjNXotRJmxom8GcAt7TnOh)

**-问**：http传输还是https传输有何区别？

**-答1：**这个只是个**传输协议**的问题吧？而且加S也不一定是合规的。很多加密方式也是不合规的。

**-答2：**要求是**https**，得要**TLS 1.2**以上。

**-答3：****https**是基本。只是是通讯传输加密方式。但**是用什么加**密**协议进行加****密，就要另**当别论。S**SL协议**，TLS协议都是基于传输的加密协议。然后这些协议本身，还有各种加密算法的支持。比如RSA算法甚至是国密SM算法。现在金融监管在推动国密改造，就是我们https证书里面加密算法都要SM算法支持。

推荐关注这个网站：https://myssl.com，也是运维和安全常用的网站。

**TL**S 1.0 1.1协议现在已经被公安部门禁用**了。以**及SSL协议，全系列都不安全了。TLS1.3太高，没1.2基础广。大部分还在用1.2。

[技术概念科普](https://cbe8t1r4a5.feishu.cn/wiki/GfWtwCXWbiBzgQk1sxUcfFgXnSP)