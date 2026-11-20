
# Midjourney - AI绘画最强软件

> 来源：[https://my.feishu.cn/docx/LQPbdmiSKouZkOxjy9YcIOMMnNh](https://my.feishu.cn/docx/LQPbdmiSKouZkOxjy9YcIOMMnNh)

## 需要订阅MJ才可使用

如果要在🛰️机器人上使用Midjourney，需要2个步骤。

1. 安装 `midjourney-proxy` 环境，是基于Discord的API，等于联通后🛰️和Discord建立链接，可以互通消息。

2. 安装 `midjourney-proxy-on-wechat` 插件

### 注：亲测代理方式容易频繁掉线，会导致Discord频繁被监

[Coze.com（国际版）](https://tffyvtlai4.feishu.cn/wiki/NBEQwc2xvi05pmkRZx3cjh15nib#WOx9di09Yoc0cgxbFCVc8LF0nYb)需要修改密码，建议使用新加坡服务器，参考中的服务器申请教程，价格每月30元。

## Midjourney - Proxy

### 订阅Midjourney

首先需要确认自己已经订阅了Midjourney，并创建自己的服务器和频道，参考https://docs.midjourney.com/docs/quick-start

### 获取用户Token、服务器ID、频道ID

#### 获取用户Token

进入频道，打开`network`，刷新页面，找到 `messages` 的请求，这里的 `authorization` 即用户Token，后续设置到 `mj.discord.user-token`

![](img/bb02b8db1dc944738989ecb3530ba429.png)

#### 获取服务器ID、频道ID

频道的`url`里取出 服务器ID、频道ID，后续设置到配置项

![](img/a808c806c63abedee78c9a2765f01eee.png)

#### Docker部署

1. `/xxx/xxx/config`目录下创建 `application.yml`(`mj`配置项)、`banned-words.txt`(可选，覆盖默认的敏感词文件)；参考`src/main/resources`下的文件

![](img/c13f99449f88b54d4d99bd1c042fe8db.png)

配置参考（填写时删除汉字备注）：

```yaml
mj:
  accounts:
    - guild-id: xxx #discord服务器ID
      channel-id: xxx #discord频道ID
      user-token: xxxx #discord用户Token
```

全部配置项（按需添加到上面的配置参考中）：

1. 终端中启动容器，映射`config`目录，第3行更换成实际路径，第4行中注意版本信息，会不断更新。

```bash
docker run -d --name midjourney-proxy \
 -p 8080:8080 \
 -v /xxx/xxx/config:/home/spring/config \
 novicezk/midjourney-proxy:2.6.3
```

1. 确认Docker状态

![](img/39988b6c35fcf7304f70b2d3546413c3.png)

1. 开通防火墙

![](img/8ddbca9f9fd1f52e2866535ee63b0677.png)

## Midjourney - Proxy - On - Wechat

### 插件安装说明

+ 机器人私聊，一行一行输入指令，安装插件

```shell
#installp https://github.com/mouxangithub/midjourney.git
#scanp
```

+ 安装成功后，输出输入`$mj_help`有提示说明插件安装成功

+ 输入`$mj_admin_password` 密码（未配置`mj_admin_password`则临时密码为123456，认证完后请尽快输入`$set_mj_admin_password`进行修改）

+ 输入代理地址，如果你和我一样使用了海外服务器，那么只需要设定一个代理地址：

`$set_mj_url` mj代理地址

`mj_api_secret`请求参数

`discordapp`代理地址 进行设置MJ服务器信息

![](img/e44e179abcd338911614f330a84919e7.png)

+ 无需其他配置，直接使用即可：

![](img/115ef01098ecc3a18502c22ce89da257.png)

### 指令说明

#### 通用指令

+ `$mj_help` 说明文档

+ `$mj_admin_password` 口令 进行管理员认证(如未配置，临时管理员密码为123456)

+ `$mj_admin_cmd` 查询管理员指令

#### 管理员指令

+ `$set_mj_admin_password` 新口令 进行设置新密码（此方式会直接写入`config.json`方便重启直接使用）

+ `$set_mj_url` mj代理地址 `mj_api_secret`请求参数 `discordapp`代理地址 进行设置MJ服务器信息（此方式会直接写入`config.json`方便重启直接使用）

+ `$stop_mj`: 暂停MJ服务

+ `$enable_mj`: 启用MJ服务

+ `$clean_mj`: 清除MJ服务缓存

+ `$g_prefix` : 查询前缀

+ `$s_prefix` 前缀类名 前缀: 设置前缀

+ `$r_prefix` 前缀类名 前缀或序列号: 移除前缀

+ `$set_mj_admin_password` 口令: 修改管理员口令

+ `$g_admin_list` : 查询管理员列表

+ `$s_admin_list` 用户ID或昵称: 设置管理员列表

+ `$r_admin_list` 用户ID或昵称或序列号: 移除管理员列表

+ `$c_admin_list` : 清空管理员列表

+ `$g_wgroup` : 查询白名单群组

+ `$s_wgroup` 群组名称: 设置白名单群组

+ `$r_wgroup` 群组名称或序列号: 移除白名单群组

+ `$c_wgroup` : 清空白名单群组

+ `$g_wuser` : 查询白名单用户

+ `$s_wuser` 用户ID或昵称: 设置白名单用户

+ `$r_wuser` 用户ID或昵称或序列号: 移除白名单用户

+ `$c_wuser` : 清空白名单用户

+ `$g_bgroup` : 查询黑名单群组

+ `$s_bgroup` 群组名称: 设置黑名单群组

+ `$r_bgroup` 群组名称或序列号: 移除黑名单群组

+ `$c_bgroup` : 清空黑名单群组

+ `$g_buser` : 查询黑名单用户

+ `$s_buser` 用户ID或昵称: 设置黑名单用户

+ `$r_buser` 用户ID或昵称或序列号: 移除黑名单用户

+ `$c_buser` : 清空黑名单用户

+ `$s_limit` : 设置每日作图数限制

### 配置说明

前往插件目录`/plugins/midjourney`

找到`config.json.template`，复制成`config.json`，并进行修改，修改后重启插件即可。

```json
{
    "mj_url": "", // midjourney-proxy的服务地址
    "mj_api_secret": "", // midjourney-proxy的api请求头，如果midjourney-proxy没配置此处可以不配
    "mj_tip": true, // 是否发送请求提示，让漫长的等待不会枯燥，如果嫌啰嗦可关闭，即：发送一些成功的内容
    "mj_admin_password": "", // MJ管理员密码
    "daily_limit": 3, // 普通用户每日作图数
    "discordapp_proxy": "", // cdn.discordapp.com反代地址
    "imagine_prefix": "[\"/i\", \"/mj\"]", // imagine画图触发前缀
    "fetch_prefix": "[\"/f\"]", // fetch任务查询触发前缀
    "up_prefix": "[\"/u\"]", // up图片放大和变换触发前缀
    "pad_prefix": "[\"/p\"]", // 垫图画图触发前缀
    "blend_prefix": "[\"/b\"]", // 混图画图触发前缀
    "describe_prefix": "[\"/d\"]", // 图生文触发前缀
    "queue_prefix": "[\"/q\"]",  // 查询正在执行中任务触发前缀
    "end_prefix": "[\"/e\"]",  // 结束存储打包发送任务（目前用于混图）触发前缀
    "reroll_prefix": "[\"/r\"]"  // 重新绘制触发前缀
}
```

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>
