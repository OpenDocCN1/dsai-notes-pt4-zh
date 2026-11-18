
+   `F1_Niji`：漫梦琉璃萧瑟模型，类似`MJ`中`Niji`的质感
+   `F1_Realism`：绘声绘色模型，适合生成写实风格图像
+   `F1_DRealistic`：真彩进阶v1.7，高颜值亚洲人脸模型
+   `F1_Architecture`：建筑设计模型，专门用于建筑场景
+   `F1_illustration`：次元插画模型，适合生成动漫风格图像
+   `F1_3Dcartoon`：3D卡通模型，适合生成3D卡通风格图像
+   `F1_IPdesign`：IP设计模型，适合生成IP三视图
+   `F1_e-commerce`：电商场景模型，适合电商产品展示
+   `F1_Pixelwave`：`FluxDev`的微调模型，艺术风格表现增强
+   预设`SDXL`绘画底模（可自行添加）：
+   `XL_新世界V1`：泛化能力较强，`SDXL`生图模式默认底模
+   `XL_筑梦工业V8`：筑梦工业V8凛冬奇迹
+   `XL_筑梦动漫V5`：筑梦动漫V5星辰之愿
+   `XL_3D国潮V1`：深度结合国潮元素，具备多样化生成能力
+   `XL_一青十色V4`：绪儿摄影写真模型，脸型触发词`chinese girl`
+   `XL_红尘白梦V3`：绪儿动漫人物模型，无需`lora`直出各种动漫角色
+   `XL_高清人像`：泛化较强的写实风格人像摄影模型
+   `XL_老王建筑`：老王建筑室内景观规划大模型
+   `XL_AWPortrait`：老王人像摄影专用模型
+   更多自定义底模和`Lora`的查找和添加使用方式：

1）在`Lib官网`检索可商用的`Checkpoint`和`LoRA`模型

![](img/96b4a80943450884b39dc6870ee9febe.png)

![](img/600fd546f98fa7bddd801a2e764fbd5d.png)

2）选择喜欢的模型版本，从浏览器网址中复制`versionUuid`

![](img/3e0c3161ee60a7358b839f549d2a5010.png)

3）粘贴到`文生图`或`图生图`的参数模板中使用

![](img/1528834c08b5506fc0bda91ff4d5bd50.png)

![](img/2227d7864662197dc763a078444f3cc9.png)

![](img/4f2c1c2a362d9d5860ff427181c5da04.png)

4）若忘记了在生图参数中应用的模型是哪一款，可以使用“`哩布模型`+`模型ID`”进行查询

![](img/f77893d5c5f9533dc9fabe8c2058a1b5.png)

# 五、高级参数及使用示例

## 星流文生图

+   支持的高级参数类型：`line`、`depth`、`pose`、`IPAdapter`

+   参数格式：

+   `星流文生图[图像描述]<line:0.75,https://example.com/image.jpg> --ar 3:4`

+   `星流文生图[图像描述]<depth:0.75,https://example.com/image.jpg> --ar 3:4`

+   `星流文生图[图像描述]<pose:0.75,https://example.com/image.jpg> --ar 3:4`

+   `星流文生图[图像描述]<IPAdapter:0.75,https://example.com/image.jpg> --ar 3:4`

+   使用示例：

| 参考图 | 控制类型：`line` | 控制类型：`depth` | 控制类型：`pose` |
| --- | --- | --- | --- |
| ![](img/77503c405e2ab2669789bb737343e68d.png) | ![](img/ba2524c966afe256ee8258d38fcebf89.png) | ![](img/f4f7924a7d92ebdb0264226a40fbaf1d.png) | ![](img/f96566814d8ed13b0b0aaeee528fa277.png) |
| 带高级参数的星流文生图提示词示例➡️ | `星流文生图  shuimo,Epic CG masterpiece, A portrait of a Chinese girl with a gorgeous headpiece,exquisite makeup,gradient color background,collarbone visible,Sexy,cleavage, Smooth skin. Gentle smile. The wind blows her long hair,there are petals in the air, dynamic poses,stunning colors,cinematic lighting effects,&lt;line:0.75,https://i.ibb.co/wYcLG0G/6a611349447e.jpg&gt; --ar 3:4` | `星流文生图  shuimo,Epic CG masterpiece, A portrait of a Chinese girl with a gorgeous headpiece,exquisite makeup,gradient color background,collarbone visible,Sexy,cleavage, Smooth skin. Gentle smile. The wind blows her long hair,there are petals in the air, dynamic poses,stunning colors,cinematic lighting effects,&lt;depth:0.75,https://i.ibb.co/wYcLG0G/6a611349447e.jpg&gt; --ar 3:4` | `星流文生图  shuimo,Epic CG masterpiece, A portrait of a Chinese girl with a gorgeous headpiece,exquisite makeup,gradient color background,collarbone visible,Sexy,cleavage, Smooth skin. Gentle smile. The wind blows her long hair,there are petals in the air, dynamic poses,stunning colors,cinematic lighting effects,&lt;pose:0.75,https://i.ibb.co/wYcLG0G/6a611349447e.jpg&gt; --ar 3:4` |
| 控制能力 | 弱（可以看到脸上的头发实际就是左边参考图的人脸轮廓，说明它识别到了但就是不按线稿轮廓来） | 强（高度遵循提示词的同时，人物姿态和景深关系都得到了很好的还原） | 一般（可以看到人脸和五官基本上控制住了，但是肢体姿势控制不佳） |
| 推荐程度 | ⭐️ | ⭐️⭐️⭐️⭐️ | ⭐️⭐️ |

| 参考图 | 控制类型：`IPAdapter` |
| --- | --- |
| ![](img/38c2c87a5107345269d6a5cc9b9afc0f.png) | ![](img/c5204255d7f0e44a7236e9cd9ba8bf33.png) |
| 带高级参数的星流文生图提示词示例➡️ | `星流文生图  shuimo,Epic CG masterpiece, A portrait of a Chinese girl with a gorgeous headpiece,exquisite makeup,gradient color background,collarbone visible,Sexy,cleavage, Smooth skin. Gentle smile. The wind blows her long hair,there are petals in the air, dynamic poses,stunning colors,cinematic lighting effects, &lt;IPAdapter:0.75,https://i.ibb.co/BrvXSwk/48ca5988f25a.png&gt; --ar 3:4` |
| 控制能力 | 强（高度遵循提示词的同时，画面风格和构图也得到了很好的迁移） |
| 推荐程度 | ⭐️⭐️⭐️⭐️ |

## FLUX文生图

+   支持的高级参数类型：`IPAdapter`（官方暂未开放其他控制类型的`API`接入方式）

+   参数格式：

+   `FLUX文生图[图像描述]<IPAdapter:0.75,https://example.com/image.jpg> --ar 3:4`

+   使用示例：

| 参考图 | 控制类型：`IPAdapter` |
| --- | --- |
| ![](img/e98271d26d8b323011fca6db5ab43502.png) | ![](img/2ff4a837c90b037a3dcec8c5d133e6bf.png) |
| 带高级参数的FLUX文生图提示词示例➡️ | `FLUX文生图 Profile portrait of an Asian woman&lt;IPAdapter:0.7,https://i.ibb.co/8dGWSXk/48ca5988f25a.png&gt; --ar 3:4` |
| 控制能力 | 强（高度遵循提示词的同时，画面风格和构图也得到了很好的迁移） |
| 推荐程度 | ⭐️⭐️⭐️⭐️⭐️ |

## FLUX图生图

+   支持的高级参数类型：`denoise`（重绘幅度）、`IPAdapter`

所谓图生图，本质上就是对原图进行逆采样(将一张图片逐步由像素恢复为原始噪点)的过程，因此相对原图而言，降噪幅度(也就是重绘幅度)越低，生成的结果变化越小；降噪幅度越高，留给AI发挥的空间也就越大

+   参数格式：

+   `FLUX图生图[图像描述]<denoise:0.75,https://example.com/image.jpg> --ar 3:4`

+   使用示例：

| 参考图 | 图生图类型：写实 | 图生图类型：卡通 | 图生图类型：动漫 |
| --- | --- | --- | --- |
| ![](img/7fb2f493f39f343f5971982e1e2dd453.png) | ![](img/e6991fb6af4fee3b5a845a86e56196af.png) | ![](img/e3e03997594cc9f44f31031fd0354bea.png) | ![](img/e54c00a062f2994e5b0eda8b52ed6192.png) |
| FLUX图生图提示词示例➡️ | `FLUX图生图 Asian portrait,A young woman wearing a green baseball cap, close shot, background is coffee store, masterpiece, best quality &lt;Ckpt:F1_Photo&gt;&lt;denoise:0.75,https://i.ibb.co/wYcLG0G/6a611349447e.jpg&gt; --ar 3:4` | `FLUX图生图 Asian women, Disney style &lt;Ckpt:F1_3Dcartoon&gt;&lt;denoise:0.75,https://i.ibb.co/wYcLG0G/6a611349447e.jpg&gt; --ar 3:4` | `FLUX图生图 Asian women, black and white line art&lt;Ckpt:F1_illustration&gt;&lt;denoise:0.75,https://i.ibb.co/wYcLG0G/6a611349447e.jpg&gt; --ar 3:4` |

## SDXL文生图

+   支持的高级参数类型：`XL_canny`、`XL_depth`、`XL_pose`、`XL_IPA`

+   参数格式：

+   `SDXL文生图[图像描述]<XL_canny:0.75,https://example.com/image.jpg> --ar 3:4`

+   `SDXL文生图[图像描述]<XL_depth:0.75,https://example.com/image.jpg> --ar 3:4`

+   `SDXL文生图[图像描述]<XL_pose:0.75,https://example.com/image.jpg> --ar 3:4`

+   `SDXL文生图[图像描述]<XL_IPA:0.75,https://example.com/image.jpg> --ar 3:4`

+   使用示例：

| 参考图 | 控制类型：`XL_canny` | 控制类型：`XL_depth` | 控制类型：`XL_pose` |
| --- | --- | --- | --- |
| ![](img/5557a3fdd34c393abc157bb70ad9f53c.png) | ![](img/4d572ff961caed4d9fd932f5e81166ef.png) | ![](img/139f54ed537f8da22aba772459c5236a.png) | ![](img/3eae92faee5c32dc2a0e1c9f18352342.png) |
| 带高级参数的SDXL文生图提示词示例➡️ | `SDXL文生图  shuimo,Epic CG masterpiece, A portrait of a Chinese girl with a gorgeous headpiece,exquisite makeup,gradient color background,collarbone visible,Sexy,cleavage, Smooth skin. Gentle smile. The wind blows her long hair,there are petals in the air, dynamic poses,stunning colors,cinematic lighting effects,&lt;XL_canny:0.75,https://i.ibb.co/wYcLG0G/6a611349447e.jpg&gt; --ar 3:4` | `SDXL文生图  shuimo,Epic CG masterpiece, A portrait of a Chinese girl with a gorgeous headpiece,exquisite makeup,gradient color background,collarbone visible,Sexy,cleavage, Smooth skin. Gentle smile. The wind blows her long hair,there are petals in the air, dynamic poses,stunning colors,cinematic lighting effects,&lt;XL_depth:0.75,https://i.ibb.co/wYcLG0G/6a611349447e.jpg&gt; --ar 3:4` | `SDXL文生图  shuimo,Epic CG masterpiece, A portrait of a Chinese girl with a gorgeous headpiece,exquisite makeup,gradient color background,collarbone visible,Sexy,cleavage, Smooth skin. Gentle smile. The wind blows her long hair,there are petals in the air, dynamic poses,stunning colors,cinematic lighting effects,&lt;XL_pose:0.75,https://i.ibb.co/wYcLG0G/6a611349447e.jpg&gt; --ar 3:4` |
| 控制能力 | 一般 | 强 | 弱 |
| 推荐程度 | ⭐️⭐️⭐️ | ⭐️⭐️⭐️⭐️ | ⭐️ |

| 参考图 | 控制类型：`XL_IPA` |
| --- | --- |
| ![](img/3a0bcf9c354e211ec8eb4ffeb9d0874c.png) | ![](img/4917362854e2dd2d7b726ce97586a7cc.png) |
| 带高级参数的SDXL文生图提示词示例➡️ | `SDXL文生图  shuimo,Epic CG masterpiece, A portrait of a Chinese girl with a gorgeous headpiece,exquisite makeup,gradient color background,collarbone visible,Sexy,cleavage, Smooth skin. Gentle smile. The wind blows her long hair,there are petals in the air, dynamic poses,stunning colors,cinematic lighting effects,&lt;Ckpt:XL_筑梦动漫V5&gt;&lt;XL_IPA:0.75,https://i.ibb.co/BrvXSwk/48ca5988f25a.png&gt; --ar 3:4` |
| 控制能力 | 强（高度遵循提示词的同时，画面风格和构图也得到了很好的迁移） |
| 推荐程度 | ⭐️⭐️⭐️⭐️ |

## SDXL人脸参考
