# 如何添加图片水印

> 来源：[https://hyperspace.feishu.cn/docx/LOUzdnSa8oJoM8xnaZdcBmVin7e](https://hyperspace.feishu.cn/docx/LOUzdnSa8oJoM8xnaZdcBmVin7e)

插件输出结果中包含图片，且图片中的文本内容或图像内容是由 AI 生成的，需通过此接口在 AI 生成图片上增加水印，插件输出结果的图片如未添加水印，插件将被审核驳回或强制下线。

目前只支持在bmp、jpeg、jpg、png格式的图片上增加水印。

请求地址

```
POST https://openapi.baidu.com/file/2.0/lingjing/open/addWaterMark
```

公共请求参数

请求参数

公共响应参数

请求示例

```
curl -X POST &#x27;https://openapi.baidu.com/file/2.0/lingjing/open/addWaterMark?access_token=45.d5399ff94657c24e05541e9083cb6a5b.3600.1582805648.XK9_OHvhaMiG9JYKYV3hqQ9fNQ8e-qsN6gz8WG1fqJ8ZR5Q&#x27; \
--form &#x27;file=@/Users/XXXX/Downloads/00002.jpg&#x27; \
--form &#x27;sid=1000000&#x27;
```

响应示例

```
{
        &quot;errno&quot;: 0,
        &quot;msg&quot;: &quot;success&quot;,
        &quot;data&quot;: &quot;https://lingjing-open.bj.bcebos.com/wm/2024/01/02/5290c066e5dd7729df662052cc1a894d.jpg&quot;
}
```

错误码