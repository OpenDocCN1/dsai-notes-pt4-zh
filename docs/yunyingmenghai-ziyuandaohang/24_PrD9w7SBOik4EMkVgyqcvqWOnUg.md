# AutoDL在线转绘/文生图

> 来源：[https://yunyingmenghai.feishu.cn/docx/SOwHdREeboURbdx7Sw7cWm6On0d](https://yunyingmenghai.feishu.cn/docx/SOwHdREeboURbdx7Sw7cWm6On0d)

# 算力平台

https://www.autodl.com/console/instance/list

# ComfyUI 工业级AI绘画创作工具

内置文生图、图生图、ControlNet、SVD图生视频、Animatediff动画等若干工作流，开箱即用。

镜像

https://www.codewithgpu.com/i/comfyanonymous/ComfyUI/yinghuoai-ComfyUI

# ComfyUI 镜像

https://www.codewithgpu.com/i/comfyanonymous/ComfyUI/tzwm_ComfyUI

ComfyUI 整合包，预置汉化，集成所有常用插件和模型包括 Flux 等，一键启动。

说明

启动和重启 webui 点上方工具栏中的「重启并运行所有单元格」。出现 http://0.0.0.0:6006 这个字样就算成功了。可以去 autodl 控制台打开「自定义服务」了

首次启动需要下载基本的模型，请稍等几分钟

镜像在 4090, A5000, 3090, 3080 上测试没问题

模型路径：/root/autodl-tmp/models 里的对应目录下

生成文件路径：/root/ComfyUI/output

以下为可以加速访问的学术资源地址：

+   github.com

+   githubusercontent.com

+   githubassets.com

+   huggingface.co

如果在终端中使用：

```
source /etc/network_turbo
```

```
curl -L -o FLUX.1-dev-ControlNet-Union-Pro.safetensors https://huggingface.co/Shakker-Labs/FLUX.1-dev-ControlNet-Union-Pro/resolve/main/diffusion_pytorch_model.safetensors
```