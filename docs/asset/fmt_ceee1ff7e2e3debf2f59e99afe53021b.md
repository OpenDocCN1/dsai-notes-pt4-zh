
# 2025-03-14📕一键提取图文标题到飞书多维表格

> 来源：https://xuqiwei1986.feishu.cn/docx/OrFCd1kFjovBz0xOTRrcKOjOnWd

> 🔗 原文链接：https://scys.com/articleDetail/xq_topic/4848115458812548

各位赚钱的圈友，大家好，我是富贵。

长安文化创始人，公司的主营业务是📕高客单线索类，目前自持素人矩阵号3000➕，MCN合作达人10000➕。

最近又和赚钱联合创办了AI MCN公司，需要大量的内容创作，近期在AI工作流上动了很多脑筋。

因为是流量型公司，所以对技术的一切理解都基于如何更好的降本增效，为了提高公司小伙伴们的效率，一直在“📕黑科技”上，苦心钻研，外部采买➕自主研发，属于资深的老韭菜了。

因为我们的技术同学不是专业搞AI的，也许文档里会有很多本可以简化的弯路，如果AI行业相关的圈友，还请大家帮忙多提建议呀。

# 图文配置教程（苹果手机版）

## 总流程

核心逻辑就是通过快捷指令调用API接口执行coze工作流，需要使用coze的`token`+`workflowid`

第一步：coze工作流做的工作是识别app的短连接还原成长连接或者复制的就是长连接，然后请求📕接口

第二步：返回相关参数，通过JavaScript脚本整合json保存变量然后调用飞书表格API接口

第三步：飞书表格接口需要授权+请求表格`base`路径+表格id

第四步：飞书表格我共享的模版会自动把图片链接转附件加入飞书表格显示封面

第五步：根据我共享的表格模版会自动进行标题和文案的AI（DeepSeek）创作

最终效果呈现苹果手机点击📕笔记链接，一键同步到飞书多维表格，因为教程是刚写，所有还有很多复杂的场景都可以实现，比如直接拆解爆款视频，爆款图文，进行二创，根据比较生成词云等等，待完善

## 一、飞书账号准备

飞书官网：https://www.feishu.cn/

如果已经注册过飞书账号，这一步跳过，直接看第二步。

### 1.注册飞书账号

1.1、访问 [https://accounts.feishu.cn/accounts/page/login?app_id=11&no_trap=1&redirect_uri=https://www.feishu.cn/login_redirect&template_id=7159153320657698818](https://accounts.feishu.cn/accounts/page/login?app_id=11&no_trap=1&redirect_uri=https://www.feishu.cn/login_redirect&template_id=7159153320657698818)

1.2、填写手机号完成注册

### 2.创建企业

2.1.1、登录后点击"创建企业"

2.1.2、填写企业名称等信息（信息随便填就行，对使用没有影响）

2.1.3、完成企业创建

## 二、创建多维表格

1.打开模板链接（需要运营提供模板链接）。要先登录飞书账号在打开哈。

[📕笔记coze笔记收藏](https://ccn5rcywl0hv.feishu.cn/base/IWOYb11Mxa5vRhsfF8ScIZtknqd?from=from_copylink)

2.点击上面的模板链接后，稍等一会会自动弹出模板弹框，点击‘使用该模板’。

点击云盘然后新建多维表格

![](img/3d2dfee75a7b6da099c2a2384cb39c31.png)

3.模板就会自动保存到你的账号。

![](img/3c98e622f8d8a8987f6b9b9c00df34a0.png)

4.确认一下模板链接是以`xxx.feishu.cn/base/`开头的。如果不是的话，要删除模板，重新创建多维表格（就是按第一步重新操作），不然用不了哈。

![](img/812343745de5c4ee105f0adb7a4b8677.png)

5.点击右上角‘分享’，弹框下面的权限把‘可阅读’切换到‘可编辑’。

![](img/80c0fd891ffae32d71249406dbe53acb.png)

6.在浏览器顶部多维表格链接里，记录以下信息：

复制 URL 中的 `多维表格Token`（即 `base/` 后面的字符串）

复制当前表格的 `表格ID`（即 `table=` 后面的字符串）

**这两个信息要保存好，等会用到。**

然后就是coze注册

![](img/68da3874e8cec5c1fd3c266fd0fdc132.png)

## 三、coze工作流配置

使用飞书或者抖音扫码都可以

![](img/7d21f446c47a033ffd70bb931c2f5deb.png)

然后coze工作流配置扫码登录创建工作流

![](img/b49a22ea81822c31ef80e1612c081816.png)

填写建立

![](img/f6abe4fba1c0a52e001a29435e78498e.png)

![](img/b1f53676d74021396a18c37116a5bc65.png)

![](img/40a41e884ee407895a11ef36ce33330e.png)

![](img/dcf2f70b664a4bd0eda74108f6e14a26.png)

![](img/000d8161daeef67d486ebb1f78e9efcd.png)

![](img/e99e4b0a619b63345554ac5cec1c5f67.png)

![](img/5e58563ac62ce2af366eb1e29cfe3156.png)

![](img/a21a83cec6c7b6215a188b776df71da5.png)

![](img/246307e3b5e91c4808b5c0b179f9ce86.png)

### 代码内容

```
// 在这里，您可以通过 ‘params’ 获取节点中的输入变量，并通过 'ret' 输出结果

// 'params' 和 'ret' 已经被正确地注入到环境中

// 下面是一个示例，获取节点输入中参数名为‘input’的值：

// const input = params.input;

// 下面是一个示例，输出一个包含多种数据类型的 'ret' 对象：

// const ret = { "name": ‘小明’, "hobbies": [“看书”, “旅游”] };

async function main({ params }: Args): Promise<Output> {

// 构建输出对象

const list =[{"title":params.title,"nickname":params.nickname,"desc":params.desc,"imageUrl":params.imageUrl,"noteid":params.noteid}]

let records= [];

for(let item of list){

let record ={"fields":"{}"};

let fields = {};

fields.标题 = item.title ;

fields.作者 = item.nickname ;

fields.文案 = item.desc ;

fields.图片链接 = item.imageUrl ;

fields.帖子id = item.noteid ;

record.fields = JSON.stringify(fields);

records.push(record);

}

const ret = {

info: records

}

return ret;

}
```

![](img/94134dea299b09ec0fc81bc61f1a24d8.png)

![](img/eed1cc46362a9260ddb3b1e7c7dfd63f.png)

关键步骤使用coze的接口调用实现跨平台保存笔记

![](img/000bc3a40b653337f73f1823045bf47d.png)

通过coze的调试验证API是否可以试用

![](img/cc78afb071e32bf86e4bbc405398e225.png)

使用接口调用需要获取`token`和`workflowid`

![](img/313e194f85c716e79fd1c40f740f08b1.png)

![](img/88042fd58a4375a7269ea0cda56aa161.png)

![](img/e9d37e5983c8eedb06a693aafd03c9b8.png)

![](img/3c8de40a79234f37aee009b8e4b0c7bb.png)

![](img/f10f579b6e4cd4464a09f090afb5e691.png)

给个人空间工作流添加授权

![](img/ba3aeb63f62f0c4585aceec9f7ccc964.png)

### 四、随便填写一个📕笔记链接

[https://www.xiaohongshu.com/explore/67bc5f55000000000900e3dc?xsec_source=pc_user&xsec_token=ABg1aRY_LIGGAmq12SC74oFzz6Ig-ZCelcWkBM2te5YyA=&xsec_source=pc_user](https://www.xiaohongshu.com/explore/67bc5f55000000000900e3dc?xsec_source=pc_user&xsec_token=ABg1aRY_LIGGAmq12SC74oFzz6Ig-ZCelcWkBM2te5YyA=&xsec_source=pc_user)

填写的格式是这样

"url": "https://www.xiaohongshu.com/explore/67bc5f55000000000900e3dc?xsec_token=ABg1aRY_LIGGAmq12SC74oFzz6Ig-ZCelcWkBM2te5YyA=&xsec_source=pc_user"

![](img/c3a23f7733c23d02043bbafa5c34e47c.png)

工作流页面就是工作流id

![](img/220ffb049f0d1f6a0ee300b381b27966.png)

![](img/33504ceea1c1da156c56c65d7d9635f6.png)

![](img/6a80af2dc2accadd3c01bd517a22632e.png)

![](img/c94004e12bd475000210a8d78b547cd7.png)

![](img/cab5f72470dfdc3372604705922c4b3e.png)

![](img/06f6abdb2aa639a95a75dd64f4e5994c.png)

![](img/d52b6ad9b02103617958a609d57332e8.png)

测试成功只需要2个参数给苹果快捷指令即可`token` 2 `workflow_id`

### 五、获取快捷指令

5.1、在 苹果手机浏览器或🛰️ 上打开链接，点击获取快捷指令

[https://www.icloud.com/shortcuts/391e0cd9346c4fdf8fa100ae37e573ee](https://www.icloud.com/shortcuts/391e0cd9346c4fdf8fa100ae37e573ee)

5.2、在手机上打开快捷指令应用，点击‘自动选题库’

![](img/cb4db80c6184b393dcc44ff07e9ef4d2.png)

5.3、向下滑，找到图片中这条链接， 把获得的`Token`以及coze工作流填入(即`&token=`后面添加) ，填写完点击右上角的 ‘x’ 就可以了。

![](img/0c621a3533ee9a8bdd8d0dfbd7f0be2b.png)

### 六、设置触发方式

6.1、进入苹果手机设置页面，点击 ‘辅助功能’

![](img/7631aef618a1ce1ce669a0a5a95ed061.png)

6.2、点击 ‘触控’

![](img/e2892317e79a79249b69d2d9d44feff8.png)

6.3、点击 ‘辅助触控’，如果没有开启，就点击开启辅助触控

![](img/7e2d9ed3026cf3f6f14b488a77b95bde.png)

![](img/3f0a57b98522cf0309b58b124faa5b55.png)

6.4、选择 ‘单点’ 。你也可以选择 ‘轻敲背面两下’，看个人操作喜欢

![](img/0d9399c2fd3080a437e7dd1667aee190.png)

6.5、选择 ‘自动选题库’就可以了。

![](img/b7798b75e635512c560e2eb57009ea9b.png)

### 七、📕使用操作

7.1、点击📕笔记右上角

![](img/cdf2d9296a459cce6d41addb02032170.png)

7.2、点击 ‘复制链接’

![](img/667999c17e70490943850b647da28ef5.png)

![](img/0589a5377e79662da247b39f5d85ebbf.png)

7.3、点击手机上的控制键，就会提示自动保存到多维表格。

![](img/d9d2c3ec2ca96461ca12e22dd6b363c2.png)

## 常见问题解答

注意事项：

1.请妥善保管 `Token`，不要泄露给他人

2.如遇同步失败，请检查：

网络连接是否正常

`Token` 是否正确

飞书应用权限是否正常

3.如有问题请联系客服

## DeepSeek数据分析教程

1.用电脑浏览器打开deepseek官网 https://www.deepseek.com/

2.点击 ‘开始对话’，进入对话框。如果还没登录会自动跳到注册页面。 注意：要先注册账号才能使用 。

![](img/be1c8799b602c38854142d1865717fc2.png)

3.deepseek注册登录后，就会弹出对话框

![](img/3ed0bcd11dfc3aacbd2d7eaf4565ef16.png)

4.打开飞书多维表格页面，点击右上角‘...’，选择 ‘导出’ - Excel/CSV文件

![](img/dbc3b64b893d16734fa523445cd0e130.png)

5.默认选csv，点击 ‘下载’

![](img/0d03160f9a51ed4365864e0ba1da5432.png)

6.下载后把文件名改成`one.csv`，方便上传到deepseek对话

![](img/d790bb09384f20adff3fc73396a70ef9.png)

7.在deepseek页面对话框，点击‘大头针’logo按钮，上传`one.csv`文件

![](img/81c2ae3254405190ea5ef44b80e69bbf.png)

![](img/0f50c76ff3584c8bf278635bb72579d6.png)

8.把提示词模板，复制到对话框里，点击‘发送’，deepseek就会自动分析笔记内容

**提示词模板：↓↓↓**

根据`one.csv`文件是📕笔记内容，请分析出笔记的内容结构、标题公式、情感分析、关键词，标签分布、智能打标签、笔记内容结构。

1.标题公式：根据笔记标题划分笔记类型，分析标题特点和结构，最好能总结出公式、常用的关键词

2.情感分析：哪些关键词或内容能调动用户情绪、吸引用户

3.关键词体系：关键词类型、关键词出现次数、关键词在标题、笔记内容中出现频率

4.笔记标签：标签类型、统计重要标签

5.笔记内容结构：分析内容的结构内容并统计归类，优缺点等

6.根据数据详细说说：用户核心需求、内容优化建议等

7.分析完内容后，统计你分析的数据和结论，写十个标题、写十遍笔记内容、写十个标签。

![](img/def7368eed8179851dafc2838a04e2a7.png)
