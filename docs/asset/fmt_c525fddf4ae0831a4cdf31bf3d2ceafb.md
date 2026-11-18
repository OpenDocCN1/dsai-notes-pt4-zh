
```python
import os

from colorama import Fore

from camel.models import ModelFactory
from camel.societies import RolePlaying
from camel.types import ModelPlatformType
from camel.utils import print_text_animated

# 获取API密钥
api_key = os.getenv("INTERN_API_KEY")

# 创建模型实例, 添加max_tokens配置
model = ModelFactory.create(
    model_platform=ModelPlatformType.OPENAI_COMPATIBLE_MODEL,
    model_type="intern-s1",
    url="https://chat.intern-ai.org.cn/api/v1/",
    api_key=api_key,
    model_config_dict={
        "max_tokens": 4000,  # 添加max_tokens配置
        "temperature": 0.7,
    },
)

def main(model=model, chat_turn_limit=3) -> None:
    task_prompt = "Develop a trading bot for the stock market"

    # 初始化角色扮演会话
    role_play_session = RolePlaying(
        assistant_role_name="Python Programmer",
        assistant_agent_kwargs=dict(model=model),
        user_role_name="Stock Trader",
        user_agent_kwargs=dict(model=model),
        task_prompt=task_prompt,
        with_task_specify=True,
        task_specify_agent_kwargs=dict(model=model),
    )

    print(f"{Fore.CYAN}=== 股票交易机器人开发会话 ==={Fore.RESET}")
    print(f"{Fore.YELLOW}任务: {task_prompt}{Fore.RESET}")
    print(f"{Fore.GREEN}开始对话...{Fore.RESET}\n")

    # 初始化会话
    input_msg = role_play_session.init_chat()

    # 开始多轮对话
    for i in range(chat_turn_limit):
        try:
            print(f"{Fore.BLUE}=== 第 {i + 1} 轮对话 ==={Fore.RESET}")

            # 助手回应
            assistant_response, user_response = role_play_session.step(
                input_msg
            )

            if assistant_response.terminated or user_response.terminated:
                print(f"{Fore.RED}对话结束{Fore.RESET}")
                break

            # 打印助手回应
            print(f"{Fore.MAGENTA}Python程序员:{Fore.RESET}")
            print_text_animated(assistant_response.msg.content)
            print()

            # 打印用户回应
            print(f"{Fore.CYAN}股票交易员:{Fore.RESET}")
            print_text_animated(user_response.msg.content)
            print()

            # 准备下一轮输入
            input_msg = assistant_response.msg

        except KeyboardInterrupt:
            print(f"\n{Fore.YELLOW}用户中断对话{Fore.RESET}")
            break
        except Exception as e:
            print(f"{Fore.RED}发生错误: {e}{Fore.RESET}")
            break

if __name__ == "__main__":  # 修复了这里的语法错误
    # 检查API密钥
    if not api_key:
        print(f"{Fore.RED}错误: 请设置环境变量 INTERN_API_KEY{Fore.RESET}")
        exit(1)

    main()
```

![](img/f2f97fafbbca3ca16925c959ccfd2999.png)

## 5.3 沉浸式写AI小说

引入 `CrticAgent`，`with_critic_in_the_loop` 设置为 `True` 的时候将会在循环中引入 `CrticAgent`，如果我们将 `"human"` 赋值给 `critic_role_name`，我们人类将可以在与智能体之间交互中掌握主动权，去选择优化和调整，并且提升角色表现。
