
# 图片 OCR 增强脚本

## 代码实现

```python
import os, re, io, base64, requests, json
from PIL import Image

DEFAULT_PROMPT = (
    "You are an OCR & document understanding assistant.\n"
    "Analyze this image region and produce:\n"
    "1) ALT: a very short alt text (<=12 words).\n"
    "2) CAPTION: a 1-2 sentence concise caption.\n"
    "3) CONTENT_MD: if the image contains a table, output a clean Markdown table;"
    "   if it contains a formula, output LaTeX ($...$ or $$...$$);"
    "   otherwise provide 3-6 bullet points summarizing key content, in Markdown.\n"
    "Return strictly in the following format:\n"
    "ALT: <short alt>\n"
    "CAPTION: <one or two sentences>\n"
    "CONTENT_MD:\n"
    "<markdown content here>\n"
)

IMG_PATTERN = re.compile(r'!\[[^\]]*\]\(([^)]+)\)')

def call_deepseek-ocr_image(vllm_url, model, img_path,
                      temperature=0.2, max_tokens=2048,
                      prompt=DEFAULT_PROMPT):
    """调用 vLLM(deepseek-ocr) 进行图片解析，返回 {alt, caption, content_md}"""
    with Image.open(img_path) as im:
        bio = io.BytesIO()
        im.save(bio, format="PNG")
        img_bytes = bio.getvalue()

    payload = {
        "model": model,
        "messages": [{
            "role": "user",
            "content": [
                {"type": "text", "text": prompt},
                {"type": "image_url",
                 "image_url": {"url": f"data:image/png;base64,{base64.b64encode(img_bytes).decode()}", "detail": "auto"}}
            ]
        }],
        "temperature": temperature,
        "max_tokens": max_tokens,
    }
    r = requests.post(vllm_url, json=payload, timeout=180)
    r.raise_for_status()
    text = r.json()["choices"][0]["message"]["content"].strip()

    # 解析返回
    alt, caption, content_md_lines = "", "", []
    mode = None
    for line in text.splitlines():
        l = line.strip()
        if l.upper().startswith("ALT:"):
            alt = l.split(":", 1)[1].strip()
            mode = None
        elif l.upper().startswith("CAPTION:"):
            caption = l.split(":", 1)[1].strip()
            mode = None
        elif l.upper().startswith("CONTENT_MD:"):
            mode = "content"
        else:
            if mode == "content":
                content_md_lines.append(line.rstrip())

    return {
        "alt": alt or "Figure",
        "caption": caption or alt or "",
        "content_md": "\n".join(content_md_lines).strip()
    }

def augment_markdown(md_path, out_path,
                     vllm_url="http://localhost:8001/v1/chat/completions",
                     model="deepseek-ocr",
                     temperature=0.2, max_tokens=2048,
                     image_root=".",
                     cache_json=None):
    with open(md_path, "r", encoding="utf-8") as f:
        md_lines = f.read().splitlines()

    cache = {}
    if cache_json and os.path.exists(cache_json):
        try:
            cache = json.load(open(cache_json, "r", encoding="utf-8"))
        except Exception:
            cache = {}

    out_lines = []
    for line in md_lines:
        out_lines.append(line)
        m = IMG_PATTERN.search(line)
        if not m:
            continue

        img_rel = m.group(1).strip().split("?")[0]
        img_path = img_rel if os.path.isabs(img_rel) else os.path.join(image_root, img_rel)

        if not os.path.exists(img_path):
            out_lines.append(f"<!-- WARN: image not found: {img_rel} -->")
            continue

        if cache_json and img_path in cache:
            result = cache[img_path]
        else:
            result = call_deepseek-ocr_image(vllm_url, model, img_path,
                                       temperature, max_tokens)
            if cache_json:
                cache[img_path] = result

        alt, cap, body = result["alt"], result["caption"], result["content_md"]

        if cap:
            out_lines.append(f"*{cap}*")
        if body:
            out_lines.append("<details><summary>解析</summary>\n")
            out_lines.append(body)
            out_lines.append("\n</details>")

    with open(out_path, "w", encoding="utf-8") as f:
        f.write("\n".join(out_lines))

    if cache_json:
        with open(cache_json, "w", encoding="utf-8") as f:
            json.dump(cache, f, ensure_ascii=False, indent=2)

    print(f"✅ 已写入增强后的 Markdown：{out_path}")

augment_markdown(
    md_path="output.md",                     # 第一步生成的 md
    out_path="output_augmented.md",          # 增强后的 md
    vllm_url="http://localhost:8001/v1/chat/completions",  # 你的 vLLM 服务
    model="deepseek-ocr",
    image_root=".",                          # 图片路径相对根目录
    cache_json="image_cache.json"            # 可选，缓存文件
)
```

## 实现效果对比

![](img/37e259a59869cab309f614d3e04b4161.png)

由此，便可实现更高精度的视觉检索。

## 课程信息

《大模型Agent开发实战》正课详情扫码添加助教，回复“大模型”，即可领取课程大纲&查看课程详情👇

![](img/d73d5fc7525f2cc423195634c74c3333.png)
