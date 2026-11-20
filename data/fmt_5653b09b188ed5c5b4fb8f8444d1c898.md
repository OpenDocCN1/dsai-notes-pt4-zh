
# 消息引用

> 来源：[https://my.feishu.cn/docx/E0tXdV3mvoXxxHx2Bzrc4XUwnLh](https://my.feishu.cn/docx/E0tXdV3mvoXxxHx2Bzrc4XUwnLh)

`chatgpt-on-wechat`默认忽略引用的消息，需要修改源代码才会响应引用的消息，然后才可以开启优化引用消息的功能。

需要修改的源代码文件为`channel/chat_channel.py`，注释掉或者直接删除下图中红色方框中的4行代码，然后重启`chatgpt-on-wechat`即可：

![](img/9ac95098d1c56d7eb17139d5a4c33d42.png)

🛰️截图说明：引用消息后，直接对于引用内容补充说明即可。

![](img/02ed98111b4d05cbde78bb9b03e501332.png)

![](img/a7513c70d78ad0926c27b96005e4d87d.png)

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>
