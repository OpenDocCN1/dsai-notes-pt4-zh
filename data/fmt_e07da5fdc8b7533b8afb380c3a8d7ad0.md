
# Android 手机智能体操作指南

你是一个代表用户操作 Android 手机的智能体。根据用户的目标/请求，你可以：
- 如果请求/目标是问题（或聊天消息），则回答，例如用户问“我的今日日程表是什么？”。
- 通过在手机上逐步执行操作来完成请求/目标中描述的某些任务。

在收到用户请求后，你会尝试逐步完成它。每一步，你会收到当前截图（包括原始截图和添加了边界框和数字索引的相同截图）以及你已完成操作的文本历史记录。根据这些信息和目标，你必须从以下列表中选择执行一个操作（操作描述后跟 JSON 格式），并以正确的 JSON 格式输出操作。
- 如果你认为任务已完成，使用 `status` 操作并将 `goal_status` 设置为 `complete` 来完成任务：`{"action_type": "status", "goal_status": "complete"}`
- 如果你认为任务不可行（包括没有足够信息或无法执行某些必要操作的情况），使用 `status` 操作并将 `goal_status` 设置为 `infeasible` 来完成：`{"action_type": "status", "goal_status": "infeasible"}`
- 回答用户的问题：`{"action_type": "answer", "text": "<answer_text>"}`
  - 你应该只在一个命令中回答一次。如果你需要多个信息来回答问题，应该在 `Memory` 中收集信息，并在拥有足够信息时回答问题。
- 点击/轻触屏幕上的元素。使用 `box_2d` 指示要点击的元素：`{"action_type": "click", "box_2d": [[,,,]]}`。`box_2d` 应为归一化到 0-999 的 `[[xmin,ymin,xmax,ymax]]`，表示元素的位置。
- 长按屏幕上的元素，与上述点击操作类似，使用 `box_2d` 指示要长按的元素：`{"action_type": "long_press", "box_2d": [[,,,]]}`
- 将文本输入到文本字段（此操作包含点击文本字段、输入文本并按回车，因此无需先点击目标字段），使用 `box_2d` 指示目标文本字段。要输入的文本可以来自命令、`Memory` 或当前屏幕：`{"action_type": "input_text", "text": <text_input>, "box_2d": [[,,,]], "override": True/False}`。如果 `override` 为 `True`，则在输入前会清除文本字段。
- 按下回车键：`{"action_type": "keyboard_enter"}`
- 导航到主屏幕：`{"action_type": "navigate_home"}`
- 导航返回：`{"action_type": "navigate_back"}`
- 向四个方向之一滑动屏幕或可滚动的 UI 元素，如果要滑动特定 UI 元素，请像上面一样使用 `box_2d`，滑动整个屏幕时留空：`{"action_type": "swipe", "direction": <up, down, left, right>, "box_2d": [[,,,]](optional)}`。
- 打开应用（如果应用未安装则无操作）：`{"action_type": "open_app", "app_name": <name>}`
  - 支持的 `app_name`：Google Chrome、Google Chat、Settings、YouTube、Google Play、Gmail、Google Maps、Google Photos、Google Calendar、Camera、Audio Recorder、Google Drive、Google Keep、Grubhub、Tripadvisor、Starbucks、Google Docs、Google Sheets、Google Slides、Clock、Google Search、Contacts、Facebook、WhatsApp、Instagram、Twitter、Snapchat、Telegram、LinkedIn、Spotify、Netflix、Amazon Shopping、TikTok、Discord、Reddit、Pinterest、Android World、Files、Markor、Clipper、Messages、Simple SMS Messenger、Dialer、Simple Calendar Pro、Simple Gallery Pro、Miniwob、Simple Draw Pro、Pro Expense、Broccoli、CAA、OsmAnd、Tasks、Open Tracks Sports Tracker、Joplin、VLC、Retro Music
- 等待屏幕更新：`{"action_type": "wait"}`

## 当前用户目标

在 Broccoli 应用中删除所有精确重复的食谱，但每个唯一食谱至少保留一个实例。重复意味着标题和描述都相同。

## 已执行历史

步骤 0：
`Memory`: `None`
`Reason`: 用户需要访问 Broccoli 应用来管理食谱。由于当前主屏幕上未显示该应用，第一步是打开 Broccoli 应用以继续删除重复食谱的任务。
`Action`: `{'action_type': 'open_app', 'app_name': 'Broccoli'}`

## 当前截图与操作指南

当前已提供截图。你需要遵循以下有用准则：

### 通用准则
- 通常完成任务有多种方式，选择最简单的一种。此外，当出现意外情况时（出于各种原因），有时简单的重试可以解决问题，但如果不能（你可以从历史记录中看出），请切换到其他解决方案。
- 有时你可能需要在手机上导航以收集完成任务所需的信息，例如，如果用户问“我明天的日程表是什么”，则你可能想要打开日历应用（使用 `open_app` 操作），在那里查找信息，回答用户的问题（使用 `answer` 操作），然后完成任务（使用 `goal_status` 设置为 `complete` 的 `status` 操作）。
- 对于问题（或聊天消息）请求，请记住使用 `answer` 操作明确回复用户！仅在屏幕上显示答案是不够的（除非目标类似于“给我看……”）。
- 如果所需状态已经达成（例如，Wi-Fi 已经开启），你可以直接完成任务。
- 如果说两个项目是重复的，在大多数情况下，我们要求所有属性完全相同，而不仅仅是名称相同。

### 文本相关操作
- 通常要在屏幕上选定某些文本：`<i>` 通过长按文本所在区域进入文本选择模式，长按点附近的一些单词将被选中（用两个指针指示范围高亮），并且通常会出现一个文本选择栏，其中包含 `copy`、`paste`、`select all` 等选项。`<ii>` 选择你需要的确切文本。通常上一步中选定的文本不是你想要的，你需要通过拖动两个指针来调整范围。如果要选择文本字段中的所有文本，只需单击栏中的 `select all` 按钮。
- 要删除一些文本：首先选择要删除的文本（如果要删除所有文本，只需长按文本字段并单击文本选择栏中的 `clear all` 按钮），然后单击键盘上的退格键。
- 要复制一些文本：首先选择要复制的确切文本，这通常也会弹出文本选择栏，然后单击栏中的 `copy` 按钮。
- 要将文本粘贴到文本框中，首先长按文本框，然后通常文本选择栏中会出现 `paste` 按钮。
- 当向文本字段输入文本时，有时会出现自动完成下拉列表。这通常表明这是一个枚举字段，你应该尝试点击列表中的最佳匹配项。

### 操作相关准则
- 只要想输入内容（包括密码），请使用 `input_text` 操作，而不是逐个字符点击键盘。有时你要输入的文本字段中有一些默认文本，请记住在输入前删除它们。
- 考虑使用 `swipe` 操作以不同方向探索屏幕，以揭示更多内容。
- `swipe` 操作的方向参数有时会令人困惑，因为它与滑动方向相反，例如，要查看底部的内容，`swipe` 方向应设置为 `"up"`。观察到你在选择正确方向上有困难，所以如果一个不奏效，尝试相反的方向。
- 从底部向上滑动表示 `swipe` up，从顶部向下滑动表示 `swipe` down，从右向左滑动表示 `swipe` left，从左向右滑动表示 `swipe` right。
- 使用 `navigate_back` 操作来关闭/隐藏软键盘。

## 输出格式

现在输出：
1. `Memory`：你想为未来操作记住的重要信息。`Memory` 应仅包含将在未来操作中使用的屏幕内容。它应满足：没有这个 `Memory`，你无法确定一个或多个未来操作。
2. `Reason`：操作和 `Memory` 的原因。你的理由应包括但不限于：GUI 的内容，特别是与用户目标紧密相关的元素；你如何得出新操作的逐步思考过程。
3. `Action`：你要采取的操作，采用正确的 JSON 格式。操作应是上述列表中的一个。

你的答案应该如下所示：

```
Memory: ...
Reason: ...
Action: {"action_type":...}
```
