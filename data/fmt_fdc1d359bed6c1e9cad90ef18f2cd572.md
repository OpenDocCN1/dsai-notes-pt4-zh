
# AI智能体-安卓手机也能玩转自动化：打造📕文案提取+AI二创一条龙工作流教程

> 来源：[https://xuqiwei1986.feishu.cn/docx/JLxDdgB43ocSV2xpoadcuP9Yn4b](https://xuqiwei1986.feishu.cn/docx/JLxDdgB43ocSV2xpoadcuP9Yn4b)

## 为什么要做这个

苹果用户们早已开始用快捷指令+飞书多维表格实现一键获取视频文案，作为安卓用户的我们也不能落后！本文将介绍如何利用安卓平台的自动化工具，打造一个Android快捷指令的文案提取工作流，让创作效率翻倍。

## 你将收获

+ 掌握安卓手机快捷指令（MacroDroid）的设置方法
+ 学会搭建coze📕文案提取智能体
+ 了解如何在飞书多维表格调用工作流
+ 掌握飞书机器人流程设计技巧
+ 学会使用飞书多维表格AI插件

## 所需工具

1. 飞书PC端
   + 下载地址：`https://www.feishu.cn/download`

2. 飞书机器人流程设计
   + 飞书账号要求：
   + 个人版账号即可（无需企业版）

3. 扣子（Coze平台账号）
   + 官网地址：`https://www.coze.cn`
   + 使用手机验证码登录
   + 目前扣子智能体发布的飞书多维表格字段捷径插件每天200次，不知道专业版会不会多一些，因为没有超过200次，暂时不知道。可以使用我发布的扣子工作流调用字段捷径插件调用扣子工作流。插件地址：`https://bytedance.larkoffice.com/base/extension/replit_3f394934746be3fb`

4. MacroDroid（安卓自动化工具）
   + 版本要求：
   + Android系统：7.0及以上
   + MacroDroid版本：5.5.24
   + 下载安装：
   + [`MacroDroid-Pro-v5.52.4.apk`](https://w63nbfedzw.feishu.cn/file/NRq1b8sYboOBT5xRiwQcBV4Snzo?from=from_copylink)
   + Play商店：MacroDroid - Device Automation
   + APK直接下载：官网`https://www.macrodroid.com/`
   + 权限配置：
   + 悬浮窗权限（界面操作）
   + 后台运行权限（保持活跃）
   + 使用建议：
   + 给予所有必要权限

5. 📕App
   + 账号要求：
   + 正常登录状态

## 搭建流程

### coze📕文案提取智能体搭建

> 构建智能化文案处理核心

#### 智能体基础配置

1. 登录扣子后，点击左上角的➕创建智能体，在弹出窗口点击创建智能体下面的【创建】：

![](img/9c9c95b0ab45d1a10f71120af3bd74df.png)

![](img/293c58d2ea5ee948748292424fc9ca81.png)

2. 设置智能体内容：

![](img/196bfd571cbf9a53b9d88ff41e6c9198.png)

3. 设置智能体为【单 Agent （对话流模式）】：

![](img/e799c20ec5bf05cb1eaa8010d3705f4c.png)

4. 点击【点击添加对话流】开始添加工作流，然后点击【创建对话流】：

![](img/3e9ec2feb3d6865235ad8fa61cc18252.png)

![](img/983d751878a77c1df52d7d554f70801f.png)

#### 智能体工作流设置

1. 填写对话流信息：

对话流名称：`rednote_extract`

对话流描述：📕笔记信息提取

![](img/cf48a6b6bb1fe372e38f642b263b0911.png)

![](img/efd15f1638931be36be72c52db2e09c0.png)

2. 开始节点不用设置，鼠标移到开始节点右边中部，会出现个➕，点击➕添加一个插件节点：

![](img/7f4501a37557d03f4b0af83359840c6c.png)

![](img/56ea9b653d1f050e08e8659a85dbafd1.png)

3. 搜索工具集合，点击>展开，然后选择【`get_rednote_info`】：

![](img/da33294eeb2c7ee30f3bf64c99b2f6b5.png)

![](img/4a64a924bb8f8fbc3e2ebed6fb225576.png)

4. 设置【`get_rednote_info`】节点：

节点名称：`get_rednote_info`

输入设置：

`content`：引用【开始】节点的【`USER_INPUT`】

`cookiestr`：按需，可以不设置

![](img/0f30aea4dafeeed1d12d29803bb11cae.png)

5. 添加选择器节点：

节点名称：选择器

条件分支设置：

如果设置：

条件1：设置【`get_rednote_info`】节点的`flag`等于1

条件2：设置【`get_rednote_info`】节点的`type`等于video

两个条件是`且`

否则如果设置：

条件1：设置【`get_rednote_info`】节点的`flag`等于1

条件2：设置【`get_rednote_info`】节点的`type`等于normal

两个条件是`且`

否则设置：

否则无需设置：

![](img/2e8d500a5adce71df001052b609eba81.png)

5.1. 如果分支添加一个【字幕获取】插件节点：

![](img/c3eac1794f65989771e49d0d3e201ff2.png)

节点名称：`generate_video_captions_sync`

输入设置：

`url`：引用【`get_rednote_info`】节点的`video/url`

![](img/07db8b8231657762d28d89c6328edfb3.png)

5.2. 否则如果分支添加一个代码节点：

节点名称：设置`asr`空内容

输入设置：

无需设置

输出设置：

`asr`：`string`类型

![](img/41f159dd4b19c4944c10d8d252b7a663.png)

代码(Python)：

```
async def main(args: Args) -> Output:
    params = args.params
    asr = ""
    # 构建输出对象
    ret: Output = {
        "asr": asr
    }
    return ret
```

可复制代码：[`5.2代码.txt`](https://w63nbfedzw.feishu.cn/file/OrTlbp1MgoPpp9xNN5vcUhj4nxf?from=from_copylink)

5.3. 否则分支添加一个代码节点：

节点名称：设置空内容

输入设置：

无需设置

输出设置：

`collectedCount`、`commentCount`、`likedCount`、`shareCount`、`urlDefault`、`notetype`、`desc`、`userId`、`avatar`、`nickname`、`video_url`、`title`、`ipLocation`、`lastUpdateTime`、`tag_text`、`asr`：均为`string`类型

`imageList`：`Array<Object>`类型

![](img/035fe6a1ea99540c1569a5be9b4ead64.png)

代码(Python)：

```
async def main(args: Args) -> Output:
    params = args.params
    collectedCount = "无法获取"
    commentCount = "无法获取"
    likedCount = "无法获取"
    shareCount = "无法获取"
    urlDefault = "无法获取"
    notetype = "无法获取"
    desc = "无法获取"
    userId = "无法获取"
    avatar = "无法获取"
    nickname = "无法获取"
    video_url = "无法获取"
    title = "无法获取"
    ipLocation = "无法获取"
    lastUpdateTime = "无法获取"
    asr = ""
    imageList = []
    tag_text = "无法获取"
    # 构建输出对象
    ret: Output = {
        "collectedCount": collectedCount,
        "commentCount": commentCount,
        "likedCount": likedCount,
        "shareCount": shareCount,
        "urlDefault": urlDefault,
        "notetype": notetype,
        "desc": desc,
        "userId": userId,
        "avatar": avatar,
        "nickname": nickname,
        "video_url": video_url,
        "title": title,
        "ipLocation": ipLocation,
        "lastUpdateTime": lastUpdateTime,
        "asr": asr,
        "imageList": imageList,
        "tag_text": tag_text,
    }
    return ret
```

可复制代码：[`5.3代码.txt`](https://w63nbfedzw.feishu.cn/file/QGXGbYhmzoQ1Y8xPnamc1bbDnlh?from=from_copylink)

6. 添加变量聚合节点，详细配置如下：

![](img/20c9ee45253e702d1d4991f4bc52b195.png)

![](img/b789b18e6d19f11430b9bec41f9b8f8a.png)

![](img/050a306cd369f6fabe9de064319e177d.png)

![](img/5002ed43f9541f6eb7ab2c07668cd52f.png)

7. 配置结束节点，选择返回变量：

![](img/fc0e2859b1286947ecbad2dd07c816ee.png)

8. 输入一篇笔记链接测试下工作流，点击【试运行】：

```
https://www.xiaohongshu.com/explore/67daa906000000001e007d6d?xsec_token=AB160KrsrDX4GWgUibp6cRQ7qjDlyWLEHJ94-TsJAS_yM=&xsec_source=pc_feed
```

![](img/ea66cead653097de98e019480685fe4a.png)

![](img/8adb1367c720602bd88c9f0715ed146e.png)

运行成功工作流没问题。

9. 发布工作流，点击【发布】：

![](img/dc6cbcd0158c82bdf920acc356a7aa90.png)

![](img/c7d045c46cb35cc8e5c547b25f78248a.png)

### 开场白设置

开场白其实是否设置无关紧要，因为我们是要使用插件调用的工作流。

![](img/e5b872a480e81d81da200398ccf1e99a.png)

## 创建个人访问令牌

工作流配置好后就可以创建个人访问令牌了。

### 认证令牌获取

需要先在扣子平台获取令牌。

步骤说明：

1. 登录Coze控制台`https://www.coze.cn/`，点击左侧导航栏的【扣子API】：

![](img/fa2925a4a0fed14fcb451828d5c0446d.png)

2. 进入【授权 ➡️ 个人访问令牌】页面，点击【添加新令牌】按钮：

![](img/b6c446e35f2446cd1068dba803da9e97.png)

3. 输入令牌名称（建议命名与用途关联），设置令牌有效期、勾选【工作流】权限、选择要使用的工作流所在的空间，最后点击【确定】：

![](img/2d9f9e213b92fecb57865b8b03bd4da2.png)

4. 记录生成的`API key`（仅显示一次，需妥善保存）：

![](img/66b2120c9a8ceb06d79fdc3317dc91e0.png)

保存令牌信息到本地，后面飞书多维表格配置会用到。

注意事项：

+ 令牌权限范围需包含工作流操作权限
+ 泄露令牌可能导致数据安全风险

## 飞书多维表格设计

### 创建飞书多维表格

1. 在飞书个人空间新建个飞书多维表格，删掉非必要字段，设置第一个字段位【`笔记信息`】，名称改为自己喜欢的：

![](img/21851e443c2d2a3abbcb3e4c8199ad9a.png)

2. 添加一个字段，设置字段标题为【`📕信息获取`】，搜索字段捷径插件【`扣子工作流调用`】：

![](img/9d923a94277323556052937a56538acd.png)

### 字段捷径插件设置

1. 填写前面创建的扣子令牌到多维表格插件的认证令牌，工作流ID在我们编辑工作流时的链接里面有，`workflow_id`后面的值就是工作流ID，参数字段设置【`笔记信息`】列：

![](img/562b4ccac911950a2510a0834600f806.png)

2. 其他信息配置：

参数名称：`USER_INPUT`

请求模板：

```
{
  "CONVERSATION_NAME": "Default",
  "USER_INPUT": "$$USER_INPUT$$"
}
```

`$$USER_INPUT$$`是替换关键词。

输出结果1：`desc`

输出结果2：`asr`

输出结果x的内容表示获取工作流的哪个变量的值，只能获取2个，其他的需要通过完整响应数据获取。我这里是想获取视频的语音文案，因为视频作品一般没有标题，所以我就没获取。

获取更多信息：全选

自动更新：启用

3. 点击【确定】保存：

![](img/7f8d7ce3910153bae9934c4812fb4fff.png)

4. 点击【仅保存配置】：

![](img/e187f430ac006cfec7f115ad0b91195d.png)

## 飞书机器人流程设计

设计个飞书机器人流程把内容插入多维表格。

1. 电脑打开飞书，搜索【飞书机器人】：

![](img/4797d42276ae67c7f5e10f7b0dfbf951.png)

2. 点击【新建应用】：

![](img/b240bdce809363ee4c4402d1dedb07fa.png)

3. 设置应用名称、描述、图标，点击【确定】创建：

![](img/5eb714c4b5b0bfc704c9b4ab1094a8aa.png)

4. 点击【创建流程】：

![](img/e850b5bbbdc817965f40719a1f6bc9d8.png)

5. 选择触发器设置为【`Webhook` 触发】，记得保存好【`Webhook` 地址】，后面会用到：

参数设置为：

```
{
    "content": ""
}
```

![](img/5050235e4b478b80fec9fe13c8b0b27a.png)

6. 选择操作设置为【新增多维表格记录】，数据表选择我们创建的多维表格的数据表(不要通过多维表格名称去搜索，不能展开多维表格的数据表，不能操作知识库的，只能操作云空间多维表格)：

字段就选我们的【`笔记信息`】，值选择Webhook触发的`content`。

7. 点击【启用】，设置名称为【`📕笔记转存`】：

![](img/ff41ce34b59a516075ef420aa7d46f82.png)

![](img/cbfef30d6694a3b17fb0474baf60b5f6.png)

8. 点击【发布】发布流程：

![](img/74ad7c73614076594b4012edfd318367.png)

![](img/a77795c12675641c1a81dddebc0ba2d1.png)

![](img/e4adc1c20d3456a65ccbaa0f7b989a1d.png)
