# AbletonMCP：实现Claude和Ableton交互制作音乐

> 来源：[https://geekbang.feishu.cn/docx/XU8ZdA6xFog8cNxLdeJcXy9vnnb](https://geekbang.feishu.cn/docx/XU8ZdA6xFog8cNxLdeJcXy9vnnb)

> 原文链接：https://x.com/sidahuj/status/1902719460278198658

> 作者通过构建一个 MCP，让 Claude 可以直接与 Ableton 对话，只需输入提示即可创作音乐！

> 项目链接：https://github.com/ahujasid/ableton-mcp

![](img/1ab140fc3846e8c294d93412268c3353.png)

[2nKWonLhwf0iamcb.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/G8zYbS3biofbKlxXdG5cyni2n3t)

# 详细教程

## 工具安装

### Ableton

说明里提示需要安装 **Ableton Live 10** 或更高版本，这里我们安装 Ableton Live 12 试用版

点击下方链接进行注册即可

> https://www.ableton.com/zh-cn/login/?next=/zh-cn/account/%3Fpk_vid%3D9907e659aa8ac8991742544922128a1f

![](img/554c6569e1a1fb318f2e259be110acce.png)

注册成功后，点击右上角进行下载页面

![](img/c689d917d4997bf3645a3420f5681873.png)

我这里演示的是Windows 系统，直接下载即可

![](img/3368993082360de8a1b22ee8b5f1d268.png)

其他系统可参考下面的教程进行安装

> https://help.ableton.com/hc/zh-cn/articles/209071129-%E5%A6%82%E4%BD%95%E5%AE%89%E8%A3%85-Live-%E8%AF%95%E7%94%A8%E7%89%88%E6%9C%AC

下载完成后进行解压，得到下图，双击 "Ableton Live 12 Trial Installer" 进行安装即可

![](img/ed096751c7ae3712296e5bcbbcd059c5.png)

安装完成后打开，点击 开始免费试用，进行网页验证，再退回软件界面进行加载，Ableton Live 12安装完成。

注意：安装后的Ableton Live 12记得设置桌面快捷方式，方便使用

![](img/1ee8a5c0d9bc4f44f0adae7fd6b37286.png)

![](img/98b62824515cfb46707c6cf88e56653a.png)

### Python

说明里提示需要安装Python 3.8或更高版本，这里我选择python 3.13.2版本

下载链接：https://www.python.org/downloads/

进入网站后，找到 Downloads，选择Windows，进入页面

![](img/4c54fed7cd776f030b9c24aeedfad92e.png)

我的电脑64位，选择如下图，点击下载

![](img/5ceb9fcc0909f3ef10c6d4b4770ef54f.png)

下载完成后，点击安装，如下图所示勾选，然后选择 Customize installation

![](img/1e806e161442ccd80819cbd10c61ac3d.png)

后边没有什么特殊，直接安装即可

![](img/e4df9f5e77cfab79297ba50c9826aeb6.png)

### uv

> 安装教程：https://docs.astral.sh/uv/getting-started/installation/

这里还是以Windows为例，打开powershell，将下面指令复制粘贴进去，回车

```
powershell -ExecutionPolicy ByPass -c &quot;irm https://astral.sh/uv/install.ps1 | iex&quot;
```

![](img/e1244008dbdc409c2ec106375d7e3367.png)

接着以管理员身份打开第二个powershell，这一步主要是更改执行策略，允许从互联网运行脚本

输入下面指令，然后回车，会显示当前的执行策略

```
Get-ExecutionPolicy
```

再输入Set-ExecutionPolicy RemoteSigned，这一步是允许本地脚本运行，并允许从互联网下载的脚本，只要它们已经被签名。

```
Set-ExecutionPolicy RemoteSigned
```

最后输入Y，回车即可，可以再输入一次Get-ExecutionPolicy进行验证

![](img/93fd5c5505274ac7350d57b2381b8a66.png)

再进入powershell，输入下面的指令，回车，再输入uv进行验证，成功运行如下图

```
powershell -ExecutionPolicy ByPass -c &quot;irm https://astral.sh/uv/0.6.9/install.ps1 | iex&quot;
```

![](img/0fd294518e1448cd222efa373eb6a3d5.png)

打开命令行，输入下面命令，回车，安装python包

```
uv python install
```

### Visual Studio Code 安装（这一步如果你有IDE可以直接跳过）

进入下面链接点击下载安装即可

下载地址：https://code.visualstudio.com/

![](img/ec8e795c92a4c01221886ca7af497ec3.png)

## 环境配置

### Python

> 参考教程：https://blog.csdn.net/sensen_kiss/article/details/141940274

在本地按照下面的路径找到环境变量

> 系统--系统信息--高级系统设置

![](img/4de91826c3bd69ec34053b9ddcff9668.png)

找到系统变量里的Path，新建，把Python的安装路径输入进去，再点确定，到此完成python的环境配置

![](img/9406559ef6ae4a1a8af2ef8ad0af4429.png)

![](img/ecbd53b5c8b9467071315db5792ce2cf.png)

最后在运行输入cmd，回车，在命令行输入下面的命令进行验证即可，下图为成功配置

```
python --version
```

![](img/425b54e89933744c87f267df86e40918.png)

### Visual Studio Code（后面简称VSCode）python 配置

> 参考教程：https://blog.csdn.net/jiqimiao/article/details/143446166

进入VSCode，如下图左侧选择扩展图标，搜索栏输入python，回车，选择如下图第二个，点击Install安装

![](img/7337f4a6a5cb0a4b48a55bb7d411d776.png)

选择左上角，新建一个python文件，记得保存一下，以便运行

![](img/7fb956a596db5177cd729fb8fa63c0f5.png)

![](img/fb90b5ae4d00bae702018a529eb85ba4.png)

按住Ctrl+Shift+p，打开命令面板，将下面的命令输入，点击对应选项，再选择你安装的python解释器

```
Python:Select Interpreter
```

![](img/119c02596ec2da192842609ff398193c.png)

打开终端，输入下面命令，安装python依赖

```
pip install package_name
```

![](img/702fb94a0bd8ae3143146e25fe1f981c.png)

## MCP 部署

### Claude

首先，需要下载Claude 桌面端

> 下载链接：https://claude.ai/download

账号注册这里略过

打开后，点击左上角符号，选择File--Settings，点击它

![](img/d3d3e426919ffc3b3bc3707d9d38ee29.png)

选择Developer页面，然后点击Edit Config选项，会跳转到Claude目录

![](img/3e3aa01f34f98140aeb8d9172b56db09.png)

右键点击"claude_desktop_config.json"文件，选择 VSCode 打开

![](img/9f31a81e9229a5affd3cc2f770f74e4e.png)

然后把下面的代码复制粘贴进去

```
{
    &quot;mcpServers&quot;: {
        &quot;AbletonMCP&quot;: {
            &quot;command&quot;: &quot;uvx&quot;,
            &quot;args&quot;: [
                &quot;ableton-mcp&quot;
            ]
        }
    }
}
```

像这样，然后保存，退出

![](img/db9c7acbb8e43047b44c20adab704f9b.png)

重新启动 Claude，得到下图小锤子图标，代表部署成功

![](img/4318c92439cf2f20f64290202d69df89.png)

### Ableton

回到项目，点击"AbletonMCP_Remote_Script"（如下图）

链接：https://github.com/ahujasid/ableton-mcp

![](img/9d24a184916dbaae8506392ad7196eab.png)

然后再点击"_init_.py"文件

![](img/77b73a9fb7c42e411a232d301fff94ac.png)

右上角选择Download下载文件

![](img/03ac7e0216b326987bca6c99fdc993ab.png)

这一步需要将文件夹复制到 Ableton 的 MIDI 远程脚本目录下，作者提供一些路径思路供参考

> **For macOS:**

+   Method 1: Go to Applications > Right-click on Ableton Live app → Show Package Contents → Navigate to: Contents/App-Resources/MIDI Remote Scripts/

+   Method 2: If it's not there in the first method, use the direct path (replace XX with your version number): /Users/[Username]/Library/Preferences/Ableton/Live XX/User Remote Scripts

> **For Windows:**

+   Method 1: C:\Users[Username]\AppData\Roaming\Ableton\Live x.x.x\Preferences\User Remote Scripts

+   Method 2: C:\ProgramData\Ableton\Live XX\Resources\MIDI Remote Scripts\

+   Method 3: C:\Program Files\Ableton\Live XX\Resources\MIDI Remote Scripts\ Note: Replace XX with your Ableton version number (e.g., 10, 11, 12)

这里我用的是Windows系统，路径如下

```
C:\ProgramData\Ableton\Live 12 Trial\Resources\MIDI Remote Scripts
```

在 MIDI Remote Scripts 目录里新建一个文件夹，命名"AbletonMCP"

![](img/9b87bc25d6e9de35bcf59e5cb822b7c2.png)

把前面下载好的文件导入进AbletonMCP文件夹

![](img/1c8543e8f14b8ef368507e22433ccded.png)

打开 Ableton live 12，在选项栏找到偏好设置

![](img/7caffdf847d0d99c3616c7a8711a0bf6.png)

如下图设置，Ableton live 12部署完成

![](img/c3e6e5ecf5bfc0c5995a8858670ff59e.png)

运行效果如下

![](img/f2c6cbe700e2a347e7f557ad1999c829.png)

# 实录效果

![](img/755bf2051eee28d479dd6be754e67214.png)

[AbletonMCP测试案例1.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/XBeYbliUGoMuqyxz2gccQXCCnKd)

![](img/4e933e36b7da2c5ae85ef021e5a76ce5.png)

[AbletonMCP测试.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/R3pSbhTudovX9wxIL4acqxMmnab)

> 作者小结

> 测试下来，Claude 基础版本只能生成一些简单的音轨，读者如果需要实现更复杂的指令，请升级至Pro版。