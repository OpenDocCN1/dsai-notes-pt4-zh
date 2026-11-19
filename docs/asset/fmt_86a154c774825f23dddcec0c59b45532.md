
# AbletonMCP：实现Claude和Ableton交互制作音乐

> 来源：[https://v11enp9ok1h.feishu.cn/docx/N6bhdN7dmo6thYxtUcWcP4uvnA3](https://v11enp9ok1h.feishu.cn/docx/N6bhdN7dmo6thYxtUcWcP4uvnA3)

> 作者通过构建一个 MCP，让 Claude 可以直接与 Ableton 对话，只需输入提示即可创作音乐！

> 项目链接：https://github.com/ahujasid/ableton-mcp

![](img/db0f64d5867fa83782ec6399c2f5b4fc.png)

[2nKWonLhwf0iamcb.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/YEg8bEI8AoMHbJx9LH0cmYr5nkc)

## 详细教程

### 工具安装

#### Ableton

说明里提示需要安装 Ableton Live 10 或更高版本，这里我们安装 Ableton Live

其他系统可参考下面的教程进行安装

下载完成后进行解压，得到下图，双击 `Ableton Live 12 Trial Installer` 进行安装即可

![](img/14321dd370d40188a64d502f81e5f3ef.png)

安装完成后打开，点击 开始免费试用，进行网页验证，再退回软件界面进行加载，Ableton Live 12安装完成。

注意：安装后的Ableton Live 12记得设置桌面快捷方式，方便使用

![](img/61dc3320b7185064aa0edd5da5be0ada.png)

![](img/849110a2f4c2636bab3bbcca8776b803.png)

#### Python

说明里提示需要安装 `Python 3.8` 或更高版本，这里我选择 `python 3.13.2` 版本

下载链接：https://www.python.org/downloads/

进入网站后，找到 Downloads，选择Windows，进入页面

![](img/eb71a57df74a0cbd6691126d5cc47afe.png)

我的电脑64位，选择如下图，点击下载

![](img/378f820a0a024c071bc02969e32833ef.png)

下载完成后，点击安装，如下图所示勾选，然后选择 `Customize installation`

![](img/c02a63392fafdc523e410feace887dec.png)

后边没有什么特殊，直接安装即可

![](img/133dfde3b3ef61c194d68f978cf32988.png)

#### uv

> 安装教程：https://docs.astral.sh/uv/getting-started/installation/

这里还是以Windows为例，打开 `powershell`，将下面指令复制粘贴进去，回车

```
powershell -ExecutionPolicy ByPass -c &quot;irm https://astral.sh/uv/install.ps1 | iex&quot;
```

![](img/b7565f247cbcaad39a26d35fdff7ba5d.png)

接着以管理员身份打开第二个 `powershell`，这一步主要是更改执行策略，允许从互联网运行脚本

输入下面指令，然后回车，会显示当前的执行策略

```
Get-ExecutionPolicy
```

再输入 `Set-ExecutionPolicy RemoteSigned`，这一步是允许本地脚本运行，并允许从互联网下载的脚本，只要它们已经被签名。

```
Set-ExecutionPolicy RemoteSigned
```

最后输入Y，回车即可，可以再输入一次 `Get-ExecutionPolicy` 进行验证

![](img/9d7ed0360950fce98ebfd350c64835a8.png)

再进入 `powershell`，输入下面的指令，回车，再输入 `uv` 进行验证，成功运行如下图

```
powershell -ExecutionPolicy ByPass -c &quot;irm https://astral.sh/uv/0.6.9/install.ps1 | iex&quot;
```

![](img/c43acec63ba4287f45cdbbc3d9c74290.png)

打开命令行，输入下面命令，回车，安装 `python` 包

```
uv python install
```

#### Visual Studio Code 安装（这一步如果你有IDE可以直接跳过）

进入下面链接点击下载安装即可

下载地址：https://code.visualstudio.com/

![](img/c20256f06b615f5530a7ea8254c35a90.png)

### 环境配置

#### Python

> 参考教程：https://blog.csdn.net/sensen_kiss/article/details/141940274

在本地按照下面的路径找到环境变量

> 系统--系统信息--高级系统设置

![](img/506acaef67507ab61d6e7592dd70e104.png)

找到系统变量里的 `Path`，新建，把 `Python` 的安装路径输入进去，再点确定，到此完成 `python` 的环境配置

![](img/f86d5a3891f77f2aa0a24e6f208441e2.png)

![](img/535781e2cfba967d45bf0e5e8f283b70.png)

最后在运行输入 `cmd`，回车，在命令行输入下面的命令进行验证即可，下图为成功配置

```
python --version
```

![](img/cdb4e08dfa02c07f4b7184f77ec480b9.png)

#### Visual Studio Code（后面简称 `VSCode`）`python` 配置

> 参考教程：https://blog.csdn.net/jiqimiao/article/details/143446166

进入 `VSCode`，如下图左侧选择扩展图标，搜索栏输入 `python`，回车，选择如下图第二个，点击 `Install` 安装

![](img/e27405a94b03272739b8db5139b1a462.png)

选择左上角，新建一个 `python` 文件，记得保存一下，以便运行

![](img/2d4fb3a34373e99a0cce97bb079ce1ed.png)

![](img/16f7dd0f7ab42f481da76cc2666b5a86.png)

按住 `Ctrl+Shift+p`，打开命令面板，将下面的命令输入，点击对应选项，再选择你安装的 `python` 解释器

```
Python:Select Interpreter
```

![](img/6f93c4744ac025cd836cc7bad40efeb7.png)

打开终端，输入下面命令，安装 `python` 依赖

```
pip install package_name
```

![](img/991a1a24c0930610fd281eb2d3905c0e.png)

### MCP 部署

#### Claude

首先，需要下载 `Claude` 桌面端

> 下载链接：https://claude.ai/download

账号注册这里略过

打开后，点击左上角符号，选择 `File--Settings`，点击它

![](img/0010a838d50ca0a315db8d895279b52c.png)

选择 `Developer` 页面，然后点击 `Edit Config` 选项，会跳转到 `Claude` 目录

![](img/b71d969d3bdc828118fed204c2c76e11.png)

右键点击 `claude_desktop_config.json` 文件，选择 `VSCode` 打开

![](img/dabd052269456c832f60f15b6cfa07d3.png)

然后把下面的代码复制粘贴进去

```
{
    &quot;mcpServers&quot;: {
        &quot;AbletonMCP&quot;: {
            "command": "uvx",
            "args": [
                "ableton-mcp"
            ]
        }
    }
}
```

像这样，然后保存，退出

![](img/bb98114a9b368de010a31ce582e1a584.png)

重新启动 `Claude`，得到下图小锤子图标，代表部署成功

![](img/6bd1097921ec862cf3c673b79f6c3b93.png)

#### Ableton

回到项目，点击 `AbletonMCP_Remote_Script`（如下图）

链接：https://github.com/ahujasid/ableton-mcp

![](img/e5e0de7a696255e6be839fa2d31021d9.png)

然后再点击 `_init_.py` 文件

![](img/b8fb7abd0dc61d353319a45d20146ef3.png)

右上角选择 `Download` 下载文件

![](img/b9fcab0c4e0efd0415ea11d4e5709e96.png)

这一步需要将文件夹复制到 `Ableton` 的 `MIDI Remote Scripts` 目录下，作者提供一些路径思路供参考

> **For macOS:**

+   Method 1: Go to Applications > Right-click on `Ableton Live` app → Show Package Contents → Navigate to: `Contents/App-Resources/MIDI Remote Scripts/`

+   Method 2: If it's not there in the first method, use the direct path (replace XX with your version number): `/Users/[Username]/Library/Preferences/Ableton/Live XX/User Remote Scripts`

> **For Windows:**

+   Method 1: `C:\Users[Username]\AppData\Roaming\Ableton\Live x.x.x\Preferences\User Remote Scripts`

+   Method 2: `C:\ProgramData\Ableton\Live XX\Resources\MIDI Remote Scripts\`

+   Method 3: `C:\Program Files\Ableton\Live XX\Resources\MIDI Remote Scripts\` Note: Replace XX with your Ableton version number (e.g., 10, 11, 12)

这里我用的是Windows系统，路径如下

```
C:\ProgramData\Ableton\Live 12 Trial\Resources\MIDI Remote Scripts
```

在 `MIDI Remote Scripts` 目录里新建一个文件夹，命名 `AbletonMCP`

![](img/724deaab352ef24869484d1c2ec529d8.png)

把前面下载好的文件导入进 `AbletonMCP` 文件夹

![](img/a3748f49c5dbd498d9318f8a23abea43.png)

打开 `Ableton live 12`，在选项栏找到偏好设置

![](img/6321628f924fd85f9b2bca9329dafc11.png)

如下图设置，`Ableton live 12` 部署完成

![](img/481e6cb2ed30426fa0c7c135de70bfe7.png)

运行效果如下

![](img/aee5e19e97945bf18589553c4261bd30.png)

## 实录效果

![](img/51547f4060b48987b538e39c94b03708.png)

[AbletonMCP测试案例1.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Q6NEbyjAkocHxsx1zlFcBFB0nWd)

![](img/b946d0171f93c5234baf75e6044d5107.png)

[AbletonMCP测试.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/PaO6bQhMroL1Ifx5SeJcKONjngg)

> 作者小结

> 测试下来，`Claude` 基础版本只能生成一些简单的音轨，读者如果需要实现更复杂的指令，请升级至 `Pro` 版。
