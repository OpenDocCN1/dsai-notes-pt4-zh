
# 被封了？DOW颠覆更新拯救你！

> 来源：[https://my.feishu.cn/docx/Xmv9d7x5Zou4zqxqsWncE9Menwe](https://my.feishu.cn/docx/Xmv9d7x5Zou4zqxqsWncE9Menwe)

HI，大家好，年前好忙，接近一个月没有更新了。

依稀记得在元旦期间大规模的检测，让大家的小助手都挂掉了，时至今日都无法再使用。

先给大家讲讲原理。

大家使用的COW或DOW之前都是依托于itchat，它是一个Web协议，也就是只能在网页登录的。

所以大家登录后在手机上看到的是“windows 🛰️已登录”或者“网页已登录”。

此次检测也是针于Web协议，所以，一扫一个准儿。

DOW在元旦前，更新了GEWE协议，它是一个Ipad协议，不在检测范围内。

老金我使用了大概一个多月了，没有任何异常发生。

再来谈谈优劣势。

**优势：**

GEWE协议中，包含了很多更好的接口，比如之前大家问过的。

是否能发语音条？是否能操作朋友圈？是否能自动拉别人进群？

这些它都做得到，但是要自己接，如果你和老金学过AI编程，那这些很简单。

老金我一个不懂代码看不懂英语的人，接的已经七七八八了。

**劣势：**

接口废了一大半，插件一半以上无法使用，需要重新封装。

原理如下图，简单可以理解成为原来的接口是A，现在的接口是B了，名字都换了，所以识别不到。

好了，废话不多说，这些和初级玩家没啥关系，咱就先找个方法用就好。

## 安装部署

对比之前，多了需要部署GEWE的步骤。

如果你一直是老金的粉丝，那很好，这里我说什么你应该很容易看得懂。

**如果你之前没看过**老金的文[COW初级使用教程](https://tffyvtlai4.feishu.cn/wiki/AS5pw7JOQi9lHWk56pucqjkinSc)，那么请自行申请腾讯云宝塔服务器。可参考 。

[DOW使用教程](https://tffyvtlai4.feishu.cn/wiki/SiGgwujIRiEkvwk6sBecFhoEnce)ify-On-Wechat项目部署需要参考 。

### 1、部署GEWE

这里我们使用Docker部署，这里为了方便小白操作，咱直接放宝塔上的步骤，不使用指令了。

先下载GEWE镜像，从下选择合适自己的：

```
# 从阿里云镜像仓库拉取(国内)
docker pull registry.cn-chengdu.aliyuncs.com/tu1h/wechotd:alpine
docker tag registry.cn-chengdu.aliyuncs.com/tu1h/wechotd:alpine gewe

# 或从GitHub镜像仓库拉取
docker pull ghcr.io/tu1h/wechotd/wechotd:alpine
docker tag ghcr.io/tu1h/wechotd/wechotd:alpine gewe
```

`root` 下新建一个 `gewechat` 文件夹。

![](img/49fd65108c204c62e3f9b537e816d058.png)

创建 `data` 文件夹和 `docker-compose.yml` 文件。

![](img/1e528e28a1c1e4c32d45a962c1cca2b8.png)

复制以下内容到 `docker-compose.yml` 文件。

```
version: &#x27;3&#x27;
services:
  gewechat:
    image: gewe
    container_name: gewe
    volumes:
      - ./data:/root/temp
    ports:
      - &quot;2531:2531&quot;
      - &quot;2532:2532&quot;
    networks:
      - gewe-network
    extra_hosts:
      - &quot;host.docker.internal:host-gateway&quot;  # 添加这行，允许容器访问宿主机
    restart: always

networks:
  gewe-network:
    driver: bridge
    
```

然后点击终端，执行指令，启动Docker，直接执行第2个，第1个只是备注给大家说明。

```
docker-compose down     #会停止并移除所有容器
docker-compose up -d    #以 后台模式 启动所有容器
```

![](img/1f32fce000399ff192a6f5029dd4a8a3.png)

看到如下日志，代表启动成功。

![](img/2eec7e657038c8748e5a47baf852db0d.png)

然后查看Docker中是否已经启动，如果已经启动了，那么代表GEWE部署完成。

![](img/f45ad7680a8c165cad411db7f2dc6317.png)

### 2、部署Dify-On-Wechat

[DOW使用教程](https://tffyvtlai4.feishu.cn/wiki/SiGgwujIRiEkvwk6sBecFhoEnce)下简称为DOW。下载安装我就不多解释了，看老金之前的文章教学，

这里只说怎么连接GEWE。

`dify-on-wechat`、`dify`、`gewechat` 服务的调用关系

![](img/df498b32fe6a0364f74df9e92d287080.png)

在 `dify-on-wechat` 项目的 `config.json` 中需要配置以下 `gewechat` 相关的参数，注意删除解释的文字：

这里如果你和老金一样 `gewechat` 是在Docker中部署的，那么直接和老金写一样的就行。

Dify相关的请自行配置。

```
{
    &quot;channel_type&quot;: &quot;gewechat&quot;   # 通道类型，请设置为gewechat    
    &quot;gewechat_app_id&quot;: &quot;&quot;,
    &quot;gewechat_token&quot;: &quot;&quot;,
    
    &quot;gewechat_base_url&quot;: &quot;http://172.17.0.1:2531/v2/api&quot;,
    &quot;gewechat_callback_url&quot;: &quot;http://172.17.0.1:9919/v2/api/callback/collect&quot;,
    &quot;gewechat_download_url&quot;: &quot;http://172.17.0.1:2532/download&quot;,
}
```

参数说明：

+   `gewechat_token`: `gewechat` 服务的认证 `token`，首次登录时，可以留空，启动 `dify-on-wechat` 服务时，会自动获取 `token` 并自动保存到 `config.json` 中

+   `gewechat_app_id`: `gewechat` 服务分配的设备ID，首次登录时，可以留空，启动 `dify-on-wechat` 服务时，会自动获取 `appid` 并自动保存到 `config.json` 中

+   `gewechat_base_url`: `gewechat` 服务的API基础地址，请根据实际情况配置，如果 `gewechat` 服务与 `dify-on-wechat` 服务部署在同一台机器上，可以配置为 `http://本机ip:2531/v2/api`

+   `gewechat_callback_url`: 接收 `gewechat` 消息的回调地址，请根据实际情况配置，如果 `gewechat` 服务与 `dify-on-wechat` 服务部署在同一台机器上，可以配置为 `http://本机ip:9919/v2/api/callback/collect`，如无特殊需要，请使用9919端口号

+   `gewechat_download_url`: 文件下载地址，用于下载语音、图片等文件，请根据实际部署情况配置，如果 `gewechat` 服务与 `dify-on-wechat` 服务部署在同一台机器上，可以配置为 `http://本机ip:2532/download`

> 请确保您的回调地址 `callback_url`，即 `dify-on-wechat` 启动的回调服务可以被 `gewechat` 服务正常访问到。如果您使用Docker部署，需要注意网络配置，确保容器之间可以正常通信。

> 本机 `ip` 是指 `局域网 ip` 或 `公网 ip`，可通过 `ipconfig` 或 `ifconfig` 命令查看

> 对与 `gewechat_callback_url`，`ip` 不能填 `127.0.0.1` 或 `localhost`，否则会报错

> `9919` 端口是 `dify-on-wechat` 服务监听的端口，如果是用 `docker` 启动的 `dify-on-wechat` 服务，请把 `9919` 端口映射到宿主机

启动成功后，可以看到如下日志信息，注意 `token` 和 `appid` 会自动保存到 `config.json`，无需手动保存

![](img/dbdef042189ac66754a4ccc762738819.png)

⚠️如果遇到 `gewechat` 创建设备失败，`unexpected EOF` 错误，请排查网络是否是以下情况：

1、代理：请关闭代理后尝试；

2、国外服务器：请更换为国内服务器；

3、回调地址为外网：请更换为内网地址；

4、异地服务器：请更换为同省服务器；

5、若手机上退出了Ipad，则需要把 `config` 里面 `GEWE` 的 `id` 和 `token` 的值删除；

#### `gewechat_channel` 限制

1.  `gewechat` 要求必须搭建服务到同省服务器或者电脑里方可正常使用，即登录🛰️的手机与 `gewechat` 服务必须在同一省

1.  `gewechat` 开源框架只支持下载接收到的图片，不支持下载文件

1.  `gewechat_channel` 目前暂时只支持接收文字消息，只支持发送文字消息与图片消息，后续支持的消息类型会逐步完善

1.  此项目仅用于个人娱乐场景，请勿用于任何商业场景

#### 语音条说明

老金我自己还没接，有兴趣的小伙伴可以自己试试。

语音相关配置如下，另外需要在 `dify` 应用中开启语音转文字以及文字转语音功能，注意语音功能需要安装 `ffmpeg` 依赖，如使用 `docker` 部署 `dify`，已集成 `ffmpeg` 依赖，无需额外安装。

```
{
  &quot;dify_api_base&quot;: &quot;https://api.dify.ai/v1&quot;,
  &quot;dify_api_key&quot;: &quot;app-xxx&quot;,
  &quot;dify_app_type&quot;: &quot;chatbot&quot;,  # 或agent
  &quot;channel_type&quot;: &quot;gewechat&quot;,  # 通道类型设置为gewechat
  &quot;model&quot;: &quot;dify&quot;,    
  &quot;speech_recognition&quot;: true,  # 是否开启语音识别
  &quot;voice_reply_voice&quot;: true,   # 是否使用语音回复语音
  &quot;always_reply_voice&quot;: false, # 是否一直使用语音回复
  &quot;voice_to_text&quot;: &quot;dify&quot;,     # 语音识别引擎
  &quot;text_to_voice&quot;: &quot;dify&quot;      # 语音合成引擎
}
```

`gewechat` 支持发送语音条消息，但是 `gewechat` 服务只能获取到 `20s` 以内的语音

所以你只能给 `bot` 发送 `20s` 以内的语音，而 `bot` 给你发送语音时无此限制。

![](img/b4f707b7004dd35295280f829dfce06a.png)

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>
