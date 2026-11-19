
# 为什么“Vary Region”功能不起作用❓

> 来源：[https://my.feishu.cn/docx/JDVzdM2JLo3qjqxy7QKcA9Ydn5c](https://my.feishu.cn/docx/JDVzdM2JLo3qjqxy7QKcA9Ydn5c)

有几个原因：

①为了使“Vary Region”功能生效，你需要准确选择你想要编辑的图像区域，使用“Rectangle”（矩形）和“Lasso”（套索）工具。选择所需区域后，按“Submit”（提交）按钮以确认你的选择。

![](img/e636d444afa09808e02d02f5ed5c9574.png)

②为了获得“Vary Region”功能的最佳效果，建议不要选择太大和太小的图像进行修改，大约占总面积的20%到50%。如果选择的区域太小，编辑器可能不会响应更改请求。

![](img/4c78d6cc1533b2c6c90e2c66720d7982.png)

③当你在提示中进行更改时，保留版本参数（例如，`--v 6`）是很重要的。删除此参数可能导致图像生成不正确或失败。

![](img/46e541096607aae4fd970a18519b60f0.png)
