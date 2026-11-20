
# SDWebUI 爆显存&出图慢问题 副本

> 来源：[https://xatc0v8uz5m.feishu.cn/docx/JlHWd7pJDoSKMixdUdxc05h8nUh](https://xatc0v8uz5m.feishu.cn/docx/JlHWd7pJDoSKMixdUdxc05h8nUh)

## 全局说明

+ 步骤一：自查生成相关参数

请先查看参数设置，是否有高消耗显存的参数，例如图片分辨率、视频帧数等，可以适当降低图片分辨率或视频帧数等尝试。

+ 步骤二：自查显卡配置和参数匹配度

请先确认当前使用显卡配置，若是低配显卡带不动任务，可以尝试更换高配置显卡。

+ 步骤三：调整显存模式

若步骤一和步骤二均完成仍旧会爆显存，请参考如下说明进行显存模式调整尝试 👇👇👇

## 一、常用启动参数

SD的启动参数可以参考[官方github上的文档](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Command-Line-Arguments-and-Settings)。其中，与显存相关的参数如下：

### 终端执行命令

#### 1、打开终端

点击`jupyter`页面，打开【日志与终端】

以官方版SD举例，调至【`medvram**】? 

![](img/d85e9155b61c9be496dd4f8513f88552.png)

![](img/aba106e6c8809b6893389f4d6fd485ee.png)

#### 2、输入命令

在终端按需执行下面一条命令即可

`#sd` 设置为低显存并重启

```
test -f /usr/bin/stablediffusion-startup.env && ( sed -i -E 's/--(lowvram|medvram)//g ; s/^ARGS="/ARGS="--lowvram /g ; s/[ ]+/ /g' /usr/bin/stablediffusion-startup.env && supervisorctl restart stable-diffusion )
```

![](img/800f3f528263bf919a35e746b866ace3.png)

`#sd` 设置为高显存并重启

```
test -f /usr/bin/stablediffusion-startup.env && ( sed -i -E 's/--(lowvram|medvram)//g ; s/[ ]+/ /g' /usr/bin/stablediffusion-startup.env && supervisorctl restart stable-diffusion )
```

![](img/64f4f31dae878453ac9ae5735f8ab406.png)
