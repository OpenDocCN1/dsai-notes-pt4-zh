# Glif：图像流强大的多，使用和接入Coze

> 来源：[https://waytoagi.feishu.cn/docx/IouydWUw6oBAfYxdSFbc14cen7g](https://waytoagi.feishu.cn/docx/IouydWUw6oBAfYxdSFbc14cen7g)

> 作者: 彬子

> 玩梗 WOJAK MEME GENERATOR[ 最早发起是 Glif ](https://www.coze.cn/store/bot/7385098430101766144)的创[始者5分钟捏出来](https://www.coze.cn/store/bot/7385188509868605491)的 AI 小应用。扣子上也有了很火的版本，wojak_meme复刻, 万物皆可meme 。

去到 Glif 官网，他们管一个应用就叫一个glif， glif 搭建的便捷性和可玩性很棒，非常适合快速捏一个小应用，尤其是图像流所拥有的节点和能力极其方便去发挥创意。

## Glif 的使用

[接下来我以一个示例来介绍如何搭](https://glif.app/@Binllm/glifs/clxwncgy2000aybxuqtmvd4lx)一个给宠物起名的 glif: 【Pet Name Genius】

![](img/daa9e92535985b5fbc8e6bc5146a605d.png)

一，首先确认这个 glif 的入口参数，设计输入项为：

> 输入项1：宠物的性别

> 输入项2：语言

> 输入项3：用户提供一张宠物照片

![](img/b0ab13f8a05612092c4e06b4085641bc.png)

分别新增【宠物性别】Text Input，【起名语言】Text Input，【宠物照片】Image Input 三个节点。

![](img/f625238f00d85a33abaa7bb17b56be92.png)

二，接下来要将图片内容识别出来，这里有两种选择：

1.  使用Image to Text 节点，可以选择里面Blip2 和 MiniGPT4（很慢）。

1.  使用 GPT-Vision 的多模态能力，已经有封装好的 glif block（推荐）。

这里选择第二种方式，新增一个 glif block 节点。

![](img/fb8b4701b5233f0aec7782843e773801.png)

1.  Select Glif 这里选择里面的 GPT Vision。

1.  在 textPrompt 里给出指令：

> Describe the all characteristics of the pet, e.g. detailed descriptions fur, color, ears,  eyes, facial features, body features, posture

1.  imageUrl 填入上面图片输入项的变量。

![](img/3e679bb79146d0df50784ce4b82f1e79.png)

三，由于识别图的内容相对混乱，尤其宠物特征和环境的描述很可能是杂糅在一起，通过大模型做清理。

新增一个 Text Generator（LLM） 节点。

![](img/45aa2c0662be58b4a1de6affb7f85228.png)

这里的 User Prompt 参考：

> Extract descriptive content from the input that describes the characteristics of the pet itself, and external descriptions such as the environment

> Provide direct descriptions of pet type and characteristics separate with commas

> Clearly describe the body parts or organs corresponding to the characteristics

> No any preparation or explanation

> Input:{pet_characteristic}

> Output using JSON is as follows:

> {

> "pet":"xxx",

> "external":"xxx"

> }

![](img/1c69904b0d454d627b14a135c891dbc0.png)

点击 Advanced controls 展开面板，大模型选择 Claude-3.5-sonnet，能免费用最强大模型真香～

![](img/f4431eb79676411aba6956505a0ba371.png)

四，这里由于没有直接的Function Call能用，所以 JSON的解析需要单独的节点。

新增 JSON Extractor 节点。

![](img/9b6063f4cc62ccaaccd0fce170ca71c0.png)

1.  JSON 输入选择上面大模型的清理结果的变量。

1.  输出选择预设的字段，这里是 【pet】 和 【external】。

![](img/5ce45b9a5bf3b6dc23c0fe94fba70c3a.png)

五，以上的识图内容和用户输入项都已准备就绪，需要将文本内容进行合并。

新增 Text Combiner 节点。

![](img/9988a80d8bfd3298ed5444cc134d8ec0.png)

这里合并了清理出来的宠物特征和用户输入的宠物性别来作为上下文信息。

![](img/fd2ec01b778f2bcef720d0985aa825fb.png)

六，要给宠物起名了，希望给出6个备选项和对于的理由，新增一个 Text Generator （LLM）节点。

![](img/d065087e8745739e8c5adeed757f5008.png)

1.  User Prompt 参考：

> Now do JSON for the input: {combiner1}

> Deep think of six extremely creative names and use a fun tone to explain the reasons.

> IMPORTANT: The names should be concise and concise, e.g. Chinese names are two to three Chinese characters , or English names are within 5 English letters.

> Only output JSON, start with {

> Output using language of {name_language}

![](img/9b9bc734bd553419fd3760bbe55d12fe.png)

1.  System Prompt 参考：

> You are simulating an imaginative pet name generator that provides unique and creative names based on the pet's characteristics and gender. Your output should be in JSON format. Below is an example input for a pet with specific characteristics:

---

> Example

> Input:

> Characteristics of the pet: A little dog, full of energy, small in stature, with brown fur. It is girl

> Output：

> {

> "name1": "Elf",

> "reason1": "Considering she is a girl, full of energy, always jumping around, like a lovely little elf.",

> "name2": "Cocoa",

> "reason2": "Her brown fur makes people think of rich cocoa, with a sweet and fragrant feeling, making people want to get close.",

> "name3": "Lasso",

> "reason3": "A homophone of 'zipper', because she moves quickly and is full of energy, just like a zipper coming back and forth tirelessly.",

> "name4": "Peanut",

> "reason4": "Her small stature and brown color make people think of peanuts. Peanuts are also liked by everyone, and the name is also very loud when called.",

> "name5": "Twinkle",

> "reason5": "Her energetic nature makes her shine with excitement, so eye-catching in the group of dogs, everyone notices her at a glance.",

> "name6": "Sucoa",

> "reason6": "Her brown fur makes people think of cocoa, and her sweet personality makes her cling to the owner like a piece of soft candy."

> }

---

> Rules:

1.  Based on the pet's characteristics and gender, ensure the names are extremely creative names and use a fun tone to explain the reasons.

1.  Name using nickname phrase style, such as two characters in Chinese and one word in English

1.  The reason is creative and interesting, within 30 words.

1.  Model 选择 Claude-3.5-sonnet。

![](img/1cd8f6158c3175c7eb42dd8656587b15.png)

七，JSON 解析起名的结果内容，新增一个 JSON Extractor。

![](img/73fda8f39f43c5a1ca6941b25d73a5f7.png)

1.  JSON 输入项为大模型的起名结果变量

1.  JSON 输出项为 6个备选宠物名和理由，我这里考虑后面Canvas的使用，图省事直接平铺所有变量。

![](img/0a67c31b4459e0d8059221129f3b9579.png)

八，这里为了避免页面单调，给宠物做一个拟人化的画像。

Glif 里支持 SD XL， Dall-E之外，还支持 ComfyUI，作图这块简直是超大杯。

这些选择 glif block 里封装好的 IPAdapter 节点。

![](img/c63ca2ff6067705d768364899f3687ec.png)

1.  Select Glif 选择 SDXL IPAdapter

1.  Image-input 选择输入项的宠物图变量

1.  Prompt 举例用：

> anthropomorphic, it is {pet_gender}

![](img/70c45931f115f9f069bffe19575fc439.png)

九，最后就是将上面的文本内容和图像组装到一起，新增一个 Canvas 节点。

![](img/8412db7f6c6f876a7c0cafa1459067dc.png)

Open Canvas 去编辑。

![](img/c240e5dc6bfc57a9c22f537e67a20deb.png)

按照设计插入文本和图片组件，拖拽到合适的位置，缩放合适的大小。

![](img/f8075dc015801f0fd4b6f4a9a73ea750.png)

选中每个组件，关联 Source，就是上面那些节点的输出变量。

![](img/5a36c6ab002afb20800f4a59aa90e3da.png)

最后，在页面的右侧测试一下，符合预期就发布吧～

![](img/10af38410f82f802cf4877c4a8c71f2d.png)

## 接入 Coze

Glif 虽好，但我们更熟悉Coze，那就把Glif 的功能通[过 API ](https://www.coze.cn/store/bot/7385145130602479655)方式接过来，Coze版 【 Ai 宠昵名】

一，先要创建一个插件：

1.  填入插件名，插件描述。

1.  创建方式选择云侧插件。

1.  IDE 运行时选择 Python。

![](img/f82974251ac37332363aab87cefccd7c.png)

点击【在IDE中创建工具】。

![](img/0ebb63901527e2d50de790c762be80f9.png)

创建工具面板里：

1.  填入工具名称。

1.  填入工具介绍。

![](img/5e29ff80088cd084207907d079358bce.png)

Glif 是允许通过 API 调用平台上的应用，API接入：https://docs.glif.app/api/getting-started

![](img/3855adf9820a878865ffd04dcae96a5e.png)

去[注册 Token 页面](https://glif.app/settings/api-tokens)创建一个新的 Token并复制备用。

![](img/93371f7a3b4eb9b984f9b06c7d6c3c74.png)

将自己发布的 glif 链接上的id 复制备用。

![](img/9e3be85a94b3fe6fb8150f2a716ba7f9.png)

在 Coze 的IDE 中引入依赖和编写 handler 方法。

```
import requests
import json
```

```
def handler(args: Args[Input])-&gt;Output:
        gender = args.input.gender
        language = args.input.language
        photo = args.input.pet_photo
        inputs = [gender, language, photo]
        response = requests.post(
            &quot;https://simple-api.glif.app&quot;,
            json={&quot;id&quot;: &quot;你的 glif id&quot;, &quot;inputs&quot;: inputs},
            headers={&quot;Authorization&quot;: &quot;Bearer 你的 glif Token&quot;},
        )
        print(response.content)
        data = response.json()
        img_url = data[&#x27;output&#x27;]
        return {&quot;names_image_url&quot;: img_url}
```

![](img/87df42e647ef41f7632047cec267f57a.png)

代码中对应的数据字段在元数据 Tab中设置：

1.  填写入参字段和描述，对完整链接要写清楚。

1.  填写出参字段。

![](img/2976b6daf58f24685268f1685bf6b1c3.png)

左侧的依赖包添加 【requests】。

![](img/d1a3694ce649dbddbf0cdb773b247a74.png)

填入测试数据运行，测试通过后即可发布。

![](img/78ec73336253a073fbe919706ce8fc87.png)

创建一个 Bot ，挂载新创建的插件即可，在 Prompt 中做一些对入参的校验和约束。

![](img/40bb5308fc2e6a003d48ed179aa8e33a.png)

最后，测试从 Coze 调用 Glif 功能跑通，发布 Coze ，大功告成～

![](img/e4c83b02fee7a71ad7b12e8bee5c6318.png)

以上就是大致的思路了。

**通**过这种插件的方式取长补短，可以将 Glif 作为 Coze 工作流中的一环，有效弥补图像流较弱影响整体效果～

抛砖引玉，更多玩法大家多多探索～

欢迎找我交流 Glif 使用心得和创新用法

![](img/421c7d4310d83d692a023d38b5822322.png)