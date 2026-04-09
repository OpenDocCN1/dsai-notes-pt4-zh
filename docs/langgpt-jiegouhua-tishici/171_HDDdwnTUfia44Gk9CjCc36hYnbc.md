# 高德天气插件的YAML描述

> 来源：[https://langgptai.feishu.cn/docx/EfgMdUr2yoLSjgx8MBccEBDxnEb](https://langgptai.feishu.cn/docx/EfgMdUr2yoLSjgx8MBccEBDxnEb)

腾讯元器高德天气YAML的完整描述：

```
openapi: &quot;3.0.0&quot;
info:
  title: &quot;高德天气API&quot;
  version: &quot;1.0.0&quot;
  description: &quot;根据用户输入的adcode，查询目标区域当前/未来的天气情况，数据来源是中国气象局。&quot;
servers:
  - url: &quot;https://restapi.amap.com/v3/weather&quot;
    description: &quot;高德天气API服务地址&quot;
paths:
  &quot;/weatherInfo&quot;:
    get:
      summary: &quot;天气查询&quot;
      description: &quot;根据城市编码查询天气信息&quot;
      operationId: &quot;getWeatherInfo&quot;
      parameters:
        - name: &quot;city&quot;
          in: &quot;query&quot;
          description: &quot;城市编码&quot;
          required: true
          schema:
            type: &quot;string&quot;
        - name: &quot;extensions&quot;
          in: &quot;query&quot;
          description: &quot;气象类型&quot;
          required: false
          schema:
            type: &quot;string&quot;
            enum: [&quot;base&quot;, &quot;all&quot;]
        - name: &quot;output&quot;
          in: &quot;query&quot;
          description: &quot;返回格式&quot;
          required: false
          schema:
            type: &quot;string&quot;
            default: &quot;JSON&quot;
            enum: [&quot;JSON&quot;, &quot;XML&quot;]
      responses:
        &quot;200&quot;:
          description: &quot;成功的响应&quot;
          content:
            application/json:
              schema:
                type: &quot;object&quot;
                properties:
                  status:
                    type: &quot;integer&quot;
                    description: &quot;返回状态&quot;
                    enum: [0, 1]
                  count:
                    type: &quot;integer&quot;
                    description: &quot;返回结果总数目&quot;
                  info:
                    type: &quot;string&quot;
                    description: &quot;返回的状态信息&quot;
                  infocode:
                    type: &quot;string&quot;
                    description: &quot;返回状态说明, 10000代表正确&quot;
                  lives:
                    type: &quot;array&quot;
                    description: &quot;实况天气数据信息&quot;
                    items:
                      type: &quot;object&quot;
                      properties:
                        province:
                          type: &quot;string&quot;
                          description: &quot;省份名&quot;
                        city:
                          type: &quot;string&quot;
                          description: &quot;城市名&quot;
                        adcode:
                          type: &quot;string&quot;
                          description: &quot;区域编码&quot;
                        weather:
                          type: &quot;string&quot;
                          description: &quot;天气现象（汉字描述）&quot;
                        temperature:
                          type: &quot;number&quot;
                          description: &quot;实时气温，单位：摄氏度&quot;
                        winddirection:
                          type: &quot;string&quot;
                          description: &quot;风向描述&quot;
                        windpower:
                          type: &quot;integer&quot;
                          description: &quot;风力级别，单位：级&quot;
                        humidity:
                          type: &quot;number&quot;
                          description: &quot;空气湿度&quot;
                        reporttime:
                          type: &quot;string&quot;
                          description: &quot;数据发布的时间&quot;
                  forecast:
                    type: &quot;object&quot;
                    properties:
                      city:
                        type: &quot;string&quot;
                        description: &quot;城市名称&quot;
                      adcode:
                        type: &quot;string&quot;
                        description: &quot;城市编码&quot;
                      province:
                        type: &quot;string&quot;
                        description: &quot;省份名称&quot;
                      reporttime:
                        type: &quot;string&quot;
                        description: &quot;预报发布时间&quot;
                      casts:
                        type: &quot;array&quot;
                        description: &quot;预报数据list结构，元素cast,按顺序为当天、第二天、第三天的预报数据&quot;
                        items:
                          type: &quot;object&quot;
                          properties:
                            date:
                              type: &quot;string&quot;
                              description: &quot;日期&quot;
                            week:
                              type: &quot;string&quot;
                              description: &quot;星期几&quot;
                            dayweather:
                              type: &quot;string&quot;
                              description: &quot;白天天气现象&quot;
                            nightweather:
                              type: &quot;string&quot;
                              description: &quot;晚上天气现象&quot;
                            daytemp:
                              type: &quot;number&quot;
                              description: &quot;白天温度&quot;
                            nighttemp:
                              type: &quot;number&quot;
                              description: &quot;晚上温度&quot;
                            daywind:
                              type: &quot;string&quot;
                              description: &quot;白天风向&quot;
                            nightwind:
                              type: &quot;string&quot;
                              description: &quot;晚上风向&quot;
                            daypower:
                              type: &quot;integer&quot;
                              description: &quot;白天风力&quot;
                            nightpower:
                              type: &quot;integer&quot;
                              description: &quot;晚上风力&quot;
        &quot;400&quot;:
          description: &quot;错误的请求&quot;
        &quot;401&quot;:
          description: &quot;未授权&quot;
        &quot;500&quot;:
          description: &quot;服务器内部错误&quot;
```