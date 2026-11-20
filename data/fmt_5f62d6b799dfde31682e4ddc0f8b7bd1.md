
```python
match = re.search(pattern, rsp, re.DOTALL)

code_text = match.group(1) if match else rsp

return code_text
```

```python
class SimpleRunCode(Action):

    name: str = "SimpleRunCode"

    async def run(self, code_text: str):

        result = subprocess.run([sys.executable, "-c", code_text], capture_output=True, text=True)

        code_result = result.stdout

        logger.info(f"{code_result=}")

        return code_result
```

```python
class RunnableCoder(Role):

    name: str = "Alice"

    profile: str = "RunnableCoder"

    def __init__(self, **kwargs):

        super().__init__(**kwargs)

        self._init_actions([SimpleWriteCode, SimpleRunCode])

        self._set_react_mode(react_mode="by_order")

    async def _act(self) -> Message:

        logger.info(f"{self._setting}: ready to {self.rc.todo}")

        # By choosing the Action by order under the hood

        # todo will be first SimpleWriteCode() then SimpleRunCode()

        todo = self.rc.todo

        msg = self.get_memories(k=1)[0] # find the most k recent messagesA

        result = await todo.run(msg.content)

        msg = Message(content=result, role=self.profile, cause_by=type(todo))

        self.rc.memory.add(msg)

        return msg
```

```python
async def main():

    msg = "write a function that calculates the sum of a list"

    role = RunnableCoder()

    logger.info(msg)

    result = await role.run(msg)

    logger.info(result)

asyncio.run(main())
```

# 实现一个更复杂的Agent：技术文档助手

在前文中我们已经介绍了如何实现一个简单的agent帮我们生成代码并执行代码，下面我们将带领大家实现更复杂的agent，并向大家展示MetaGPT中关于agent的更多设计细节

现在试着想想怎么让大模型为我们写一篇技术文档？

可能想到的是，我们告诉大模型：“请帮我生成关于Mysql的技术文档”，他可能很快地就能帮你完成这项任务，但是受限于大模型自身的token限制，我们无法实现让他一次性就输出我们希望的一个完整的技术文档。

当然我们可以将我们的技术文档拆解成一个一个很小的需求，然后一个一个的提问，但是这样来说不仅费时，而且还需要人工一直去跟他交互，非常的麻烦，下面我们就将利用MetaGPT框架来解决这个问题

我们利用上文中提到的agent框架来拆解我们的需求

## 需求分析

因为token限制的原因，我们先通过 LLM 大模型生成教程的目录，再对目录按照二级标题进行分块，对于每块目录按照标题生成详细内容，最后再将标题和内容进行拼接，解决 LLM 大模型长文本的限制问题。

![](img/1ffddaa2f1b4d089ed655a23358334e6.png)

## 编写 `WriteDirectory` 动作

我们先来实现根据用户需求生成文章大纲的代码

```python
class WriteDirectory(Action):

    """Action class for writing tutorial directories.

    Args:

        name: The name of the action.

        language: The language to output, default is "Chinese".

        用于编写教程目录的动作类。

        参数：

        name：动作的名称。

        language：输出的语言，默认为"Chinese"。

    """

    name: str = "WriteDirectory"

    language: str = "Chinese"

    async def run(self, topic: str, *args, **kwargs) -> Dict:

        """Execute the action to generate a tutorial directory according to the topic.

        Args:

            topic: The tutorial topic.

        Returns:

            the tutorial directory information, including {"title": "xxx", "directory": [{"dir 1": ["sub dir 1", "sub dir 2"]}]}.

        根据主题执行生成教程目录的操作。

            参数：

            topic：教程主题。

            返回：

            教程目录信息，包括{"title": "xxx", "directory": [{"dir 1": ["sub dir 1", "sub dir 2"]}]}.

        """

        COMMON_PROMPT = """

        You are now a seasoned technical professional in the field of the internet. 

        We need you to write a technical tutorial with the topic "{topic}".

        您现在是互联网领域的经验丰富的技术专业人员。

        我们需要您撰写一个关于"{topic}"的技术教程。

        """

        DIRECTORY_PROMPT = COMMON_PROMPT + """

        Please provide the specific table of contents for this tutorial, strictly following the following requirements:

        1. The output must be strictly in the specified language, {language}.

        2. Answer strictly in the dictionary format like {{"title": "xxx", "directory": [{"dir 1": ["sub dir 1", "sub dir 2"]}, {"dir 2": ["sub dir 3", "sub dir 4"]}]}}.

        3. The directory should be as specific and sufficient as possible, with a primary and secondary directory.The secondary directory is in the array.

        4. Do not have extra spaces or line breaks.

        5. Each directory title has practical significance.

        请按照以下要求提供本教程的具体目录：

        1. 输出必须严格符合指定语言，{language}。

        2. 回答必须严格按照字典格式，如{{"title": "xxx", "directory": [{"dir 1": ["sub dir 1", "sub dir 2"]}, {"dir 2": ["sub dir 3", "sub dir 4"]}]}}。

        3. 目录应尽可能具体和充分，包括一级和二级目录。二级目录在数组中。

        4. 不要有额外的空格或换行符。

        5. 每个目录标题都具有实际意义。

        """

        prompt = DIRECTORY_PROMPT.format(topic=topic, language=self.language)

        resp = await self._aask(prompt=prompt)

        return OutputParser.extract_struct(resp, dict)
```

基本就是我们把自己的需求放入我们准备好的提示词模板里，询问大模型得到结果，然后我们对得到的内容做一个解析。（数据格式化）

```python
def extract_struct(cls, text: str, data_type: Union[type(list), type(dict)]) -> Union[list, dict]:

    """Extracts and parses a specified type of structure (dictionary or list) from the given text.

    The text only contains a list or dictionary, which may have nested structures.

    Args:

        text: The text containing the structure (dictionary or list).

        data_type: The data type to extract, can be "list" or "dict".

    Returns:

        - If extraction and parsing are successful, it returns the corresponding data structure (list or dictionary).

        - If extraction fails or parsing encounters an error, it throw an exception.

    返回：

    - 如果提取和解析成功，它将返回相应的数据结构（列表或字典）。

    - 如果提取失败或解析遇到错误，则抛出异常。

    Examples:

        >>> text = 'xxx [1, 2, ["a", "b", [3, 4]], {"x": 5, "y": [6, 7]}] xxx'

        >>> result_list = OutputParser.extract_struct(text, "list")

        >>> print(result_list)

        >>> # Output: [1, 2, ["a", "b", [3, 4]], {"x": 5, "y": [6, 7]}]

        >>> text = 'xxx {"x": 1, "y": {"a": 2, "b": {"c": 3}}} xxx'

        >>> result_dict = OutputParser.extract_struct(text, "dict")

        >>> print(result_dict)

        >>> # Output: {"x": 1, "y": {"a": 2, "b": {"c": 3}}}

    """

    # Find the first "[" or "{" and the last "]" or "}"

    start_index = text.find("[" if data_type is list else "{")

    end_index = text.rfind("]" if data_type is list else "}")

    if start_index != -1 and end_index != -1:

        # Extract the structure part

        structure_text = text[start_index : end_index + 1]

        try:

            # Attempt to convert the text to a Python data type using ast.literal_eval

            result = ast.literal_eval(structure_text)

            # Ensure the result matches the specified data type

            if isinstance(result, list) or isinstance(result, dict):

                return result

            raise ValueError(f"The extracted structure is not a {data_type}.")

        except (ValueError, SyntaxError) as e:

            raise Exception(f"Error while extracting and parsing the {data_type}: {e}")

    else:

        logger.error(f"No {data_type} found in the text.")

        return [] if data_type is list else {}
```

注释里给了解析的example，这里再提一下

```python
>>> text = 'xxx [1, 2, ["a", "b", [3, 4]], {"x": 5, "y": [6, 7]}] xxx'

>>> result_list = OutputParser.extract_struct(text, "list")
```
