
# Ffmpeg - 多媒体处理工具

> 来源：[https://my.feishu.cn/docx/UtowdOuJpoMkrXxKz0GcnNxbn6z](https://my.feishu.cn/docx/UtowdOuJpoMkrXxKz0GcnNxbn6z)

宝塔界面左侧【终端】中直接输入。

```
sudo yum update
sudo yum install epel-release
sudo yum install ffmpeg ffmpeg-devel
```

如果无法下载的话，试试这段。

```
sudo curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
sudo yum clean all
sudo yum makecache
```

```
sudo yum update
sudo yum install -y ffmpeg ffmpeg-devel
```

![](img/64f0b3b6fa63a19982e7b1329672955f.png)

![](img/c4b6d0b308e6c0528476d4c9c4b2ada0.png)

![](img/296917f2bf6eaeabe7b49bd81ee61ca3.png)

再次显示[]，代表安装完成。

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>
