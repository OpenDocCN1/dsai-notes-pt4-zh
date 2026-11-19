
# 基于华为昇腾910B，实战InternVL2-8B模型推理

> 来源：[https://aicarrier.feishu.cn/docx/QrMWdwhH0o9nlCxIY5XcQVsRngM](https://aicarrier.feishu.cn/docx/QrMWdwhH0o9nlCxIY5XcQVsRngM)

本文将带领大家基于启智平台，使用 `LMDeploy` 推理框架在华为昇腾 910B 上实现 `InternVL2-8B` 模型的推理。

https://github.com/OpenGVLab/InternVL

https://github.com/InternLM/lmdeploy

# 1. 登录启智平台

https://openi.pcl.ac.cn/

![](img/62762c9316753df95d3dbe0370bf2141.png)

# 2. 创建云脑任务

我们新建云脑任务。

![](img/d54e58c2a44ab2027e7fe6aeb55e8b15.png)

目前启智平台提供 4 种 AI 训练任务（调试任务、训练任务、在线推理、通用任务），这里我们选择调试任务。

所属项目，我们选择一个已经有的项目，没有项目，可以新建，这个就不详细展开。（可以点击右上角）

![](img/beb39ceca5c65a0679529555a2502334.png)

![](img/3848c5ec8c860dc4c38ae2d92c398cdc.png)

接下来是选择算力平台，启智平台目前提供好几个厂商的算力平台（英伟达、昇腾NPU、遂源GCU、寒武纪MLU、海光DCU、天数智芯GPGPU、沐曦GPGPU），这里我们选华为昇腾NPU。

资源规格栏，我们选择 D910B 的显卡。（显存 64GB、CPU 24、内存 192GB）

![](img/7625f8a95c725ac7caa5ab88536e08b2.png)

镜像栏会随着我们选择的显卡出现相应的模型镜像，这里我们选择 `openmind_cann8`。

![](img/2531487876cdbcaa1e6c83b30717e703.png)

![](img/5bc0ef099bb307718cdd1812abd607e6.png)

点击新建任务等待服务器创建新任务，创建完成系统会进入服务器资源分配等待列表中。

![](img/98fc4262bb8f85c27e0f65cd4c8c0573.png)

当分配资源完成后，状态变成运行状态，这个时候右边操作会出现调试按钮。

![](img/663fda8c3da97a1d45b39400a4c5eb65.png)

# 3. 模型调试

我们点击调试按钮，进入 `JupyterLab` 调试代码界面。

![](img/5f46906d0614f34b9bdb672ac7b96806.png)

## 3.1 下载模型

我们在 `JupyterLab` 界面编写下载模型代码脚本。

`start.sh`

```
#!/bin/bash

# conda install git-lfs -y

pip install -U openi -i https://pypi.tuna.tsinghua.edu.cn/simple

pip install c2net
```

`model_download.sh`

```
#!/bin/bash

# 定义变量
model_user_name="FoundationModel/OpenGVLab"
model_name="InternVL2-8B"
save_path="/home/ma-user/work/pretrainmodel/InternVL2-8B"

# 检查保存路径是否存在，如果不存在则创建
if [ ! -d "$save_path" ]; then
  mkdir -p "$save_path"
fi

# 调用 openi 命令下载模型
openi model download $model_user_name $model_name --save_path $save_path
```

`checkmodel.py`

```
from c2net.context import prepare

# 初始化导入数据集和预训练模型到容器内
c2net_context = prepare()

# 获取预训练模型路径
InternVL2_8B_v1_path = c2net_context.pretrain_model_path + "/" + "InternVL2-8B"

print(InternVL2_8B_v1_path)
```

我们可以将脚本和代码上传到 `JupyterLab` 调试界面里面。

![](img/3b63a2d424f457a174ce102111473873.png)

![](img/20d5ac611736ae07e845ad2680ddfd36.png)

分别执行以上脚本和代码，先执行 `start.sh`，再执行 `model_download.sh`。

```
bash start.sh
```

![](img/c26d99e5288903b23d9195a13fb3811c.png)

![](img/49a724b9233c6728ce18226e6e6faa06.png)

再执行 `model_download.sh` 下载模型。

```
bash model_download.sh
```

![](img/010518993289d707b216bb5723fe0b24.png)

当模型下载完成后，我们执行 `checkmodel.py` 检查模型并初始化 代码目录、数据集、模型输出、模型预训练目录。

```
python checkmodel.py
```

![](img/5267f123c200941d1c48277123c0ad56.png)

刷新一下当前目录，我们会看到左边代码区 `code`、`dataset`、`output`、`pretrainmodel` 4 个文件夹，顾名思义，`code` 是放代码的；`dataset` 是放数据集的；`output` 是模型训练或者微调输出目录；`pretrainmodel` 是模型挂载的目录。

我们进入 `pretrainmodel`：

```
cd /home/ma-user/work/pretrainmodel/InternVL2-8B
```

![](img/054aa344245e6e9a05fc9bcbab31ffc0.png)

我们已经可以看到下载好的模型权重文件了。

## 3.2 支持华为 ascend 国产硬件接入大模型推理框架 dlinfer

这里我们借助一下开源项目 `dlinfer`，项目地址 [https://github.com/DeepLink-org/dlinfer](https://github.com/DeepLink-org/dlinfer)。

目前它支持 `LMDeploy` 部分模型推理，见下表：

![](img/58147de204e6c9360d29e1fa6ba554e4.png)

安装参考：[https://pypi.org/project/dlinfer-ascend/](https://pypi.org/project/dlinfer-ascend/)

```
pip install dlinfer-ascend==0.1.0.post1
```

![](img/2e46df6a0a63e199f3f9649b4b112582.png)

## 3.3 LMDeploy 推理框架安装

接下来我们需要在 `code` 代码目录下面下载 `LMDeploy` 推理程序并安装。安装之前我们需要下载 `LMDeploy` 源码，源码中需要删除 `LMDeploy` 推理框架中 `requirements/runtime.txt` 中关于 `triton` 依赖包。

因为 `Triton` 是 NVIDIA 推出的一款开源推理服务软件，旨在简化深度学习模型在生产环境中的部署和执行。它属于 NVIDIA AI 平台的一部分，能够在基于 GPU 或 CPU 的基础设施（如云、数据中心或边缘设备）上运行。我们在华为平台上部署也用不到它。（如果你安装大概率你是安装不上的，不信你试一试。）

我们修改 `requirements/runtime.txt`：

![](img/92db641b2cb6cb27028faf27b6b271e8.png)

```
cd code
git clone https://github.com/InternLM/lmdeploy.git
# 如果网络慢可以使用下面代理
git clone -b v0.6.1 https://mirror.ghproxy.com//https://github.com/InternLM/lmdeploy.git
cd lmdeploy
pip install -e .
# 升级 pillow 到 11.0.0 版本
pip install --upgrade pillow
# 使用 transformers 4.44.1 版本，不要升级到最新版本
pip install transformers==4.44.1 -U
```

![](img/c3087916de629295e9efbee28c323b80.png)

检查一下 `LMDeploy`

```
pip show lmdeploy
```

![](img/ac37f1af26bb9c31906cd1402217307b.png)

注意：这里我们需要注意 `LMDeploy` 版本必须要大于 0.6.0 版本。

## 3.4 LMDeploy 推理

接下来我们编写推理代码实现模型推理。

`inference.py`

```
image = load_image('https://raw.githubusercontent.com/open-mmlab/mmdeploy/main/tests/data/tiger.jpeg')

from lmdeploy import pipeline, PytorchEngineConfig
from lmdeploy.vl import load_image
if __name__ == "__main__":
    pipe = pipeline('/home/ma-user/work/pretrainmodel/InternVL2-8B',
                    backend_config=PytorchEngineConfig(tp=1, device_type='ascend'))
    response = pipe(('describe this image', image))
    print(response)
```

执行如下命令

```
cd code
touch inference.py
```

复制以上代码到 `inference.py` 代码中

![](img/b21f40e58214c88f53a79a99b1dc6096.png)

执行推理代码

```
python inference.py
```

![](img/cb595d6bf1cfa27361e6bbbb7a4b11c2.png)

我们查看一下这张原图

[https://raw.githubusercontent.com/open-mmlab/mmdeploy/main/tests/data/tiger.jpeg](https://raw.githubusercontent.com/open-mmlab/mmdeploy/main/tests/data/tiger.jpeg)

![](img/417cf97381e752275dde2a3ed2a7c76b.png)

从模型推理来看回答的是正确的。通过简单的推理代码就可以完成多模态模型的推理，效果还是不错的。

我们查看一下显存情况

```
npu-smi info
```

![](img/5fa3dec01831c46a478dac5ed206824b.png)

`LMDeploy` 默认使用 0.9 的缓存参数设置，这里我们没有修改默认参数，所以模型推理后剩余的显存都分配给模型缓存使用了，所以占用的显存比较大。这里就不详细展开技术细节了。想了解这块知识可以参加书生大模型实战营，学习关于模型量化相关的知识。

我们使用 `lmdeploy chat` 来推理。

```
lmdeploy chat /home/ma-user/work/pretrainmodel/InternVL2-8B --backend pytorch --device ascend
```

![](img/cbffaa9d8c78b3d8059b4db8a999afca.png)

我们输入问题测试下推理效果：

![](img/05dea34acba6bc4b494b9c09cc169171.png)

# 4. 总结

在 0.6.0 这个版本上开始支持华为昇腾 NPU，本文主要讲述了如何使用 `LMDeploy` 推理框架在华为昇腾 910B 上实现 `InternVL2-8B` 模型的推理。

`LMDeploy` 目前支持 `InternVL1.5` 和 `InternVL2` 系列的多模态模型，因为时间关系我这里只做了 2B 和 8B 模型的推理，其他模型没有做过测试。感兴趣的小伙伴也可以根据文档实现其它模型的推理和适配，我们也期待 `LMDeploy` 推理框架支持更多更好用的多模态大模型。
