
![](img/f1953c24e8e3b53427e56d65eba798d3.png)

推理速度则能达到14tokens/s。

## 六、Qwen3 接入OpenWeb-UI

### 1. Open-WebUI部署流程

首先需要安装Open-WebUI，官网地址如下：`https://github.com/open-webui/open-webui`。

![](img/ed8521da59a8cee87e961f41fe5ed6f4.png)

我们可以直接使用`pip`命令快速完成安装：

```
    pip install open-webui
```

![](img/425d211a74ef41da053b0e6c434a0c39.png)

可以直接使用在GitHub项目主页上直接下载完整代码包，并上传至服务器解压缩运行：

![](img/501a8d75736d1aee6731d02b48945d17.png)

此外，也可以在课件网盘中领取完整代码包，并上传至服务器解压缩运行：

![](img/4fec41b524b0954e19df40943da7e608.png)

![](img/05c7aa98a7b3aa52daf8a9220388140e.png)

在确保`ollama`正常运行的情况下，进行后续操作。

### 2. Open-WebUI启动与对话流程

在准备好了Open-WebUI和一系列模型权重后，接下来我们尝试启动Open-WebUI，并借助本地模型进行问答。

首先需要设置离线环境，避免Open-WebUI启动时自动进行模型下载：

```
    export HF_HUB_OFFLINE=1
```

然后启动Open-WebUI

```
    open-webui serve
```

![](img/14c7c97f3736ff778d4b464b9151899b.png)

需要注意的是，如果启动的时候仍然报错显示无法下载模型，是Open-WebUI试图从huggingface上下载embedding模型，之后我们会手动将其切换为本地运行的Embedding模型。

![](img/982192cc6e017808364ec436e6262956.png)

然后在本地浏览器输入地址:8080端口即可访问：

![](img/0d8a05dc1ada9c93ef450f95ddefaf4e.png)

> 若使用AutoDL，则需要使用SSH隧道工具进行地址代理

![](img/838651c44899bd70f8b4e8f85331fe80.png)

> 更多AutoDL相关操作详见公开课：《AutoDL快速入门与GPU租赁使用指南》|https://www.bilibili.com/video/BV1bxB7YYEST/

![](img/11bb43b12cc0644d57cf9833d38bdf86.png)

然后首次使用前，需要创建管理员账号：

![](img/f59cd0d92764ecb55f28930c715d4bbf.png)

然后点击登录即可。需要注意的是，此时Open-WebUI会自动检测后台是否启动了`ollama`服务，并列举当前可用的模型。稍等片刻，即可进入到如下页面：

![](img/3876f89f7e8559529600666d29d0ddb0.png)

接下来即可进入到对话页面：

![](img/28174d94eb4471500bdb02d1091c182d.png)

对话效果如下所示：

![](img/0dcd6150b786a4c92f4188b0a4bd7e2e.png)

### 3. 本地知识库检索

![](img/a8e91edd44301cb29b25cdd7d0c22cb4.png)

![](img/fe298b9b71dac2889810c45ed505abed.png)

![](img/7b1f9daa5d7c79524bef773abfafb951.png)

### 4. 代码解释器

![](img/b19227b61573d1618eebc79608078d1f.png)

![](img/7c94a7ad6358f2899d59058d13cf520f.png)

### 5. 调用外部工具

Qwen3全系列支持Function calling，因此我们可以基于此完成Open-WebUI的外部工具调用工作。

+   Open-WebUI工具调用功能实现

![](img/81e285c14210fe04a4f5aa9026d0319d.png)

然后编写天气查询函数：

```
    import requests
    import json
    from fastapi import Request
    from open_webui.models.users import Users

    class Tools:
        def __init__(self):
            pass

        async def get_weather(
            self, loc: str, __request__: Request, __user__: dict, __event_emitter__=None
        ) -&gt; str:
            &quot;&quot;&quot;
            获取指定城市的即时天气信息。

            :param loc: 城市名称（如果是非英文城市名称，请先将其翻译为英文城市名称再输入）。
            :param __request__: HTTP 请求对象（来自 FastAPI）。
            :param __user__: 用户信息（可以用于个性化或记录请求）。
            :param __event_emitter__: 事件发射器，用于将实时状态更新发送到前端。
            :return: 格式化后的天气数据，作为字符串返回。
            
            说明：
            如果输入的是非英文城市名称，请先将其翻译为对应的英文城市名称再输入。
            比如，输入“中国，北京”，需要转为 &quot;Beijing&quot;。
            &quot;&quot;&quot;

            # Step 1. 通知用户正在获取天气数据
            await __event_emitter__(
                {
                    &quot;type&quot;: &quot;status&quot;,
                    &quot;data&quot;: {&quot;description&quot;: f&quot;正在获取 {loc} 的天气数据...&quot;, &quot;done&quot;: False},
                }
            )

            try:
                # Step 2. 构建请求的 URL 和查询参数
                url = &quot;https://api.openweathermap.org/data/2.5/weather&quot;
                params = {
                    &quot;q&quot;: loc,
                    &quot;appid&quot;: &quot;YOUR_API_KEY&quot;,  # 你的 API 密钥
                    &quot;units&quot;: &quot;metric&quot;,  # 使用摄氏度
                    &quot;lang&quot;: &quot;zh_cn&quot;,  # 输出简体中文
                }

                # Step 3. 向 OpenWeather API 发送 GET 请求
                response = requests.get(url, params=params)

                # Step 4. 解析响应数据
                data = response.json()

                # Step 5. 提取并格式化天气描述
                weather_data = data.get(&quot;weather&quot;, [])
                if weather_data:
                    main_weather = weather_data[0].get(&quot;main&quot;, &quot;&quot;)
                    description = weather_data[0].get(&quot;description&quot;, &quot;&quot;)
                    weather_info = f&quot;当前天气：{main_weather} - {description}&quot;
                else:
                    weather_info = &quot;天气信息不可用。&quot;

                # Step 6. 通知用户天气数据已经成功获取
                await __event_emitter__(
                    {
                        &quot;type&quot;: &quot;status&quot;,
                        &quot;data&quot;: {&quot;description&quot;: f&quot;成功获取 {loc} 的天气数据&quot;, &quot;done&quot;: True},
                    }
                )

                # Step 7. 返回格式化后的天气信息并发送到前端
                await __event_emitter__(
                    {
                        &quot;type&quot;: &quot;message&quot;,
                        &quot;data&quot;: {&quot;content&quot;: f&quot;{loc} 的天气信息: {weather_info}&quot;},
                    }
                )

                # 返回格式化后的天气数据作为字符串
                return f&quot;{loc} 的天气信息: {weather_info}&quot;;

            except Exception as e:
                # Step 8. 如果发生错误，通知用户
                await __event_emitter__(
                    {
                        &quot;type&quot;: &quot;status&quot;,
                        &quot;data&quot;: {&quot;description&quot;: f&quot;发生错误: {str(e)}&quot;, &quot;done&quot;: True},
                    }
                )

                # 返回错误信息
                return f&quot;获取天气数据时发生错误：{str(e)}&quot;;
```

并将函数进行写入：

![](img/7e6ecb7bcc75f20fd50ee4821dcf56a4.png)

完成后即可看到新的工具：

![](img/0e6378ef45fea024247e8dc88277eca7.png)

在对话时可以开启天气查询函数：

![](img/5ef44530dd03e90169a7ad05e9670b85.png)

![](img/75d228b328be36f003767a35cccda764.png)

至此，我们就详细介绍了QwQ模型的各类本地部署与调用方法。
