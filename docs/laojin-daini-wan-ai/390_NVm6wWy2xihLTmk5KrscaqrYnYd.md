
# Apilot - 丰富小功能

> 来源：[https://my.feishu.cn/docx/ReoKdssLBoFCsYxflmCcvZJ3nSc](https://my.feishu.cn/docx/ReoKdssLBoFCsYxflmCcvZJ3nSc)

## 转载自Github官网：https://github.com/6vision/Apilot

### 安装

[使用管理员](https://www.wangpc.cc/aigc/chatgpt-on-wechat_plugin/)口令在线安装即可，参考这里去如何认证管理员！

```
#installp https://github.com/6vision/Apilot.git
```

安装成功后，根据提示使用`#scanp`命令来扫描新插件，再使用`#enablep` Apilot开启插件，参考下图

![](img/b723f01929522dfcc25bbbcec6f7c298.png)

### 配置

直接安装不配置也可以使用一部分接口，部分接口(快递、天气)需要配置alapi的token。

+   服务器部署：复制插件目录的`config.json.template`文件并重命名为`config.json`，

+   `alapi_token`：填入申请的`alapi` token。

+   `morning_news_text_enabled`：默认`false`，发送早报图片；`true`，发送文字版早报。

+   [dock](https://github.com/zhayujie/chatgpt-on-wechat#3-%E6%8F%92%E4%BB%B6%E4%BD%BF%E7%94%A8)er部署：参考项目docker部署的插件使用，在挂载的`config.json`配置文件内增加apilot插件的配置参数，如下图，每次重启项目，需要使用`#installp`指令重新安装

![](img/f79a546784b09ce0436fbad7b6b75bae.png)

### Token申请

+   `alapi_token`申请点击这里[alapi](https://admin.alapi.cn/account/center)

### 使用

+   对话框发送“早报”、“摸鱼”、"🧣热搜（已更新为"🧣热榜）"、”任意星座名称”可以直接返回对应的内容！

![](img/58384129a16d497658b6f20215e36a2f.png)

![](img/25cc994b7ef4aa516a33cf99eb71813d.png)

![](img/8ad97d4cc3d2859ec5bffbadecd7d234.png)

![](img/42fab2c14def6da06b9c5c3efe6ac9a4.png)

+   快递查询格式：快递+快递编号。如：快递YT2505082504474，如下图!

### 亲测不太好用查快递

![](img/d85a884fd40b3d18c37b99795413740e.png)

+   天气查询格式：城市+天气。如：成都天气。（支持3400+城市天气，输入不正确或者查询失败返回北京天气）

![](img/2669c3f885354bdf79d36003cb07ce80.png)

+   热榜查询。支持:<🧣/虎扑/知乎/哔哩哔哩/36氪/抖音/少数派/IT最新/IT科技>

![](img/18700a20e19094ed547528f0777a9b7d.png)

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>
