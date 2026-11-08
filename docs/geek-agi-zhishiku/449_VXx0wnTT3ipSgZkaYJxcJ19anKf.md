# FlashFace：一个能改变你年龄的个性化照片工具，不止这一个功能哦

> 来源：[https://geekbang.feishu.cn/docx/ViVTdEMltoZu0Xxh96tczoHOnic](https://geekbang.feishu.cn/docx/ViVTdEMltoZu0Xxh96tczoHOnic)

![](img/27c2c664ba1bd02899a3e2100885cfef.png)

**板块主理人：尼福财**

**·向量光年科技有限公司创始人**

·《开源AI项目落地》👸号及社群主理人

专注于AI产品开发落地，移动端、PC端软件快速高质量交付

愿景：加速全行业向AI转变

![](img/fdf10454f1ef758c62ee2ed5a444f551.png)

**项目简介**

FlashFace 是一个开源的人像个性化生成工具，专注于高保真的身份保持。该项目能够在几秒钟内使用一个或多个参考面部，进行零次人像定制，并强调保持非名人的身份。它支持根据语言提示进行年龄或性别的改变，并允许用户自定义身份图像控制和语言提示控制的强度。此外，FlashFace 支持各种参数调整以优化生成效果，特别对亚洲人脸有良好的适应性。

**扫码加入AI交流群**

**获得更多技术支持和交流**

**（请注明自己的职业）**

![](img/ff195af1677ed1cf0987700ef1aa4faa.png)

**功能**

**·人像个性化结果**

FlashFace生成了多样化的人像个性化结果，具有以下特点：

**1. **在很大程度上保留参考面部的身份特征（例如，纹身、疤痕，甚至虚拟角色的稀有面部形状）；

**2. **当文本提示与参考图像相矛盾时，能够精确地遵循指令（例如，将成人自定义为“儿童”或“老者”）。

![](img/916e85a09f1dc4ffe10461846d268b2a.png)

**·更改年龄或性别**

![](img/c273078a1e5e32d410e0e07bfd72b0e5.png)

**·将虚拟角色转变为真实人物**

![](img/558e0ab8ee15214b7df2725b18faafb4.png)

**·将真实人物转化为艺术品**

![](img/4dd3fefd1009f9e716f55d60ad3ef517.png)

**·身份混合**

![](img/4b52946537b54c84e13551ba196f2919.png)

**·在语言控制下的面部交换**

![](img/b80fb7beb1dcdee285ac26bef200270a.png)

**安装**

**1.克隆FlashFace**

```
git clone https://github.com/ali-vilab/FlashFace.gitcd FlashFace
```

**2.创建环境**

```
conda create -n flashface python=3.10 -yconda activate flashfacepip install -r requirements.txt
```

**项目链接**

https://github.com/ali-vilab/FlashFace

关注「 开源AI项目落地 」👸号

与AI时代更靠近一点