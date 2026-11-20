
## 文本+图像

```python
from openai import OpenAI

openai_api_key = "EMPTY"
openai_api_base = "http://127.0.0.1:8000/v1"
client = OpenAI(
    api_key=openai_api_key,
    base_url=openai_api_base,
)

response = client.chat.completions.create(
    model="glm-4.5v",
    messages=[
        {
            "role": "user",
            "content": [
                {"type": "text", "text": "请比较这两张图片的主要不同点"},
                {"type": "image_url", "image_url": {"url": "file:///your_img1"}},
                {"type": "image_url", "image_url": {"url": "file:///your_img2"}}
            ]
        }
    ],
    max_tokens=512,
    temperature=0.0,
)
print(response.choices[0].message.content.strip())
reasoning_content = getattr(response.choices[0].message, "reasoning_content", None)
print("======")
print(reasoning_content.strip() if reasoning_content else "No reasoning_content field in response.")
```

## 文本+视频

```python
from openai import OpenAI

openai_api_key = "EMPTY"
openai_api_base = "http://127.0.0.1:8000/v1"
client = OpenAI(
    api_key=openai_api_key,
    base_url=openai_api_base,
)

response = client.chat.completions.create(
    model="glm-4.5v",
    messages=[
        {
            "role": "user",
            "content": [
                {"type": "text", "text": "请根据这个视频总结其主题"},
                {"type": "video_url", "video_url": {"url": "file:///your_video"}}
            ]
        }
    ],
    max_tokens=1024,
    temperature=0.0,
)
print(response.choices[0].message.content.strip())
reasoning_content = getattr(response.choices[0].message, "reasoning_content", None)
print("======")
print(reasoning_content.strip() if reasoning_content else "No reasoning_content field in response.")
```

## 引用论文

如果您使用了本模型，请引用以下论文：

```bibtex
@misc{glmvteam2025glm41vthinkingversatilemultimodalreasoning,
      title={GLM-4.1V-Thinking: Towards Versatile Multimodal Reasoning with Scalable Reinforcement Learning}, 
      author={GLM-V Team and Wenyi Hong and Wenmeng Yu and Xiaotao Gu and Guo Wang and Guobing Gan and Haomiao Tang and Jiale Cheng and Ji Qi and Junhui Ji and Lihang Pan and Shuaiqi Duan and Weihan Wang and Yan Wang and Yean Cheng and Zehai He and Zhe Su and Zhen Yang and Ziyang Pan and Aohan Zeng and Baoxu Wang and Boyan Shi and Changyu Pang and Chenhui Zhang and Da Yin and Fan Yang and Guoqing Chen and Jiazheng Xu and Jiali Chen and Jing Chen and Jinhao Chen and Jinghao Lin and Jinjiang Wang and Junjie Chen and Leqi Lei and Letian Gong and Leyi Pan and Mingzhi Zhang and Qinkai Zheng and Sheng Yang and Shi Zhong and Shiyu Huang and Shuyuan Zhao and Siyan Xue and Shangqin Tu and Shengbiao Meng and Tianshu Zhang and Tianwei Luo and Tianxiang Hao and Wenkai Li and Wei Jia and Xin Lyu and Xuancheng Huang and Yanling Wang and Yadong Xue and Yanfeng Wang and Yifan An and Yifan Du and Yiming Shi and Yiheng Huang and Yilin Niu and Yuan Wang and Yuanchang Yue and Yuchen Li and Yutao Zhang and Yuxuan Zhang and Zhanxiao Du and Zhenyu Hou and Zhao Xue and Zhengxiao Du and Zihan Wang and Peng Zhang and Debing Liu and Bin Xu and Juanzi Li and Minlie Huang and Yuxiao Dong and Jie Tang},
      year={2025},
      eprint={2507.01006},
      archivePrefix={arXiv},
      primaryClass={cs.CV},
      url={https://arxiv.org/abs/2507.01006}, 
}
```

## 🏠🛰️交流群

![](img/191c9f6fe8624b1da9fa722b4c961614.png)
