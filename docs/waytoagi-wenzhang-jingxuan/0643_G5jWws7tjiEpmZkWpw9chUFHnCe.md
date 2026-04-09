# 画小二：通过Coze定制开发插件案例：根据食物识别营养热量

> 来源：[https://waytoagi.feishu.cn/docx/Gcp8dCasUoD73ExIdIJcBRZ9nLe](https://waytoagi.feishu.cn/docx/Gcp8dCasUoD73ExIdIJcBRZ9nLe)

> Create By：画小二

> [通过Coze定制开发插件案例：根据食物识别营养热量](https://a1i1hjmvcf0.feishu.cn/docx/T6vndRhVNoM9M4x0qx3cZDOmnTf)

# 一、为什么要自己开发插件

问题：目前扣子市场的AI插件不完善，无法实现个性化、定制化需求；

目标：学会通过API接口形式开发自定义AI功能，满足各种需求。（后面课程会介绍基于IDE模式，自由度会更高）

# 二、创建插件

## 1、创建插件

打开扣子官网：https://www.coze.cn/home

![](img/d82adc6cc6c34ec715fa1ba5dba46982.png)

在个人空间的插件中点击创建插件

![](img/a08174b60327946619ed21555ce2fc05.png)

## 2、插件基本配置

选择基于已有的服务创建，起号名字

![](img/f6e54ca343dda4a5bb668a686aea1e78.png)

看到下面不要被劝退，虽然很多不明白，比这葫芦画瓢

插件URL https://apis.tianapi.com

授权方式选择 Service

位置选择：Query

Parameter name 参数名字填写 key

Service token / API key 填写自己的，目前表单是是画小二个人的，如何获得请往下看。

![](img/91017d041f68422cb87fb382143bd072.png)

## 3、如何获取API key

天聚数行的API市场，在数据智能分类中找到实物营养识别，免费白嫖20条。

![](img/e0b1f0ef1f83d32de97d4bba34db6043.png)

![](img/d2a98c4c371d98034fa314dbfa6b1445.png)

登录注册之后可以看到如下界面

![](img/460f7dec790ced58bd161a5c22d96a14.png)

看清楚（网址不要全部复制），将对应内容复制进去

![](img/9c4586cccc0b243dfc7d198ec01226ac.png)

## 4、插件详细参数配置

这里容易迷糊，我不是创建了嘛！为什么还要创建工具？这里要参考画小二课程详细解释这里的原理，搞清楚这里就有醍醐灌顶的感觉。

![](img/5b6d496ca85edda99c5577605f1b3729.png)

如图所示，名字自己起，其他的就不要问为什么？想问就去加群~（底部有二维码）

![](img/fc72f14a4ae24578ee0fee06e6d35557.png)

![](img/915b99bce984e547c51ef5e710d0b721.png)

用图片链接https://www.tianapi.com/static/img/ocr/kc.jpg 自动解析，测试接口并自动获取参数

![](img/49c1182b1457ee4e60bfe115271911cb.png)

![](img/37f1e6ae2a6d1713384867682b46205b.png)

对比一下输出

![](img/dc550cd49e7471326222d0619efa26c9.png)

插件中解析的数据

![](img/ddd2865549197e96973bbfe3b92a6c4b.png)

在参数值输入图片地址，点击运行，则可以看到右侧代码解析器里面将图片地址解析出来

![](img/d838e5c5a3b277fa0fc884ebba8e5a3a.png)

通过Resopnse可以查看响应值，同时也提示调试通过。

![](img/6f27605f98936d4f73933b2cbc474c66.png)

通过之后就可以发布自己的插件了

![](img/82b4e33aeac1a1ce4303ee66fda9fca8.png)

## 5、插件发布

![](img/891692e81c5bf40b500702ef76d2e42a.png)

选择一项，不用收集客户敏感信息

![](img/8d7adace572a9369fe9b88bbb7142556.png)

# 三、插件使用

## 1、创建智能体

![](img/9bb89f759eb96ea01a20e69e8924b672.png)

## 2、添加插件

![](img/a9e6728cc2e7809738c0f981876eef91.png)

![](img/99c18d1dca3dbe1660cb88acf122e957.png)

![](img/eb0f902fcec383cc85445cef0b012bcd.png)

```
# 角色
你是一个能够通过图片识别食物热量的智能体，可精准分析用户上传图片中食物的卡路里热量。

## 技能
### 技能 1: 分析食物热量
1. 当用户上传食物图片后，你需通过调用“FoofScanCalorie”插件来分析图片来确定食物的卡路里热量。
2. 对于无法确定的食物，可要求用户提供更多信息或进行其他辅助操作。
3. 请输出食物的名字和热量数值。

## 限制
- 仅处理与食物热量识别相关的任务，拒绝其他无关请求。
- 需用户上传图片才能进行热量分析。
```

也可以发布到自己微信中，成为一个专职的客服技能。

# 四、发布使用

![](img/49363844c8fb9d3745c55a567510841c.png)

成功查询出食物为羊肉串，热量为430千卡。

![](img/b7398e60ad706c83e94625df5170b271.png)

![](img/62fdad8e8d153da0c12c01bd7b08b25f.png)

# 五、粉丝交流学习群

![](img/2139e76be0d91f360c79f0d11dc00328.png)

AIGC商业案例实操课

![](img/e0ff5bc17cdc6c2b2363a4b844d1bae8.png)

画小二微信交流群

![](img/0ae164fbee43647af4c38bb07001f945.png)

画小二微信公众号

# 六、手把手教程

## 1、会员详细教程

根据以上内容若还是无法掌握该技能，本节视频有35分钟的手把手视频会员教程：

![](img/029f12098538a4666bcac97cf076046b.png)

![](img/b46ea299785e1a2765a4f0122f2146f9.png)

![](img/30c0f1ee04e8e99dece507ff2cdb5bb5.png)

![](img/707f132c13e13072530e04b3b0e64909.png)

## 2、AIGC商业案例实操课海报

![](img/19097b93d17a47ceef7e8f62e7353163.png)

备注：本文档教程配套项目文档，版权归画小二所有，禁止商用。