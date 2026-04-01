# AI 声音克隆神器！MockingBird 轻松实现用自己的声音进行文字转语音 ！

> 来源：[https://aiyanxishe.feishu.cn/docx/UPljdpN9eoipm4xquQscmgvxnTh](https://aiyanxishe.feishu.cn/docx/UPljdpN9eoipm4xquQscmgvxnTh)

![](img/cb21f830bbd33fc4837fc44f40c52d4c.png)

[1.安装](https://www.python.org/)Python 选择3.8 或更高版本 【官方链接】

[2.安装](https://pytorch.org/get-started/locally/) PyTorch， 直接官网下载 【官方链接】，如果GPU不支持CUDA，请默认选择，没有显卡就选择CPU。

3.安装 ffmpeg 【[下载链接](http://ffmpeg.org/download.html#build-windows)】，下载后把bin目录路径path到系统变量里，见视频教程。

[4.下载](https://github.com/babysor/MockingBird) MockingBird 【开源项目】下载解压后，直接在目录输入CMD 进入终端

5.在终端下安装剩余的必备环境包：

```
python -m pip install --upgrade pip
```

```
pip install -r requirements.txt
```

```
pip install webrtcvad-wheels
```

6.下载语言模型，或使用自己训练好的模型

7.下载模型后，确保 xxx.pt 格式的文件放在 synthesizer\saved_models文件夹下，saved_models 如不存在请新建

8.最后执行运行命令，就可以启动程序了！

### 启动Web程序（v2）

```
python web.py
```

运行成功后在浏览器打开地址, 默认为 http://localhost:8080

### 训练数据集下载【[点击](https://drive.google.com/u/0/uc?id=110A11KZoVe7vy6kXlLb6zVPLb_J91I_t&export=download)】

GUI启动命令:

### 启动工具箱：

```
python demo_toolbox.py -vc -d &lt;datasets_root&gt;
```

假如数据集路径为 C:\data\aidatatang_200zh，那么 <datasets_root>就是 C:\data

程序界面功能说明：

![](img/890648ab899506e351746eb9b69e0d34.png)