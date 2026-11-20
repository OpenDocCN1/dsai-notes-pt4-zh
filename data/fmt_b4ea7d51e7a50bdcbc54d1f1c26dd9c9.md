
```
Response(id=&#x27;resp_67d3e43ec15c8190be1d4ef786c5000d04483d504f8a42a4&#x27;, created_at=1741939774.0, error=None, incomplete_details=None, instructions=None, metadata={}, model=&#x27;gpt-4o-2024-08-06&#x27;, object=&#x27;response&#x27;, output=[ResponseFunctionWebSearch(id=&#x27;ws_67d3e43f2d348190af9efa09bfe0101304483d504f8a42a4&#x27;, status=&#x27;completed&#x27;, type=&#x27;web_search_call&#x27;), ResponseOutputMessage(id=&#x27;msg_67d3e4414c3481908cf889203e6abe8f04483d504f8a42a4&#x27;, content=[ResponseOutputText(annotations=[AnnotationURLCitation(end_index=215, start_index=122, title=&#x27;香港政商界：降息对香港经济和资产市场影响正面_新闻频道_中国青年网&#x27;, type=&#x27;url_citation&#x27;, url=&#x27;https://news.youth.cn/jsxw/202409/t20240919_15529696.htm?utm_source=openai&#x27;), AnnotationURLCitation(end_index=411, start_index=320, title=&#x27;中国青年对欧洲及德国总体评价积极正面_新闻频道_中国青年网&#x27;, type=&#x27;url_citation&#x27;, url=&#x27;https://news.youth.cn/gj/202207/t20220726_13871411.htm?utm_source=openai&#x27;), AnnotationURLCitation(end_index=597, start_index=519, title=&#x27;正面舆情：新闻规律回归与正面报道新路径_腾讯新闻&#x27;, type=&#x27;url_citation&#x27;, url=&#x27;https://news.qq.com/rain/a/20240908A00ZCQ00?utm_source=openai&#x27;)], text=&#x27;以下是近期的一些正面新闻：\n\n1. **香港政商界对降息持积极态度**：2024年9月19日，美国联邦储备委员会四年来首次降息，香港金融管理局随即下调基本利率。香港政商界人士认为，此举有助于降低企业资金成本，对香港经济和资产市场产生正面影响。 ([news.youth.cn](https://news.youth.cn/jsxw/202409/t20240919_15529696.htm?utm_source=openai))\n\n2. **中国青年对欧洲及德国评价积极**：2022年7月发布的《中国青年的欧洲观》报告显示，中国青年普遍对欧洲，特别是德国，持积极正面的评价，认为中欧及中德关系总体友好互利，并对未来关系发展持乐观态度。 ([news.youth.cn](https://news.youth.cn/gj/202207/t20220726_13871411.htm?utm_source=openai))\n\n3. **正面舆情引领新闻报道新路径**：近年来，天津“跳水大爷”、广东龙舟赛“摇头哥”、淄博烧烤等正面舆情事件增多，体现了网络生态的积极变化，为媒体进行正面报道提供了新的思路，有助于克服“强行正能量”等问题。 ([news.qq.com](https://news.qq.com/rain/a/20240908A00ZCQ00?utm_source=openai))\n\n希望这些新闻能为您带来积极的感受。 &#x27;, type=&#x27;output_text&#x27;)], role=&#x27;assistant&#x27;, status=&#x27;completed&#x27;, type=&#x27;message&#x27;)], parallel_tool_calls=True, temperature=1.0, tool_choice=&#x27;auto&#x27;, tools=[WebSearchTool(type=&#x27;web_search_preview&#x27;, search_context_size=&#x27;medium&#x27;, user_location=UserLocation(type=&#x27;approximate&#x27;, city=None, country=&#x27;US&#x27;, region=None, timezone=None))], top_p=1.0, max_output_tokens=None, previous_response_id=None, reasoning=Reasoning(effort=None, generate_summary=None), status=&#x27;completed&#x27;, text=ResponseTextConfig(format=ResponseFormatText(type=&#x27;text&#x27;)), truncation=&#x27;disabled&#x27;, usage=ResponseUsage(input_tokens=326, output_tokens=364, output_tokens_details=OutputTokensDetails(reasoning_tokens=0), total_tokens=690, input_tokens_details={&#x27;cached_tokens&#x27;: 0}), user=None, store=True)
```

```
response1 = client.responses.create(
    model=&quot;gpt-4o&quot;,
    tools=[{&quot;type&quot;: &quot;web_search_preview&quot;}],  # 启用 Web 搜索工具
    input=&quot;请帮我讲个笑话吧。&quot;
)
```

```
print(response1.output_text)
```

```
当然可以！你听说过那个关于小蘑菇的笑话吗？

为什么小蘑菇去派对？

因为它是一朵“欢人”！（Fun-guy，fungi）

希望你喜欢！😊
```

## 2. 强制使用 Web 搜索

如果希望确保模型一定使用 Web 搜索（避免它仅使用内部知识回答），可以设置`tool_choice`参数：

```
tool_choice={&quot;type&quot;: &quot;web_search_preview&quot;}
```

### 📌 作用

- 让 Web 搜索始终执行，而不是让模型决定是否使用搜索工具。
- 提升一致性，但可能会增加查询时间。

## 3. 输出格式与引用

如果模型调用了 Web 搜索，API 响应将包含两部分：

1. Web 搜索调用的 ID
2. 模型的回答，并带有网页来源的引用信息

### 📌 示例输出

```
[
  {
    &quot;type&quot;: &quot;web_search_call&quot;,
    &quot;id&quot;: &quot;ws_67c9fa0502748190b7dd390736892e100be649c1a5ff9609&quot;,
    &quot;status&quot;: &quot;completed&quot;
  },
  {
    &quot;id&quot;: &quot;msg_67c9fa077e288190af08fdffda2e34f20be649c1a5ff9609&quot;,
    &quot;type&quot;: &quot;message&quot;,
    &quot;status&quot;: &quot;completed&quot;,
    &quot;role&quot;: &quot;assistant&quot;,
    &quot;content&quot;: [
      {
        &quot;type&quot;: &quot;output_text&quot;,
        &quot;text&quot;: &quot;On March 6, 2025, several news...&quot;,
        &quot;annotations&quot;: [
          {
            &quot;type&quot;: &quot;url_citation&quot;,
            &quot;start_index&quot;: 2606,
            &quot;end_index&quot;: 2758,
            &quot;url&quot;: &quot;https://...&quot;,
            &quot;title&quot;: &quot;Title...&quot;
          }
        ]
      }
    ]
  }
]
```

```
response
```

```
Response(id=&#x27;resp_67d3e43ec15c8190be1d4ef786c5000d04483d504f8a42a4&#x27;, created_at=1741939774.0, error=None, incomplete_details=None, instructions=None, metadata={}, model=&#x27;gpt-4o-2024-08-06&#x27;, object=&#x27;response&#x27;, output=[ResponseFunctionWebSearch(id=&#x27;ws_67d3e43f2d348190af9efa09bfe0101304483d504f8a42a4&#x27;, status=&#x27;completed&#x27;, type=&#x27;web_search_call&#x27;), ResponseOutputMessage(id=&#x27;msg_67d3e4414c3481908cf889203e6abe8f04483d504f8a42a4&#x27;, content=[ResponseOutputText(annotations=[AnnotationURLCitation(end_index=215, start_index=122, title=&#x27;香港政商界：降息对香港经济和资产市场影响正面_新闻频道_中国青年网&#x27;, type=&#x27;url_citation&#x27;, url=&#x27;https://news.youth.cn/jsxw/202409/t20240919_15529696.htm?utm_source=openai&#x27;), AnnotationURLCitation(end_index=411, start_index=320, title=&#x27;中国青年对欧洲及德国总体评价积极正面_新闻频道_中国青年网&#x27;, type=&#x27;url_citation&#x27;, url=&#x27;https://news.youth.cn/gj/202207/t20220726_13871411.htm?utm_source=openai&#x27;), AnnotationURLCitation(end_index=597, start_index=519, title=&#x27;正面舆情：新闻规律回归与正面报道新路径_腾讯新闻&#x27;, type=&#x27;url_citation&#x27;, url=&#x27;https://news.qq.com/rain/a/20240908A00ZCQ00?utm_source=openai&#x27;)], text=&#x27;以下是近期的一些正面新闻：\n\n1. **香港政商界对降息持积极态度**：2024年9月19日，美国联邦储备委员会四年来首次降息，香港金融管理局随即下调基本利率。香港政商界人士认为，此举有助于降低企业资金成本，对香港经济和资产市场产生正面影响。 ([news.youth.cn](https://news.youth.cn/jsxw/202409/t20240919_15529696.htm?utm_source=openai))\n\n2. **中国青年对欧洲及德国评价积极**：2022年7月发布的《中国青年的欧洲观》报告显示，中国青年普遍对欧洲，特别是德国，持积极正面的评价，认为中欧及中德关系总体友好互利，并对未来关系发展持乐观态度。 ([news.youth.cn](https://news.youth.cn/gj/202207/t20220726_13871411.htm?utm_source=openai))\n\n3. **正面舆情引领新闻报道新路径**：近年来，天津“跳水大爷”、广东龙舟赛“摇头哥”、淄博烧烤等正面舆情事件增多，体现了网络生态的积极变化，为媒体进行正面报道提供了新的思路，有助于克服“强行正能量”等问题。 ([news.qq.com](https://news.qq.com/rain/a/20240908A00ZCQ00?utm_source=openai))\n\n希望这些新闻能为您带来积极的感受。 &#x27;, type=&#x27;output_text&#x27;)], role=&#x27;assistant&#x27;, status=&#x27;completed&#x27;, type=&#x27;message&#x27;)], parallel_tool_calls=True, temperature=1.0, tool_choice=&#x27;auto&#x27;, tools=[WebSearchTool(type=&#x27;web_search_preview&#x27;, search_context_size=&#x27;medium&#x27;, user_location=UserLocation(type=&#x27;approximate&#x27;, city=None, country=&#x27;US&#x27;, region=None, timezone=None))], top_p=1.0, max_output_tokens=None, previous_response_id=None, reasoning=Reasoning(effort=None, generate_summary=None), status=&#x27;completed&#x27;, text=ResponseTextConfig(format=ResponseFormatText(type=&#x27;text&#x27;)), truncation=&#x27;disabled&#x27;, usage=ResponseUsage(input_tokens=326, output_tokens=364, output_tokens_details=OutputTokensDetails(reasoning_tokens=0), total_tokens=690, input_tokens_details={&#x27;cached_tokens&#x27;: 0}), user=None, store=True)
```

```
response.output
```
