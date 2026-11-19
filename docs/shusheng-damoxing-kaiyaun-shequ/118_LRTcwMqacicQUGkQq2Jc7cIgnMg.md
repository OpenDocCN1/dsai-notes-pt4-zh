
# G0 玩转魔乐社区

> 来源：[https://aicarrier.feishu.cn/docx/KgySd61YYoK2YrxMDQrc9aaonYg](https://aicarrier.feishu.cn/docx/KgySd61YYoK2YrxMDQrc9aaonYg)

在本次课程中，我们带领大家从零开始玩转魔乐社区，通过实操方式帮助大家完成国产 AI 开发的第一步，包括社区平台使用、开发环境配置、模型创建与上传、体验空间等流程，助你快速融入国产 AI 生态圈，开启 AI 模型创作之旅。

## 前提条件

1. 登录注册魔乐平台 `https://modelers.cn/my/overview`
2. 登录注册沐曦开发机 `https://console.d.run/`

## 创建模型

### 平台操作

![](img/035ec697fc0bd995ddccd57df5cf1727.png)

![](img/a8954de39f800a03768b67b688bba976.png)

关于模型名命名小规范

```
<用途关键词>-<模型基础>-<技术关键词>-<版本/作者>
#eg
paper-classify-internlm-finetuned-v1
```

### 配置环境

#### 开发机的选择

去[开发机](https://console.d.run/zestu/instance/list)开一台机器，如图配置

![](img/43b937f9893d974a9bc5f703dc3788a2.png)

下载`git`、`python` 基础工具

```
apt update && apt install -y git git-lfs python3.10 python3.10-venv python3.10-distutils wget curl
```

#### 配置魔乐环境

`git` 拉下魔乐的模型空间

```
git clone https://modelers.cn/fak111/paper-classify-internlm-finetuned-zfc-v1.git
```

配置`python`环境，参考：`https://modelers.cn/docs/zh/openmind-library/1.0.0/install.html`

```
cd ~/paper-classify-internlm-finetuned-v1
python3.10 -m venv .venv
source .venv/bin/activate
pip install openmind_hub[all]
pip install "openmind[pt]" --extra-index-url https://download.pytorch.org/whl/cpu
```

q：如何理解`openmind_hub`和`openmind`

![](img/f5317a0e25ceaa8224e9404104d963eb.png)

检查`openmind_hub`

```
python -c "from openmind_hub import repo_info; print(repo_info('PyTorch-NPU/t5_small'))"
```

检查`openmind`

```
pip uninstall torch_npu -y
python -c "import openmind;print(openmind.__version__);"
```

输出如下图所示：

![](img/672fcd08277cc952171b793e4049b42e.png)

### 下载模型

下一个[小模型`t5_small`](https://modelers.cn/models/PyTorch-NPU/t5_small)，用`openMind Library`下载并加载模型 方式

```
touch openMind_download.py
```

```
# Use pipeline
from openmind import pipeline
pipe = pipeline("translation", model="PyTorch-NPU/t5_small", framework="pt")

print(pipe("who are you?"))
```

运行脚本

```
python openMind_download.py
```

输出如图所示

![](img/112a492562d2ade69c61062325851dbd.png)

### 上传模型

使用`openmind_hub`库中的`upload_file`向仓库中上传单个文件。文件

```
from openmind_hub import upload_folder

upload_folder(
    token="9exxd592(替换成自己的)",
    folder_path="/root/.cache/openmind/hub/models--PyTorch-NPU--t5_small",#模型位置
    repo_id="fak111/paper-classify-internlm-finetuned-zfc-v1",#模型空间名字
)
```

其中`token` 是访问令牌，获取方式如下图`https://modelers.cn/my/tokens`：

![](img/2f3f5feba2ccb45209c80c3c6c83c2a7.png)

至此，我们就完成了模型上传😄

![](img/aa972d07534768d5ad932fa7982eeea7.png)

## 创建空间

### 平台操作

![](img/c9b8017122c64e5d1acfce01321deb8c.png)

我们部署个简单的静态网页应用作为教学

![](img/5751f10b08cebba9d22af1e1cf497880.png)

### 开发机操作

```
git clone https://modelers.cn/fak111/hello-world.git
cd hello_world
```

创建`index.html`的脚本

```
cat > index.html << EOF
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>书生大模型实战营（沐曦魔乐专场）</title>
    <style>
        body {
            margin: 0;
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            background: linear-gradient(to right, #0f2027, #203a43, #2c5364);
            font-family: 'Helvetica Neue', sans-serif;
            color: #ffffff;
        }

        .container {
            text-align: center;
            max-width: 90%;
        }

        h1 {
            font-size: 2.8em;
            margin-bottom: 0.4em;
            background: linear-gradient(90deg, #00c6ff, #0072ff);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        p {
            font-size: 1.3em;
            color: #e0e0e0;
            margin-top: 0;
        }
    </style>
</head>

<body>
    <div class="container">
        <h1>书生大模型实战营（沐曦魔乐专场）</h1>
        <p>🏆欢迎报名</p>
    </div>
</body>

</html>

EOF
```

提交`index.html`文件

```
git add .
git commit -m "space init"
git branch -M main
git push -u origin main
```

完整操作的流程截图

![](img/fa366379204374dc3dafe39813851410.png)

效果如下图所示：

![](img/25de0e58813bc445489ad813730230cc.png)

## 总结

通过今天的学习，大家成功掌握了魔乐社区平台的基本功能，从环境搭建到模型、空间上传，整个流程简单流畅。希望大家能继续实践探索，借助魔乐社区轻松上手 AI 模型开发，一起助力国产 AI 更进一步！
