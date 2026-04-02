# Temp 0412

> 来源：[https://zsyhjtnsa5.feishu.cn/docx/LVVDdyXqyolHYIxl5bDcklrsn1d](https://zsyhjtnsa5.feishu.cn/docx/LVVDdyXqyolHYIxl5bDcklrsn1d)

> 有任何问题可以直接询问LangChain Chatbot

```
from langchain_openai import ChatOpenAI

llm = ChatOpenAI(api_key=&quot;sk-t3D6Hy6n6NNMY8kt37FaD528694e47798e732e37C8541485&quot;, base_url=&quot;https://api.huaqloud.com/v1&quot;)
# Enter an interactive loop
while True:
    user_input = input(&quot;You: &quot;)
    if user_input.lower() == &quot;exit&quot;:
        break
    try:
        response = llm.invoke(user_input)
        print(f&quot;LLM: {response}&quot;)
    except Exception as e:
        print(f&quot;Error: {e}&quot;)
```