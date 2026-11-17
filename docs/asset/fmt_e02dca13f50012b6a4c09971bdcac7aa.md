
# RustDesk编译自动配置工具

> 来源：[https://xodnytdcaw.feishu.cn/docx/X9DadoZPNoqgMExqAO2caIiqnXC](https://xodnytdcaw.feishu.cn/docx/X9DadoZPNoqgMExqAO2caIiqnXC)

## 准备工作

```
✅编译服务项目
1.中继续服务器（基本）
2.API服务器（可选）
3.固定密码（可选）
4.隐藏连接窗口（可选）
5.充许远程修改配置（可选）
6.隐藏底部广告（可选）
```

> RustDesk中继服务器自己不会搭建的或者嫌麻烦的，可以找我买

> 50/年  带APi

> 68/年  带APi+编译客户端

> 联系🛰️：dqtx33

### 路径地址

`flutter\lib\desktop\pages\connection_page.dart`

自行修改找到81-110行，使用如下代码替换。

⚠️从81行代码最左边选择

```
Widget setupServerWidget() => Flexible(
   child: Offstage(
     offstage: !(!_svcStopped.value &&
         stateGlobal.svcStatus.value == SvcStatus.ready &&
         _svcIsUsingPublicServer.value),
     child: Row(
       crossAxisAlignment: CrossAxisAlignment.center,
       children: [], 
     ),
   ),
 );
```

![](img/68424a072f91802416fece9375ccc70d.png)

出来的效果`80`与`92`行代码空，`w`与`b`为同一垂直线

![](img/d349c9db5b7730af82f361bef75e2c19.png)

## 编译客户端

打开配置工具：[点击打开](https://r.vumstar.com/)

操作教程：https://www.bilibili.com/video/BV1MibBz5Emy/?spm_id_from=333.1387.upload.video_card.click&vd_source=206031f494850e57fd6c92ace02b1bed

> 在编译之前，[点此直达](https://github.com/dqtx760/1269/settings/actions)开放Actions工作流权限，需要开

+   读取权限

+   允许 GitHub Actions 创建和批准拉取请求

![](img/34b6f09b16139a1a26c4a8eb843b2f68.png)

编译时选用Flutter Tag Build VUMSTAR 这个工作流编译

![](img/4835475baee6d47fefae3c51a60e18e7.png)

### 交付文案

```
亲，客户端已帮你编译完成
github下载地址：https://github.com/dqtx760/1269/releases/tag/master
网盘下载地址：http://4275.com/1knrgi
固定密码：1144

✅编译说明
1️⃣中继服务器编译
2️⃣API服务器编译
#控制端登需登陆才有控制权限，被控端无需登入
3️⃣固定密码
#控制端连接被控端无需对方提供密码，输入1144即可建立远程
4️⃣隐藏连接管理窗口
5️⃣隐藏底部就绪处超链接广告
```
