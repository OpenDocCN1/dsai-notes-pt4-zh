
# 不懂前端也没关系，trae 10分钟就能帮你写一个还不错的页面~

> 来源：[https://agijuejin.feishu.cn/docx/XPYydsqq8ohwShx0bQzc5GN7nJh](https://agijuejin.feishu.cn/docx/XPYydsqq8ohwShx0bQzc5GN7nJh)
> 作者：lybtt
> 链接：https://juejin.cn/post/7462733343765037119
> 来源：稀土掘金
> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
> 如有侵权，请联系删除。

## 活动声明

我正在参加Trae「超级体验官」创意实践征文，本文所使用的 Trae 免费下载链接：`www.trae.ai/?utm_source…`

## Trae 初体验

今天来试一试trae，从下图的配置导入界面不难看出，这个就是来对标cursor的。cursor最近几个月在圈内很火，优秀的产品特性和用户体验获得了很多程序员的认可，也是在不断的融资。面对这个趋势，大厂想要进来分一杯羹也很正常，这不，字节来了~

![](img/4a8f2644627a33925a0b651ad4707271.png)

设置和cursor一样，在右上角，不过图标不一样。目前这个可以设置的内容还是比较少的，模型只有Claude和GPT-4o选择，而且也不支持自己增加，不过毕竟还只是个测试版，一些高级的自定义功能没有放出来也很正常。

![](img/74e0559354a9cc6b22e874ca64ec8d83.png)

对话界面很常规，没什么好说的，这里和cursor不同的是，cursor引用是在对话框上面的+号，trae是对话框下面的#号。然后对话框有一个比较酷炫的动效，挺好看的。

![](img/ac029589810fda35d38ac63992e9e284.png)

## Builder 界面实践

OK，对话直接跳过吧，我们来到builder界面。快过年了，我就让他给我写一个简单的春联吧。这个地方的交互还是不错的，能点击直接运行命令。

![](img/a3d2ab80dd998fabe98066e650c64693.png)

下面是第一版生成的结果，用时能用，就是丑了点。

![](img/c9211377182fb214cbb6b96b4de8584c.png)

我是老板，我当然得pua这个trae啊，你这审美真的不行，必须得改，我要五彩斑斓的黑~

![](img/7f4d87a5984e2cfcff7c84d7da61951f.png)

改了几版之后，我受不了了，把截图发给他，都没有对齐，你这前端不合格呀，你倒底有没有看我的设计稿（trae：你有个屁的设计稿）！

![](img/e4088235f9eb9056299ef7f05d8f2d60.png)

几分钟过去了，又改了几版，最终效果完成，就这样吧，我妥协了，又不是不能用，可能还是得结合别的设计来搞，审美这一块还是得human来~

![](img/98e06a107c19fbe1360a84e07059f0e3.png)

## 总结

不过不得不说，和cursor对比，我只能说大厂还是大厂，字节还是字节，审美是强很多，无论是对话框的动态效果还是整体UI/UX设计，都彰显出了高水平的艺术感和技术实力，可能人多力量大吧，如果trae价格合适的话，为cursor感到担忧。下面cursor的界面就感觉差了很多。不过这个是一个比较简单的需求，还比较顺利，还需要更深入的测试，看看对于一个完整的大项目来说，trae能否胜任？

![](img/e78ee671f8bdabb95c39169b4144b83d.png)

下面附带生成的代码：

```python
from jinja2 import Template
import webbrowser
import os

# HTML模板
html_template = &quot;&quot;&quot;
&lt;!DOCTYPE html&gt;
&lt;html lang=&quot;zh&quot;&gt;
&lt;head&gt;
    &lt;meta charset=&quot;UTF-8&quot;&gt;
    &lt;title&gt;新年春联&lt;/title&gt;
    &lt;style&gt;
        body {
            background-color: #CC0000;
            background: linear-gradient(135deg, #CC0000 0%, #8B0000 100%);
            position: relative;
            overflow: hidden;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            margin: 0;
            padding: 20px;
        }
        body::before {
            content: &#x27;福&#x27;;
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%) rotate(180deg);
            font-size: 400px;
            color: rgba(255, 215, 0, 0.1);
            font-family: &#x27;STKaiti&#x27;, &#x27;KaiTi&#x27;, &#x27;楷体&#x27;, sans-serif;
            pointer-events: none;
            z-index: 0;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
            font-family: &#x27;STKaiti&#x27;, &#x27;KaiTi&#x27;, &#x27;楷体&#x27;, sans-serif;
        }
        .couplet-container {
            display: flex;
            justify-content: center;
            gap: 200px;
            width: 100%;
            max-width: 1200px;
            margin-top: 20px;
        }
        .horizontal {
            color: #FFD700;
            background-color: #8B0000;
            padding: 20px 80px;
            font-size: 64px;
            margin-bottom: 40px;
            border-radius: 15px;
            border: 3px solid #FFD700;
            box-shadow: 0 4px 15px rgba(255, 215, 0, 0.3);
            animation: fadeInDown 1.5s, glow 2s infinite;
            text-align: center;
        }
        .vertical {
            color: #FFD700;
            background-color: #8B0000;
            padding: 60px 40px;
            font-size: 48px;
            writing-mode: vertical-rl;
            height: 600px;
            border-radius: 15px;
            border: 3px solid #FFD700;
            box-shadow: 0 4px 15px rgba(255, 215, 0, 0.3);
            position: relative;
            overflow: hidden;
            display: flex;
            align-items: center;
            justify-content: center;
            margin: 20px 0;
        }
        @media screen and (max-width: 768px) {
            .couplet-container {
                gap: 80px;
            }
            .horizontal {
                font-size: 48px;
                padding: 15px 60px;
            }
            .vertical {
                height: 400px;
                font-size: 36px;
                padding: 40px 30px;
            }
        }
        @media screen and (max-width: 480px) {
            .couplet-container {
                gap: 40px;
            }
            .horizontal {
                font-size: 36px;
                padding: 10px 40px;
            }
            .vertical {
                height: 300px;
                font-size: 24px;
                padding: 30px 20px;
            }
        }
        .left {
            animation: fadeInLeft 1.5s;
        }
        .right {
            animation: fadeInRight 1.5s;
        }
        @keyframes fadeInDown {
            from {
                opacity: 0;
                transform: translateY(-50px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }
        @keyframes fadeInLeft {
            from {
                opacity: 0;
                transform: translateX(-50px);
            }
            to {
                opacity: 1;
                transform: translateX(0);
            }
        }
        @keyframes fadeInRight {
            from {
                opacity: 0;
                transform: translateX(50px);
            }
            to {
                opacity: 1;
                transform: translateX(0);
            }
        }
        @keyframes glow {
            0% { box-shadow: 0 0 15px rgba(255, 215, 0, 0.3); }
            50% { box-shadow: 0 0 30px rgba(255, 215, 0, 0.6); }
            100% { box-shadow: 0 0 15px rgba(255, 215, 0, 0.3); }
        }
        .vertical::before {
            content: &#x27;&#x27;;
            position: absolute;
            top: -150%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: linear-gradient(transparent, rgba(255, 215, 0, 0.2), transparent);
            transform: rotate(45deg);
            animation: shine 3s infinite;
        }
        @keyframes shine {
            0% { transform: rotate(45deg) translateY(0); }
            100% { transform: rotate(45deg) translateY(100%); }
        }
    &lt;/style&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;div class=&quot;horizontal&quot;&gt;{{ horizontal }}&lt;/div&gt;
    &lt;div class=&quot;couplet-container&quot;&gt;
        &lt;div class=&quot;vertical left&quot;&gt;{{ left_couplet }}&lt;/div&gt;
        &lt;div class=&quot;vertical right&quot;&gt;{{ right_couplet }}&lt;/div&gt;
    &lt;/div&gt;
&lt;/body&gt;
&lt;/html&gt;
&quot;&quot;&quot;

# 春联内容
horizontal = &quot;金蛇献瑞&quot;
left_couplet = &quot;龙腾虎跃新春至&quot;
right_couplet = &quot;蛇舞凤翔瑞气来&quot;

# 生成HTML文件
template = Template(html_template)
html_content = template.render(
    horizontal=horizontal,
    left_couplet=left_couplet,
    right_couplet=right_couplet
)

# 保存HTML文件
output_file = &#x27;spring_couplet.html&#x27;
with open(output_file, &#x27;w&#x27;, encoding=&#x27;utf-8&#x27;) as f:
    f.write(html_content)

# 在浏览器中打开HTML文件
file_path = &#x27;file://&#x27; + os.path.abspath(output_file)
webbrowser.open(file_path)
```
