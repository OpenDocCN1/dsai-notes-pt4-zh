# Sora 2：国庆朋友圈刷屏的“AI 视频神器”与实战工作流教程

> 来源：[https://xuqiwei1986.feishu.cn/docx/W65LdXYeOoGZlExnYAVcv9zfnAb](https://xuqiwei1986.feishu.cn/docx/W65LdXYeOoGZlExnYAVcv9zfnAb)

国庆**第一天，你有**没有被朋友圈里铺天盖地的“AI 视频”刷屏？有人在火星旅游，有人在轮船上遭遇风暴，还有人在宇宙飞船里举着红旗……这些看似“烧钱拍大片”的效果，其实大部分都来自 OpenAI 最新发布的 Sora 2。

**这不是 Photo**shop，而是下一代的 AI 视频生成工具。它的出现，让过去需要团队、摄制组、动辄数十万成本的视觉创作，变成了普通人动动手指就能完成的事情。

## 什么是 Sora 2？

Sora 2 是 OpenAI 刚推出的全新版本 AI 视频生成工具。它和之前的 AIGC 产品相比，突破在于：

1.  **音画同步**：不仅能生成画面，还能自动配上对白、环境音和背景音乐，直接就是完整视频。

1.  **物理真实**：动作、光影、镜头切换都符合现实规律，不会出现“人影分离”或“手脚乱动”的穿帮。

1.  **Cameo 功能**：你可以录制自己的形象和声音，授权后，AI 就能把你变成“数字演员”，让你出现在任何视频里。

1.  **Remix 功能**：把热门视频的风格、节奏、镜头参数提取出来，再替换成新角色或新场景，轻松做出差异化作品。

1.  **资产库**：视频不再是一锤子买卖，可以沉淀成可调用的“视觉基因库”，以后随时复用。

**一句话总结：过去**拍摄 + 剪辑 + 后期的繁琐流程，现在只要一条指令，90 秒就能出片。

**碎碎念：中文支持不是太好**

## 为什么它会在国庆刷屏？

首先，节日自带流量，朋友圈和短视频平台都在找“视觉冲击 + 节日氛围”的内容。Sora 2 恰好满足了这种需求：

+   **快速产出**：不用约摄影师、不用租设备，10 秒设定一个场景，半小时就能做一批视频。

+   **全民参与**：哪怕是普通人，也能让自己“出现在天安门广场看升旗”，或者“在火星上旅游”。

+   **创意无限**：不同于模板化的滤镜，Sora 2 可以自由生成奇幻场景，让“假期大片”更有想象力。

于是我们就看到：朋友圈里有人在火星旅游，有人在外滩上空穿梭，还有人化身“航天员”，把国庆过成了大片。

## Sora 2 能带来什么？

1、对普通人

拍短视频再也不受场景、预算限制。以前想做古装大片、未来科幻片，只能看看就算，现在真的能“我就是主角”。

2、对品牌与团队

广告投放、节日营销，能以极低成本批量产出素材。比如一个国庆主题活动，可以快速生成几十条差异化视频，测试哪条点击率最高，再放大投放。

3、对教育与培训

老师可以生成课堂演示视频，培训机构能做低成本课程片头，甚至学生自己都能做实验模拟。

4、对内容平台

视频生产的门槛降低后，分发逻辑会改变。平台不仅仅是承载作品的地方，更变成协作和再创作的“编辑器”。

## 如何体验 Sora 2？

+   **开放形式**：目前支持 iOS 与网页版(https://**sora.c****h**atgpt.com/)，需要邀请码，另外可以通过https://api.aicso.top/ 接口平台生成，只要0.0495元一个视频，生成时间2-5分钟一个，本篇文章将会使用该接口进行Sora2视频生成。

+   **Pro 用户**：可体验更高质量的视频效果。

+   **安全机制**：Cameo 的授权可随时撤销，用户可以追踪和管理包含自己形象的视频。

## 实战：用 Coze 工作流 + 飞书多维表格，一键生成 Sora 2 视频（约 ¥0.0495/条，2–5 分钟/条）

> 目标：在飞书多维表格里输入 提示词/参考图，使用「扣子工作流调用」字段捷径插件调用 Coze 工作流，通过 api.aicso.top 接口生成 Sora 2 视频，并把请求结果与视频链接回写到表格。

### 架构与流程

+   **入口**：飞书多维表格➡️记录一行任务（提示词、参考图、工作流结果、视频预览等）➡️使用「扣子工作流调用」字段捷径插件调用工作流

+   **编排**：Coze 工作流➡️接收表格参数➡️调用 HTTP 请求节点到 api.aicso.top 创建视频任务➡️获取任务结果➡️返回视频 URL

+   **回写**：「扣子工作流调用」字段捷径插件➡️调用工作流获取该行记录提示词生成的视频地址

### 前置准备

1.  Coze 账号，开通工作流与插件能力。

1.  飞书账号，能创建与编辑多维表格，使用自动化与按钮字段。

1.  api.aicso.top 的 API Key。

### Coze 工作流流程图

![](img/092a4b46730f12faa8db8dd6913b3a2c.png)

### Coze 工作流步骤

1、创建工作流：

**工作流名称：**sora_video_generate

**工作流描述：**sora_video_generate，sora视频生成

![](img/120e51bacc656b93bc960f05aacdf8dd.png)

2、开始节点设置如下：

image_url：图片地址，对应飞书多维表格参考图

prompt：提示词，对应飞书多维表格提示词

api_key：**api_key，https**://api.aicso.top平台的令牌，记得创建时选择「限时特价」分组

api_url：API接口平台地址，默认：https://api.aicso.top/v1/chat/completions

model：模型，设置为sora-2，还可以选择sora-2-hd、sora-2-landscape-hd、sora-2-portrait-hd

![](img/cfb1f90c529c00cfbc301655ce978d30.png)

模型：

![](img/6fc59a70c6910a082331f886529358aa.png)

有-hd的是720p的视频，视频清晰度会高些。

令牌创建(https://api.aicso.top/console/token)：

![](img/339d449e491e5965bbc366d5e213b884.png)

3、设置「设置令牌」文本处理节点，String1引用开始节点的api_key：

```
Bearer {{String1}}
```

![](img/fc143f218fd8b32966a898bc07371af2.png)

4、「HTTP 请求」节点配置：

请求头：

Authorization：引用「设置令牌」节点的output

请求体：

```
**{
  &quot;model&quot;: &quot;**sora-2&quot;,
  &quot;messages&quot;: [
    {
      &quot;role&quot;: &quot;user&quot;,
      &quot;content&quot;: [
        {
          &quot;type&quot;: &quot;text&quot;,
          &quot;text&quot;: &quot;{{修改为开始节点的prompt}}&quot;
        },
        {
          &quot;type&quot;: &quot;image_url&quot;,
          &quot;image_url&quot;: {
            &quot;url&quot;: &quot;{{修改为开始节点的image_url}}&quot;
          }
        }
      ]
    }
  ]
}
```

超时设置：600秒，因为这个模型生成视频比较久，需要设置为600秒超时

![](img/40537b1aa4988575666941aa593d0cd2.png)

5、「代码_提取视频地址」代码节点配置：

输入：

body：引用「HTTP 请求」节点的body

输出：

content：String类型

url：String类型

error_info：String类型

![](img/06bbfcb7cb57a7d056e8a7969c1defde.png)

**代码(Python代码)：**

```
import json
import re
async def main(args: Args) -&gt; Output:
    params = args.params
    body = params[&quot;body&quot;]
    body_json = json.loads(body)
    content = &quot;&quot;
    url = &quot;&quot;
    error_info = &quot;&quot;
    try:
        content = body_json[&quot;choices&quot;][0][&quot;message&quot;][&quot;content&quot;]
        rawstr = &quot;\[.*?\]\((https://.*?)\)&quot;
        result = re.findall(rawstr,content)
        if len(result):
            url = result[0]
    except Exception as e:
        error_info = str(e)
    # 构建输出对象
    ret: Output = {
        &quot;content&quot;: content,
        &quot;url&quot;: url,
        &quot;error_info&quot;:error_info
    }
    return ret
```

6、结束节点输出：

![](img/bf10accb86d4630b585037207b7314fc.png)

7、试运行，试运行时需要把开始节点的「image_url」更改为File-Image类型：

![](img/0a51fe667604bed84ba1b7e8666f7f1c.png)

![](img/8f092a99f77166afa0e9dd37be769e24.png)

![](img/69a6d86c6fdf727779d5bfad9b118abe.png)

![](img/81d4d2acf83cce4c8bc795e1ecac2d6d.png)

[08fb0404-e417-4cdc-9a82-959bc91535fd.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/VpVVbGtUIo5XBrxQjKucWyjSn6K)

挺久的，4分多钟才生成视频，不过结果还不错，就是有水印不太好。

8、把开始节点的「image_url」修改为String类型，，然后发布工作流：

![](img/67ad6598946f6a1381ffbdfc22babf8f.png)

### 飞书多维表格配置步骤

1、创建一个飞书多维表格，删除不必要的列，然后第一列设置为提示词，第二列附件列设置为参考图，第三列添加「扣子工作流调用」字段捷径插件：

请求模板：

```
{
    &quot;api_key&quot;**: &quot;修改**为自己api.aicso.top的令牌&quot;,
    &quot;api_url&quot;: &quot;https://api.aicso.top/v1/chat/completions&quot;,
    &quot;image_url&quot;: &quot;$$image_url$$&quot;,
    &quot;model&quot;: &quot;sora-2&quot;,
    &quot;prompt&quot;: &quot;引用提示词&quot;
}
```

![](img/8b4f882ed57bde23724d4ca78babd8c2.png)

![](img/b5c41e32eb8e5b608b2596ea6c66bf22.png)

![](img/1c32d64c3bbc69c1593402bb55dfe5bc.png)

配置好插件后点击「确定」。

2、在弹出窗口点击「仅保存配置」：

![](img/e0fd557d62401a175b9e91fc1500b09e.png)

3、设置提示词和上传参考图：

![](img/e6b834ccad599c04988ff994b2f24c97.png)

4、增加「视频预览」字段，使用「链接转换附件」字段捷径插件把视频链接转成附件：

![](img/023cc61666a7c2e05481c55c01fc6274.png)

![](img/eda653008b3aa4c9206dcca5829a26de.png)

![](img/f32c1e7d0aef40188366c537a0cda863.png)

[20251002_013602.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/BkdVbHut8oHq1xxXJLGcW5gbnhy)

5、可以使用AI生成剧本和分镜，然后用即梦生成图片：

![](img/2e97983e740a0ea64996a768a1a685dc.png)

![](img/76ad670e81c0e96c9fc0573f25559844.png)

![](img/211df3995fdb8df8ddf9bd7ae4876e54.png)

![](img/5b99b2bbbf684897200cba820454fc5d.png)

![](img/0d4a540025353999211a45f8263593d7.png)

![](img/4f3a78e434dc27f6466bd1e4a1aed545.png)

6、使用多维表格调用工作流生成不同分镜的视频：

![](img/db060d932e4d678d6f750d929adf405a.png)

生成的视频最后用剪映合并即可，这样子就可以制作完一个视频了。

**Tips：**

1、剧本和分镜的提示词需要优化，我这里只是用Kimi随便生成的分镜提示词

2、即梦生成图片需要调整提示词和使用参考图，保持前后的人物一致性。

3、可以用来复刻ytb爆款视频，也可以用来生成漫画短剧

4、Sora现在不可以用真人或者比较真实的图片做参考图

5、使用-hd的模型生成的视频质量会好些

## 总结

Sora 2 的出现，正在重写“内容创作”的定义。它不仅仅是一个生成工具，更像是一套“创意操作系统”：让每个人都能低成本试错，用数据验证创意，把“拍内容”升级为“管资产”。

而通过 Coze 工作流 + 飞书多维表格的组合，Sora 2 已经不只是“炫酷玩具”，而是能落地到团队的生产流水线里：一键触发，2–5 分钟出片，低成本大规模生成。

未来的大片，真的可能就是普通人用 AI 在表格里点一下按钮，90 秒后就生成的短片。

你学会了吗？赶紧去搭建一个，解放你的双手！

👉 如果你在搭建过程中遇到任何问题，欢迎在评论区留言交流！

觉得有用，点个「赞」+「在看」，转发给小伙伴吧！