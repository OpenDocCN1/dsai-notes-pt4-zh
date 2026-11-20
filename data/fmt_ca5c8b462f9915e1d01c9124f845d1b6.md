
# 连接COW的方法

> 来源：<https://my.feishu.cn/docx/VLBcdF9JLo21Wyx5aJ7ca4iunog>

QwenMax是我一直推崇的国内LLM，今天介绍下它怎么在`Cow`（`Chatgpt-on-wechat`）项目中配置。

首先根据官方[COW的Config设定](https://tffyvtlai4.feishu.cn/wiki/Ti7uwsKgiiW6bpk2nmQcVhQ8nnb)，我们能找到以下配置信息：

```
# 通义千问API, 获取方式查看文档 https://help.aliyun.com/document_detail/2587494.html
"qwen_access_key_id": "",
"qwen_access_key_secret": "",
"qwen_agent_key": "",
"qwen_app_id": "",
"qwen_node_id": "",  # 流程编排模型用到的id，如果没有用到qwen_node_id，请务必保持为空字符串
```

接下来，我们去寻找以上所需要的`Key`，Qwen强的LLM都是1,000,000 tokens限时免费使用额度，有效期 30 天，且应用只支持`Qwen_Max`，今儿我就告大家这个Key怎么获取。

确实很绕，估计大家自己都找不着。需要结合灵积和百炼两个业务系统寻找所需要的配置。

已和阿里人员进行了反馈，希望能尽快调整。

进入正文，一共所需4个配置内容。

## 通义旧版SDK，走Agent

### Access ID 和 Access Key

打开网页`https://ram.console.aliyun.com/overview`，右上角点击头像，弹出框选`AccessKey`管理

![](img/76c63d5dd90a107af9d0ad2fbdd59de5.png)

打开界面后在这获取。

### 关闭界面后无法再查看`Secret`，复制后自行保存。

![](img/ff7fed547a3baa4e1b966f8e58629ca1.png)

### APP ID

还是这个地址`https://bailian.console.aliyun.com/#/home`，选中左侧我的应用，创建应用，然后在3区域复制`APP ID`，需要注意的是`APP ID`可能显示不全，需要把鼠标放到上面查看是否完整。

![](img/f902f4ab142fab9b7859d866b6229352.png)

### Agent Key

还是这个地址`https://bailian.console.aliyun.com/#/home`，选中左上角，然后弹出框里找到`Agent Key`

### 需要注意的是`APP ID`可能显示不全，需要把鼠标放到上面查看是否完整。

![](img/f5935567a7497aa4319e3889648f1042.png)

## 灵积新版SDK，只走LLM，不走Agent

### API Key

打开网页`https://bailian.console.aliyun.com/#/home`，右上角点击头像，弹出框选`API-Key`

![](img/e92ab67ca308bbb20fa6d077fc0c06c1.png)

## Cow配置

修改`/chatgpt-on-wechat/config.json`，修改`model`配置，添加`qwen`相关`Key`

配置时注意清空中文

```
"model": "qwen",

# 通义千问API, 获取方式查看文档 https://help.aliyun.com/document_detail/2587494.html
"qwen_access_key_id": "",
"qwen_access_key_secret": "",
"qwen_agent_key": "",
"qwen_app_id": "",
"qwen_node_id": "",  # 流程编排模型用到的id，如果没有用到qwen_node_id，请务必保持为空字符串


———————————————————————————————————— 或 ————————————————————————————————


"model": "qwen-max",

# 阿里灵积(通义新版sdk)模型api key
"dashscope_api_key": "",
```

完成后，重新扫码登录即可。

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>
