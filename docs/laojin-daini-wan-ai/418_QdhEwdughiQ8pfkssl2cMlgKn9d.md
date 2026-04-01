# XXXBOT

> 来源：[https://my.feishu.cn/docx/GBTYdow1MoOqbOxc7ifcsNBUnSD](https://my.feishu.cn/docx/GBTYdow1MoOqbOxc7ifcsNBUnSD)

![](img/602248e451385095c9001085f433ce30.png)

![](img/5eb1d2509bc872a91c70d485c5e06629.png)

![](img/bceea7c4b4aa83b9499b4bb044ea8fef.png)

![](img/e568f5114baedeb794330e1391331bb2.png)

安装完成后，记得启动并设置开机自启：

```
# 启动 Redis 服务
sudo systemctl start redis

# (推荐) 设置开机自启
sudo systemctl enable redis

# (可选) 检查服务状态，确保 Active: active (running)
sudo systemctl status redis
```

项目说明提到需要 FFmpeg 来处理语音。请运行以下命令来安装它：

```
sudo yum install ffmpeg -y
```