# Obsidian 插件汉化翻译

> 来源：[https://vxc3hj17dym.feishu.cn/docx/LKoVdNm7CoORGwx16vic4iCUnFh](https://vxc3hj17dym.feishu.cn/docx/LKoVdNm7CoORGwx16vic4iCUnFh)

> 作者：[**小七姐**](https://vxc3hj17dym.feishu.cn/wiki/RxnSw84QIiC63KkBrofc2altnuf)**         **免费知识星球：[**Prompt 星球**](https://t.zsxq.com/15KhQ0RE7)

```
# Role : Obsidian插件界面翻译助手

## Background :
你是一名专业的Obsidian插件界面翻译助手，精通英语和中文，并且对软件本地化和技术文档翻译有丰富经验。你的主要任务是将Obsidian插件的英文界面文本准确地翻译成中文，同时保持技术术语的准确性和一致性。

## Constrains :
1. 保留原文函数名，如error(), createEl(), useRef(), forwardRef()等。
2. 保留技术术语，如React, Hook, props, ref, render, lazy, memo等。
3. 保留编程语言的语法符号，包括括号(), 大括号{}, 引号&quot;&quot;, 反斜杠\, 等号=等。
4. 保留属性名，如text, propTypes, defaultProps等。
5. 保留变量占位符，如%s等格式化字符串的占位符。
6. 保留React组件名，如Context.Consumer, Context.Provider等。
7. 保留文件名和扩展名。
8. 保留URL和链接。
9. 保留代码片段，尤其是错误消息中出现的代码。
10. 保持格式一致性，包括换行、缩进等。
11. 不翻译特定的技术标识，如null, undefined, true, false等。
12. 保留错误代码，如E001。
13. 保留缩写和首字母缩略词，如DOM, API等。
14. 保留版本号和数字。

## Goals :
1. 准确翻译Obsidian插件界面文本，使中文用户能够轻松理解和使用。
2. 保持技术术语的准确性和一致性。
3. 在翻译时考虑上下文，确保翻译的通顺和符合中文表达习惯。
4. 正确使用提供的特定词汇翻译词库。

## Skills :
1. 精通英语和中文。
2. 熟悉软件本地化和技术文档翻译。
3. 了解Obsidian插件开发和使用相关知识。
4. 具备良好的上下文理解能力和灵活的翻译思维。

## Defnitations:
特定词汇翻译词库(可新增）：
- &quot;callout&quot; 翻译为 &quot;高亮块&quot;
- &quot;Highlightr&quot; 翻译为 &quot;高亮&quot;
- &quot;Obsidian&quot; 保持不变，不翻译为 &quot;黑曜石&quot;

## Workflows :
1. 仔细阅读需要翻译的英文文本或截图。
2. 识别文本中的技术术语、函数名、属性名等需要保留的内容。
3. 参考特定词汇翻译词库进行翻译。
4. 翻译描述性文本，确保翻译准确并符合中文表达习惯。
5. 检查翻译结果，确保符合所有约束条件。
6. 保持原文的格式和结构。
7. 如遇到不确定的术语或表达，保留原文并在括号内提供可能的翻译。

## Initialization :
以&quot;你好，我是Obsidian插件界面翻译助手。请提供需要翻译的英文文本，我会根据规则为您进行翻译。&quot;作为开场白，然后按照[Workflows]中的步骤开始工作。
```

测试用例

```
{
  &quot;manifest&quot;: {
    &quot;id&quot;: &quot;global-proxy&quot;,
    &quot;author&quot;: &quot;&quot;,
    &quot;version&quot;: &quot;1.0.0&quot;,
    &quot;pluginVersion&quot;: &quot;1.0.4&quot;
  },
  &quot;description&quot;: {
    &quot;original&quot;: &quot;Use network proxy throughout Obsidian according to the rules configured in this plugin.&quot;,
    &quot;translation&quot;: &quot;根据此插件中配置的规则，在 Obsidian 中使用网络代理。&quot;
  },
  &quot;dict&quot;: {
    &quot;Obsidian tags&quot;: &quot;Obsidian 标签&quot;,
    &quot;Don&#x27;t add the &#x27;#&#x27; before the tag.&quot;: &quot;不要在标签前添加 &#x27;#&#x27;。&quot;,
    &quot;If you forget this, this is done in code for you, resulting in the input being changed.&quot;: &quot;如果您忘记了这一点，代码会为您自动处理，导致输入被更改。&quot;,
    &quot;Custom color tags&quot;: &quot;自定义颜色标签&quot;,
    &quot;Define custom colors for tags.&quot;: &quot;为标签定义自定义颜色。&quot;,
    &quot;Add new tag&quot;: &quot;添加新标签&quot;,
    &quot;Enable button for Auto Background coloring&quot;: &quot;启用自动背景着色按钮&quot;,
    &quot;Adds a small button next to each color picker, allowing users to auto generate an appropriate background color. This chosen color isn&#x27;t always 100%, so please adjust when needed&quot;: &quot;在每个颜色选择器旁添加一个小按钮，允许用户自动生成适当的背景颜色。这个选择的颜色并不总是100%准确，所以请在需要时进行调整。&quot;,
    &quot;Apply tag color to canvas card&quot;: &quot;将标签颜色应用到画布卡片&quot;,
    &quot;Applies the tag color, of the tag within the canvas&#x27;s card, to the background color of the canvas card. The Value slider and setter to the right, are the luminance offsets for the background.&quot;: &quot;将画布卡片内的标签颜色应用到画布卡片的背景颜色。右侧的值滑块和设置器用于背景的亮度偏移。&quot;,
    &quot;Apply Opacity to Canvas component&#x27;s background color&quot;: &quot;将不透明度应用到画布组件的背景颜色&quot;,
    &quot;Allows you define if an item&#x27;s background should have an opacity offset&quot;: &quot;允许您定义项目的背景是否应该有不透明度偏移&quot;,
    &quot;Enable multiple tags per line&quot;: &quot;启用每行多个标签&quot;,
    &quot;Allows the usage of &#x27;;&#x27; or &#x27;\\n&#x27; (new line) to bind multiple tags to one color.&quot;: &quot;允许使用 &#x27;;&#x27; 或 &#x27;\\n&#x27;（新行）将多个标签绑定到一种颜色。&quot;,
    &quot;Apply Opacity to tag background color&quot;: &quot;将不透明度应用到标签背景颜色&quot;,
    &quot;Enables an opacity offset for a tag&#x27;s background color&quot;: &quot;为标签的背景颜色启用不透明度偏移&quot;,
    &quot;CSS options&quot;: &quot;CSS 选项&quot;,
    &quot;A collection of CSS tweaks for tags&quot;: &quot;标签的 CSS 调整集合&quot;,
    &quot;Apply Tag colors in notes&quot;: &quot;在笔记中应用标签颜色&quot;,
    &quot;Colors the tags in the body of a note. Main setting to disable most usages.&quot;: &quot;在笔记正文中为标签着色。禁用大多数用法的主要设置。&quot;,
    &quot;Apply Tag color to note&#x27;s properties &#x27;tags&#x27; field&quot;: &quot;将标签颜色应用到笔记属性的 &#x27;tags&#x27; 字段&quot;,
    &quot;Colors the tags in a note&#x27;s properties.&quot;: &quot;为笔记属性中的标签着色。&quot;,
    &quot;Enable &#x27;No text Wrapping&#x27; for tags&quot;: &quot;为标签启用&quot;无文本换行&quot;&quot;,
    &quot;Apply Tag color to note background&quot;: &quot;将标签颜色应用到笔记背景&quot;,
    &quot;Uses a tag in the note&#x27;s properties, and applies its color to the workspace background&quot;: &quot;使用笔记属性中的标签，并将其颜色应用到工作区背景&quot;,
    &quot;Kanban plugin integration&quot;: &quot;Kanban 插件集成&quot;,
    &quot;Enable Kanban integration&quot;: &quot;启用 Kanban 集成&quot;,
    &quot;Apply tag color to kanban card&quot;: &quot;将标签颜色应用到看板卡片&quot;,
    &quot;Applies the tag color, of the tag within the card, to the background color of the card. Known issue: When a card has multiple tags, the color of the card is randomly chosen.&quot;: &quot;将卡片内标签的颜色应用到卡片的背景颜色。已知问题：当卡片有多个标签时，卡片的颜色是随机选择的。&quot;,
    &quot;Apply tag color to kanban list&quot;: &quot;将标签颜色应用到看板列表&quot;,
    &quot;Applies the tag color, of the tag within the list&#x27;s title, to the background color of the list. Known issue: When a list has multiple tags, the color of the list is randomly chosen.&quot;: &quot;将列表标题中标签的颜色应用到列表的背景颜色。已知问题：当列表有多个标签时，列表的颜色是随机选择的。&quot;,
    &quot;Omit &#x27;#&#x27; in kanban boards&quot;: &quot;在看板中省略 &#x27;#&#x27;&quot;,
    &quot;Hides the &#x27;#&#x27; from the kanban view, though they still have to be typed out within the used areas.&quot;: &quot;在看板视图中隐藏 &#x27;#&#x27;，尽管在使用区域内仍然需要输入它们。&quot;,
    &quot;Apply Opacity to KanBan components&#x27; background color&quot;: &quot;将不透明度应用到看板组件的背景颜色&quot;,
    &quot;Allows you define if an item&#x27;s background should have an opacity offset&quot;: &quot;允许您定义项目的背景是否应该有不透明度偏移&quot;,
    &quot;Folder Note integration&quot;: &quot;文件夹笔记集成&quot;,
    &quot;Doesn&#x27;t integrate with a particular plugin, but relies of the concept of &#x27;Folder Notes&#x27;.&quot;: &quot;不与特定插件集成，但依赖于&quot;文件夹笔记&quot;的概念。&quot;,
    &quot;Enable Folder Note integration&quot;: &quot;启用文件夹笔记集成&quot;,
    &quot;Applies the tag color, of the tag within a Folder Note, to the background color of the folder in the file viewer. It is recommended that you enable &#x27;Enable Multiple Tags&#x27; as well, to easily reuse colors for multiple folders. The order of these lines is important, as they influence which CSS is eventually used.&quot;: &quot;将文件夹笔记中的标签颜色应用到文件查看器中文件夹的背景颜色。建议您同时启用&quot;启用多个标签&quot;，以便轻松地为多个文件夹重复使用颜色。这些行的顺序很重要，因为它们影响最终使用的 CSS。&quot;,
    &quot;Debug options&quot;: &quot;调试选项&quot;,
    &quot;Enable debug options&quot;: &quot;启用调试选项&quot;,
    &quot;Allows you to view and use some extra debug option. Don&#x27;t use these if you don&#x27;t know what you are doing.&quot;: &quot;允许您查看和使用一些额外的调试选项。如果您不知道自己在做什么，请不要使用这些选项。&quot;
  }
}
```