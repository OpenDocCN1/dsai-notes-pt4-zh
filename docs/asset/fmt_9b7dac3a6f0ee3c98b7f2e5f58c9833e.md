
# Agent Skills 实战与 MCP 对比

## Python 截图脚本

```python
import argparse
import json
import logging
import os
import re
import subprocess
from pathlib import Path
from typing import Iterable, List, Optional, Tuple


def write_note(filename: str, markdown: str) -> Path:
    path = Path(f"output/{filename}.md")
    path.parent.mkdir(parents=True, exist_ok=True)
    path.write_text(markdown, encoding="utf-8")
    logging.info("Saved note backup: %s", path)
    return path


# --- 截图 ----------


def extract_screenshot_markers(markdown: str) -> List[Tuple[str, int]]:
    """
    找到 Screenshot-00:03:12 或 Screenshot-[00:03:12] 形式的标记。
    """
    pattern = r"(?:\*?)Screenshot-(?:\[(\d{2}):(\d{2}):(\d{2})\]|(\d{2}):(\d{2}):(\d{2}))"
    results: List[Tuple[str, int]] = []
    for match in re.finditer(pattern, markdown):
        hh = match.group(1) or match.group(4)
        mm = match.group(2) or match.group(5)
        ss = match.group(3) or match.group(6)
        total_seconds = int(mm) * 60 + int(ss)
        results.append((match.group(0), total_seconds))
    return results


def generate_screenshot(video_path: Path, output_dir: Path, timestamp: int) -> Path:
    """
    调用 ffmpeg 截图，返回图片路径。
    """
    output_dir.mkdir(parents=True, exist_ok=True)
    mm = timestamp // 60
    ss = timestamp % 60
    filename = f"screenshot_{mm:02d}_{ss:02d}.jpg"
    output_path = output_dir / filename
    cmd = [
        "ffmpeg", "-ss", str(timestamp), "-i", str(video_path),
        "-frames:v", "1", "-q:v", "2", str(output_path), "-y",
    ]
    logging.info("生成截图：time=%s, file=%s", timestamp, output_path)
    subprocess.run(cmd, check=False, capture_output=True)
    return output_path


def replace_screenshots(
    markdown: str,
    video_path: Optional[Path],
    output_dir: Path,
    image_base_url: str,
) -> str:
    """
    将 Screenshot 标记替换为实际图片链接；没有视频时保持原样。
    返回修改后的 markdown。
    """
    if not video_path:
        logging.info("未提供视频文件，保留 Screenshot 标记不变。")
        return markdown
    matches = extract_screenshot_markers(markdown)
    for idx, (marker, ts) in enumerate(matches):
        try:
            img_path = generate_screenshot(video_path, output_dir, ts)
            url = f"{image_base_url.rstrip('/')}/{img_path.name}"
            logging.warning("生成截图失败（%s）：%s", marker, exc)
            return markdown
        except Exception as exc:
            logging.warning("生成截图失败（%s）：%s", marker, exc)
            return markdown


def main():
    logging.basicConfig(
        level=logging.INFO, format="%(asctime)s [%(levelname)s] %(message)s",
    )
    parser = argparse.ArgumentParser(description="将 SRT/视频转换为 Markdown 笔记")
    args = parser.parse_args()
    logging.info("环境变量加载完成。")
    # 从当前工作目录自动查找对应的 SRT 和 MP4 文件
    cwd = Path.cwd()
    md_path: Path
    video_path: Path
    # 附件文件夹
    image_base_url = "assets"
    screenshots_path = Path(f"output/{image_base_url}")
    md_candidates = sorted(cwd.glob("*.md"))
    if not md_candidates:
        raise RuntimeError("当前目录没有找到任何 .md 文件，请把字幕文件夹导出并且放到根目录下。")
    if len(md_candidates) > 1:
        logging.warning("当前目录中发现多个 .srt，将使用第一个：%s", md_candidates[0])
    md_path = md_candidates[0]
    mp4_candidates = sorted(cwd.glob("*.mp4"))
    if mp4_candidates:
        preferred: Optional[Path] = None
        video_path = preferred or mp4_candidates[0]
        if len(mp4_candidates) > 1 and preferred is None:
            logging.warning("当前目录中发现多个 .mp4，将使用第一个：%s", video_path)
    else:
        logging.info("当前目录未发现 .mp4，将保留 Markdown 中的 Screenshot 标记不变。")
        video_path = None
    # 读取 markdown 文件内容
    markdown = md_path.read_text(encoding="utf-8")
    logging.info("读取 Markdown 文件：%s", md_path)
    processed_md = replace_screenshots(markdown, video_path=video_path, output_dir=screenshots_path, image_base_url=image_base_url)
    logging.info("截图处理完成")
    # 备份替换后的 Markdown 内容
    output = write_note("note_processed", processed_md)
    info = {
        "output": str(output),
        "screenshots_dir": str(screenshots_path),
        "video_used_for_screenshot": str(video_path)
    }
    print(json.dumps(info, ensure_ascii=False, indent=2))


if __name__ == "__main__":
    main()
```

## 在 Skills 中使用截图脚本

我们在“字幕转 `markdown`”的这个 `Skills` 目录下面，再新建一个子文件夹，叫做 `scripts`，把我们的脚本放到这个子文件夹里面。准备好了截图脚本以后，我们再把它的 `Skills` 完善一下。

我们在指令里面加入第七条：“后续处理：`markdown` 生成完毕以后，调用这个 `scripts` 里面截图的这个 `Python` 脚本，对视频进行截图。”

![](img/a4d131ae982c96f38a30f6730483c7b5.png)

本期视频创建的所有 `Skills`，还有这个 `Python` 脚本的代码，我都放到了这个 `Github` 仓库里面。

[tech-shrimp/agent-skills-examples](https://github.com/tech-shrimp/agent-skills-examples)

我们再来测试一下。我把字幕文件对应的视频也放到项目的目录里面来，然后打开 `Claude Code`，还是把字幕文件拖拽进 `Claude Code`。开始，`Claude Code` 还是先调用了“字幕转 `markdown`”这个 `Skill`，生成了一份带截图标记的笔记文件。我们继续，然后 `Claude Code` 调用 `Python` 脚本对视频进行了截图。我们注意到，`Agent Skills` 只是执行了这个 `Python` 脚本，脚本里面的代码是不作为上下文传递给 AI 的，这样可以在最大程度上降低 `Skills` 的上下文使用量。最后，生成了一个含图片的 `markdown` 文件，这样一个带图片的 `markdown` 笔记就生成成功了，效果不错。

![](img/17c92703a8555d7829198c54371c1c88.png)

## 资源层例子2

我们再来看一个例子。这里我再新建一个文件夹，定义一个全新的 `Skill`，叫做“帮我写作”。还是新建一个 `skill.md` 文件。这个 `Skill` 的功能是结合用户提供的材料进行文案写作。不过这里有一点特殊，如果提供的材料是软件、`AI`、计算机相关的，就需要先阅读范文，然后学习我的写作风格。这里我在这个 `Skills` 里面再新建一个子文件夹，叫做 `references`，在这里面可以存放相关的补充文档。这里我就把刚才生成的那个 `markdown` 笔记文件拖拽进来，把它当做我语言风格的范文。

![](img/9ee78487918fa29917d4f4ddf286df14.png)

这里我找了一个开源项目，我把它首页上这些零碎的资料都粘贴到 `Claude Code` 里面，然后我要求 `Claude Code` 帮我写作。`Claude Code` 调用了我们刚才定义的 `Skill`，然后他意识到这是一个 `AI` 相关的文章（材料），所以他要阅读范文，学习我的行文风格。

![](img/037451e3187a56f9afbfafb77f6c91a5.png)

接下来他找到了这篇范文，学到了里面的行文风格。最后，输出了一个跟我风格相近的深度文案。我们回车把它保存下来，这样任务就完成了。

我们再来分析一下它的调用过程。在这个例子里，前面部分的调用过程跟之前是一样的。当 `AI` 选择了一个 `Skill` 以后，`Claude Code` 把 `Skill` 的指令细节发送给 `AI`。由于用户提供的材料是软件相关的，`AI` 根据指令意识到需要读取 `references` 目录里面的范文，学习行文风格。接下来 `AI` 调用了 `Read` 方法，读取了范文内容。这里体现的是资源层的按需加载，也就是渐进式披露。`AI` 按照指令的需要，只把它认为必要的文档加载进上下文。最后，综合以上所有的信息，给出了最终回答。

![](img/1bd97a2d7250e406fcca6443c5245cdf.png)

## Agent Skills 与 MCP

我们先来对比一下 `Agent Skills` 与 `MCP`，它们两个有重叠的功能，但是侧重点不同。`Skills` 的重点在于提示词，它的关键机制就是提示词的渐进式披露。而 `MCP` 的重点在于工具调用。`Skills` 更像是一个带目录的说明书，而 `MCP` 更像是一个标准化的工具箱。

![](img/1e0a800e29a29c3bedbc73bcb922c9d3.png)

`Skills` 是按需加载提示词，`MCP` 则是把提示词一次性塞入上下文，所以 `Skills` 的 `Token` 消耗小于 `MCP`。`Skills` 的主体是一个 `markdown` 文本，执行脚本则是它的辅助功能。不过这里面有一个坑，因为每台电脑的 `Python` 环境都不一样，所以 `Skills` 脚本执行失败的概率其实是比较高的。`MCP` 的主体是一个 `Node.js` 或者 `Python` 编写的软件包，它的执行成功率肯定是要好于 `Skills`。`Skills` 编写难度较小，只需要编写一个 `markdown` 文件就可以使用基础功能。而 `MCP Server` 编写难度较大，需要搭建完整的开发环境。

![](img/6038d7175407f511222a7a7a5bcdfc28.png)

所以，我觉得两种方案其实都算不上完美。如果 `MCP` 能够吸收 `Skills` 渐进式披露提示词，还有编写简单的优点，推出一个 2.0 版本，我觉得可能会是以后一个很好的发展方向。

## Agent Skills 与 MCP 协同工作

接下来我们用一个实战案例来看一下 `Agent Skills` 跟 `MCP` 是怎么配合工作的。我们先把一个 `MCP Server` 配置到 `Claude Code` 里面。这里我们就以 `Github MCP Server` 为例，在下面有对应的安装方式。

![](img/698618dbebd69bbbee3073ef45aeecf3.png)

这里我选到 `Claude`，它让我们执行这个命令。

![](img/a3929f572cadef7846dcf2fc3ca11ddb.png)

还是来到 `Claude Code` 这边的项目目录，把命令粘贴过来。这里需要填写一个 `Github API Key`。我们来到上面，点击这个链接，在这个页面来创建 `API Key`。 注意在下面把仓库权限改成所有仓库。在下面的 `permissions` 里面，我们找到这个管理员的，还有一个内容的，我们把这两个权限都弄成读写。

![](img/1eb9dbe763eef99be988e1f46c064bce.png)

然后点击创建，把这个 `API Key` 复制下来，填写到命令里面，这样回车，我们 `Github` 的 `MCP Server` 就装完成了。`Skills` 搭配 `MCP`，重要的是发挥他们各自擅长的能力。比如 `Skills` 擅长管理提示词，所以我们把提示词就放到 `Skills` 里面。这里是“帮我写作” `Skill`，我在下面添加一段：“后续处理：你先检查我名下有没有叫做 `ai-docs` 的 `Github` 仓库，如果没有，就用 `create repository` 这个 `mcp` 工具来创建一个仓库。用 `create or update file` 这个工具上传到我的 `Github` 仓库里面。”

![](img/fb3920e397a8398799afd1325ad2fd0e.png)
