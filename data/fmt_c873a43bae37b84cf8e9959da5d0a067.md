
# AI Studio部署排坑指南

> 来源：[https://datawhaler.feishu.cn/docx/ITILdxcqkofnxyx3Va9cD0QPne6](https://datawhaler.feishu.cn/docx/ITILdxcqkofnxyx3Va9cD0QPne6)

## 更新应用

1. 建议本地调试，复制这行`git`命令

> ps：这里我们会发现，需要谨防他人看到我们应用的`git clone`命令，要不然他们能拿到代码

![](img/a3ec880ceed62ed93d9e04f390738867.png)

1. 拿到自己的令牌加到`git`命令上，要不然`push`不上去

> 组合起来的命令类似

> `git clone http://${GitToken}@git.aistudio.baidu.com/123/hello-world.git`

![](img/152c53ad04c397137cec7c607becc017.png)

![](img/9d1887cf3cc77f706a61f28a1495284c.png)

1. 更新代码并`push`上去后，点击【发布应用】

> 记得添加新版本，要不然不会更新效果，用的会是老的代码

![](img/6f370815787bdd18a19a1f4457a0019c.png)

![](img/13738fc596308593b91ca39311f25c79.png)

![](img/066d46d53427bd6633079db698853adb.png)

## 执行文件与包下载

1. 只支持执行 `xxx.gradio.py` 的文件

1. 可能安装支持不了一些`paddle`之外的框架

1. 安装`baidu`镜像之外的包可能比较慢

1. 应用目录层级不能超过2级

![](img/cb0f95372ad7ce9c058662bb73b7e09e.png)
