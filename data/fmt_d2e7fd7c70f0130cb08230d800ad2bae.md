
# StableDiffusion：云端在线版免费使用笔记分享（Kaggle版）

> 来源：[https://xmgtoagi.feishu.cn/docx/WsF0dWnwCoJ5FvxI8SUcXwpKnIw](https://xmgtoagi.feishu.cn/docx/WsF0dWnwCoJ5FvxI8SUcXwpKnIw)

玩AI绘画（SD），自己电脑配置不够？今天给大家介绍一下如何baipiao在线版AI绘画StableDiffusion。

Kaggle 是世界上最大的数据科学社区，拥有强大的工具和资源，可帮助您实现数据科学目标。（每周可以免费使用30个小时）。

文末有更多相关AI资源。

文章较长，可阅读原文享F受更佳阅读体验，以及获取更多AI相关资源

## 一、快速上手

### 1、注册账号（Kaggle）

不过多介绍：https://www.kaggle.com/

![](img/ccb27a49df8ce89e35771e0f9e6f6513.png)

### 2、复制并编辑代码

打开如下链接，复制并编辑后，创建你自己的代码

https://www.kaggle.com/code/xinsiac/zh-stable-diffusion-webui-kaggle

![](img/1825fad8680bde0a0f7bf6d7707a8c2d.png)

### 3、配置代码：GPU

进入你的代码（上一步点击按钮后会跳转入你的代码页面），进行GPU配置，StableDiffusion需要GPU的算力，Kaggle的CPU算力很弱很弱

步骤如下：

1. 点开NoteBook Options选项

![](img/fb628603230b60e25ebc5633e510994c.png)

2. 验证手机，开启GPU选项

这里不过多介绍。

![](img/fa6463d42c7c7cd0c760b83642c33f06.png)

3. 开启成功后，如下图介绍

![](img/afd52e244e35c91b17d714b5eda2b912.png)

### 4、运行服务

配置好后就可以运行代码，生成一个AI绘画服务StableDiffusion

步骤如下：

1. 启动虚拟机（远程电脑），注意：每周三十小时的GPU使用时间

![](img/9bbce5d876a3537c9b6a47b5b0d00a26.png)

2. 启动代码

注意：代码执行过程中，如果看到红色的警告不用太担心，只要代码没停止执行，就继续等等，直到看到输出链接

![](img/b5cf8ffe219e4960db7e72d89c7fa272.png)

3. 复制链接，在浏览器中打开：

![](img/f4c787138857bd150cbcbaff4a7329ae.png)

![](img/e50678ab5d2380ff5240610bc438239e.png)

## 二、进阶操作

### 1、模型下载

默认的代码下载的模型只有两个，当你想用一下其他模型的时候，应该怎么做呢？

#### 方式一、手动下载

操作方式麻烦，高手随意，小白绕行，直接参考方法二

参考下面代码分析

![](img/22bf14ffdf95d7b372635b4af54c7f6a.png)

```
!aria2c --console-log-level=error -c -x 16 -s 16 -k 1M https://civitai.com/api/download/models/57618 -d /kaggle/working/stable-diffusion-webui/models/Stable-diffusion -o Counterfeit-V3.safetensors
##########
#更换上面👆🏻这句代码里的链接和输出的文件名，贴会Kaggle里或者直接在Kaggle里修改，再执行代码即可下载模型
```

获取模型下载链接的方式，这里以 civitai.com为例

![](img/a5960dc489525537b7c50d67a3c82e7a.png)

> 建议不要下载太多模型，只有20GB空间，所以我把不需要的ControlNet模型，都注释了下载代码，不让它下载，减少空间占用

#### 如何下载 Lora 和 VEA

与更换模型的原理类似，除了修改下载链接和输出文件名，还要修改模型存放位置

![](img/051e385d2a424c924b470d376c82bc99.png)

```
# Lora
!aria2c --console-log-level=error -c -x 16 -s 16 -k 1M https://civitai.com/api/download/models/32988 -d /kaggle/working/stable-diffusion-webui/models/Lora -o blindbox_v1_mix.safetensors
    
# VAE
!aria2c --console-log-level=error -c -x 16 -s 16 -k 1M https://huggingface.co/gemasai/vae-ft-mse-840000-ema-pruned/resolve/main/vae-ft-mse-840000-ema-pruned.ckpt -d /kaggle/working/stable-diffusion-webui/models/VAE -o vae-ft-mse-840000-ema-pruned.ckpt
```

#### 方式二、插件下载

代码下载太复杂？来试试civitai 的SD插件

##### 插件安装

默认代码已经给安装了，如果没有请自己检查。

检查一下，代码里有没有安装这个civitai浏览插件，可以通过ctrl + F，打开查找框，输入civitai，快速查找相关代码：https://github.com/camenduru/sd-civitai-browser `/kaggle/working/stable-diffusion-webui/extensions/sd-civitai-browser`

![](img/7875ce604f60642198cb796ea2323067.png)

##### 模型下载

在运行的StableDiffusionWebUI中，切换到civitAi选项卡

![](img/f8177fd243ddcb7a333871b4bd5711f9.png)

![](img/41fc567128aaf72a2fdb5c67da3d92a1.png)

![](img/a4006b51602f528ab26fbd92a9be6460.png)

##### 刷新并读取模型

到这里模型下载已经完成了

![](img/64d2bcd83cd515d8a190abd6a0912a16.png)

### AI绘画和HuggingFace数据同步

生成的图想直接存放在云端，就需要这一步的操作。

#### 方式一：手动配置

操作方式繁琐，高手随意，小白直接用方式二

##### 注册HuggingFace账号

这里不过多介绍

![](img/606a9d1d345bc4a6e2e4294b0e7d030c.png)

##### 创建数据集DataSet

创建Dataset，来存储配置文件Config（我创建了两个一个公开的用来存储配置文件，一个私有的存储生成图的压缩文件）

![](img/6f6ac57b905f57c15b44f79f6615ba80.png)

![](img/283b62e152327bf5f7db32aee07acb46.png)

##### 生成Access Tokens

作用就是配置Kaggle 代码可以自动上传文件到HuggingFace。

![](img/95143201324b5888712fc8fbe9889f4e.png)

![](img/46836c19055e58a1baf014ae7a6703dd.png)

![](img/3601d500ccd6aa50475a941f38c2cf40.png)

##### 配置token到Kaggle里

![](img/a8a1e56960831b19cfa561cfab5c3b6c.png)

复制token到txt文件

![](img/ec909a285951706cab9bb65ecff1c8aa.png)

![](img/2ff8307fb82de7b67443a4781a37b7e9.png)

然后就可以看到token文件

![](img/7d4fd040179d3b3faf1bdc453c88e1db.png)

将文件路径写到代码里

用途：用于代码上传文件到HuggingFace时调取使用token

![](img/0b57f2cfb0a87def3969233642ced9cc.png)

![](img/8c8b37d0210688f4961e8d3917c2a94c.png)

```
#####################################################################
#  同步配置
# 使用huggingface保存和载入webui配置文件

huggingface_use = True
zip_output=True
huggingface_token_file = '/kaggle/input/hgtoken/huggingfacetoken.text'
huggiingface_repo_id = 'MSa/sd-output-images'
huggiingface_config_repo_id = 'MSa/sd-config'
# 将会同步的文件
config_files = [
    'ui-config.json',
    'config.json',
#     'styles.csv'
]

#功能函数
from pathlib import Path
from huggingface_hub import HfApi, login

# config 上传
def hugface_upload_config(huggingface_token_file, config_files, repo_id):
    if Path(huggingface_token_file).exists():
        with open(huggingface_token_file, encoding="utf-8") as nkfile:
            hugToken = nkfile.readline()
        if hugToken != '':
            # 使用您的 Hugging Face 访问令牌登录
            login(token=hugToken)
            # 实例化 HfApi 类
            api = HfApi()
            print("HfApi 类已实例化")
            %cd /kaggle/working/stable-diffusion-webui
            # 使用 upload_file() 函数上传文件
            print("开始上传文件...")
            for config_file in config_files:
                if Path(config_file).exists():
                    response = api.upload_file(
                        path_or_fileobj=config_file,
                        path_in_repo=config_file,
                        repo_id=repo_id,
                        repo_type="dataset"
                    )
                    print("文件上传完成")
                    print(f"响应: {response}")
                else:
                    print(f'Error: File {config_file} does not exist')
    else:
        print(f'Error: File {huggingface_token_file} does not exist')
```

##### 注意事项：

![](img/cfae28f6841a2eae2f439f40059155ff.png)

#### 方式二、插件配置

方式一手动操作太复杂？那就使用HuggingFace 的SD插件的方式吧！

##### 插件安装

确保代码里有下载HuggingFace 的SD插件

```
## Kaggle下载HuggingFace 的SD插件的代码
!git clone https://github.com/camenduru/stable-diffusion-webui-huggingface /kaggle/working/stable-diffusion-webui/extensions/stable-diffusion-webui-huggingface
```

##### HuggingFace选项卡

插件安装成功后，StableDiffusion WebUI 里会有HuggingFace选项卡

![](img/e341ce0e899f8058f837eea6b9d844e6.png)

##### 获得Kaggle 输出文件夹路径的方法

![](img/f3a9e12be1bd3ba05f129e8194bf2d67.png)

![](img/3b6de38e4b41c7c3f2dbd9c7b867edae.png)

```
# 其实Kaggle的文件夹路径都是相对固定的（如果你没做修改的话），可以直接用下面👇这个路径

/kaggle/working/stable-diffusion-webui/outputs
```

##### 在HuggingFace创建Dataset

![](img/0038d8d0385867c027b5b180e0ce441f.png)

![](img/592dd0ec041cd6ea00738c8217a5ec8b.png)

##### 复制HuggingFace的Dataset路径

![](img/eefaf2c33d0f8602a9fddec0a65d39fb.png)

##### 生成HuggingFace的写入token

![](img/e869ff854f76ce35f7ab71f687cde589.png)

![](img/aba665744d0dc3d93178182e741093cf.png)

![](img/84dad993496ce6d73d583655b1308901.png)

![](img/44348d9cef1676f95e3a60804868d328.png)

复制token，然后粘贴到插件的token输入框里

推送到HuggingFace的效果

![](img/11bc6cc0b60dff323070a8f26c0aed51.png)

### 如何导出导入代码

#### 导出代码

如果担心Kaggle封禁项目代码，大家可以下载自己的代码备份

![](img/a330267f3a5ee24b6e88c4aac3e0fd69.png)

#### 新建项目，导入代码

有了代码文件，可以新建项目，再导入代码，这样，你就获得一个新的StableDiffusion项目了（配置文件会是旧的，因为是保存在HuggingFace上，不过只要不同步，就是新的配置文件）

![](img/e1763c8689a7fa20a8048d1ab508d292.png)

![](img/7510d935ab4ff21a355e67feeee86697.png)

![](img/c07301b08e7ebadc802520d221e97945.png)
