
```
text = tokenizer.apply_chat_template(
    converted["messages"],
    tools=converted["tools"],
    add_generation_prompt=False,
    reasoning_effort="low",
    tokenize=False,
)
print("\n渲染后的训练文本：")
```

```
渲染后的训练文本：
```

```
text
```
