
# 剪映编辑器 Skill 的 Windows 便携整合包

> 来源：[https://yunyingmenghai.feishu.cn/docx/EkjUdJGG9ov56bxtzQcchDBqn2g](https://yunyingmenghai.feishu.cn/docx/EkjUdJGG9ov56bxtzQcchDBqn2g)

## 原项目

https://github.com/luoluoluo22/jianying-editor-skill

核心能力是让 AI 通过 Python 脚本全自动生成、编辑和导出剪映视频草稿。

它采用"文件注入驱动"策略——直接操作剪映底层的 `draft_content.json` 草稿数据，而非模拟 UI 点击，从而实现对时间轴、素材坐标、滤镜参数的精确控制。

## win整合包下载

https://pan.quark.cn/s/e69db8ea4494

## 使用方式

解压到无中文/空格的英文路径后，双击 `bat` 测试运行；

可让 AI Agent（Kimi Code / Cursor / Claude Code 等）读取 Skill 规则后，用自然语言生成剪辑脚本自动执行。

## 关键限制

+   仅支持 Windows 剪映专业版 5.9 及以下（自动导出功能在 6.0+ 弹窗干扰下不稳定）

+   不能替代剪映本身渲染，只是自动生成草稿时间轴

+   无法调用剪映 GPU 实时特效（如智能抠图、美颜、语音识别字幕）

+   不能触发剪映内置 AI 按钮（如"一键成片"）

## 主要功能

+   素材自动导入与裁剪排列

+   AI 配音 (剪映原生音色 / 微软 Edge TTS)

+   自动字幕生成与动画对齐

+   自动配乐 (本地 / 剪映云端曲库)

+   特效/转场/滤镜按名搜索应用

+   网页动效转视频: HTML/JS/Canvas 动画通过 `Playwright` 录屏导入

+   录屏 + 智能变焦: 屏幕录制后自动给鼠标点击处加缩放和红圈标记

+   影视解说自动化 (分析视频 → 生成分镜脚本 → 合成)

+   自动导出 (模拟鼠标键盘自动化，依赖剪映 5.9)

## 技术栈

+   Python 3.13 便携运行时 (`python_runtime/`) — 无需系统预装 Python

+   核心库: `pyJianYingDraft` (`vendor` 内置) + `uiautomation` + `playwright`

+   其他依赖: `pynput`, `edge-tts`, `pymediainfo`, `opencv-python`, `numpy`, `imageio`, `psutil`, `requests`, `websockets`

## 目录结构概览

| 目录 | 说明 |

| --- | --- |

| `python_runtime/` | 自带 Python 3.13 解释器及 DLL |

| `scripts/` | 核心 API：`jy_wrapper.py` (高层封装)、`core/` (`media/text/vfx/project_base`)、`utils/` (配置/格式化/日志/常量) |

| `examples/` | 10 个示例脚本，包括 `my_first_vlog.py` (示例 Vlog)、`robust_auto_export.py` (自动导出)、`web_to_video_intro_demo.py` (网页动效转视频) |

| `tools/` | 录屏与辅助工具 (`recording/recorder.py`)、仓库健康检查脚本 |

| `assets/` | 测试素材（`video.mp4`、`audio.mp3`、字幕 SRT、特效资源等） |

| `data/` | CSV 资源库（云端音乐、字体样式、转场、滤镜、TTS 发音人等） |

| `docs/` | API 文档、Agent Playbook、SOP |

| `*.bat` | Windows 一键脚本：环境诊断、启动示例 Vlog、重装依赖 |

## 核心架构

+   `JyProject` (`jianying-editor-portable/scripts/jy_wrapper.py:35`) 继承自 `JyProjectBase` + 多个 Mixins (`MediaOpsMixin`, `TextOpsMixin`, `VfxOpsMixin`, `MockingOpsMixin`)

+   `JyProjectBase` (`jianying-editor-portable/scripts/core/project_base.py:10`) 负责草稿创建/加载、安全路径处理、时间轴审计、剪映占用锁自动释放

+   剪辑流程示例 (`jianying-editor-portable/examples/my_first_vlog.py:11`):

1.  `JyProject("My_First_Vlog", overwrite=True)` 创建草稿

1.  `add_media_safe()` 导入视频到轨道

1.  `add_audio_safe()` 添加 BGM

1.  `add_text_simple()` 添加带打字机动画的标题

1.  `save()` 保存并注入云端素材 ID
