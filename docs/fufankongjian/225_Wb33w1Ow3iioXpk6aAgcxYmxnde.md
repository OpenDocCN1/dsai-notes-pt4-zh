# 【补充资料】Windows安装Docker

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/EvyudfIzyo2tyoxUKfxc2mQCnid](https://kq4b3vgg5b.feishu.cn/docx/EvyudfIzyo2tyoxUKfxc2mQCnid)

## 课程说明：

+   体验课内容节选⾃《2025大模型Agent智能体开发实战》完整版付费课程

体验课时间有限，若想深度学习⼤模型技术，欢迎⼤家报名由我主讲的《2025大模型Agent智能体开发实战》：

![](img/aa1fe7220854008de5ce11570e7af324.png)

![](img/7ab2b567949b4d2f3fd7fa883fe9c41f.png)

此外，公开课全部课件，以及项目代码、数据等学习资料，扫码⬆️联系助教即可领取～

---

# 1. 安装Git

+   **Step 1. 进入Git官网：**https://git-scm.com/

![](img/ee83bd92bd5af3245d7f446e3cf34fc6.png)

+   Step 2. 下载Windwos版本的Git安装程序

![](img/37955a58a873b5ec283d02c359c196aa.png)

这里选择Windwos版本进行下载。

![](img/bcef9ba62fe68fa51a28a87d8b966bfc.png)

根据自己的Windwos系统情况，选择32-bit或者64bit。

![](img/d3e1b824626399edcae7e81244c85d94.png)

选择Git安装程序的存储路径。

![](img/8b2ed22d22aff239f9e39fe5b5541261.png)

+   **Step 3. 安装Git**

![](img/e973c03d8cd6f5444d87f0574f5e14aa.png)

选择Next。

![](img/60919a67d6243e60ba099343f3cd1ca6.png)

自行选择Git的安装位置。

![](img/80f806128adfaccd8224d05e3082ce08.png)

接下来的选项，全部默认，点击Next即可。不影响正常的使用。

![](img/19d8c042616098fef79a6caa14b641ee.png)

![](img/3d2cf7c5bd659f0dbda30ff15d5790fb.png)

![](img/58e12ffd00609afcbd78021362862ec7.png)

![](img/a6b2be808e52e7398c6e319b67114bc2.png)

![](img/cf20ba9d9080d81791f285d35254eae6.png)

![](img/b92b80dd56f235cf434eb9fa159eb3f9.png)

![](img/e2e0d40a0baf7afedf0d084a104a0526.png)

![](img/397af614882a5a161791b74f00f08634.png)

![](img/eaf8add715cf32ed594e99951a570f21.png)

![](img/4198e1a6f2cd877de3e0c467e98ea493.png)

![](img/55949ab92a9ed8bb52d03168c5aa3683.png)

![](img/193fec38f2354c11124c25f03183bfbc.png)

默认选择到这里，执行安装。

![](img/b71a3847d0b2d6381e99f65036e92208.png)

执行到此进度，说明安装已经完成。

![](img/a215d9b29b576f0997ea30d19b16aa19.png)

+   Step 4. 验证Git安装是否成功

在桌面上鼠标右键，如能看到如下两个快捷方式，说明已经正常安装Git。

![](img/790823be366861860d59b49c61882299.png)

点击Git Bash 后，进入命令行终端，可以输入git --version 查看当前的Git版本。

![](img/1f4793ad97bd31e4fd6ad9d5e7901295.png)

# 2. 安装 FFmpeg

下面是如何在Windows系统上安装和配置ffmpeg的步骤：

1.  **下载FFmpeg**：访问BtbN's Windows builds on GitHub，下载适用于Windows的FFmpeg压缩文件。

![](img/ae6b660bf155f6619f4100ef19619dfd.png)

1.  **解压FFmpeg**：将下载的文件解压到你选择的目录，比如C:\FFmpeg。

1.  **配置环境变量**：

+   右键点击“此电脑”或“我的电脑”，选择“属性”。

+   点击“高级系统设置”，然后选择“环境变量”。

+   在“系统变量”部分，找到并选择Path变量，然后点击“编辑”。

+   点击“新建”，然后添加FFmpeg的bin目录的路径，比如C:\FFmpeg\bin。

+   点击“确定”保存你的设置。

1.  **验证安装**：

+   打开命令提示符（CMD），输入ffmpeg -version并回车。如果系统返回了FFmpeg的版本信息，说明FFmpeg已经成功安装和配置。

![](img/01b0a59b08628dbd6fb6fb6e222e5573.png)

# 3. Window 安装 Docker

Docker 是一个开源的平台，它可以使软件开发和部署变得更加简便和高效。通过使用 Docker，开发者可以将应用程序及其所有依赖项打包到一个被称为“容器”的轻量级、可移植的、自给自足的单元中。这样，无论在哪种操作系统或环境下，这个容器都能保证以相同的方式运行。

想象一下，你把一个应用程序比作一个需要特定环境才能生存的植物。如果你想把这个植物带到不同的地方（不同的计算环境），你需要确保每个地方都有合适的土壤、光照和水分。Docker 就像是一个可以随处携带的植物生存盒，无论你把它放到哪里，它都包含了植物生长所需要的一切条件。这样，你就不需要担心不同地方的环境变化，植物（应用程序）总是可以顺利成长。

这种方式使得软件部署更加简单，也减少了因环境不一致导致的问题，这在开发和测试阶段特别有帮助。因此，Docker 在云计算和微服务架构中非常受欢迎。

Windows Desktop桌面版官方下载地址：https://www.docker.com/products/docker-desktop/

+   Step 1. 下载Windwos安装程序

![](img/398a0ac25b15ceeaeebfa2b1c240a01c.png)

选择Windows操作系统版本。

![](img/7ff950a4e10f5ccde4cefb4599cf234e.png)

选择安装包存放路径，点击等待下载完成即可。

![](img/c6d3ce2d19c01ccf4ef0100e268cdbcf.png)

+   Step 2. 安装完双击打开Docker

![](img/1e40aa5cdb0c8566d4364b2d0fab3594.png)

+   **Step 3. 执行安装过程**

![](img/617375c7bbb5f40ad6c34a76cfddf925.png)

![](img/69d7fd7241d6782d1e47c4c26668ecb0.png)

+   Step 4. 安装完成后，需要重启电脑才能加载Docker应用

![](img/9f6be66c15f0f5a38586fb0c3b11d8f5.png)

+   **Step 5. 重启完电脑后，打开Docker Des**ktop客户端，首次使用需要接受服务协议

![](img/456a82b48cfa6c4bb573b26daf8ea32d.png)

+   Step 6. 这里选择默认配置即可

![](img/4506e44ab227c7a85b3dbc87a7df26e7.png)

+   Step 7. 在使用前，需要登陆账户，可以免费注册

![](img/b435370d68e28a4fd9fe607cde519542.png)

登录后，即可正常使用Docker服务了。

![](img/37373dd89cc3248dc526c29af9221189.png)