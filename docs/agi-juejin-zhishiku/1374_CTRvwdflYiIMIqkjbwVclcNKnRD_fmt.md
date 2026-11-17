
# 大模型`Prompt`提示设计简介(2)：有效的建议

> 来源：[https://agijuejin.feishu.cn/docx/H5oKdte9QowbyGxRIQwcJiAgnAe](https://agijuejin.feishu.cn/docx/H5oKdte9QowbyGxRIQwcJiAgnAe)

> 作者：哎呀AIYA

> 原文：`https://mp.weixin.qq.com/s/SiVypDgDXZYCwpRYVNPg1Q`

本文将介绍在设计`Prompt`时可以采用的一些常规设计策略。虽然 `Prompt` 设计方法没有正确或错误一说，但可以使用常见的策略来影响模型的回答。严格地测试和评估对于优化模型性能仍然至关重要。

大语言模型（`LLM`）使用大量文本数据进行训练，以学习语言单元之间的模式和关系。在获得一些文本（`Prompt`）后，语言模型可以预测接下的内容，就像一个复杂的自动补全工具。因此，在设计 `Prompt` 时，请考虑可能影响模型预测结果的不同因素。

# 提供清晰而具体的说明

### 通过向模型提供关于要做什么的指令，可以有效且高效地自定义模型行为。确保提供的指令清晰而具体。指令可以很简单，比如是一个分步说明列表；也可以很复杂，例如反映用户的思考过程和思维模式。

### 划重点：

+   明确向模型提供有关要做什么的指令。

+   说明应清晰而具体。

+   明确指定限制条件或格式要求。

### 定义要执行的任务

详细描述希望模型执行的任务。以下提示告诉模型提取一段文本的摘要：

```
Summarize this text.
Text: A quantum computer exploits quantum mechanical phenomena to perform calculations exponentially
faster than any modern traditional computer. At very tiny scales, physical matter acts as both
particles and as waves, and quantum computing uses specialized hardware to leverage this behavior.
The operating principles of quantum devices is beyond the scope of classical physics. When deployed
at scale, quantum computers could be used in a wide variety of applications such as: in
cybersecurity to break existing encryption methods while helping researchers create new ones, in
meteorology to develop better weather forecasting etc. However, the current state of the art quantum
computers are still largely experimental and impractical.
```

模型结果：

```
Quantum computers utilize quantum mechanical phenomena to perform computations significantly faster
than traditional computers. By exploiting the particle-wave duality of matter at tiny scales,
quantum computers leverage specialized hardware to solve problems beyond classical physics.
Potential applications include breaking encryption, developing better weather forecasting, and more.
However, current quantum computers are still largely experimental and impractical.
```

### 指定限制条件

明确指定有关读取`Prompt`或生成回答的限制条件。可以告诉模型要做什么和不应该做什么。在上一个示例中，摘要比原始文本短，但您可能想要更短的摘要。在这种情况下，可以在`Prompt`中指定一个限制条件，以限制摘要长度(只输出两句话)：

```
Summarize this text in two sentences.
Text: A quantum computer exploits quantum mechanical phenomena to perform calculations exponentially
faster than any modern traditional computer. At very tiny scales, physical matter acts as both
particles and as waves, and quantum computing uses specialized hardware to leverage this behavior.
The operating principles of quantum devices is beyond the scope of classical physics. When deployed
at scale, quantum computers could be used in a wide variety of applications such as: in
cybersecurity to break existing encryption methods while helping researchers create new ones, in
meteorology to develop better weather forecasting etc. However, the current state of the art quantum
computers are still largely experimental and impractical.
```

模型结果：

```
Quantum computers use quantum mechanical phenomena to perform calculations much faster than
traditional computers. They have the potential to revolutionize various fields, but current quantum
computers are still experimental and impractical.
```

### 定义回答格式

可以提供指定回答格式的说明。例如，可以要求将回答格式设置为表格、项目符号列表、电梯间推销、关键字、句子或段落。以下`Prompt`指示模型使用项目符号列出文本中的关键信息：

```
Summarize this text as bullets points of key information.
Text: A quantum computer exploits quantum mechanical phenomena to perform calculations exponentially
faster than any modern traditional computer. At very tiny scales, physical matter acts as both
particles and as waves, and quantum computing uses specialized hardware to leverage this behavior.
The operating principles of quantum devices is beyond the scope of classical physics. When deployed
at scale, quantum computers could be used in a wide variety of applications such as: in
cybersecurity to break existing encryption methods while helping researchers create new ones, in
meteorology to develop better weather forecasting etc. However, the current state of the art quantum
computers are still largely experimental and impractical.
```

模型结果：

```
- Quantum computers leverage quantum mechanical phenomena for exponentially faster calculations than
traditional computers.
- Quantum devices utilize specialized hardware to exploit tiny-scale matter behavior as both
particles and waves.
- Their operating principles transcend classical physics.
- Potential applications include cybersecurity, weather forecasting, and more.
- Current quantum computers are largely experimental and impractical.
```

指定格式在实际项目中是非常有用的操作，这样可以便于我们对结果进行提取和加工，满足下一步的业务需求。

# 添加少样本示例

可以在`Prompt`中添加示例，以向模型示范想要的结果。模型会尝试识别示例中的模式和关系，并在生成回答时应用它们。包含少量示例的提示称为 少样本提示 ，而不提供示例的提示称为 零样本提示 。  少样本提示通常用于规定模型回答的 格式、措辞、范围界定或一般模式 。使用具体而多样的示例来帮助模型缩小其重点并生成更准确的结果。

建议始终在`Prompt`中包含少样本示例。不含少样本示例的`Prompt`可能效果不大，因为它们向模型展示了如何应用指令。事实上，如果示例足够清晰地显示当前任务，则可以从提示中移除指令。

### 划重点：

+   在`Prompt`中包含提示回答示例有助于模型学习如何回答。

+   向模型提供要遵循的模式示例，而不是应避免的模式示例。

+   尝试在提示中添加不同数量的示例。具体取决于模型，示例太少会导致无法有效改变模型行为，示例太多会导致模型过拟合。

+   在各个示例之间使用一致的格式

### 零样本提示与少样本提示

以下零样本提示要求模型选择最佳解释。

```
Please choose the best explanation to the question:

Question: How is snow formed?
Explanation1: Snow is formed when water vapor in the air freezes into ice crystals in the
atmosphere, which can combine and grow into snowflakes as they fall through the atmosphere and
accumulate on the ground.
Explanation2: Water vapor freezes into ice crystals forming snow.
Answer:
```

模型结果：

```
Explanation1
```

如果您的用例要求模型生成简洁的回答，您可以在提示中添加偏好简洁回答的示例。

以下提示提供了两个偏好较短解释的示例。在回答中，您可以看到这些示例引导模型选择较短的解释（解释 `Explanation2`），而不是像之前那样较长的解释（解释 `Explanation1`）。

```
Please choose the best explanation to the question:

Question: Why is sky blue?
Explanation1: The sky appears blue because of Rayleigh scattering, which causes shorter blue
wavelengths of light to be scattered more easily than longer red wavelengths, making the sky look
blue.
Explanation2: Due to Rayleigh scattering effect.
Answer: Explanation2

Question: What is the cause of earthquakes?
Explanation1: Sudden release of energy in the Earth&#x27;s crust.
Explanation2: Earthquakes happen when tectonic plates suddenly slip or break apart, causing a
release of energy that creates seismic waves that can shake the ground and cause damage.
Answer: Explanation1

Question: How is snow formed?
Explanation1: Snow is formed when water vapor in the air freezes into ice crystals in the
atmosphere, which can combine and grow into snowflakes as they fall through the atmosphere and
accumulate on the ground.
Explanation2: Water vapor freezes into ice crystals forming snow.
Answer:
```

模型结果：

```
Explanation2
```

### 找到最佳示例数量

您可以改变提示中提供的 示例数量来进行实验 ，以获得最想要的结果。PaLM 和 Gemini 等模型通常可以通过少量示例来学习模式，但您可能需要进行多次实验，以找到可产生所需结果的示例数量。对于 BERT 等较简单的模型，您可能需要更多示例。但是，如果您添加太多示例，模型的回答可能会与示例过拟合。

### 使用示例肯定模式而不是否定模式

使用示例向模型展示要遵循的模式比使用示例向模型展示要避免的否定模式更有效。

### 否定模式

```
Don&#x27;t end haikus with a question:
Haiku are fun
A short and simple poem
Don&#x27;t you enjoy them?
```

### 肯定模式

```
Always end haikus with an assertion:
Haiku are fun
A short and simple poem
A joy to write
```

在`Prompt`中，我们使用肯定语句的效果比否定语句更好，应该告诉模型做什么，而不是要求他不要做什么。

### 在各个示例之间使用一致的格式

确保少样本示例的结构和格式相同，以避免回答具有不需要的格式。在`Prompt`中添加少样本示例的主要目标之一是向模型展示回答格式。因此，必须确保在所有示例中使用一致的格式，尤其是注意 `XML` 标记、空格、换行符和示例分隔符。

# 添加上下文信息

可以在`Prompt`中添加指令和模型解决问题所需的信息，而不是假设模型具备所有必要信息。这些上下文信息可帮助模型了解您要求其执行的操作的限制和详细信息。

### 划重点：

+   在希望模型在生成回答时使用的`Prompt`中添加信息（上下文）。

+   为模型提供有关如何使用上下文信息的指令。

以下示例要求模型提供路由器的问题排查指导：



```
1. **Check the Cables:**
- Make sure that all cables are securely connected to your Google Wifi router and modem.
- Try replacing the cables with new ones to ensure they are not faulty.

2. **Reboot the Router:**
- Unplug your Google Wifi router and modem from the power outlet.
- Wait for at least 30 seconds.
- Plug the modem and router back in and wait for them to start up.

3. **Factory Reset the Router:**
- Resetting the router will restore it to its default settings.
- Find the small reset button on the back of the router.
- Press and hold the reset button with a paper clip or similar object for 10 seconds.
- Release the button and wait for the router to restart.
- If you've tried the above steps and the issue persists, contact Google support for further assistance.
```

模型结果：

```
1. **Check the Cables:**- Make sure that all cables are securely connected to your Google Wifi router and modem.- Try replacing the cables with new ones to ensure they are not faulty.

2. **Reboot the Router:**- Unplug your Google Wifi router and modem from the power outlet.- Wait for at least 30 seconds.- Plug the modem and router back in and wait for them to start up.

3. **Factory Reset the Router:**- Resetting the router will restore it to its default settings.- Find the small reset button on the back of the router.- Press and hold the reset button with a paper clip or similar object for 10 seconds.- Release the button and wait for the router to restart.- If you've tried the above steps and the issue persists, contact Google support for further assistance.
```

这个回答看起来是通用的问题排查信息，并非特定于路由器或 LED 指示灯的状态。

如需针对特定路由器自定义回答，可以在Prompt中添加该路由器的问题排查指南作为上下文，供模型在回答时参考。

```
Answer the question using the text below. Respond with only the text provided.
Question: What should I do to fix my disconnected wifi? The light on my Google Wifi router is yellow and blinking slowly.

Text:
Color: Slowly pulsing yellow
What it means: There is a network error.
What to do:
Check that the Ethernet cable is connected to both your router and your modem and both devices are turned on. You might need to unplug and plug in each device again.

Color: Fast blinking yellow
What it means: You are holding down the reset button and are factory resetting this device.
What to do:
If you keep holding down the reset button, after about 12 seconds, the light will turn solid yellow. Once it is solid yellow, let go of the factory reset button.

Color: Solid yellow
What it means: Router is factory resetting.
What to do:
This can take up to 10 minutes. When it's done, the device will reset itself and start pulsing white, letting you know it's ready for setup.

Color: Solid red
What it means: Something is wrong.
What to do:
Critical failure. Factory reset the router. If the light stays red, contact Wifi customer support.
响应：
```

模型回答：

```
Check that the Ethernet cable is connected to both your router and your modem and both devices are turned on. You might need to unplug and plug in each device again.
```

由于大模型的能力限制，使用上下文是目前比较火的应用领域，可以用来加入领域知识回答专有领域的问题；也可以加入上下文，限制回答的范围等。

## 添加前缀

前缀是添加到Prompt内容中的字词或短语，有多种用途，具体取决于前缀的位置：

+   **输入前缀**：为输入添加前缀可向模型指示输入中在语义上有意义的部分。例如，前缀“英语：”和“法语：”区分两种不同的语言。

+   **输出前缀**：虽然输出由模型生成，也可以在Prompt中为输出添加前缀。输出前缀可为模型提供有关预期回答的信息。例如，输出前缀“JSON：”告诉模型输出应采用 JSON 格式。

+   **示例前缀**：在少样本Prompt中，为示例添加前缀提供了模型在生成输出时可以使用的标签，使解析输出内容变得更加容易。

在以下示例中，“Text：”是输入前缀，“The answer is：”是输出前缀。

```
Classify the text as one of the following categories.
- large
- small
Text: Rhino
The answer is: large
Text: Mouse
The answer is: small
Text: Snail
The answer is: small
Text: Elephant
The answer is:
```

模型回答：

```
large
```

使用前缀可以明确的告诉模型，后面内容块作用，在实践中尽量要提供出来。

内容比较多，本篇文章介绍到这，下篇文章我们继续。

如果对内容有什么疑问和建议可以私信和留言，也可以添加我加入大模型交流群，一起讨论大模型在创作、RAG和agent中的应用。

好了，这就是我今天想分享的内容。如果你对大模型应用感兴趣，别忘了点赞、关注噢~

## 往期推荐

+   大模型Prompt提示设计简介(1)

+   详解GPT-4o的收费设计，为什么输出token贵？

+   大模型LLM的定位器Attention自注意力|兼看Qwen2参数构成

+   一文读懂token到底是什么鬼，消耗那么多钱？
