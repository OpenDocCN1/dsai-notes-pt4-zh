
# 高德天气插件的YAML描述

> 来源：[https://langgptai.feishu.cn/docx/EfgMdUr2yoLSjgx8MBccEBDxnEb](https://langgptai.feishu.cn/docx/EfgMdUr2yoLSjgx8MBccEBDxnEb)

腾讯元器高德天气YAML的完整描述：

```yaml
openapi: "3.0.0"
info:
  title: "高德天气API"
  version: "1.0.0"
  description: "根据用户输入的adcode，查询目标区域当前/未来的天气情况，数据来源是中国气象局。"
servers:
  - url: "https://restapi.amap.com/v3/weather"
    description: "高德天气API服务地址"
paths:
  "/weatherInfo":
    get:
      summary: "天气查询"
      description: "根据城市编码查询天气信息"
      operationId: "getWeatherInfo"
      parameters:
        - name: "city"
          in: "query"
          description: "城市编码"
          required: true
          schema:
            type: "string"
        - name: "extensions"
          in: "query"
          description: "气象类型"
          required: false
          schema:
            type: "string"
            enum: ["base", "all"]
        - name: "output"
          in: "query"
          description: "返回格式"
          required: false
          schema:
            type: "string"
            default: "JSON"
            enum: ["JSON", "XML"]
      responses:
        "200":
          description: "成功的响应"
          content:
            application/json:
              schema:
                type: "object"
                properties:
                  status:
                    type: "integer"
                    description: "返回状态"
                    enum: [0, 1]
                  count:
                    type: "integer"
                    description: "返回结果总数目"
                  info:
                    type: "string"
                    description: "返回的状态信息"
                  infocode:
                    type: "string"
                    description: "返回状态说明, 10000代表正确"
                  lives:
                    type: "array"
                    description: "实况天气数据信息"
                    items:
                      type: "object"
                      properties:
                        province:
                          type: "string"
                          description: "省份名"
                        city:
                          type: "string"
                          description: "城市名"
                        adcode:
                          type: "string"
                          description: "区域编码"
                        weather:
                          type: "string"
                          description: "天气现象（汉字描述）"
                        temperature:
                          type: "number"
                          description: "实时气温，单位：摄氏度"
                        winddirection:
                          type: "string"
                          description: "风向描述"
                        windpower:
                          type: "integer"
                          description: "风力级别，单位：级"
                        humidity:
                          type: "number"
                          description: "空气湿度"
                        reporttime:
                          type: "string"
                          description: "数据发布的时间"
                  forecast:
                    type: "object"
                    properties:
                      city:
                        type: "string"
                        description: "城市名称"
                      adcode:
                        type: "string"
                        description: "城市编码"
                      province:
                        type: "string"
                        description: "省份名称"
                      reporttime:
                        type: "string"
                        description: "预报发布时间"
                      casts:
                        type: "array"
                        description: "预报数据list结构，元素cast,按顺序为当天、第二天、第三天的预报数据"
                        items:
                          type: "object"
                          properties:
                            date:
                              type: "string"
                              description: "日期"
                            week:
                              type: "string"
                              description: "星期几"
                            dayweather:
                              type: "string"
                              description: "白天天气现象"
                            nightweather:
                              type: "string"
                              description: "晚上天气现象"
                            daytemp:
                              type: "number"
                              description: "白天温度"
                            nighttemp:
                              type: "number"
                              description: "晚上温度"
                            daywind:
                              type: "string"
                              description: "白天风向"
                            nightwind:
                              type: "string"
                              description: "晚上风向"
                            daypower:
                              type: "integer"
                              description: "白天风力"
                            nightpower:
                              type: "integer"
                              description: "晚上风力"
        "400":
          description: "错误的请求"
        "401":
          description: "未授权"
        "500":
          description: "服务器内部错误"
```
