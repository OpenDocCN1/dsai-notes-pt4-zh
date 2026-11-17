
```
{ 'content': 'Netflix does not publicly release real-time lists of its top-rated series, but you can find updated information from third-party platforms and review sites. Here are some reliable sources to check for the latest top-rated shows:

### 1. IMDb Top TV Shows List

   - IMDb’s [Top Rated TV Shows](https://www.imdb.com/chart/tvmeter/) page ranks TV series by user ratings and popularity. Many Netflix originals appear here (e.g., The Bear, House of the Dragon, Squid Game).

### 2. Rotten Tomatoes Certified Fresh List

   - Rotten Tomatoes highlights shows with high critic and audience scores. Check their [Certified Fresh TV section](https://www.rottentomatoes.com/tv) for critically acclaimed series.

### 3. Netflix’s "Top 10" Feature

   - If you’re a Netflix subscriber, log in and check the "Top 10" section in your account (under "See What’s Trending"). This shows the most-watched shows in your region.

### 4. Recent Awards-Winning Series

   - Look for shows that have won major awards (e.g., Emmys, Golden Globes). For example:
     - The Bear (2023)
     - House of the Dragon (2022–present)
     - Squid Game (2021)
     - Stranger Things (2016–present)

### 5. Genre-Specific Recommendations

   - Use sites like [JustWatch](https://www.justwatch.com/) to filter Netflix shows by genre (e.g., drama, comedy, thriller) and sort by ratings.

### Why Ratings Vary

   - User ratings can differ across platforms, so cross-check multiple sources (IMDb, Rotten Tomatoes, Metacritic) for a balanced view.

For the most current and accurate list, check these platforms directly, as rankings and availability can change frequently. Let me know if you’d like help finding specific types of shows!',
  'role': 'assistant',
  'thinking': '
Okay, the user is asking for the top-rated series currently on Netflix. Let me start by recalling that Netflix doesn’t publicly release their current top-rated lists in real-time. However, there are other sources where users often check, like IMDb, Rotten Tomatoes, and other review aggregators.

First, I should mention that Netflix doesn’t publish real-time data, so I can’t provide an official list. But I can suggest where they might find this information. Maybe list some popular sources. Also, I should consider that the user might want to know what’s trending now, so mentioning that the list can change and advising them to check the latest data would be helpful.

I should also think about the types of series that are currently popular. For example, recent releases like "The Bear" or "House of the Dragon" might be on the list. But I need to be careful not to assume; instead, direct them to the sources where they can find up-to-date information.

Additionally, the user might not know about these third-party sites, so providing examples like IMDb’s Top TV Shows list or Rotten Tomatoes’ Certified Fresh list would be useful. Maybe also mention that they can check Netflix’s own "Top 10" feature if they’re a subscriber.

I should also consider if there are any recent awards or nominations that might influence the top-rated lists. For instance, shows that won Emmys or other awards might be highlighted. However, again, without real-time data, it’s better to point them to the sources rather than list specific shows.

Another angle is to mention that user ratings can vary, so it’s good to look at multiple sources for a well-rounded view. Also, some users might prefer different genres, so suggesting that they can filter by genre on those sites would be helpful.

Wait, but the user specifically asked for a list. Since I can’t provide a current list, I need to be clear that I can’t do that but can guide them on where to find it. I should also check if there’s any official Netflix page or feature they can use, like the "Top 10" section, which is available to subscribers.

I should also think about possible errors. For example, if the user is not a Netflix subscriber, they might not have access to certain features. So, I should mention that some features require a subscription.

In summary, the response should inform the user that Netflix doesn’t release real-time top-rated lists, suggest third-party sources like IMDb and Rotten Tomatoes, mention Netflix’s own "Top 10" feature, and advise them to check the latest data from these sources. Also, highlight that user ratings can vary and recommend checking multiple sources for accuracy.
'
}
```

## 数据集翻译

接下来我们创建如下函数进行翻译：

```
# -*- coding: utf-8 -*-

# ✅ 依赖：pip install datasets tqdm openai

from datasets import load_dataset

from tqdm import tqdm

from openai import OpenAI

import os, re, json, time, copy

from typing import Optional

from dotenv import load_dotenv

load_dotenv(override=True)
```

```
True
```

```
# ----------------------------
# 0) DeepSeek OpenAI风格客户端
# ----------------------------

sd_api_key = os.getenv("DEEPSEEK_API_KEY") or "<YOUR_DEEPSEEK_KEY>"

client = OpenAI(api_key=sd_api_key, base_url="https://api.deepseek.com")

def translate_fn(text: str, max_retries: int = 3, sleep_sec: float = 0.5) -> str:

    """
    使用 DeepSeek Chat 将文本翻译为中文。mask/unmask 会保护代码/公式/数字，所以这里直接翻译即可。

    """

    if not isinstance(text, str) or not text.strip():

        return text

    system_prompt = (

        "You are a strict translator. Only translate into Simplified Chinese. "

        "Do NOT answer the question, do NOT add content. "

        "Return ONLY the translation of the text between <<<TEXT>>> markers."

    )

    last_err = None

    for _ in range(max_retries):

        try:

            resp = client.chat.completions.create(

                model="deepseek-chat",

                messages=[

                    {"role": "system", "content": system_prompt},

                    {"role": "user", "content": text}

                ],

                temperature=0.2,

            )

            return resp.choices[0].message.content.strip()

        except Exception as e:

            last_err = e

            time.sleep(sleep_sec)

    print("⚠️ 翻译失败，已回退原文：", last_err)

    return text
```

```
# --------------------------------
# 1) 占位与还原：保护代码 / 公式 / 数字

# --------------------------------
```
