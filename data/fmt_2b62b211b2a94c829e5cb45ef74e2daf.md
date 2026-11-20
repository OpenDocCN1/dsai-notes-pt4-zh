
# ComfyUI环境部署和插件安装（模型互用）

> 来源：[https://aigcreative.feishu.cn/docx/TLlod9SI4oyc2qx1u7UcXrO3nJh](https://aigcreative.feishu.cn/docx/TLlod9SI4oyc2qx1u7UcXrO3nJh)

> 来源：https://mp.weixin.qq.com/s/cNR0H2O-5Qj8-Dad-dt7jw

ComfyUI为Stable Diffusion提供了一个强大的节点流程操作界面，带来了更多的自由度和可复现性，同时降低了显存要求。对于那些希望深入了解和利用Stable Diffusion技术的用户来说，是一个不可多得的选择。(但节点式的工作流也提高了一部分使用门槛。)本文将带您进入comfyui的世界。

![](img/d7063ef8f1d78378d11bc1896b2f173b.png)

## 1. 下载和安装ComfyUI

首先从官方的github连接上找到最新的release包，下载解压即可使用：

![](img/b4b4e32dfe7eafa3b9372d7799b611ed.png)

点击进入Releases，点击下面的"Download link with stable pythorch 2.1 cu121"下载安装包。

地址：https://github.com/comfyanonymous/ComfyUI/releases

![](img/e08c115e8dd413442a7f416430aea569.png)

解压安装包到合适的位置，打开文件夹我们可以看到`run_nvidia_gpu`，双击就能启动 ComfyUI 。

![](img/86c0687370a83d84cd1b4b3e5f27931f.png)

注意：初始安装中不包含任何模型和插件。

## 2. 配置模型、LORA

ComfyUI虽然自带了pytorch和stable diffusion环境，但是不包括模型，所以需要将模型放置到ComfyUI对应目录中：

1. 将模型放入以下目录：
   - 大模型：`ComfyUI_windows_portable\ComfyUI\models\checkpoints`
   - VAE模型：`ComfyUI_windows_portable\ComfyUI\models/vae`
   - Lora模型：`ComfyUI_windows_portable\ComfyUI\models/loras`

![](img/0dd25da170cbc88bbfcda474819985c4.png)

如果你本地电脑中有使用过WebUI，那么你的模型文件通常是放在stable-diffusion-webui目录下的，复制过来就会很浪费空间，所以ComfyUI的作者提供了使ComfyUI和Web UI共享相同的模型，以避免重复拷贝大量模型文件。

![](img/bd123c053a5b370e33b4b294e2cf31bf.png)

2. 用记事本打开`extra_model_paths.yaml`文件，替换你本地webUI的路径地址：把里面`base_path`的路径改成你的 webui 的地址。比如我的是`F:\novelai-webui-aki-v3A`

![](img/db8bc2dc53d80cbc4cf69a2c94316f26.png)

3. controlnet地址需修改成WEBUI的 controlnet 模型安装目录，`extensions\sd-webui-controlnet\models`。启动ComfyUI就可以在模型栏读取到本地模型了：

![](img/652f409ce14d6bed832751ac7525bc64.png)

## 3. 更新ComfyUI

如果后面需要更新ComfyUI，可以直接使用ComfyUI中的update脚本，不需要重新下载和配置：

在`ComfyUI_windows_portable\update`文件夹中找到`update_comfyui`和`update_comfyui_and_python_dependencies`两个文件。使用`update_comfyui`来更新ComfyUI，等待出现"Done"表示成功。关于配置环境的更新，一般情况下不需要进行。

![](img/4deae7478bd88ab75db13936667ef839.png)

## 4. 安装插件

ComfyUI还支持多种插件，它们可以增强用户的体验和功能。

1. 辣椒酱的界面汉化：该插件可以汉化ComfyUI的界面，使中文用户更容易理解和操作。

https://github.com/AIGODLIKE/AIGODLIKE-COMFYUI-TRANSLATION

2. 提示词风格样式：这个插件允许用户自定义提示词的风格和样式，增加了生成图像的创造性。

https://github.com/twri/sdxl_prompt_styler

3. 提示词中文输入：该插件为用户提供中文输入功能，使用户可以更便捷地进行中文文本生成。

你可以通过GitHub下载这些插件并按照说明进行安装。

安装方式：

进入上面需要的插件链接，点击“Code - Download ZIP”进行下载，下载后解压放入`ComfyUI_windows_portable\ComfyUI\custom_nodes`中。

![](img/753e4f47fe86efce2d0792b27a7ce329.png)

你可以使用我网盘提供的整合包进行下载，整合包已经集成了常用的插件（比如汉化补丁，插件管理等），省去下载后还要安装插件。

链接：https://pan.quark.cn/s/1b6b4d451e9c

![](img/04cc306c49af1e4fd922323bc70f6475.png)

## 5. ComfyUI的优势和劣势

### 优势

1. 低显存要求和快速启动：ComfyUI相对于Web UI对显存要求较低，启动速度快，生成图像速度也更快。

2. 更高的生成自由度：ComfyUI允许用户自定义工作流程，调整模块连接以获得更灵活的生成效果。

3. 与Web UI共享环境和模型：用户可以同时使用ComfyUI和Web UI，并且可以共享模型和环境。

4. 可复现性：用户可以构建自己的工作流程，导出流程并与他人分享，同时在出现错误时能够清晰地定位问题。

5. 图像还原功能：生成的图片可以还原整个工作流程，模型选择也会被保存。

### 劣势

1. 操作门槛高：使用ComfyUI需要具备清晰的逻辑和操作理解，相较于Web UI，有一定的学习曲线。

2. 生态不如Web UI多：相对于Web UI，ComfyUI的生态系统较小，虽然包含了常用的插件，但Web UI仍然拥有更多可用插件。

## 6. 生成效果图

照旧放几张用ComfyUI生成的图片。

![](img/fb3fac543dcf3d492f1a55dc8bb3e5e2.png)

<https://aigcreative.feishu.cn/sync/K4Gvd9vUQsy9Oubwqaeclu0Fnbg>
