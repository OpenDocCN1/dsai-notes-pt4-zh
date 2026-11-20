
# DeepSeek-V3.1 Usage Guide-vLLM

> 来源：[https://gotoaiworld.feishu.cn/docx/IhoTd8OxNozUzWxpDn0cWscsnie](https://gotoaiworld.feishu.cn/docx/IhoTd8OxNozUzWxpDn0cWscsnie)

`https://docs.vllm.ai/projects/recipes/en/latest/DeepSeek/DeepSeek-V3_1.html#thinkingfalse`

## Introduction

[DeepSeek-V3.1](https://huggingface.co/deepseek-ai/DeepSeek-V3.1) is a hybrid model that supports both.choices[0].message.content
print("content:\n", content)
```

### Example Outputs

#### thinking=True

+   As shown below, the output results contain `</think>`

```
 Hmm, the user is asking which number is greater between 9.11 and 9.8. This seems straightforward, but I should be careful because decimals can sometimes confuse people. 
I recall that comparing decimals involves looking at each digit from left to right. Both numbers have the same whole number part (9), so I need to compare the decimal parts. 0.11 is greater than 0.8 because 0.11 is equivalent to 0.110 and 0.8 is 0.800, so 110 thousandths is greater than 800 thousandths? Wait no, that’s wrong. 
Actually, 0.8 is the same as 0.80, and 0.11 is less than 0.80. So 9.11 is actually less than 9.8. I should double-check that. Yes, 9.8 is larger because 0.8 > 0.11. 
I’ll explain it clearly by comparing the tenths place: 9.8 has 8 tenths, while 9.11 has 1 tenth and 1 hundredth, so 8 tenths is indeed larger. 
The answer is 9.8 is greater. I’ll state it confidently and offer further help if needed.</think>9.8 is greater than 9.11.  
To compare them:  
- 9.8 is equivalent to 9.80  
- 9.80 has 8 tenths, while 9.11 has only 1 tenth  
- Since 8 tenths (0.8) is greater than 1 tenth (0.1), 9.8 > 9.11  
Let me know if you need further clarification! 😊
```

#### thinking=False

```
 The number `9.11` is greater than `9.8`.  
To compare them:  
- 9.11 = 9 + 11/100  
- 9.8 = 9 + 80/100  
Since 11/100 (0.11) is less than 80/100 (0.80), 9.11 is actually smaller than 9.8. Wait, let me correct that:  
Actually, `9.8 is greater than 9.11`.  
- 9.8 can be thought of as 9.80  
- Comparing 9.80 and 9.11: 80 hundredths is greater than 11 hundredths.  
So, `9.8 > 9.11`.  
Apologies for the initial confusion! 😅
```

### curl Example

You can run the following `curl` command:

```
curl http://localhost:8000/v1/chat/completions \    -H "Content-Type: application/json" \    -d '{        "model": "ds31",        "messages": [            {                "role": "user",                "content": "9.11 and 9.8, which is greater?"            }        ],        "chat_template_kwargs": {            "thinking": true        }    }';
```
