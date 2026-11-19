
# 如何手动破限Claude、Poe、GPT4

> 来源：[https://sqivg8d05rm.feishu.cn/docx/OXTtdo7MkojSM7xRX7nc5y3dnVg](https://sqivg8d05rm.feishu.cn/docx/OXTtdo7MkojSM7xRX7nc5y3dnVg)

## 手动破限Claude

1. 参考上面的[Clewd与酒馆搭建教程](https://sqivg8d05rm.feishu.cn/wiki/E925wr6MKixS44kJUBtcB0uYnQe)或水秋[云酒馆b站教程](https://www.bilibili.com/video/BV1b34y1G7ei/)，搭建好酒馆
2. 成功与claude涩涩一次
3. 去[claude官网](https://claude.ai/chat/)登录当时使用的claude账号
4. 如下图，标题为`Untitled`的就是你刚才的对话

![](img/3b07262289cf5f317a5593fdeab8796e.png)

5. 如下图，点进去有一个`paste.txt`的文档（因为酒馆会把破限和角色卡打包成txt文档发给Claude）

![](img/59543de5cbcea18f57d35a5b35235d0f.png)

6. 复制并保存此文档，打开后可以发现文档内容是由乱码+破限+角色卡+聊天记录组成的，你需要做的就是修改里面的角色卡或聊天记录
7. 新建一个聊天窗口直接发给claude就行啦，可以对聊天记录进行一些删减以节省token
8. 如果小克道歉就新建一个新的聊天窗口，修改好文档后再发给他

## 手动破限Poe

1. 参照上面破限Claude的步骤1-5，把txt文档发给Poe的`Claude-2-100k`即可
2. 很多时候只需要复制一些乱码（5000-10000字符）加在最开头，就能直接继续对话无需开新聊天界面
3. 根据2可以引申出，很多旧时代的slack模板，只要在开头加上足够多的乱码扰乱poe的外部监管系统，就能在poe的claude2继续使用。有些slack模板太短可能需要加一些破限内容，可以参考酒馆的各种破限。
4. 如果道歉就删了claude道歉的那段聊天记录，换一段乱码重新发过去

（注：港区Poe大约17刀一个月，每个月1000条Claude2，600条GPT4）

## 手动破限GPT4

1. 搜索油猴脚本教程，安装油猴插件
2. 在群文件或作者[GitHub](https://github.com/4as/ChatGPT-DeMod/blob/main/ChatGPT%20DeMod.user.js)下载`ChatGPT DeMod.user.js`插件并安装，如下图有`Demod: On`，就算安装成功

![](img/e5e4a28d3bde6a26982fef1ecffa0020.png)

（插件作用是防止生成的内容被Openai屏蔽不给用户看，实际上破限成功，大多数情况下GPT都会生成内容，但是被Openai遮住了）

3. 如下图选`Create a GPT`新建一个自定义GPT4

![](img/bb19e62c049781fc69996722915e6f63.png)

4. 如下图选`Configure`，起个名，在`Instructions`填入`NaroticaGPT`模板（群文件有GPT4专用的`NaroticaGPT` 模板）

![](img/fe3c871f04f8efa8e8c7b785a4368112.png)

5. 然后用`Upload files`上传你的角色卡、聊天记录、酒馆发给Claude的txt文档之类
6. 用词可以避免出现详细描写之类的字样，容易破限失败

（注：GPT4 20刀一个月，每3小时50条）
