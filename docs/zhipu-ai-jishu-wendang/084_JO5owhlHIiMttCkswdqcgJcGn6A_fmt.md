
# 增强 CogVideoX 模型的提示

> 来源：[https://ai.feishu.cn/docx/OeY9deh4ioBqD9xOcFqcNBRfnUe](https://ai.feishu.cn/docx/OeY9deh4ioBqD9xOcFqcNBRfnUe)

该脚本旨在增强 `CogVideoX` 模型的提示，将用户输入转化为更详细、描述性更强的提示，以便该模型用来生成更高质量的视频。该脚本利用 `OpenAI` 的应用程序接口来完善和扩展用户提供的提示，使其更适合视频生成任务。

- 提示细化：`CogVideoX` 模型得益于详细的提示，将用户的基本输入细化为更详细、更生动的描述可以提高生成视频的质量。
- OpenAI 集成：与 `OpenAI` 类 的 API 交互，自动生成这些增强型提示。

此步骤并非必须的，即使不使用此脚本完善提示，模型仍可正常运行。请在运行此脚本前将 `OPENAI_API_KEY` 和 `OPENAI_BASE_URL` （如需要）环境变量设置为 OpenAI API 密钥。

## 调用 GLM-4等大模型模型进行提示词润色

```
python convert_demo.py --prompt "A girl is riding a bike."
```

## 预定义的系统 Prompt 设定

预定义设置模型行为的 `prompt`，其指导模型如何将用户的输入提示转换或完善为高度描述性的详细视频描述。

```
import argparse

from openai import OpenAI

sys_prompt = """You are part of a team of bots that creates videos. You work with an assistant bot that will draw anything you say in square brackets.

For example , outputting " a beautiful morning in the woods with the sun peaking through the trees " will trigger your partner bot to output an video of a forest morning , as described. You will be prompted by people looking to create detailed , amazing videos. The way to accomplish this is to take their short prompts and make them extremely detailed and descriptive.
There are a few rules to follow:

You will only ever output a single video description per user request.

When modifications are requested , you should not simply make the description longer . You should refactor the entire description to integrate the suggestions.
Other times the user will not want modifications , but instead want a new image . In this case , you should ignore your previous conversation with the user.

Video descriptions must have the same num of words as examples below. Extra words will be ignored.
"""
```

## 转换 Prompt

输入用户的基本 `Prompt`，使用 `OpenAI` API 将其转换为适合视频生成的更详细的描述性格式。根据预定义的 `prompt` 以写好的几轮对话的例子，模型最终可以给出更加适合视频生成的 `prompt`。

此脚本已经验证过 `GLM-4-0520` 和 `GPT-4o` 两个模型。

```
def convert_prompt(prompt: str, retry_times: int = 3):
    """
    Convert a prompt to a format that can be used by the model for inference
    """

    client = OpenAI()
    text = prompt.strip()

    for i in range(retry_times):
        response = client.chat.completions.create(
            messages=[
                {"role": "system", "content": f"{sys_prompt}"},
                {
                    "role": "user",
                    "content": 'Create an imaginative video descriptive caption or modify an earlier caption for the user input : " a girl is on the beach"',
                },
                {
                    "role": "assistant",
                    "content": "A radiant woman stands on a deserted beach, arms outstretched, wearing a beige trench coat, white blouse, light blue jeans, and chic boots, against a backdrop of soft sky and sea. Moments later, she is seen mid-twirl, arms exuberant, with the lighting suggesting dawn or dusk. Then, she runs along the beach, her attire complemented by an off-white scarf and black ankle boots, the tranquil sea behind her. Finally, she holds a paper airplane, her pose reflecting joy and freedom, with the ocean's gentle waves and the sky's soft pastel hues enhancing the serene ambiance.",
                },
                {
                    "role": "user",
                    "content": 'Create an imaginative video descriptive caption or modify an earlier caption for the user input : " A man jogging on a football field"',
                },
                {
                    "role": "assistant",
                    "content": "A determined man in athletic attire, including a blue long-sleeve shirt, black shorts, and blue socks, jogs around a snow-covered soccer field, showcasing his solitary exercise in a quiet, overcast setting. His long dreadlocks, focused expression, and the serene winter backdrop highlight his dedication to fitness. As he moves, his attire, consisting of a blue sports sweatshirt, black athletic pants, gloves, and sneakers, grips the snowy ground. He is seen running past a chain-link fence enclosing the playground area, with a basketball hoop and children's slide, suggesting a moment of solitary exercise amidst the empty field.",
                },
                {
                    "role": "user",
                    "content": 'Create an imaginative video descriptive caption or modify an earlier caption for the user input : " A woman is dancing, HD footage, close-up"',
                },
                {
                    "role": "assistant",
                    "content": "A young woman with her hair in an updo and wearing a teal hoodie stands against a light backdrop, initially looking over her shoulder with a contemplative expression. She then confidently makes a subtle dance move, suggesting rhythm and movement. Next, she appears poised and focused, looking directly at the camera. Her expression shifts to one of introspection as she gazes downward slightly. Finally, she dances with confidence, her left hand over her heart, symbolizing a poignant moment, all while dressed in the same teal hoodie against a plain, light-colored background.",
                },
                {
                    "role": "user",
                    "content": f'Create an imaginative video descriptive caption or modify an earlier caption in ENGLISH for the user input: " {text} "',
                },
            ],
            model="glm-4-0520",  # glm-4-0520 and gpt-4o have be tested
            temperature=0.01,
            top_p=0.7,
            stream=False,
            max_tokens=250,
        )
        if response.choices:
            return response.choices[0].message.content
    return prompt
```

## 解析 CLI 参数和函数调用

### 参数

`prompt`：用户提供的需要增强的原始提示。

`retry_times`：如果第一次尝试没有得到满意的结果，API 调用的重试次数。

脚本获取用户提示，将其传递给 `convert_prompt` 函数，然后打印改进后的提示。

```
if __name__ == "__main__":
    parser = argparse.ArgumentParser()
    parser.add_argument("--prompt", type=str, required=True, help="Prompt to convert")
    parser.add_argument("--retry_times", type=int, default=3, help="Number of times to retry the conversion")
    args = parser.parse_args()

    converted_prompt = convert_prompt(args.prompt, args.retry_times)
    print(converted_prompt)
```
