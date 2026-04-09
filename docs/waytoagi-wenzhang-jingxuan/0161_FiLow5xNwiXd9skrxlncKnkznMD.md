# [应用开发] 将PPT转换为MD语言文本 - PPTX2MD

> 来源：[https://waytoagi.feishu.cn/docx/PcqLdGmetoJPLSx8vUBc3c2Gnqd](https://waytoagi.feishu.cn/docx/PcqLdGmetoJPLSx8vUBc3c2Gnqd)

Github上面一个很简单的项目，把PPTX格式的文件转换成MD语言（可以用txt或者其他任何文本输出），没有用到大模型，不过感觉可以在这个基础上接上大模型做进一步的开发。

原项目地址 https://github.com/ssine/pptx2md

这个趋势应该还是有的，因为Markdown是一种非常轻量化的结构语言，而PPT通常也是以同样的结构来制作的，两者的切换可以让更多用户用markdown语言的习惯进行PPT写作。作为一个写了十几年PPT的牛马，虽然这个模式仍然存在一些漏洞（比如图片，图表，表格等插件的处理），但整理来讲，确实是大大提升了写作效率，而且可以清晰检查自己的前后逻辑是否有漏洞。

Colab上跑通的代码，本地运行可以自行修改

```
#安装库，本地一次性安装就可以
!pip install pptx2md
!pip install python-pptx
!apt-get update
!apt-get install -y pandoc

# 清空当前目录下存储的历史文件
!rm -rf /content/*

# 上传 PPTX 文件
from google.colab import files
uploaded = files.upload()

# 识别上传的文件名
pptx_files = {file_name: file for file_name, file in uploaded.items() if file_name.endswith(&#x27;.pptx&#x27;)}
if not pptx_files:
    print(&quot;没有找到 PPTX 文件，请上传 PPTX 文件。&quot;) #执行报错检查
else:
    # 假设只上传了一个 PPTX 文件，取第一个文件
    pptx_file_name = next(iter(pptx_files))
    print(f&quot;上传的 PPTX 文件名: {pptx_file_name}&quot;)

    # 执行 pptx2md 脚本，转换 PPTX 文件到 Markdown
    !pptx2md {pptx_file_name}

    # 确保 out.md 文件存在
    !ls -l | grep out.md

    # 将 Markdown 文件转换为 TXT 格式
    !pandoc out.md -o output.txt

    # 下载 TXT 格式的文件
    files.download(&#x27;output.txt&#x27;)
```

由于之前我们的PPT通常不是用现有框架做的（我之前比较喜欢白板，然后自己拉标题，正文），所以识别度并不是很好，如果在ppt现有框架内写作的会相对好一些（比如在预设的标题，正文框里面填写）。这个之后可以进一步去做优化。

---

准备接上智谱的API试试总结能力