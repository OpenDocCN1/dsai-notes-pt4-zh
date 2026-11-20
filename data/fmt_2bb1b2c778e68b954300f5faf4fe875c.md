
# Temp 0412

> 来源：[https://zsyhjtnsa5.feishu.cn/docx/LVVDdyXqyolHYIxl5bDcklrsn1d](https://zsyhjtnsa5.feishu.cn/docx/LVVDdyXqyolHYIxl5bDcklrsn1d)
>
> 有任何问题可以直接询问LangChain Chatbot

```python
from langchain_openai import ChatOpenAI

llm = ChatOpenAI(api_key="sk-t3D6Hy6n6NNMY8kt37FaD528694e47798e732e37C8541485", base_url="https://api.huaqloud.com/v1")
# Enter an interactive loop
while True:
    user_input = input("You: ")
    if user_input.lower() == "exit":
        break
    try:
        response = llm.invoke(user_input)
        print(f"LLM: {response}")
    except Exception as e:
        print(f"Error: {e}")
```
