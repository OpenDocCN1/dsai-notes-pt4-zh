
## 响应示例（非流式）

```http
HTTP/1.1 200 OK
Date: Mon, 12 Apr 2021 06:27:55 GMT
Content-Type: application/json;charset=utf-8
Statement: AI-generated

{
  &quot;id&quot;: &quot;as-1tbbc0x5af&quot;,
  &quot;object&quot;: &quot;chat.completion&quot;,
  &quot;created&quot;: 1680166551,
  &quot;result&quot;: &quot;2023年4月2日上海气温13~21℃，多云转阴，东风3-4级，空气质量良，空气质量指数55。\n\n\n\n近7日天气信息：\n\n2023-03-29：阴转小雨，11~17℃，东北风&lt;3级，空气质量良。\n\n2023-03-30：小雨转阴，10~14℃，东风3-4级，空气质量良。\n\n2023-03-31：多云，12~18℃，东风3-4级，空气质量优。\n\n2023-04-01：多云转晴，11~20℃，东南风3-4级，空气质量良。\n\n2023-04-02：多云转阴，13~21℃，东风3-4级，空气质量良。\n\n2023-04-03：阴转中雨，15~18℃，东南风4-5级，空气质量良。\n\n2023-04-04：中雨转小雨，10~17℃，南风5-6级，空气质量优。\n\n2023-04-05：阴，9~14℃，西北风3-4级，空气质量优。&quot;,
  &quot;is_truncated&quot;:false,
  &quot;need_clear_history&quot;: false,
  &quot;usage&quot;: {
    &quot;prompt_tokens&quot;: 470,
    &quot;completion_tokens&quot;: 198,
    &quot;total_tokens&quot;: 668
  }
}
```

## 请求示例（流式）

```bash
# 步骤一，获取access_token，替换下列示例中的API Key与Secret Key
curl &#x27;https://aip.baidubce.com/oauth/2.0/token?grant_type=client_credentials&amp;client_id=[API Key]&amp;client_secret=[Secret Key]&#x27;

# 步骤二，调用本文API，使用步骤一获取的access_token，替换下列示例中的”调用接口获取的access_token“
curl -XPOST &#x27;https://aip.baidubce.com/rpc/2.0/ai_custom/v1/wenxinworkshop/chat/bloomz_7b1?access_token=[步骤一调用接口获取的access_token]&#x27; -d &#x27;{
  &quot;messages&quot;: [
    {&quot;role&quot;:&quot;user&quot;, &quot;content&quot;: &quot;给我推荐一些自驾游路线&quot;}
  ],
  &quot;stream&quot;: true
}&#x27;
```

```python
import requests
import json

def get_access_token():
    &quot;&quot;&quot;
    使用 API Key，Secret Key 获取access_token，替换下列示例中的应用API Key、应用Secret Key
    &quot;&quot;&quot;
        
    url = &quot;https://aip.baidubce.com/oauth/2.0/token?grant_type=client_credentials&amp;client_id=[应用API Key]&amp;client_secret=[应用Secret Key]&quot;
    
    payload = json.dumps(&quot;&quot;)
    headers = {
        &#x27;Content-Type&#x27;: &#x27;application/json&#x27;,
        &#x27;Accept&#x27;: &#x27;application/json&#x27;
    }
    
    response = requests.request(&quot;POST&quot;, url, headers=headers, data=payload)
    return response.json().get(&quot;access_token&quot;)

def main():
        
    url = &quot;https://aip.baidubce.com/rpc/2.0/ai_custom/v1/wenxinworkshop/chat/bloomz_7b1?access_token=&quot; + get_access_token()
    
    payload = json.dumps({
        &quot;messages&quot;: [
            {
                &quot;role&quot;: &quot;user&quot;,
                &quot;content&quot;: &quot;给我推荐一些自驾游路线&quot;
            }
        ],
         &quot;stream&quot;: True
    })
    headers = {
        &#x27;Content-Type&#x27;: &#x27;application/json&#x27;
    }
    
    response = requests.request(&quot;POST&quot;, url, headers=headers, data=payload)
    
    print(response.text)
    

if __name__ == &#x27;__main__&#x27;:
    main()
```

## 响应示例（流式）

```http
HTTP/1.1 200 OK
Date: Mon, 12 Apr 2021 06:27:55 GMT
Content-Type: text/event-stream;charset=utf-8
Cache-Control: no-cache
Statement: AI-generated

data: {&quot;id&quot;:&quot;as-ywwpgx4dt7&quot;,&quot;object&quot;:&quot;chat.completion&quot;,&quot;created&quot;:1680166793,&quot;sentence_id&quot;:0,&quot;is_end&quot;:false,&quot;is_truncated&quot;:false,&quot;result&quot;:&quot;以下是一些适合自驾游的路线推荐：\n\n1.&quot;,&quot;need_clear_history&quot;:false,&quot;usage&quot;:{&quot;prompt_tokens&quot;:11,&quot;completion_tokens&quot;:16,&quot;total_tokens&quot;:27}}

data: {&quot;id&quot;:&quot;as-ywwpgx4dt7&quot;,&quot;object&quot;:&quot;chat.completion&quot;,&quot;created&quot;:1680166794,&quot;sentence_id&quot;:1,&quot;is_end&quot;:false,&quot;is_truncated&quot;:false,&quot;result&quot;:&quot; 中国大陆最美的景观大道：川藏线，从成都出发，沿着川藏公路一路向西，经过稻城亚丁、理塘、巴塘、芒康等美景，最终到达拉萨。&quot;,&quot;need_clear_history&quot;:false,&quot;usage&quot;:{&quot;prompt_tokens&quot;:11,&quot;completion_tokens&quot;:50,&quot;total_tokens&quot;:77}}

data: {&quot;id&quot;:&quot;as-ywwpgx4dt7&quot;,&quot;object&quot;:&quot;chat.completion&quot;,&quot;created&quot;:1680166795,&quot;sentence_id&quot;:2,&quot;is_end&quot;:false,&quot;is_truncated&quot;:false,&quot;result&quot;:&quot;\n2. 丝绸之路：这是一条贯穿中国东西部的公路，从上海出发，经过西安、兰州、乌鲁木齐等城市，最终到达喀什。&quot;,&quot;need_clear_history&quot;:false,&quot;usage&quot;:{&quot;prompt_tokens&quot;:11,&quot;completion_tokens&quot;:43,&quot;total_tokens&quot;:120}}

data: {&quot;id&quot;:&quot;as-ywwpgx4dt7&quot;,&quot;object&quot;:&quot;chat.completion&quot;,&quot;created&quot;:1680166796,&quot;sentence_id&quot;:3,&quot;is_end&quot;:false,&quot;is_truncated&quot;:false,&quot;result&quot;:&quot;沿途可以欣赏到中国北方和南方的不同景色。\n3. 西北大环线：从成都出发，经过都江堰、青城山、丹巴、塔公草原、新都桥等景点，最终到达西宁。&quot;,&quot;need_clear_history&quot;:false,&quot;usage&quot;:{&quot;prompt_tokens&quot;:11,&quot;completion_tokens&quot;:56,&quot;total_tokens&quot;:176}}

data: {&quot;id&quot;:&quot;as-ywwpgx4dt7&quot;,&quot;object&quot;:&quot;chat.completion&quot;,&quot;created&quot;:1680166799,&quot;sentence_id&quot;:4,&quot;is_end&quot;:false,&quot;is_truncated&quot;:false,&quot;result&quot;:&quot;这条路线可以领略到中国西北地区的壮美风光。\n4. 东北环线：从沈阳出发，经过长春、吉林、延吉等城市，最终到达哈尔滨。&quot;,&quot;need_clear_history&quot;:false,&quot;usage&quot;:{&quot;prompt_tokens&quot;:11,&quot;completion_tokens&quot;:48,&quot;total_tokens&quot;:224}}

data: {&quot;id&quot;:&quot;as-ywwpgx4dt7&quot;,&quot;object&quot;:&quot;chat.completion&quot;,&quot;created&quot;:1680166800,&quot;sentence_id&quot;:5,&quot;is_end&quot;:false,&quot;is_truncated&quot;:false,&quot;result&quot;:&quot;沿途可以欣赏到中国东北地区的广阔草原和森林。\n5. 西南环线：从成都出发，经过雅安、康定、理塘、稻城亚丁等景点，最终到达香格里拉。&quot;,&quot;need_clear_history&quot;:false,&quot;usage&quot;:{&quot;prompt_tokens&quot;:11,&quot;completion_tokens&quot;:54,&quot;total_tokens&quot;:278}}

data: {&quot;id&quot;:&quot;as-ywwpgx4dt7&quot;,&quot;object&quot;:&quot;chat.completion&quot;,&quot;created&quot;:1680166802,&quot;sentence_id&quot;:6,&quot;is_end&quot;:false,&quot;is_truncated&quot;:false,&quot;result&quot;:&quot;这条路线可以领略到中国西南地区的高山峡谷和民族文化。\n6. 海南环线：从三亚出发，经过海口、文昌、万宁等城市，最终到达三亚。&quot;,&quot;need_clear_history&quot;:false,&quot;usage&quot;:{&quot;prompt_tokens&quot;:11,&quot;completion_tokens&quot;:52,&quot;total_tokens&quot;:330}}

data: {&quot;id&quot;:&quot;as-ywwpgx4dt7&quot;,&quot;object&quot;:&quot;chat.completion&quot;,&quot;created&quot;:1680166803,&quot;sentence_id&quot;:7,&quot;is_end&quot;:false,&quot;is_truncated&quot;:false,&quot;result&quot;:&quot;这条路线可以欣赏到中国南方的海岸线和热带雨林。\n7. 滇藏线：从昆明出发，经过大理、丽江、香格里拉等景点，最终到达西藏拉萨。&quot;,&quot;need_clear_history&quot;:false,&quot;usage&quot;:{&quot;prompt_tokens&quot;:11,&quot;completion_tokens&quot;:52,&quot;total_tokens&quot;:382}}

data: {&quot;id&quot;:&quot;as-ywwpgx4dt7&quot;,&quot;object&quot;:&quot;chat.completion&quot;,&quot;created&quot;:1680166804,&quot;sentence_id&quot;:8,&quot;is_end&quot;:true,&quot;is_truncated&quot;:false,&quot;result&quot;:&quot;这条路线可以领略到中国西南地区和西藏地区的自然风光和人文景观。&quot;,&quot;need_clear_history&quot;:false,&quot;usage&quot;:{&quot;prompt_tokens&quot;:11,&quot;completion_tokens&quot;:30,&quot;total_tokens&quot;:412}}
```

## 错误码

如果请求错误，服务器返回的JSON文本包含以下参数。

| 名称 | 描述 |
| --- | --- |
| `error_code` | 错误码 |
| `error_msg` | 错误描述信息，帮助理解和解决发生的错误 |

例如`Access Token`失效返回：

```json
{
  &quot;error_code&quot;: 110,
  &quot;error_msg&quot;: &quot;Access token invalid or no longer valid&quot;
}
```

需要重新获取新的`Access Token`再次请求。

| 错误码 | 错误信息 | 描述 |
| --- | --- | --- |
| 1 | Unknown error | 服务器内部错误，请再次请求， 如[果持续出现](https://ticket.bce.baidu.com/#/ticket/create?useticket=1&amp;productId=279)此类错误，请在百度云控制台内 提交工单 反馈 |
| 2 | Service temporarily unavailable | [服务暂不可](https://ticket.bce.baidu.com/#/ticket/create?useticket=1&amp;productId=279&amp;questionId=1549)用，请再次请求， 如果持续出现此类错误，请在百度云控制台内 提交工单 反馈 |
| 3 | Unsupported openapi method | 调用的API不存在，请检查后重新尝试 |
| 4 | Open api request limit reached | 集群超限额 |
| 6 | No permission to access data | 无权限访问该用户数据 |
| 13 | Get service token failed | 获取token失败 |
| 14 | IAM Certification failed | IAM鉴权失败 |
| 15 | app not exists or create failed | 应用不存在或者创建失败 |
