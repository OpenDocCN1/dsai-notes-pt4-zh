# Macos安装Ollama

> 来源：[https://jlqyxzzvbk.feishu.cn/docx/TFzWd8POaoeETZxMooDcBFvanbd](https://jlqyxzzvbk.feishu.cn/docx/TFzWd8POaoeETZxMooDcBFvanbd)

在 macOS 上安装 Ollama 的步骤大致如下：

1.  **下载安装包**

+   打开 Ollama 官方下载页面：Download for macOS

+   该安装包支持 macOS 11 Big Sur 及以上版本。点击“Download for macOS”按钮开始下载。

1.  **安装应用程序**

+   下载完成后，双击下载的压缩包（通常为 “Ollama-darwin.zip”）进行解压。

+   解压后，会出现安装程序或一个应用文件。按照系统提示（一般包括点击“打开”并选择“Move to Applications”）完成安装。

1.  **启动与验证**

+   安装结束后，在“应用程序”中找到 Ollama 并启动，或通过 Spotlight 搜索 “Ollama” 打开应用。

+   启动后，系统状态栏通常会显示 Ollama 的图标，表明程序已经在后台运行。

+   可打开终端，输入命令

```
ollama -h
```

+    来查看所有可用的命令，确保安装成功。

1.  **运行模型**

+   使用 Ollama 非常简单。例如，要运行官网支持的 Llama 3 模型，可以在终端输入：

```
ollama run llama3
```

+    第一次运行时会自动下载对应模型，下载完成后便可直接与模型对话。

1.  **其他配置（可选）**

+   如果需要自定义模型存放位置或其他环境变量，在 macOS 下可以通过修改终端配置文件（如 ~/.zshrc 或 ~/.bash_profile）添加相应的 export 命令。例如，设置模型下载路径：

```
export OLLAMA_MODELS=&quot;/path/to/your/models&quot;
```

+   修改后记得执行 source ~/.zshrc（或对应的配置文件）使配置生效。