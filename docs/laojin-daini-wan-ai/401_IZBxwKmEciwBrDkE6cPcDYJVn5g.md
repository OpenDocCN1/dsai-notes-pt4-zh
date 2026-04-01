# CogView2Cow - 智谱AI绘画&视频

> 来源：[https://my.feishu.cn/docx/PYtHdTQfAo1XRfxVxKkcElBknPg](https://my.feishu.cn/docx/PYtHdTQfAo1XRfxVxKkcElBknPg)

#10月31日赠送已关闭，使用需要充值

##     前   言

智谱又放大招了。

这次不仅给了LLM，还新增了**图像**和**视频**生成。

限时1个月！

白送的LLM自行配置，份额很大！

其中PLUS老金用了很久，很好用！

![](img/4c988fbc7079fb5e5d4d689cbd70b0e3.png)

于是我就颠颠的跑去把它接入了微信机器人。

改版来自Lingyuzhou提供的原版，很喜欢的一个小伙子，研究能力很强。

他跟我说图像生成OK了，但是视频怎么都搞不了，让老金我来看看怎么事儿。

先放生成效果。

**视频：**

![](img/7a9348c94502073eea1342110ab092cb.png)

![](img/87b797a4531ce98a77961f91a2901d51.png)

[cac1cdb22466556d3047c343404930ec.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Z6MobIuuRo8YbTxOOXMcgq3RnMh)

**画图：**

## **e**mm 个人建议使用Siliconflow的Flux.dev或Schnell，因为和智谱视频共用次数，画图没什么亮点，且有水印。

![](img/3df994de134c8022a9e664e768e55881.png)

可以参考之前老金写的2篇文章玩绘图：

**MJ：**[最强AI绘画-Midjourney接入微信机器](http://mp.weixin.qq.com/s?__biz=MzI0NzU2MDgyNA==&mid=2247486268&idx=1&sn=12a9bbedc2ef2167e441dad2c9f1eae7&chksm=e9af642bded8ed3d115b654ee57b44253f00156948e5019050b40e0c12156a95b497d2bba2c7#rd)人手把手教程！

Flux Dev或Schnell：微信上如何优雅的白嫖最强开源AI绘画模型-Flux，手把手教程！没剩几天！要的抓紧囤！

---

##   微信机器人使用方法

下面开始讲解在微信机器人中使用，首先要满足几个基础条件。

1、已安装chatgpt-on-wechat或dify-on-wechat项目。

2、有LLMAPI，你可以使用智谱本次给的，也可以和老金我一样，用Siliconflow提供的Qwen2.5系列，我认为是国产第一梯队，申请链接：https://cloud.siliconflow.cn/i/wdyoipMi

3、申请智谱API

4、关注老金我的公众号，发送“**智谱AI视频**”，获取Github安装插件链接。

## 1和2在之前的教程中都有，如果不会，请添加老金我的个人联系方式，拉你进AI交流群，会有一些好心人会教你们。

本文主讲3和4，如何安装插件，以及插件的使用方法。

### 获取智谱API

打开地址，如果没申请需要申请一下：

https://open.bigmodel.cn/usercenter/apikeys

创建API后复制Key：

![](img/40cf0ae81ff41a9dc05a2009e58432da.png)

### 配置机器人

一行一行私聊机器人以下代码，第一行安装插件，第二行激活插件，第二行请在安装完成提示后进行。

```
#installp &lt;公众号获取的插件地址&gt;.git
#scanp
```

打开项目路径 chatgpt-on-wechat/plugins/CogView2Cow，找到Config.json文件。

```
{
    &quot;cogview_api_key&quot;: &quot;your_cogview_api_key&quot;,                                    #智谱API KEY
    &quot;image_base_url&quot;: &quot;https://open.bigmodel.cn/api/paas/v4/images/generations&quot;,  #不用改
    &quot;video_base_url&quot;: &quot;https://open.bigmodel.cn/api/paas/v4/videos/generations&quot;,  #不用改
    &quot;video_result_url&quot;: &quot;https://open.bigmodel.cn/api/paas/v4/async-result/{id}&quot;, #不用改
    &quot;image_model&quot;: &quot;cogview-3-plus&quot;,                                              #不用改
    &quot;video_model&quot;: &quot;cogvideox&quot;,                                                   #不用改
    &quot;translate_api_url&quot;: &quot;https://api.siliconflow.cn/v1/chat/completions&quot;,        #使用LLM的交互地址
    &quot;translate_api_key&quot;: &quot;your_translate_api_key&quot;,                                #LLM的API KEY
    &quot;ranslate_model&quot;: &quot;Qwen/Qwen2.5-7B-Instruct&quot;,                                 #LLM使用的模型
    &quot;storage_path&quot;: &quot;/root/chatgpt-on-wechat/plugins/CogView2Cow/image_video&quot;,    #存储路径
    &quot;cleanup_days&quot;: 3,                                                            #不用改
    &quot;cleanup_check_interval_minutes&quot;: 1440,                                       #不用改
    &quot;image_command&quot;: &quot;智谱画图&quot;,                                                   #触发画图
    &quot;video_command&quot;: &quot;智谱视频&quot;,                                                   #触发视频
    &quot;query_command&quot;: &quot;查询进度&quot;                                                    #触发进度查询+使用时添加ID
}

```

配置完成后，私聊机器人

```
#reloadp CogView2Cow
```

搞定，大家就可以愉快的使用微信机器人生成AI视频与图像了。

## **插件使用**

**生成图像：**

智谱画图 一个小姑娘 --ar 1：1

```
    &quot;1:1&quot;: &quot;1024x1024&quot;,
    &quot;1:2&quot;: &quot;720x1440&quot;,
    &quot;2:1&quot;: &quot;1440x720&quot;,
    &quot;3:4&quot;: &quot;864x1152&quot;,
    &quot;4:3&quot;: &quot;1152x864&quot;,
    &quot;9:16&quot;: &quot;768x1344&quot;,
    &quot;16:9&quot;: &quot;1344x768&quot;
```

**生成视频：**

智谱视频 一个小姑娘

## F&Q：

1、如果无法使用，请先重启一遍项目。

2、请检查各个配置是否正确。

最后，祝大家国庆快乐！

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>