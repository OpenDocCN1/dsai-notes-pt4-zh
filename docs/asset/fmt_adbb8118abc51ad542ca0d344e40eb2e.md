
# 【微调实战二】微调GPT-OSS模型Agent性能微调

`ds_text[6]['text']`

```
' <|start|>system<|message|>You are ChatGPT, a large language model trained by OpenAI.
Knowledge cutoff: 2024-06
Current date: 2025-08-25

Reasoning: low

# Valid channels: analysis, commentary, final. Channel must be included for every message.
Calls to these tools must go to the commentary channel: 'functions'.<|end|><|start|>developer<|message|># Instructions

You are a deep. The Python code will need to use the function name and pass the dictionary with the required details. Once the function is called, the result will be in JSON format, which I should parse to show the availability information."}, {"role": "tool", "content": "{\"name\": \"is_hotel_available\", \"content\": {\"hotel\": \"Queens Hotel\", \"city\": \"Berlin, Germany\", \"checkin\": \"2022-03-15\", \"checkout\": \"2022-03-20\", \"availability\": \"Available\", \"rooms_left\": 5, \"price_per_night\": 189, \"total_price\": 945, \"cancellation_policy\": \"Flexible until 5 days before arrival\", \"special_offers\": []}}]"}]', 'tools_json': '[{"type": "function", "function": {"name": "is_hotel_available", "description": "Checks the availability of a hotel for a given date range.", "parameters": {"type": "object", "properties": {"hotel": {"type": "string", "description": "The name of the hotel."}, "city": {"type": "string", "description": "The city where the hotel is located."}, "checkin": {"type": "string", "description": "The check-in date in the format \"YYYY-MM-DD\"."}, "checkout": {"type": "string", "description": "The check-out date in the format \"YYYY-MM-DD\"."}}, "required": ["hotel", "city", "checkin", "checkout"]}}]'}
```

## Step 2.数据格式转化

有了数据集之后，我们还需要将数据集转化为gpt-oss能够识别的数据格式，首先第一步是需要将其转化为gpt-oss的基本消息格式：

```
ds_text = dataset.map(render_batch, batched=True)
```

`ds_text`

```
Dataset({
    features: ['messages_json', 'tools_json', 'text'],
    num_rows: 200
})
```

其中 `text` 字段中就保存着可以用于微调的完整文本结构，其中 `assistant` 之前是输入，之后是输出：

```
print(ds_text[0]['text'])
```
