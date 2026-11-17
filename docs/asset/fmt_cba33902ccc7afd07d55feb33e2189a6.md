
# 多智能体系统提示词与代码执行示例

## 系统提示词（System Message）

```
You are a helpful AI assistant.
Solve tasks using your coding and language skills.
In the following cases, suggest python code (in a python coding block) or shell script (in a sh coding block) for the user to execute.
    1. When you need to collect info, use the code to output the info you need, for example, browse or search the web, download/read a file, print the content of a webpage or a file, get the current date/time, check the operating system. After sufficient info is printed and the task is ready to be solved based on your language skill, you can solve the task by yourself.
    2. When you need to perform some task with code, use the code to perform the task and output the result. Finish the task smartly.
Solve the task step by step if you need to. If a plan is not provided, explain your plan first. Be clear which step uses code, and which step uses your language skill.
When using code, you must indicate the script type in the code block. The user cannot provide any other feedback or perform any other action beyond executing the code you suggest. The user can&#x27;t modify your code. So do not suggest incomplete code which requires users to modify. Don&#x27;t use a code block if it&#x27;s not intended to be executed by the user.
If you want the user to save the code in a file before executing it, put # filename: &lt;filename&gt; inside the code block as the first line. Don&#x27;t include multiple code blocks in one response. Do not ask users to copy and paste the result. Instead, use &#x27;print&#x27; function for the output when relevant. Check the execution result returned by the user.
If the result indicates there is an error, fix the error and output the code again. Suggest the full code instead of partial code or code changes. If the error can&#x27;t be fixed or if the task is not solved even after the code is executed successfully, analyze the problem, revisit your assumption, collect additional info you need, and think of a different approach to try.
When you find an answer, verify the answer carefully. Include verifiable evidence in your response if possible.
Reply &quot;TERMINATE&quot; in the end when everything is done.
    You have access to the following user defined functions. They can be accessed from the module called `functions` by their function names.

For example, if there was a function called `foo` you could import it by writing `from functions import foo`

def get_stock_prices(stock_symbols, start_date, end_date):
    &quot;&quot;&quot;Get the stock prices for the given stock symbols between
        the start and end dates.

        Args:
            stock_symbols (str or list): The stock symbols to get the
            prices for.
            start_date (str): The start date in the format 
            &#x27;YYYY-MM-DD&#x27;.
            end_date (str): The end date in the format &#x27;YYYY-MM-DD&#x27;.

        Returns:
            pandas.DataFrame: The stock prices for the given stock
            symbols indexed by date, with one column per stock 
            symbol.
    &quot;&quot;&quot;
    ...

def plot_stock_prices(stock_prices, filename):
    &quot;&quot;&quot;Plot the stock prices for the given stock symbols.

        Args:
            stock_prices (pandas.DataFrame): The stock prices for the 
            given stock symbols.
    &quot;&quot;&quot;
    ...
```

## 提示词版本对比（2.0 vs 1.0）

> 2.0版本的提示词相较于1.0版本有以下几个优点：

1. **任务描述更详细**：2.0版本详细描述了如何解决任务，包括收集信息、执行任务、逐步解决问题的步骤等。这使得任务的解决方案更清晰，更易于理解和执行。

1. **代码执行的明确指示**：2.0版本明确指出了在需要用户执行代码时，应该使用完整的代码块，并且不允许用户修改代码。这避免了用户对代码进行错误修改的可能性，确保代码的正确执行。

1. **文件保存指示**：2.0版本在需要保存代码到文件时，提供了明确的指示（使用`# filename: <filename>`）。这让用户知道需要将代码保存到特定文件中，从而更好地组织和管理代码。

1. **错误处理和调试**：2.0版本详细说明了如何处理错误，包括检查执行结果、修复错误、提供完整代码、分析问题以及尝试不同的方法。这使得错误处理过程更加系统化和高效。

1. **验证答案**：2.0版本强调了在找到答案后要仔细验证答案，并尽可能提供可验证的证据。这提高了答案的可靠性和准确性。

1. **内置函数的说明**：2.0版本提供了用户定义函数的访问示例，并解释了如何导入和使用这些函数。这为用户提供了更清晰的指导，帮助他们更好地利用现有的工具和函数。

1. **统一的结束标志**：2.0版本在任务完成后使用`TERMINATE`作为统一的结束标志，明确指示任务的结束。这使得任务的开始和结束更加清晰。

> 总体来说，2.0版本的提示词更详细、更系统化，并且提供了更明确的指导和错误处理步骤，使得用户在解决任务时更加高效和准确。

## 智能体实例化

### Let's update the agents with the new system message

```
code_writer_agent = ConversableAgent(
    name=&quot;code_writer_agent&quot;,
    system_message=code_writer_agent_system_message,
    llm_config=llm_config,
    code_execution_config=False,
    human_input_mode=&quot;NEVER&quot;,
)
```

> 这段Python代码创建了一个`ConversableAgent`实例，并将其赋值给变量`code_writer_agent`。
> `ConversableAgent`可能是一个用于创建可对话的代理的类。在这个实例化过程中，传入了以下参数：
> 1. `name="code_writer_agent"`：这个参数设置了代理的名称为`"code_writer_agent"`。
> 1. `system_message=code_writer_agent_system_message`：这个参数传入了一个变量`code_writer_agent_system_message`，这可能是用于配置代理的系统消息。
> 1. `llm_config=llm_config`：这个参数传入了一个字典`llm_config`，这可能是用于配置代理的一些设置。
> 1. `code_execution_config=False`：这个参数设置了`code_execution_config`为`False`，这可能表示不使用某种代码执行配置。
> 1. `human_input_mode="NEVER"`：这个参数设置了人类输入模式为`"NEVER"`，这可能表示这个代理从不接受人类的输入。
> 所以，这段代码的作用是创建了一个名为`"code_writer_agent"`的可对话的代理，这个代理可能用于编写代码，从不接受人类的输入。

```
code_executor_agent = ConversableAgent(
    name=&quot;code_executor_agent&quot;,
    llm_config=False,
    code_execution_config={&quot;executor&quot;: executor},
    human_input_mode=&quot;ALWAYS&quot;,
    default_auto_reply=
    &quot;Please continue. If everything is done, reply &#x27;TERMINATE&#x27;.&quot;,
)
```

> 这段Python代码创建了一个`ConversableAgent`实例，并将其赋值给变量`code_executor_agent`。
> `ConversableAgent`可能是一个用于创建可对话的代理的类。在这个实例化过程中，传入了以下参数：
> 1. `name="code_executor_agent"`：这个参数设置了代理的名称为`"code_executor_agent"`。
> 1. `llm_config=False`：这个参数设置了`llm_config`为`False`，这可能表示不使用某种配置。
> 1. `code_execution_config={"executor": executor}`：这个参数传入了一个字典，其中包含了一个键值对`"executor": executor`。这可能表示这个代理使用`executor`实例来执行代码。
> 1. `human_input_mode="ALWAYS"`：这个参数设置了人类输入模式为`"ALWAYS"`，这可能表示这个代理总是接受人类的输入。
> 1. `default_auto_reply="Please continue. If everything is done, reply 'TERMINATE'."`：这个参数设置了默认的自动回复为`"Please continue. If everything is done, reply 'TERMINATE'."`。
> 所以，这段代码的作用是创建了一个名为`"code_executor_agent"`的可对话的代理，这个代理可能用于执行代码，总是接受人类的输入，其默认的自动回复为`"Please continue. If everything is done, reply 'TERMINATE'."`。

## 对话启动

```
chat_result = code_executor_agent.initiate_chat(
    code_writer_agent,
    message=f&quot;Today is {today}.&quot;
    &quot;Download the stock prices YTD for NVDA and TSLA and create&quot;
    &quot;a plot. Make sure the code is in markdown code block and &quot;
    &quot;save the figure to a file stock_prices_YTD_plot.png.&quot;,
)
```

> 这段Python代码调用了`code_executor_agent`实例的`initiate_chat`方法，以`code_writer_agent`为对话对象，初始化了一场对话。在这个过程中，传入了以下参数：
> 1. `code_writer_agent`：这个参数设置了对话的对象为`code_writer_agent`。
> 1. `message=f"Today is {today}." "Download the stock prices YTD for NVDA and TSLA and create" "a plot. Make sure the code is in markdown code block and " "save the figure to a file stock_prices_YTD_plot.png."`：这个参数设置了对话的初始消息。这个消息首先显示了当前的日期，然后是一个指示，要求下载NVDA和TSLA年初至今的股票价格，并创建一个图表，确保代码在Markdown代码块中，并将图表保存为`stock_prices_YTD_plot.png`文件。
> `initiate_chat`方法的返回值被赋值给了变量`chat_result`。
> 所以，这段代码的作用是初始化了一场由`code_executor_agent`发起、与`code_writer_agent`进行的对话，对话的初始消息是一个包含指示的字符串，这个指示要求下载NVDA和TSLA年初至今的股票价格，并创建一个图表，确保代码在Markdown代码块中，并将图表保存为`stock_prices_YTD_plot.png`文件。

## 结果可视化

```
Image(os.path.join(&quot;coding&quot;, &quot;stock_prices_YTD_plot.png&quot;))
```

> 这段Python代码使用了`os.path.join`函数和`IPython.display.Image`类。
> `os.path.join`函数用于连接路径。在这个例子中，它连接了字符串`"coding"`和`"stock_prices_YTD_plot.png"`，生成了一个新的路径。
> `IPython.display.Image`类用于在Jupyter notebook中显示图片。在这个例子中，它创建了一个新的`Image`实例，该实例用于显示路径为`"coding/stock_prices_YTD_plot.png"`的图片。
> 所以，这段代码的作用是在Jupyter notebook中显示路径为`"coding/stock_prices_YTD_plot.png"`的图片。

![](img/9acbd8c88c6fffff6e2a71b8efe1a4b1.png)

> **Note**: The agent will automatically save the code in a `.py` file and the plot in a `.png` file. To access and check the files generated by the agents, go to the File menu and select Open.... Then, open the folder named `coding` to find all the generated files.

# Lesson 6: 编排多编码智能体实现复杂金融分析

## Setup
