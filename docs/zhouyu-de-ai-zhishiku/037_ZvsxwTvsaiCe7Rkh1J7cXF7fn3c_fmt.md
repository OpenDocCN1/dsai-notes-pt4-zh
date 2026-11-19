
# 下集）【优惠码兑换处】、【邀请推荐奖励】

> 来源：[https://xatc0v8uz5m.feishu.cn/docx/XzXLdaE50ovVmXxvFiscLPegnDe](https://xatc0v8uz5m.feishu.cn/docx/XzXLdaE50ovVmXxvFiscLPegnDe)

## 页面与功能调整需求

单独做一个`/pricing`子页面，其中的FQA也复制一版到pricing子页面中，pricing页眉和页脚的价格按钮，都要指向对应的子网页；删除掉FQA的页眉按钮。博客中缺失的图片给他们换一张，选择pose图库中的fun、hot类别的图片替换。所有blog的博文作者用Zhihao Peng，不能用虚假的AI生成的假名字。去除掉页脚中Update Log\FAQ，去除掉关于 Features 和 Update Log：直接删掉。contact us要一个新的子页面，内容是网页的功能介绍和解决的场景、公司地址资料，和联系方式。附件2中的Have another question? Contact us.不要弄超链接了。

![](img/ceb1b5467c4f94b9b4dccd2f7a3bc740.png)

![](img/f1da26f85840f0ae6103b65e33c6415d.png)

![](img/7bcc3da056b7f2b632b774ea5c81203f.png)

## 支付合规与KYC检查

我们网站正在面临最严格的支付集成的审查，他们要检查网站的合规性。已达到最后的真人支付的功能。你是一个专业的网页KYC认证专家，同时是一个严格的审核人员，要对我们网页做一些的合规检查，比如按钮有没有合理的跳转、评价是不是真实、图片有没有全部加载好等等细节。附件1是creem反馈回来的点，很严重，我们没有第二次机会了，必须要在接下来的这次申请中通过。然后你要做的是使用Chrome devtool工具进行检查，以及页面画面，见附件2、3等等，然后你把所有检查出来的问题，汇总出来，然后不能改动代码，所有罗列出来，我每一个确认好之后，才能执行下一步

![](img/595259b619421a8ceac50036057c6dfe.png)

![](img/e720cec0d4e2f05e6cec2d2e178e3f15.png)

![](img/37f70e281bc711aaacb541d5e2915610.png)

![](img/b5e704da81e067b074ac3f9bcfac1e87.png)

## 优化执行及后续需求

好的，开始按照检查出来的结果，进行优化，关于我们（把contact us改成about us，这个是允许的吧？about us包含了contact us的信息）、补充上【更新日志】。然后还要针对性的过一遍所有的法律条款的内容，确保cookie、隐私政策、Legal Terms of Service、Privacy Policy、Cookie Policy、Copyright Policy、Disclaimer里面的文档内容统统进行检查；修复blog中未显示的照片

![](img/2f6a6996e998f7fc705ef9cbbdc87940.png)

![](img/6e18baf7358b21435bd4c2cacc3cc056.png)

切换语言的按钮去除掉，目前只保留英文版本；附件1中的about us标题要居中。【Have questions or feedback? We'd love to hear from you.】放到[We usually respond within 24 hours on business days.]下方；更新日志的时间线不要写太久，就最近两个星期的时间内容，不能写正式公开上线版吧，我的正式支付集成都没打通，写测试版？；附件2中，【your clothes, and your background 100% untouched. 】这句话改动一下，不要写的太绝对，优化一下，不要过度承诺；Privacy Policy文章内容重点检查数据保留期、GDPR权利、AI数据处理声明是否完善。terms of service 检查是否有写清楚版权说明。思考标准的合规的页脚方案，判断是否支持Terms of Service (服务条款) —— "规则总集"

合并对象：原 Terms of Service + Disclaimer + Copyright Policy + (Pricing规则)，如果可以合并就合并一下。

![](img/be1ee120d1d44d5c5320f9b74bf05a1c.png)

![](img/c031b6f1191df601fed64efa6df96b60.png)

## 已完成的界面与功能调整

### 界面与功能调整

+ 语言切换：已移除语言切换按钮，网站目前仅保留英文版本。
+ About Us (关于我们)：

    ```
    About Us
    ```

+ 标题已居中显示（参考附件1要求）。
+ 联系我们副标题 “Have questions or feedback? We'd love to hear from you.” 已移动至 “We usually respond within 24 hours...” 下方。
+ Update Log (更新日志)：

    + 时间线已调整为最近两周（Dec 01, Dec 10）。
    + 版本号定义为 Beta v0.8.0 和 Beta v0.9.0 测试版，符合当前未正式上线支付功能的状态。

### 文案与合规性优化

+ FAQ 文案优化：将 “100% untouched” 修改为 “highly consistent”（高度一致），避免绝对化承诺，降低法律风险（参考附件2要求）。
+ Privacy Policy (隐私政策)：已补充完善以下关键合规条款：

    + Data Retention (数据保留)：说明数据仅在必要期间保留。
    + GDPR Rights (用户权利)：明确列出访问、更正、删除等用户权利。
    + AI Data Processing (AI数据处理)：明确声明未经同意不会使用用户照片训练 AI 模型。

+ Terms of Service (服务条款)：

    + 已将 Copyright Policy (版权政策) 和 Disclaimer (免责声明) 的内容完整合并至服务条款页面中，实现了“规则总集”方案。
    + 明确了用户生成内容（UGC）的版权归属（用户保留所有权）。

+ 页脚优化：

    + 简化了页脚法律链接，整合为标准的 Terms of Service、Privacy Policy 和 Cookie Policy 三项。

请检查各页面是否符合您的预期。

## 新增需求

添加上删除账号的功能，在Account Settings的位置；首页页眉按钮添加高亮点击；然后blog的文章中涉及到用户图片保存7天的，改成21天。
