# ChatGPT保姆级部署教程

> 来源：[https://diwfpqo78m.feishu.cn/docx/DUK9dpNJ4oMiyUx5gpYco6hNnnf](https://diwfpqo78m.feishu.cn/docx/DUK9dpNJ4oMiyUx5gpYco6hNnnf)

> 以下内容全为动手实操内容，且需要你有一定的计算机基础知识，例如Linux基础命令，目录创建，文件修改，DNS域名解析等，且较强的动手能力！

> 如果你动手能力不强，并且对于基础知识没有一个了解，就不要来花这个冤枉钱，看一看就好，能够了解一些基础知识就好！

# 预览

首先来看最终的效果图，可以实现不用翻墙，既可正常访问与沟通交流，免费额度10万次，然后账号的AppKey，我会定时替换，大家可以无压力来使用！再也不用科学上网了！

![](img/9aa9736820b13878cf927f4dfc85ad52.png)

# 购买域名

推荐到阿里云购买，后面备用：https://mi.aliyun.com

# 部署方法

## 一、Github+vercel

### 1、Github

对于GitHub，地址如下：https://github.com/，如果会使用可以跳过这部分。

我们先进行一个注册，主要内容也就是填写个人信息和用户名密码。

![](img/9cc2dc04371538ed203ed109635039f0.png)

然后点击** new repository**

![](img/c8b864909c2feea59ec8e501dfbcfde5.png)

填写基本信息，进行内容的注册。

![](img/2060e2376651bb8c9b94cc0b5244eaa2.png)

创建结束之后，你会看到这样的一个页面。

![](img/879d12d480c7d09f0ef012ae3b70c127.png)

然后上传我给你的资源：

![](img/d66daf40926bfdbd151a373f3f5f68a4.png)

资源如下，下载到自己的电脑里面，然后解压缩：

![](img/d1ab78a91683f222302b7919f02c26eb.png)

[chatgpt-website-main.zip](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/PtqLbXJW1o8azlxYmFHceluknMe)

把这个里面的内容，拖动到这个页面里面（这个页面就是点击Upload files出现的）

![](img/dbb18d16599b8ac6cf49b5dc8f9263fb.png)

上传成功之后，写一些内容，把文件提交上去。

![](img/903814f013a9f76f6a6ac65a5030b858.png)

得到的结果就是如下图所示，表示你所有的内容都已经上传成功。

![](img/f0776131001e4dd2afa8845e6782b69d.png)

以上就完成了基础的部分，剩下就是资源的整合了。

### 2、vercel

我们在上面那张图的基础上，继续往下滑，找到这个地方：

![](img/22cb788de2200dc7791f871a77d17db0.png)

点击之后进入到这个界面，然后点击你的用户名

![](img/c2fb1cab672a4ce05524d42c3e26f20a.png)

然后在这个界面点击，找到Project。

![](img/695d646d86e5c773b16d696b203d1476.png)

然后选中自己刚才创建的项目，点击右侧的导入按钮，进行导入。

![](img/31e80c03f09528ae7ab13fae9992bd3c.png)

点击Deploy 既可，这个过程可能需要一段时间，他会完成基础功能的构建。

![](img/befb540056a7f393927fa620d1d127b1.png)

然后你会得到这个界面，点击黑色的部分：

![](img/32d4c11b39d7db92318889f08d981e9d.png)

点击右上角“Continue to Dashboard”，然后点击下图“Visit” ，即可访问：

![](img/f474e902abca0ad2ba4ddd052ba2c754.png)

然后就大功告成了，这个时候，你输入自己的AppKey，就可以进行正常的操作了。

![](img/6fa549dc5901d459092765c50b91cb45.png)

到这里，基础的工作就全部完成了！

### 3、域名转接

大家可以发现的是，到这一步，你的域名，也就是这里，是随机的域名值，但是我们在前面购买过自己的单独的域名，所以后面的就是进行记录值的绑定，给我们的网站更换域名！

![](img/d39aa2187cf4f99eb5c11767fccb197e.png)

首先点击，这里这里左图的按钮，进入到右图的状态，然后点击settings。

![](img/92fbd0dd3faf6a5f560c33f8bfc2fb78.png)

![](img/e26eb975e3c8617fadefeb56c093624b.png)

然后选择Domains。

![](img/5cb536361146ce3d75f6ba04cf542a98.png)

把自己购买的域名，填进去，然后点击Add。

![](img/c02eb415eed7f3760d26d145a4b6f0be.png)

![](img/45778c7e4649fa9ee8a744dd9a33cb66.png)

因为我绑定过，所以不重复绑定，大家点击之后，出现的应该是这个样子的。

![](img/dd818929f5f329f34bc3d6e6d266fc45.png)

表示绑定是失败的，这个时候，我们复制，这个数据值出来，到我们的域名购买的地方。

![](img/e6ec901d0bc67903395b0eef297b0aed.png)

### 4、域名A记录绑定

网址如下：https://porkbun.com/account/domainsSpeedy

![](img/d1061252b6abb8a46badf927aa575604.png)

然后把信息填进去

![](img/6f5ba5abfe7b1198a4361208f1eca6a6.png)

最后得到的结果如下图所示：

![](img/8f06485bf75bd2fdda65c4a1bfc2ea49.png)

然后，我们回到这里进行一个检查，发现就是正常的状态，可以正常访问了。

![](img/aeb596876cacd38e42cc88463cd7a62c.png)

完美搞定：

![](img/85860c2c0ec851c9463520d8ae439ef5.png)

###  5、设置全局变量

到上面之后，我们的基础内容就弄完，但是现在需要大家每个人来手动输入AppKey 进行访问，我们可以设置全局变量，让大家免费使用。

点击这里的Environment Variables，进行全局变量的设置：

![](img/10a9ac5ef68e613024371d52d300bc2a.png)

修改环境变量需要Redeploy

![](img/8d8fbcf3811ef5f057c8c877c1190354.png)

![](img/89966aa5b805e15436ad08684a72e8f3.png)

至此，第一种部署方式就完全搞定！

6、修改信息

大家拿到代码之后，就可以在GitHub上进行实时的更新和修改，替换掉我的公众号信息，替换成为自己的，剩下的就是自己操作了。