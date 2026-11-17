# 保姆级 | Coze开源版本地部署教程

> 来源：[https://v11enp9ok1h.feishu.cn/docx/SbqXdXiQco97h0xjvvZcvaoqndG](https://v11enp9ok1h.feishu.cn/docx/SbqXdXiQco97h0xjvvZcvaoqndG)

对于智能体开发者来说，Coze开源是十足的好消息！

**一共五个步骤，约25分钟完成：**

+   一、注册云服务器

+   二、配置环境

+   三、部署Coze

+   四、获取key和model接入点

+   五、启动Coze，快乐玩耍~

---

## 一、注册云服务器

1、新用户可以有免费服务器，老用户可以去别的云服商那里购买，点击去注册腾讯云：轻量应用服务器

2、进入腾讯云，🛰️扫码注册，

首次注册 会进入这个页面， Coze项目要求，最低2核4G

![](img/c419b5ccda2e1860954f51db8ef5b4a9.png)

![](img/42a72ee645d442e70f8f151fa12d9009.png)

3、点击立即试用后，会出现下图。

地域随便选择，镜像选择下拉框最上边的 宝塔8.1.0。然后点击“立即试用”

![](img/4bdad78172b790d39541272c1bec05f6.png)

4、进入腾讯云服务台了，点击“登录”。

![](img/665cb32b4ca8b0ee70f6117b51654f0a.png)

5、这里直接登录即可，后续也可以🛰️扫码登录

![](img/6e5c7420f6ebccb794005c9b753c1972.png)

6、登录后，就在当前页面，复制：echo "8887" > /www/server/panel/data/port.pl 回车继续：sudo kill -9 $(sudo lsof -t -i :8888) 回车继续：sudo /etc/init.d/bt default，粘贴进入图示位置，然后点击回车。

此处输出的内容，也要保存好。

![](img/1f77dd94d30cdc4a6c629fc08c5a0000.png)

7、返回服务器控制台。点击图中，箭头指示的空白区域。


8、选择“防火墙”菜单栏，点击【添加规则】按钮，

只需要填写再添加“8887”和“8888”作为开放端口。

![](img/a234fe8e431a19d5ed298c2414ee1121.png)

## 二、配置环境

1、刚才在这里保存的“外网面板地址”，点击打开。

![](img/431b40cb790902bd7fb916cd06e67fb2.png)

（有小伙伴反馈，命令输出的地址是login结尾的[，点击打不开。那你只需要把login改](http://xxx.xxx.xx.xxx:8887/tencentcloud%E5%B0%B1%E5%8F%AF%E4%BB%A5%E4%BA%86%EF%BC%89)[成http](http://xxx.xxx.xx.xxx:8887/tencentcloud%E5%B0%B1%E5%8F%AF%E4%BB%A5%E4%BA%86%EF%BC%89)://xxx.xxx.xx.xxx:8887/tencentcloud就可以了）

2、输入账号密码，即上图中的 username、password

![](img/36767e7b7b5c94bdcdd3fe4d065e7197.png)

3、第一次进入会让你绑定一下，点击免费注册，注册完成后，返回此页，登录账号。

![](img/7593de4e83788f5bfb2cbc1be9a40845.png)

4、首次会有个推荐安装，只安装第一个即可。其他的取消勾选。

![](img/a8693098d2f57adc850af8ec42e1d7dc.png)

1、回到宝塔，选择Docker

（如果如图显示，就跟着我安装。 如果没显示“当前未安装”，就不需要这一步）

![](img/674f8769f0abb47bed795f47ca00c77c.png)

2、选择，点击确定。比较慢，等待安装完成。

![](img/33ac1e6f3b424662b765a0e228685165.png)

3、安装完成后，刷新当前页面。看到下图的样子，则表示安装成功。

![](img/af299293edc2092556fcf7d6cd3598c6.png)

4、打开左侧【终端】，粘贴以下两行，再验证下Docker是否可用。看见版本号，就是没问题了。

![](img/ad06df11788f6fd6770c17532afcdf38.png)

## 三、部署Coze

1、点击文件，进入“根目录”进入“home文件夹”，点击“新建文件夹”

![](img/3c5a765ebf15a967ec255a70fe0d02d6.png)

2、然后进入Coze文件夹，在顶部点击“终端”，并在终端中粘贴以下命令。

注意：不要用ctrl+V粘贴，直接鼠标右键粘贴。

```
git clone https://github.com/coze-dev/coze-studio.git
```

![](img/e22e4956a27387fbe9b7480a04b01a56.png)

如果你的服务器网不好，一直在进行中，下载不了，可以直接去Github下载代码包。

![](img/22138ba148b5e9320ce4d592e24dc2b7.png)

然后把下载的包直接拖拽到Coze文件夹下，解压即可。

![](img/86538d663d11966346dca66dc748a384.png)

3、双击打开下载过的Coze-studio文件夹，在顶部点击“终端”输入以下命令并点击回车。

![](img/1b98cc411a19c2aa863d45f48d3bd25e.png)

4、关掉这个终端窗口，

5、然后在文件夹中找到以下目录

5.1 进入目录 backend/conf/model。打开文件ark\_doubao-seed-1.6.yaml。

![](img/4b67267bf72c1e8b542cc36d1d0bab36.png)

5.2 需要设置 id、meta.conn\_config.api\_key、meta.conn\_config.model 这三个字段。

+   id：Coze Studio 是中的模型 ID，由开发者自行定义，必须是非 0 的整数，且全局唯一。模型上线后请勿修改模型 id。 意思就是id可以随便设置，保持唯一性就好。我们直接设置“888”

![](img/2b1871511a54611a48a8e4bb0f4bcecf.png)

继续往下翻翻到底，需要填写以下两个值，如果是开发者，你可以直接去火山后台获取，如果是新手，看下方教程。

+   meta.conn_config.api_key：在线模型服务的 API Key。

+   meta.conn_config.model：在线模型服务的 model ID，在本示例中为火山方舟模型接入点的 Endpoint ID，

![](img/5d4a4c0bcd9c88ddab724b2a99fe06ca.png)

## 四、获取key和model接入点

1、注册并登录火山引擎，点击立即体验进入控制台。

![](img/2ae5183e41ecc0d54b538e84572f4e46.png)

2、创建一个接入点

点击在线推理--创建推理接入点

![](img/01aa210a5037976cb6aa1d5cb5db6065.png)

3、为接入点命名为DeepSeek-R1

![](img/d30fdf6ee071444cab3ab1d411dbdbd1.png)

![](img/93d6d6bc6091ce55bc217ace148434f6.png)

然后可能会提示：“该模型未开通，开通后可创建推理接入点”。 如果有提示，就点击“立即开通”，开通一下就可以了。

如果无提示则直接到：第5步，点击确认接入。

![](img/489d32181c0d1e92ca12da9b583e0207.png)

4、点击“立即开通”跳转到此页面，勾选全部模型和协议，一路点击开通即可。（这里是免费的）

![](img/b2d29610bdd48fac80ddbc10c52c84c1.png)

5、确认以下无误后，点击“确认接入”按钮。

![](img/54212fcc47975e1d8f0d17619a198a9e.png)

6、自动返回创建页面。发现多出一行接入点名是“DeepSeek-R1”（我们刚才自己设置的命名）。

![](img/53c7381bc3e37b5b48dc953e19f5876f.png)

重点来了：这个就是推理点的ID，也就是刚才提到的 meta.conn\_config.model：在线模型服务的 model ID。

复制他放到你的🛰️里，发给自己保存一下。

![](img/ffe74f39b4dec07c287ff1fd3c26f360.png)

7、保存后再点击【API调用】按钮，进入后点击【选择API Key并复制】

![](img/a0f1941d9ed0cac9764dad2521f7b197.png)

8、 如果你已经有API key了，就直接查看并复制。

如果没有，则点击【创建API key】

![](img/4b624ef2cd8e56848485c0fbb225ec33.png)

![](img/c476aebbd49f49555502c751ad2b62b5.png)

![](img/f604b4c696d5e1b4caf79a8233999fcf.png)

9、把这个复制好之后，也放到你自己🛰️里，保存好。

这个就是meta.conn_config.api_key：在线模型服务的 API Key。

![](img/3ee389f6ecd4ea7a74d207453bac26da.png)

把这两个填入对应的位置，如上图。 然后保存

## 五、启动Coze

1、已更新，此步跳过）返回上一层文件夹，进入到/home/Coze/coze-studio/docker文件夹中

（不用改了|之前这里修改端口是因为，宝塔面板是8888，和Coze的冲突了，所以修改一下）

2、然后保存文件。回到主文件夹“coze-studio”文件夹下，打开“终端”一次一行，粘贴并执行以下命令：

```
cd docker  
cp .env.example .env  
docker compose --profile &#x27;*&#x27; up -d
```

![](img/fa65f91e6de740cbe91cae16441dd3f6.png)

3、首次部署并启动 Coze Studio 需要拉取镜像、构建本地镜像，可能耗时较久，耐心等待即可。

最终看到终端：提示 "Container coze-server Started"，表示 Coze Studio 服务已成功启动。

4、至此，本地部署已经全部完成。可以直接通过浏览器访问了