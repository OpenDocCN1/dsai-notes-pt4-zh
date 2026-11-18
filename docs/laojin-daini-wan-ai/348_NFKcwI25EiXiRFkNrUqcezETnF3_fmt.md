
# OneAPI-接口管理和分发神器：所有大模型一键封装成OpenAI协议

> 来源：[https://my.feishu.cn/docx/RaVjd00r4oDxWVxlh0mccXG8nYe](https://my.feishu.cn/docx/RaVjd00r4oDxWVxlh0mccXG8nYe)

一款接口管理和分发神器，将所有大模型一键封装成OpenAI协议。

一张图讲明白原理：

![](img/98c0cbb4e61614e52cee47637462edf4.png)

首先需要安装2个工具：

Git，参考[Git 安装](https://tffyvtlai4.feishu.cn/wiki/MF1ywmPKhig7p3kpIqNcFsmPnPe)

Docker，参考[Docker安装](https://tffyvtlai4.feishu.cn/wiki/C4pzw37L9ibcf2kx99OcY3nznJc)

## 一、下载安装

打开命令提示符（按 Win + R，输入`cmd`后按回车）。

切换到你想要安装的路径`G:\_LLMOPS\`：

```
cd /d G:\_LLMOPS
```

`cd /d`用于切换驱动器和目录。

克隆`one-api`仓库到这个路径下：

```
git clone https://github.com/songquanpeng/one-api.git
```

![](img/8a4ff21d9cf62c554fd732867ba833c0.png)

这会将代码克隆到`G:\_LLMOPS\oneapi`文件夹中。

克隆完成后，输入以下命令进入`one-api`文件夹：

```
cd one-api
```

在`G:\_LLMOPS\oneapi`路径下，运行以下命令启动`Docker`容器：

```
docker-compose up -d
```

![](img/7cd9937f85b79ff17a2cec82c0aa0ca8.png)

打开`http://localhost:3000/`，显示页面，即安装完成。

![](img/1855c27a351a0b3343f5c434a6ba85d6.png)

## 二、配置OneAPI

打开`http://localhost:3000/login`登录账号。

初始账号用户名为`root`，密码为`123456`。

![](img/56434966b61dc291482279ab83bce310.png)

添加新的渠道。

![](img/4e25bfb66a4f5521a0213f0af43b7510.png)

类型选择`Ollama`，模型自己下载什么了就填什么，必须一模一样，包含大小写一致。

参考教程：[Ollama开源大语言模型部署](https://tffyvtlai4.feishu.cn/wiki/Fjv4wcML6i4r4QkK9iEcJEOonfc)

如下，我填写自己下载的模型信息，密钥随便填，代理填一样的。

名称：`qwen2`

模型：`qwen2:7b`

密钥：`sk-sky`

代理：`http://host.docker.internal:11434`

![](img/f91a8aa05d34b9b6114e0551a8207584.png)

点击提交，然后回到“渠道”会发现有了`qwen2`的一列。 点击“测试” 如果弹出，测试成功，说明大模型调通了。

![](img/e3d4efcbc1b000ab14f5f40f95643e63.png)

点击【令牌】 - 【添加新令牌】

![](img/bbbc629a9e81689c7d3cda827d8085dc.png)

名称随意，时间设为永不过期、额度设为无限额度。 点击【提交】

![](img/9671eb26614aa4169c0c9a9a58418b8c.png)

点击【令牌】，会看到自己设置的。 点击复制即可复制`key`。

![](img/fb094fbcabf5efcebbaa4b7e8b8e2e14.png)

## 常见问题

1. 额度是什么？怎么计算的？One API 的额度计算有问题？

+ `额度 = 分组倍率 * 模型倍率 * （提示 token 数 + 补全 token 数 * 补全倍率）`

+ 其中补全倍率对于 GPT3.5 固定为 1.33，GPT4 为 2，与官方保持一致。

+ 如果是非流模式，官方接口会返回消耗的总 token，但是你要注意提示和补全的消耗倍率不一样。

+ 注意，One API 的默认倍率就是官方倍率，是已经调整过的。

1. 账户额度足够为什么提示额度不足？

+ 请检查你的令牌额度是否足够，这个和账户额度是分开的。

+ 令牌额度仅供用户设置最大使用量，用户可自由设置。

1. 提示无可用渠道？

+ 请检查的用户分组和渠道分组设置。

+ 以及渠道的模型设置。

1. 渠道测试报错：`invalid character '<' looking for beginning of value`

+ 这是因为返回值不是合法的`JSON`，而是一个`HTML`页面。

+ 大概率是你的部署站的`IP`或代理的节点被 CloudFlare 封禁了。

1. ChatGPT Next Web 报错：`Failed to fetch`

+ 部署的时候不要设置`BASE_URL`。

+ 检查你的接口地址和`API Key`有没有填对。

+ 检查是否启用了`HTTPS`，浏览器会拦截`HTTPS`域名下的`HTTP`请求。

1. 报错：当前分组负载已饱和，请稍后再试

+ 上游渠道`429`了。

1. 升级之后我的数据会丢失吗？

+ 如果使用`MySQL`，不会。

+ 如果使用`SQLite`，需要按照我所给的部署命令挂载`volume`持久化`one-api.db`数据库文件，否则容器重启后数据会丢失。

1. 升级之前数据库需要做变更吗？

+ 一般情况下不需要，系统将在初始化的时候自动调整。

+ 如果需要的话，我会在更新日志中说明，并给出脚本。

1. 手动修改数据库后报错：数据库一致性已被破坏，请联系管理员？

+ 这是检测到`ability`表里有些记录的渠道`id`是不存在的，这大概率是因为你删了`channel`表里的记录但是没有同步在`ability`表里清理无效的渠道。

+ 对于每一个渠道，其所支持的模型都需要有一个专门的`ability`表的记录，表示该渠道支持该模型。

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>
