# GLM-4.1V-9B-Thinking GUI Agent 说明

> 来源：[https://ai.feishu.cn/docx/FjuDdGmyUoiYTPxbaPLc5ZxFnPd](https://ai.feishu.cn/docx/FjuDdGmyUoiYTPxbaPLc5ZxFnPd)

在这个文档中，我们说明在 GLM-4.1V-9B-Thinking 在手机、电脑和网页中发挥 GUI Agent 能力的最佳实践。遵循以下设定可以有效保证模型在对应榜单和环境下的表现。

## 手机环境

如果你想要测试GLM-4.1V-9B-Thinking在榜单AndroidWorld上的性能，请遵循这一节的说明。

### 动作空间

1.  status：表示当前任务已经结束，并给出执行状态。它包含一个参数：

+   goal_status。该参数有两个枚举值：complete 和 infeasible；前者表示任务正常完成，后者表示任务无法完成。

1.  answer：表示回答用户的问题。它包含一个参数：

+   text：表示回答的内容。

1.  click：表示单击界面上的某个位置。它包含一个参数：

+   box_2d：以[[xmin,ymin,xmax,ymax]]的形式表示点击的位置；坐标中的每一个数字都应该是 0-999 之间的整数，表示相对截图的宽（或者高）的千分比。

1.  long_press：表示单击界面上的某个位置。它和click使用相同的参数。

1.  input_text：表示先点击给定位置，然后再输入一段文本。它有如下三个参数：

+   text：表示被输入的文本内容。

+   box_2d：和click的box_2d参数有相同的形式和含义。表示文本框的位置。

+   override：布尔类型。这个参数为true，表示应当先清空文本框中的现有内容，然后进行文本输入；这个参数为false ，表示会在当前文本框的最后追加新内容。

1.  keyboard_enter：表示按下回车键。它没有参数。

1.  navigate_home：表示按下主屏幕键。它没有参数。

1.  navigate_back：表示按下返回键。它没有参数。

1.  swipe：表示手指滑动。它有以下两个参数。

+   direction：该参数有四个枚举值：up、down、left、right，表示手指滑动的方向。例如，up 表示手指在手机屏幕向上移动，对应于屏幕内容向下滚动。

+   box_2d：表示被滚动的区域。如果没有指明这个参数，表示进行全屏幕的滚动。

1.  open_app：根据应用名称启动应用。它有以下一个参数：

+   app_name：表示需要打开的应用名称。我们强烈建议在 prompt 中明确指定需要在哪个（些）应用中执行任务。例如，以下配置为AndroidWorld评测时的支持软件。

```
app_names = [&quot;Google Chrome&quot;, &quot;Google Chat&quot;, &quot;Settings&quot;, &quot;YouTube&quot;, &quot;Google Play&quot;, &quot;Gmail&quot;,&quot;Google Maps&quot;, &quot;Google Photos&quot;, &quot;Google Calendar&quot;, &quot;Camera&quot;, &quot;Audio Recorder&quot;,&quot;Google Drive&quot;, &quot;Google Keep&quot;, &quot;Grubhub&quot;, &quot;Tripadvisor&quot;, &quot;Starbucks&quot;, &quot;Google Docs&quot;,&quot;Google Sheets&quot;, &quot;Google Slides&quot;, &quot;Clock&quot;, &quot;Google Search&quot;, &quot;Contacts&quot;, &quot;Facebook&quot;,&quot;WhatsApp&quot;, &quot;Instagram&quot;, &quot;Twitter&quot;, &quot;Snapchat&quot;, &quot;Telegram&quot;, &quot;LinkedIn&quot;, &quot;Spotify&quot;,&quot;Netflix&quot;, &quot;Amazon Shopping&quot;, &quot;TikTok&quot;, &quot;Discord&quot;, &quot;Reddit&quot;, &quot;Pinterest&quot;,&quot;Android World&quot;, &quot;Files&quot;, &quot;Markor&quot;, &quot;Clipper&quot;, &quot;Messages&quot;, &quot;Simple SMS Messenger&quot;,&quot;Dialer&quot;, &quot;Simple Calendar Pro&quot;, &quot;Simple Gallery Pro&quot;, &quot;Miniwob&quot;, &quot;Simple Draw Pro&quot;,&quot;Pro Expense&quot;, &quot;Broccoli&quot;, &quot;CAA&quot;, &quot;OsmAnd&quot;, &quot;Tasks&quot;, &quot;Open Tracks Sports Tracker&quot;,&quot;Joplin&quot;, &quot;VLC&quot;, &quot;Retro Music&quot;]
```

1.  wait：表示等待界面加载。它没有参数。

### Prompt

下面是 prompt 的模板。其中#SUPPORTED_APPS#、#COMMAND#、#HISTORT_STEPS# 需要被替换成实际的内容。在调用模型的时候，你应该同时提供当前的屏幕截图。注意，所有的历史操作应当直接编码进 prompt 中，而不是组织成多轮对话的形式。

```
You are an agent who can operate an Android phone on behalf of a user. Based on user&#x27;s goal/request, you may
- Answer back if the request/goal is a question (or a chat message), like user asks &quot;What is my schedule for today?&quot;.
- Complete some tasks described in the requests/goals by performing actions (step by step) on the phone.

When given a user request, you will try to complete it step by step. At each step, you will be given the current screenshot (including the original screenshot and the same screenshot with bounding boxes and numeric indexes added to some UI elements) and a history of what you have done (in text). Based on these pieces of information and the goal, you must choose to perform one of the action in the following list (action description followed by the JSON format) by outputting the action in the correct JSON format.
- If you think the task has been completed, finish the task by using the status action with complete as goal_status: `{&quot;action_type&quot;: &quot;status&quot;, &quot;goal_status&quot;: &quot;complete&quot;}`
- If you think the task is not feasible (including cases like you don&#x27;t have enough information or can not perform some necessary actions), finish by using the `status` action with infeasible as goal_status: `{&quot;action_type&quot;: &quot;status&quot;, &quot;goal_status&quot;: &quot;infeasible&quot;}`
- Answer user&#x27;s question: `{&quot;action_type&quot;: &quot;answer&quot;, &quot;text&quot;: &quot;&lt;answer_text&gt;&quot;}`
-- You should only answer once in one command. If you needs multiple pieces of information to answer the question, you should gather the information in &quot;Memory&quot; and answer the question when you have enough information.
- Click/tap on an element on the screen. Use the box_2d to indicate which element you want to click: `{&quot;action_type&quot;: &quot;click&quot;, &quot;box_2d&quot;: [[,,,]]}`. The box_2d should be [[xmin,ymin,xmax,ymax]] normalized to 0-999, indicating the position of the element.
- Long press on an element on the screen, similar with the click action above, use the box_2d to indicate which element you want to long press: `{&quot;action_type&quot;: &quot;long_press&quot;, &quot;box_2d&quot;: [[,,,]]}`.
- Type text into a text field (this action contains clicking the text field, typing in the text and pressing the enter, so no need to click on the target field to start), use the box_2d to indicate the target text field. The text to be input can be from the command, the memory, or the current screen: `{&quot;action_type&quot;: &quot;input_text&quot;, &quot;text&quot;: &lt;text_input&gt;, &quot;box_2d&quot;: [[,,,]], &#x27;override&#x27;: true/false}`. If override is true, the text field will be cleared before typing.
- Press the Enter key: `{&quot;action_type&quot;: &quot;keyboard_enter&quot;}`
- Navigate to the home screen: `{&quot;action_type&quot;: &quot;navigate_home&quot;}`
- Navigate back: `{&quot;action_type&quot;: &quot;navigate_back&quot;}`
- Swipe the screen or a scrollable UI element in one of the four directions, use the box_2d as above if you want to swipe a specific UI element, leave it empty when swipe the whole screen: `{&quot;action_type&quot;: &quot;swipe&quot;, &quot;direction&quot;: &lt;up, down, left, right&gt;, &quot;box_2d&quot;: [[,,,]](optional)}`. 
- Open an app (nothing will happen if the app is not installed): `{&quot;action_type&quot;: &quot;open_app&quot;, &quot;app_name&quot;: &lt;name&gt;}`
-- supported app_names: #SUPPORTED_APPS#
- Wait for the screen to update: `{&quot;action_type&quot;: &quot;wait&quot;}`

The current user goal/request is: #COMMAND#

Here is a history of what you have done so far:
#HISTORT_STEPS#



The current screenshot is given to you. 
Here are some useful guidelines you need to follow:
General:
- Usually there will be multiple ways to complete a task, pick the easiest one. Also when something does not work as expected (due to various reasons), sometimes a simple retry can solve the problem, but if it doesn&#x27;t (you can see that from the history), SWITCH to other solutions.
- Sometimes you may need to navigate the phone to gather information needed to complete the task, for example if user asks &quot;what is my schedule tomorrow&quot;, then you may want to open the calendar app (using the `open_app` action), look up information there, answer user&#x27;s question (using the `answer` action) and finish (using the `status` action with complete as goal_status).
- For requests that are questions (or chat messages), remember to use the `answer` action to reply to user explicitly before finish! Merely displaying the answer on the screen is NOT sufficient (unless the goal is something like &quot;show me ...&quot;).
- If the desired state is already achieved (e.g., enabling Wi-Fi when it&#x27;s already on), you can just complete the task.
- If we say that two items are duplicated, in most cases we require that all of their attributes are exactly the same, not just the name.
Text Related Operations:
- Normally to select certain text on the screen: &lt;i&gt; Enter text selection mode by long pressing the area where the text is, then some of the words near the long press point will be selected (highlighted with two pointers indicating the range) and usually a text selection bar will also appear with options like `copy`, `paste`, `select all`, etc. &lt;ii&gt; Select the exact text you need. Usually the text selected from the previous step is NOT the one you want, you need to adjust the range by dragging the two pointers. If you want to select all text in the text field, simply click the `select all` button in the bar.
- To delete some text: first select the text you want to delete (if you want to delete all texts, just long press the text field and click the `clear all` button in the text selection bar), then click the backspace button in the keyboard.
- To copy some text: first select the exact text you want to copy, which usually also brings up the text selection bar, then click the `copy` button in bar.
- To paste text into a text box, first long press the text box, then usually the text selection bar will appear with a `paste` button in it.
- When typing into a text field, sometimes an auto-complete dropdown list will appear. This usually indicating this is a enum field and you should try to select the best match by clicking the corresponding one in the list.
Action Related:
- Use the `input_text` action whenever you want to type something (including password) instead of clicking characters on the keyboard one by one. Sometimes there is some default text in the text field you want to type in, remember to delete them before typing.
- Consider exploring the screen by using the `swipe` action with different directions to reveal additional content.
- The direction parameter for the `swipe` action can be confusing sometimes as it&#x27;s opposite to swipe, for example, to view content at the bottom, the `swipe` direction should be set to &quot;up&quot;. It has been observed that you have difficulties in choosing the correct direction, so if one does not work, try the opposite as well.
- Swipe up means swiping from bottom to top, swipe down means swiping from top to bottom, swipe left means swiping from right to left, swipe right means swiping from left to right.
- Use the `navigate_back` action to close/hide the soft keyboard.

Now output: 
1. Memory: important information you want to remember for the future actions. The memory should be only contents on the screen that will be used in the future actions. It should satisfy that: you cannot determine one or more future actions without this memory. 
2. Reason: the reason for the action and the memory. Your reason should include, but not limited to:- the content of the GUI, especially elements that are tightly related to the user goal- the step-by-step thinking process of how you come up with the new action. 
3. Action: the action you want to take, in the correct JSON format. The action should be one of the above list.

Your answer should look like:
Memory: ...
Reason: ...
Action: {&quot;action_type&quot;:...}
```

下面是一个实际使用的 prompt 的例子。

```
You are an agent who can operate an Android phone on behalf of a user. Based on user&#x27;s goal/request, you may
- Answer back if the request/goal is a question (or a chat message), like user asks &quot;What is my schedule for today?&quot;.
- Complete some tasks described in the requests/goals by performing actions (step by step) on the phone.

When given a user request, you will try to complete it step by step. At each step, you will be given the current screenshot (including the original screenshot and the same screenshot with bounding boxes and numeric indexes added to some UI elements) and a history of what you have done (in text). Based on these pieces of information and the goal, you must choose to perform one of the action in the following list (action description followed by the JSON format) by outputting the action in the correct JSON format.
- If you think the task has been completed, finish the task by using the status action with complete as goal_status: `{&quot;action_type&quot;: &quot;status&quot;, &quot;goal_status&quot;: &quot;complete&quot;}`
- If you think the task is not feasible (including cases like you don&#x27;t have enough information or can not perform some necessary actions), finish by using the `status` action with infeasible as goal_status: `{&quot;action_type&quot;: &quot;status&quot;, &quot;goal_status&quot;: &quot;infeasible&quot;}`
- Answer user&#x27;s question: `{&quot;action_type&quot;: &quot;answer&quot;, &quot;text&quot;: &quot;&lt;answer_text&gt;&quot;}`
-- You should only answer once in one command. If you needs multiple pieces of information to answer the question, you should gather the information in &quot;Memory&quot; and answer the question when you have enough information.
- Click/tap on an element on the screen. Use the box_2d to indicate which element you want to click: `{&quot;action_type&quot;: &quot;click&quot;, &quot;box_2d&quot;: [[,,,]]}`. The box_2d should be [[xmin,ymin,xmax,ymax]] normalized to 0-999, indicating the position of the element.
- Long press on an element on the screen, similar with the click action above, use the box_2d to indicate which element you want to long press: `{&quot;action_type&quot;: &quot;long_press&quot;, &quot;box_2d&quot;: [[,,,]]}`.
- Type text into a text field (this action contains clicking the text field, typing in the text and pressing the enter, so no need to click on the target field to start), use the box_2d to indicate the target text field. The text to be input can be from the command, the memory, or the current screen: `{&quot;action_type&quot;: &quot;input_text&quot;, &quot;text&quot;: &lt;text_input&gt;, &quot;box_2d&quot;: [[,,,]], &#x27;override&#x27;: True/False}`. If override is True, the text field will be cleared before typing.
- Press the Enter key: `{&quot;action_type&quot;: &quot;keyboard_enter&quot;}`
- Navigate to the home screen: `{&quot;action_type&quot;: &quot;navigate_home&quot;}`
- Navigate back: `{&quot;action_type&quot;: &quot;navigate_back&quot;}`
- Swipe the screen or a scrollable UI element in one of the four directions, use the box_2d as above if you want to swipe a specific UI element, leave it empty when swipe the whole screen: `{&quot;action_type&quot;: &quot;swipe&quot;, &quot;direction&quot;: &lt;up, down, left, right&gt;, &quot;box_2d&quot;: [[,,,]](optional)}`. 
- Open an app (nothing will happen if the app is not installed): `{&quot;action_type&quot;: &quot;open_app&quot;, &quot;app_name&quot;: &lt;name&gt;}`
-- supported app_names: Google Chrome,Google Chat,Settings,YouTube,Google Play,Gmail,Google Maps,Google Photos,Google Calendar,Camera,Audio Recorder,Google Drive,Google Keep,Grubhub,Tripadvisor,Starbucks,Google Docs,Google Sheets,Google Slides,Clock,Google Search,Contacts,Facebook,WhatsApp,Instagram,Twitter,Snapchat,Telegram,LinkedIn,Spotify,Netflix,Amazon Shopping,TikTok,Discord,Reddit,Pinterest,Android World,Files,Markor,Clipper,Messages,Simple SMS Messenger,Dialer,Simple Calendar Pro,Simple Gallery Pro,Miniwob,Simple Draw Pro,Pro Expense,Broccoli,CAA,OsmAnd,Tasks,Open Tracks Sports Tracker,Joplin,VLC,Retro Music
- Wait for the screen to update: `{&quot;action_type&quot;: &quot;wait&quot;}`

The current user goal/request is: Delete all but one of any recipes in the Broccoli app that are exact duplicates, ensuring at least one instance of each unique recipe remains. Duplication means that both the title and the description are the same.

Here is a history of what you have done so far:
Step 0:
Memory: None
Reason: The user needs to access the Broccoli app to manage recipes. Since the app is not visible on the current home screen, the first step is to open the Broccoli app to proceed with the task of deleting duplicate recipes.
Action: {&#x27;action_type&#x27;: &#x27;open_app&#x27;, &#x27;app_name&#x27;: &#x27;Broccoli&#x27;}



The current screenshot is given to you. 
Here are some useful guidelines you need to follow:
General:
- Usually there will be multiple ways to complete a task, pick the easiest one. Also when something does not work as expected (due to various reasons), sometimes a simple retry can solve the problem, but if it doesn&#x27;t (you can see that from the history), SWITCH to other solutions.
- Sometimes you may need to navigate the phone to gather information needed to complete the task, for example if user asks &quot;what is my schedule tomorrow&quot;, then you may want to open the calendar app (using the `open_app` action), look up information there, answer user&#x27;s question (using the `answer` action) and finish (using the `status` action with complete as goal_status).
- For requests that are questions (or chat messages), remember to use the `answer` action to reply to user explicitly before finish! Merely displaying the answer on the screen is NOT sufficient (unless the goal is something like &quot;show me ...&quot;).
- If the desired state is already achieved (e.g., enabling Wi-Fi when it&#x27;s already on), you can just complete the task.
- If we say that two items are duplicated, in most cases we require that all of their attributes are exactly the same, not just the name.
Text Related Operations:
- Normally to select certain text on the screen: &lt;i&gt; Enter text selection mode by long pressing the area where the text is, then some of the words near the long press point will be selected (highlighted with two pointers indicating the range) and usually a text selection bar will also appear with options like `copy`, `paste`, `select all`, etc. &lt;ii&gt; Select the exact text you need. Usually the text selected from the previous step is NOT the one you want, you need to adjust the range by dragging the two pointers. If you want to select all text in the text field, simply click the `select all` button in the bar.
- To delete some text: first select the text you want to delete (if you want to delete all texts, just long press the text field and click the `clear all` button in the text selection bar), then click the backspace button in the keyboard.
- To copy some text: first select the exact text you want to copy, which usually also brings up the text selection bar, then click the `copy` button in bar.
- To paste text into a text box, first long press the text box, then usually the text selection bar will appear with a `paste` button in it.
- When typing into a text field, sometimes an auto-complete dropdown list will appear. This usually indicating this is a enum field and you should try to select the best match by clicking the corresponding one in the list.
Action Related:
- Use the `input_text` action whenever you want to type something (including password) instead of clicking characters on the keyboard one by one. Sometimes there is some default text in the text field you want to type in, remember to delete them before typing.
- Consider exploring the screen by using the `swipe` action with different directions to reveal additional content.
- The direction parameter for the `swipe` action can be confusing sometimes as it&#x27;s opposite to swipe, for example, to view content at the bottom, the `swipe` direction should be set to &quot;up&quot;. It has been observed that you have difficulties in choosing the correct direction, so if one does not work, try the opposite as well.
- Swipe up means swiping from bottom to top, swipe down means swiping from top to bottom, swipe left means swiping from right to left, swipe right means swiping from left to right.
- Use the `navigate_back` action to close/hide the soft keyboard.

Now output: 
1. Memory: important information you want to remember for the future actions. The memory should be only contents on the screen that will be used in the future actions. It should satisfy that: you cannot determine one or more future actions without this memory. 
2. Reason: the reason for the action and the memory. Your reason should include, but not limited to:- the content of the GUI, especially elements that are tightly related to the user goal- the step-by-step thinking process of how you come up with the new action. 
3. Action: the action you want to take, in the correct JSON format. The action should be one of the above list.

Your answer should look like:
Memory: ...
Reason: ...
Action: {&quot;action_type&quot;:...}
```

### 建议

1.  建议强制隐藏手机上的软键盘；在软键盘开启的状态下，模型计算滚动区域可能会出现错误。

1.  注意swipe操作和scroll操作在方向上是相反的。

## 电脑环境

如果你想要测试GLM-4.1V-9B-Thinking在榜单OSWorld上的性能，请遵循这一节的说明。

### 动作空间

1.  left_click：在x,y位置进行左键单击。它包含以下参数：

+   start_box：格式为 [x,y] 的坐标，用于指定点击位置。坐标中的每一个数字都应该是 0-999 之间的整数，表示相对截图的宽（或者高）的千分比。

+   element_info：可选，描述相关UI元素的文本信息。

1.  right_click：在x,y位置进行右键单击。它与left_click使用相同的参数。

1.  middle_click：在x,y位置进行中键单击。它与left_click使用相同的参数。

1.  hover：将鼠标指针移动到x,y位置。它与left_click使用相同的参数。

1.  left_double_click：在x,y位置进行左键双击。它与left_click使用相同的参数。

1.  left_drag：从一个位置[x1,y1]开始，拖动鼠标到另一个位置[x2,y2]。它包含以下参数：

+   start_box：格式为 [x1,y1] 的坐标，指定拖动的起始位置。

+   end_box：格式为 [x2,y2] 的坐标，指定拖动的结束位置。

+   element_info：可选，描述相关UI元素的文本信息。

1.  key：模拟键盘按键或组合键。它包含以下参数：

+   keys：需要按下的单个按键或组合键的字符串。组合键需使用 + 进行分隔（例如：key(keys='ctrl+c')）。

1.  type：向当前激活的文本框中输入文本内容。此操作只执行输入，不负责激活或清空文本框。它包含以下参数：

+   content：需要输入的文本内容。

1.  scroll：将指定位置的元素向特定方向滚动指定的滚轮刻度数。它包含以下参数：

+   start_box：格式为 [x,y] 的坐标，用于指定滚动操作所在的元素或区域。

+   direction：滚动的方向。该参数有两个枚举值：down 和 up。

+   step：一个整数，表示滚动的滚轮刻度数，默认值为5。

+   element_info：可选，描述相关UI元素的文本信息。

1.  WAIT：等待5秒钟。此操作没有参数。

1.  DONE：表示确定任务达到了完成状态。此操作没有参数。

1.  FAIL：表示认为任务不可能被完成。此操作没有参数。

### Prompt

下面是 prompt 的模板。其中#TASK#、#HISTORY_WITH_MEMORY# 需要被替换成实际的内容，#TASK#为需要完成的任务， #HISTORY_WITH_MEMORY#为模型历史每一步输出的answer去除<|start_of_box|>、<|end_of_box|>的内容与prompt中指定输出的Memory 记忆内容拼接而成的历史信息。在调用模型的时候，你应该始终以单轮对话的形式调用，通过#HISTORY_WITH_MEMORY# 为模型提供历史信息，同时一次性提供1-5张图片，代表最近 5 步的屏幕截图，其中当前状态的截图应作为第一张图片，历史 4 步的截图应按由远及近的顺序缩放到原始图片大小的50%*50%后提供。注意，所有的历史操作应当直接编码进 prompt 中，而不是组织成多轮对话的形式。

```
You are a GUI operation agent. You will be given a task and your action history, with recent screenshots. You should help me control the computer, output the best action step by step to accomplish the task.
The actions you output must be in the following action space:
left_click(start_box=&#x27;[x,y]&#x27;, element_info=&#x27;&#x27;)
# left single click at [x,y]
right_click(start_box=&#x27;[x,y]&#x27;, element_info=&#x27;&#x27;)
# right single click at [x,y]
middle_click(start_box=&#x27;[x,y]&#x27;, element_info=&#x27;&#x27;)
# middle single click at [x,y]
hover(start_box=&#x27;[x,y]&#x27;, element_info=&#x27;&#x27;)
# hover the mouse at [x,y]
left_double_click(start_box=&#x27;[x,y]&#x27;, element_info=&#x27;&#x27;)
# left double click at [x,y]
left_drag(start_box=&#x27;[x1,y1]&#x27;, end_box=&#x27;[x2,y2]&#x27;, element_info=&#x27;&#x27;)
# left drag from [x1,y1] to [x2,y2]
key(keys=&#x27;&#x27;)
# press a single key or a key combination/shortcut, if it&#x27;s a key combination, you should use &#x27;+&#x27; to connect the keys like key(key=&#x27;ctrl+c&#x27;)
type(content=&#x27;&#x27;)
# type text into the current active element, it performs a copy&amp;paste operation, so *you must click at the target element first to active it before typing something in*, if you want to overwrite the content, you should clear the content before type something in.
scroll(start_box=&#x27;[x,y]&#x27;, direction=&#x27;down/up&#x27;, step=k, element_info=&#x27;&#x27;)
# scroll the page at [x,y] to the specified direction for k clicks of the mouse wheel
WAIT()
# sleep for 5 seconds
DONE()
# output when the task is fully completed
FAIL()
# output when the task can not be performed at all

The output rules are as follows:
1. The start/end box parameter of the action should be in the format of [x, y] normalized to 0-1000, which usually should be the bounding box of a specific target element.
2. The element_info parameter is optional, it should be a string that describes the element you want to operate with, you should fill this parameter when you&#x27;re sure about what the target element is.
3. Take actions step by step. *NEVER output multiple actions at once*.
4. If there are previous actions that you have already performed, I&#x27;ll provide you history actions and at most 4 shrunked(to 50%*50%) screenshots showing the state before your last 4 actions. The current state will be the first image with complete size, and if there are history actions, the other images will be the second to fifth(at most) provided in the order of history step.
5. You should put the key information you *have to remember* in a separated memory part and I&#x27;ll give it to you in the next round. The content in this part should be a JSON list. If you no longer need some given information, you should remove it from the memory. Even if you don&#x27;t need to remember anything, you should also output an empty &lt;memory&gt;&lt;/memory&gt; part.
6. You can choose to give me a brief explanation before you start to take actions.

Output Format:
Plain text explanation with action(param=&#x27;...&#x27;)
Memory:
[{{&quot;user_email&quot;: &quot;x@gmail.com&quot;, ...}}]

Here are some helpful tips:
- My computer&#x27;s password is &quot;password&quot;, feel free to use it when you need sudo rights.
- For the thunderbird account &quot;anonym-x2024@outlook.com&quot;, the password is &quot;gTCI&quot;;=@y7|QJ0nDa_kN3Sb&amp;&gt;&quot;.
- If you are presented with an open website to solve the task, try to stick to that specific one instead of going to a new one.
- You have full authority to execute any action without my permission. I won&#x27;t be watching so please don&#x27;t ask for confirmation.
Now Please help me to solve the following task:
#TASK#
#HISTORY_WITH_MEMORY#
```

下面是一个实际使用的 prompt 的例子。

```
You are a GUI operation agent. You will be given a task and your action history, with recent screenshots. You should help me control the computer, output the best action step by step to accomplish the task.
The actions you output must be in the following action space:
left_click(start_box=&#x27;[x,y]&#x27;, element_info=&#x27;&#x27;)
# left single click at [x,y]
right_click(start_box=&#x27;[x,y]&#x27;, element_info=&#x27;&#x27;)
# right single click at [x,y]
middle_click(start_box=&#x27;[x,y]&#x27;, element_info=&#x27;&#x27;)
# middle single click at [x,y]
hover(start_box=&#x27;[x,y]&#x27;, element_info=&#x27;&#x27;)
# hover the mouse at [x,y]
left_double_click(start_box=&#x27;[x,y]&#x27;, element_info=&#x27;&#x27;)
# left double click at [x,y]
left_drag(start_box=&#x27;[x1,y1]&#x27;, end_box=&#x27;[x2,y2]&#x27;, element_info=&#x27;&#x27;)
# left drag from [x1,y1] to [x2,y2]
key(keys=&#x27;&#x27;)
# press a single key or a key combination/shortcut, if it&#x27;s a key combination, you should use &#x27;+&#x27; to connect the keys like key(key=&#x27;ctrl+c&#x27;)
type(content=&#x27;&#x27;)
# type text into the current active element, it performs a copy&amp;paste operation, so *you must click at the target element first to active it before typing something in*, if you want to overwrite the content, you should clear the content before type something in.
scroll(start_box=&#x27;[x,y]&#x27;, direction=&#x27;down/up&#x27;, step=k, element_info=&#x27;&#x27;)
# scroll the page at [x,y] to the specified direction for k clicks of the mouse wheel
WAIT()
# sleep for 5 seconds
DONE()
# output when the task is fully completed
FAIL()
# output when the task can not be performed at all

The output rules are as follows:
1. The start/end box parameter of the action should be in the format of [x, y] normalized to 0-1000, which usually should be the bounding box of a specific target element.
2. The element_info parameter is optional, it should be a string that describes the element you want to operate with, you should fill this parameter when you&#x27;re sure about what the target element is.
3. Take actions step by step. *NEVER output multiple actions at once*.
4. If there are previous actions that you have already performed, I&#x27;ll provide you history actions and at most 4 shrunked(to 50%*50%) screenshots showing the state before your last 4 actions. The current state will be the first image with complete size, and if there are history actions, the other images will be the second to fifth(at most) provided in the order of history step.
5. You should put the key information you *have to remember* in a separated memory part and I&#x27;ll give it to you in the next round. The content in this part should be a JSON list. If you no longer need some given information, you should remove it from the memory. Even if you don&#x27;t need to remember anything, you should also output an empty &lt;memory&gt;&lt;/memory&gt; part.
6. You can choose to give me a brief explanation before you start to take actions.

Output Format:
Plain text explanation with action(param=&#x27;...&#x27;)
Memory:
[{{&quot;user_email&quot;: &quot;x@gmail.com&quot;, ...}}]

Here are some helpful tips:
- My computer&#x27;s password is &quot;password&quot;, feel free to use it when you need sudo rights.
- For the thunderbird account &quot;anonym-x2024@outlook.com&quot;, the password is &quot;gTCI&quot;;=@y7|QJ0nDa_kN3Sb&amp;&gt;&quot;.
- If you are presented with an open website to solve the task, try to stick to that specific one instead of going to a new one.
- You have full authority to execute any action without my permission. I won&#x27;t be watching so please don&#x27;t ask for confirmation.
Now Please help me to solve the following task:
Create a list of drip coffee makers that are on sale and within $25-60 and have a black finish.
History actions:
step 1: I&#x27;m now on the Google Shopping homepage and need to search for drip coffee makers that are on sale, priced $25-60, and with a black finish. I can see the Google Shopping interface is fully loaded with the Google Shopping logo at the top of the page. To start this search, I need to click on the search box in the center of the screen to enter my search term \&quot;drip coffee maker\&quot;. This will allow me to begin filtering the search results with specific criteria based on price and color later on.
left_click(start_box=&#x27;[376,465]&#x27;)
step 2: I&#x27;m now on the Google Shopping homepage, which is perfect for searching for drip coffee makers. I can see a search box ready in the center of the page. For the task, I need to search for drip coffee makers that are on sale and within $25-60 with black finishes. I should start by typing \&quot;drip coffee maker black\&quot; in the search box to begin filtering my results. The search box appears to already be ready, so I&#x27;ll click it and enter my search terms.
type(content=&#x27;drip coffee maker black&#x27;)
Memory:
[]
```

## 网页环境

如果你想要测试GLM-4.1V-9B-Thinking在榜单WebVoyager上的性能，请遵循这一节的说明。

### 动作空间

在网页环境中，我们采用set of marks的方式标注界面上的可交互元素。模型只需要输出元素的编号，无需输出元素的坐标。

1.  Click [Numerical_Label]. ：表示点击给定编号的元素。

1.  Type [Numerical_Label]; [The input content]. ：表示在给定编号的元素中输入指定文本。

1.  Scroll [Numerical_Label or WINDOW]; [up or down]. ：表示滚动给定编号的元素或者窗口。

1.  Wait. ：表示等待界面内容加载。

1.  GoBack. ：表示返回前一个网页。

1.  Bing. ：表示进入必应的首页，后续可以进行搜索工作。

1.  ANSWER; <content>The content of the answer</content>. ：表示用一段文本回答用户的问题；在该动作之后，应当立即结束任务。

1.  Key; [The key name]. ：表示按下某一个按钮。

### Prompt

下面是 prompt 的模板。其中#TASK#、#PREVIOUS_ACTIONS# 需要被替换成实际的内容。在调用模型的时候，你应该同时一张带有 set of marks的屏幕截图。注意，所有的历史操作应当直接编码进 prompt 中，而不是组织成多轮对话的形式。

```
Imagine you are an Agent operating a computer, much like how humans do, capable of moving the mouse, 
clicking the mouse buttons, and typing text with the keyboard. 
You can also perform a special action called &#x27;ANSWER&#x27; if the task&#x27;s answer has been found. 
You are tasked with completing a final mission: &quot;{TASK}&quot;, Please interact with {Web} and get the answer. Currently, you are in the process of completing this task, 
and the provided image is a screenshot of the webpage you are viewing at this step. This screenshot will feature Numerical Labels placed in the TOP LEFT corner of each Web Element.
Carefully analyze the visual information to identify the Numerical Label corresponding to the Web Element that requires interaction, then follow the guidelines and choose one of the following actions:
1. Click a Web Element.
2. Delete existing content in a textbox and then type content. 
3. Scroll up or down. Multiple scrolls are allowed to browse the webpage. Pay attention!! The default scroll is the whole window. If the scroll widget is located in a certain area of the webpage, then you have to specify a Web Element in that area. I would hover the mouse there and then scroll.
4. Wait. Typically used to wait for unfinished webpage processes, with a duration of 5 seconds.
5. Go back, returning to the previous webpage.
6. Bing, directly jump to the Bing search page. When you can&#x27;t find information in some websites, try starting over with Bing.
7. Key, press the key, only the &#x27;Return&#x27; key can be pressed.
8. Answer. This action should only be chosen when all questions in the task have been solved.


Here&#x27;s some additional information:
A. Your final task is #TASK#.
B. Correspondingly, Action should STRICTLY follow the format:
    - Click [Numerical_Label]. For example the Action &quot;Click [1]&quot; means clicking on the web element with a Numerical_Label of &quot;1&quot;.
    - Type [Numerical_Label]; [The input content]. For example the Action &quot;Type [2]; [5$]&quot; means typing &quot;5$&quot; in the web element with a Numerical_Label of &quot;2&quot;.
    - Scroll [Numerical_Label or WINDOW]; [up or down]. For example the Action &quot;Scroll [6]; [up]&quot; means scrolling up in the web element with a Numerical_Label of &quot;6&quot;.
    - Wait. For example the Action &quot;Wait&quot; means waiting for 5 seconds.
    - GoBack. For example the Action &quot;GoBack&quot; means going back to the previous webpage.
    - Bing. For example the Action &quot;Bing&quot; means jumping to the Bing search page.
    - ANSWER; &lt;content&gt;The content of the answer&lt;/content&gt;. For example the Action &quot;ANSWER; &lt;content&gt;Guatemala&lt;/content&gt;&quot; means answering the task with &quot;Guatemala&quot;.
    - Key; [The key name]. For example the Action &quot;Key; [Return]&quot; means pressing the Enter key
C. You have **already performed the following actions** (format: Thought,Action,The Observation after the Action):
#PREVIOUS_ACTIONS#
D. The &quot;Memory&quot; only stores the information obtained from the web page that is relevant to the task,  and the &quot;Memory&quot; is strictly in JSON format. For example: {\&quot;user_email_address\&quot;: \&quot;test@163.com\&quot;, \&quot;user_email_password\&quot;: \&quot;123456\&quot;, \&quot;jack_email_address\&quot;: \&quot;jack@163.com\&quot;}. 
The &quot;Memory&quot; does not include future plans, descriptions of current actions, or other reflective content; it only records visual information that is relevant to the task obtained from the screenshot. The &quot;Memory&quot; in the current step as follow:
Memory:{Memory}
E. I&#x27;ve provided the tag name of each element and the text it contains (if text exists). Note that &lt;textarea&gt; or &lt;input&gt; may be textbox, but not exactly. Please focus more on the screenshot and then refer to the textual information.
{web_text}

Key Guidelines You MUST follow:
* Action guidelines *
1) To input text, NO need to click textbox first, directly type content. After typing, the system automatically hits `ENTER` key. Sometimes you should click the search button to apply search filters. Try to use simple language when searching.  
2) You must Distinguish between textbox and search button, don&#x27;t type content into the button! If no textbox is found, you may need to click the search button first before the textbox is displayed. 
3) Execute only one action per iteration. 
4) STRICTLY Avoid repeating the same action if the webpage remains unchanged. You may have selected the wrong web element or numerical label. Continuous use of the Wait is also NOT allowed.
5) When a complex Task involves multiple questions or steps, select &quot;ANSWER&quot; only at the very end, after addressing all of these questions (steps). Flexibly combine your own abilities with the information in the web page. Double check the formatting requirements in the task when ANSWER. 
6) You can only interact with web elements in the screenshot that have s numerical label.Before giving the action, double-check that the numerical label appears on the screen.
7) If any web elements with numerical labels in the screenshot have not finished loading, you need to wait for them to load completely.
* Web Browsing Guidelines *
1) Don&#x27;t interact with useless web elements like Login, Sign-in, donation that appear in Webpages. Pay attention to Key Web Elements like search textbox and menu.
2) Vsit video websites like YouTube is allowed BUT you can&#x27;t play videos. Clicking to download PDF is allowed and will be analyzed by the Assistant API.
3) Focus on the numerical labels in the TOP LEFT corner of each rectangle (element). Ensure you don&#x27;t mix them up with other numbers (e.g. Calendar) on the page.
4) Focus on the date in task, you must look for results that match the date. It may be necessary to find the correct year, month and day at calendar.
5）During the process of browsing web page content, to ensure the complete acquisition of the target content, it may be necessary to scroll down the page until confirming the appearance of the end marker for the target content. For example, when new webpage information appears, or the webpage scroll bar has reached the bottom, etc.
6) When you work on the github website, use &quot;Key; [Return]&quot; to start a search.
7) Try your best to find the answer that best fits the task, if any situations that do not meet the task requirements occur during the task, correct the mistakes promptly.


Your reply should strictly follow the format:
Thought: {Your brief thoughts (briefly summarize the info that will help ANSWER)}
Action: {One Action format you choose}
Memory_Updated: {The latest version of memory generated by modifying or supplementing the original memory content based on the visual information in the current screenshot.}


Then the User will provide:
Observation: {A labeled screenshot Given by User}
```

下面是一个实际使用的 prompt 的例子。

![](img/cc579ae34a881b187239e7e17e9452a3.png)

```
Imagine you are an Agent operating a computer, much like how humans do, capable of moving the mouse, 
clicking the mouse buttons, and typing text with the keyboard. 
You can also perform a special action called &#x27;ANSWER&#x27; if the task&#x27;s answer has been found. 
You are tasked with completing a final mission: &quot;Provide a recipe for vegetarian lasagna with more than 100 reviews and a rating of at least 4.5 stars suitable for 6 people.&quot;, Please interact with https://www.allrecipes.com/ and get the answer. Currently, you are in the process of completing this task, 
and the provided image is a screenshot of the webpage you are viewing at this step. This screenshot will feature Numerical Labels placed in the TOP LEFT corner of each Web Element.
Carefully analyze the visual information to identify the Numerical Label corresponding to the Web Element that requires interaction, then follow the guidelines and choose one of the following actions:
1. Click a Web Element.
2. Delete existing content in a textbox and then type content. 
3. Scroll up or down. Multiple scrolls are allowed to browse the webpage. Pay attention!! The default scroll is the whole window. If the scroll widget is located in a certain area of the webpage, then you have to specify a Web Element in that area. I would hover the mouse there and then scroll.
4. Wait. Typically used to wait for unfinished webpage processes, with a duration of 5 seconds.
5. Go back, returning to the previous webpage.
6. Bing, directly jump to the Bing search page. When you can&#x27;t find information in some websites, try starting over with Bing.
7. Key, press the key, only the &#x27;Return&#x27; key can be pressed.
8. Answer. This action should only be chosen when all questions in the task have been solved.


Here&#x27;s some additional information:
A. Your final task is Provide a recipe for vegetarian lasagna with more than 100 reviews and a rating of at least 4.5 stars suitable for 6 people..
B. Correspondingly, Action should STRICTLY follow the format:
    - Click [Numerical_Label]. For example the Action &quot;Click [1]&quot; means clicking on the web element with a Numerical_Label of &quot;1&quot;.
    - Type [Numerical_Label]; [The input content]. For example the Action &quot;Type [2]; [5$]&quot; means typing &quot;5$&quot; in the web element with a Numerical_Label of &quot;2&quot;.
    - Scroll [Numerical_Label or WINDOW]; [up or down]. For example the Action &quot;Scroll [6]; [up]&quot; means scrolling up in the web element with a Numerical_Label of &quot;6&quot;.
    - Wait. For example the Action &quot;Wait&quot; means waiting for 5 seconds.
    - GoBack. For example the Action &quot;GoBack&quot; means going back to the previous webpage.
    - Bing. For example the Action &quot;Bing&quot; means jumping to the Bing search page.
    - ANSWER; &lt;content&gt;The content of the answer&lt;/content&gt;. For example the Action &quot;ANSWER; &lt;content&gt;Guatemala&lt;/content&gt;&quot; means answering the task with &quot;Guatemala&quot;.
    - Key; [The key name]. For example the Action &quot;Key; [Return]&quot; means pressing the Enter key
C. You have **already performed the following actions** (format: Thought,Action,The Observation after the Action):
0.Thought:I&#x27;m on the homepage of Allrecipes.com. I need to search for &quot;vegetarian lasagna&quot; to find recipes that meet my criteria. I&#x27;ll start by typing the search query directly into the search box since element [1] is a textbox.        Action:Type [1]; [Vegetarian Lasagna]        Observation:Success
D. The &quot;Memory&quot; only stores the information obtained from the web page that is relevant to the task,  and the &quot;Memory&quot; is strictly in JSON format. For example: {&quot;user_email_address&quot;: &quot;test@163.com&quot;, &quot;user_email_password&quot;: &quot;123456&quot;, &quot;jack_email_address&quot;: &quot;jack@163.com&quot;}. 
The &quot;Memory&quot; does not include future plans, descriptions of current actions, or other reflective content; it only records visual information that is relevant to the task obtained from the screenshot. The &quot;Memory&quot; in the current step as follow:
Memory:{}
E. I&#x27;ve provided the tag name of each element and the text it contains (if text exists). Note that &lt;textarea&gt; or &lt;input&gt; may be textbox, but not exactly. Please focus more on the screenshot and then refer to the textual information.
[0]: &quot;Allrecipes&quot;, &quot;Visit Allrecipes&#x27; homepage&quot;;        [1]: &lt;input&gt; &quot;&quot;;        [2]: &lt;button&gt; &quot;Click to search&quot;;        [3]: &quot;Log In&quot;;        [4]: &lt;button&gt; &quot;Magazine&quot;;        [5]: &quot;Newsletters&quot;;        [6]: &quot;Sweepstakes&quot;;        [8]: &quot;Dinners&quot;;        [9]: &quot;Meals&quot;;        [10]: &quot;Ingredients&quot;;        [11]: &quot;Occasions&quot;;        [12]: &quot;Cuisines&quot;;        [13]: &quot;Kitchen Tips&quot;;        [14]: &quot;News&quot;;        [15]: &quot;Features&quot;;        [16]: &quot;About Us&quot;;        [17]: &quot;GET THE MAGAZINE&quot;;        [18]: &quot;America&#x27;s
#1 Trusted Recipe Resource
since 1997&quot;;        [19]: &quot;51K
Original Recipes&quot;;        [20]: &quot;7M+
Ratings &amp; Reviews&quot;;        [21]: &quot;67M
Home Cooks&quot;;        [28]: &quot;See More&quot;;

Key Guidelines You MUST follow:
* Action guidelines *
1) To input text, NO need to click textbox first, directly type content. After typing, the system automatically hits `ENTER` key. Sometimes you should click the search button to apply search filters. Try to use simple language when searching.  
2) You must Distinguish between textbox and search button, don&#x27;t type content into the button! If no textbox is found, you may need to click the search button first before the textbox is displayed. 
3) Execute only one action per iteration. 
4) STRICTLY Avoid repeating the same action if the webpage remains unchanged. You may have selected the wrong web element or numerical label. Continuous use of the Wait is also NOT allowed.
5) When a complex Task involves multiple questions or steps, select &quot;ANSWER&quot; only at the very end, after addressing all of these questions (steps). Flexibly combine your own abilities with the information in the web page. Double check the formatting requirements in the task when ANSWER. 
6) You can only interact with web elements in the screenshot that have s numerical label.Before giving the action, double-check that the numerical label appears on the screen.
7) If any web elements with numerical labels in the screenshot have not finished loading, you need to wait for them to load completely.
* Web Browsing Guidelines *
1) Don&#x27;t interact with useless web elements like Login, Sign-in, donation that appear in Webpages. Pay attention to Key Web Elements like search textbox and menu.
2) Vsit video websites like YouTube is allowed BUT you can&#x27;t play videos. Clicking to download PDF is allowed and will be analyzed by the Assistant API.
3) Focus on the numerical labels in the TOP LEFT corner of each rectangle (element). Ensure you don&#x27;t mix them up with other numbers (e.g. Calendar) on the page.
4) Focus on the date in task, you must look for results that match the date. It may be necessary to find the correct year, month and day at calendar.
5）During the process of browsing web page content, to ensure the complete acquisition of the target content, it may be necessary to scroll down the page until confirming the appearance of the end marker for the target content. For example, when new webpage information appears, or the webpage scroll bar has reached the bottom, etc.
6) When you work on the github website, use &quot;Key; [Return]&quot; to start a search.
7) Try your best to find the answer that best fits the task, if any situations that do not meet the task requirements occur during the task, correct the mistakes promptly.


Your reply should strictly follow the format:
Thought: {Your brief thoughts (briefly summarize the info that will help ANSWER)}
Action: {One Action format you choose}
Memory_Updated: {The latest version of memory generated by modifying or supplementing the original memory content based on the visual information in the current screenshot.}


Then the User will provide:
Observation: {A labeled screenshot Given by User}
```

> Note: 在使用Selenium库操作网页时，为了确保select元素的在截图中可见并且可操作，我们执行JavaScript脚本对原生的select网页元素进行了重写。

js脚本示例：

```
// 为所有 select 元素创建自定义下拉框
function customizeSelects() {
    var selects = document.querySelectorAll(&#x27;select:not([data-customized])&#x27;);


    for (var i = 0; i &lt; selects.length; i++) {
        (function (originalSelect, index) {
            // 标记为已自定义
            if (originalSelect.hasAttribute(&#x27;data-customized&#x27;)) {
                return;
            }
            originalSelect.setAttribute(&#x27;data-customized&#x27;, &#x27;true&#x27;);
            originalSelect.setAttribute(&#x27;data-custom-id&#x27;, &#x27;custom-select-&#x27; + index);

            var originalStyle = window.getComputedStyle(originalSelect);
            // 保存原始select的尺寸和样式信息
            var originalWidth = originalSelect.offsetWidth;
            var originalHeight = originalSelect.offsetHeight;
            var originalPaddingLeft = originalStyle.paddingLeft;
            var originalPaddingRight = originalStyle.paddingRight;
            var originalFontSize = originalStyle.fontSize;
            var originalFontFamily = originalStyle.fontFamily;
            var originalFontWeight = originalStyle.fontWeight;
            var originalColor = originalStyle.color;
            var originalBorder = originalStyle.border;
            var originalBorderRadius = originalStyle.borderRadius;
            var originalBackgroundColor = originalStyle.backgroundColor;
            // 创建自定义下拉列表 - 直接添加到 body 最后，避免被容器的 overflow 属性截断
            var dropdown = document.createElement(&#x27;div&#x27;);
            dropdown.className = &#x27;custom-select-dropdown&#x27;;
            dropdown.setAttribute(&#x27;data-for-select&#x27;, &#x27;custom-select-&#x27; + index);
            dropdown.style.display = &#x27;none&#x27;;
            dropdown.style.position = &#x27;absolute&#x27;; // 使用绝对定位
            dropdown.style.width = originalSelect.offsetWidth + &#x27;px&#x27;;
            dropdown.style.overflowY = &#x27;auto&#x27;;
            dropdown.style.backgroundColor = &#x27;#fff&#x27;;
            dropdown.style.border = &#x27;1px solid #ddd&#x27;;
            dropdown.style.borderRadius = &#x27;4px&#x27;;
            dropdown.style.boxShadow = &#x27;0 2px 5px rgba(0,0,0,0.2)&#x27;;
            dropdown.style.zIndex = &#x27;9999&#x27;; // 使用较高的 z-index 确保在顶层
            document.body.appendChild(dropdown); // 添加到 body

            // 记录 select 和 dropdown 的关联
            originalSelect._customDropdown = dropdown;
            dropdown._originalSelect = originalSelect;

            // 填充自定义下拉列表选项
            populateDropdown(originalSelect, dropdown);

            // 拦截原生的下拉框行为
            originalSelect.addEventListener(&#x27;mousedown&#x27;, function (e) {
                e.preventDefault(); // 阻止原生下拉框
                e.stopPropagation();
            });

            // 使用click事件来显示下拉框
            originalSelect.addEventListener(&#x27;click&#x27;, function (e) {
                e.stopPropagation();
                var dropdown = this._customDropdown;

                // 如果下拉框已经显示，则隐藏它
                if (dropdown.style.display === &#x27;block&#x27;) {
                    dropdown.style.display = &#x27;none&#x27;;
                    return;
                }

                // 关闭所有其他下拉框
                closeAllDropdowns();

                // 更新下拉列表选项
                populateDropdown(this, dropdown);

                // 计算并设置下拉框的位置
                positionDropdown(this, dropdown);

                // 显示当前下拉列表
                dropdown.style.display = &#x27;block&#x27;;
            });

            // 当select值改变时
            originalSelect.addEventListener(&#x27;change&#x27;, function () {
                // 如果下拉列表可见，更新选中状态
                var dropdown = this._customDropdown;
                if (dropdown.style.display === &#x27;block&#x27;) {
                    updateSelectedOption(this, dropdown);
                }
            });

            // 当窗口大小改变时重新定位所有可见的下拉框
            window.addEventListener(&#x27;resize&#x27;, function () {
                var visibleDropdowns = document.querySelectorAll(&#x27;.custom-select-dropdown[style*=&quot;display: block&quot;]&#x27;);
                for (var j = 0; j &lt; visibleDropdowns.length; j++) {
                    var select = visibleDropdowns[j]._originalSelect;
                    positionDropdown(select, visibleDropdowns[j]);
                }
            });

            // 当页面滚动时重新定位所有可见的下拉框
            document.addEventListener(&#x27;scroll&#x27;, function () {
                var visibleDropdowns = document.querySelectorAll(&#x27;.custom-select-dropdown[style*=&quot;display: block&quot;]&#x27;);
                for (var j = 0; j &lt; visibleDropdowns.length; j++) {
                    var select = visibleDropdowns[j]._originalSelect;
                    positionDropdown(select, visibleDropdowns[j]);
                }
            }, true); // 使用捕获阶段以捕获所有滚动事件

        })(selects[i], i);
    }

    // 点击页面其他区域时隐藏所有下拉列表
    document.addEventListener(&#x27;click&#x27;, closeAllDropdowns);
}

// 定位下拉框，确保其不会被屏幕边缘截断
function positionDropdown(select, dropdown) {
    var rect = select.getBoundingClientRect();
    var scrollTop = window.pageYOffset || document.documentElement.scrollTop;
    var scrollLeft = window.pageXOffset || document.documentElement.scrollLeft;

    // 计算下拉框的位置
    var top = rect.bottom + scrollTop;
    var left = rect.left + scrollLeft;
    // 检查是否有足够的空间向下展开
    var spaceBelow = window.innerHeight - rect.bottom;

    // 检查水平方向是否有足够的空间
    var dropdownWidth = dropdown.offsetWidth;
    var rightEdge = left + dropdownWidth;
    var viewportWidth = window.innerWidth + scrollLeft;

    // 如果会超出右边界，则向左调整
    if (rightEdge &gt; viewportWidth) {
        left = Math.max(scrollLeft, viewportWidth - dropdownWidth);
    }

    // 设置下拉框的位置
    dropdown.style.top = top + &#x27;px&#x27;;
    dropdown.style.left = left + &#x27;px&#x27;;
}

// 关闭所有下拉列表
function closeAllDropdowns() {
    var dropdowns = document.querySelectorAll(&#x27;.custom-select-dropdown&#x27;);
    for (var i = 0; i &lt; dropdowns.length; i++) {
        dropdowns[i].style.display = &#x27;none&#x27;;
    }
}

// 填充下拉列表
function populateDropdown(select, dropdown) {
// 清空下拉列表
    dropdown.innerHTML = &#x27;&#x27;;

// 获取原始select元素的计算样式
    var selectStyle = window.getComputedStyle(select);

// 将字体相关样式应用到下拉框本身
    dropdown.style.fontSize = selectStyle.fontSize;
    dropdown.style.fontFamily = selectStyle.fontFamily;
    dropdown.style.fontWeight = selectStyle.fontWeight;
    dropdown.style.lineHeight = selectStyle.lineHeight;
    dropdown.style.color = selectStyle.color;

// 重新填充选项
    for (var j = 0; j &lt; select.options.length; j++) {
        (function (optionIndex) {
            var option = document.createElement(&#x27;div&#x27;);
            option.className = &#x27;custom-select-option&#x27;;
            option.textContent = select.options[optionIndex].text;
            option.dataset.value = select.options[optionIndex].value;
            option.style.padding = &#x27;0px 2px&#x27;;
            option.style.cursor = &#x27;pointer&#x27;;

            // 应用与原始select相同的字体样式
            option.style.fontSize = selectStyle.fontSize;
            option.style.fontFamily = selectStyle.fontFamily;
            option.style.fontWeight = selectStyle.fontWeight;
            option.style.lineHeight = selectStyle.lineHeight;

            // 设置悬停效果
            option.onmouseover = function () {
                this.style.backgroundColor = &#x27;#f0f0f0&#x27;;
            };

            option.onmouseout = function () {
                if (optionIndex !== select.selectedIndex) {
                    this.style.backgroundColor = &#x27;#fff&#x27;;
                }
            };

            // 将当前选中项标记为活动状态
            if (optionIndex === select.selectedIndex) {
                option.style.backgroundColor = &#x27;#f0f0f0&#x27;;
            }

            // 设置点击事件
            option.onclick = function (e) {
                e.stopPropagation();
                select.selectedIndex = optionIndex;

                // 触发原始select的change事件
                var event = new Event(&#x27;change&#x27;, {bubbles: true});
                select.dispatchEvent(event);

                dropdown.style.display = &#x27;none&#x27;;
            };

            dropdown.appendChild(option);
        })(j);
    }
}

// 更新下拉列表选中状态
function updateSelectedOption(select, dropdown) {
    var options = dropdown.querySelectorAll(&#x27;.custom-select-option&#x27;);
    for (var i = 0; i &lt; options.length; i++) {
        if (i === select.selectedIndex) {
            options[i].style.backgroundColor = &#x27;#f0f0f0&#x27;;
        } else {
            options[i].style.backgroundColor = &#x27;#fff&#x27;;
        }
    }
}

// 清理函数 - 用于当元素从DOM中移除时清理对应的下拉框
function cleanupDropdowns() {
    var allDropdowns = document.querySelectorAll(&#x27;.custom-select-dropdown&#x27;);
    for (var i = 0; i &lt; allDropdowns.length; i++) {
        var dropdown = allDropdowns[i];
        var selectId = dropdown.getAttribute(&#x27;data-for-select&#x27;);
        var select = document.querySelector(&#x27;select[data-custom-id=&quot;&#x27; + selectId + &#x27;&quot;]&#x27;);

        // 如果找不到对应的select元素，则移除下拉框
        if (!select) {
            dropdown.remove();
        }
    }
}

// 初始化自定义下拉框
customizeSelects();

// 定期清理孤立的下拉框
setInterval(cleanupDropdowns, 5000);

// 为动态添加的select元素添加MutationObserver
var observer = new MutationObserver(function (mutations) {
    var needsUpdate = false;

    mutations.forEach(function (mutation) {
        if (mutation.type === &#x27;childList&#x27;) {
            needsUpdate = true;
        }
    });

    if (needsUpdate) {
        customizeSelects();
    }
});

observer.observe(document.body, {
    childList: true,
    subtree: true
});
```