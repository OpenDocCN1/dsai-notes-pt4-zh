# Dify本地部署

> 来源：[https://yunyinghui.feishu.cn/docx/KuyNdmalAoanrhxLf6WcwTcan9e](https://yunyinghui.feishu.cn/docx/KuyNdmalAoanrhxLf6WcwTcan9e)

---

# 本地部署教程

> 建议全程用梯子，保证安装速度

## 1、下载

在"https://github.com/langgenius/dify"中下载 Dify，这里选择最新版本，我已经下载好了，需要自取

![](img/d4f28003ca94ad8e480ae24cbba95c7b.png)

[dify-main.zip](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/QqxKbm8NFoTSQwxdhO9cPhb1npc)

![](img/c50279da087117436ed558ac524acafa.png)

下载完成后，将压缩包解压到 E 盘“D:\dify”中。进入到“E:\dify\docker”目录【目录看你们自己需要存放】，将

“.env.example”文件改名为“.env”，然后在该目录下打开 cmd

## 2、安装并启动

通过如下命令启动 Dify:

输入E:

![](img/a4d52a6394c621d61f37bd5bdac0902c.png)

输入cd E:\dify\docker

![](img/350e297511593ee6a77cba9c3d128500.png)

docker compose up -d

![](img/00a5d56604f77a411f336c2de81488f1.png)

等待一段时间后，当所有 images 下载完成后，Dify 启动成功。

停止容器命令：

docker compose down

## 3、访问网址

http://127.0.0.1/install 和 http://localhost/install

进入安装页面

![](img/d936bc6972cbffaf26b8fd3596560bea.png)

填写用户名和密码以及邮箱，点击设置后咱们就可以进入页面了