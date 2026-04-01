# 怎么上传自己的GPT模型

> 来源：[https://aiyanxishe.feishu.cn/docx/BO3udZ87woZhmYxTVH2coFaBnKk](https://aiyanxishe.feishu.cn/docx/BO3udZ87woZhmYxTVH2coFaBnKk)

**充分必要条件****：**

1.  科学上网

1.  订阅了OpenAI官网的GPT Plus计划

1.  注册的域名（阿里云、腾讯云、华为云什么云都行）

### **1 创建项目**

在Explore GPTs中点击+Create，自行引导词构建一个模型，记得完善GPT的名称，简介，示例问题等信息。小编这里是随便做了个解决R和Py包错问题的模型，随后暂时保存

![](img/b1cf57ead26f99f57fafe150a5bcdb4d.png)

在未解析域名到自己的openai上时是无法公开发布GPTs的

![](img/f857e82d2f0d4ec8c999febe44e67ed3.png)

### **2 解析域名**

点击左下角设置，进入Builder profile，然后点击这里的Verify new domain

![](img/ca6e4761624dacfded2899e3f181d9ef.png)

输入自己的域名，然后会提供一条验证码，复制

![](img/df449fd7db06dbcdf27dff9bc09076da.png)

打开自己注册域名的云控制台，在解析域名栏添加记录，修改以下三个地方：

+   **「主机记录」**：@

+   **「记录类型」**：TXT

+   **「线路类型」**：境外

+   **「记录值」**：刚刚复制的那段验证码

然后点击确定即可（这里是腾讯云，其他云也同理）

![](img/51f424333d81b2f2c643372638784e21.png)

随后返回ChatGPT的设置页面check一下，check完回到自己创建的模型选择发布Everyone即可。