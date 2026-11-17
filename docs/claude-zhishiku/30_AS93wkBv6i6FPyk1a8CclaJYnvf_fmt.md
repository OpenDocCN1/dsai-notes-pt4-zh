
# 如何获取&使用Gemini API

> 来源：[https://sqivg8d05rm.feishu.cn/docx/IykGdfRo9oLW7txTQ3QcitIEn8c](https://sqivg8d05rm.feishu.cn/docx/IykGdfRo9oLW7txTQ3QcitIEn8c)

## 前置准备

谷歌目前提供3flash，不再提供免费pro，上下文250k。

不过你至少得有个谷歌账号，自己注册或者去买都可以。

下面讲如何获取Gemini API，需要用电脑。

## 获取API步骤

1. 访问[控制台](https://console.cloud.google.com/)，登入谷歌账号，该授权的授权，该同意的同意

![](img/01bb034dbfa1e05d1ef70df236ab6cac.png)

2. 打开`Shell`，该授权的授权，该同意的同意

![](img/28d78a4aaf0fb96a7b924f1219c14fab.png)

3. 粘贴代码块里的命令并且回车，粘贴是Shift + Ins（批量获取脚本源自类脑，我不是作者

```bash
bash <(curl -sL https://raw.githubusercontent.com/3753910346/geminiscripts/refs/heads/main/gemini_pro.sh)
```

4. 跟着脚本引导走就好了，一个号最多能创建12个项目，更多需要申请，一个项目一天可用100次2.5Pro

![](img/775558fe972027ec977ba91abbbac563.png)

5. 等待创建完成后脚本输出密钥信息，跟图上一样就是创建好了，使用逗号进行分割

![](img/0686f062b312a19e84e0876090d99f28.png)

6. 把密钥在酒馆里填入即可使用

## 使用注意事项

对了，记得关闭流式传输使用，Gemini有检测，会截断。

如果你关闭了依旧截断空回复，请尝试更换破限或者检查角色卡是否为未成年

![](img/09fe73d59a7bafddead67c2c0cc921cc.png)

## 反代服务（可选）

题外话，如果你连不上谷歌，可以试试看关魔法直连我的反代，使用方法如图。

当然，你要是怕我偷你密钥就算了。

还有，情况不对我会关反代跑路的，就这样（）。

暂时挂了用不了

```
https://gemini.meow33.my
```

![](img/3a9c73f4424940f1c60a9e702db3f6eb.png)
