
# 如何利用 AI 帮助孩子爱上查字典（一）

> 来源：[https://agijuejin.feishu.cn/docx/IVGJdqMTHovy5xxw6Q0c3z1NnGf](https://agijuejin.feishu.cn/docx/IVGJdqMTHovy5xxw6Q0c3z1NnGf)

> 作者：景淮Maple | 景淮AI探索之旅

> 原文： [https://mp.weixin.](https://mp.weixin.qq.com/s/zJ1dinx9Xa-_I-JpDeJwvg)qq.com/s/zJ1dinx9...

嘿，大家好呀，我是景淮，一个在加拿大的朋友，每天陪你一起玩转 AI。

今天终于腾出时间，更新了一下之前的一个 GPTs ： 儿童汉语词典

具体内容可以参考下下面的文章

GPTs Action 实战：儿童汉语词典

因为今年 3，4 月份的时候 AirCode 的服务器停止运行了，所以这个 GPTs 也就同时停止使用啦~

当时把 API 架在 AirCode 的服务器上了。今天终于抽出空，自己写了 API 接口还有弄了下服务器，把需要使用的 API 放在了对应的我自己的服务器上。

所以也就借着这个机会把，这个部署的过程和代码记录和分享一下。

本文会根据以下内容顺序进行：

+ ChatGPT GPTs 使用链接和一些问题
+ 扣子搭建逻辑图
+ 使用扣子重新搭建
+ 总结

## 一、ChatGPT GPTs 使用链接

因为这篇之前写过了，关于 GPTs 的搭建内容我就不再重述了，大家感兴趣的可以直接查看下之前的内容。

我把我重新配置的 Action 和 提示词重新分享一下，大家可以看看参考下。

### 一）Action

```
openapi: 3.1.0
info:
  title: Chinese Characters API
  description: API for querying Chinese characters, their stroke orders, and generating stroke order images.
  version: 1.0.5
servers:
  - url: https://jinghuaiapi.fun
    description: Main API server
paths:
  /characters:
    get:
      operationId: queryChineseCharacters
      summary: Query Chinese characters
      description: Retrieve information about Chinese characters based on criteria like pinyin, strokes, radical, or combined radical and strokes.
      parameters:
        - in: query
          name: pinyin
          schema:
            type: string
            description: Pinyin of the character, optionally with tone number.
          required: false
        - in: query
          name: strokes
          schema:
            type: integer
            description: Number of strokes excluding the radical.
          required: false
        - in: query
          name: radical
          schema:
            type: string
            description: The radical of the character.
          required: false
        - in: query
          name: rs
          schema:
            type: string
            description: Combined radical and strokes, formatted as &#x27;radical,strokes&#x27;. For example, &#x27;60,4&#x27; where &#x27;60&#x27; is the radical and &#x27;4&#x27; is the stroke count.
          required: false
      responses:
        &quot;200&quot;:
          description: Successful response with character data
          content:
            application/json:
              schema:
                type: array
                items:
                  type: string
                description: List of characters or character details
        &quot;400&quot;:
          description: Bad request due to invalid query parameters
        &quot;500&quot;:
          description: Internal server error

  /character/{char}:
    get:
      operationId: getStrokeOrder
      summary: Get stroke order of a Chinese character
      description: Retrieve the stroke order details for a specific Chinese character.
      parameters:
        - in: path
          name: char
          schema:
            type: string
          required: true
          description: The Chinese character for which to retrieve stroke order information.
      responses:
        &quot;200&quot;:
          description: Successful response with stroke order data
          content:
            application/json:
              schema:
                type: array
                items:
                  type: object
                  properties:
                    shape:
                      type: string
                      description: The shape of the stroke.
                    type:
                      type: string
                      description: The type of stroke.
                    foldCount:
                      type: string
                      description: The number of folds in the stroke.
                    name:
                      type: string
                      description: The name of the stroke.
                    letter:
                      type: string
                      description: The letter associated with the stroke.
                description: List of stroke order details for the character
        &quot;500&quot;:
          description: Internal server error

  /generate-png:
    post:
      operationId: generatePngImages
      summary: Generate stroke order PNG images for a Chinese character
      description: Generate and return PNG images showing the stroke order for a given Chinese character.
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                text:
                  type: string
                  description: The Chinese character for which to generate stroke order images.
              required:
                - text
      responses:
        &quot;200&quot;:
          description: Successful response with URLs of generated PNG images
          content:
            application/json:
              schema:
                type: array
                items:
                  type: object
                  properties:
                    pngUrl:
                      type: string
                      description: URL of the generated PNG image.
        &quot;400&quot;:
          description: Bad request due to missing or invalid &#x27;text&#x27; parameter
        &quot;500&quot;:
          description: Internal server error
```

服务器是用的我自己的，API 中转也是重新部署的，大家要是感兴趣可以试试哈。

对应的 `api` 参数我也放在下面了。

```
1. 查询汉字
接口
GET /characters
描述
根据拼音、笔画数或部首查询汉字。
查询参数
- pinyin: (字符串) 汉字的拼音。
- strokes: (数字) 汉字的笔画数。
- radical: (字符串) 汉字的部首。
- rs: (字符串) 部首和笔画数的组合，格式为 部首,笔画数。
示例
GET /characters?pinyin=ma
GET /characters?pinyin=ma3
GET /characters?strokes=16
GET /characters?radical=6
GET  /characters?rs=60,3

2. 获取汉字的笔画顺序
接口
GET /character/:char
描述
获取指定汉字的笔画顺序详情。
路径参数
- char: (字符串) 需要查询笔画顺序的汉字。
示例
GET /character/汉

3. 生成汉字笔画的PNG图片
接口
POST /generate-png
描述
生成指定汉字的每个笔画的PNG图片。
请求体
- text: (字符串, 必填) 需要生成PNG图片的汉字。
注意: 请求体应为JSON格式。
示例请求
POST /generate-png
Content-Type: application/json

{
    &quot;text&quot;: &quot;汉&quot;
}
```

### 二）提示词

```
# Workflows
1. 查字典：获取用户想要使用的查字法（部首查字法、笔画查字法、拼音查字法）
    101. 当用户选择“部首查字法”时，执行以下步骤：
        1011. 用户输入，部首的笔画数
        1012. 根据用户输入的笔画数，从[简化部首笔画.txt]中找到对应的部首，以Markdown格式的列表形式（列为：数字编号和部首）展示给用户，等待用户选择对应的部首。
        1013. 根据用户选择的部首，从[部首对照.txt]中进行搜索，并提醒用户输入去除部首后剩余的笔画数
        - 先从简化部首列，找到用户所选择的部首
        - 然后根据同一行的原则找到对应的康熙部首，并获取其康熙部首的编号。
        1014. 然后利用 Action[jinghuaiapi.fun]中的getCharacterByRadicalAndStrokes使用内置两个参数：a为部首编号，b为剩余笔画数的方式进行搜索,然后把返回的结果依次展示给用户，并对其进行标号。
            参数示例：
            ```json

            {

            &quot;a&quot;: 61,

            &quot;b&quot;: 10

            }

            ```
    102. 当用户选择“拼音查字法”时，执行以下步骤：
        1021. 提醒用户输入想要查的字的拼音（如果未知声调，可不进行输入，如：“peng”；已知声调可以以拼音加数字的形式发送给我，如“peng2”）
        1021. 然后利用 Action[jinghuaiapi.fun]使用参数&quot;pinyin&quot;进行搜索,然后把返回的结果依次展示给用户，并对其进行标号。
    103. 当用户选择“笔画查字法”时，执行以下步骤：
        1031. 提醒用户输入想要查的字的总笔画数
        1032. 然后利用 Action[jinghuaiapi.fun]使用参数&quot;strokes&quot;进行搜索,然后把返回的结果依次展示给用户，并对其进行标号。
2. 讲解汉字：引导用户，从上述结果中选择他想要学习和查询的字。
    201. 当用户给出他的选择之后，开始&quot;讲故事&quot;，根据所讲的故事画出其中的图片
        2011. 讲故事：以用户所选的[字]为主题，从你的知识库中获取有关这个字的结构的&quot;起源与发展&quot;相关的内容，参考[示例]的方法写一段有趣生动的文字介绍。
           - 示例：晚饭吃完了没有事情，在门里坐着，看看月亮。门缝里头，月亮照进来，悠哉游哉，这个当然是很清閒的閒。
        2012. 调用 Dalle-3 来完成绘画，以这个字的结构为主题，画一幅儿童插画风格的图片。让孩子更容易理解故事中的内容，画图的时候不要出现任何字样。
        2013. 字的变化过程：利用Python完成一个展示字的变化的图片(首先确定是否为繁体字汉字，如果为繁体先转换成对应的简体字，再执行python的流程)
            - 按照HYChenTiJiaGuWen-2.ttf、书体坊金文大篆体.ttf、方正小篆体.ttf、FangZhengShengShiKaiShuJianTi-1.ttf 的顺序，然后使用Python绘制图片，依次使用上面四种字体写一个字字。 四个字要放在同一行，字体大小65px 同时每个字之间流出50px的空隙。整个字当做一个字组，整个组保证top，bottom，left，right 有60px的空隙间距，依次分别在每个字下方用20px的字体依次表上“甲骨文”“金文”“小篆”“楷书”（不用带“”，同时小字都是用FangZhengShengShiKanTi-1.ttf 来书写），最后利用show（）方法展示给我 
            -参考代码：
            ```python

            from PIL import Image, ImageDraw, ImageFont

            import os

            # File paths for the fonts
```
