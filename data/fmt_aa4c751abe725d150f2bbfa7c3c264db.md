
```python
def main(top_p: float, temperature: float, prompt_text: str):
    if 'ci_history' not in st.session_state:
        st.session_state.ci_history = []

    history: list[Conversation] = st.session_state.ci_history

    for conversation in history:
        conversation.show()

    if prompt_text:
        prompt_text = prompt_text.strip()
        role = Role.USER
        append_conversation(Conversation(role, prompt_text), history)

        input_text = preprocess_text(
            SYSTEM_PROMPT,
            None,
            history,
        )
        print("=== Input:")
        print(input_text)
        print("=== History:")
        print(history)

        placeholder = st.container()
        message_placeholder = placeholder.chat_message(name="assistant", avatar="assistant")
        markdown_placeholder = message_placeholder.empty()

        for _ in range(5):
            output_text = ''
            for response in client.generate_stream(
                system=SYSTEM_PROMPT,
                tools=None,
                history=history,
                do_sample=True,
                max_length=MAX_LENGTH,
                temperature=temperature,
                top_p=top_p,
                stop_sequences=[str(r) for r in (Role.USER, Role.OBSERVATION)],
            ):
                token = response.token
                if response.token.special:
                    print("=== Output:")
                    print(output_text)

                    match token.text.strip():
                        case '<|user|>':
                            append_conversation(Conversation(
                                Role.ASSISTANT,
                                postprocess_text(output_text),
                            ), history, markdown_placeholder)
                            return
                        # Initiate tool call
                        case '<|assistant|>':
                            append_conversation(Conversation(
                                Role.ASSISTANT,
                                postprocess_text(output_text),
                            ), history, markdown_placeholder)
                            message_placeholder = placeholder.chat_message(name="interpreter", avatar="assistant")
                            markdown_placeholder = message_placeholder.empty()
                            output_text = ''
                            continue
                        case '<|observation|>':
                            code = extract_code(output_text)
                            print("Code:", code)

                            display_text = output_text.split('interpreter')[-1].strip()
                            append_conversation(Conversation(
                                Role.INTERPRETER,
                                postprocess_text(display_text),
                            ), history, markdown_placeholder)
                            message_placeholder = placeholder.chat_message(name="observation", avatar="user")
                            markdown_placeholder = message_placeholder.empty()
                            output_text = ''
                            
                            with markdown_placeholder:
                                with st.spinner('Executing code...'):
                                    try:
                                        res_type, res = execute(code, get_kernel())
                                    except Exception as e:
                                        st.error(f'Error when executing code: {e}')
                                        return
                            print("Received:", res_type, res)

                            if res_type == 'text' and len(res) > TRUNCATE_LENGTH:
                                res = res[:TRUNCATE_LENGTH] + ' [TRUNCATED]'

                            append_conversation(Conversation(
                                Role.OBSERVATION,
                                '[Image]' if res_type == 'image' else postprocess_text(res),
                                tool=None,
                                image=res if res_type == 'image' else None,
                            ), history, markdown_placeholder)
                            message_placeholder = placeholder.chat_message(name="assistant", avatar="assistant")
                            markdown_placeholder = message_placeholder.empty()
                            output_text = ''
                            break
                        case _:
                            st.error(f'Unexpected special token: {token.text.strip()}')
                            break
                output_text += response.token.text
                display_text = output_text.split('interpreter')[-1].strip()
                markdown_placeholder.markdown(postprocess_text(display_text + '▌'))
            else:
                append_conversation(Conversation(
                    Role.ASSISTANT,
                    postprocess_text(output_text),
                ), history, markdown_placeholder)
                return
```

# 体验代码解释器

如果想直接体验代码解释器，可以前往综合demo直接体验。

使用代码解释器状态并在demo中输入：

```
画一个爱心
```

你将会看到命令行中出现以下日志：

```
=== History:
[Conversation(role=<Role.USER: 2>, content='画一个爱心', tool=None, image=None), Conversation(role=<Role.ASSISTANT: 3>, content='很抱歉，我无法直接个简单的ASCII艺术爱心或提供一个小程序来绘制爱心形状。您希望选择哪种方式？', tool=None, image=None), Conversation(role=<Role.USER: 2>, content='用pyersation(role=<Role.ASSISTANT: 3>, content="好的，我将为您提供一个简单的Python代码来绘制一个爱心形状。\n\n```python\nimport math\n\ndef print_heart:\n        line = ''\n        for x in range(7):\n            if (x == 2 or x == 6 or y == 2 or y == 6):\n                line += '*'\n            else:\n                line += ' '\n        print(line)\n\nprint_heart()\n```\n\n这段代码将使用Python的`print`函数来打印一个简单的爱心形状。您希望我e=None), Conversation(role=<Role.USER: 2>, content='执行', tool=None, image=None)]

=== Output:

interpreter
```

```python
def print_heart():
    for y in range(6):
        line = ''
        for x in range(7):
            if (x == 2 or x == 6 or y == 2 or y == 6):
                line += '*'
            else:
                line += ' '
        print(line)

print_heart()
```

```
Code: def print_heart():

    for y in range(6):

        line = ''

        for x in range(7):

            if (x == 2 or x == 6 or y == 2 or y == 6):

                line += '*'

            else:

                line += ' '

        print(line)

print_heart()

Backend kernel started with the configuration: /tmp/tmprie8vhq_.json

{'control_port': 37755,

 'hb_port': 58129,

 'iopub_port': 51739,

 'ip': '127.0.0.1',

 'key': b'df3d3860-c68ef973ec4c58c5b561bcbd',

 'shell_port': 44607,

 'signature_scheme': 'hmac-sha256',

 'stdin_port': 49903,

 'transport': 'tcp'}

Code kernel started.

Received: text   *   *

  *   *

*******

  *   *

  *   *

  *   *

<|user|> 画一个爱心 None

=== Input:

<|system|>

你是一位智能AI助手，你叫ChatGLM，你连接着一台电脑，但请注意不能联网。在使用Python解决任务时，你可以运行代码并得到结果，如果运行结果有错误，你需要尽可能对代码进行改进。你可以处理用户上传到电脑上的文件，文件默认存储路径是/mnt/data/。<|user|>

画一个爱心<|assistant|>

=== History:

[Conversation(role=<Role.USER: 2>, content='画一个爱心', tool=None, image=None)]

=== Output:

interpreter
```
