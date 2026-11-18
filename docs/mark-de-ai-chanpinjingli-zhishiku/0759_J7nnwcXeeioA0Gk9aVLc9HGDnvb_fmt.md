
# 手把手教你在Cursor使用Skills

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/BOLFdFSfroaIgYxDu70cFZdnnsd](https://qqs7y1hozd1.feishu.cn/docx/BOLFdFSfroaIgYxDu70cFZdnnsd)

**文章包含以下几个部分内容：**

1. 安装`OpenSkills()`
2. 安装官方Skills
3. 创建`AGENTS.md`并进行同步
4. 案例分享-生成用例到Excel
5. 案例分享-`frontend-desisn` 前端设计skills

## 操作步骤

> 前提： 安装`nodejs`（且版本要20以上）

### 安装OpenSkills

> 目的：让cursor/trae的项目也可以使用skills。

```
npm install -g openskills@1.2.1
```

### 安装官方Skills

找到自己的项目（随便创建一个），进入到`CMD`，执行下列指令

> 选择想要的skills，确认之后敲下Enter，等待安装成功，

> 这里一般都会把全部skill都下载下来，

> 因此随便选择一个也OK

![](img/7099e61ab8617df8dea08adb2a5a1cb6.png)

如果想要全局生效的话，则可以执行

```
openskills install anthropics/skills --global
```

### 创建`AGENTS.md`

创建一个`AGENTS.md` 空文件即可

![](img/4b0aad0b8c3a444cce8cf704eced9e95.png)

### 执行同步指令

在`CMD`中执行

```
openskills sync
```

该步骤执行后，又会出现让你选择Skills的步骤，也是随便选择一个，然后点击enter键，出现以下内容就表示成功

![](img/a02ec78f3c10569d0e686aa9aa701539.png)

可以看到`Agent.md`文件中写入了一些内容，简单理解就是Skills如何使用，具备哪些skills

![](img/8e120d64b5eed068b270fd8626a20379.png)

### 案例验证

在Cursor打开刚刚创建的项目(如果你是设置了全局的，就可以在任意项目使用）

![](img/50e60f5e45afe3376792137fbf808924.png)

#### 案例一：生成测试用例到Excel

输入：

![](img/7af1f0e6cbb18bc924358d4d23540244.png)

过程：

+ 首先会去读取 xlsx skills

![](img/7b1f51333124846a7f960f257a90d4ad.png)

+ 创建`python`文件并执行

![](img/1a2277d0a4b7ff151f1d0ca9c3205c86.png)

+ 生成excel

![](img/523109b02d24614849af95a1f08acb3d.png)

+ 最后再将临时创建的`python`文件删除

![](img/1d7fb2c31bf12c3fd2ea1cd9656aef79.png)

+ 生成的excel的效果如下：

![](img/8cd7ac463e29097c54511de6757b7df6.png)

#### 案例二：`frontend-desisn` 前端设计skills

在对话框输入：

![](img/f0fbcbce13bde362e9db9d5b00ae71c7.png)

之后可以看到，会使用`openskills`指令，去读取`frontend-desgin` skills去执行

![](img/d41e0a95432723754fedb74628264bed.png)

使用`frontend-design`效果

![](img/a0cc24670855d7659cdfe9ccf8f20d14.png)

未使用`frontend-desgin`效果

![](img/8f7759b15b28d0fcfef2681d23db3101.png)
