# 敲敲手机就能一键去水印收藏"抖音"作品信息并同步到飞书表格全套教程

> 来源：[https://xuqiwei1986.feishu.cn/docx/QijEdbj9MoFBf0xdZuFcXA2Kndg](https://xuqiwei1986.feishu.cn/docx/QijEdbj9MoFBf0xdZuFcXA2Kndg)

# 效果展示

![](img/c69b7c63f7756191da0ba94e6537ef9f.png)

![](img/c4d17c0f706a2cb48a1c8b166afd4565.png)

[copy_8B5951A3-3EDC-45E1-A16E-3C4C27D62617.mov](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/D3u6bN4wKoiI1LxJsR1cgfyunGh)

![](img/bedfc5d0f5cf68dca3b8ff6d2eada3a8.png)

> 最近在星球看到很多小伙伴实现各种快捷指令到飞书文档，不过没看到抖音收藏到飞书的。我来补充个

# 配置流程

![](img/0955b5614c80a04cede87d6d315dd649.png)

## 1.注册飞书

![](img/0e4ab07a6b7b21317742da5d281c7726.png)

### 创建多维表格副本

[【副本】抖音 to飞书收藏](https://vpbq24ngx4.feishu.cn/base/WItSbqaYvaJyYMsUTInc8PTsnJb?table=tblVoiCC7Yhsi9rs&view=vewnUAkX6L)册完后，打开我的副本🔗👉：

创建副本，保存进你自己的飞书文档里

![](img/e7a1af2c73f291d761d18508b11158d5.png)

### 记录下多维表格的访问凭证token和tableid

后面需要填写

> **token**:在多维表格的浏览器上，在base/123?,其中123就是token

![](img/406f25cb275ff814532f1d6cfb3fba4e.png)

> **tableid**：在多维表格的浏览器上，在table=456&,其中456就是tableid

![](img/01aaebdfb755fed7bea05386fe7eabf2.png)

## 2.注册coze

coze官网链接🔗:https://www.coze.cn/

### 创建智能体

名字按你自己意思取就行。

![](img/1826da86e6c488982bfcc9152c446928.png)

![](img/69be54618ada1307c7533fa9853c18c0.png)

![](img/b05d631fb71e5cf1bcd75f02143f0013.png)

![](img/687435e016284b5618a104057c3f72a3.png)

![](img/3e58f82758ab4a4ae962767d091c899b.png)

### 配置工作流

![](img/fedef43a6cefddc775b8f6146417e4e3.png)

#### 添加插件「抖音短视频链接转换」

![](img/10266c4c1b5735e9daad6d26b27c2e0a.png)

![](img/afecce18de1f66ac6e2f39cbd0994f15.png)

#### 添加插件「douyin_link_reader」

![](img/a5e47924749f686aa542c4bf2699cf8e.png)

![](img/b374fe5e067cf012c78482550c829336.png)

#### 添加插件「抖音视频图集无水印下载」

![](img/9ab222f57a67bed3d20fa13370eef97c.png)

![](img/b5ff44e06465c8e880c385e6ecf9f250.png)

#### 添加节点「代码」

![](img/07a55957f00dc446166102b18fc933fc.png)

![](img/478b9a30494fa648cb75a0ae1aee5876.png)

> 点击「代码」节点，「输入参数」位置新增：data，yw_url ，content，content_type。

> 「输出变量」位置，填一个record。类型为string.看视频操作

![](img/2a15926cb90c6811d780d79ab54d8e35.png)

[20250330_120747.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/ISEabgxdPoK3KexNtNvcEjxfnRc)

#### 复制填写代码

点击链接🔗复制代码👉

> 先将代码的语言切换成pyhton，如下图。然后将上面复制来的代码。全部进行替换进行

> 最后直接关闭代码编辑窗口即可，会自动保存

![](img/05f5bf7ed0cc382164d1be49ab44306e.png)

![](img/3833489b1b575695a60717fead8abf9f.png)

#### 添加插件「飞书多维表格」

![](img/01d3501bca5ec5e76ff2484fe56f7d4b.png)

![](img/1dbbfb8de5c218434f52a43416d376b4.png)

![](img/aa729c93b984daf2ba375ae2d366cad0.png)

![](img/07d19781df746268135b156f8d0ddedf.png)

#### 配置结束，可以试运行下.没问题就点击右上角的「发布」按钮

![](img/c068d43a0cc6fc01e0d36406d2822046.png)

![](img/c16cc246572b156ef73940bdf124ec1f.png)

[copy_6DB9FAFB-ED90-4870-A91F-D0E7C848AA66.mov](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/PrZsb3TC3oFXqExq1ndct1w4nwf)

> 其实到这里工作流已经搭建完成了。剩下的是配置手机快捷指令部分

#### 获取coze的访问凭证

![](img/3232398d6409d0ea9f0cdcfbabc7c9e9.png)

![](img/c1d3c560a7fbf1861fb2e35ddeb93662.png)

![](img/9fa340cc6c33f6a2b7212d9a3ce19b14.png)

![](img/216d898743548766575fc989bfaa09cc.png)

![](img/9beacf8e4f5ec0c47166f3e76452f4e4.png)

![](img/93d91ea9fd178510bf626771787df247.png)

![](img/ca168d262581091d3df28f4f6cb41d07.png)

##### 访问凭证测试

![](img/23e6511f23a601c67879279424564cc4.png)

> 第一次运行，会返回下面结果。复制文档里，点蓝色的链接进行授权

[parameters参数填写](https://vpbq24ngx4.feishu.cn/docx/Sa3fd7orAo9LsDxtUT8chSdunpf)aramenters:参数复制🔗👉

![](img/64bd0c8ca0d3c84da8cc0307018574cf.png)

![](img/f1066944434431025d05b0bbd34b40e6.png)

##### coze的访问令牌token

这里创建的令牌就是token了。保存一下

##### 工作流id

![](img/eff0e8eb41e7e33c44b0b217e5ea7336.png)

# 3.配置iphone快捷指令

快捷指令链接🔗：https://www.icloud.com/shortcuts/4ad51015dd4045d59de775f9e8c3e5c7

点击保存为你自己的，

然后打开，编辑拉到最底下

注意这里口令前有个前缀Bearer 空格。不要删

```
Bearer pat_XXX
```

![](img/95b30d176f5d78b26f6283f8d8dd5e6b.png)

## 设置触发方式

> 可选，双击背面、三击打背面。或者辅助按钮

![](img/53b2194fc0a3a732b929bc4674a2c790.png)

![](img/02e756d844146fd1168ad121b5695912.png)