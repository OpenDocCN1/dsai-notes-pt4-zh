
# Replit 部署 Clewd 与酒馆

> 来源：[https://docs.feishu.cn/docx/MgyodBPmGoTHOAxqT4nc8yAJnRd](https://docs.feishu.cn/docx/MgyodBPmGoTHOAxqT4nc8yAJnRd)
> replit从2024年开始需要手动启动

## 视频教程参考

[PC端](https://www.bilibili.com/video/BV1b34y1G7ei) / [手机端](https://www.bilibili.com/video/BV16u4y147er)

## 搭建 Clewd

1. 注册[Repit](https://replit.com)：https://replit.com/signup （遇到报错尝试使用魔法/更换魔法节点）

1. 注册后进入https://replit.com/@waterqiu/clewd

1. 点击Fork

![](img/cad6aec16821315c6633e3f72538b473.png)

1. 点击 Tools 栏中的 Secrets，手机端滑到最右侧点击 Tools 栏中的 Secrets

![](img/92e6ef8b3a276420ec9789eeabe895bb.png)

1. 点击 + New Secret，Key 为 `CookieArray`（区分大小写），Value 为你的Cookies。

![](img/f15611f9b740cd0b56e9da633cee5866.png)

1. 再添加一个 `ProxyPassword`（区分大小写），Value 为你想设置的密码。

![](img/8d48259f3bc8d4c7cede2c022be624a7.png)

1. 点击 `Run`，在页面中点击“点击复制”，并保存，这就是你的Clewd反向代理地址。

![](img/fa078bb347e96e38bf1cedac5736b11e.png)

## 搭建 酒馆

视频教程参考：[PC端](https://www.bilibili.com/video/BV1b34y1G7ei) / [手机端](https://www.bilibili.com/video/BV16u4y147er)

1. 注册[Repit](https://replit.com)：https://replit.com/signup （遇到报错尝试使用魔法/更换魔法节点）

1. 进入 https://replit.com/@waterqiu/SillyTavern，点击Fork

![](img/410e64fb6974e75f563a3a86fe28daba.png)

1. 点击 Tools 栏中的 Secrets，手机端滑到最右侧点击 Tools 栏中的 Secrets

![](img/b4f33b1cbd084afb9eebea68cb8164c3.png)

1. 点击 + New Secret，添加 `username` 和 `password` 两个 Key，具体 Value（账号密码) 自行设置。

![](img/728dae81c51219fb819bf4d07888e728.png)

![](img/a8956b0859aae1e417a33124640034df.png)

1. 点击 `Run`，弹出验证框后先不要验证，直接关闭验证框，注意是直接关闭验证框。

1. 点击 Webview 中的 New Tab，跳到新网页，输入之前填写的账号密码进入酒馆

![](img/b6ee2618545b0f99c4179787f73ff6b9.png)

1. [进入](https://sqivg8d05rm.feishu.cn/docx/MgyodBPmGoTHOAxqT4nc8yAJnRd#AsxXdAsRAoQJYIxEP3QcYnU7nYf)酒馆后，点击第一个按钮。填入上文的 Clewd 地址与 `ProxyPassword` 密码。

![](img/fdad84389efe892aaccd8939dbdd537e.png)

1. 点击第二个按钮，点击 `Connect` 连接，并勾上自动连接，绿色连接成功。（注意，必须选择OpenAI！）

1. 点击最后一个按钮导入角色卡，选择角色卡进行对话。

![](img/08641483f42597c1935f22007646ad0a.png)
