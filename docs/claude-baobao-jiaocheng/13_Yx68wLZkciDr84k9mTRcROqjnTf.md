
# PC/VPS一键脚本(初的小工具)

> 来源：[https://docs.feishu.cn/docx/Gq2mdLVbSocDkRx8cEkcQixmn0c](https://docs.feishu.cn/docx/Gq2mdLVbSocDkRx8cEkcQixmn0c)

工具作者: 初 教程作者: 水秋喵 测试:

## PC 本地自动部署 Clewd+酒馆

旧版初的小工具如果报错，需要手动更新到`v0.0.42`。下载链接https://www.123pan.com/s/P8OAjv-Glhtv.html

### 运行脚本

在[网盘(点击下载)](https://www.123pan.com/s/P8OAjv-Glhtv.html)找到`初的部署小工具.exe`，新建文件夹`Claude`并放入。杀毒软件可能会报毒，无毒的请放心。

![](img/b2ef08718cf08a43a0924924648e49a9.png)

按照需求安装所需的 Git 和 Nodejs，弹出安装界面一路点`next`即可。

Git和Nodejs安装好后，关闭小工具重新运行。

![](img/6306c32401d51319918bd9e3ee8f9cc2.png)

![](img/ab4ee8e04732f405a1495d163bb91e96.png)

### 部署酒馆

选择1. `SillyTavern`（酒馆），1. 安装 `SillyTavern`（酒馆），国内镜像。

安装完成后选择2. 启动 `SillyTavern`（酒馆），等待自动弹出网址或进入 `http://127.0.0.1:8000/`。

![](img/82b78f14bcd635c4a8adea351afdc4cc.png)

![](img/3e3081a97f25b493b4e2910ad1420e22.png)

其他先别动，返回小工具。

### 部署Clewd

(下面所有内容都是选择clewd（修改版）Test，非Tset版本没有修复BUG一般不能正常使用，因为初偷懒一直不改，请等待下个版本将Test设为默认。)

返回主菜单选择2. clewd（修改版）Test，1. 安装 clewd（修改版），国内镜像。

![](img/5a06491e2464dcd0921c0269ee29fae3.png)

安装完成后先选择2. 启动 clewd（修改版），运行后再选择3. 配置 clewd（修改版）。

[连接第三方API（OpenAI格式）](https://sqivg8d05rm.feishu.cn/wiki/TS8zwRkqyiZk8SkCGRTcNMg6nMd)如需使用API，可以跳过此处，先看下方的4. 启动酒馆，然后看）

#### 使用Cookie

选择3. Cookie列表 (`CookieArray`) 当前设置：`[]`，2. 追加至 Cookie 列表。

将Cookie全部粘贴进去，然后多次回车，最后选择8. 保存配置并生效。

![](img/7244d74b7e2da77d5770da3de6e8791a.png)

![](img/beea7fc46e81bf480d1af93eb21bc085.png)

返回并选择2. 启动 clewd（修改版），出现cookie的账号信息代表启动成功。

![](img/dc8d36cfc7646c511bbe5990659a1377.png)

网络报错：包括并不限于`302/403`报错，以及下图

![](img/8f2c3741573619eaedee895f52de4fe3.png)

![](img/4b866eb79dd6b1065cd1cf47d9724257.png)

![](img/47b111b1f3f15b5030ee717e1e93a002.png)

![](img/ec97c0aa79eb5c7d993ff78bbdc6e7a1.png)

检查魔法是否开启 服务模式(小地球变绿) 和 TUN模式，并选择非香港的节点。

![](img/53d222e0656038bb4aec11d2628ceeb6.png)

![](img/d262c3aa01d9704469694a457a45ecac.png)

#### 使用API

请直接看：[连接第三方API（OpenAI格式）](https://sqivg8d05rm.feishu.cn/wiki/TS8zwRkqyiZk8SkCGRTcNMg6nMd)

### 启动酒馆

1. 进入酒馆（`http://127.0.0.1:8000/`），填入你喜欢的昵称并保存。

不要勾选`简单UI`（`Enable simple UI mode`），会导致缺少很多必要设置。

![](img/58e10b68eed240d352192716406c2f04.png)

1. 按照图示设置酒馆。

注意聊天补全源不要自作主张的选Claude，老老实实选择OpenAI里的`gpt–3.5`或者`gpt4`，他会自动选择真正的Claude3.5模型。

必须选择OpenAI模型里的`gpt–3.5`或者`gpt4`，千万不能改！实际上用的就是Claude！乱选的结果只有报错！

![](img/a0aba7ee75a01e4ff0eccd8c99454885.png)

1. 再回到第二个图标，点击`connect`，下面变成`Valid`就可以了。

![](img/072fd25ea66b3e04a53a1be539fef5f7.png)

## VPS 云端自动部署 Clewd+酒馆

### 运行脚本

#### 1.1 (已有VPS，且连接上SSH终端)

1. [连接到VPS，请](https://www.123pan.com/s/P8OAjv-Glhtv.html)输入下方的完整命令并按回车键，或者在网盘(点击访问)中找到VPS运行指令并输入。

```
wget https://raw.githubusercontent.com/4665x312ad/Chu_clewd_and_SillyTavern/main/初的部署小工具.bin -O 初的部署小工具.bin && chmod +x 初的部署小工具.bin && ./初的部署小工具.bin
```

1. 等待脚本自动下载安装，直到出现脚本主界面。
