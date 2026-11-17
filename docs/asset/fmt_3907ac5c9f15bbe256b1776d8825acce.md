
# Coze api 如何运行workflow？

> 来源：[https://now.feishu.cn/docx/IZPLdUTcHoRqGsxfa4Acl47Gn1f](https://now.feishu.cn/docx/IZPLdUTcHoRqGsxfa4Acl47Gn1f)

官方文档：

`https://www.coze.com/docs/developer_guides/workflow_run?_lang=zh`

让LLM改写成`fetch`请求，复制官方提供的`curl`请求示例，打开`Mixcopilot`，点击右上角的信息源，添加后，发送指令：写成`fetch`

![](img/9543102481b16dba21724ee79f4ed154.png)

```
fetch('https://api.coze.com/v1/workflow/run', {
    method: 'POST',
    headers: {
        'Authorization': 'Bearer pat_fA5hfLH***********',
        'Content-Type': 'application/json',
        'Accept': '*/*',
        'Host': 'api.coze.com',
        'Connection': 'keep-alive'
    },
    body: JSON.stringify({
        "workflow_id": "7432684057693044744",
        "parameters": {
            "t": "12345" 
        }
    })
})
.then(response => response.json())
.then(data => console.log(data))
.catch(error => console.error('Error:', error));
```

工作流id：

![](img/d99359881d068dc6a942b4dd94ded109.png)

![](img/63528058a8309ea526685e163109599e.png)

加入社群
