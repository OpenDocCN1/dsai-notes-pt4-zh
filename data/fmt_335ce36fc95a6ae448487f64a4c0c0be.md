
# 【工具】N8N

> 来源：[https://my.feishu.cn/docx/HRuCdS7tToynXGxSt84ceoG8nkd](https://my.feishu.cn/docx/HRuCdS7tToynXGxSt84ceoG8nkd)

## n8n汉化

[N8N与Docker中文版部署教程分享](https://ncn6pyjmc4g9.feishu.cn/docx/UY05d0IarodNSpxYxjQcBsAgnbf?from=from_copylink)

> mac 反正很简单，直接去这个 GitHub 项目里面

> `https://github.com/other-blowsnow/n8n-i18n-chinese`

第一种方法：自带中文 `docker` 镜像），这是最适合新手、最简单的方法。我们不需要去下载复杂的文件，直接用别人打包好的中文版镜像替换掉你现在的英文版即可。

请按照以下三个步骤操作：

### 第一步：给新容器“腾位置”

因为你现在的 `n8n` 占用了 `5678` 这个端口，我们需要先把刚才那个英文版的容器删掉（放心，如果你之前用了我给的代码，数据保存在你的电脑里，不会丢）。

1. 打开 `Docker Desktop`。
1. 找到刚才那个绿色的 `n8n` 容器。
1. 点击右侧的 `垃圾桶图标` 删除它。

### 第二步：运行中文版命令

打开你的 Mac `终端 (Terminal)`，直接复制粘贴下面这段代码并回车：

```bash
docker run -d \
  --name n8n-chinese \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  -e N8N_SECURE_COOKIE=false \
  blowsnow/n8n-chinese
```

> 这段代码做了什么？

+   `blowsnow/n8n-chinese`：这是你截图里推荐的中文版镜像，会自动下载并运行。
+   `-p 5678:5678`：依旧使用 `5678` 端口，这样你刚才收藏的网址不用变。
+   `-v ~/.n8n:/home/node/.n8n`：关键，这会读取你之前保存的数据，所以你之前的设置都在。

### 第三步：等待并刷新

1. 输入命令后，终端会显示一串很长的字符，并且 `Docker Desktop` 里会出现一个新的容器 `n8n-chinese`。
1. 稍微等 `10-20` 秒（因为它需要初始化中文环境）。
1. 回到浏览器，刷新 `http://localhost:5678`。

这时候界面应该就是中文的了！
