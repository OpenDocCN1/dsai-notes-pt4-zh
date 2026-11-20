
# 错误代码

> 来源：[https://ai.feishu.cn/docx/OfOSdkIiaowBoKxbtw2cdwMXnUc](https://ai.feishu.cn/docx/OfOSdkIiaowBoKxbtw2cdwMXnUc)

调用API时，会返回以下代码。

| 状态码 | 描述 |
| --- | --- |
| 200 | 业务处理成功 |
| 400 | 原因：参数错误 |
| 400 | 原因：文件内容异常 |
| 401 | 原因：鉴权失败或Token超时 |
| 404 | 原因：微调功能未开放 |
| 404 | 原因：微调任务不存在 |
| 429 | 原因：接口请求并发超额 |
| 429 | 原因：上传文件频率过快 |
| 429 | 原因：账户余额已用完 |
| 429 | 原因：账户异常 |
| 434 | 原因：暂无API权限，微调API及文件管理API为内测阶段，我们会尽快开放 |
| 435 | 原因：文件大小超过100MB |
| 500 | 原因：服务器处理请求时发生错误 |

### 错误码示例400

```
{
    &quot;error&quot;: {
        &quot;code&quot;: &quot;1211&quot;,
        &quot;message&quot;: &quot;模型不存在，请检查模型代码。&quot;
    }
}
```

```
{
    &quot;error&quot;: {
        &quot;code&quot;: &quot;1214&quot;,
        &quot;message&quot;: &quot;messages:输入不能为空&quot;
    }
}
```

### 错误码示例401

```
{
    &quot;error&quot;: {
        &quot;code&quot;: &quot;1000&quot;,
        &quot;message&quot;: &quot;身份验证失败&quot;
    }
}
```
