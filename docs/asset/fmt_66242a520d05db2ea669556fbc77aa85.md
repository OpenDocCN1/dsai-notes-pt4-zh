
# V6 中如何创建文本：💕

> 来源：[https://my.feishu.cn/docx/BpwpdwvEdoroMzxUlVFclDTSncg](https://my.feishu.cn/docx/BpwpdwvEdoroMzxUlVFclDTSncg)

让我们了解一下在 V6 中如何创建文本：💕

①始终将所需的文本放在“双引号”中。这比使用‘单引号’能显著改善生成结果，后者可能会出现错误。

②添加字体风格的描述，例如：书法的、哥特式的、手写的等，

③使用 `--style raw` 参数使文本更加突出。但请注意，这可能会略微降低图像的艺术质量。

④调整 `stylize` 参数的值，以平衡文本的大小和风格。较高的值会创建更大更清晰的文本，较低的值则产生更装饰性和细小的文本。

## 案例

```
/Imagine prompt: monochrome withe blue color of the background, through which the shape of the letter "Love" made of flowers shows through --v 6.0 --s 750 --ar 16:9
```

![](img/c2ed4b8ca37b355c3314efb076ec12b9.png)
