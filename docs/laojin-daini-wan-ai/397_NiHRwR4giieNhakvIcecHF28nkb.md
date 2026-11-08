# Send_msg - 群发消息功能

> 来源：[https://my.feishu.cn/docx/AgFldhJdaocjfQxl2mMcYYTTnyc](https://my.feishu.cn/docx/AgFldhJdaocjfQxl2mMcYYTTnyc)

## 转载自Github官网：https://github.com/Isaac20231231/send_msg

# **file_api服务**

## **简介**

file_api 插件用于chatgpt-on-wechat项目。

实现手动发送消息通知到🛰️功能，写入json消息数据到data.jaon文件里，

## **python版本**

python3.11 目前发现有低版本不兼容的问题，建议使用大于3.10版本

## **安装**

此插件作为🛰️聊天机器人系统的一部分，需要将其放置在正确的插件目录下：

### **安装方法（三种方式）**

下载插件,访问插件[仓库地址](https://github.com/Isaac20231231/send_msg)

#### **第一种:手动下载压缩包,将****send_msg** **文件夹复制到您的聊天机器人的** **plugins** 目录中（注意需要修改文件夹名称，会自动带上main分支）。

#### **第二种:🛰️执行命令**

```
#installp https://github.com/Isaac20231231/send_msg.git
#scanp
```

#### **第三种:进入****plugins** **目录克隆**

```
git clone https://github.com/Isaac20231231/send_msg.git
```

### **注意事项**

1.  确保 init.py,file_api.py和send_msg.py 文件位于 send_msg 文件夹中。

1.  安装插件相关依赖 pip install -r requirements.txt。

```
cd plugins/send_msg
pip install -r requirements.txt
```

## **data.json文件介绍**

file_api 服务数据依赖于 data.json 文件进行写入。

```
{
&quot;data_list&quot;:[
{
 &quot;receiver_name&quot;: [&quot;微信备注名1&quot;],
 &quot;message&quot;: &quot;发送消息&quot;,
 &quot;group_name&quot;: []
},
{
 &quot;receiver_name&quot;: [&quot;微信备注名1&quot;,&quot;微信备注名2&quot;],
 &quot;message&quot;: &quot;发送消息&quot;,
 &quot;group_name&quot;: []
},
{
 &quot;receiver_name&quot;: [],
 &quot;message&quot;: &quot;发送消息&quot;,
 &quot;group_name&quot;: [&quot;群名1&quot;,&quot;群名2&quot;]
}
],
{
 &quot;receiver_name&quot;: [&quot;微信备注名1&quot;],
 &quot;message&quot;: &quot;发送消息&quot;,
 &quot;group_name&quot;: [&quot;群名1&quot;,&quot;群名2&quot;]
}
]
}
```

### **参数说明:**

```
receiver_name: 接收者的微信备注名，可以是多个
message: 消息内容
group_name: 群聊名称，可以是多个
```

发送个人消息时，group_name为空，填写receiver_name,message即可。多个时列表逗号分隔即可

发送群聊消息时，group_name,message必填,receiver_name可选

填写🛰️备注名时(支持多个)，发送@某人消息，填写所有人发送@所有人消息,不填写不@。

## **使用**

安装并正确配置插件后，您可以通过以下方式使用：

打开postman，请求api接口"http://127.0.0.1:5688/send_message"

注意: 127.0.0.1是本机ip，如果是部署服务器要改成服务器ip地址，5688是端口号，如果修改了端口号要改成对应的端口号（端口号可以在run_flask_app() 修改端口启动） 发送消息到🛰️

```
{&quot;receiver_name&quot;: [&quot;微信备注名1&quot;],
  &quot;message&quot;: &quot;这是一条测试消息&quot;,
  &quot;group_name&quot;: []
}
```

成功返回：

```
{
  &quot;message&quot;: &quot;发送成功&quot;,
  &quot;status&quot;: &quot;success&quot;
}
```

异常返回参考send_msg.py文件里的validate_data函数

![](img/c9650931b5319e04751ea4230150170b.png)

# **send_msg插件介绍**

## **注意事项**

发送个人消息时，一定要有好友关系，否则无法发送消息。

发送群聊消息时，如果出现找不到群聊的情况，要把🛰️群聊设置成通讯录群聊。

## **第一种使用**

send_msg.py插件采用watchdog监听文件变化，和file_api服务相互使用，

file_api负责写入文件，send_msg负责监听文件，有内容时触发发送消息到🛰️。

### **命令说明**

send_msg支持以下命令：

+   $start watchdog 开启监听

+   $stop watchdog 停止监听

+   $check watchdog 查看监听状态

![](img/2ee38cab1a415756d27ef3dc17e47322.png)

### **使用方法**

插件默认不启动监听，需要手动启动监听，监听文件为data.json文件，当data.json文件有内容时，触发发送消息到🛰️。 启动方式看上方,成功效果如下

![](img/159273b56d1a11153c0834924db2e01d.png)

## **第二种使用**

send_msg.py插件可以使用🛰️命令来发送消息到🛰️，不需要file_api服务，

发送消息分两种方式，一种是发送个人消息，一种是发送群聊消息。

### **命令说明**

send_msg支持以下命令(支持一次性发多人,单人时列表只填一个即可)：

+   $send_msg [🛰️备注名1,🛰️备注名2] 消息内容 发送个人消息

+   $send_msg [🛰️备注名1,🛰️备注名2] 消息内容 group[群聊名称1,群聊名称2] 发送群聊消息,并且@某人

+   $send_msg [所有人] 消息内容 group[群聊名称1,群聊名称2] 发送群聊消息,并且@所有人

+   $send_msg [] 消息内容 group[群聊名称1,群聊名称2] 发送群聊消息，不@任何人 注意:$send_msg后面是2个空格

![](img/4a64f93374e3db601f525cfb04ffc075.png)

+

![](img/b995597967cf027c304f791295d83425.png)

## **更新日志**

### V2.3 （2024-09-05）

+   1.增加channel判断，兼容[w](https://github.com/Tishon1532/chatgpt-on-wechat-win%EF%BC%89%EF%BC%8C)in版本机器人的ntchat发送消息（https://github.com/Tishon1532/chatgpt-on-wechat-win），

+   支持itchat和ntchat两种channel类型，注意ntchat目前还没解决群聊@所有人的场景，只能@单个人，另外ntchat的receiver_name只支持填写🛰️名字

### V2.2 （2024-08-15）

+   修改了插件名字和文件夹一致

+   api服务修改逻辑，不注册插件，采用函数调用的方式启动api服务

+   ps:注意因为此次修改了插件名称，之前/plugins/plugins.json目录下的file_writer和file_watcher两个插件名字需要删除

### v2.1 (2024-07-19)

+   优化兼容发送好友消息，先查找🛰️备注名，找不到再查找🛰️昵称，receiver_name支持填写🛰️备注名和🛰️昵称。

+   优化了发送群聊消息的逻辑，之前一定要加好友才能@指定人，现在不需要加好友也可以@指定人。（先从群聊里找🛰️名，找不到通过好友列表找🛰️备注名，备注名没有再找🛰️昵称）

### v2.0 (2024-07-18)

+   新增支持发送图片、视频和文件的消息格式（文件内容传参http或者https的url）。

### v1.5 (2024-05-29)

+   新增支持🛰️命令发送消息功能。

+   优化了文件监听的模式，使用了python看门狗模式监听文件变化

+   更新了文档，添加了更多的使用示例。

+   修复了一些已知的 bug。

### v1.0 (2024-05-24)

+   初始插件版本发布，支持基本api触发消息发送🛰️功能。

+   提供了简单的配置选项和说明文档。

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>